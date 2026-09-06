Worked from the textbook example where $\overrightarrow{PQ} = 2\mathbf{a} - \mathbf{b} = 3\mathbf{i} - 5\mathbf{j} - 5\mathbf{k}$ and $\overrightarrow{OP} = 2\mathbf{j} + 3\mathbf{k}$, and we want the point $Q$.

## The distinction

The step that trips people up is that $\overrightarrow{PQ}$ and $\overrightarrow{OQ}$ are two different kinds of object, even though both are written as vectors.

$\overrightarrow{PQ} = 3\mathbf{i} - 5\mathbf{j} - 5\mathbf{k}$ is a **displacement**: an instruction for movement — go 3 along $x$, 5 backwards along $y$, 5 down $z$. It carries no information about where you started, so on its own it can never tell you where $Q$ is.

$\overrightarrow{OQ}$ is a **position vector**: a displacement that happens to start at the origin. That's the special one, because its components *are* the coordinates of $Q$.

## The triangle law

To pin $Q$ down you need an anchor, and $P$ is it. You're told $\overrightarrow{OP} = 2\mathbf{j} + 3\mathbf{k}$, so $P = (0, 2, 3)$. Walk from $O$ to $P$, then walk the displacement, and you've arrived at $Q$.

<svg width="380" viewBox="0 0 380 230" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Triangle law for vector addition: O to P, then P to Q, equals O to Q">
<defs>
<marker id="arrowN" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker>
<marker id="arrowP" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse"><path d="M2 1L8 5L2 9" fill="none" stroke="#7F77DD" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/></marker>
</defs>
<line x1="60" y1="200" x2="146" y2="76" stroke="currentColor" stroke-width="1.5" marker-end="url(#arrowN)"/>
<line x1="154" y1="70" x2="294" y2="155" stroke="currentColor" stroke-width="1.5" marker-end="url(#arrowN)"/>
<line x1="60" y1="200" x2="294" y2="161" stroke="#7F77DD" stroke-width="1.5" fill="none" marker-end="url(#arrowP)"/>
<circle cx="60" cy="200" r="4" fill="currentColor"/>
<circle cx="150" cy="70" r="4" fill="currentColor"/>
<circle cx="300" cy="160" r="4" fill="currentColor"/>
<text x="46" y="214" fill="currentColor" font-family="sans-serif" font-size="14">O</text>
<text x="150" y="52" fill="currentColor" font-family="sans-serif" font-size="14" text-anchor="middle">P</text>
<text x="314" y="164" fill="currentColor" font-family="sans-serif" font-size="14">Q</text>
<text x="52" y="132" fill="currentColor" font-family="sans-serif" font-size="12" text-anchor="end" opacity="0.7">start here</text>
<text x="225" y="98" fill="currentColor" font-family="sans-serif" font-size="12" text-anchor="middle" opacity="0.7">the displacement</text>
<text x="175" y="208" fill="currentColor" font-family="sans-serif" font-size="12" text-anchor="middle" opacity="0.7">where Q actually is</text>
</svg>

$$\overrightarrow{OQ} = \overrightarrow{OP} + \overrightarrow{PQ} = (2\mathbf{j} + 3\mathbf{k}) + (3\mathbf{i} - 5\mathbf{j} - 5\mathbf{k}) = 3\mathbf{i} - 3\mathbf{j} - 2\mathbf{k}$$

Component by component that's just $(0+3,\ 2-5,\ 3-5)$, and because $\overrightarrow{OQ}$ starts at the origin, those components read off directly as the point $Q = (3, -3, -2)$.

## The thing to memorise

$$\overrightarrow{PQ} = \overrightarrow{OQ} - \overrightarrow{OP}$$

Head minus tail. Everything of this shape falls out of it — the book's version is the same identity solved for the unknown end.