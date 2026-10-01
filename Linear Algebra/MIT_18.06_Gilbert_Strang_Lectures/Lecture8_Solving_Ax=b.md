
## 1. Big Picture

In the previous lectures, we focused on solving the homogeneous system, $Ax=0$. Now, we are shifting our focus to the general nonhomogeneous system:

$$Ax = b$$

To understand this conceptually, we must break down the components:

- $A$ is a matrix representing a specific linear transformation (or a system of equations).
    
- $x$ is the input vector (the unknowns we want to find).
    
- $b$ is the target output vector.
    

Solving $Ax=b$ is fundamentally about asking a reverse-engineering question: **"Can the columns of $A$ be combined to produce the exact target vector $b$, and if so, what combinations (inputs $x$) will do it?"**

This is directly connected to the **column space** of $A$. The column space represents every possible output the matrix $A$ can produce. Therefore, finding a solution to $Ax=b$ is exactly the same as asking if $b$ lives inside the column space of $A$.

**Why do we care so much about $Ax=0$ when solving $Ax=b$?**

The homogeneous system ($Ax=0$) is the foundation for the nonhomogeneous system because it reveals the "hidden" inputs that do absolutely nothing to the output. If you find one way to produce $b$, you can add any of these "do-nothing" inputs to it, and you will _still_ produce $b$. This is why $Ax=0$ dictates whether a solution is unique or if there are infinitely many.

## 2. Geometric Meaning of Ax = b

Let’s visualize $Ax=b$ geometrically. If we write $A$ as a collection of column vectors:

$$A = \begin{bmatrix} a_1 & a_2 \end{bmatrix}$$

Then multiplying $A$ by a vector $x$ is the same as taking a linear combination of its columns:

$$Ax = x_1a_1 + x_2a_2$$

Solving $Ax=b$ means we are searching for the specific scalar weights ($x_1$ and $x_2$) that stretch and add the vectors $a_1$ and $a_2$ together so that their tip perfectly lands on the vector $b$.

The **span** of $a_1$ and $a_2$ is all the points you can reach by taking linear combinations of them. This span is the **column space**.

Therefore, geometrically, the question of whether a solution exists is simply: **"Is $b$ inside the column space of $A$?"** If $b$ points to a location outside the plane (or line) formed by the columns of $A$, no combination of those columns will ever reach $b$, meaning no solution exists.

## 3. Existence of Solutions

Let's state the rule for the existence of solutions carefully:

The system

$$Ax = b$$

has a solution **if and only if**

$$b \in C(A)$$

**Algebraically:** This means $b$ can be written as a linear combination of the columns of $A$.

**Geometrically:** This means the vector $b$ lies flat inside the subspace spanned by the columns of $A$.

**Example of Existence ($b$ is in the column space):**

Let $A = \begin{bmatrix} 1 & 0 \\ 0 & 1 \\ 0 & 0 \end{bmatrix}$. The column space is the $xy$-plane in 3D space.

If $b = \begin{bmatrix} 3 \\ 4 \\ 0 \end{bmatrix}$, it lies on the $xy$-plane. A solution exists: $x_1 = 3$, $x_2 = 4$.

**Example of Non-Existence ($b$ is outside the column space):**

Using the same $A$, suppose $b = \begin{bmatrix} 3 \\ 4 \\ 5 \end{bmatrix}$. The target has a $z$-component of 5, but our columns only move along the $x$ and $y$ axes. No combination of columns can ever reach a height of 5. The vector $b$ is outside $C(A)$. No solution exists.

When we perform Gaussian elimination, it logically forces these truths out into the open. If a solution does not exist, elimination will eventually yield a contradictory equation, such as:

$$0 = 3$$

This physically means: "To reach $b$, you need to combine zero amounts of your columns to get a length of 3." Since $0 \neq 3$, it’s mathematically impossible. This contradiction is elimination's way of telling you that $b \notin C(A)$.

## 4. Solving Ax = b Using Elimination

Let’s walk through a concrete numerical example to see how this works mechanically:

$$\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} 5 \\ 11 \end{bmatrix}$$

To solve this, we create an **augmented matrix** $[A \mid b]$. We attach $b$ as an extra column because whatever algebraic operations we do to the left side of the equations, we must do to the right side to maintain equality.

$$\begin{bmatrix} 1 & 2 & \mid & 5 \\ 3 & 4 & \mid & 11 \end{bmatrix}$$

**Step 1: Eliminate the 3 in the bottom left.**

We subtract 3 times Row 1 from Row 2 ($R_2 \to R_2 - 3R_1$).

- New Row 2 left side: $[3, 4] - 3[1, 2] = [3-3, 4-6] = [0, -2]$
    
- New Row 2 right side: $11 - 3(5) = 11 - 15 = -4$
    

The new augmented matrix is:

$$\begin{bmatrix} 1 & 2 & \mid & 5 \\ 0 & -2 & \mid & -4 \end{bmatrix}$$

From the second row, we immediately see $-2x_2 = -4$, so $x_2 = 2$.

Substituting back into the first row: $x_1 + 2(2) = 5$, so $x_1 = 1$.

The solution is $x = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$.

**Why are these row operations allowed, and why don't they change the solution set?**

An equation is a true statement of equality. If you know that $A = B$ and $C = D$, then basic logic dictates that $A - 3C = B - 3D$. Adding or subtracting equations creates a new, valid truth. Furthermore, these operations are entirely _reversible_. Because no information is destroyed (we can always add $3R_1$ back to $R_2$ to recover the original matrix), the set of $x$ values that makes the system true remains exactly the same.

## 5. Particular Solution + Null Space

This is one of the most conceptually profound ideas in linear algebra:

If the system $Ax=b$ has at least one solution (let's call it a **particular solution**, $x_p$), then **every** solution can be written as:

$$x = x_p + x_n$$

where $x_n$ is a vector in the null space of $A$ (meaning $Ax_n = 0$).

Therefore:

$$\boxed{\text{All solutions of } Ax=b = \text{one particular solution } (x_p) + \text{all solutions of } Ax=0 \text{ } (x_n)}$$

**Why does this work algebraically?**

Because matrix multiplication distributes over addition. If we plug our proposed solution $(x_p + x_n)$ into $A$, we get:

$$A(x_p + x_n) = Ax_p + Ax_n$$

By definition, $Ax_p = b$, and since $x_n$ is in the null space, $Ax_n = 0$.

$$b + 0 = b$$

This proves that adding any null-space vector to a valid solution gives you another valid solution.

**The Reverse Direction (Why must ALL solutions look like this?):**

Suppose you and a friend both find a solution to $Ax=b$. Your solution is $x$ and your friend's is $y$.

This means $Ax = b$ and $Ay = b$.

If we subtract your friend's equation from yours, we get:

$$Ax - Ay = b - b$$

$$A(x - y) = 0$$

This means the difference between _any_ two valid solutions ($x - y$) must yield zero when multiplied by $A$. Therefore, the difference _must_ belong to the null space! You can rearrange this to $x = y + \text{something in the null space}$.

## 6. Why the Null Space Controls Uniqueness

Because of the equation $x = x_p + x_n$, the null space entirely controls how many solutions exist (assuming at least one exists).

1. **If $Ax=b$ has no solution:** Then $b$ isn't in the column space. Uniqueness doesn't matter; there are zero solutions.
    
2. **If it has a solution, and $N(A) = \{0\}$:** The only vector in the null space is the zero vector. Therefore, $x = x_p + 0 = x_p$. There are no alternative directions to travel in. The system has **exactly one solution**.
    
3. **If it has a solution, and $N(A)$ contains nonzero vectors:** You can pick $x_p$, and then add any multiple of the nonzero vectors in $N(A)$. Since you can multiply them by any real number, you have **infinitely many solutions**.
    

The size (nullity) of the null space determines how much freedom we have to move around without changing the output. If there are free variables, the null space has nonzero vectors, leading to infinite solutions.

## 7. Pivot Variables and Free Variables in Ax = b

From Lecture 7, we know that after elimination, columns with pivots correspond to **pivot variables**, and columns without pivots correspond to **free variables**.

- **Free variables** represent your degrees of freedom. You can set them to _any number you want_, and the equations will still hold.
    
- **Pivot variables** are fully constrained and dictated by your choices for the free variables.
    

**What changes when the system is $Ax=b$ rather than $Ax=0$?**

When solving $Ax=0$, if you set all free variables to 0, the pivot variables are forced to be 0, yielding the origin.

When solving $Ax=b$, if you set all free variables to 0, the pivot variables will be forced to equal whatever specific constants are left on the right-hand side of the augmented matrix. This specific set of values forms your **particular solution ($x_p$)**. The free variables still generate the null-space directions, but now we start traveling along those directions starting from $x_p$ rather than starting from the origin.

## 8. Special Solutions and the Complete Solution

To find the complete solution for a matrix with free variables, we follow a three-step process:

1. **Solve for one particular solution ($x_p$):** Set all free variables to $0$ and solve for the pivot variables.
    
2. **Solve for special solutions in $N(A)$:** Set $b = 0$. For each free variable, set it to $1$ and all other free variables to $0$, then solve for the pivot variables. These give your special null-space vectors $s_1, s_2, \dots$.
    
3. **Combine them:**
    

$$x = x_p + c_1s_1 + c_2s_2 + \cdots$$

**Geometric Meaning:**

- $x_p$ acts as an "anchor." It is a single specific point in space that satisfies the equation.
    
- $c_1s_1 + c_2s_2 + \dots$ defines a subspace (like a line or a flat plane) passing through the origin.
    
- By adding them together, we are taking a flat plane and translating (shifting) it away from the origin by the vector $x_p$. A subspace that has been shifted away from the origin is called an **affine subspace**.
    

## 9. Geometric Interpretation

Let's visualize how the number of equations and variables affects the geometry of the solution set.

### One equation in two variables

$$ax + by = c$$

This is a single constraint in 2D space. Usually, this defines a **line**. There is 1 free variable, meaning 1 degree of freedom (a 1-dimensional line).

### Two independent equations in three variables

This is two flat planes in 3D space intersecting each other. Two non-parallel planes intersect to form a **line**. Here, 3 variables minus 2 pivot variables leaves 1 free variable (a 1D line).

### Three independent equations in three variables

This is three planes intersecting. Two intersect in a line, and the third cuts across that line. They meet at exactly **one point**. 3 variables minus 3 pivot variables leaves 0 free variables (a 0-dimensional point).

**Key Takeaway:**

When the system is consistent, the dimension of the solution set (how "roomy" the answer is) is exactly the **nullity** (the number of free variables).

Do not confuse this with the **rank** (the number of pivot variables), which measures the dimension of the _column space_ (how "roomy" the possible outputs are).

## 10. Connection to Rank and Nullity

Recall the fundamental Rank-Nullity theorem for an $m \times n$ matrix (where $n$ is the number of columns/variables):

$$\operatorname{rank}(A) + \operatorname{nullity}(A) = n$$

$$(\text{pivot columns}) + (\text{free columns}) = (\text{total columns})$$

What does this tell us about $Ax=b$?

If a matrix $A$ has 5 columns ($n=5$) and rank 3 (3 independent equations/pivots):

$$\text{nullity} = 5 - 3 = 2$$

This immediately guarantees that _if_ a target vector $b$ is reachable (consistent), the solution set will have exactly 2 independent degrees of freedom. Geometrically, the solutions will form a 2-dimensional 2D plane living inside 5D space.

## 11. Special Cases

It is highly useful to categorize systems into three cases based on what we've learned:

### Case 1 — No solution

$$b \notin C(A)$$

**Logic:** The target $b$ cannot be built out of the columns of $A$. Elimination will produce a contradiction like $0 = c$ where $c \neq 0$.

### Case 2 — Exactly one solution

$$b \in C(A), \qquad N(A) = \{0\}$$

**Logic:** The target $b$ is reachable, meaning we have a particular solution $x_p$. The null space is empty (except for zero), meaning there are no free variables. There are no alternative directions to explore, so $x_p$ is unique. This happens when rank equals $n$ (full column rank).

### Case 3 — Infinitely many solutions

$$b \in C(A), \qquad N(A) \neq \{0\}$$

**Logic:** The target $b$ is reachable, giving us $x_p$. However, the matrix has free variables, meaning its null space contains nonzero vectors. We can add any multiple of these vectors to $x_p$ and still satisfy the equation. This happens when rank is strictly less than $n$.

## 12. Important Distinctions

|**Concept**|**Meaning**|
|---|---|
|**Column space**|The set of all possible outputs $Ax$ you can generate.|
|**Null space**|The set of all inputs $x$ that get mapped to the zero vector.|
|**Rank**|The dimension of the column space (number of pivot columns).|
|**Nullity**|The dimension of the null space (number of free columns).|
|**Pivot variable**|A variable whose value is strictly determined by equations.|
|**Free variable**|An independent parameter you can set to any value.|
|**Particular solution**|Just one specific input vector that successfully produces $b$.|
|**Homogeneous solution**|Any vector that produces the output $0$.|
|**General solution**|The complete set of answers, written as Particular + Homogeneous.|

## 13. Deep Connections

Connecting Lecture 8 back to what we already know is crucial for true understanding:

- **Lecture 7:** In Lecture 7, we learned how to find special solutions to $Ax=0$. Today, we learned that these same special solutions act as the mathematical "directions" that sweep out all possible solutions for $Ax=b$.
    
- **Column space:** The existence of a solution isn't about arbitrary algebra; it's a structural question. Does $b$ live inside the subspace generated by $A$'s columns?
    
- **Null space:** The uniqueness of a solution depends entirely on whether $A$ collapses inputs. If $A$ maps multiple inputs to $0$, it will map multiple inputs to $b$.
    
- **Rank-nullity:** Every column must either provide new directional reach for the output (rank/pivot) OR it must be redundant, creating flexibility in the input (nullity/free variable).
    
- **Gaussian elimination:** Elimination is a master key. In one sweep, it reveals if $b$ is reachable (no contradictions), identifies pivots (rank), exposes free variables (nullity), and provides the exact numbers to write down the solutions.
    

## 14. A Complete Worked Example

Let's solve $Ax=b$ where there are infinitely many solutions.

$$A = \begin{bmatrix} 1 & 2 & 2 \\ 2 & 4 & 5 \end{bmatrix}, \qquad b = \begin{bmatrix} 1 \\ 4 \end{bmatrix}$$

**1. Write the augmented matrix:**

$$\begin{bmatrix} 1 & 2 & 2 & \mid & 1 \\ 2 & 4 & 5 & \mid & 4 \end{bmatrix}$$

**2. Row reduce:**

Subtract 2 times Row 1 from Row 2 ($R_2 \to R_2 - 2R_1$):

$$\begin{bmatrix} 1 & 2 & 2 & \mid & 1 \\ 0 & 0 & 1 & \mid & 2 \end{bmatrix}$$

**3. Identify pivot/free variables:**

- Pivot columns: Column 1 and Column 3.
    
- Pivot variables: $x_1$ and $x_3$.
    
- Free column: Column 2.
    
- Free variable: $x_2$.
    
    Since rank is 2 and there are 3 columns, nullity is 1.
    

**4. Find one particular solution ($x_p$):**

Set the free variable $x_2 = 0$.

From Row 2: $1x_3 = 2 \implies x_3 = 2$.

From Row 1: $1x_1 + 2(0) + 2(2) = 1 \implies x_1 + 4 = 1 \implies x_1 = -3$.

$$x_p = \begin{bmatrix} -3 \\ 0 \\ 2 \end{bmatrix}$$

**5. Find the homogeneous solution ($x_n$):**

Set the augmented side to 0 to solve $Ax=0$.

$$\begin{bmatrix} 1 & 2 & 2 & \mid & 0 \\ 0 & 0 & 1 & \mid & 0 \end{bmatrix}$$

Set the free variable $x_2 = 1$.

From Row 2: $x_3 = 0$.

From Row 1: $1x_1 + 2(1) + 2(0) = 0 \implies x_1 = -2$.

So the special solution is $s_1 = \begin{bmatrix} -2 \\ 1 \\ 0 \end{bmatrix}$.

The null space is $x_n = c \begin{bmatrix} -2 \\ 1 \\ 0 \end{bmatrix}$.

**6. Write the complete solution:**

$$x = \begin{bmatrix} -3 \\ 0 \\ 2 \end{bmatrix} + c \begin{bmatrix} -2 \\ 1 \\ 0 \end{bmatrix}$$

**7. Geometrically & Structurally:**

The solutions form a 1D line in 3D space. The line does not pass through the origin; it is shifted to pass through the point $(-3, 0, 2)$.

**A short example of No Solution:**

Suppose we row-reduce a different matrix and get:

$$\begin{bmatrix} 1 & 1 & \mid & 1 \\ 0 & 0 & \mid & 3 \end{bmatrix}$$

The second equation reads $0x_1 + 0x_2 = 3$, meaning $0 = 3$. No $x$ values can ever make this true. Therefore, $b \notin C(A)$, and there is no solution.

## The Crucial Relationship Between m, n, and r

To deeply understand the solutions to

$$Ax=b$$

, we must look at the shape of the matrix $A$ and how much "true information" it contains.

Let $A$ be an $m \times n$ matrix.

- $m$ = number of rows (the number of equations)
    
- $n$ = number of columns (the number of unknowns)
    
- $r$ = rank (the number of pivots discovered after elimination)
    

### The Fundamental Constraints

Every pivot belongs to a unique row and a unique column. Because you cannot have more than one pivot per row or per column, the rank $r$ is bounded by both dimensions:

$$r \leq m \qquad \text{and} \qquad r \leq n$$

This means the rank can never exceed the smaller of the two dimensions:

$$r \leq \min(m, n)$$

Because $n$ represents the total number of unknowns (columns), and $r$ represents the number of pivot variables, the remaining variables are free. Therefore:

$$\boxed{\text{number of free variables} = n - r}$$

The way $r$ relates to $n$ controls **uniqueness** (how many solutions exist). The way $r$ relates to $m$ controls **existence** (whether a solution exists at all). Let's break this down.

### 1. The Column Constraints: $r$ vs $n$ (Uniqueness)

This relationship tells us what happens inside the null space.

**Case: $r = n$ (Full Column Rank)**

- **What it means:** Every column has a pivot.
    
- **Free variables:** $n - n = 0$. There are no free variables.
    
- **Why it matters:** Because there are no free variables, the null space only contains the zero vector. You have no independent "dials" to turn.
    
- **Result:** If a solution to
    

$$Ax=b$$

exists, it is **unique**. (0 or 1 solution).

**Case: $r < n$**

- **What it means:** Some columns lack pivots.
    
- **Free variables:** $n - r > 0$.
    
- **Why it matters:** The missing pivots mean there are redundant columns in the matrix. This creates free variables, which generate non-zero vectors in the null space.
    
- **Result:** If a solution to
    

$$Ax=b$$

exists, there are **infinitely many solutions** because you can add any multiple of the null-space vectors to your particular solution.

### 2. The Row Constraints: $r$ vs $m$ (Existence)

This relationship tells us what happens to the column space.

**Case: $r = m$ (Full Row Rank)**

- **What it means:** Every row has a pivot.
    
- **Why it matters:** When every row has a pivot, elimination will _never_ produce a row of all zeros in the coefficient matrix. Therefore, it is impossible to ever encounter a contradiction like $0 = c$ (where $c \neq 0$).
    
- **Result:** Every target vector $b$ in $\mathbb{R}^m$ is reachable. The column space spans the entirety of $\mathbb{R}^m$. The system
    

$$Ax=b$$

will always have **at least one solution** for _any_ $b$.

**Case: $r < m$**

- **What it means:** There are fewer pivots than rows.
    
- **Why it matters:** Elimination will inevitably wipe out some rows, leaving rows of all zeros on the left side. These represent dependent, redundant equations. If the corresponding right side $b$ does not also become zero, you get a contradiction ($0 = c$).
    
- **Result:** The column space is a smaller "flat" shape (like a plane) trapped inside $\mathbb{R}^m$. If $b$ does not perfectly land on this shape, there is **no solution**.
    

### Matrix Shapes (Dimensions in Action)

Let's look at how the physical shape of the matrix forces these outcomes.

#### Short and Wide: $m < n$ (More unknowns than equations)

Because $r \leq m$, and $m < n$, the rank $r$ is strictly forced to be less than $n$ ($r < n$).

- **The "Why":** You do not have enough constraints (equations) to pin down all your variables.
    
- **The Consequence:** There must be at least $n - m$ free variables. The null space is guaranteed to contain non-zero vectors. The homogeneous system
    

$$Ax=0$$

will _always_ have non-trivial solutions. If $Ax=b$ is consistent, it will have infinite solutions.

- **RREF Example ($r=m=2, n=3$):**
    

$$\begin{bmatrix} 1 & 0 & 4 \\ 0 & 1 & 5 \end{bmatrix}$$

```
(Full row rank: always solvable, infinite solutions).
```

#### Tall and Thin: $m > n$ (More equations than unknowns)

Because $r \leq n$, and $n < m$, the rank $r$ is strictly forced to be less than $m$ ($r < m$).

- **The "Why":** You have too many constraints. The columns of $A$ do not have enough dimensions to reach everywhere in $\mathbb{R}^m$.
    
- **The Consequence:** You are guaranteed to get zero-rows during elimination. Most random vectors $b$ will not fall into the column space. There will be no solution unless $b$ is carefully chosen.
    
- **RREF Example ($r=n=2, m=3$):**
    

$$\begin{bmatrix} 1 & 0 \\ 0 & 1 \\ 0 & 0 \end{bmatrix}$$

```
(Full column rank: 0 or 1 solution, depending on if the target \(b\) has a zero in the 3rd row).
```

#### Square: $m = n$

If the matrix is square AND has full rank ($r = m = n$), this is the gold standard.

- **The "Why":** It has full row rank ($r=m$), so a solution always exists for every $b$. It has full column rank ($r=n$), so there are no free variables.
    
- **The Consequence:** Every $b$ is reachable, and there is exactly one unique set of inputs $x$ that reaches it. The matrix is perfectly invertible.
    
- **RREF Example ($r=m=n=2$):**
    

$$\begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}$$

### Summary Table: Dimensions and Solutions

This table condenses the relationship between dimensions, pivots, and solutions.

| **Dimensions**                     | **Pivots (Rank)** | **Free Variables** | **Possible Solution Behavior for Ax=b**                          |
| ---------------------------------- | ----------------- | ------------------ | ---------------------------------------------------------------- |
| **Square** ($r = m = n$)           | $r = n$           | $0$                | Exactly **1 unique solution** for every $b$.                     |
| **Short/Wide** ($r = m < n$)       | $r < n$           | $n - r$            | **Infinitely many solutions** for every $b$.                     |
| **Tall/Thin** ($r = n < m$)        | $r = n$           | $0$                | **0 or 1 solution** (depends on if $b \in C(A)$).                |
| **Not Full Rank** ($r < m, r < n$) | $r < n$           | $n - r$            | **0 or infinitely many solutions** (depends on if $b \in C(A)$). |

## 15. Common Misunderstandings

- **“If I change the equations during elimination, am I changing the original problem?”**
    
    No. As long as you only add or subtract multiples of complete equations, you are creating new, equally true statements. Because the steps are reversible, the set of $x$ values that satisfy the equations remains exactly the same.
    
- **“Why does a contradiction $0=3$ mean no solution?”**
    
    It means the target vector $b$ has components that point in directions the matrix columns simply cannot reach. The math is logically preventing you from achieving an impossible output.
    
- **“Why does one particular solution not represent all solutions?”**
    
    Because if the matrix has redundant columns (a non-trivial null space), there are hidden "do-nothing" combinations of inputs. You can add those hidden combinations to your particular solution without altering the output.
    
- **“Why can I add a null-space vector to a particular solution?”**
    
    Because of matrix algebra: $A(x_p + x_n) = Ax_p + Ax_n = b + 0 = b$. The null-space vector acts like adding "+ 0" to the target.
    
- **“Why do free variables create infinitely many solutions?”**
    
    A free variable means a column in your matrix didn't provide a new, independent direction in the output space. Therefore, you have an extra parameter you can scale by any real number $c$. Since there are infinitely many real numbers, there are infinitely many solutions.
    
- **“Why does $b \in C(A)$ determine existence?”**
    
    Because $Ax$ literally means "a linear combination of the columns of $A$". If $b$ cannot be formed by combining those columns, it does not exist in the column space, and no $x$ can produce it.
    
- **“Why does nullity determine the number of degrees of freedom?”**
    
    Nullity is the number of free variables. Each free variable gives you one independent mathematical "dial" you can turn without breaking the equation $Ax=b$.
    
- **“Why is the solution set of $Ax=b$ usually not a subspace when $b \neq 0$?”**
    
    A subspace must always contain the zero vector. If $b \neq 0$, then $x=0$ cannot be a solution (since $A(0) = 0 \neq b$). The solution set is an _affine_ subspace (a flat shape shifted away from the origin).
    

## 16. Mental Model

Think of the matrix $A$ as a mechanical machine.

- **$x$** is the raw input you feed into the machine.
    
- **$Ax$** is the final product (output) the machine produces.
    
- **$C(A)$** (Column space) is the complete catalog of all possible products this machine is physically capable of manufacturing.
    
- **$N(A)$** (Null space) represents adjustments or tweaks to the input that the machine simply ignores (they result in 0 change to the output).
    
- **$Ax=b$** is the question: "Can this machine manufacture product $b$? And if so, what inputs do I need to type in?"
    
- **One particular solution ($x_p$)** is just finding _one_ specific recipe of inputs that successfully manufactures $b$.
    
- **Null-space vectors ($x_n$)** are the inputs that the machine ignores.
    
- **Therefore, all solutions ($x = x_p + x_n$)** simply state: To get all recipes for $b$, take your working recipe ($x_p$) and combine it with any tweaks the machine ignores ($x_n$).
    

## 17. Final Takeaways

- Solving $Ax=b$ means finding the weights $x$ to combine the columns of $A$ to reach $b$.
    
- A solution exists if and only if $b$ is in the column space of $A$.
    
- If Gaussian elimination yields an equation like $0=c$ (where $c \neq 0$), there is no solution.
    
- If a system is consistent (a solution exists), every possible solution takes the form $x = x_p + x_n$.
    
- $x_p$ is a single particular solution satisfying $Ax=b$.
    
- $x_n$ represents the entire null space, satisfying $Ax=0$.
    
- The number of pivot variables defines the rank (dimension of the column space).
    
- The number of free variables defines the nullity (dimension of the null space).
    
- Free variables provide the degrees of freedom; you can assign them any value, resulting in infinitely many solutions (if nullity > 0).
    
- If the null space contains only the zero vector (nullity = 0), a consistent system will have exactly one unique solution.
    
- The complete solution set geometrically represents an affine subspace (a subspace shifted away from the origin by the vector $x_p$).

[[Lecture7_Solving_Ax=0_Pivot_Variables_&_Special_Solutions]]