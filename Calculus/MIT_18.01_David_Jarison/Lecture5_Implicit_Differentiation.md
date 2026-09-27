## Main Topics

- Explicit vs. Implicit Functions
    
- The Methodology of Implicit Differentiation
    
- Derivatives of Inverse Functions (Square Roots, Arcsine, and Arctangent)
    
- Finding Slopes of Slanted Curves (Circles and Foliums)
    

## Introduction

Until now, we have dealt exclusively with functions written in an **explicit** form, meaning they are neatly isolated as $y = f(x)$. However, many important mathematical curves cannot be cleanly solved for $y$. For example, a simple circle centered at the origin is defined by the equation $x^2 + y^2 = 25$. Forcing this equation into an explicit form requires splitting it into two separate messy radical functions: a top half ($y = \sqrt{25 - x^2}$) and a bottom half ($y = -\sqrt{25 - x^2}$).

This lecture introduces **Implicit Differentiation**, a powerful technique that allows us to find the derivative of an equation involving $x$ and $y$ directly, without ever needing to isolate $y$ first. It treats $y$ as an implicit function of $x$ and leverages the Chain Rule to differentiate both sides of an equation simultaneously.

## Topic 1: Explicit vs. Implicit Functions

### Definition

- An **explicit function** is a relation where the dependent variable is isolated entirely on one side of the equation:
    
    $$y = 3x^2 - 5x + 2$$
    
- An **implicit function** is a relation where $x$ and $y$ are tangled together in a single equation, meaning $y$ is defined _implicitly_ by its relationship to $x$:
    
    $$x^2 + y^2 = 25 \quad \text{or} \quad y^5 + x^2y^3 + x^9 = 4$$
    

### Intuition

Think of an explicit function like a direct command: _"To find $y$, take $x$, square it, and multiply it by 3."_ An implicit function is more like a riddle or a constraint: _"I'm thinking of two numbers, $x$ and $y$. When you square them both and add them together, they must always equal 25."_ Even though $y$ isn't isolated, a change in $x$ forces a corresponding change in $y$ to keep the relationship true. Because $y$ still depends dynamically on $x$, it still possesses a rate of change—a derivative $\frac{dy}{dx}$.

### Mathematical Formulation

When dealing with an implicit equation, we assume that $y$ is locally a differentiable function of $x$, even if we don't have a direct formula for it. We can represent this conceptually as:

$$F(x, y(x)) = c$$

When we see the variable $y$ in an expression, we must mentally read it as $[y(x)]$. Consequently, whenever we differentiate a term containing $y$ with respect to $x$, we _must_ apply the Chain Rule.

### Key Takeaways

- Explicit forms are isolated; implicit forms are locked within a relational expression.
    
- You do not need to algebraically isolate $y$ to study how fast $y$ changes relative to $x$.
    

## Topic 2: The Methodology of Implicit Differentiation

### Definition

Implicit differentiation is the process of finding $\frac{dy}{dx}$ by taking the derivative of both sides of an implicit equation with respect to $x$, keeping the Chain Rule in mind whenever differentiating a term containing $y$, and then algebraically isolating the symbol $\frac{dy}{dx}$.

### Intuition

Because both sides of an implicit equation are perfectly equal, their rates of change with respect to $x$ must also be perfectly equal. We apply our derivative operator $\frac{d}{dx}$ to everything from left to right.

The primary trap is remembering that $y$ is not a standalone independent variable. If you differentiate $x^2 with respect to $x$, the answer is simply $2x$. But if you differentiate $y^2$ with respect to $x$, the Chain Rule dictates that you differentiate the outside squaring function first ($2y$) and then multiply by the derivative of the inside core ($\frac{dy}{dx}$).

### Mathematical Formulation

For any term involving $y$, the fundamental operator rule is:

$$\frac{d}{dx}\big(g(y)\big) = g'(y) \cdot \frac{dy}{dx}$$

If $x$ and $y$ are multiplied together, you must combine this with the Product Rule:

$$\frac{d}{dx}(xy) = (1)(y) + x\left(\frac{dy}{dx}\right) = y + x\frac{dy}{dx}$$

### Example

**Problem:** Find the derivative $\frac{dy}{dx}$ for the circle equation $x^2 + y^2 = 25$.

**Step 1: Apply the derivative operator $\frac{d}{dx}$ to both sides of the equation.**

$$\frac{d}{dx}(x^2 + y^2) = \frac{d}{dx}(25)$$

**Step 2: Differentiate term by term.**

- $\frac{d}{dx}(x^2) = 2x$
    
- $\frac{d}{dx}(y^2) = 2y \cdot \frac{dy}{dx}$ (via the Chain Rule)
    
- $\frac{d}{dx}(25) = 0$ (the derivative of a constant is zero)
    

$$2x + 2y\frac{dy}{dx} = 0$$

**Step 3: Isolate the derivative term $\frac{dy}{dx}$.**

Subtract $2x$ from both sides:

$$2y\frac{dy}{dx} = -2x$$

Divide both sides by $2y$:

$$\frac{dy}{dx} = \frac{-2x}{2y}$$

Simplify the fraction:

$$\frac{dy}{dx} = -\frac{x}{y}$$

Notice that our derivative formula contains _both_ $x$ and $y$. This makes complete sense geometrically: on a circle, a single $x$ value has two different vertical points (top and bottom), each with a completely different tangent slope.

### Key Takeaways

- Every time you differentiate a term with a $y$ in it, you must tag a $\frac{dy}{dx}$ onto it.
    
- Implicit derivatives typically output expressions that depend on both coordinates $(x, y)$.
    

## Topic 3: Derivatives of Inverse Functions

### Definition

Implicit differentiation can be used to uncover the derivative rules for completely new functions, particularly **inverse functions**. If we know the derivative of a base function $f(x)$, we can set up an implicit relationship to discover the derivative of its inverse function $f^{-1}(x)$.

### Intuition

If you want to find the rate of change of an inverse operation, you can rewrite it in terms of its well-known forward operation. By flipping the equation, you get an implicit relationship that you can easily differentiate using the tools you already know, bypassing the need to evaluate complex limits from scratch.

### 1. The Square Root Function ($y = \sqrt{x}$)

**Step 1: Eliminate the radical by squaring both sides:**

$$y^2 = x \quad (\text{for } x > 0)$$

**Step 2: Differentiate both sides implicitly with respect to $x$:**

$$2y \cdot \frac{dy}{dx} = 1$$

**Step 3: Isolate $\frac{dy}{dx}$:**

$$\frac{dy}{dx} = \frac{1}{2y}$$

**Step 4: Substitute back into terms of $x$:**

Since $y = \sqrt{x}$, substitute it directly to get the final formula:

$$\frac{d}{dx}(\sqrt{x}) = \frac{1}{2\sqrt{x}}$$

### 2. The Arcsine Function ($y = \arcsin(x)$)

**Step 1: Invert the function to its forward form:**

$$\sin(y) = x \quad \text{where } y \in \left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$$

**Step 2: Differentiate both sides implicitly with respect to $x$:**

$$\frac{d}{dx}\big(\sin(y)\big) = \frac{d}{dx}(x)$$

$$\cos(y) \cdot \frac{dy}{dx} = 1$$

**Step 3: Isolate the derivative:**

$$\frac{dy}{dx} = \frac{1}{\cos(y)}$$

**Step 4: Substitute back into terms of $x$:**

To convert $\cos(y)$ into a sine-based expression, we use the Pythagorean identity $\sin^2(y) + \cos^2(y) = 1$, which gives $\cos(y) = \sqrt{1 - \sin^2(y)}$ (positive because cosine is non-negative on our restricted interval). Since $\sin(y) = x$:

$$\cos(y) = \sqrt{1 - x^2}$$

Plugging this back into our isolated expression yields:

$$\frac{d}{dx}(\arcsin x) = \frac{1}{\sqrt{1 - x^2}}$$

### 3. The Arctangent Function ($y = \arctan(x)$)

Professor Jerison emphasizes this specific derivation because it yields a clean algebraic polynomial fraction from a transcendental trigonometric function.

**Step 1: Invert the function:**

$$\tan(y) = x$$

**Step 2: Differentiate both sides implicitly with respect to $x$:**

$$\sec^2(y) \cdot \frac{dy}{dx} = 1$$

**Step 3: Isolate the derivative:**

$$\frac{dy}{dx} = \frac{1}{\sec^2(y)}$$

**Step 4: Substitute back into terms of $x$ using geometry:**

We look for a trigonometric identity that links $\sec^2(y)$ to $\tan(y)$. Using $1 + \tan^2(y) = \sec^2(y)$, or looking at the right triangle above where $\sec(y) = \frac{\text{hypotenuse}}{\text{adjacent}} = \frac{\sqrt{1+x^2}}{1}$, we find that:

$$\sec^2(y) = 1 + x^2$$

Substituting this directly into our denominator gives us the final formula:

$$\frac{d}{dx}(\arctan x) = \frac{1}{1 + x^2}$$

### Key Takeaways

- Implicit differentiation strips away complex transcendental trigonometric shells, mapping them directly onto simple algebraic rates of change.
    
- The technique turns difficult inverse derivatives into straightforward algebraic calculations.
    

## Important Formulas

### The Implicit Chain Rule Core

$$\frac{d}{dx}(y^n) = n y^{n-1} \cdot \frac{dy}{dx}$$

Differentiate the variable $y$ normally using the Power Rule, then multiply by $\frac{dy}{dx}$.

### The Product Rule Implicit Expansion

$$\frac{d}{dx}(x \cdot y) = y + x\frac{dy}{dx}$$

An essential expansion formula that appears frequently when equations contain cross-product terms of $x$ and $y$ multiplied together.

### Inverse Function Derivatives via Implicit Methods

$$\frac{d}{dx}(\sqrt{x}) = \frac{1}{2\sqrt{x}}$$

$$\frac{d}{dx}(\arcsin x) = \frac{1}{\sqrt{1 - x^2}}$$

$$\frac{d}{dx}(\arctan x) = \frac{1}{1 + x^2}$$

## Common Mistakes

### 1. Forgetting to Differentiate the Constant on the Right Side

- **The Mistake:** Writing $\frac{d}{dx}(x^2 + y^2 = 25) \implies 2x + 2y\frac{dy}{dx} = 25$.
    
- **Why it is wrong:** The derivative operator must apply equally to both sides of the equation. The derivative of the constant 25 is 0.
    
- **Correct approach:** Always write a $0$ (or differentiate whatever expression is present) on the right-hand side:
    
    $$2x + 2y\frac{dy}{dx} = 0$$
    

### 2. Leaving Out the $\frac{dy}{dx}$ Factor entirely

- **The Mistake:** Differentiating $x^2 + y^3 = 9$ to get $2x + 3y^2 = 0$.
    
- **Why it is wrong:** This treats $y$ as if it were an independent variable like $x$. It completely drops the internal chain link $\frac{dy}{dx}$.
    
- **Correct approach:** Tag your derivative marker onto every differentiated $y$ term:
    
    $$2x + 3y^2 \cdot \frac{dy}{dx} = 0$$
    

### 3. Misapplying the Product Rule to Coupled Terms

- **The Mistake:** Treating $\frac{d}{dx}(4xy^2)$ as simply $4 \cdot (1) \cdot (2y\frac{dy}{dx}) = 8xy\frac{dy}{dx}$.
    
- **Why it is wrong:** $x$ and $y^2$ are separate variable terms multiplied together. You must apply the full Product Rule template ($f'g + fg'$).
    
- **Correct approach:**
    
    $$\frac{d}{dx}(4x \cdot y^2) = 4y^2 + 4x\left(2y\frac{dy}{dx}\right) = 4y^2 + 8xy\frac{dy}{dx}$$
    

## Conceptual Connections

- **Connection to Lecture 4:** Implicit differentiation is not a new type of calculus; it is simply a clever application of the **Chain Rule** where the inner function happens to be an unisolated variable $y(x)$.
    
- **Looking Ahead to Future Lectures:** This technique is critical for finding slopes of complicated geometric curves. It will also serve as a prerequisite skill for solving **Related Rates** problems, where multiple variables change simultaneously with respect to time ($t$).
    

## Practice Questions

### Basic

1. Find $\frac{dy}{dx}$ for the equation $x^3 + y^3 = 8$ using implicit differentiation.
    
2. Find the slope of the tangent line to the circle $x^2 + y^2 = 100$ at the specific coordinate point $(6, -8)$.
    
3. Differentiate both sides of the equation $y^4 = x^3$ with respect to $x$ and solve for $\frac{dy}{dx}$.
    

### Intermediate

4. Find the equation of the tangent line to the curve defined by $x^2 + 2xy + y^2 = 16$ at the point $(2, 2)$.
    
5. Find the derivative $\frac{dy}{dx}$ for the equation $y^2 + \sin(y) = x^2$.
    

### Challenge

6. Consider the famous curve known as the **Folium of Descartes**, defined by the equation:
    
    $$x^3 + y^3 = 6xy$$
    
    - Find a general formula for $\frac{dy}{dx}$ in terms of $x$ and $y$.
        
    - Find the equation of the tangent line to this curve at the specific coordinate point $(3, 3)$.
        

## Lecture Recap

- **Implicit differentiation handles equations** where $x$ and $y$ are tied together and cannot be easily isolated explicitly.
    
- **The golden rule** is to treat $y$ as a function of $x$. Differentiate normally with respect to $y$, but always multiply by $\frac{dy}{dx}$ due to the Chain Rule.
    
- **The algebraic goal** requires moving all terms containing $\frac{dy}{dx}$ to one side of the equal sign, factoring out the $\frac{dy}{dx}$ term, and dividing to isolate it.
    
- **The technique naturally unlocks derivatives of inverse functions**, allowing us to map complex transcendental paths directly onto clean algebraic fractions.

[[Lecture4_Chain_Rule]]
[[Lecture6_Exponentials_and_Logarithms]]