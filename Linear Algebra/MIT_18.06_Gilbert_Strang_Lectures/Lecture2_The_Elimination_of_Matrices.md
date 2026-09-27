
This lecture details the systematic process of solving a system of linear equations $Ax = b$ through **Gaussian Elimination** and shows how to represent these row operations algebraically as matrix multiplications. By translating physical row transformations into "elimination matrices" $E_{ij}$, we transition from computational arithmetic to structural matrix algebra. Ultimately, the lecture builds the foundation for understanding matrix factorizations, specifically the $LU$ decomposition, by viewing elimination as a systematic way of simplifying a matrix into an upper triangular form $U$.

# Big Picture

### What mathematical problem is this lecture trying to solve?

We want to solve a system of $n$ linear equations with $n$ unknowns, represented as $Ax = b$, in the most computationally efficient and systematic way possible. While Lecture 1 showed us what a solution means geometrically (intersection of hyperplanes or combination of column vectors), it did not give us an automated, algorithmic method to _find_ that solution for any arbitrary, large-scale system.

### Why was this concept invented?

Gaussian elimination was designed to reduce complex, highly coupled linear systems into simple, uncoupled systems that can be solved trivially. If every equation contains every variable, you must look at all of them at once. If we can systematically eliminate variables one by one, we decouple the equations. This allows us to solve for one variable at a time from the bottom up, a process known as **back substitution**.

### How does it connect to Lecture 1?

Lecture 1 established the identity $Ax = b$ and explored its geometry. Lecture 2 introduces the actual engine that solves this equation. Crucially, it connects the geometric row transformations of elimination directly to matrix multiplication. We learn that multiplying a matrix $A$ on the left by a specially designed matrix $E$ performs a row operation. This bridges the gap between raw computational steps (subtracting Row 1 from Row 2) and clean matrix algebra ($EA$).

### Where does it fit in the larger world of linear algebra?

Elimination is the fundamental computational workhorse of linear numerical algebra. Almost every software package (like NumPy or MATLAB) solves systems of equations using a variation of this method. More importantly, understanding elimination matrices ($E_{ij}$) and permutation matrices ($P$) is the direct gateway to **Matrix Factorization** ($A = LU$), which is the algebraic description of the elimination process.

# Core Concepts

## 1. Gaussian Elimination (Pivots and Multipliers)

### What is it?

Gaussian Elimination is a systematic algorithm that performs row operations on a system of linear equations to transform the coefficient matrix $A$ into an **upper triangular matrix** $U$ (where all entries below the main diagonal are zero).

### Why does it exist?

Without a systematic algorithm, solving equations is a chaotic process of guessing which equation to substitute where. Gaussian elimination provides a deterministic, step-by-step pipeline that is easily programmable and guarantees a solution (or proves that no solution exists) in a predictable number of steps.

### Intuition

Think of a system of equations as a tangled knot of variables. Elimination systematically unties the knot by isolating one variable per row. By the time we reach the bottom row, we have a single equation with a single variable, which is trivially solved. We then propagate this known value back up through the system to unlock the rest of the variables.

### Mathematical Definition

For a matrix $A \in \mathbb{R}^{n \times n}$:

1. Identify the **pivot** $d_{ii}$ at position $(i, i)$. The pivot must be non-zero ($d_{ii} \neq 0$).
    
2. For each row $j > i$ below the pivot, calculate the **multiplier**:
    
    $$\ell_{ji} = \frac{a_{ji}}{a_{ii}}$$
3. Subtract $\ell_{ji}$ times row $i$ from row $j$ to produce a $0$ at position $(j, i)$.
    
4. Repeat this process for all diagonal elements $i = 1, \dots, n-1$ until $A$ is transformed into the upper triangular matrix $U$:
    
    $$U = \begin{bmatrix} d_{11} & u_{12} & \dots & u_{1n} \\ 0 & d_{22} & \dots & u_{2n} \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \dots & d_{nn} \end{bmatrix}$$

### Step-by-Step Example

Let us perform elimination on the coefficient matrix:

$$A = \begin{bmatrix} 2 & 1 & 1 \\ 4 & -6 & 0 \\ -2 & 7 & 2 \end{bmatrix}$$

**Step 1: Eliminate under the first pivot.**

- The first pivot is $a_{11} = \mathbf{2}$ (non-zero, so we can proceed).
    
- We want to eliminate the $4$ in row 2, column 1. The multiplier is:
    
    $$\ell_{21} = \frac{a_{21}}{a_{11}} = \frac{4}{2} = 2$$
    
    Subtract $2 \times (\text{Row 1})$ from $\text{Row 2}$:
    
    $$\text{Row 2}_{\text{new}} = [4, -6, 0] - 2 \times [2, 1, 1] = [4 - 4, -6 - 2, 0 - 2] = [0, -8, -2]$$
- We want to eliminate the $-2$ in row 3, column 1. The multiplier is:
    
    $$\ell_{31} = \frac{a_{31}}{a_{11}} = \frac{-2}{2} = -1$$
    
    Subtract $-1 \times (\text{Row 1})$ from $\text{Row 3}$ (which is equivalent to adding $\text{Row 1}$ to $\text{Row 3}$):
    
    $$\text{Row 3}_{\text{new}} = [-2, 7, 2] - (-1) \times [2, 1, 1] = [0, 8, 3]$$

Our matrix now looks like:

$$A^{(1)} = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 8 & 3 \end{bmatrix}$$

**Step 2: Eliminate under the second pivot.**

- The second pivot is the element at $(2, 2)$, which is $a^{(1)}_{22} = \mathbf{-8}$ (non-zero, so we can proceed).
    
- We want to eliminate the $8$ in row 3, column 2. The multiplier is:
    
    $$\ell_{32} = \frac{a^{(1)}_{32}}{a^{(1)}_{22}} = \frac{8}{-8} = -1$$
    
    Subtract $-1 \times (\text{Row 2})$ from $\text{Row 3}$ (equivalent to adding $\text{Row 2}$ to $\text{Row 3}$):
    
    $$\text{Row 3}_{\text{new}} = [0, 8, 3] - (-1) \times [0, -8, -2] = [0, 0, 1]$$

Our final upper triangular matrix $U$ is:

$$U = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix}$$

The three pivots of the matrix are $\mathbf{2, -8, \text{ and } 1}$.

### Common Misconceptions

- **Choosing Zero as a Pivot:** Beginners often try to use $0$ as a pivot. This is mathematically impossible because the multiplier calculation $\ell_{ji} = \frac{a_{ji}}{a_{ii}}$ would require division by zero. If a zero appears in a pivot position, you must swap rows with a row below it to put a non-zero number in the pivot slot.
    
- **Confusing Pivots with Original Elements:** The pivots are _not_ the diagonal elements of the original matrix $A$. They are the diagonal elements of the matrix _as they exist at the moment they are used to eliminate entries below them_. In the example above, the second pivot was $-8$, not the original $-6$.
    

## 2. Elimination Matrices ($E_{ij}$)

### What is it?

An elimination matrix $E_{ij}$ is an identity matrix modified by a single off-diagonal element. When you multiply a matrix $A$ on the left by $E_{ij}$ (i.e., $E_{ij}A$), it performs a row operation: it subtracts a multiple of row $j$ from row $i$.

### Why does it exist?

In computer science and algebra, we cannot write "subtract 2 times row 1 from row 2" as a mathematical operator. We need a way to express this physical action as a standard algebraic matrix multiplication. The $E_{ij}$ matrix turns physical row manipulation into formal algebra.

### Intuition

Multiplication by the identity matrix $IA$ leaves $A$ completely unchanged. If we want to change only Row $i$ by subtracting a multiple $m$ of Row $j$, we should modify the identity matrix _only_ in Row $i$, placing the value $-m$ in column $j$. When this modified identity matrix acts on $A$, it carries out that precise instruction.

### Mathematical Definition

To construct the elimination matrix $E_{ji}$ that subtracts $\ell_{ji}$ times row $i$ from row $j$: Start with the identity matrix $I_n$. Replace the zero at row $j$, column $i$ with $-\ell_{ji}$.

$$E_{ji} = \begin{bmatrix} 1 & 0 & \dots & 0 \\ \vdots & \ddots & \dots & \vdots \\ -\ell_{ji} & \dots & 1 & \dots \\ \vdots & \dots & \dots & \ddots \end{bmatrix}$$

### Step-by-Step Example

Let us write the elimination matrix $E_{21}$ that subtracts $2 \times (\text{Row 1})$ from $\text{Row 2}$ for a $3 \times 3$ system.

- Start with the $3 \times 3$ identity matrix:
    
    $$I = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
- We want to target Row 2, Column 1. Put the negative multiplier $-\ell_{21} = -2$ in that slot:
    
    $$E_{21} = \begin{bmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
- To verify, multiply $E_{21}$ by our original matrix $A$:
    
    $$E_{21}A = \begin{bmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & 1 \\ 4 & -6 & 0 \\ -2 & 7 & 2 \end{bmatrix} = \begin{bmatrix} 2 & 1 & 1 \\ 4-2(2) & -6-2(1) & 0-2(1) \\ -2 & 7 & 2 \end{bmatrix} = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ -2 & 7 & 2 \end{bmatrix}$$
    
    It worked perfectly.
    

### Common Misconceptions

- **Left vs. Right Multiplication:** Beginners often multiply on the right ($AE_{ij}$). Left multiplication ($EA$) performs **row** operations. Right multiplication ($AE$) performs **column** operations. Since elimination is a row-based process, we must always multiply on the left.
    
- **Sign Confusions:** The value placed in the matrix is the **negative** of the multiplier ($-\ell_{ji}$). If you want to subtract $2$ times row 1, the entry must be $-2$. If you want to add $1$ times row 1 (meaning the multiplier was $-1$), the entry must be $-(-1) = 1$.
    

## 3. Permutation Matrices ($P_{ij}$)

### What is it?

A permutation matrix $P_{ij}$ is an identity matrix with rows $i$ and $j$ swapped. Multiplying $P_{ij}A$ swaps row $i$ and row $j$ of matrix $A$.

### Why does it exist?

During elimination, we might encounter a zero in the pivot position (e.g., $a_{ii} = 0$). Since we cannot divide by zero to find our multipliers, the algorithm will halt. If there is a non-zero entry below this position, we can swap the current row with the row below it to rescue the elimination process. We need a matrix operator to represent this swap algebraically.

### Intuition

The identity matrix $I$ maps Row 1 of the output to Row 1 of the input, Row 2 to Row 2, etc. If we swap Row 1 and Row 2 of the identity matrix, we are instructing the multiplication process to map Row 1 of the output to Row 2 of the input, and Row 2 of the output to Row 1 of the input.

### Mathematical Definition

To swap Row $i$ and Row $j$ of an $n \times n$ matrix, construct $P_{ij}$ by swapping Row $i$ and Row $j$ of the $n \times n$ Identity matrix.

$$P_{ij} = \text{Identity with Row } i \text{ and Row } j \text{ exchanged.}$$

### Step-by-Step Example

Suppose we have a system where a zero appears in the first pivot slot:

$$A = \begin{bmatrix} 0 & 2 & 3 \\ 1 & 1 & 1 \\ 3 & 4 & 5 \end{bmatrix}$$

We need to swap Row 1 and Row 2 to place the non-zero element $1$ in the pivot position.

- Take the $3 \times 3$ identity matrix and swap Row 1 and Row 2:
    
    $$P_{12} = \begin{bmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$
- Multiply $P_{12}A$:
    
    $$P_{12}A = \begin{bmatrix} 0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 0 & 2 & 3 \\ 1 & 1 & 1 \\ 3 & 4 & 5 \end{bmatrix} = \begin{bmatrix} 1 & 1 & 1 \\ 0 & 2 & 3 \\ 3 & 4 & 5 \end{bmatrix}$$
    
    Row 1 and Row 2 have been swapped, and our new first pivot is $\mathbf{1}$.
    

### Common Misconceptions

- **Temporary vs. Permanent Failure:** A zero in the pivot position is only a _temporary_ failure of elimination if there is a non-zero entry somewhere below it in that column. If the entire column below the diagonal slot is also filled with zeros, row swapping cannot save us. This is a _permanent_ failure, meaning the matrix is singular and does not have a full set of pivots.
    

# Visual Interpretation

Elimination has a beautiful geometric meaning. Let's look at what is changing under the hood.

### What moves?

In the **Row Picture**, as we perform elimination, the intersecting planes are rotating in space.

- When we subtract a multiple of Row 1 from Row 2, the plane represented by Row 2 tilts and swings.
    
- Crucially, it rotates around the line where Plane 1 and Plane 2 intersect.
    

### What stays constant?

The solution point $x$ (the point in space where all the planes meet) **never moves**.

- Even though the planes are rotating and changing their orientations, they are constrained to pivot along their shared intersection boundaries. The final upper triangular system of planes $Ux = c$ intersects at the exact same spatial coordinate as the original system $Ax = b$.
    

### What is being measured?

The "slopes" or alignment of the planes is being aligned to our coordinate axes.

- The first equation (Row 1) remains untouched.
    
- The second equation (Row 2) is tilted until it is parallel to the z-axis (meaning it no longer contains the $x$ variable).
    
- The third equation (Row 3) is tilted until it is parallel to both the x and y axes (meaning it contains only the $z$ variable).
    

### What information is preserved?

The **vector space spanned by the rows** (the Row Space) is preserved. We are not adding new information or losing existing constraints; we are simply changing our frame of reference to make the coordinate values read out directly.

# Key Proofs / Derivations

### Derivation: Why Left-Multiplying by $E_{21}$ Performs Row Subtraction

Let us prove why multiplying a matrix $A$ on the left by $E_{21} = \begin{bmatrix} 1 & 0 & 0 \\ -c & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$ subtracts $c$ times Row 1 from Row 2.

Let the rows of the matrix $A$ be represented as row vectors:

$$A = \begin{bmatrix} \text{Row}_1 \\ \text{Row}_2 \\ \text{Row}_3 \end{bmatrix} = \begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix}$$

We compute the product $E_{21}A$:

$$E_{21}A = \begin{bmatrix} 1 & 0 & 0 \\ -c & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} a_{11} & a_{12} & a_{13} \\ a_{21} & a_{22} & a_{23} \\ a_{31} & a_{32} & a_{33} \end{bmatrix}$$

Let's compute each row of the resulting matrix using the row-combination view of matrix multiplication (where Row $i$ of the result is a linear combination of the rows of $A$, weighted by Row $i$ of the left matrix):

1. **Row 1 of the result:**
    
    $$\text{Result}_1 = 1 \times \text{Row}_1 + 0 \times \text{Row}_2 + 0 \times \text{Row}_3 = \text{Row}_1$$
    
    _(Row 1 remains completely unchanged)_
    
2. **Row 2 of the result:**
    
    $$\text{Result}_2 = -c \times \text{Row}_1 + 1 \times \text{Row}_2 + 0 \times \text{Row}_3 = \text{Row}_2 - c\,\text{Row}_1$$
    
    _(This is exactly the row operation we wanted: Row 2 minus_ $c$ _times Row 1)_
    
3. **Row 3 of the result:**
    
    $$\text{Result}_3 = 0 \times \text{Row}_1 + 0 \times \text{Row}_2 + 1 \times \text{Row}_3 = \text{Row}_3$$
    
    _(Row 3 remains completely unchanged)_
    

This proves that left multiplication by $E_{ji}$ acts purely as a local row operation on Row $j$, subtracting $c$ times Row $i$ while leaving all other rows untouched.

# Connections

### Connection to Vectors and Matrices

Row operations can be represented as matrix operations. This shows us that matrices are not just static boxes of numbers; they are **active operators** that can transform other matrices.

### Connection to Linear Equations

The augmented matrix $[A \mid b]$ reminds us that whatever row operations we perform on the coefficient matrix $A$ (to turn it into $U$), we must also perform on the target vector $b$ (to turn it into a new vector $c$). This keeps the system balanced, maintaining the identity:

$$Ax = b \iff Ux = c$$

### Connection to Computer Science & Complexity

Solving an $n \times n$ system using elimination requires about $\frac{1}{3}n^3$ floating-point operations (flops).

- Eliminating the first column takes about $n^2$ operations.
    
- Eliminating the next takes $(n-1)^2$, then $(n-2)^2$, and so on.
    
- The sum of squares $\sum_{k=1}^n k^2 \approx \frac{1}{3}n^3$ dictates that doubling the size of our system of equations leads to an **8x increase** in computing time. This makes algorithmic efficiency and matrix structures incredibly important for large-scale engineering systems.
    

# Important Properties

|Property / Matrix Type|Mathematical Meaning|Physical / Numerical Significance|Why It Matters|
|---|---|---|---|
|**Upper Triangular (**$U$**)**|$u_{ij} = 0$ for all $i > j$ (all entries below diagonal are zero).|Represents a completely decoupled system.|Allows trivial resolution of variables via back-substitution.|
|**Pivots**|The diagonal entries $d_{ii}$ of $U$.|They are the scale factors of our coordinates.|If any pivot is $0$, the matrix is singular and cannot be inverted.|
|**Permutation (**$P$**)**|Identity matrix with swapped rows.|Swaps rows or reorders equations.|Crucial for computational stability and handling zero-pivots.|
|**Invertibility of** $E_{ij}$|$E_{ij}^{-1}$ is the same matrix but with the sign of the multiplier flipped.|Reverses the row subtraction (adds back what was subtracted).|The key to reconstructing $A$ from $U$, leading directly to $A = LU$.|

# Mental Models

Instead of thinking of row reduction as a manual pencil-and-paper calculation, try using these mental models:

- **Instead of saying:** "Subtract row 1 from row 2." **Think:** "Left-multiply by the elimination matrix $E_{21}$."
    
- **Instead of thinking:** "A matrix is a static grid of numbers." **Think:** "A matrix is an action movie. Left-multiplying is an instruction to perform row transformations; right-multiplying is an instruction to perform column transformations."
    
- **Instead of thinking:** "Elimination changes our system of equations." **Think:** "Elimination rotates our geometric coordinate system to align with the axes while holding the solution point perfectly still."
    

# Frequently Asked Questions

### Why do we always perform row operations instead of column operations when solving $Ax = b$?

Because $Ax = b$ is a linear combination of the **columns** of $A$. If we performed column operations, we would be changing the columns themselves—which means we would be changing the meaning of our variables $x$. By performing **row** operations, we are simply combining our existing constraint equations. This changes the coefficients on both sides of the equal sign but keeps the variables $x$ completely intact.

### What happens if we run out of non-zero entries during row swapping?

If you have a zero in a pivot position and every single entry below it in that column is also zero, you cannot find a pivot. This means the matrix is **singular**. It has fewer than $n$ pivots. Geometrically, this means your equations do not define a unique point in space—instead, the planes intersect along an infinite line, or do not intersect at all.

### Is the order of elimination matrices important?

Yes, absolutely. Matrix multiplication is **not commutative** ($AB \neq BA$). The order in which you apply the elimination matrices must match the chronological steps of your elimination algorithm. For example, you must apply $E_{21}$ first, and then apply $E_{32}$ to the result:

$$E_{32}(E_{21}A) \neq E_{21}(E_{32}A)$$

# Summary

Lecture 2 outlines the systematic path to solving $Ax = b$ via Gaussian Elimination.

```
   [ Coeff Matrix A ]  ---( Left-Multiply by E_ji )--->  [ Upper Triangular U ]
          |                                                       |
   [ Target Vector b ] ---( Perform Same Row Ops )----->  [ Scaled Target c ]
          \                                                       /
           \---> Solved trivially using Back-Substitution <------/
```

We start with a coupled coefficient matrix $A$. By identifying diagonal **pivots** and calculating **multipliers**, we systematically clear out all numbers below the diagonal, converting $A$ into an upper triangular matrix $U$.

If a zero lands in a pivot slot, we apply a row swap via a **Permutation Matrix** $P$. If no non-zero element can be found below the slot, the system is singular and cannot be solved uniquely.

Crucially, every single row calculation is translated into formal algebra by multiplying on the left by an **Elimination Matrix** $E_{ij}$. This shows us that the whole computational journey from $A$ to $U$ can be represented as a clean, continuous string of matrix multiplications:

$$(E_{32} E_{31} E_{21}) A = U$$

# Cheat Sheet

### Essential Formulas

- **Multiplier:** $\ell_{ji} = \frac{\text{Entry to eliminate}}{\text{Pivot}} = \frac{a_{ji}}{a_{ii}}$
    
- **Elimination Matrix construction (**$E_{ji}$**):** Place $-\ell_{ji}$ in row $j$, column $i$ of the Identity Matrix.
    
- **System Equivalence:** $Ax = b \iff Ux = c$
    
- **Computational Cost:** Approximately $\frac{1}{3}n^3$ arithmetic operations for an $n \times n$ system.
    

### Rules to Memorize

- **Left-multiplying** a matrix by a row vector or another matrix performs **row** operations.
    
- **Right-multiplying** a matrix performs **column** operations.
    
- **Pivots can never be zero.** If a zero occurs on the diagonal, you must swap rows using a Permutation Matrix.
    
- If a full set of $n$ pivots cannot be found after all row swaps are attempted, the matrix is **singular** (non-invertible).
    

# Intuition Notebook

## What object was introduced?

The **Elimination Matrix (**$E_{ij}$**)** and the **Permutation Matrix (**$P$**)**.

## Why did mathematicians invent it?

To express physical, computational row-reduction operations (adding, subtracting, and swapping rows) as formal algebraic multiplications. This allowed them to analyze the elimination process mathematically using matrix algebra.

## What does it measure or describe?

It describes the step-by-step structural decoupling of a system of equations. It tracks how much of each equation's constraints are distributed into the other equations to isolate variables.

## What stays invariant?

The solution vector $x$ (the intersection point of the equations in space) and the Row Space of the matrix.


[[Lecture1_The_Geometry_of_Linear_Equations]]
[[Lecture3_Multiplication_of_Matrices_and_Inversion]]