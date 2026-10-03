We can explore how a shader works with the following code:

```wgsl
struct Record {
    origin: vec3<f32>,
    width: f32,
    direction: vec3<f32>,
    height: f32,
    colour: vec4<f32>,
    owner: u32,
    part: u32,
    flags: u32,
    reserved: u32,
};

@group(1) @binding(0) var<storage, read> records: array<Record>;

struct VsOut {
    @builtin(position) position: vec4<f32>,
    @location(0) @interpolate(flat) normal: vec3<f32>,
    @location(1) colour: vec4<f32>,
    @location(2) @interpolate(flat) part: u32,
};

// Fixed camera: turn 30 deg about z, then tip -60 deg about x so z leans toward the viewer.
fn view(p: vec3<f32>) -> vec3<f32> {
    let b = radians(30.0);
    let a = radians(-60.0);
    let x1 = p.x * cos(b) - p.y * sin(b);
    let y1 = p.x * sin(b) + p.y * cos(b);
    let y2 = y1 * cos(a) - p.z * sin(a);
    let z2 = y1 * sin(a) + p.z * cos(a);
    return vec3<f32>(x1, y2, z2);
}

fn clip(p: vec3<f32>) -> vec4<f32> {
    return vec4<f32>(p.x * 0.55, p.y * 0.55, 0.5 - 0.25 * p.z, 1.0);
}

@vertex
fn vs_main(@location(0) position: vec3<f32>, @builtin(instance_index) instance: u32) -> VsOut {
    let record = records[instance];
    var out: VsOut;
    out.part = record.part;
    // A dead record draws nothing: every vertex lands behind the far plane and is clipped.
    if ((record.flags & TOMBSTONE) != 0u) {
        out.position = vec4<f32>(0.0, 0.0, 2.0, 1.0);
        out.normal = vec3<f32>(0.0, 0.0, 1.0);
        out.colour = record.colour;
        return out;
    }
    let normal = normalize(record.direction);
    var tangent = vec3<f32>(1.0, 0.0, 0.0);
    var bitangent = vec3<f32>(0.0, sign(normal.z), 0.0);
    if abs(normal.z) <= 0.999 {
        tangent = normalize(cross(vec3<f32>(0.0, 0.0, 1.0), normal));
        bitangent = cross(normal, tangent);
    }
    let world = record.origin
        + tangent * (position.x * record.width)
        + bitangent * (position.y * record.height);

    out.position = clip(view(world));
    out.normal = normalize(view(normal));
    out.colour = record.colour;
    return out;
}

struct FsOut {
    @location(0) colour: vec4<f32>,
    @location(1) id: vec2<u32>,
};

@fragment
fn fs_main(in: VsOut) -> FsOut {
    let light = normalize(vec3<f32>(-0.4, 0.6, 1.0));
    let shade = 0.25 + 0.75 * abs(dot(in.normal, light));
    return FsOut(vec4<f32>(in.colour.rgb * shade, in.colour.a), pack_id(in.part));
}
```
We can break this shader down starting with the `Record`. The `Record` is from the storage buffer. This must align with the bytes being passed into the storage buffer. We then move onto the bindings below:
```
@group(1) @binding(0) var<storage, read> records: array<Record>;
```
Essentially, what we are saying here is that the binding zero in group one is a read only storage buffer that 