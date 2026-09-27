
## Main Topics

- Intuitive and Geometric Definition of a Limit
    
- Computing Easy Limits vs. Indeterminate Forms ($\frac{0}{0}$)
    
- One-Sided Limits and Non-Existence of Limits
    
- Important Trigonometric Limits ($\frac{\sin x}{x}$)
    

## Introduction

In Lecture 1, we defined the derivative as a limit: the slope of a secant line as the interval length $\Delta x$ shrinks to zero. However, we glided past a glaring logical contradiction: plugging $\Delta x = 0$ straight into our formula results in $\frac{0}{0}$, which is completely mathematically undefined. To build calculus on solid ground, we must precisely define what it means to "approach" a value without ever actually reaching it. This lecture covers **limits**, the foundational bedrock of all calculus.

## Topic 1: The Concept and Definition of a Limit

### Definition

A **limit** is the value that a function $f(x)$ approaches as its input $x$ approaches a specific target value, say, $c$. Crucially, a limit does _not_ care what the function actually equals exactly at $x = c$; it only cares about the behavior of the function as $x$ gets arbitrarily close to $c$.

### Intuition

Imagine walking along a path on a foggy mountain toward a designated observation deck. As you follow the path closer and closer to the coordinate longitude $x = c$, you can see where the deck _should_ be. Even if you arrive at $x = c$ and find that a sinkhole has completely swallowed the deck (leaving an empty hole in the function), or that the deck was built higher up on a platform (a displaced point), your journey along the path still clearly pointed you toward a specific expected height $L$. That expected height is the limit.

### Mathematical Formulation

We write the limit of a function as:

$$\lim_{x \to c} f(x) = L$$

This is read as: _"The limit of $f(x)$ as $x$ approaches $c$ equals $L$."_

This means that we can make $f(x)$ as close to $L$ as we want by choosing an $x$ that is sufficiently close to $c$ (but where $x \neq c$).

### Example

**Problem:** Evaluate $\lim_{x \to 2} \frac{x^2 - 4}{x - 2}$

**Step 1: Attempt direct substitution.**

If we try to plug in $x = 2$ directly:

$$f(2) = \frac{2^2 - 4}{2 - 2} = \frac{0}{0}$$

This tells us the function is undefined at $x = 2$. There is a hole in the graph.

**Step 2: Simplify algebraically to find the limit.**

We can factor the numerator using the difference of squares:

$$\frac{x^2 - 4}{x - 2} = \frac{(x - 2)(x + 2)}{x - 2}$$

**Step 3: Cancel the common factor.**

Since a limit examines $x$ values _close_ to 2 but explicitly _not equal_ to 2, the term $(x - 2)$ is not zero. Therefore, we are legally allowed to cancel it out:

$$\lim_{x \to 2} \frac{(x - 2)(x + 2)}{x - 2} = \lim_{x \to 2} (x + 2)$$

**Step 4: Evaluate the simplified limit.**

Now we can safely substitute $x = 2$:

$$\lim_{x \to 2} (x + 2) = 2 + 2 = 4$$

Even though the function does not exist at $x = 2$, it approaches the value of $4$ perfectly from both sides.

### Key Takeaways

- $\lim_{x \to c} f(x) = L$ means $f(x)$ gets close to $L$ when $x$ gets close to $c$.
    
- The value of $f(c)$ can be equal to $L$, it can be completely undefined, or it can be a totally different number. The limit remains unchanged.
    

## Topic 2: One-Sided Limits and When Limits Fail to Exist

### Definition

For a general two-sided limit $\lim_{x \to c} f(x)$ to exist, the function must approach the exact same value $L$ from both the left-hand side ($x < c$) and the right-hand side ($x > c$). If the function approaches two different values depending on which side you approach from, the two-sided limit **does not exist (DNE)**.

### Intuition

Imagine a bridge that has broken in the middle due to an earthquake. If you drive from the left bank, you end up at a height of $10\text{ meters}$. If you drive from the right bank, the road has shifted up, and you end up at a height of $15\text{ meters}$. Because the two sides do not meet up at the same point in space, there is no single "expected location" for the bridge center. The limit fails to exist.

### Mathematical Formulation

- **Left-Hand Limit (approaching from values smaller than $c$):**
    
    $$\lim_{x \to c^-} f(x) = L_1$$
    
- **Right-Hand Limit (approaching from values larger than $c$):**
    
    $$\lim_{x \to c^+} f(x) = L_2$$
    

**The Core Existence Theorem:**

$$\lim_{x \to c} f(x) = L \quad \text{if and only if} \quad \lim_{x \to c^-} f(x) = \lim_{x \to c^+} f(x) = L$$

### Example

**Problem:** Analyze the limit of the step function $f(x) = \frac{|x|}{x}$ as $x$ approaches $0$.

**Step 1: Evaluate the function for positive values ($x \to 0^+$).**

For any $x > 0$, $|x| = x$. Thus, $f(x) = \frac{x}{x} = 1$.

$$\lim_{x \to 0^+} \frac{|x|}{x} = \lim_{x \to 0^+} (1) = 1$$

**Step 2: Evaluate the function for negative values ($x \to 0^-$).**

For any $x < 0$, $|x| = -x$. Thus, $f(x) = \frac{-x}{x} = -1$.

$$\lim_{x \to 0^-} \frac{|x|}{x} = \lim_{x \to 0^-} (-1) = -1$$

**Step 3: Compare the one-sided limits.**

Since the left-hand limit ($-1$) does not equal the right-hand limit ($1$):

$$\lim_{x \to 0} \frac{|x|}{x} \quad \text{Does Not Exist (DNE)}$$

### Key Takeaways

- Limits can fail to exist due to sharp jumps, vertical asymptotes (blowing up to $\pm\infty$), or wild, infinite oscillation (like $f(x) = \sin(1/x)$ near $0$).
    
- Always test both sides of a target point if a function changes behavior there (e.g., piecewise or absolute value functions).
    

## Topic 3: An Essential Trigonometric Limit

### Definition

In calculus, certain foundational limits cannot be solved by simple factoring or algebraic manipulation. The most critical geometric limit introduced by Professor Jerison is the behavior of $\frac{\sin x}{x}$ near zero, where $x$ is measured strictly in **radians**.

### Intuition

As an angle $x$ becomes incredibly tiny, the arc length along a unit circle ($x$) and the straight vertical height ($\sin x$) become practically identical in size. Because these two values grow closer and closer to a $1:1$ ratio as the angle closes down to zero, their fraction approaches exactly $1$.

### Mathematical Formulation

$$\lim_{x \to 0} \frac{\sin x}{x} = 1$$

_(Note: Direct substitution yields $\frac{\sin(0)}{0} = \frac{0}{0}$, an indeterminate form. The proof of this limit relies on sandwiching the area of a circle sector between two triangles, a technique known as the Squeeze Theorem)._

### Example

**Problem:** Find $\lim_{x \to 0} \frac{\sin(4x)}{x}$.

**Step 1: Recognize that the input arguments do not match.**

The formula dictates that the angle inside the sine function must match the denominator exactly. Here we have $4x$ inside the sine, but only $x$ underneath.

**Step 2: Multiply by a clever form of 1 to balance the fraction.**

Multiply both the top and bottom by $4$:

$$\lim_{x \to 0} \frac{4 \cdot \sin(4x)}{4 \cdot x} = \lim_{x \to 0} 4 \cdot \left( \frac{\sin(4x)}{4x} \right)$$

**Step 3: Pull out constants and use a change of variable.**

Let a temporary variable $u = 4x$. As $x \to 0$, it is also true that $u \to 0$.

$$4 \cdot \lim_{u \to 0} \left( \frac{\sin u}{u} \right)$$

**Step 4: Substitute the known fundamental trig limit.**

Since $\lim_{u \to 0} \frac{\sin u}{u} = 1$:

$$4 \cdot (1) = 4$$

### Key Takeaways

- The limit formula $\lim_{\theta \to 0} \frac{\sin \theta}{\theta} = 1$ applies to _any_ expression $\theta$, provided that expression approaches $0$.
    
- By extension, another crucial structural limit to remember is:
    
    $$\lim_{x \to 0} \frac{1 - \cos x}{x} = 0$$
    

## Important Formulas

### The Fundamental Existence Condition

$$\lim_{x \to c} f(x) = L \iff \lim_{x \to c^-} f(x) = \lim_{x \to c^+} f(x) = L$$

A limit only exists if the graph arrives at the same structural height from both the left and right directions simultaneously.

### The Special Sine Limit

$$\lim_{x \to 0} \frac{\sin x}{x} = 1$$

Crucial for finding the derivatives of trigonometric functions later. It dictates that for exceptionally small values of $x$, $\sin x \approx x$.

### The Special Cosine Limit

$$\lim_{x \to 0} \frac{1 - \cos x}{x} = 0$$

Another core geometric limit used to verify changes in trigonometric rates.

## Common Mistakes

### 1. Writing "Limit = $\frac{0}{0}$"

- **The Mistake:** Concluding that because substitution yields $\frac{0}{0}$, the limit itself equals $\frac{0}{0}$ or does not exist.
    
- **Why it is wrong:** $\frac{0}{0}$ is not a number; it is an **indeterminate form**. It is an explicit message that you have more analytical work to do (factoring, rationalizing, or using trig identities) to discover the actual hidden answer.
    
- **Correct approach:** Keep working algebraically to cancel out the problematic terms before re-evaluating.
    

### 2. Dropping the Limit Notation Prematurely

- **The Mistake:** Dropping the $\lim_{x \to c}$ text before actually performing the evaluation step.
    
    $$\lim_{x \to 2} \frac{x^2-4}{x-2} = x + 2 = 4 \quad \text{(Incorrect notation alignment)}$$
    
- **Why it is wrong:** $ \frac{x^2-4}{x-2}$ does not algebraically equal $x+2$ everywhere (it does not at $x=2$). You must keep writing the limit operator symbol until the step where you physically substitute the target number into the equation.
    
- **Correct approach:**
    
    $$\lim_{x \to 2} \frac{x^2-4}{x-2} = \lim_{x \to 2} (x+2) = 2+2 = 4$$
    

### 3. Mixing Up Radians and Degrees

- **The Mistake:** Thinking $\lim_{x \to 0} \frac{\sin x}{x} = 1$ works if $x$ is measured in degrees.
    
- **Why it is wrong:** If $x$ is in degrees, $\sin(1^\circ)$ is vastly smaller than the number $1$. The limit only holds true because radian measure links the angle directly to linear arc length.
    
- **Correct approach:** Always default to radians across all calculus applications.
    

## Conceptual Connections

- **Connection to Lecture 1:** The derivative definition $f'(x) = \lim_{\Delta x \to 0} \frac{\Delta y}{\Delta x}$ is simply a specific application of a limit where the input variable is $\Delta x$ and the target value $c = 0$.
    
- **Looking Ahead to Lecture 3:** We will use limits to formalize the mathematical definition of **Continuity**. A function is continuous if the path you expect to follow (the limit) matches the actual physical point built on the graph ($f(c)$).
    

## Practice Questions

### Basic

1. Evaluate $\lim_{x \to 5} (3x^2 - 2x + 4)$ using direct substitution.
    
2. Evaluate $\lim_{x \to -3} \frac{x+3}{x^2 - 9}$ by factoring the denominator.
    
3. If $\lim_{x \to c^-} f(x) = 4$ and $\lim_{x \to c^+} f(x) = 4$, what is $\lim_{x \to c} f(x)$? Does $f(c)$ have to equal 4?
    

### Intermediate

4. Evaluate $\lim_{x \to 0} \frac{\tan x}{x}$. _(Hint: Rewrite $\tan x$ as $\frac{\sin x}{\cos x}$ and break up the limit into parts)._
    
5. Let $f(x)$ be a piecewise function defined as:
    
    $$f(x) = \begin{cases} 2x + 1 & x < 1 \\ 5 & x = 1 \\ x^2 + 2 & x > 1 \end{cases}$$
    
    Find $\lim_{x \to 1^-} f(x)$, $\lim_{x \to 1^+} f(x)$, and determine if $\lim_{x \to 1} f(x)$ exists.
    

### Challenge

6. Evaluate the geometric limit: $\lim_{x \to 0} \frac{x}{\sin(3x)} \cdot \frac{1 - \cos x}{x^2}$.
    

## Lecture Recap

- **Limits look at behavior _near_ a point**, entirely independent of what occurs exactly at that point.
    
- **An indeterminate form ($\frac{0}{0}$)** indicates that common structural factors are cloaking the limit's true value, requiring algebraic simplification.
    
- **A limit exists if and only if** both the left-hand and right-hand one-sided approaches converge onto the exact same numerical value.
    
- **The foundational geometric limit** $\lim_{x \to 0} \frac{\sin x}{x} = 1$ is an essential building block that will allow us to differentiate trigonometric operations.

**[[Lecture1_Rate_of_Cahnge]]**
**[[Lecture3_Derrivatives]]**
