
## Main Topics

- The Derivative as a Function ($f'(x)$ vs. $f'(x_0)$)
    
- Differentiation of Linear and Constant Functions
    
- Proving the Power Rule for Positive Integers ($x^n$)
    
- Linearity of the Derivative (Sum and Constant Multiple Rules)
    

## Introduction

In our opening lectures, we calculated the derivative at one specific, frozen point $x_0$. While that geometric and physical framework is essential, doing a full multi-step limit calculation every single time we want to find a slope is highly inefficient. In this lecture, we shift our perspective: we treat the derivative not just as a single number, but as a brand-new **function** in its own right. By treating the derivative as a function, we can use algebra to uncover structural patterns. This allows us to establish powerful, lightning-fast shortcut rules that bypass the limit definition entirely for common functions.

## Topic 1: The Derivative as a Function

### Definition

The derivative of a function $f(x)$ is a new function, denoted $f'(x)$, whose output at any given input $x$ is the instantaneous rate of change (or tangent slope) of the original function $f(x)$ at that point. The domain of $f'(x)$ consists of all points in the domain of $f$ where the function is **differentiable** (meaning the limit definition successfully converges to a finite number).

### Intuition

Think of the original function $f(x)$ as a profile of a roller coaster track showing its elevation at any horizontal distance $x$. The derivative function $f'(x)$ is a completely separate graph that tracks the _steepness_ of that roller coaster at position $x$. If the coaster is climbing a steep hill, the graph of $f'(x)$ is high above the x-axis (highly positive). If the coaster flattens out momentarily at the very peak, $f'(x)$ drops down to cross exactly at zero. If the coaster plunges downward, $f'(x)$ drops deep into negative territory.

### Mathematical Formulation

To calculate the derivative as a general function, we swap the fixed point $x_0$ for a free variable $x$ in the limit definition:

$$f'(x) = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}$$

Alternatively, it is common across many calculus textbooks to replace the symbol $\Delta x$ with a single variable $h$ to clean up the algebraic presentation:

$$f'(x) = \lim_{h \to 0} \frac{f(x + h) - f(x)}{h}$$

### Example

**Problem:** Find the general derivative function for $f(x) = \frac{1}{x}$ using the limit definition.

**Step 1: Set up the difference quotient using $h$.**

$$f'(x) = \lim_{h \to 0} \frac{\frac{1}{x+h} - \frac{1}{x}}{h}$$

**Step 2: Combine the fractions in the numerator using a common denominator.**

The common denominator for the top terms is $x(x+h)$:

$$\frac{1}{x+h} - \frac{1}{x} = \frac{x - (x+h)}{x(x+h)} = \frac{-h}{x(x+h)}$$

**Step 3: Substitute this back into the full limit expression.**

$$f'(x) = \lim_{h \to 0} \frac{\frac{-h}{x(x+h)}}{h} = \lim_{h \to 0} \frac{-h}{h \cdot x(x+h)}$$

**Step 4: Cancel the common factor of $h$ from the top and bottom.**

$$f'(x) = \lim_{h \to 0} \frac{-1}{x(x+h)}$$

**Step 5: Safely evaluate the limit by setting $h = 0$.**

$$f'(x) = \frac{-1}{x(x+0)} = -\frac{1}{x^2}$$

Thus, the derivative function of $f(x) = x^{-1}$ is $f'(x) = -x^{-2}$.

### Key Takeaways

- Finding $f'(x)$ gives you a general formula. You can then plug any specific $x$-coordinate into this formula to instantly find the slope at that point, completely bypassing the limit process.
    
- If $f(x)$ has a sharp corner, a vertical tangent, or a discontinuity at a point, the derivative function $f'(x)$ will be undefined at that point.
    

## Topic 2: Derivatives of Constants, Linear Functions, and the Power Rule

### Definition

Instead of calculating fractions for every type of function, we can establish universal rules for basic algebraic classes: constant functions ($f(x) = c$), linear functions ($f(x) = mx + b$), and power functions ($f(x) = x^n$).

### Intuition

- **Constant Functions:** A constant function $f(x) = c$ draws a perfectly horizontal flat line. A flat line has a steepness of zero everywhere, at all times. Therefore, its derivative must be zero.
    
- **Linear Functions:** A linear function $f(x) = mx + b$ already has a fixed, unchanging slope defined by $m$. Therefore, its instantaneous rate of change at any single point is identical to its overall slope $m$.
    
- **Power Functions:** For curves like $x^2, x^3, x^4$, the power rule establishes that the exponent drops down to front-multiply the term, and the remaining power is decremented by exactly 1.
    

### Mathematical Formulation

- **Constant Rule:**
    
    $$\frac{d}{dx}(c) = 0$$
    
- **Linear Rule:**
    
    $$\frac{d}{dx}(mx + b) = m$$
    
- **The Power Rule (for any positive integer $n$):**
    
    $$\frac{d}{dx}(x^n) = n x^{n-1}$$
    

### Example (Proof of the Power Rule)

To prove _why_ $\frac{d}{dx}(x^n) = n x^{n-1}$ is true, we evaluate the limit definition using the binomial expansion theorem to expand $(x+h)^n$:

$$(x+h)^n = x^n + n x^{n-1}h + \frac{n(n-1)}{2} x^{n-2}h^2 + \dots + h^n$$

Plugging this expansion directly into our derivative limit:

$$f'(x) = \lim_{h \to 0} \frac{(x^n + n x^{n-1}h + \text{terms with } h^2 \text{ or higher}) - x^n}{h}$$

The $x^n$ terms cancel out cleanly:

$$f'(x) = \lim_{h \to 0} \frac{n x^{n-1}h + \text{terms with } h^2 \text{ or higher}}{h}$$

Divide every term in the numerator by $h$:

$$f'(x) = \lim_{h \to 0} (n x^{n-1} + \text{terms still containing at least one } h)$$

When we evaluate the limit by driving $h \to 0$, every single trailing term vanishes to zero, leaving only the first term:

$$f'(x) = n x^{n-1}$$

### Key Takeaways

- The power rule works beautifully for all real-numbered exponents (integers, fractions, and negative values), even though this lecture focuses on proving it for positive integers.
    
- Visually, the power rule shows that polynomial differentiation reduces the overall degree of the function by one.
    

## Topic 3: Linearity of the Derivative (Sum and Constant Multiple Rules)

### Definition

The derivative operator is a **linear operator**. This means it distributes cleanly across addition and ignores scaling coefficients. If you know how to differentiate two individual functions separately, you can easily differentiate their sum or scalar multiples.

### Intuition

If you double the height of a hill (multiply a function by 2), you also double how steep the hill is at every single point along the path. Similarly, if you stack two terrain variations on top of one another (add two functions), the resulting steepness is simply the combined steepness of both individual profiles added together.

### Mathematical Formulation

- **The Constant Multiple Rule:** If $c$ is a constant number, then:
    
    $$\frac{d}{dx}[c \cdot f(x)] = c \cdot f'(x)$$
    
- **The Sum Rule:**
    
    $$\frac{d}{dx}[f(x) + g(x)] = f'(x) + g'(x)$$
    

Combining these rules allows us to differentiate any complex polynomial term-by-term.

### Example

**Problem:** Differentiate the polynomial function $y = 4x^3 - 5x^2 + 7x - 9$.

**Step 1: Break the polynomial into individual terms using the Sum Rule.**

$$\frac{dy}{dx} = \frac{d}{dx}(4x^3) - \frac{d}{dx}(5x^2) + \frac{d}{dx}(7x) - \frac{d}{dx}(9)$$

**Step 2: Pull out the constant coefficients using the Constant Multiple Rule.**

$$\frac{dy}{dx} = 4\frac{d}{dx}(x^3) - 5\frac{d}{dx}(x^2) + 7\frac{d}{dx}(x) - \frac{d}{dx}(9)$$

**Step 3: Apply the Power Rule, Linear Rule, and Constant Rule to their respective targets.**

- $\frac{d}{dx}(x^3) = 3x^2$
    
- $\frac{d}{dx}(x^2) = 2x$
    
- $\frac{d}{dx}(x) = 1$
    
- $\frac{d}{dx}(9) = 0$
    

$$\frac{dy}{dx} = 4(3x^2) - 5(2x) + 7(1) - 0$$

**Step 4: Multiply out coefficients to find the final derivative function.**

$$\frac{dy}{dx} = 12x^2 - 10x + 7$$

### Key Takeaways

- You can differentiate long polynomials by working through them term-by-term from left to right.
    
- Constant coefficients "ride along" through the differentiation process and multiply the resulting derivative.
    

## Important Formulas

### The Limit Definition of the Derivative Function

$$f'(x) = \lim_{h \to 0} \frac{f(x+h) - f(x)}{h}$$

The universal tool used to construct derivative shortcut formulas from scratch.

### The Power Rule

$$\frac{d}{dx}(x^n) = n x^{n-1}$$

The rapid shortcut for differentiating any power function. Drop the exponent to the front, then subtract 1 from the power.

### Combined Linearity Rule

$$\frac{d}{dx}[a \cdot f(x) + b \cdot g(x)] = a \cdot f'(x) + b \cdot g'(x)$$

Confirms that differentiation respects addition and constant scaling factors.

## Common Mistakes

### 1. Forgetting that the Derivative of a Standalone Constant is Zero

- **The Mistake:** Writing $\frac{d}{dx}(5x^2 + 7) = 10x + 7$.
    
- **Why it is wrong:** The number 7 is a standalone constant term. It does not change as $x$ changes, so its rate of change is 0. Students often confuse a standalone constant with a constant _coefficient_ (like the 5 in $5x^2$).
    
- **Correct approach:**
    
    $$\frac{d}{dx}(5x^2 + 7) = 10x + 0 = 10x$$
    

### 2. Applying the Power Rule to Exponential Functions

- **The Mistake:** Stating that $\frac{d}{dx}(2^x) = x \cdot 2^{x-1}$.
    
- **Why it is wrong:** The Power Rule _only_ applies when the base is a variable and the exponent is a constant ($x^n$). If the base is a number and the exponent is a variable ($a^x$), it is an exponential function, which requires a completely different rule.
    
- **Correct approach:** Do not use the power rule on expressions where $x$ is up in the exponent.
    

### 3. Misapplying the Power Rule to Radical Fractions

- **The Mistake:** Thinking that $\frac{d}{dx}\left(\frac{1}{x^3}\right) = \frac{1}{3x^2}$.
    
- **Why it is wrong:** You cannot run the power rule directly on a term buried inside a denominator. You must bring it to the numerator using negative exponents first.
    
- **Correct approach:** Rewrite as $x^{-3}$ first, then apply the power rule:
    
    $$-3x^{-4} = -\frac{3}{x^4}$$
    

## Conceptual Connections

- **Connection to Lectures 1 & 2:** We have elevated the limit concept from an approximation tool to a functional operator. The limit eliminates the $\frac{0}{0}$ ambiguity of the difference quotient, leaving us with a clean polynomial output.
    
- **Looking Ahead to Future Lectures:** Differentiating term-by-term works perfectly for addition and subtraction. However, differentiation **does not** distribute simply across multiplication or division. In upcoming lectures, we will develop specialized formulas to handle those operations: the **Product Rule** and the **Quotient Rule**.
    

## Practice Questions

### Basic

1. Find the derivative function $f'(x)$ for $f(x) = x^5 - 4x^3 + 2x - 1$.
    
2. Find the derivative of $g(x) = \frac{3}{x^4}$ by rewriting it with a negative exponent first.
    
3. Determine the slope of the curve $f(x) = 3x^2 - 5$ exactly at the coordinate point $x = 2$.
    

### Intermediate

4. Find the equation of the tangent line to the polynomial curve $f(x) = x^3 - 2x^2 + 4$ at the point where $x = 1$.
    
5. Use the limit definition of the derivative (using $h$) to prove that the derivative of a basic linear function $f(x) = mx + b$ is exactly $m$. Show every algebraic step cleanly.
    

### Challenge

6. Using our newly proven derivative for fractions ($\frac{d}{dx}(\frac{1}{x}) = -\frac{1}{x^2}$) along with the limit definition, prove the derivative function for $f(x) = \frac{1}{x^2}$ is $f'(x) = -\frac{2}{x^3}$.
    

## Lecture Recap

- **The derivative is a function $f'(x)$**, meaning it outputs a dynamic equation that maps the changing slope across the entire domain of $f(x)$.
    
- **The derivative of any constant is zero**, and the derivative of any linear expression $mx+b$ is simply the constant slope value $m$.
    
- **The Power Rule** ($\frac{d}{dx}x^n = n x^{n-1}$) simplifies polynomial differentiation by letting us drop and decrement powers.
    
- **Linearity allows term-by-term differentiation**, allowing us to find the derivative of complex polynomials by breaking them down into simpler steps.

**[[Lecture2_Limits]]**
**[[Lecture4_Chain_Rule]]**
