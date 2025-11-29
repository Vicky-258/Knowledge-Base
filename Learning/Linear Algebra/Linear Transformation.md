#matrices #linearalgebra #completed 

## Topic 📖: Linear Transformation ft-Matrices

## Key Concepts 🔑:

- **Transformation:** Transformation is a fancy way of saying functions they just take a input do things with it and return an output but in this vectors case we can think of it like moving something from one place to another.
- **Linear Transformation:** Visually, It's about doing transformation to vectors such that the span or the grid they construct have all the lines parallel including diagonal lines and the origin doesn't move. But Mathematically, It's when,
$$
\begin{aligned}
\text{A transformation } L: \mathbf{V} \to \mathbf{W} \text{ is linear if it satisfies:} \\
\text{1. Additivity: } L(\mathbf{u} + \mathbf{v}) = L(\mathbf{u}) + L(\mathbf{v}) \quad \forall \, \mathbf{u}, \mathbf{v} \in \mathbf{V}, \\
\text{2. Scaling (Homogeneity): } L(c\mathbf{u}) = cL(\mathbf{u}) \quad \forall \, \mathbf{u} \in \mathbf{V}, \, c \in \mathbb{R}.
\end{aligned}
$$
- For linear Transformation we have to know a single vector's transformation most likely the unit vectors $\vec{i}$ and $\vec j$, If we know their positions after the transformation we can find all the other vector's position by multiplying the x with $\vec i$ and y with $\vec j$ 's coordinates.

- The multiplying part simply means having a 2-d matrix like $\begin{bmatrix} 3 & 1\\ 4 & 0\end{bmatrix}$ in which the 1st column represent $\vec i$ 's coordinates and 2nd column represent $\vec j$ 's coordinates after transformation, and multiplying this with the coordinates of the required vector say $\vec v$ with value $\begin{bmatrix} 5\\4 \end{bmatrix}$ then the new position would be $5 \times \begin{bmatrix} 3 \\ 4\end{bmatrix}$ and $4 \times \begin{bmatrix} 1\\0 \end{bmatrix}$ which will give out $\begin{bmatrix} 35 \\ 4 \end{bmatrix}$ which will be the position of the vector after transformation.

## My Insights 🔭:

- The whole concept of linear transformation has been thought to me as “MATRIX VECTOR MULTIPLICATION” without even telling what it actually is.
- My entire Mathematics classes where a lie.
- Darn it My whole life was a lie.
## Links/References 🔗:

- [[https://youtu.be/kYB8IZa5AuE?si=gW--NnbUywxGOFlW|3 Blue 1 Brown's video]]
- [[Spans, Linear combinations, basis|Next]]

