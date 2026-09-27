
**Core Insight:** Solving $Ax=0$ is not about finding a single hidden vector. It is about discovering the _internal structural redundancies_ of a matrix machine—identifying which inputs collapse to total zero, how many free directions exist, and building a complete geometric subspace to describe them all.

## Lecture Overview

In Lecture 6, we introduced the concept of the **Null Space** of a matrix $A$ as the abstract collection of all input vectors $x$ satisfying $Ax = 0$. Lecture 7 bridges the gap between abstract definition and concrete computation. While the equation $Ax = 0$ appears deceptively simple—because the target on the right is merely the zero vector—it holds the key to understanding matrix structure. If the only solution is $x = 0$, the matrix preserves every input direction uniquely. However, if non-zero solutions exist, the matrix compresses space, wiping out certain directions completely.

This lecture details the explicit algebraic process—using elimination—to find every single input vector that is mapped to zero. Elimination transforms $A$ into an upper triangular or echelon form $U$, exposing the fundamental dichotomy of variables: **pivot variables** (which are constrained by equations) and **free variables** (which have complete freedom). By systematically setting free variables to basic values (such as 1 and 0), we isolate the core "building block" solutions called **special solutions**. Ultimately, we prove that the linear combinations of these special solutions construct the entire Null Space, establishing its dimension and basis.

## Big Picture

Before launching into Gaussian elimination, let us establish the core conceptual motivation behind studying $Ax = 0$:

- **Why study $Ax = 0$?** In science and engineering, systems are rarely perfectly rigid. $Ax = 0$ asks: _Are there non-trivial inputs that yield zero output?_ In mechanics, non-zero solutions represent structural collapses or self-stresses. In differential equations, they represent natural frequencies or homeostatic steady states. In data science, they represent redundant features.
    
- **Why is $x = 0$ always a solution?** Linear transformations preserve the origin: $A(0) = 0$. The zero vector is always in the Null Space. It is the "trivial" solution that tells us nothing about the matrix's unique structure.
    
- **Why hunt for non-zero solutions?** A non-zero solution $x \neq 0$ reveals that the matrix is _singular_ (non-invertible) or non-square with dependent columns. It tells us that different distinct inputs can produce the exact same output.
    
- **Geometric meaning of a solution:** Geometrically, a linear transformation takes vectors from an input space $\mathbb{R}^n$ and lands them in an output space $\mathbb{R}^m$. A non-zero solution to $Ax = 0$ is a vector pointing along a direction that gets entirely squashed into a single zero point.
    
- **Why elimination works:** Elimination applies row operations (combining equations). Row operations do not change the truth of $Ax = 0$ because if a set of equations equals zero, any linear combination of those equations must also equal zero. Thus, elimination preserves the Null Space while turning a tangled system into a clear hierarchical system where dependencies become obvious.
    

## Connection to Lecture 6

Lecture 6 introduced the fundamental vector spaces associated with an $m \times n$ matrix $A$:

Column Space $C(A)$

The subspace of $\mathbb{R}^m$ formed by all possible linear combinations (outputs) of the columns of $A$. It answers: _Which right-hand sides $b$ can be solved in $Ax = b$?_

Null Space $N(A)$

The subspace of $\mathbb{R}^n$ formed by all input vectors $x$ that map to the zero vector in $\mathbb{R}^m$. It answers: _Which input vectors does the matrix send to zero?_

Recall matrix multiplication expressed as a linear combination of columns:

$$Ax = x_1 a_1 + x_2 a_2 + \dots + x_n a_n = 0$$

This formulation makes the explicit link clear: searching for a non-zero solution $x$ to $Ax = 0$ is identical to asking whether a non-trivial linear combination of the columns of $A$ yields the zero vector. If such an $x$ exists, the columns of $A$ are linearly dependent.

## The Homogeneous System

A system of linear equations is called **homogeneous** when all constant terms on the right-hand side are zero ($Ax = 0$).

Why is the solution set of a homogeneous system automatically a vector space (specifically, a subspace of $\mathbb{R}^n$)?

0. **Zero vector rule:** $A(0) = 0$, so $0 \in N(A)$.
    
1. **Closed under addition:** If $v$ and $w$ are in $N(A)$ (meaning $Av = 0$ and $Aw = 0$), then $A(v + w) = Av + Aw = 0 + 0 = 0$. Thus, $v + w \in N(A)$.
    
2. **Closed under scalar multiplication:** If $v \in N(A)$ and $c \in \mathbb{R}$, then $A(cv) = c(Av) = c(0) = 0$. Thus, $cv \in N(A)$.
    

Geometrically, this means solutions to $Ax=0$ cannot form arbitrary curved surfaces or detached lines. They must form flat geometric structures—lines, planes, or hyperplanes—that pass directly through the origin.

## Pivot Variables and Free Variables

When performing Gaussian elimination on a general $m \times n$ matrix, we do not always find a clean pivot in every column. Some columns will lack pivots.

Pivot Columns & Pivot Variables

Columns that contain a staircase step (a non-zero entry after row reduction). The corresponding components of $x$ are **pivot variables**. They represent constrained variables whose values are dictated by lower rows.

Free Columns & Free Variables

Columns that lack a pivot. The corresponding components of $x$ are **free variables**. They are unconstrained by the elimination process and can be assigned _any_ scalar value freely.

**Why do free variables appear?** Free variables appear when a column is a linear combination of the preceding columns. Elimination cleans out redundant information, turning duplicate or dependent column directions into rows of complete zeros. The free variables measure the total number of redundant dimensions (degrees of freedom) in your domain.

## A Complete Example

Consider the $3 \times 4$ matrix system $Ax = 0$:

$$A = \begin{bmatrix} 1 & 2 & 2 & 2 \\ 2 & 4 & 6 & 8 \\ 3 & 6 & 8 & 10 \end{bmatrix}$$

### Step 1: Row Reduction to Upper Echelon Form ($U$)

We perform standard elimination using row operations:

- Subtract $2 \times \text{Row 1}$ from Row 2: $R_2 \to R_2 - 2R_1$
    
- Subtract $3 \times \text{Row 1}$ from Row 3: $R_3 \to R_3 - 3R_1$
    

$$\begin{bmatrix} 1 & 2 & 2 & 2 \\ 0 & 0 & 2 & 4 \\ 0 & 0 & 2 & 4 \end{bmatrix}$$

Next, subtract Row 2 from Row 3: $R_3 \to R_3 - R_2$:

$$U = \begin{bmatrix} \mathbf{1} & 2 & 2 & 2 \\ 0 & 0 & \mathbf{2} & 4 \\ 0 & 0 & 0 & 0 \end{bmatrix}$$

### Step 2: Identify Pivot and Free Columns

|**Column**|**Type**|**Variable**|**Reason**|
|---|---|---|---|
|Column 1|**Pivot**|$x_1$|Contains first pivot ($1$) in Row 1|
|Column 2|**Free**|$x_2$|No pivot step|
|Column 3|**Pivot**|$x_3$|Contains second pivot ($2$) in Row 2|
|Column 4|**Free**|$x_4$|No pivot step|

Number of pivots $r = 2$. Number of columns $n = 4$. Number of free variables = $n - r = 4 - 2 = 2$.

### Step 3: Solve Backwards for Pivot Variables

Write down the equations represented by $Ux = 0$:

[\begin{aligned} 1x_1 + 2x_2 + 2x_3 + 2x_4 &= 0 \quad (\text{Row 1}) \ 2x_3 + 4x_4 &= 0 \quad (\text{Row 2}) \end{aligned}]

Express pivot variables ($x_1, x_3$) strictly in terms of free variables ($x_2, x_4$):

**1.Solve Row 2 for x_3:**

$2x_3 = -4x_4 \implies x_3 = -2x_4$

**2.Solve Row 1 for x_1:**

$x_1 = -2x_2 - 2x_3 - 2x_4 = -2x_2 - 2(-2x_4) - 2x_4 = -2x_2 + 2x_4$

## Special Solutions

To construct a convenient basis for all solutions, we systematically assign 1 to one free variable and 0 to all other free variables. Because there are 2 free variables ($x_2, x_4$), we construct **2 special solutions**:

### Special Solution 1: Set $x_2 = 1, x_4 = 0$

- $x_3 = -2(0) = 0$
    
- $x_1 = -2(1) + 2(0) = -2$
    

$$s_1 = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{bmatrix} = \begin{bmatrix} -2 \\ 1 \\ 0 \\ 0 \end{bmatrix}$$

### Special Solution 2: Set $x_2 = 0, x_4 = 1$

- $x_3 = -2(1) = -2$
    
- $x_1 = -2(0) + 2(1) = 2$
    

$$s_2 = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \\ x_4 \end{bmatrix} = \begin{bmatrix} 2 \\ 0 \\ -2 \\ 1 \end{bmatrix}$$

**Why set free variables to 1 and 0?** Setting free variables to choices like $(1,0)$ and $(0,1)$ guarantees that the resulting special solutions are **linearly independent**. Look at rows 2 and 4 of $s_1$ and $s_2$: they contain $\begin{bmatrix}1\\0\end{bmatrix}$ and $\begin{bmatrix}0\\1\end{bmatrix}$, making it impossible for one special solution to be a multiple of another!

## Null Space as a Span

Every single solution $x$ to $Ax = 0$ can be expressed as a linear combination of the special solutions $s_1$ and $s_2$:

$$x = x_2 \begin{bmatrix} -2 \\ 1 \\ 0 \\ 0 \end{bmatrix} + x_4 \begin{bmatrix} 2 \\ 0 \\ -2 \\ 1 \end{bmatrix} = c_1 s_1 + c_2 s_2$$

Therefore, the **Null Space is the span of the special solutions**:

$$N(A) = \text{span}(s_1, s_2)$$

Because $s_1$ and $s_2$ are linearly independent and span $N(A)$, they form a **basis** for the Null Space. The dimension of the Null Space is equal to the number of free variables ($n - r = 4 - 2 = 2$).

## Geometric Interpretation

Below is a dynamic visualizer illustrating how the number of free variables determines the geometric structure of the Null Space in 3D input space.

Visualization of Null Space dimensions based on free variable counts.

## Pivot Variables vs. Free Variables

Comparison of Pivot and Free Variables
|**Attribute**|**Pivot Variables**|**Free Variables**|
|---|---|---|
|Definition|Variables corresponding to columns containing pivot steps.|Variables corresponding to columns without pivots.|
|Role|Dependent variables constrained by row equations.|Independent variables representing degrees of freedom.|
|Can we choose freely?|**No**. Derived strictly via back-substitution.|**Yes**. Can take any scalar value in $\mathbb{R}$.|
|What determines them?|Calculated from free variable choices during back-substitution.|Matrix column dependencies (columns that are linear combinations of earlier ones).|
|Geometric Meaning|Fixed dimensions bound to balance matrix constraints.|Free directions along which solutions can extend infinitely.|

## Matrix Interpretation: The "Machine" Viewpoint

Think of matrix multiplication as a machine process taking an input vector $x$ and producing output vector $b$:

$$x \xrightarrow{\quad A \quad} b = Ax$$

When solving $Ax = 0$, we look for all inputs that cause the machine output to completely collapse to zero.

SVG

Each **special solution** represents a single, fundamental, independent instruction to turn off the machine output. If a machine maps multiple distinct inputs to zero, the machine is non-invertible—information is destroyed upon transformation, and you cannot reverse the process to find which original vector created the zero output.

## Connection to Linear Dependence & Rank

The existence of free variables is directly tied to column dependence and rank:

- **Linear Dependence:** Non-zero solutions to $Ax = 0$ exist **if and only if** the columns of $A$ are linearly dependent. If all columns are independent, there are no free variables; thus $N(A) = \{0\}$.
    
- **Rank ($r$):** The number of pivots resulting from elimination is called the **rank** of $A$. It measures the number of truly independent rows and columns.
    
- **Nullity ($n - r$):** The number of free variables is the dimension of the Null Space, called **nullity**.
    

**The Rank-Nullity Theorem:**

$$\text{Rank} + \text{Nullity} = n \implies r + (n - r) = n$$

_Total Variables ($n$) = Pivot Variables ($r$) + Free Variables ($n - r$)_

  

This fundamental equation states that the total dimensional capacity of the domain ($n$) is partitioned into two parts: dimensions preserved in the output (Rank $r$) and dimensions squashed to zero (Nullity $n-r$).

## Common Misconceptions

Misconception 1: "The only solution to $Ax = 0$ is always $x = 0$."

**Correction:** $x = 0$ is always _a_ solution (the trivial solution), but whenever there are free variables ($n > r$), there are infinitely many non-zero solutions.

Misconception 2: "Free variables are variables we don't care about."

**Correction:** Free variables are the parameters that define the degrees of freedom of the solution space. They generate the entire Null Space!

Misconception 3: "Setting a free variable to 1 is mathematically arbitrary."

**Correction:** Setting free variables to 1 and 0 is a precise method to extract a clean, independent **basis**. Any non-zero choice yields valid solutions, but the standard $(1,0)$, $(0,1)$ choice guarantees linearly independent special solutions.

Misconception 4: "Special solutions are the complete solution."

**Correction:** A single special solution is just one vector. The complete solution is the set of **all linear combinations** of all special solutions ($c_1 s_1 + c_2 s_2 + \dots$).

## Connection to Previous Lectures

Lecture 1 & 2: Linear Equations & Vectors

Learned row view (intersecting hyperplanes) vs. column view (combining column vectors).

Lecture 3 & 4: Matrix Operations & Elimination (LU)

Mastered Gaussian elimination to simplify square systems with full pivots.

Lecture 5 & 6: Vector Spaces, Subspaces, C(A), N(A)

Defined vector spaces and introduced the Null Space abstractly as a subspace satisfying $Ax = 0$.

Lecture 7: Solving Ax = 0 (Pivot & Free Variables)

Applied elimination to rectangular matrices to explicitly derive pivots, free variables, special solutions, and construct a full basis for $N(A)$.

## Cheat Sheet

### Key Definitions & Formulas

- **Pivot Variable:** Variable corresponding to a column with a pivot.
    
- **Free Variable:** Variable corresponding to a column without a pivot.
    
- **Special Solution:** Solution obtained by setting one free variable to 1 and all other free variables to 0.
    
- **Rank-Nullity Theorem:** $\text{dim}(N(A)) = n - r$
    

### Procedure for Solving $Ax = 0$

**1.Perform Row Elimination:**

Reduce matrix $A$ to upper echelon form $U$.

**2.Identify Pivots:**

Count rank $r$ (number of pivots) and free variables ($n - r$).

**3.Set Free Variables:**

For each free variable, create a special solution by setting it to 1 and others to 0.

**4.Back-Substitute:**

Solve for pivot variables in terms of free variables to populate each special solution vector $s_i$.

**5.Write Complete Solution:**

Express general solution as linear combination: $x = c_1 s_1 + c_2 s_2 + \dots + c_{n-r} s_{n-r}$.

## Worked Example Without Numbers (Symbolic)

Suppose row elimination reduces a system to the echelon form system:

$$\begin{aligned} x_1 + 2x_3 &= 0 \\ x_2 - 5x_3 &= 0 \end{aligned}$$

0. **Identify variables:** Columns 1 and 2 have pivots ($x_1, x_2$ are pivot variables). Column 3 has no pivot ($x_3$ is free).
    
1. **Express pivots in terms of free:**
    
    $$x_1 = -2x_3, \quad x_2 = 5x_3$$
    
2. **Set free variable $x_3 = 1$:**
    
    $$s_1 = \begin{bmatrix} x_1 \\ x_2 \\ x_3 \end{bmatrix} = \begin{bmatrix} -2(1) \\ 5(1) \\ 1 \end{bmatrix} = \begin{bmatrix} -2 \\ 5 \\ 1 \end{bmatrix}$$
    
3. **Write Complete Solution (Null Space):**
    
    $$N(A) = \left\{ c \begin{bmatrix} -2 \\ 5 \\ 1 \end{bmatrix} \;\middle\vert{}\; c \in \mathbb{R} \right\}$$
    

## Intuition Notebook

What is a homogeneous system?

A system $Ax = 0$ where the target vector is strictly zero.

Why is $x = 0$ always a solution?

Because multiplying any linear transformation matrix by zero yields zero.

What does a free variable actually represent?

An independent choice or degree of freedom where an input vector can move without changing the zero output.

What does a pivot variable represent?

A constrained variable whose value is strictly locked by the matrix equations to balance the free variable choices.

Why do special solutions form a basis for the Null Space?

They are linearly independent (due to the 1/0 pattern in free variable slots) and span all possible solutions.

## Final Mental Model

$$Ax=0 \;\;\longrightarrow\;\; \text{Find constraints created by equations}$$

$$\;\;\longrightarrow\;\; \text{Gaussian elimination reveals pivots } (r)$$

$$\;\;\longrightarrow\;\; \text{Columns without pivots create free variables } (n - r)$$

$$\;\;\longrightarrow\;\; \text{Free variables represent independent directions}$$

$$\;\;\longrightarrow\;\; \text{Set free variables to }(1,0), (0,1)\dots \text{ to produce Special Solutions } s_i$$

$$\;\;\longrightarrow\;\; \text{Special Solutions form a Basis for the Null Space } N(A)$$

$$\;\;\longrightarrow\;\; \text{Complete solution: } x = c_1 s_1 + c_2 s_2 + \dots + c_{n-r} s_{n-r}$$

[[Lecture6_Column_&_Null_space]]