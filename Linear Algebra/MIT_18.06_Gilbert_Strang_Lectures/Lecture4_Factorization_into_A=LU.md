
# Lecture Overview

In this lecture, Gilbert Strang introduces one of the foundational pillars of applied linear algebra: the factorization of a matrix $A$ into the product of a lower triangular matrix $L$ and an upper triangular matrix $U$. While previous lectures treat Gaussian elimination as a dynamic, destructive process that alters a matrix step-by-step to solve $Ax = b$, this lecture shifts the paradigm entirely. Here, elimination is reframed not as a sequence of temporary actions, but as a permanent geometric and algebraic structure. Factoring a matrix into $A = LU$ is the ultimate mathematical expression of Gaussian elimination, acting as the foundational engine for modern scientific computing, engineering simulations, and large-scale data analysis.

# Big Picture

### Why isn't Gaussian elimination itself enough?

Imagine running an online simulation or a weather forecasting model where you must solve the system $Ax = b$ millions of times every day. The matrix $A$ (representing the physical laws of the atmosphere) remains exactly the same, but the vector $b$ (the shifting daily temperature and pressure measurements) changes constantly.

If you rely solely on standard Gaussian elimination, you are forced to append $b$ to $A$ as an augmented matrix $[A \mid b]$ and rerun the entire elimination process from scratch for every single new measurement. This is wildly inefficient. Gaussian elimination modifies the right-hand side $b$ simultaneously with $A$, meaning the work done on $A$ is bound to that specific vector $b$ and instantly lost when the next data point arrives.

### Why store elimination as matrices?

Mathematicians realized that the steps taken to eliminate coefficients in $A$ depend _only_ on the entries of $A$ itself—not on $b$. By recording the elimination process as a sequence of matrices, we isolate the operations performed on the grid of coefficients. Storing these steps structurally means we only have to compute the "skeleton" of the elimination once.

### What problem does LU factorization solve?

$A = LU$ completely decouples the matrix $A$ from the vector $b$. It processes the matrix $A$ ahead of time, breaking it down into two highly specialized components:

- $U$ (the final state of the system after elimination).
    
- $L$ (the record of how we got there).
    

When a new $b$ arrives, we do not perform elimination again. Instead, we pass $b$ through $L$ and then $U$ using fast, simple substitutions.

```
[ Raw Matrix A ] ---> Factorization ---> [ Lower L ] and [ Upper U ]
                                             |              |
                                        Processes      Finalizes 
                                        the Input      the System
```

### Why is factoring a matrix useful?

In arithmetic, factoring a number like $12$ into $3 \times 4$ or $2^2 \times 3$ reveals its inner properties (its divisors and prime structure). In linear algebra, factoring a matrix $A$ into $LU$ breaks a complex, highly interconnected linear transformation into two distinct, sequential phases that are vastly easier to compute and interpret.

# Learning Objectives

After studying this lecture, you should understand:

- **The Structural View of Elimination:** How to see Gaussian elimination not as a destructive algorithm, but as the clean algebraic factorization $A = LU$.
    
- **The Anatomy of $L$ and $U$:** The distinct informational roles played by the lower triangular matrix $L$ (history/multipliers) and the upper triangular matrix $U$ (final pivots/systems).
    
- **The Magic of Non-Interference:** Why the multipliers appear directly inside $L$ without altering one another, provided no rows are exchanged.
    
- **Computational Decoupling:** How $A = LU$ allows us to solve $Ax = b$ for varying values of $b$ with minimal computational effort.
    

# Core Idea

The central insight of this lecture is that **elimination is matrix multiplication in disguise**. Every row operation we perform during Gaussian elimination can be written as an elementary matrix $E_{ij}$. Therefore, the entire process of turning a matrix $A$ into its upper triangular form $U$ can be summarized as:

$$(E_{32} E_{31} E_{21})A = U$$

If we want to isolate $A$, we simply multiply both sides by the inverses of these elimination matrices in the reverse order:

$$A = (E_{21}^{-1} E_{31}^{-1} E_{32}^{-1})U$$

We define the product of these inverse matrices as $L$:

$$L = E_{21}^{-1} E_{31}^{-1} E_{32}^{-1}$$

### What information do they store?

- **$U$ (Upper Triangular):** Contains the _pivots_ along its diagonal and the remaining modified coefficients above them. It represents the destination of the elimination journey—a system where variables are cleanly uncoupled from bottom to top.
    
- **$L$ (Lower Triangular):** Contains $1$s on its diagonal and the _elimination multipliers_ in the exact positions below the diagonal where zeroes were created. It acts as a perfect diary of the journey, preserving the exact history of the row operations.
    

# Intuition Before Mathematics

### The "Undo" Machine Analogy

Think of $A = LU$ as breaking down a complex task into a process of recording and execution.

Imagine you are an artist creating a digital painting ($A$). To create this painting, you start with a clean canvas ($U$) and apply a series of brush strokes, adjustments, and layers ($L$).

- $U$ represents the clean, structural guidelines of the drawing—the raw, reduced essence.
    
- $L$ is the exact history of adjustments, keeping track of how you altered the space step-by-step to get to the final image.
    

Alternatively, consider a complex manufacturing assembly line. Matrix $A$ takes raw materials and scrambles them into a finished product. If you want to reverse-engineer this or feed new raw materials through it efficiently, you can split the line into two specialized machines:

1. **Machine $L$:** A system that prepares the inputs, organizing them according to how they will depend on one another.
    
2. **Machine $U$:** A system that structurally finishes the job, scaling the components by their core weights (the pivots).
    

> [!NOTE]
> 
> **The Key Insight:** Multiplying $L \times U$ looks like a complex mixture of rows and columns, but conceptually, it is a highly ordered reconstruction. $L$ holds the keys to the past (how rows were subtracted), while $U$ holds the keys to the future (the solved pivots).

# Gaussian Elimination Revisited

Let's review classical Gaussian elimination on a $3 \times 3$ matrix to see how $U$ is born. We begin with a matrix $A$:

$$A = \begin{bmatrix} 2 & 1 & 1 \\ 4 & -6 & 0 \\ -2 & 7 & 2 \end{bmatrix}$$

1. Our first goal is to create zeroes in column 1 below the first pivot, which is **$2$**.
    
2. To eliminate the $4$ in row 2, position $(2,1)$, we subtract $2 \times (\text{Row } 1)$ from $\text{Row } 2$. The multiplier is **$\ell_{21} = 2$**.
    
3. To eliminate the $-2$ in row 3, position $(3,1)$, we subtract $-1 \times (\text{Row } 1)$ from $\text{Row } 3$. The multiplier is **$\ell_{31} = -1$**.
    

After these two steps, our intermediate matrix looks like this:

$$A' = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 8 & 3 \end{bmatrix}$$

4. Our next pivot is **$-8$** in position $(2,2)$. We must eliminate the $8$ below it in position $(3,2)$.
    
5. To eliminate it, we subtract $-1 \times (\text{Row } 2)$ from $\text{Row } 3$. The multiplier is **$\ell_{32} = -1$**.
    

This gives us our final, upper triangular matrix **$U$**:

$$U = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix}$$

Notice the values along the diagonal of $U$: $2, -8, 1$. These are our **pivots**. Elimination succeeded because none of these pivots were zero.

# Matrix L

### What is it?

Matrix $L$ is a **lower triangular matrix** with $1$s stretching along its main diagonal.

### Why does it exist and what information does it contain?

It exists to store the _multipliers_ used during elimination. Using the multipliers from our previous section ($\ell_{21} = 2$, $\ell_{31} = -1$, and $\ell_{32} = -1$), we construct $L$ directly:

$$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix}$$

### Why are the diagonal entries equal to 1?

The diagonal entries of $L$ are $1$ because $L$ records how we reconstruct $A$ from $U$. To rebuild the original Row $i$ of $A$, we must start with exactly $1 \times (\text{Row } i \text{ of } U)$ and then add back combinations of the previous rows.

### Why is it lower triangular?

Because elimination moves forward from top to bottom. When clearing out entries below a pivot, we only ever subtract a higher row from a lower row. Therefore, Row $i$ only ever receives contributions from rows _above_ it (indices less than $i$).

> [!IMPORTANT]
> 
> **The Miracle of $L$:** When we look at the product of elimination matrices, their forward operations interfere with each other, creating messy cross-terms. But when we look at their _inverses_ packaged into $L$, the multipliers drop into their correct positions with absolutely zero interference. This holds true as long as there are no row exchanges.

### Common Misconceptions

- **"The entries of $L$ are just the values we zeroed out."** No. The entries of $L$ are the _multipliers_ (the ratio of the entry to be eliminated to the pivot).
    
- **"The signs in $L$ match the signs used during elimination."** No. During elimination, we _subtract_ rows (e.g., $\text{Row } 2 - \ell_{21}\text{Row } 1$). To invert this and rebuild $A$, we must _add_ rows back. Thus, the signs of the multipliers inside $L$ are the opposite of the signs used during subtraction.
    

# Matrix U

### What is it?

Matrix $U$ is an **upper triangular matrix**. It is the direct consequence of completing Gaussian elimination successfully.

$$U = \begin{bmatrix} d_{1} & * & * \\ 0 & d_{2} & * \\ 0 & 0 & d_{3} \end{bmatrix}$$

### Why does elimination always produce it?

The core strategy of elimination is to systematically remove variables from lower equations. By eliminating the first variable from all equations below the first, then the second variable from all equations below the second, we naturally carve away the bottom-left half of the matrix, leaving an upper triangular shape.

### What information remains after elimination?

$U$ holds the _pivots_ on its diagonal. These pivots indicate whether the matrix is invertible (none of them can be zero). The rows of $U$ represent the clean, decoupled directions of the original vector space spanned by $A$.

# Why A = LU Works

Let's carefully derive why the product of individual row operations collapses into this clean factorization.

Let $E_{21}$ be the elementary matrix that subtracts $2 \times (\text{Row } 1)$ from $\text{Row } 2$:

$$E_{21} = \begin{bmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}$$

Let $E_{31}$ be the elementary matrix that adds $1 \times (\text{Row } 1)$ to $\text{Row } 3$ (subtracts $-1$):

$$E_{31} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 0 & 1 \end{bmatrix}$$

Let $E_{32}$ be the elementary matrix that adds $1 \times (\text{Row } 2)$ to $\text{Row } 3$ (subtracts $-1$):

$$E_{32} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 1 & 1 \end{bmatrix}$$

The complete process of elimination is written as:

$$E_{32} (E_{31} (E_{21} A)) = U \implies (E_{32} E_{31} E_{21}) A = U$$

Let's multiply out the product $(E_{32} E_{31} E_{21})$ to see what happens when we look at the forward operations together:

$$E_{32} E_{31} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 1 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix}$$

Now multiply by $E_{21}$:

$$E_{32} E_{31} E_{21} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 1 & 1 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ -2 & 1 & 0 \\ -1 & 1 & 1 \end{bmatrix}$$

Notice the value in position $(3,1)$: it is **$-1$**. But our original multiplier to clear position $(3,1)$ was $-1$, meaning we subtracted $-1$, so we expected a $+1$ here. Why is there a $-1$?

Because row operations done early in the process get multiplied and shifted by row operations done later. This represents messy mathematical cross-talk.

Now watch what happens when we look at the **inverses** to build $L$:

$$E_{21}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix}, \quad E_{31}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & 0 & 1 \end{bmatrix}, \quad E_{32}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix}$$

We calculate $L = E_{21}^{-1} E_{31}^{-1} E_{32}^{-1}$ by multiplying from right to left:

$$E_{31}^{-1} E_{32}^{-1} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & -1 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix}$$

Now finish the product by multiplying by $E_{21}^{-1}$:

$$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix}$$

The multipliers ($2, -1, -1$) land exactly in their intended slots without changing at all.

When moving backward from $U$ to $A$, we add back higher rows to lower rows. Since higher rows are already finalized, their additions do not interfere with subsequent steps.

# Step-by-Step Numerical Example

Let's verify that our computed matrices reconstruct our original matrix $A$.

Given:

$$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ -1 & -1 & 1 \end{bmatrix}, \quad U = \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix}$$

Let's compute the product $LU$ row by row:

### Row 1 Calculation

$$\text{Row } 1 \text{ of } L \times U = \begin{bmatrix} 1 & 0 & 0 \end{bmatrix} \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} 2 & 1 & 1 \end{bmatrix}$$

### Row 2 Calculation

$$\text{Row } 2 \text{ of } L \times U = \begin{bmatrix} 2 & 1 & 0 \end{bmatrix} \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix} = 2\begin{bmatrix} 2 & 1 & 1 \end{bmatrix} + 1\begin{bmatrix} 0 & -8 & -2 \end{bmatrix} = \begin{bmatrix} 4 & -6 & 0 \end{bmatrix}$$

### Row 3 Calculation

$$\text{Row } 3 \text{ of } L \times U = \begin{bmatrix} -1 & -1 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & 1 \\ 0 & -8 & -2 \\ 0 & 0 & 1 \end{bmatrix}$$

$$= -1\begin{bmatrix} 2 & 1 & 1 \end{bmatrix} - 1\begin{bmatrix} 0 & -8 & -2 \end{bmatrix} + 1\begin{bmatrix} 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} -2 & 7 & 2 \end{bmatrix}$$

Putting the calculated rows together:

$$LU = \begin{bmatrix} 2 & 1 & 1 \\ 4 & -6 & 0 \\ -2 & 7 & 2 \end{bmatrix} = A$$

The factorization matches the original matrix perfectly.

# Matrix Interpretation as Machines

We can think of linear transformations as dynamic pipelines. When a vector $x$ enters the system, we can process it in two different ways:

### Option 1: The Raw Transformation ($A$)

```
  x ---> [ Matrix A ] ---> b
```

The vector $x$ enters a single complex machine that shears, stretches, and rotates space all at once, outputting $b$.

### Option 2: The Two-Stage Pipeline ($LU$)

```
  x ---> [ Machine U ] ---> y ---> [ Machine L ] ---> b
```

1. **First Stage (Machine $U$):** The vector $x$ enters $U$. Because $U$ is upper triangular, it processes variables from the bottom up, scaling elements directly by their final independent pivots. It outputs an intermediate vector $y$.
    
2. **Second Stage (Machine $L$):** The vector $y$ enters $L$. Because $L$ is lower triangular, it mixes the vectors from top to bottom, reapplying the historical dependencies and scaling factors that match the physical structure of the original system. The final output is $b$.
    

# Why Factorization Is Powerful

To solve $Ax = b$, we substitute $A = LU$ to get:

$$LUx = b$$

We break this down into two steps by defining an intermediate vector $y = Ux$:

|**Step**|**System**|**Operation Type**|**Computational Cost**|
|---|---|---|---|
|**1**|$Ly = b$|Forward Substitution|Very Fast ($O(n^2)$)|
|**2**|$Ux = x$|Back Substitution|Very Fast ($O(n^2)$)|

### The Real-World Advantage

Computing the $LU$ factorization takes approximately $\frac{1}{3}n^3$ floating-point operations. However, solving the triangular systems via forward and backward substitution costs only $n^2$ operations.

For a matrix of size $n = 1000$:

- **Full Elimination:** Requires roughly $\approx 333,000,000$ operations for _every single choice of $b$_.
    
- **Using LU Factorization:** We pay the $333,000,000$ operation cost **once**. Afterward, every time a new vector $b$ arrives, solving for $x$ requires only $\approx 1,000,000$ operations. This represents a $300\times$ speedup per system.
    

# Geometric Interpretation

Geometrically, a matrix transformation alters the standard basis vectors of a space. The $LU$ factorization shows that any invertible linear transformation (that does not require row swaps) can be broken down into two distinct geometric phases:

```
[ Standard Grid ] 
       |
       |  (Transform by U)
       v
[ Sheared & Stretched Upper Grid ] (Leaves upper coordinates intact)
       |
       |  (Transform by L)
       v
[ Final Target Space A ]           (Re-aligns the lower dependencies)
```

1. **The Action of $U$:** Because $U$ is upper triangular, it maps the basis vectors such that the first coordinate direction stays unmixed with the others, while cascading dependencies upward. It establishes the core directional boundaries of the transformation.
    
2. **The Action of $L$:** $L$ acts as a lower directional shear. It takes the skewed space created by $U$ and slides the coordinates vertically and horizontally back into their final positions, using the historical weights recorded during elimination.
    

# Connection to Previous Lectures

```
Lecture 1: Linear Combinations ---> Lecture 2: Elimination Algorithm
                                                    |
                                                    v
Lecture 4: A = LU Factorization <--- Lecture 3: Matrix Multiplication Matrix Inverses
```

- **Lecture 1 & 2:** Introduced the mechanics of Gaussian elimination to solve individual linear systems.
    
- **Lecture 3:** Reframed operations as matrix multiplications ($E_{ij}$).
    
- **Lecture 4:** Combines these insights. Instead of tracking changing systems, we collect individual elimination steps, invert them, and group them together to discover that the matrix $A$ inherently contains its own structural solution: $A = LU$.
    

# Important Properties

- **Existence and Uniqueness:** Every invertible matrix $A$ can be factored uniquely into $A = LU$, provided that no zero values appear in the pivot positions during elimination.
    
- **The Split Factorization ($LDU$):** If we prefer our upper triangular matrix to have $1$s on the diagonal just like $L$, we can factor out the pivots explicitly into a diagonal matrix $D$:
    
    $$A = LDU = \begin{bmatrix} 1 & 0 & 0 \\ \ell_{21} & 1 & 0 \\ \ell_{31} & \ell_{32} & 1 \end{bmatrix} \begin{bmatrix} d_1 & 0 & 0 \\ 0 & d_2 & 0 \\ 0 & 0 & d_3 \end{bmatrix} \begin{bmatrix} 1 & u_{12} & u_{13} \\ 0 & 1 & u_{23} \\ 0 & 0 & 1 \end{bmatrix}$$
    
- **Row Exchanges:** If a zero appears in a pivot position, elimination stalls. We must swap rows to bring a non-zero entry into the pivot slot. This requires **Permutation Matrices ($P$)**, leading to the expanded general factorization:
    
    $$PA = LU$$
    

# Frequently Asked Questions

### Why does $L$ have $1$s on the diagonal?

Because $L$ tracks how we rebuild $A$ out of $U$. It states: "To recreate Row $i$ of the original matrix, take exactly $1 \times$ the newly cleared Row $i$ from $U$, and add back the required multiples of the higher rows."

### Why is $U$ upper triangular?

Because elimination systematically clears out all coefficients below the main diagonal. This isolates variables so the bottom equation contains only the last variable, the second-to-last equation contains only the last two variables, and so on.

### Why multiply $L$ and $U$ instead of keeping elimination steps separate?

Keeping individual elimination matrices ($E_{21}, E_{32}$) requires storing a long list of separate matrices and performing multiple sequential matrix multiplications. Multiplying their inverses together into $L$ condenses the entire history into a single lower triangular layout.

# Common Mistakes

- **Incorrectly Copying Sign Multipliers:**
    
    If you subtract $3 \times (\text{Row } 1)$ from $\text{Row } 2$ during elimination, the entry in $L$ at position $(2,1)$ is $+3$, **not** $-3$. Remember that $L$ reverses the elimination process to rebuild $A$.
    
- **Swapping the Multiplication Order:**
    
    Matrix multiplication is not commutative. $A = LU$, but $A \neq UL$. $U$ must always act on the vector space first, followed by $L$.
    
- **Ignoring Row Exchanges:**
    
    You cannot write a clean, unpermuted $A = LU$ if your elimination process required a row swap. If rows were exchanged, you must track those swaps using a permutation matrix $P$.
    

# Historical Context

While the basic elimination technique was documented in ancient Chinese texts and later popularized by Carl Friedrich Gauss, the structural matrix factorization view ($A = LU$) was developed in the 1940s by computer scientist **Alan Turing** and mathematician **Tadeusz Banachiewicz**.

As early digital computers emerged, researchers realized that executing Gauss's algorithm directly on augmented matrices was far too slow for repeating engineering calculations. Turing's insight to factor the matrix into triangular forms laid the foundation for modern numerical software packages like LAPACK and MATLAB.

# Cheat Sheet

### Definitions

- **$A = LU$:** The algebraic representation of Gaussian elimination without row exchanges.
    
- **$L$ (Lower Triangular):** Stores the history of multipliers with $1$s on the main diagonal.
    
- **$U$ (Upper Triangular):** Stores the final pivots and modified rows resulting from elimination.
    

### Key Formula

$$A = LDU$$

### Computational Cost Comparison

- **Computing $LU$ Factorization:** $\approx \frac{1}{3}n^3$ operations (done once).
    
- **Solving via $L$ and $U$:** $\approx n^2$ operations (done for each new vector $b$).
    

# Intuition Notebook

## What mathematical object was introduced?

The matrix factorization $A = LU$ (and its symmetric variant $A = LDU$).

## Why did mathematicians invent it?

To save the work done during Gaussian elimination, turning a dynamic step-by-step algorithm into a permanent, reusable matrix structure.

## What problem does it solve?

It prevents the need to re-run full elimination from scratch when solving $Ax = b$ multiple times with the same matrix $A$ but different vectors $b$.

## What information does $L$ store?

The history of row operations, constructed using the exact multipliers used to clear entries below the pivots.

## What information does $U$ store?

The final state of the simplified linear system, including the pivots along its main diagonal.

## How does this connect to matrix multiplication?

It shows that the product of the inverses of individual elimination matrices fields a clean lower triangular layout where multipliers land precisely in their slots without interfering with one another.

[[Lecture3_Multiplication_of_Matrices_and_Inversion]]
[[Lecture5_Trasposes_Permutations_Vector_spaces]]