
## Learning Objectives

After completing this lecture notebook, you should be able to:

- Understand the fundamental concept of local approximation for smooth functions near a given point $x = x_0$ (or $x_0 = 0$).
    
- Derive the linear approximation formula $f(x) \approx f(x_0) + f'(x_0)(x - x_0)$ using the tangent line equation.
    
- Interpret second derivatives as measures of local curvature and concavity.
    
- Derive the quadratic approximation formula $f(x) \approx f(x_0) + f'(x_0)(x - x_0) + \frac{1}{2}f''(x_0)(x - x_0)^2$.
    
- Compute linear and quadratic approximations for standard transcendental and algebraic functions (e.g., $\sin x$, $\cos x$, $e^x$, $\ln(1+x)$, $(1+x)^r$).
    
- Analyze approximation errors both qualitatively and quantitatively as distance $\vert{}x - x_0\vert{}$ increases.
    
- Apply approximation techniques to real-world problems in physics, mental arithmetic, and numerical computing.
    

## Introduction

In calculus and applied mathematics, exact formulas can often be cumbersome, computationally expensive, or impossible to evaluate analytically. For instance, computing $\sqrt{4.02}$, $\cos(0.05)$, or solving nonlinear differential equations describing fluid dynamics in real time presents significant challenges.

Approximation allows us to replace complicated non-linear functions with simple polynomial functions (lines, parabolas) that are easy to compute, analyze, and manipulate.

> **Core Philosophy:** Near a specific point, a complicated smooth curve looks almost indistinguishable from its tangent line. If we need even greater accuracy, it looks almost identical to a specific fitting parabola.

These local approximations serve as the bedrock of engineering models, scientific computations, physics linearization (such as the small-angle approximation for pendulums), and modern machine learning algorithms.

# Big Picture

The central philosophy of differential calculus is **local linearity**: smooth curves, when zoomed in sufficiently near a point, look like straight lines.

```
       Zooming in on a smooth curve f(x) at x = a
       
    Curve f(x)                  Zoomed In               Very Close
      \                          \                        \
       \                          \                        \-----------  (Looks straight)
        \---                       \-------
            \                          \
```

1. **Local Behavior:** A function's global behavior may be wild or periodic, but its behavior in an infinitesimal neighborhood around $x_0$ is completely governed by its local derivatives at $x_0$.
    
2. **Polynomial Hierarchy:**
    
    - **$0^{\text{th}}$ Order:** Match the value $\Rightarrow f(x) \approx f(x_0)$ (constant approximation).
        
    - **$1^{\text{st}}$ Order:** Match the value and the slope $\Rightarrow$ **Linear Approximation** (tangent line).
        
    - **$2^{\text{nd}}$ Order:** Match the value, slope, and curvature $\Rightarrow$ **Quadratic Approximation** (fitting parabola).
        
3. **Smoothness:** This process relies on functions being _differentiable_ (smooth with no sharp corners or breaks).
    

# Linear Approximation

## Definition

The **linear approximation** (also called the _tangent line approximation_ or _linearization_) of a function $f(x)$ near a point $x_0$ is the unique linear function $L(x) = A + Bx$ that matches both the function's value and its first derivative at $x_0$.

## Geometric Interpretation

Geometrically, the linear approximation $L(x)$ represents the **tangent line** to the curve $y = f(x)$ at the point $(x_0, f(x_0))$.

```
  y ^
    |       /  y = f(x) [Original Curve]
    |      /
    |     /------- Tangent Line L(x)
    |    / .
    |   /  . Error = |f(x) - L(x)|
    |  /   .
    | /    |
    |/|    |
 ---+------+---------------------> x
    | x_0  x
```

- At $x = x_0$, $L(x_0) = f(x_0)$ exactly; the line and curve touch.
    
- For $x$ near $x_0$, the tangent line stays exceptionally close to the curve because they share the exact same instantaneous rate of change (slope).
    
- As $x$ moves further away from $x_0$, the curve pulls away from its tangent line, creating an **approximation error**.
    

## Derivation

Let $L(x) = A + B(x - x_0)$ be our target linear polynomial. We impose two constraints so that $L(x)$ best mimics $f(x)$ at $x = x_0$:

1. **Match Function Value:** $L(x_0) = f(x_0)$
    
2. **Match First Derivative (Slope):** $L'(x_0) = f'(x_0)$
    

**Step 1: Apply Condition 1**

$$L(x_0) = A + B(x_0 - x_0) = A + B(0) = A$$

Therefore:

$$A = f(x_0)$$

**Step 2: Apply Condition 2**

First, differentiate $L(x)$ with respect to $x$:

$$\frac{d}{dx}[L(x)] = \frac{d}{dx}[A + B(x - x_0)] = B$$

Setting $L'(x_0) = f'(x_0)$ yields:

$$B = f'(x_0)$$

**Step 3: Combine Results**

Substitute $A$ and $B$ back into $L(x)$:

$$L(x) = f(x_0) + f'(x_0)(x - x_0)$$

Setting $x - x_0 = \Delta x$, this can also be expressed in terms of change:

$$\Delta f \approx f'(x_0) \Delta x$$

## Formula

> **Linear Approximation Formula (Base Point $x_0$):**
> 
> $$f(x) \approx f(x_0) + f'(x_0)(x - x_0) \quad \text{for } x \approx x_0$$

When the base point is centered at the origin ($x_0 = 0$), setting $x_0 = 0$ yields the standard form:

> **Linear Approximation Formula (Base Point $x_0 = 0$):**
> 
> $$f(x) \approx f(0) + f'(0)x \quad \text{for } x \approx 0$$

### Explanation of Terms

- $f(x)$: The exact, difficult-to-calculate function value at $x$.
    
- $f(x_0)$: The known base value at anchor point $x_0$.
    
- $f'(x_0)$: The rate of change (slope of tangent line) at $x_0$.
    
- $(x - x_0)$ or $\Delta x$: The displacement distance from the base point $x_0$.
    

## Worked Examples

### Example 1: Estimating a Square Root

**Problem:** Estimate $\sqrt{4.02}$ using linear approximation.

**Solution:**

1. **Choose function and base point:** Let $f(x) = \sqrt{x} = x^{1/2}$. Choose $x_0 = 4$ because $\sqrt{4} = 2$ is exact and $4.02$ is very close to $4$.
    
2. **Compute derivatives:**
    
    $$f(x) = x^{1/2} \implies f(4) = \sqrt{4} = 2$$
    
    $$f'(x) = \frac{1}{2}x^{-1/2} = \frac{1}{2\sqrt{x}} \implies f'(4) = \frac{1}{2\sqrt{4}} = \frac{1}{4} = 0.25$$
    
3. **Set up linear approximation:**
    
    $$f(x) \approx f(4) + f'(4)(x - 4)$$
    
    $$\sqrt{x} \approx 2 + 0.25(x - 4)$$
    
4. **Evaluate at $x = 4.02$:**
    
    $$\sqrt{4.02} \approx 2 + 0.25(4.02 - 4) = 2 + 0.25(0.02) = 2 + 0.005 = 2.005$$
    

_(Actual value of $\sqrt{4.02} \approx 2.00499376...$, giving an error of less than $0.0003\%$.)_

### Example 2: Linear Approximation of $(1 + x)^r$ near $x = 0$

**Problem:** Derive the general linear approximation formula for $f(x) = (1 + x)^r$ near $x = 0$, and use it to estimate $\frac{1}{0.98}$.

**Solution:**

1. **Compute values at $x_0 = 0$:**
    
    $$f(x) = (1 + x)^r \implies f(0) = (1 + 0)^r = 1$$
    
    $$f'(x) = r(1 + x)^{r-1} \implies f'(0) = r(1 + 0)^{r-1} = r$$
    
2. **Apply Linear Formula:**
    
    $$f(x) \approx f(0) + f'(0)x \implies (1 + x)^r \approx 1 + rx$$
    
3. **Application to $\frac{1}{0.98}$:**
    
    Rewrite $\frac{1}{0.98} = (0.98)^{-1} = (1 - 0.02)^{-1}$.
    
    Here, $r = -1$ and $x = -0.02$.
    
    $$(1 - 0.02)^{-1} \approx 1 + (-1)(-0.02) = 1 + 0.02 = 1.02$$
    

### Example 3: Trigonometric Linear Approximation

**Problem:** Find the linear approximation of $f(x) = \sin x$ near $x_0 = 0$.

**Solution:**

1. $f(0) = \sin(0) = 0$
    
2. $f'(x) = \cos x \implies f'(0) = \cos(0) = 1$
    
3. Linear approximation:
    
    $$\sin x \approx 0 + 1(x - 0) \implies \sin x \approx x \quad (x \text{ in radians})$$
    

> **Key Physics Takeaway:** This is the famous **small-angle approximation** $\sin \theta \approx \theta$, used extensively in mechanics and pendulum analysis.

# Accuracy of Linear Approximation

The error of linear approximation is defined as the difference between the actual function value and the linear approximation:

$$E_1(x) = f(x) - L(x)$$

```
     Concave Up (f''(x) > 0)          Concave Down (f''(x) < 0)
     
        y = f(x)                         Tangent L(x)
          /                                 /-------\
         /   / Tangent L(x)                /         \
        /---/                             / y = f(x)  \
       /                                 /             \
    Underestimate (L(x) < f(x))        Overestimate (L(x) > f(x))
```

### Key Factors Governing Accuracy:

1. **Distance $\vert{}x - x_0\vert{}$:** As $x \to x_0$, the error $E_1(x) \to 0$ much faster than $x \to x_0$. Specifically, $E_1(x) \propto (x - x_0)^2$.
    
2. **Curvature / Second Derivative $f''(x)$:**
    
    - The linear approximation assumes the slope is constant ($f'(x_0)$).
        
    - If the graph curves away from the tangent line, $f''(x) \neq 0$.
        
    - If $f''(x) > 0$ (concave up), the tangent line sits **below** the curve $\implies L(x)$ **underestimates** $f(x)$.
        
    - If $f''(x) < 0$ (concave down), the tangent line sits **above** the curve $\implies L(x)$ **overestimates** $f(x)$.
        

# Quadratic Approximation

## Motivation

While linear approximation captures the **position** and **slope** of a function at $x_0$, it completely ignores **curvature**. If a function has strong bending (high second derivative), the tangent line rapidly diverges from the actual curve.

To fix this, we introduce a quadratic term proportional to $(x - x_0)^2$ to create a parabola $Q(x)$ that bends _with_ the function.

## Second Derivative and Curvature

Recall that:

- $f'(x)$ represents the rate of change of the function (slope).
    
- $f''(x)$ represents the rate of change of the slope, which measures **concavity** or **curvature**.
    

A linear approximation assumes $f''(x) = 0$. By matching $f''(x_0)$, our quadratic approximation bends upwards or downwards to mirror the local shape of $f(x)$.

## Derivation

We seek a second-degree polynomial $Q(x) = A + B(x - x_0) + C(x - x_0)^2$ satisfying three conditions at $x_0$:

1. $Q(x_0) = f(x_0)$ (matches position)
    
2. $Q'(x_0) = f'(x_0)$ (matches slope)
    
3. $Q''(x_0) = f''(x_0)$ (matches curvature)
    

**Step 1: Differentiate $Q(x)$**

$$Q(x) = A + B(x - x_0) + C(x - x_0)^2$$

$$Q'(x) = B + 2C(x - x_0)$$

$$Q''(x) = 2C$$

**Step 2: Apply Conditions at $x = x_0$**

1. $Q(x_0) = A + B(0) + C(0)^2 = A \implies A = f(x_0)$
    
2. $Q'(x_0) = B + 2C(0) = B \implies B = f'(x_0)$
    
3. $Q''(x_0) = 2C = f''(x_0) \implies C = \frac{1}{2}f''(x_0)$
    

Notice the factor of $\frac{1}{2}$! It appears naturally because $\frac{d^2}{dx^2}[(x - x_0)^2] = 2$. To match $f''(x_0)$, the coefficient $C$ must equal $\frac{1}{2}f''(x_0)$.

## Formula

> **Quadratic Approximation Formula (Base Point $x_0$):**
> 
> $$f(x) \approx f(x_0) + f'(x_0)(x - x_0) + \frac{1}{2}f''(x_0)(x - x_0)^2 \quad \text{for } x \approx x_0$$

Centered at $x_0 = 0$:

> **Quadratic Approximation Formula (Base Point $x_0 = 0$):**
> 
> $$f(x) \approx f(0) + f'(0)x + \frac{1}{2}f''(0)x^2 \quad \text{for } x \approx 0$$

## Geometric Interpretation

```
  y ^               Quadratic Q(x) [Osculating Parabola]
    |               .---. 
    |             .'     '.  y = f(x)
    |           .'    .---'--
    |          /    .'
    |         /   .'  Linear L(x)
    |        /  .'
 ---+-------+--'----------------> x
    |      x_0
```

The quadratic approximation $Q(x)$ is the **osculating parabola** ("kissing parabola") to $y = f(x)$ at $x_0$. It hugs the curve much tighter than the tangent line because both its slope and concavity match $f(x)$ at $x_0$.

## Standard Quadratic Approximations Near $x = 0$

Here are the fundamental quadratic approximations every calculus student should know:

### 1. Exponential Function: $f(x) = e^x$

- $f(0) = e^0 = 1$
    
- $f'(x) = e^x \implies f'(0) = 1$
    
- $f''(x) = e^x \implies f''(0) = 1$
    
    $$\mathbf{e^x \approx 1 + x + \frac{1}{2}x^2}$$
    

### 2. Sine Function: $f(x) = \sin x$

- $f(0) = \sin(0) = 0$
    
- $f'(x) = \cos x \implies f'(0) = 1$
    
- $f''(x) = -\sin x \implies f''(0) = 0$
    
    $$\mathbf{\sin x \approx x} \quad \text{(Quadratic term is zero!)}$$
    

### 3. Cosine Function: $f(x) = \cos x$

- $f(0) = \cos(0) = 1$
    
- $f'(x) = -\sin x \implies f'(0) = 0$
    
- $f''(x) = -\cos x \implies f''(0) = -1$
    
    $$\mathbf{\cos x \approx 1 - \frac{1}{2}x^2}$$
    

### 4. Natural Logarithm: $f(x) = \ln(1 + x)$

- $f(0) = \ln(1) = 0$
    
- $f'(x) = (1+x)^{-1} \implies f'(0) = 1$
    
- $f''(x) = -(1+x)^{-2} \implies f''(0) = -1$
    
    $$\mathbf{\ln(1 + x) \approx x - \frac{1}{2}x^2}$$
    

### 5. Binomial Series: $f(x) = (1 + x)^r$

- $f(0) = 1$
    
- $f'(0) = r$
    
- $f''(x) = r(r-1)(1+x)^{r-2} \implies f''(0) = r(r-1)$
    
    $$\mathbf{(1 + x)^r \approx 1 + rx + \frac{r(r-1)}{2}x^2}$$
    

# Comparing Linear and Quadratic Approximation

|**Feature**|**Linear Approximation L(x)**|**Quadratic Approximation Q(x)**|
|---|---|---|
|**Geometry**|Tangent Line|Fitting Parabola (Osculating)|
|**Formula ($x_0=0$)**|$f(0) + f'(0)x$|$f(0) + f'(0)x + \frac{1}{2}f''(0)x^2$|
|**Derivatives Required**|$f(0), f'(0)$|$f(0), f'(0), f''(0)$|
|**Captures**|Position, Slope|Position, Slope, Curvature/Concavity|
|**Error Order**|$\mathcal{O}(\Delta x^2)$|$\mathcal{O}(\Delta x^3)$|
|**Accuracy Region**|Very small neighborhood of $x_0$|Broader neighborhood around $x_0$|
|**Complexity**|Extremely simple (Linear)|Simple (Polynomial)|
|**Primary Usage**|Mental math, linear physics, differentials|Error bounds, physics, optimization|

# Error Analysis

When we replace $f(x)$ with an approximation, we incur an error $E(x)$:

- **Linear Error:** $E_1(x) = f(x) - L(x) \approx \frac{1}{2}f''(x_0)(x - x_0)^2$
    
- **Quadratic Error:** $E_2(x) = f(x) - Q(x) \approx \frac{1}{6}f'''(x_0)(x - x_0)^3$
    

```
 Error Size
    ^
    |                       / Linear Error ~ (x - x_0)^2
    |                      /
    |                     /    / Quadratic Error ~ (x - x_0)^3
    |                    /   /
    |                   /  /'
    |                  / .'
    +-----------------/.'------------> Distance |x - x_0|
                     x_0
```

### Why Quadratic Approximation is Superior Near $x_0$:

Suppose $\Delta x = 0.1$:

- The linear error term scales as $(\Delta x)^2 = (0.1)^2 = 0.01$.
    
- The quadratic error term scales as $(\Delta x)^3 = (0.1)^3 = 0.001$.
    

Because $(0.1)^3 \ll (0.1)^2$, quadratic approximation shrinks error significantly faster as $x$ approaches $x_0$.

# Applications

## 1. Fast Mental Estimation

- Compute $\sqrt{101}$: Let $x_0 = 100, \Delta x = 1$.
    
    $$\sqrt{100 + 1} \approx \sqrt{100} + \frac{1}{2\sqrt{100}}(1) = 10 + 0.05 = 10.05$$
    
    _(Exact: $10.049875...$)_
    

## 2. Physics: Pendulum Motion

The exact non-linear restoring force equation for a pendulum is:

$$\frac{d^2\theta}{dt^2} + \frac{g}{L}\sin\theta = 0$$

Using the linear approximation $\sin\theta \approx \theta$ for small angles yields the simple harmonic oscillator equation:

$$\frac{d^2\theta}{dt^2} + \frac{g}{L}\theta = 0$$

This linearization allows an exact closed-form solution for the pendulum period: $T = 2\pi\sqrt{\frac{L}{g}}$.

## 3. Physics: Kinetic Energy at Low Speeds

Einstein's relativistic total energy equation is $E = m\gamma c^2 = m c^2 (1 - v^2/c^2)^{-1/2}$.

Applying quadratic approximation $(1 + x)^{-1/2} \approx 1 - \frac{1}{2}x$ with $x = -\frac{v^2}{c^2}$:

$$E \approx m c^2 \left(1 + \frac{1}{2}\frac{v^2}{c^2}\right) = m c^2 + \frac{1}{2}m v^2$$

This reveals classical Newtonian kinetic energy ($\frac{1}{2}mv^2$) as a quadratic approximation of Einstein's relativity for $v \ll c$.

## 4. Machine Learning & Optimization

Newton's Method for finding local minima uses quadratic approximation. At any point, it builds a local parabola matching $f(x), f'(x),$ and $f''(x)$, then jumps directly to the minimum of that parabola:

$$x_{\text{next}} = x - \frac{f'(x)}{f''(x)}$$

# Connections to Previous Lectures

- **Limits (Lectures 1–3):** The derivative $f'(x_0) = \lim_{\Delta x \to 0} \frac{f(x_0+\Delta x) - f(x_0)}{\Delta x}$ is the limiting ratio that guarantees the linear approximation becomes exact in the limit.
    
- **Tangent Lines (Lecture 4):** Linear approximation is identical to the equation of the tangent line written as a function.
    
- **Exponentials, Logarithms & Trig (Lectures 5–8):** Derivative rules derived in earlier lectures provide the coefficients $f'(0)$ and $f''(0)$ for functions like $e^x, \ln(1+x), \sin x,$ and $\cos x$.
    

### Preview of Future Topics

- **Taylor Polynomials:** Extending quadratic approximation to $n^{\text{th}}$-degree polynomials matching up to $n^{\text{th}}$ derivatives.
    
- **Taylor Series:** Expressing functions as infinite series: $f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(x_0)}{n!}(x - x_0)^n$.
    

# Key Formulas

> **Linear Approximation ($x_0$):**
> 
> $$f(x) \approx f(x_0) + f'(x_0)(x - x_0)$$

> **Quadratic Approximation ($x_0$):**
> 
> $$f(x) \approx f(x_0) + f'(x_0)(x - x_0) + \frac{1}{2}f''(x_0)(x - x_0)^2$$

> **Common Quadratic Approximations Near $x = 0$:**
> 
> - $e^x \approx 1 + x + \frac{1}{2}x^2$
>     
> - $\sin x \approx x$
>     
> - $\cos x \approx 1 - \frac{1}{2}x^2$
>     
> - $\ln(1 + x) \approx x - \frac{1}{2}x^2$
>     
> - $(1 + x)^r \approx 1 + rx + \frac{r(r-1)}{2}x^2$
>     
> - $\frac{1}{1 - x} \approx 1 + x + x^2$
>     

# Common Mistakes

### 1. Forgetting the $\frac{1}{2}$ in the Quadratic Term

- **Incorrect:** $f(x) \approx f(0) + f'(0)x + f''(0)x^2$
    
- **Correct:** $f(x) \approx f(0) + f'(0)x + \mathbf{\frac{1}{2}}f''(0)x^2$
    
- _Why:_ Differentiating $x^2$ brings down a factor of $2$. The $\frac{1}{2}$ cancels it out so that $Q''(0) = f''(0)$.
    

### 2. Angle Units in Trigonometric Approximations

- **Incorrect:** Using degrees in $\sin x \approx x$ (e.g., $\sin(5^\circ) \approx 5$).
    
- **Correct:** Always convert angles to **radians** first ($5^\circ = \frac{5\pi}{180} \approx 0.0873 \implies \sin(5^\circ) \approx 0.0873$).
    

### 3. Expanding Around the Wrong Base Point $x_0$

- Trying to approximate $\sqrt{9.1}$ using $x_0 = 0$ instead of $x_0 = 9$.
    
- _Rule:_ Choose $x_0$ such that $f(x_0)$ is easy to compute _and_ $x$ is close to $x_0$.
    

# Summary

Linear and quadratic approximations are local polynomial fits to differentiable functions around a base point $x_0$.

- Linear approximation replaces a function with its **tangent line**, matching $f(x_0)$ and $f'(x_0)$.
    
- Quadratic approximation adds a second-order parabolic curve, matching $f(x_0), f'(x_0),$ and $f''(x_0)$ to capture local curvature.
    
- These approximations are vital tools for simplifying non-linear problems across mathematics, physics, engineering, and numerical analysis.
    

# Key Takeaways

1. **Local Linearity:** Smooth curves look straight when viewed up close; $f(x) \approx f(x_0) + f'(x_0)(x - x_0)$.
    
2. **Curvature Needs Quadratic:** To capture concavity/bending, include the second derivative term $\frac{1}{2}f''(x_0)(x - x_0)^2$.
    
3. **The Factor of $\frac{1}{2}$:** Arises naturally from integrating $x$ or matching second derivatives.
    
4. **Standard Approximations near $0$:** Master $e^x$, $\sin x$, $\cos x$, $\ln(1+x)$, and $(1+x)^r$.
    
5. **Error Dynamics:** Linear error scales with $\Delta x^2$; quadratic error scales with $\Delta x^3$.
    

# Practice Questions

## Basic

1. Compute the linear approximation of $f(x) = \sqrt{x}$ at $x_0 = 9$ and use it to estimate $\sqrt{9.3}$.
    
2. Derive the quadratic approximation of $f(x) = e^{-x}$ near $x = 0$.
    
3. Use the approximation $\cos x \approx 1 - \frac{1}{2}x^2$ to estimate $\cos(0.1)$.
    
4. What is the linear approximation of $f(x) = \frac{1}{1+x}$ near $x = 0$?
    
5. Determine whether the linear approximation $L(x)$ underestimates or overestimates $f(x) = x^3$ for $x > 0$.
    

## Intermediate

6. Derive the quadratic approximation of $f(x) = \tan x$ centered at $x_0 = 0$.
    
7. Use a quadratic approximation for $(1+x)^{1/3}$ to estimate $\sqrt[3]{1.06}$.
    
8. Find the linear and quadratic approximations of $f(x) = \sec x$ near $x_0 = 0$.
    
9. Estimate $\ln(1.1)$ using both linear and quadratic approximations. Compare your results to the calculator value ($0.095310...$).
    
10. Combine known approximations to find the quadratic approximation of $f(x) = e^x \cos x$ near $x = 0$ without taking third derivatives. _(Hint: Multiply $(1 + x + \frac{1}{2}x^2)(1 - \frac{1}{2}x^2)$ and discard terms higher than $x^2$.)_
    

## Challenge

11. **Relativistic Mechanics:** The relativistic energy of a particle with rest mass $m_0$ and momentum $p$ is $E = \sqrt{p^2 c^2 + m_0^2 c^4}$.
    
    - Factor out $m_0 c^2$ to express $E$ in the form $m_0 c^2 \sqrt{1 + u}$.
        
    - Find the quadratic approximation of $E$ in terms of $p$ for small momentum ($p \ll m_0 c$).
        
12. **Approximating Composite Functions:** Find the quadratic approximation of $f(x) = \ln(\cos x)$ near $x = 0$ by substituting the quadratic approximation of $\cos x$ into $\ln(1 + u)$.
    
13. **Error Bound Proof:** For $f(x) = e^x$ on the interval $[0, 0.1]$, prove that the absolute error of the linear approximation $\vert{}e^x - (1+x)\vert{}$ is bounded above by $\frac{e^{0.1}}{2} (0.1)^2 \approx 0.0055$.
    

# Exam Tips

- **Check the expansion point:** Make sure you evaluate derivatives at $x_0$ (e.g., $f'(0)$), not as general functions of $x$, before building your approximation polynomial.
    
- **Memorize standard expansions:** Memorize the quadratic approximations at $x_0 = 0$ for $e^x, \sin x, \cos x, \ln(1+x),$ and $(1+x)^r$. They are tested frequently and serve as building blocks.
    
- **Watch out for sign errors:** Pay close attention to negative signs when working with $\cos x$, $\ln(1-x)$, or negative powers like $(1+x)^{-1}$.
    
- **Radians only:** If an exam question asks for an approximation of a trigonometric function at an angle given in degrees (e.g., $1^\circ$), convert to radians before plugging into formulas!

[[Lecture6_Exponentials_and_Logarithms]]