
Until now, our calculus toolkit has been restricted to algebraic functions (like polynomials and rational functions) and trigonometric functions (like $\sin x$ and $\cos x$). However, nature rarely conforms purely to polynomial limits. Processes such as radioactive decay, population explosions, temperature cooling, and continuous compound interest require a family of functions where the variable sits in the exponent rather than the base.

In this lecture, we unlock exponential and logarithmic functions. We will see that trying to differentiate an exponential function leads directly to a mysterious constant multiplier. Resolving this multiplier geometrically is what forces the creation of the number $e$, arguably the most important base in all of mathematical analysis.

# Exponential Functions

## Definition

An **exponential function** is a function of the form:

$$f(x) = a^x$$

where the base $a$ is a fixed positive real number constant ($a > 0$) and $a \neq 1$. The independent variable $x$ serves as the exponent.

> **Crucial Conceptual Distinction:**
> 
> Do not confuse the Power Rule function $x^n$ with the Exponential Function $a^x$.
> 
> - In $x^n$, the base varies, and the exponent is constant ($\frac{d}{dx} x^n = n x^{n-1}$).
>     
> - In $a^x$, the base is constant, and the exponent varies. The power rule absolutely **does not apply** to $a^x$.
>     

## Properties

To compute $a^x$ for any real number $x$, we construct it sequentially:

1. **For Integer Exponents ($x = n$):** $a^n = a \cdot a \cdot \dots \cdot a$ (multiplied $n$ times).
    
2. **For Rational Exponents ($x = \frac{p}{q}$):** $a^{p/q} = \sqrt[q]{a^p}$, which represents the $q$-th root of $a$ raised to the power of $p$.
    
3. **For Irrational Exponents ($x = \pi$ or $\sqrt{2}$):** We define $a^x$ as a continuous limit of rational exponents approaching that irrational value (e.g., $a^{3.1}, a^{3.14}, a^{3.141}, \dots$).
    

## Graphical Interpretation

The graph of $y = a^x$ behaves as a smooth curve with the following features:

- It always passes through the $y$-intercept $(0, 1)$ because $a^0 = 1$ for any valid base.
    
- It never touches or crosses the $x$-axis ($a^x > 0$ for all $x$).
    
- The $x$-axis acts as a horizontal asymptote.
    

## Domain and Range

- **Domain:** $(-\infty, \infty)$ — You can plug any real number into the exponent.
    
- **Range:** $(0, \infty)$ — The output is strictly positive.
    

## Growth vs Decay

The structural behavior depends entirely on whether the base $a$ is greater than or less than 1:

|**Condition**|**Function Classification**|**Behavior as x→∞**|**Behavior as x→−∞**|
|---|---|---|---|
|**$a > 1$**|**Exponential Growth**|Approaches $\infty$|Approaches $0$ (Horizontal Asymptote)|
|**$0 < a < 1$**|**Exponential Decay**|Approaches $0$ (Horizontal Asymptote)|Approaches $\infty$|

## Important Identities

These three high-school identities form the operating framework for exponential calculus:

1. $a^{x_1 + x_2} = a^{x_1} \cdot a^{x_2}$
    
2. $a^{x_1 - x_2} = \frac{a^{x_1}}{a^{x_2}}$
    
3. $(a^{x_1})^{x_2} = a^{x_1 \cdot x_2}$
    

## Worked Examples

**Problem:** Simplify the expression $\frac{(2^{x+3})^2}{4^x}$ into a single unified term.

**Step-by-step Solution:**

1. Apply the power-to-power rule on the numerator:
    
    $$(2^{x+3})^2 = 2^{2(x+3)} = 2^{2x + 6}$$
    
2. Rewrite the denominator base $4$ as an explicit power of $2$:
    
    $$4^x = (2^2)^x = 2^{2x}$$
    
3. Combine using subtraction division rules:
    
    $$\frac{2^{2x + 6}}{2^{2x}} = 2^{(2x + 6) - 2x} = 2^6 = 64$$
    

# The Number e

## Definition

To find the derivative of an exponential function $f(x) = a^x$, we must apply the definition of the derivative as a limit:

$$f'(x) = \lim_{\Delta x \to 0} \frac{a^{x + \Delta x} - a^x}{\Delta x}$$

Using the identity $a^{x + \Delta x} = a^x \cdot a^{\Delta x}$, we can factor out $a^x$ from the numerator:

$$f'(x) = \lim_{\Delta x \to 0} \frac{a^x \cdot a^{\Delta x} - a^x}{\Delta x} = \lim_{\Delta x \to 0} a^x \left( \frac{a^{\Delta x} - 1}{\Delta x} \right)$$

Because $a^x$ does not depend on $\Delta x$, it can be pulled out of the limit entirely:

$$f'(x) = a^x \cdot \left( \lim_{\Delta x \to 0} \frac{a^{\Delta x} - 1}{\Delta x} \right)$$

Notice that the limit expression on the right is exactly the value of the derivative evaluated at $x = 0$ (since $f'(0) = \lim_{\Delta x \to 0} \frac{a^{\Delta x} - a^0}{\Delta x}$). Let us call this constant limit multiplier $M(a)$. Thus:

$$\frac{d}{dx}(a^x) = M(a) \cdot a^x \quad \text{where} \quad M(a) = \lim_{\Delta x \to 0} \frac{a^{\Delta x} - 1}{\Delta x}$$

## Why e is Special

The factor $M(a)$ represents the **slope of the tangent line to the curve $y = a^x$ at its $y$-intercept $(0,1)$**.

- If $a = 2$, numerical approximation shows $M(2) \approx 0.693$.
    
- If $a = 3$, numerical approximation shows $M(3) \approx 1.098$.
    

Because $M(2) < 1$ and $M(3) > 1$, there must exist a perfect, magical base between 2 and 3 such that the structural scaling multiplier $M(a)$ matches exactly $1$.

This ideal number is named **$e$** (Euler's number).

$$\text{We define } e \text{ as the unique base such that } \lim_{\Delta x \to 0} \frac{e^{\Delta x} - 1}{\Delta x} = 1$$

## Intuition Behind e

Geometrically, $e$ is the unique base whose exponential growth curve crosses the $y$-axis at an angle of exactly $45^\circ$, giving it a tangent line slope of exactly $1$.

$$\frac{d}{dx}(e^x) = 1 \cdot e^x = e^x$$

The natural exponential function is its own derivative. The taller the graph gets, the faster it grows, and its height matches its exact rate of upward climb at every single point along the domain continuum.

# Logarithmic Functions

## Definition

The **natural logarithmic function**, written as $\ln x$, is the inverse function of the natural exponential function $e^x$.

$$y = \ln x \iff e^y = x$$

Because it is an inverse, $\ln x$ answers the question: _"To what power must I raise $e$ to yield the number $x$?"_

## Inverse Relationship with Exponentials

Because $e^x$ and $\ln x$ cancel each other out, we obtain the fundamental cancellation equations:

1. $\ln(e^x) = x \quad \text{for all } x \in \mathbb{R}$
    
2. $e^{\ln x} = x \quad \text{for all } x > 0$
    

## Properties of Logarithms

Mirroring the rules of exponents, logarithms convert products into sums and powers into multipliers:

- **Product Rule:** $\ln(x_1 \cdot x_2) = \ln x_1 + \ln x_2$
    
- **Quotient Rule:** $\ln\left(\frac{x_1}{x_2}\right) = \ln x_1 - \ln x_2$
    
- **Power Rule:** $\ln(x^r) = r \ln x$
    

## Domain and Range

- **Domain:** $(0, \infty)$ — You cannot evaluate the logarithm of zero or a negative number.
    
- **Range:** $(-\infty, \infty)$ — Logarithms can output any positive or negative real value.
    

# Exponential and Logarithmic Equations

To solve equations containing variable terms locked in exponents or logs, use the inverse cancellation properties to isolate the target variable.

### Worked Example: Solving an Exponential Form

**Problem:** Solve for $x$ in the equation $5 \cdot e^{2x - 1} = 20$.

**Step-by-step Solution:**

1. Isolate the base term by dividing both sides by 5:
    
    $$e^{2x - 1} = 4$$
    
2. Take the natural logarithm ($\ln$) of both sides to cancel out the base $e$:
    
    $$\ln(e^{2x - 1}) = \ln(4) \implies 2x - 1 = \ln(4)$$
    
3. Isolate $x$:
    
    $$2x = \ln(4) + 1 \implies x = \frac{\ln(4) + 1}{2}$$
    

# Derivatives

Professor Jerison establishes two core derivative mechanics during this lecture: the derivative of the natural log and the broad rule for scaling any non-$e$ base.

## 1. Derivative of the Natural Logarithm ($\ln x$)

Using **Implicit Differentiation** (learned in Lecture 5), we can easily discover the derivative of $y = \ln x$.

**Derivation Step-by-Step:**

1. Write the logarithmic function in its inverse exponential form:
    
    $$e^y = x$$
    
2. Differentiate both sides with respect to $x$, applying the implicit Chain Rule to the left side:
    
    $$\frac{d}{dx}(e^y) = \frac{d}{dx}(x) \implies e^y \cdot \frac{dy}{dx} = 1$$
    
3. Isolate the derivative term $\frac{dy}{dx}$:
    
    $$\frac{dy}{dx} = \frac{1}{e^y}$$
    
4. Replace $e^y$ with its original definition value ($e^y = x$):
    
    $$\frac{d}{dx}(\ln x) = \frac{1}{x}$$
    

## 2. Derivative of any Base ($a^x$)

We can rewrite any arbitrary base $a$ as an exponential base $e$ using the identity $a = e^{\ln a}$. Therefore:

$$a^x = (e^{\ln a})^x = e^{(\ln a) \cdot x}$$

**Derivation Step-by-Step:**

1. Differentiate using the Chain Rule, where the outer function is $e^u$ and the inner function is $u = (\ln a) \cdot x$:
    
    $$\frac{d}{dx}(a^x) = \frac{d}{dx}\left(e^{(\ln a)x}\right) = e^{(\ln a)x} \cdot \frac{d}{dx}\big((\ln a)x\big)$$
    
2. Since $\ln a$ is a constant multiplier, the derivative of $(\ln a)x$ with respect to $x$ is simply $\ln a$:
    
    $$\frac{d}{dx}(a^x) = e^{(\ln a)x} \cdot \ln a$$
    
3. Substitute $a^x$ back in place of $e^{(\ln a)x}$:
    
    $$\frac{d}{dx}(a^x) = (\ln a) \cdot a^x$$
    

## 3. Logarithmic Differentiation

This technique is a lifesaver for differentiating large quotients containing multiple products and powers. Instead of fighting through a massive Product/Quotient rule combination, you take the log of both sides first to break the expression apart.

### Worked Example:

**Problem:** Differentiate $y = \frac{x^5 \cdot \sqrt{x^2 + 1}}{(3x - 2)^4}$.

**Step 1: Take the natural logarithm of both sides.**

$$\ln y = \ln \left( \frac{x^5 \cdot (x^2 + 1)^{1/2}}{(3x - 2)^4} \right)$$

**Step 2: Use log expansion rules to break the right side apart into independent terms.**

$$\ln y = 5\ln x + \frac{1}{2}\ln(x^2 + 1) - 4\ln(3x - 2)$$

**Step 3: Differentiate implicitly with respect to $x$.**

The derivative of the left side is always $\frac{1}{y} \cdot y'$. Apply the chain rule to each term on the right:

$$\frac{1}{y} y' = 5\left(\frac{1}{x}\right) + \frac{1}{2}\left(\frac{2x}{x^2 + 1}\right) - 4\left(\frac{3}{3x - 2}\right)$$

$$\frac{1}{y} y' = \frac{5}{x} + \frac{x}{x^2 + 1} - \frac{12}{3x - 2}$$

**Step 4: Multiply by $y$ to completely isolate $y'$.**

$$y' = y \cdot \left[ \frac{5}{x} + \frac{x}{x^2 + 1} - \frac{12}{3x - 2} \right]$$

Substitute the original formula for $y$ to finish:

$$y' = \frac{x^5 \cdot \sqrt{x^2 + 1}}{(3x - 2)^4} \cdot \left[ \frac{5}{x} + \frac{x}{x^2 + 1} - \frac{12}{3x - 2} \right]$$

## 4. Hyperbolic Functions

At the end of this session, Professor Jerison defines the **hyperbolic functions**, which are constructed directly from combinations of $e^x$ and $e^{-x}$.

- **Hyperbolic Sine:**
    
    $$\sinh x = \frac{e^x - e^{-x}}{2}$$
    
- **Hyperbolic Cosine:**
    
    $$\cosh x = \frac{e^x + e^{-x}}{2}$$
    

### Derivatives of Hyperbolic Functions

$$\frac{d}{dx}(\sinh x) = \frac{d}{dx}\left(\frac{e^x - e^{-x}}{2}\right) = \frac{e^x - (-e^{-x})}{2} = \frac{e^x + e^{-x}}{2} = \cosh x$$

$$\frac{d}{dx}(\cosh x) = \frac{d}{dx}\left(\frac{e^x + e^{-x}}{2}\right) = \frac{e^x + (-e^{-x})}{2} = \frac{e^x - e^{-x}}{2} = \sinh x$$

> **Note the structural difference from trigonometry:**
> 
> While $\frac{d}{dx}(\cos x) = -\sin x$, the hyperbolic derivative $\frac{d}{dx}(\cosh x)$ is **positive** $\sinh x$. There is no negative sign change here.

# Graphical Interpretation

Understanding the visual layout of these curves anchors the algebraic transformations:

- **Asymptotes:** The graph of $y = e^x$ has a horizontal asymptote along the negative axis line ($y = 0$). Its inverse function $y = \ln x$ flips this behavior across the line $y=x$, creating a vertical asymptote along the negative boundary line ($x = 0$).
    
- **Intersections:** $y = e^x$ crosses the $y$-axis at $(0,1)$. Meanwhile, $y = \ln x$ crosses the $x$-axis at $(1,0)$.
    
- **Parameter Adjustments ($e^{kx}$):** The variable parameter $k$ dictates the speed of growth. If $k > 0$, the function climbs exponentially. If $k < 0$, it models exponential decay.
    

# Key Formulas

$$\frac{d}{dx}(e^x) = e^x$$

$$\frac{d}{dx}(\ln x) = \frac{1}{x}$$

$$\frac{d}{dx}(a^x) = (\ln a)a^x$$

$$\frac{d}{dx}(\sinh x) = \cosh x \quad \text{and} \quad \frac{d}{dx}(\cosh x) = \sinh x$$

$$\cosh^2 x - \sinh^2 x = 1 \quad \text{(The Hyperbolic Identity)}$$

# Common Mistakes

### 1. Treating $a^x$ like a Power Rule Expression

- **The Mistake:** Writing $\frac{d}{dx}(2^x) = x \cdot 2^{x-1}$.
    
- **Why it is wrong:** The exponent is the variable here, not a constant. The Power Rule only applies when the base is a variable and the exponent is a constant number.
    
- **Correct Approach:** Use the proper exponential derivative law: $\frac{d}{dx}(2^x) = (\ln 2)2^x$.
    

### 2. Distributing Logarithms across Vector Additions

- **The Mistake:** Simplifying $\ln(x + y)$ to $\ln x + \ln y$.
    
- **Why it is wrong:** Logarithms expand _products_ into addition expressions, not additions into additions. $\ln(x+y)$ cannot be broken down algebraically.
    
- **Correct Knowledge:** Remember that $\ln(x \cdot y) = \ln x + \ln y$.
    

### 3. Dropping the Chain Rule Component inside Log Derivatives

- **The Mistake:** Writing $\frac{d}{dx} \ln(5x^3) = \frac{1}{5x^3}$.
    
- **Why it is wrong:** You must multiply by the derivative of the inner function argument (the chain step).
    
- **Correct Approach:** $\frac{1}{5x^3} \cdot \frac{d}{dx}(5x^3) = \frac{15x^2}{5x^3} = \frac{3}{x}$.
    

# Applications

- **Continuous Compound Interest:** If a financial balance $P$ is compounded continuously at an annual interest rate $r$, the money grows according to the formula $A(t) = P e^{rt}$.
    
- **Population Dynamics:** Unchecked bacterial colonies multiply at rates proportional to their current volume size, governed by $N(t) = N_0 e^{kt}$.
    
- **Radioactive Isotopes:** Unstable atoms decay according to a negative coefficient parameter: $A(t) = A_0 e^{-kt}$.
    

# Summary

Lecture 6 centers on completing our basic function toolkit by defining the calculus rules for exponentials and logarithms. We saw that differentiating $a^x$ produces a constant multiplier $M(a)$, which represents the curve's slope at $x=0$. By defining $e$ as the unique base where this slope equals exactly 1, we obtain the clean derivative rule $\frac{d}{dx}(e^x) = e^x$. The natural log ($\ln x$) is the inverse of $e^x$, and its derivative is $\frac{1}{x}$. These inverse properties allow us to use logarithmic differentiation to easily find the derivatives of complex equations.

# Key Takeaways

- The function $e^x$ is completely unique because its rate of change at any point equals its exact height value at that point.
    
- The derivative of the base function $\ln x$ is $\frac{1}{x}$.
    
- Logarithmic differentiation transforms complex products and quotients into simple addition and subtraction lines before you take any derivatives.
    
- Hyperbolic functions are combinations of $e^x$ and $e^{-x}$ that copy the behavior of standard trigonometric operations.
    

# Practice Questions

## Basic

1. Find the derivative of $f(x) = e^{4x}$.
    
2. Differentiate $y = \ln(3x)$.
    
3. Compute the derivative of $g(x) = 5^x$.
    
4. Solve for $x$: $\ln(x - 2) = 0$.
    
5. Find the slope of $y = e^x$ at $x = \ln(3)$.
    

## Intermediate

6. Differentiate $y = x^2 e^x$ using the Product Rule.
    
7. Find the derivative of $f(x) = \ln(\sin x)$.
    
8. Use implicit differentiation to find $\frac{dy}{dx}$ for $e^y + y = x$.
    
9. Compute the derivative of $h(x) = \cosh(x^2)$.
    
10. Find the equation of the tangent line to $y = \ln(x)$ at $x = e$.
    

## Challenge

11. Use logarithmic differentiation to find the derivative of the variable-base function $y = x^x$.
    
12. Prove the foundational hyperbolic identity: $\cosh^2 x - \sinh^2 x = 1$ by expanding the terms using their exponential definitions.
    
13. Find the derivative of $y = \ln\left( \frac{\sqrt{x^2+1}-x}{\sqrt{x^2+1}+x} \right)$ and simplify your final answer completely.

[[Lecture5_Implicit_Differentiation]]
[[Lecture9_Linear_&_Quadratic_approximations]]