Linear Equations tags:

- math
    
- linear-algebra
    
- foundations
    
- machine-learning
    

# MIT 18.06 · Lecture 1: The Geometry of Linear Equations

## 📌 Main Ideas of the Lecture

- **The Fundamental Problem:** The core goal of linear algebra is to solve a system of simultaneous linear equations containing $n$ equations with $n$ unknowns.
    
- **The Dual Perspective:** A system of linear equations is not just a block of algebra; it has two distinct, powerful geometric interpretations: the **Row Picture** and the **Column Picture**.
    
- **The Shift in Framework:** While the Row Picture focuses on tracking where geometric boundaries (lines and planes) intersect, the Column Picture shifts the focus to tracking how vectors combine. This introduces the most important operation in all of linear algebra: the **linear combination**.
    
- **Defining Multiplication:** Matrix-vector multiplication $Ax$ should not be thought of purely as a computational row-by-column dot product. Conceptually, $Ax$ is fundamentally a **linear combination of the columns of matrix** $A$.
    

## 📖 Definitions

- **Matrix (**$A$**):** A rectangular array of numbers. In a system of equations, it houses the static coefficients of the variables.
    
- **Vector (**$x, b$**):** A column list of numbers. Geometrically, it can represent either a static point in space or a directed arrow pointing from the origin to that point. By convention in linear algebra, vectors are always structured as column vectors unless stated otherwise.
    
- **Linear Combination:** The fundamental vector operation of stretching or shrinking vectors via scalar multiplication and gluing them together via vector addition. Mathematically expressed as:
    
    $$c_1 v_1 + c_2 v_2 + \dots + c_n v_n$$
- **Row Picture:** The geometric perspective where each individual linear equation is treated as a geometric hyperplane (a line in 2D, a plane in 3D). The solution is the single spatial coordinate where all these hyperplanes intersect.
    
- **Column Picture:** The geometric perspective where the columns of the coefficient matrix are treated as individual, movable vectors. The solution represents the scalar weights needed to scale these vectors so that they chain together to land exactly on the target vector $b$.
    
- **Singular Matrix:** A matrix whose columns do not point in completely independent directions. As a result, its column combinations fail to fill the entire dimensional space, meaning a unique solution to $Ax = b$ does not exist for every possible target $b$.
    

## 🛠️ Key Concepts

### 1. The Matrix-Vector Form: $Ax = b$

Professor Strang introduces the transition from a messy cluster of algebraic variables to a clean, consolidated matrix equation. Given a system of equations, we decouple the structure into three core mathematical components:

1. $A$: The coefficient matrix (an $n \times n$ square array of numbers).
    
2. $x$: The vector of unknowns (an $n \times 1$ column vector of variables).
    
3. $b$: The right-hand side target vector (an $n \times 1$ column vector of constants).
    

### 2. The Row Picture: Intersecting Constraints

In the Row Picture, you read the matrix equation **horizontally**, one row at a time. Each row represents a single independent constraint that limits where you can look in space.

- **In 2D (**$\mathbb{R}^2$**):** Each equation represents an infinitely long straight line. Solving the system means graphing both lines and locating the exact point $(x, y)$ where they cross.
    
- **In 3D (**$\mathbb{R}^3$**):** Each equation represents a flat, infinitely extending two-dimensional plane.
    
    - The first plane cuts through space.
        
    - The second plane intersects the first, narrowing down the potential solution zone from a whole plane to a single straight line where they meet.
        
    - The third plane acts as a final blade, intersecting that line at one specific coordinate $(x, y, z)$.
        

### 3. The Column Picture: Vector Navigation

In the Column Picture, you read the matrix **vertically**, down the columns. We strip away the matrix grid entirely and rewrite the system as a single vector equation.

Instead of asking, _"Where do these lines cross?"_ we ask a completely different, dynamic question: _"How do we mix the columns of matrix_ $A$ _to produce vector_ $b$_?"_ The unknown variables ($x, y, z$) are no longer spatial coordinates; they are **knobs or dials** that dictate how much we stretch or reverse each column vector. We place the scaled vectors tip-to-tail starting from the origin. If the combination is correct, the final vector tip lands exactly on the target vector $b$.

> [!important] The Fundamental Rule of Matrix Multiplication While schools often teach students to calculate $Ax$ by taking the dot product of rows with columns, this is a mechanical shortcut. The structural reality is:
> 
> $$\mathbf{Ax \text{ is a linear combination of the columns of } A}$$
> 
> If a matrix $A$ has columns $c_1, c_2, \dots, c_n$, then:
> 
> $$Ax = x_1 c_1 + x_2 c_2 + \dots + x_n c_n$$

### 4. Solvability and Space-Filling

Strang poses a profound question to motivate the entire trajectory of linear algebra:

> _"Can we solve_ $Ax = b$ _for every possible right-hand side vector_ $b$_?"_

To answer this using the Row Picture, you have to wonder if any set of random planes will always intersect nicely, which is tough to visualize. But the Column Picture makes the answer clear. It transforms the question into:

> _"Do the linear combinations of our column vectors fill up the entire_ $n$_-dimensional space?"_

- **The Non-Singular (Independent) Case:** If the column vectors point in independent, non-overlapping directions, their endless combinations will branch out and fill the entire space ($\mathbb{R}^n$). Because the vectors can reach any corner of the coordinate universe, you are guaranteed to find a unique combination to hit _any_ target vector $b$.
    
- **The Singular (Dependent) Case:** If your column vectors are redundant (for example, if three vectors in 3D space all lie completely flat on the same 2D desktop), their combinations are trapped. They can only reach points on that specific desktop. If a target vector $b$ hovers in the air above or below that desktop, it is impossible to reach. The system has **zero solutions** for that $b$.
    

## 📝 Worked Examples

### Example 1: The 2D System

Consider the foundational $2 \times 2$ system:

$$2x - y = 0$$$$-x + 2y = 3$$

Converted into matrix form $Ax = b$:

$$\begin{bmatrix} 2 & -1 \\ -1 & 2 \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} 0 \\ 3 \end{bmatrix}$$

#### Step-by-Step Row Picture Calculation

1. **Analyze Equation 1:** $2x - y = 0 \implies y = 2x$. This is a line passing through the origin $(0,0)$ with a slope of $2$.
    
2. **Analyze Equation 2:** $-x + 2y = 3 \implies 2y = x + 3 \implies y = \frac{1}{2}x + 1.5$. This is a line passing through the y-axis at $1.5$ with a slope of $0.5$.
    
3. **Find the Intersection:** The two lines intersect precisely at the coordinate point:
    
    $$\mathbf{x = 1, \quad y = 2}$$

#### Step-by-Step Column Picture Construction

1. **Isolate the Columns:**
    
    $$c_1 = \begin{bmatrix} 2 \\ -1 \end{bmatrix}, \quad c_2 = \begin{bmatrix} -1 \\ 2 \end{bmatrix}$$
2. **Set up the Combination Equation:**
    
    $$x \begin{bmatrix} 2 \\ -1 \end{bmatrix} + y \begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 0 \\ 3 \end{bmatrix}$$
3. **Apply the Weights (**$x=1, y=2$**):**
    
    $$1 \begin{bmatrix} 2 \\ -1 \end{bmatrix} + 2 \begin{bmatrix} -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 2 \\ -1 \end{bmatrix} + \begin{bmatrix} -2 \\ 4 \end{bmatrix} = \begin{bmatrix} 2 + (-2) \\ -1 + 4 \end{bmatrix} = \begin{bmatrix} 0 \\ 3 \end{bmatrix}$$
4. **Geometric Construction:** * Start at the origin and travel along $1$ full unit of $c_1$, landing at $(2, -1)$.
    
    - From that tip, travel $2$ full lengths of $c_2$ (moving left $2$ units and up $4$ units).
        
    - The path terminates exactly at coordinate $(0, 3)$, matching target vector $b$.
        

### Example 2: The 3D System

Next, the system scales up to $3 \times 3$:

$$\begin{aligned} 2x - y \quad\quad &= 0 \\ -x + 2y - z &= -1 \\ \quad - y + 2z &= 4 \end{aligned}$$

In consolidated matrix form $Ax = b$:

$$\begin{bmatrix} 2 & -1 & 0 \\ -1 & 2 & -1 \\ 0 & -1 & 2 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \end{bmatrix} = \begin{bmatrix} 0 \\ -1 \\ 4 \end{bmatrix}$$

#### Step-by-Step Row Picture Analysis

1. Equation 1 ($2x - y = 0$) forms a vertical plane running parallel to the z-axis.
    
2. Equation 2 forms an angled plane slashing through the center of the space. The intersection of Plane 1 and Plane 2 forms a long straight line.
    
3. Equation 3 forms a third distinct plane. The unique solution is the single point where that long intersection line pierces through Plane 3.
    
4. _Strang notes that visualizing three planes intersecting in 3D on a blackboard quickly becomes messy and difficult to interpret._
    

#### Step-by-Step Column Picture Analysis

1. Convert the system into a combination of three distinct vectors traveling through 3D space:
    
    $$x \begin{bmatrix} 2 \\ -1 \\ 0 \end{bmatrix} + y \begin{bmatrix} -1 \\ 2 \\ -1 \end{bmatrix} + z \begin{bmatrix} 0 \\ -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 0 \\ -1 \\ 4 \end{bmatrix}$$
2. **Inspection Tactics:** Instead of computing algebraic reductions immediately, Strang teaches us to build matrix intuition. Let's look at the third column: $c_3 = \begin{bmatrix} 0 \\ -1 \\ 2 \end{bmatrix}$ and our target vector $b = \begin{bmatrix} 0 \\ -1 \\ 4 \end{bmatrix}$. They are highly aligned.
    
3. Let's test a simple guess where we ignore column 1 completely ($x=0$). What if we try scaling column 2 by $1$ and column 3 by $2$?
    
    $$0 \begin{bmatrix} 2 \\ -1 \\ 0 \end{bmatrix} + 1 \begin{bmatrix} -1 \\ 2 \\ -1 \end{bmatrix} + 2 \begin{bmatrix} 0 \\ -1 \\ 2 \end{bmatrix} = \begin{bmatrix} -1 \\ 2 \\ -1 \end{bmatrix} + \begin{bmatrix} 0 \\ -2 \\ 4 \end{bmatrix} = \begin{bmatrix} -1 \\ 0 \\ 3 \end{bmatrix} \neq b$$
    
    Very close, but the first entry is $-1$ instead of $0$.
    
4. Now let's test the true solution: $x=1, y=2, z=2$.
    
    $$1 \begin{bmatrix} 2 \\ -1 \\ 0 \end{bmatrix} + 2 \begin{bmatrix} -1 \\ 2 \\ -1 \end{bmatrix} + 2 \begin{bmatrix} 0 \\ -1 \\ 2 \end{bmatrix} = \begin{bmatrix} 2 \\ -1 \\ 0 \end{bmatrix} + \begin{bmatrix} -2 \\ 4 \\ -2 \end{bmatrix} + \begin{bmatrix} 0 \\ -2 \\ 4 \end{bmatrix}$$
    
    Summing the individual rows:
    
    - **Top:** $2 - 2 + 0 = \mathbf{0}$
        
    - **Middle:** $-1 + 4 - 2 = \mathbf{-1}$
        
    - **Bottom:** $0 - 2 + 4 = \mathbf{4}$ The final combined vector lands perfectly at the target $\begin{bmatrix} 0 \\ -1 \\ 4 \end{bmatrix}$.
        

## 💡 Important Observations & Insights

> [!tip] The Limits of Row-Based Geometry The Row Picture is a conceptual dead end for high-dimensional mathematics. While our brains can visualize lines crossing in 2D and tolerate planes crossing in 3D, we completely lose the ability to imagine things when dealing with systems of 10, 100, or 10,000 equations. We cannot visualize ten 9-dimensional hyperplanes cutting through space.

> [!tip] The Scalability of Vector Columns The Column Picture scales effortlessly. Whether you are working in 3 dimensions or a 3-million-dimensional data space, the underlying mental operation never changes: you are simply scaling vectors and gluing them tip-to-tail.

## ⚠️ Common Mistakes and Misconceptions

> [!warning] The Dimension Mismatch Trap A massive point of confusion for beginners is mixing up the dimensions of the matrix with the dimensions of the vectors. If a matrix $A$ has a shape of $m \times n$ ($m$ rows and $n$ columns):
> 
> - The individual column vectors live in $\mathbb{R}^m$ (their length matches the number of rows).
>     
> - The unknown vector $x$ must live in $\mathbb{R}^n$ because you need exactly one scalar weight for each of the $n$ columns.
>     

> [!warning] The Independence Illusion Do not assume that having $n$ columns in an $n$-dimensional space automatically means you can reach any target point. If your columns are dependent (e.g., three vectors lying flat on the same 2D plane in a 3D room), they are structurally crippled. They cannot step off that plane, rendering the matrix singular.

## 🤖 Machine Learning Connections

### 📈 Machine Learning & Data Science

In real-world machine learning, datasets are organized as large tables where each row is a sample and each column is a specific feature. When you build a linear regression model to predict an outcome, your model calculates a prediction vector by taking a linear combination of those feature columns, using learned weights ($x$).

Understanding if your column vectors fill the space or are redundant directly dictates whether your model can find a stable, unique set of parameters. Redundant columns cause a classic problem known as multi-collinearity.

### 🧠 Neural Networks

Every individual layer inside a deep neural network performs a fundamental matrix-vector operation:

$$z = Wa + b$$

The weight matrix $W$ holds the connections of the layer. The activation vector $a$ from the previous layer provides the weights. Computing $Wa$ is taking a linear combination of the features learned by the network. The network builds complex representations by scaling and combining these structural columns at every step.

### 🎯 Optimization

Training a model is an optimization problem where you try to minimize an error metric. The algorithms step through parameter space by calculating a gradient vector and updating the model parameters.

These updates are linear combinations of directional vectors. Knowing how combinations of vectors fill or warp optimization space tells us whether an algorithm will converge cleanly or get stuck.

## ❓ Knowledge Check

> [!question] 1. If matrix $A$ has dimensions $5 \times 3$, how many rows and how many columns does it possess? **Answer:** It possesses $5$ rows and $3$ columns.

> [!question] 2. For the $5 \times 3$ matrix above, what must be the exact size of the vector $x$ for the operation $Ax$ to be mathematically valid? **Answer:** The vector $x$ must be a $3 \times 1$ column vector (it must have exactly 3 components to match the 3 columns of the matrix).

> [!question] 3. Explain the primary conceptual difference between the Row Picture and the Column Picture. **Answer:** The Row Picture looks for the mutual intersection point of geometric boundaries (lines/planes). The Column Picture looks for how to scale and chain together column vectors to land exactly on a target vector.

> [!question] 4. Write out the structural definition of $Ax$ using the Column Picture framework. **Answer:** $Ax = x_1 c_1 + x_2 c_2 + \dots + x_n c_n$, where $c_i$ are the individual columns of matrix $A$.

> [!question] 5. What geometric scenario occurs in the 3D Row Picture if a system of linear equations has no solution? **Answer:** The planes do not share a common intersection point. They could be parallel, or two might intersect in a line that runs completely parallel to the third plane, never touching it.

> [!question] 6. Express this system as a vector linear combination:
> 
> $$5x + 3y = 9$$$$2x - 7y = -1$$
> 
> **Answer:** >
> 
> $$x \begin{bmatrix} 5 \\ 2 \end{bmatrix} + y \begin{bmatrix} 3 \\ -7 \end{bmatrix} = \begin{bmatrix} 9 \\ -1 \end{bmatrix}$$

> [!question] 7. If a $3 \times 3$ matrix is singular, what does that mean for its column vectors geometrically? **Answer:** It means the three column vectors do not point in independent directions; they all lie flat on the same two-dimensional plane or along a single line, failing to fill out 3D space.

> [!question] 8. Calculate the linear combination:
> 
> $$3 \begin{bmatrix} 2 \\ -1 \\ 4 \end{bmatrix} - 2 \begin{bmatrix} 1 \\ 5 \\ 0 \end{bmatrix}$$
> 
> **Answer:** >
> 
> $$\begin{bmatrix} 3(2) - 2(1) \\ 3(-1) - 2(5) \\ 3(4) - 2(0) \end{bmatrix} = \begin{bmatrix} 6 - 2 \\ -3 - 10 \\ 12 - 0 \end{bmatrix} = \begin{bmatrix} 4 \\ -13 \\ 12 \end{bmatrix}$$

> [!question] 9. True or False: Calculating $Ax$ by taking the dot product of matrix rows with the vector $x$ yields a different numerical result than combining the columns. **Answer:** False. Both approaches yield the exact same numerical output vector. The row method is an arithmetic shortcut, while the column method provides the underlying geometric insight.

> [!question] 10. If the target vector $b$ is exactly equal to column 2 of a $3 \times 3$ matrix $A$, what is the exact solution vector $x$ to the equation $Ax = b$? **Answer:** The solution vector is $x = \begin{bmatrix} 0 \\ 1 \\ 0 \end{bmatrix}$. This specifies that you need exactly $0$ of column 1, $1$ of column 2, and $0$ of column 3.

## ⚡ Revision Sheet (One-Page Exam Summary)

### 1. The Core Matrix Identity

$$Ax = b$$

- $A$ **(Matrix):** Data transformations / structural constraints. Size: $m \times n$.
    
- $x$ **(Vector):** Combinatorial weights / unknown coefficients. Size: $n \times 1$.
    
- $b$ **(Vector):** Target state / output boundary. Size: $m \times 1$.
    

### 2. Row Picture vs. Column Picture

- **Row Picture (Intersection):** Reads horizontally. Each row is an equation representing a line or plane. Solution is the shared crossing point. **Does not scale visually.**
    
- **Column Picture (Combination):** Reads vertically. Each column is a vector. Solution is the set of scaling factors to reach the target vector. **Scales perfectly to** $n$**-dimensions.**
    

### 3. Core Operational Identity

$$Ax = x_1 (\text{Column 1}) + x_2 (\text{Column 2}) + \dots + x_n (\text{Column n})$$

- **Multiplication Rule:** To multiply a matrix by a vector, take the elements of the vector as weights and form a linear combination of the matrix columns.
    

### 4. Solvability Criteria

- **Non-Singular Matrix:** Columns point in independent directions. Their linear combinations completely fill the space ($\mathbb{R}^n$). A unique solution exists for **every** possible target vector $b$.
    
- **Singular Matrix:** Columns are dependent and trapped on a lower-dimensional subspace (a flat line or plane within a larger space).
    
    - If $b$ lies **off** that subspace: **Zero Solutions**.
        
    - If $b$ lies **on** that subspace: **Infinitely Many Solutions**.

[[Lecture2_The_Elimination_of_Matrices]]