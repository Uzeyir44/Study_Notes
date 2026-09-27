
This lecture elevates our understanding of matrices from static tables of coefficients to dynamic mathematical operators. We explore matrix multiplication through four distinct conceptual frameworks—each revealing a different facet of how systems interact—and establish why matrix multiplication represents the composition of linear transformations. We then introduce the inverse matrix $A^{-1}$ as the unique algebraic engine that perfectly undoes spatial deformation, detailing both its geometric meaning and the Gauss-Jordan elimination method used to construct it.

# Big Picture

### What mathematical problem does matrix multiplication solve?

In isolation, a single matrix-vector multiplication $Ax = b$ describes how a system transforms a single input vector $x$ into an output vector $b$. However, real-world systems do not operate in isolation; they are chained together. We need a way to calculate the net effect of applying multiple linear transformations in sequence.

Matrix multiplication solves the problem of **composition**. If transformation $B$ acts on a vector $x$ to produce $y$, and transformation $A$ acts on $y$ to produce $z$, matrix multiplication allows us to combine these two steps into a single, master operator $C = AB$ that maps $x$ directly to $z$ without calculating any intermediate steps.

### Why wasn't ordinary arithmetic enough?

In ordinary arithmetic, multiplication is simple element-by-element scaling. If we defined matrix multiplication by simply multiplying corresponding elements (e.g., $C_{ij} = A_{ij} \times B_{ij}$), we would preserve arithmetic simplicity but completely destroy geometric utility.

Element-wise multiplication fails to capture the coupling between variables. If changing variable $x_1$ impacts variable $y_2$, our algebraic operation must reflect this cross-talk. Matrix multiplication was invented precisely to capture how the components of multidimensional spaces interact.

### Why do inverse matrices exist?

If a matrix $A$ acts as a "machine" that takes a space, rotates it, stretches it, and shears it, the **inverse matrix** $A^{-1}$ is the "rewind button." It is the exact counter-transformation that returns the warped space to its original, pristine state.

If we warp a space via $A$ and then apply $A^{-1}$, the net effect is the identity transformation $I$ (doing absolutely nothing).

### How does this lecture naturally continue Lecture 2?

In Lecture 2, we solved systems of equations by performing a chain of raw, manual row operations. We represented these operations as individual elimination matrices ($E_{21}$, $E_{31}$, $E_{32}$) acting on a matrix $A$ to yield an upper triangular matrix $U$:

$$E_{32}(E_{31}(E_{21}A)) = U$$

This chain of operations begs several deep algebraic questions:

1. Can we consolidate all these individual step-matrices into a single, master elimination matrix $E = E_{32}E_{31}E_{21}$? (Yes, through **matrix multiplication**).
    
2. Can we reverse this journey to reconstruct our original matrix $A$ from $U$? (Yes, through **inverse matrices**).
    

# Learning Objectives

- **Master the Four Views of Multiplication:** Move fluidly between the dot-product view, the column-combination view, the row-combination view, and the outer-product view.
    
- **Internalize Matrices as Transformations:** Shift from thinking of a matrix as a static grid of numbers to visualizing it as an active geometric map that bends, stretches, and rotates vector space.
    
- **Deconstruct Invertibility:** Understand why certain matrices cannot be inverted (singular matrices) and grasp the geometric and algebraic conditions that cause inversion to fail.
    
- **Grasp Gauss-Jordan Elimination:** Understand the algebraic beauty of finding an inverse by applying elimination to an augmented matrix $[A \mid I]$.
    

# Core Concepts

## 1. Matrix Multiplication (The Four Interpretations)

### What is it?

Matrix multiplication is an algebraic operation that combines two matrices $A$ (of size $m \times n$) and $B$ (of size $n \times p$) to produce a third matrix $C$ (of size $m \times p$).

### Why was it invented?

It was invented to represent the composition of linear transformations. If $y = Bx$ and $z = Ay$, then $z = A(Bx) = (AB)x = Cx$. The definition of matrix multiplication is the unique mathematical structure that makes this associative property hold true.

### Intuition

Do not limit yourself to the classic row-by-column dot product. To truly understand linear algebra, you must hold four distinct mental models of what matrix multiplication is doing under the hood:

```
                          MATRIX MULTIPLICATION (C = AB)
                                        |
      +--------------------+------------+------------+--------------------+
      |                    |                         |                    |
[Dot Product View]  [Column View]              [Row View]           [Outer Product]
Single entries are  Columns of C are           Rows of C are        C is the sum of
dot products of     linear combinations        linear combinations  rank-1 matrices
rows of A & cols B  of columns of A            of rows of B         (cols A * rows B)
```

1. **The Entry View (Row** $\times$ **Column Dot Product):** The number at position $(i,j)$ in the final matrix $C$ is the dot product of Row $i$ of matrix $A$ and Column $j$ of matrix $B$.
    
2. **The Column View (Combinations of Columns):** Each column of the product matrix $C$ is a linear combination of the columns of $A$, weighted by the corresponding column of $B$.
    
3. **The Row View (Combinations of Rows):** Each row of the product matrix $C$ is a linear combination of the rows of $B$, weighted by the corresponding row of $A$.
    
4. **The Slab View (Sum of Outer Products):** Matrix $C$ is the sum of the "outer products" of the columns of $A$ and the rows of $B$. This views $C$ as a composite built of simpler, "rank-1" matrices.
    

### Mathematical Definitions

Let $A$ be an $m \times n$ matrix and $B$ be an $n \times p$ matrix. The product $C = AB$ is an $m \times p$ matrix.

#### 1. Entry View:

$$C_{ij} = \sum_{k=1}^n A_{ik} B_{kj}$$

#### 2. Column View:

$$\text{Column } j \text{ of } C = A \times (\text{Column } j \text{ of } B)$$

#### 3. Row View:

$$\text{Row } i \text{ of } C = (\text{Row } i \text{ of } A) \times B$$

#### 4. Outer Product View:

$$C = \sum_{k=1}^n (\text{Column } k \text{ of } A) \times (\text{Row } k \text{ of } B)$$

### Step-by-Step Example (All Four Views)

Let us multiply the following matrices:

$$A = \begin{bmatrix} 2 & 1 \\ 3 & 0 \end{bmatrix}, \quad B = \begin{bmatrix} 1 & 2 \\ 4 & 5 \end{bmatrix}$$

#### View 1: Entry-by-Entry (Dot Product)

- **Entry** $C_{11}$**:** Row 1 of $A$ $\cdot$ Col 1 of $B$:
    
    $$[2, 1] \cdot \begin{bmatrix} 1 \\ 4 \end{bmatrix} = (2 \times 1) + (1 \times 4) = 6$$
- **Entry** $C_{12}$**:** Row 1 of $A$ $\cdot$ Col 2 of $B$:
    
    $$[2, 1] \cdot \begin{bmatrix} 2 \\ 5 \end{bmatrix} = (2 \times 2) + (1 \times 5) = 9$$
- **Entry** $C_{21}$**:** Row 2 of $A$ $\cdot$ Col 1 of $B$:
    
    $$[3, 0] \cdot \begin{bmatrix} 1 \\ 4 \end{bmatrix} = (3 \times 1) + (0 \times 4) = 3$$
- **Entry** $C_{22}$**:** Row 2 of $A$ $\cdot$ Col 2 of $B$:
    
    $$[3, 0] \cdot \begin{bmatrix} 2 \\ 5 \end{bmatrix} = (3 \times 2) + (0 \times 5) = 6$$

Result:

$$C = \begin{bmatrix} 6 & 9 \\ 3 & 6 \end{bmatrix}$$

#### View 2: Column View

- **Column 1 of** $C$ is matrix $A$ multiplied by Column 1 of $B$:
    
    $$\text{Col}_1(C) = 1 \begin{bmatrix} 2 \\ 3 \end{bmatrix} + 4 \begin{bmatrix} 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 2 \\ 3 \end{bmatrix} + \begin{bmatrix} 4 \\ 0 \end{bmatrix} = \begin{bmatrix} 6 \\ 3 \end{bmatrix}$$
- **Column 2 of** $C$ is matrix $A$ multiplied by Column 2 of $B$:
    
    $$\text{Col}_2(C) = 2 \begin{bmatrix} 2 \\ 3 \end{bmatrix} + 5 \begin{bmatrix} 1 \\ 0 \end{bmatrix} = \begin{bmatrix} 4 \\ 6 \end{bmatrix} + \begin{bmatrix} 5 \\ 0 \end{bmatrix} = \begin{bmatrix} 9 \\ 6 \end{bmatrix}$$

#### View 3: Row View

- **Row 1 of** $C$ is Row 1 of $A$ multiplying matrix $B$:
    
    $$\text{Row}_1(C) = 2 \begin{bmatrix} 1 & 2 \end{bmatrix} + 1 \begin{bmatrix} 4 & 5 \end{bmatrix} = \begin{bmatrix} 2 & 4 \end{bmatrix} + \begin{bmatrix} 4 & 5 \end{bmatrix} = \begin{bmatrix} 6 & 9 \end{bmatrix}$$
- **Row 2 of** $C$ is Row 2 of $A$ multiplying matrix $B$:
    
    $$\text{Row}_2(C) = 3 \begin{bmatrix} 1 & 2 \end{bmatrix} + 0 \begin{bmatrix} 4 & 5 \end{bmatrix} = \begin{bmatrix} 3 & 6 \end{bmatrix} + \begin{bmatrix} 0 & 0 \end{bmatrix} = \begin{bmatrix} 3 & 6 \end{bmatrix}$$

#### View 4: Outer Product View

We multiply Column $k$ of $A$ by Row $k$ of $B$, and sum the resulting rank-1 matrices:

- **First Product (**$k=1$**):** Col 1 of $A$ $\times$ Row 1 of $B$:
    
    $$\begin{bmatrix} 2 \\ 3 \end{bmatrix} \begin{bmatrix} 1 & 2 \end{bmatrix} = \begin{bmatrix} 2 \times 1 & 2 \times 2 \\ 3 \times 1 & 3 \times 2 \end{bmatrix} = \begin{bmatrix} 2 & 4 \\ 3 & 6 \end{bmatrix}$$
- **Second Product (**$k=2$**):** Col 2 of $A$ $\times$ Row 2 of $B$:
    
    $$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \begin{bmatrix} 4 & 5 \end{bmatrix} = \begin{bmatrix} 1 \times 4 & 1 \times 5 \\ 0 \times 4 & 0 \times 5 \end{bmatrix} = \begin{bmatrix} 4 & 5 \\ 0 & 0 \end{bmatrix}$$
- **Sum them up:**
    
    $$C = \begin{bmatrix} 2 & 4 \\ 3 & 6 \end{bmatrix} + \begin{bmatrix} 4 & 5 \\ 0 & 0 \end{bmatrix} = \begin{bmatrix} 6 & 9 \\ 3 & 6 \end{bmatrix}$$

All four views yield the exact same matrix, proving their mathematical equivalence.

## 2. The Inverse Matrix ($A^{-1}$)

### What is it?

For a square matrix $A \in \mathbb{R}^{n \times n}$, its inverse is a unique matrix denoted as $A^{-1}$ such that:

$$A^{-1}A = I \quad \text{and} \quad AA^{-1} = I$$

where $I$ is the identity matrix.

### Why was it invented?

In scalar algebra, if we want to solve $ax = b$, we simply divide both sides by $a$ (or multiply by the reciprocal $a^{-1}$). In matrix algebra, **division is not defined**. We cannot write $x = \frac{b}{A}$. To solve the vector equation $Ax = b$, we invented the inverse matrix to act as the algebraic equivalent of division:

$$x = A^{-1}b$$

### Intuition

If matrix $A$ acts as a spatial warp (for example, tilting a 2D space to the right), $A^{-1}$ is the exact counter-warp that pulls it back upright.

```
   [ Space R^n ]  ---( Multiplied by A )--->   [ Warped Space R^n ]
         ^                                              |
         |-------------( Multiplied by A^-1 )-----------v
```

If a matrix compresses a 3D space completely flat into a 2D sheet, information is lost forever. You cannot un-flatten space because you do not know where the points originally hovered. This is why **singular (non-invertible) matrices** exist: they destroy dimensions, making reverse-navigation impossible.

### Mathematical Definition

A square matrix $A$ is **invertible** (or **non-singular**) if there exists a matrix $A^{-1}$ such that $A^{-1}A = I$. If no such matrix exists, $A$ is **singular**.

#### Crucial Characterizations of Invertibility:

- $A$ is invertible if and only if its elimination process yields a full set of $n$ pivots.
    
- $A$ is invertible if and only if the only solution to the equation $Ax = 0$ is the trivial solution $x = 0$. If a non-zero vector $x$ can satisfy $Ax = 0$, then $A$ is singular.
    

### Common Misconceptions

- **Assuming all non-zero matrices are invertible:** Beginners often assume that as long as a matrix does not consist entirely of zeros, it is invertible. This is false. The matrix $A = \begin{bmatrix} 1 & 3 \\ 2 & 6 \end{bmatrix}$ is non-zero, but its columns lie on the exact same line. It has no inverse.
    
- **Order of Inversion:** Students often write $(AB)^{-1} = A^{-1}B^{-1}$. This is a major mistake. The correct formula is:
    
    $$(AB)^{-1} = B^{-1}A^{-1}$$
    
    _Analogy:_ If you put on your socks ($B$) and then put on your shoes ($A$), the process of undoing this requires you to first take off your shoes ($A^{-1}$) and then take off your socks ($B^{-1}$).
    

## 3. Gauss-Jordan Elimination

### What is it?

Gauss-Jordan elimination is an algorithmic process that finds the inverse of a matrix $A$ by solving $n$ systems of equations simultaneously. It does this by appending the identity matrix $I$ to $A$ to form an augmented matrix $[A \mid I]$, and then running row operations until the left side is transformed into $I$. The right side will miraculously have transformed into $A^{-1}$.

### Why was it invented?

To find the inverse of $A$, we need to solve the equation $AX = I$, where $X = A^{-1}$. If we treat the columns of $X$ as unknown vectors $x_1, x_2, \dots, x_n$ and the columns of $I$ as standard basis vectors $e_1, e_2, \dots, e_n$, we are solving $n$ distinct systems of equations:

$$Ax_1 = e_1, \quad Ax_2 = e_2, \quad \dots \quad Ax_n = e_n$$

Instead of solving these $n$ systems one-by-one (which is highly redundant), Gauss-Jordan elimination bundles them into a single, highly efficient matrix pipeline.

### Intuition

If you have a scale balancing $A$ on the left and $I$ on the right, any row operation you perform to simplify $A$ must be applied to $I$ to keep the scale balanced. By the time you have stripped all complexity away from $A$ (turning it into the identity matrix $I$), the accumulated history of those balancing operations on the right side will have constructed $A^{-1}$.

### Step-by-Step Example

Let us find the inverse of:

$$A = \begin{bmatrix} 2 & -1 \\ -1 & 2 \end{bmatrix}$$

**Step 1: Set up the augmented matrix** $[A \mid I]$**.**

$$\left[ \begin{array}{cc|cc} 2 & -1 & 1 & 0 \\ -1 & 2 & 0 & 1 \end{array} \right]$$

**Step 2: Eliminate below the first pivot.**

- The first pivot is $a_{11} = 2$.
    
- To eliminate the $-1$ in row 2, column 1, we add $\frac{1}{2}$ of Row 1 to Row 2:
    
    $$\text{Row 2}_{\text{new}} = [-1, 2, 0, 1] + \frac{1}{2} [2, -1, 1, 0] = \left[ 0, \frac{3}{2}, \frac{1}{2}, 1 \right]$$
- Our augmented matrix now looks like:
    
    $$\left[ \begin{array}{cc|cc} 2 & -1 & 1 & 0 \\ 0 & \frac{3}{2} & \frac{1}{2} & 1 \end{array} \right]$$

**Step 3: Eliminate upwards (the "Jordan" step).**

- We want to eliminate the $-1$ in row 1, column 2, to make the left side diagonal. Our second pivot is $\frac{3}{2}$.
    
- We add $\frac{2}{3}$ of Row 2 to Row 1:
    
    $$\text{Row 1}_{\text{new}} = [2, -1, 1, 0] + \frac{2}{3} \left[ 0, \frac{3}{2}, \frac{1}{2}, 1 \right] = \left[ 2, 0, \frac{4}{3}, \frac{2}{3} \right]$$
- Our augmented matrix is now:
    
    $$\left[ \begin{array}{cc|cc} 2 & 0 & \frac{4}{3} & \frac{2}{3} \\ 0 & \frac{3}{2} & \frac{1}{2} & 1 \end{array} \right]$$

**Step 4: Normalize the diagonal entries to** $1$**.**

- Divide Row 1 by $2$ (multiply by $\frac{1}{2}$):
    
    $$\text{Row 1}_{\text{final}} = \left[ 1, 0, \frac{2}{3}, \frac{1}{3} \right]$$
- Divide Row 2 by $\frac{3}{2}$ (multiply by $\frac{2}{3}$):
    
    $$\text{Row 2}_{\text{final}} = \left[ 0, 1, \frac{1}{3}, \frac{2}{3} \right]$$
- Our final augmented matrix is:
    
    $$\left[ \begin{array}{cc|cc} 1 & 0 & \frac{2}{3} & \frac{1}{3} \\ 0 & 1 & \frac{1}{3} & \frac{2}{3} \end{array} \right]$$

The left side is now $I$. Therefore, the right side is our inverse matrix:

$$A^{-1} = \begin{bmatrix} \frac{2}{3} & \frac{1}{3} \\ \frac{1}{3} & \frac{2}{3} \end{bmatrix}$$

# Matrix as a Transformation

To truly master linear algebra, you must stop viewing a matrix as a static block of numbers.

> [!important] The Transformation Paradigm A matrix is an active **machine** that takes in an input vector $x$ from one space, processes it, and spits out an output vector $Ax$ in another space.

```
       INPUT VECTOR (x)
             │
             ▼
   ┌───────────────────┐
   │ Matrix Machine A  │ ───► Transformations: Stretch, Rotate, Shear
   └───────────────────┘
             │
             ▼
      OUTPUT VECTOR (Ax)
```

### Visualizing Basic Matrix Actions (2D Space)

#### 1. Stretching/Scaling Matrix:

$$A = \begin{bmatrix} 2 & 0 \\ 0 & 1 \end{bmatrix}$$

- **Action:** Grabs the 2D plane and stretches it horizontally to double its width, while leaving its height completely untouched.
    

#### 2. Shearing Matrix:

$$A = \begin{bmatrix} 1 & 1 \\ 0 & 1 \end{bmatrix}$$

- **Action:** Slides the top of the space to the right while keeping the baseline locked in place. Square grids tilt into slanted parallelograms.
    

#### 3. Rotation Matrix:

$$A = \begin{bmatrix} 0 & -1 \\ 1 & 0 \end{bmatrix}$$

- **Action:** Rotates the entire 2D plane counterclockwise by $90^\circ$.
    

#### 4. Reflection Matrix:

$$A = \begin{bmatrix} 1 & 0 \\ 0 & -1 \end{bmatrix}$$

- **Action:** Flips the entire space upside down across the horizontal x-axis.
    

# Visual Interpretation of Invertibility

Let's look at the geometry behind these transformations:

```
        INVERTIBLE MATRIX                         SINGULAR MATRIX
    (Full Dimension Preserved)                 (Dimension Collapsed)
    
            ▲                                         ▲
            │   ┌───┐                                 │  /
            │   └───┘                                 │ / (All space squashed
            └───┴──────►                              └──┴──────► onto a line)
   Every point has a unique home.              Points are packed on top of each other.
   No information is lost.                      Information is lost forever.
```

### What moves?

- Under any linear transformation $A$, the coordinate grid lines of the space bend, rotate, and stretch. However, they are constrained to remain straight, parallel, and evenly spaced.
    

### What remains invariant?

- Under all linear transformations, the **origin** $(0,0)$ **never moves**. It is the anchor point of the entire universe. No matter how much a matrix warps, stretches, or shears space, $A \mathbf{0} = \mathbf{0}$ always holds true.
    

### What information is preserved vs. changed?

- **In an Invertible Transformation:** The overall dimensionality of the space is preserved. A 2D plane remains a 2D plane, and a 3D volume remains a 3D volume. Every individual coordinate point lands on a unique destination point. Because no two points land on top of each other, we can track them backwards.
    
- **In a Singular Transformation:** The space collapses into a lower dimension. A 2D plane is squashed flat into a 1D line, or a 3D space is compressed into a 2D sheet. Multiple distinct starting points are crushed onto the exact same ending point. Because we cannot know which of the starting points mapped to that destination, we cannot reverse the transformation. Information is lost forever.
    

# Key Proofs and Derivations

### Proof 1: Why the Inverse of a Product is Reversed: $(AB)^{-1} = B^{-1}A^{-1}$

We want to find a matrix $C$ such that:

$$(AB)C = I$$

Let's unpack the product step-by-step:

1. Start with the equation:
    
    $$(AB)C = I$$
2. Since matrix multiplication is associative, we can group the terms however we like. Let's isolate $B$ by left-multiplying both sides of the equation by $A^{-1}$:
    
    $$A^{-1}(AB)C = A^{-1}I$$
3. Since $A^{-1}A = I$, this simplifies to:
    
    $$(I)BC = A^{-1}$$$$BC = A^{-1}$$
4. Now, isolate $C$ by left-multiplying both sides of the equation by $B^{-1}$:
    
    $$B^{-1}(BC) = B^{-1}A^{-1}$$
5. Since $B^{-1}B = I$, we get:
    
    $$IC = B^{-1}A^{-1}$$$$C = B^{-1}A^{-1}$$

This proves that the unique inverse of $(AB)$ is indeed $B^{-1}A^{-1}$.

### Proof 2: Why Gauss-Jordan Elimination Works Mathematically

Let $A$ be an invertible $n \times n$ matrix. We set up our augmented matrix as $[A \mid I]$.

Performing a row operation on a matrix is mathematically equivalent to multiplying that matrix on the left by an elimination matrix $E_k$.

If we perform a sequence of row operations $E_1, E_2, \dots, E_p$ to transform $A$ into the identity matrix $I$, then the product of these operations can be written as a single composite matrix:

$$M = E_p \dots E_2 E_1$$

By definition, these operations successfully map $A$ to $I$:

$$MA = I$$

Since $MA = I$, this composite operator $M$ must be the inverse of $A$:

$$M = A^{-1}$$

Now let us look at what happens to the right side of our augmented scale during this exact same sequence of operations. We apply the same operator $M$ to the identity matrix $I$:

$$\text{Right Side} = MI$$

Since $M = A^{-1}$, this evaluates to:

$$\text{Right Side} = A^{-1}I = A^{-1}$$

This proves that the exact sequence of row operations that transforms the left side of the augmented matrix from $A$ to $I$ will mathematically transform the right side from $I$ to $A^{-1}$.

# Connections

### Connection to Dot Products

The classic entry-by-entry method of computing $C = AB$ relies on taking dot products of the rows of $A$ and the columns of $B$. The value $C_{ij} = a_i \cdot b_j$ measures how aligned the $i$-th sensor or constraint of $A$ is with the $j$-th direction of $B$.

### Connection to Systems of Equations

The matrix equation $Ax = b$ can be solved in a single step if the inverse matrix is known: $x = A^{-1}b$. This shows that solving a system of equations is conceptually equivalent to running the transformation matrix backwards.

### Connection to Computer Graphics & Video Games

In modern 3D video games, character models are defined by clusters of 3D vectors. To move, rotate, or scale a character, the game engine multiplies every single vertex vector by a $4 \times 4$ transformation matrix.

If a player moves forward and turns right, the engine multiplies the movement matrix by the rotation matrix to create a single composite matrix, ensuring the rendering engine only needs to perform one multiplication per vertex.

### Connection to Machine Learning & Deep Learning

Every layer of a deep neural network is a matrix transformation $y = \sigma(Wx + b)$. The training process (backpropagation) requires us to calculate how changes in the output impact the inputs.

This calculation flows backward through the network, applying the transpose of the weight matrices ($W^T$), which is closely related to the inverse mapping.

# Mental Models

- **Instead of thinking:** "Matrix multiplication is a tedious formula of multiplying rows by columns." **Think:** "Matrix multiplication is a recipe. The columns of the product are built by taking the ingredients (the columns of $A$) and mixing them in the proportions specified by $B$."
    
- **Instead of thinking:** "An inverse matrix is a complex grid of fractions." **Think:** "An inverse matrix is a 'ctrl+z' undo command. It takes space that has been warped out of shape and pulls it back to its original grid."
    
- **Instead of thinking:** "A singular matrix is just a matrix whose determinant is zero." **Think:** "A singular matrix is a trash compactor. It crushes space, flattening a dimension and destroying information forever so that it can never be reconstructed."
    

# Frequently Asked Questions

### Why does the identity matrix $I$ act like the number $1$ in multiplication?

The identity matrix has $1$s on the main diagonal and $0$s everywhere else. Geometrically, it represents the "do nothing" transformation. It maps the standard basis vector $e_1$ to $e_1$, $e_2$ to $e_2$, and so on. Since it leaves the coordinate axes completely untouched, multiplying any matrix by $I$ leaves that matrix unchanged ($AI = IA = A$).

### Why is matrix multiplication not commutative ($AB \neq BA$)?

Because the order in which you apply transformations matters geometrically.

- Let $A$ be a matrix that rotates space by $90^\circ$.
    
- Let $B$ be a matrix that shears space horizontally.
    
- If you shear space first and then rotate it ($AB$), you get a completely different geometric result than if you rotate space first and then shear it ($BA$).
    

### Can a non-square matrix have a true inverse?

No. A matrix must be square ($n \times n$) to have a true, two-sided inverse ($A^{-1}A = AA^{-1} = I$). Non-square matrices map vectors between spaces of different dimensions (e.g., from 2D to 3D). You cannot have a seamless two-sided inverse because you cannot map space into a higher dimension without leaving gaps, and you cannot map space into a lower dimension without collapsing information.

_(Note: We can have one-sided "left" or "right" inverses, which we will explore in later lectures)._

# Common Mistakes

> [!warning] The Scalar Cancellation Trap In scalar algebra, if $ab = ac$ and $a \neq 0$, you can cancel $a$ to get $b = c$. **This does not work in matrix algebra.** > If $AB = AC$, you cannot assume $B = C$ unless you know for certain that $A$ is invertible. If $A$ is singular, it can squash different matrices into the same output space.

> [!warning] The Order Reversal Oversight When applying operations to both sides of an equation, you must multiply on the exact same side. If you start with $A = B$:
> 
> - It is correct to write: $CA = CB$ (multiplying on the left).
>     
> - It is correct to write: $AC = BC$ (multiplying on the right).
>     
> - **It is completely wrong to write:** $CA = BC$**.**
>     

# Summary

Lecture 3 shifts our perspective of matrices from static objects to dynamic, spatial operators.

The story of the lecture is the transition from calculation to action. We start by unpacking matrix multiplication $C = AB$, realizing it is not a singular formula but a rich tapestry of four distinct viewpoints. The most crucial viewpoint is the column view: **each column of** $C$ **is a linear combination of the columns of** $A$. This view explains why matrix multiplication represents the composition of transformations—it tracks how the basis vectors of one space are mapped and combined by the next operator.

With this operational model established, the lecture introduces the concept of reversing these transformations. The inverse matrix $A^{-1}$ exists to undo the spatial mapping of $A$.

However, this reversal is only possible if $A$ preserves the dimensionality of the space. If $A$ is singular, it collapses space, destroying information and making inversion impossible.

Finally, the lecture shows us how to calculate this reverse map using Gauss-Jordan elimination, demonstrating that by systematically simplifying $A$ to $I$ on one side of an augmented matrix, we construct the inverse $A^{-1}$ on the other.

# Cheat Sheet

### Four Ways to Compute $AB = C$

1. **Entry** $(i,j)$**:** (Row $i$ of $A$) $\cdot$ (Column $j$ of $B$)
    
2. **Columns of** $C$**:** Column $j$ of $C$ is a linear combination of all columns of $A$ using weights from Column $j$ of $B$.
    
3. **Rows of** $C$**:** Row $i$ of $C$ is a linear combination of all rows of $B$ using weights from Row $i$ of $A$.
    
4. **Outer Products:** $C$ is the sum of (Column $k$ of $A$) $\times$ (Row $k$ of $B$) for all $k$.
    

### Crucial Properties of Multiplication

- **Associative:** $(AB)C = A(BC)$
    
- **Distributive:** $A(B + C) = AB + AC$
    
- **NOT Commutative:** $AB \neq BA$ (in general)
    

### Crucial Properties of Inverses

- **Product Inverse:** $(AB)^{-1} = B^{-1}A^{-1}$
    
- **Transpose Inverse:** $(A^T)^{-1} = (A^{-1})^T$
    
- For a $2 \times 2$ matrix $A = \begin{bmatrix} a & b \\ c & d \end{bmatrix}$, it is invertible if $ad - bc \neq 0$, and:
    
    $$A^{-1} = \frac{1}{ad-bc} \begin{bmatrix} d & -b \\ -c & a \end{bmatrix}$$

# Intuition Notebook

## What mathematical object was introduced?

The **Inverse Matrix (**$A^{-1}$**)** and the concept of **Matrix Composition (**$AB$**)**.

## Why did mathematicians invent it?

To solve systems of equations $Ax = b$ algebraically without using division (which is undefined for matrices), and to calculate the net effect of applying multiple linear transformations in sequence.

## What does it measure, represent, or transform?

It represents the "reverse-gear" of a linear transformation. It measures whether a transformation preserves the dimensionality of space (invertible) or collapses it (singular).

## What stays invariant?

Under all transformations and their inverses, the **origin** $(0,0,\dots,0)$ **remains perfectly locked at the center of space**.

## How does it connect to previous lectures?

It provides the algebraic meaning behind the elimination steps in Lecture 2. The product of the elimination matrices $E_{32}E_{31}E_{21}$ is the master operator that reduces $A$ to $U$.

[[Lecture2_The_Elimination_of_Matrices]]
[[Lecture4_Factorization_into_A=LU]]