# Lecture 1: Rate of Change

## Main Topics

- The Geometric Interpretation of the Derivative (Tangent vs. Secant Lines)
    
- The Physical Interpretation of the Derivative (Instantaneous Velocity)
    
- Computing a Derivative from Scratch ($f(x) = x^2$)
    
- Equations of Tangent Lines
    

## Introduction

Calculus is fundamentally the mathematics of change. While algebra and geometry excel at describing static, rigid systems, calculus allows us to analyze a world in motion. The core problem of differential calculus is determining how fast something is changing at a single, precise moment. This lecture introduces the fundamental tool used to solve this problem: **the derivative**. We will explore this concept through two distinct lenses: geometry (finding the slope of a curved line) and physics (finding the exact speed of a moving object).

## Topic 1: The Geometric View (Tangents and Secants)

### Definition

The geometric goal of differential calculus is to find the slope of a curve at a single point, known as the **tangent line**. Because a curve constantly changes its direction, we cannot use ordinary algebraic slope formulas directly. Instead, we approximate the tangent line using a **secant line**, which is a straight line drawn through two distinct points on the curve.

### Intuition

Imagine driving a car along a winding mountain road. If you freeze time at one exact moment, your headlights will point in a straight line that is perfectly aligned with the direction you are traveling at that exact spot. That straight line is the tangent line.

To find its slope mathematically, we pick two points on the curve that are close to each other and draw a straight line through them (a secant line). If we bring those two points closer and closer together until they practically merge into one, the changing slope of the secant line will gradually stabilize and match the exact slope of the tangent line.

### Mathematical Formulation

Let $P = (x_0, f(x_0))$ be a fixed point on the curve $y = f(x)$. Let $Q = (x_0 + \Delta x, f(x_0 + \Delta x))$ be a nearby moving point, where $\Delta x$ (read "delta x") represents a small change in the horizontal position.

The slope of the secant line ($m_{\text{sec}}$) is the change in $y$ divided by the change in $x$:

$$m_{\text{sec}} = \frac{\Delta y}{\Delta x} = \frac{f(x_0 + \Delta x) - f(x_0)}{\Delta x}$$

The slope of the tangent line ($m_{\text{tan}}$) is the **limit** of the secant slope as $\Delta x$ approaches zero:

$$f'(x_0) = \lim_{\Delta x \to 0} \frac{f(x_0 + \Delta x) - f(x_0)}{\Delta x}$$

This resulting value, $f'(x_0)$ (read "f-prime of x-nought"), is the **derivative** of $f$ at $x_0$.

### Example

**Problem:** Find the slope of the tangent line to the curve $f(x) = x^2$ at a general point $x_0$.

**Step 1: Set up the difference quotient.**

$$\frac{\Delta y}{\Delta x} = \frac{(x_0 + \Delta x)^2 - x_0^2}{\Delta x}$$

**Step 2: Expand the numerator algebraically.**

$$\frac{\Delta y}{\Delta x} = \frac{(x_0^2 + 2x_0\Delta x + (\Delta x)^2) - x_0^2}{\Delta x}$$

**Step 3: Simplify by canceling out $x_0^2$.**

$$\frac{\Delta y}{\Delta x} = \frac{2x_0\Delta x + (\Delta x)^2}{\Delta x}$$

**Step 4: Factor out and cancel $\Delta x$ from the numerator and denominator.**

$$\frac{\Delta y}{\Delta x} = \frac{\Delta x(2x_0 + \Delta x)}{\Delta x} = 2x_0 + \Delta x$$

**Step 5: Take the limit as $\Delta x \to 0$.**

$$f'(x_0) = \lim_{\Delta x \to 0} (2x_0 + \Delta x) = 2x_0$$

Thus, the slope of the tangent line to $y = x^2$ at any point $x$ is simply $2x$.

### Key Takeaways

- A **secant line** measures the _average_ rate of change over an interval.
    
- A **tangent line** measures the _instantaneous_ rate of change at a specific point.
    
- The derivative is the mathematical tool that turns a secant line into a tangent line via a limit.
    

## Topic 2: The Physical View (Instantaneous Velocity)

### Definition

Physically, the derivative represents an **instantaneous rate of change**. The most common application of this is velocity. If a function $s(t)$ tracks the position of an object over time $t$, its derivative $s'(t)$ tells us the exact speed and direction of that object at one precise instant.

### Intuition

If you drive $120\text{ kilometers}$ in $2\text{ hours}$, your _average speed_ is $60\text{ km/h}$. However, you were not traveling exactly $60\text{ km/h}$ during every single second of that trip. Sometimes you stopped at traffic lights ($0\text{ km/h}$), and sometimes you accelerated on the highway ($90\text{ km/h}$). Your speedometer shows your _instantaneous speed_—the speed you are traveling right now. Calculus lets us calculate this exact speedometer reading mathematically.

### Mathematical Formulation

Let $s(t)$ be the position function.

The **Average Velocity** over a time interval $\Delta t$ is:

$$v_{\text{avg}} = \frac{\Delta s}{\Delta t} = \frac{s(t_0 + \Delta t) - s(t_0)}{\Delta t}$$

The **Instantaneous Velocity** ($v$) at time $t_0$ is the limit of the average velocity as the time window shrinks to zero:

$$v(t_0) = \frac{ds}{dt} = \lim_{\Delta t \to 0} \frac{s(t_0 + \Delta t) - s(t_0)}{\Delta t}$$

### Example

**Problem:** A ball is dropped from the top of an exceptionally tall building. According to Galileo's law of falling bodies, the total distance fallen in meters after $t$ seconds is modeled by the function:

$$s(t) = 5t^2$$

Find the instantaneous velocity of the ball exactly $3\text{ seconds}$ after it is dropped.

**Step 1: Set up the limit definition using $t_0 = 3$.**

$$v(3) = \lim_{\Delta t \to 0} \frac{5(3 + \Delta t)^2 - 5(3)^2}{\Delta t}$$

**Step 2: Expand the terms inside the numerator.**

$$v(3) = \lim_{\Delta t \to 0} \frac{5(9 + 6\Delta t + (\Delta t)^2) - 45}{\Delta t}$$

$$v(3) = \lim_{\Delta t \to 0} \frac{45 + 30\Delta t + 5(\Delta t)^2 - 45}{\Delta t}$$

**Step 3: Simplify the numerator.**

$$v(3) = \lim_{\Delta t \to 0} \frac{30\Delta t + 5(\Delta t)^2}{\Delta t}$$

**Step 4: Factor out and divide by $\Delta t$.**

$$v(3) = \lim_{\Delta t \to 0} (30 + 5\Delta t)$$

**Step 5: Evaluate the limit by setting $\Delta t = 0$.**

$$v(3) = 30 + 5(0) = 30\text{ m/s}$$

The ball is falling at an instantaneous speed of $30\text{ meters per second}$ at $t = 3$.

### Key Takeaways

- Average velocity requires an interval of time; instantaneous velocity happens at a single instant.
    
- The units of a physical derivative are always $\text{units of output} / \text{units of input}$ (e.g., meters per second).
    

## Topic 3: Finding the Equation of a Tangent Line

### Definition

Once we know the derivative of a function at a specific point, we can construct the full algebraic equation of the straight tangent line passing through that point.

### Intuition

A line is uniquely defined if you know two things: where it sits (a point on the line) and where it points (its slope). For a tangent line to a curve at $x_0$, it must pass through the point $(x_0, f(x_0))$ on the curve, and it must have a slope equal to the derivative $f'(x_0)$. Armed with a point and a slope, we can use simple high-school algebra to write down the line's exact equation.

### Mathematical Formulation

Using the **point-slope formula** for a line ($y - y_0 = m(x - x_0)$), we substitute:

- $y_0 = f(x_0)$
    
- $m = f'(x_0)$
    

This gives us the standard equation for the tangent line:

$$y - f(x_0) = f'(x_0)(x - x_0)$$

Alternatively, solving explicitly for $y$:

$$y = f'(x_0)(x - x_0) + f(x_0)$$

### Example

**Problem:** Find the equation of the tangent line to the curve $f(x) = x^2$ at the point where $x = 3$.

**Step 1: Find the y-coordinate of the point.**

$$f(3) = 3^2 = 9 \implies \text{Point is } (3, 9)$$

**Step 2: Find the slope using the derivative.**

From our previous work, we know that if $f(x) = x^2$, then $f'(x) = 2x$.

$$m = f'(3) = 2(3) = 6$$

**Step 3: Plug the point $(3, 9)$ and slope $m = 6$ into the point-slope equation.**

$$y - 9 = 6(x - 3)$$

**Step 4: Simplify into slope-intercept form ($y = mx + b$).**

$$y - 9 = 6x - 18$$

$$y = 6x - 9$$

The equation of the tangent line is $y = 6x - 9$.

### Key Takeaways

- The derivative provides _only_ the slope of the line, not the full equation itself.
    
- You must always calculate the specific $y$-value ($f(x_0)$) and the specific slope value ($f'(x_0)$) before assembling the linear equation.
    

## Important Formulas

### The Difference Quotient

$$\frac{\Delta y}{\Delta x} = \frac{f(x_0 + \Delta x) - f(x_0)}{\Delta x}$$

This measures the average rate of change between two points or the slope of a secant line. It is the raw algebraic expression you must set up before applying a limit.

### The Definition of the Derivative

$$f'(x) = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}$$

This is the foundational definition of differential calculus. It computes the instantaneous rate of change or the exact slope of the tangent line at any point $x$.

### The Tangent Line Equation

$$y = f'(x_0)(x - x_0) + f(x_0)$$

The equation of the unique line that kisses a curve $f(x)$ perfectly at the localized coordinate point $(x_0, f(x_0))$.

## Common Mistakes

### 1. Treating $\Delta x$ as Two Separate Variables

- **The Mistake:** Trying to separate $\Delta$ from $x$ or canceling out the $\Delta$ symbol independently in fractions.
    
- **Why it is wrong:** $\Delta x$ is a single, unified symbol representing a "change in $x$". It cannot be broken apart into $\Delta \cdot x$.
    
- **Correct approach:** Treat $\Delta x$ exactly like you would treat a single variable placeholder (like $h$ or $z$) during algebraic manipulations.
    

### 2. Setting $\Delta x = 0$ Too Early

- **The Mistake:** Plunging $\Delta x = 0$ straight into the initial difference quotient expression:
    
    $$\frac{f(x+0) - f(x)}{0} = \frac{0}{0}$$
    
- **Why it is wrong:** Division by zero is undefined. This yields an indeterminate form ($\frac{0}{0}$), which provides no useful information.
    
- **Correct approach:** You must perform algebraic expansions and simplify the expression to completely cancel out the $\Delta x$ from the denominator _before_ evaluating the limit as $\Delta x \to 0$.
    

### 3. Confusing the Derivative Value with the Line Equation

- **The Mistake:** Stating that "the equation of the tangent line to $x^2$ at $x=3$ is $6$".
    
- **Why it is wrong:** $6$ is a single scalar number representing the _slope_ of the line. An equation for a line must contain variables (usually $x$ and $y$).
    
- **Correct approach:** Use the slope value as $m$ inside the point-slope template $y - y_0 = m(x - x_0)$ to derive the true linear formula.
    

## Conceptual Connections

- **Connection to Algebra:** In algebra, you learned that the slope of a line is constant ($m = \frac{y_2 - y_1}{x_2 - x_1}$). In Calculus, we expand this concept to curves, where the slope changes continuously at every single point.
    
- **Looking Ahead to Future Lectures:** This lecture computed derivatives by tedious algebraic expansion (the hard way). In upcoming lectures, we will use this exact definition to discover rapid shorthand rules (like the Power Rule and Product Rule) that allow us to find derivatives of complex functions instantly without setting up limits every time.
    

## Practice Questions

### Basic

1. Use the limit definition of the derivative to find $f'(x)$ for the constant function $f(x) = 7$. Explain why your answer makes intuitive geometric sense.
    
2. Find the average rate of change of the function $f(x) = x^2$ over the specific interval from $x = 2$ to $x = 5$.
    
3. Given that the derivative of $f(x) = x^2$ is $f'(x) = 2x$, find the slope of the tangent line to this curve at $x = -4$.
    

### Intermediate

4. Using the limit definition of the derivative, prove that the derivative of $f(x) = x^3$ is $f'(x) = 3x^2$. _(Hint: Expand $(x + \Delta x)^3$ using binomial expansion)._
    
5. Find the exact coordinate equations for the tangent line to the curve $f(x) = x^2$ at the point $x = -2$. Sketch both the curve and the line roughly to check your work.
    

### Challenge

6. A particle moves along a straight coordinate line such that its displacement position is governed by $s(t) = 2t^2 + 3t$ (where $s$ is in meters and $t$ is in seconds).
    
    - Find the formula for its instantaneous velocity at any time $t$ using limits.
        
    - Determine exactly when the particle reaches an instantaneous velocity of $15\text{ m/s}$.
        

## Lecture Recap

- **The Derivative represents two things simultaneously:** The geometric slope of a tangent line to a curve, and the physical instantaneous rate of change of a system.
    
- **The Secant line approximation** yields an average rate of change ($\frac{\Delta y}{\Delta x}$). By pushing the interval width $\Delta x$ toward $0$, we find the exact limit.
    
- **The algebraic process** requires writing out the difference quotient, expanding the numerator terms, canceling common components, factoring out $\Delta x$ to clear the denominator, and then substituting $\Delta x = 0$.
    
- **For the base function $y = x^2$**, the foundational derivative is mathematically proven to be $\frac{dy}{dx} = 2x$.

**[[Lecture2_Limits]]**