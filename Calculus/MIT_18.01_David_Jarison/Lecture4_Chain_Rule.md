
## Main Topics

- Composition of Functions (The Inner vs. Outer Function)
    
- The Chain Rule Definition and Proof Intuition
    
- Leibniz Notation vs. Prime Notation for the Chain Rule
    
- The Generalized Power Rule
    

## Introduction

Up to this point, we have learned how to differentiate simple algebraic combinations of functions, such as sums, differences, and constant multiples. However, many real-world mathematical models involve functions nested inside other functions—known as composite functions. For instance, computing the derivative of $f(x) = (3x^2 + 1)^{50}$ or $f(x) = \sin(x^2)$ using only standard limit definitions or algebraic expansion would be incredibly tedious or downright impossible. This lecture introduces the **Chain Rule**, an elegant and foundational tool that allows us to seamlessly differentiate nested composite functions by breaking them down into a chain of simpler derivatives.

## Topic 1: Composition of Functions (The Inner vs. Outer Function)

### Definition

Before differentiating a nested expression, we must first learn to decompose it. A composite function is a function of the form $f(g(x))$. It represents a process where the output of an internal function $g(x)$ (the **inner function**) becomes the direct input for a subsequent function $f(u)$ (the **outer function**).

### Intuition

Think of function composition as an industrial assembly line or a manufacturing process.

- The raw materials ($x$) enter the first machine, which we will call the **inner machine** $g$. This machine processes $x$ and outputs an intermediate product ($u$).
    
- This intermediate product $u$ is then fed directly into the **outer machine** $f$. The outer machine processes $u$ and produces the final output ($y$).
    

To understand how the final output changes relative to our initial raw material, we must understand how both machines behave in sequence.

### Mathematical Formulation

If we define our ultimate output variable as $y$, we can break the composition $y = f(g(x))$ down explicitly into two interconnected equations:

$$u = g(x) \quad \text{(Inner Function)}$$

$$y = f(u) \quad \text{(Outer Function)}$$

### Example

**Problem:** Identify the inner function $g(x)$ and the outer function $f(u)$ for the composite expression $y = \sin(3x^2 + 5)$.

**Step 1: Identify the final operation being performed.**

The last operation performed when calculating this expression for any number $x$ is taking the sine. This means the sine function is wrapped around everything else on the outside.

**Step 2: Isolate the inner core expression.**

The algebraic argument tucked inside the sine operator is $3x^2 + 5$.

**Step 3: Write down the explicit decomposition.**

- Inner function: $u = g(x) = 3x^2 + 5$
    
- Outer function: $y = f(u) = \sin(u)$
    

### Key Takeaways

- Always look from the "inside out" when auditing a complex mathematical expression.
    
- The outer function must always be expressed in terms of an intermediate variable placeholder (like $u$), not in terms of $x$.
    

## Topic 2: The Chain Rule

### Definition

The **Chain Rule** states that the derivative of a composite function is the product of the derivative of the outer function (evaluated at the inner function) and the derivative of the inner function (evaluated at the independent variable).

### Intuition

Imagine three gears connected in a line: Gear $A$, Gear $B$, and Gear $C$.

- If Gear $A$ turns, it causes Gear $B$ to turn. Suppose Gear $B$ rotates **3 times faster** than Gear $A$.
    
- As Gear $B$ turns, it drives Gear $C$. Suppose Gear $C$ rotates **2 times faster** than Gear $B$.
    

If you turn Gear $A$ exactly once, how many times will Gear $C$ rotate? It will rotate $2 \times 3 = 6$ times faster than Gear $A$. You simply multiply the individual relative rates of change together. The Chain Rule does exactly this with rates of change on a smooth curve.

### Mathematical Formulation

There are two complementary ways to write the Chain Rule, and a student must become fluent in both.

#### 1. Leibniz Notation

If $y$ is a function of $u$, and $u$ is a function of $x$, then the rate of change of $y$ with respect to $x$ is:

$$\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$$

_Why this is beautiful:_ It looks exactly like fraction multiplication where the $du$ terms cancel out cleanly. While derivatives are technically limits of fractions rather than literal fractions, this notation is perfectly rigorous and serves as an excellent memory aid.

#### 2. Prime Notation

If we write the composite function as $h(x) = f(g(x))$, then its derivative is:

$$h'(x) = f'(g(x)) \cdot g'(x)$$

_How to read this aloud:_ "The derivative of the outside function, keeping the inside exactly the same, multiplied by the derivative of the inside function."

### Example

**Problem:** Find the derivative of $y = (x^3 + 2x)^5$.

**Step 1: Decompose into inner and outer components.**

- Inner: $u = x^3 + 2x$
    
- Outer: $y = u^5$
    

**Step 2: Differentiate both components independently using standard rules.**

- $\frac{du}{dx} = 3x^2 + 2$
    
- $\frac{dy}{du} = 5u^4$
    

**Step 3: Apply the Chain Rule by multiplying the derivatives together.**

$$\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx} = (5u^4) \cdot (3x^2 + 2)$$

**Step 4: Substitute back the original expression for $u$ so the final answer is completely in terms of $x$.**

$$\frac{dy}{dx} = 5(x^3 + 2x)^4 \cdot (3x^2 + 2)$$

### Key Takeaways

- The Chain Rule requires you to multiply individual link components together.
    
- When differentiating the outer shell, **never** change the inner contents until you move down the chain to the next step.
    

## Topic 3: The Generalized Power Rule

### Definition

A highly common special case of the Chain Rule occurs when a complex variable expression is raised to a constant power. Combining the classic Power Rule ($\frac{d}{dx} x^n = n x^{n-1}$) with the Chain Rule gives us the **Generalized Power Rule**.

### Intuition

Instead of expanding a large polynomial out into a massive string of single terms before running basic rules, we treat the entire internal grouping as a unified single item. We bring the power down to the front, reduce the power index by one, and then multiply the final structural result by the baseline rate of change of that original grouped item.

### Mathematical Formulation

If $u(x)$ is a differentiable function and $n$ is any real number:

$$\frac{d}{dx} [u(x)]^n = n \cdot [u(x)]^{n-1} \cdot u'(x)$$

### Example

**Problem:** Find the derivative function of $f(x) = \frac{1}{\sqrt{x^2 + 4}}$.

**Step 1: Rewrite the fraction using standard exponent rules.**

Before applying calculus, turn the radical fraction into a clean exponent expression:

$$f(x) = (x^2 + 4)^{-1/2}$$

**Step 2: Identify the core parts for the Generalized Power Rule.**

- Inner function: $u(x) = x^2 + 4$
    
- Exponent constant: $n = -\frac{1}{2}$
    

**Step 3: Apply the formula.**

First, bring down the exponent and decrement it by 1:

$$\text{Outer Part} = -\frac{1}{2}(x^2 + 4)^{-3/2}$$

Next, find the derivative of the inner function:

$$\text{Inner Derivative } u'(x) = \frac{d}{dx}(x^2 + 4) = 2x$$

**Step 4: Assemble and simplify.**

Multiply the outer part by the inner derivative:

$$f'(x) = -\frac{1}{2}(x^2 + 4)^{-3/2} \cdot (2x)$$

Notice that the $-\frac{1}{2}$ and the $2$ cancel out beautifully:

$$f'(x) = -x(x^2 + 4)^{-3/2}$$

Converting back to radical fraction notation:

$$f'(x) = -\frac{x}{\sqrt{(x^2 + 4)^3}}$$

### Key Takeaways

- Radical functions ($\sqrt{\dots}$) should always be converted to fractional powers ($\dots^{1/2}$) before evaluating derivatives.
    
- The Generalized Power Rule is not a new law; it is simply the Chain Rule hardwired for polynomial combinations.
    

## Important Formulas

### The Chain Rule (Leibniz Form)

$$\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$$

Ideal for visualizing rates of change as canceling fractions across a link of dependencies.

### The Chain Rule (Functional Form)

$$\frac{d}{dx}[f(g(x))] = f'(g(x)) \cdot g'(x)$$

Emphasizes that the outer derivative must be computed while holding the inner function completely constant, before multiplying by the inner function's own derivative.

### The Generalized Power Rule

$$\frac{d}{dx} [u]^n = n \cdot u^{n-1} \cdot \frac{du}{dx}$$

Bypasses long polynomial distributions for expressions of the format $[g(x)]^n$.

## Common Mistakes

### 1. Differentiating the Inside and Outside Simultaneously

- **The Mistake:** Writing the derivative of $\sin(x^2)$ as $\cos(2x)$.
    
- **Why it is wrong:** This approach incorrectly differentiates the outer function ($\sin \to \cos$) and the inner function ($x^2 \to 2x$) at the exact same time. The outer function's derivative must evaluate its arguments using the _original_ inner function.
    
- **Correct approach:** Leave the inside alone first, then multiply by the tail derivative:
    
    $$\frac{d}{dx}[\sin(x^2)] = \cos(x^2) \cdot 2x = 2x\cos(x^2)$$
    

### 2. Forgetting the "Tail" (Omitting $g'(x)$)

- **The Mistake:** Stating that if $y = (5x^3 + 1)^4$, then $\frac{dy}{dx} = 4(5x^3 + 1)^3$.
    
- **Why it is wrong:** This completely leaves out the final link in the chain ($\frac{du}{dx}$). It treats $(5x^3 + 1)$ as if it were just a plain $x$.
    
- **Correct approach:** Always remember to multiply by the derivative of that inner expression at the end:
    
    $$\frac{dy}{dx} = 4(5x^3 + 1)^3 \cdot \frac{d}{dx}(5x^3 + 1) = 4(5x^3 + 1)^3 \cdot (15x^2) = 60x^2(5x^3 + 1)^3$$
    

### 3. Misidentifying the Order of Operations

- **The Mistake:** Confusing the structural composition of $y = \sin^2(x)$ with $y = \sin(x^2)$.
    
- **Why it is wrong:** - $\sin^2(x)$ means $(\sin x)^2$. The _outer_ function is the squaring operation, and the _inner_ function is $\sin x$.
    
    - $\sin(x^2)$ means the _outer_ function is the sine operation, and the _inner_ function is $x^2$.
        
        Swapping these will lead to an incorrect derivative chain.
        
- **Correct approach:** Take a moment to read the expression carefully to confirm which operation is applied last.
    

## Conceptual Connections

- **Connection to Previous Lectures:** In Lecture 3, we proved that $\frac{d}{dx}(x^2) = 2x$. If we want to find the derivative of a matching compound term like $y = f(x)^2$, the chain rule tells us the rate is $2 \cdot f(x) \cdot f'(x)$. This shows how basic rules serve as the foundation for the Chain Rule.
    
- **Looking Ahead to Future Lectures:** The Chain Rule is arguably the most powerful tool in differential calculus. We will use it later to unlock **Implicit Differentiation** (differentiating equations that aren't isolated for $y$, like circles $x^2 + y^2 = r^2$) and to solve **Related Rates** problems in physics and engineering.
    

## Practice Questions

### Basic

1. Decompose the function $h(x) = \tan(4x^3)$ into its inner function $u = g(x)$ and outer function $y = f(u)$.
    
2. Differentiate $y = (2x + 7)^9$ using the Generalized Power Rule.
    
3. Find $\frac{dy}{dx}$ for $y = \cos(x^4)$.
    

### Intermediate

4. Find the equation of the tangent line to the curve $f(x) = \sqrt{3x^2 + 1}$ at the coordinate point $x = 1$.
    
5. Differentiate the fraction function $g(x) = \frac{4}{(2x^3 - 5)^2}$ by converting it to a negative exponent power rule format first.
    

### Challenge

6. **The Double Chain Rule:** Find the derivative function of $y = \sin^3(4x^2)$. _(Hint: This function can be rewritten as $y = [ \sin(4x^2) ]^3$. It features three layers of nested functions, requiring you to apply the Chain Rule twice in a row)._
    

## Lecture Recap

- **Composite functions wrap an outer framework around an inner core.** To differentiate them, you must work systematically from the outside in.
    
- **The Chain Rule states that $\frac{dy}{dx} = \frac{dy}{du} \cdot \frac{du}{dx}$.** You multiply the derivative of the outer layer by the derivative of the inner layer.
    
- **Never alter the internal argument** when taking the derivative of the outer function. Keep the inner function the same, then multiply by its derivative afterward.
    
- **The Generalized Power Rule** simplifies differentiating expressions of the form $[u(x)]^n$, producing a clean output of $n \cdot u^{n-1} \cdot u'$.

**[[Lecture3_Derrivatives]]**
[[Lecture5_Implicit_Differentiation]]