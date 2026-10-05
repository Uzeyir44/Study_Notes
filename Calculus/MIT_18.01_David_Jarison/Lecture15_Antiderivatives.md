
## 1. Motivation: Why Do We Need Antiderivatives?

Up to this point in calculus, we have focused entirely on **differentiation**. Differentiation asks a specific "forward" question:

- Given a function $F(x)$, what is its rate of change, $F'(x)$?
    

**Antidifferentiation** flips this process around. It asks the "reverse" question:

- Given a rate of change $f(x)$, can we find the original function $F(x)$ that produced it?
    

In other words, if we know how fast something is changing, can we determine how much of it is actually there?

Consider a simple example. Suppose we know the derivative of some mystery function $F(x)$ is $3x^2$.

$$F'(x) = 3x^2$$

From our previous rules of differentiation, we know that:

$$\frac{d}{dx}(x^3) = 3x^2$$

Therefore, the mystery function $F(x)$ could be $x^3$. We say that $x^3$ is an **antiderivative** of $3x^2$.

This reverse operation is incredibly useful in physics, engineering, and economics. If you know the velocity of a car (the rate of change of position), antidifferentiation allows you to recover the exact position of the car at any given time.

## 2. Definition of an Antiderivative

To formalize this idea, we distinguish carefully between the function we start with and the function we are trying to find. We typically use a lowercase letter for the given function and an uppercase letter for its antiderivative.

**Definition:** A function $F(x)$ is an **antiderivative** of $f(x)$ if:

$$F'(x) = f(x)$$

Let's look at the notation and what it represents:

- $f(x)$: The given function (the rate of change / the derivative we already have).
    
- $F(x)$: The antiderivative (the original function we are trying to find).
    
- $F'(x)$: The derivative of the antiderivative, which must equal $f(x)$ to be correct.
    

**Example:**

If $f(x) = \cos(x)$, then an antiderivative is $F(x) = \sin(x)$, because taking the derivative of $F(x)$ gives us exactly $f(x)$:

$$F'(x) = \frac{d}{dx}(\sin x) = \cos x = f(x)$$

## 3. Why Are There Infinitely Many Antiderivatives?

Here is a crucial conceptual point: **differentiation destroys constant information**.

Suppose we have the equation $F'(x) = f(x)$. What happens if we create a new function, $G(x)$, by taking our antiderivative $F(x)$ and adding a constant number $C$ to it?

$$G(x) = F(x) + C$$

Let's take the derivative of $G(x)$ to see if it is also an antiderivative of $f(x)$:

$$G'(x) = \frac{d}{dx}[F(x) + C] = F'(x) + 0 = f(x)$$

Because the derivative of any constant is zero, $G(x)$ is _also_ an antiderivative of $f(x)$.

For example, if $f(x) = 3x^2$, the following are **all** valid antiderivatives:

- $F_1(x) = x^3$
    
- $F_2(x) = x^3 + 5$
    
- $F_3(x) = x^3 - 42$
    
- $F_4(x) = x^3 + \pi$
    

Geometrically, adding a constant $C$ shifts the graph of a function vertically up or down. Because a vertical shift doesn't change the _steepness_ (slope) of the curve at any given $x$-value, all these parallel curves share the exact same derivative $f(x)$.

### Integral Notation

Because there is an entire family of antiderivatives for any function, we use a specific notation to represent all of them at once, called the **indefinite integral**:

$$\int f(x) \, dx = F(x) + C$$

- $\int$: The **integral sign**. It indicates the operation of antidifferentiation.
    
- $f(x)$: The **integrand**. This is the function we want to reverse-engineer.
    
- $dx$: The **differential**. It tells us which variable we are integrating with respect to (in this case, $x$). Think of $\int$ and $dx$ as bookends holding the function.
    
- $F(x)$: One specific antiderivative.
    
- $C$: The **constant of integration**. It represents any real number and ensures we capture the _entire family_ of possible antiderivatives.
    

## 4. Basic Antiderivative Rules

Instead of memorizing new formulas blindly, we can derive the rules for antiderivatives directly by reading our differentiation rules in reverse.

### The Power Rule for Antiderivatives

Let's find $\int x^n \, dx$. We know the derivative power rule is:

$$\frac{d}{dx}(x^{n+1}) = (n+1)x^n$$

We want the right side to be just $x^n$. So, let's divide both sides by the constant $(n+1)$:

$$\frac{d}{dx}\left(\frac{x^{n+1}}{n+1}\right) = x^n$$

Reading this backward, the function whose derivative is $x^n$ is $\frac{x^{n+1}}{n+1}$. Adding our constant of integration, we get the **Power Rule for Antiderivatives**:

$$\int x^n \, dx = \frac{x^{n+1}}{n+1} + C \quad \text{for } n \neq -1$$

To use this, you **add 1 to the exponent, and divide by the new exponent**.

_Note: $n \neq -1$ is a special case we will discuss in the next section._

### Linearity Rules

Because derivatives are linear (meaning you can pull out constants and split up addition), antiderivatives are linear as well.

1. **Constant Multiple Rule:**
    

$$\int c f(x) \, dx = c \int f(x) \, dx$$

```
*Reasoning:* \(\frac{d}{dx}[c F(x)] = c F'(x) = c f(x)\).
```

2. **Sum/Difference Rule:**
    

$$\int (f(x) + g(x)) \, dx = \int f(x) \, dx + \int g(x) \, dx$$

```
*Reasoning:* \(\frac{d}{dx}[F(x) + G(x)] = F'(x) + G'(x) = f(x) + g(x)\).
```

3. **Antiderivative of a Constant:**
    

$$\int c \, dx = cx + C$$

_Reasoning:_ The derivative of $cx$ is $c$. (This is just the power rule where $n=0$: $\int cx^0 dx = \frac{cx^1}{1} = cx$).

## 5. Important Special Case: $\int \frac{1}{x} \, dx$

Why did we specify $n \neq -1$ in the power rule? Let's see what happens if we blindly apply the power rule to $x^{-1}$, which is $\frac{1}{x}$:

$$\int x^{-1} \, dx = \frac{x^{-1+1}}{-1+1} + C = \frac{x^0}{0} + C$$

This results in division by zero, which is mathematically undefined. The power rule breaks down.

To find $\int \frac{1}{x} \, dx$, we have to search our memory: what function has a derivative of $\frac{1}{x}$?

From earlier lectures, we know that:

$$\frac{d}{dx}(\ln x) = \frac{1}{x}$$

However, the domain of $\frac{1}{x}$ is all real numbers except $x=0$, while the domain of $\ln(x)$ is only $x > 0$. What if $x$ is negative?

Let's check the derivative of $\ln(-x)$ for $x < 0$ using the chain rule:

$$\frac{d}{dx}(\ln(-x)) = \frac{1}{-x} \cdot (-1) = \frac{1}{x}$$

Remarkably, whether $x$ is positive or negative, the antiderivative results in the natural log of the positive version of $x$. We combine these two cases using absolute value bars:

$$\int \frac{1}{x} \, dx = \ln\vert{}x\vert{} + C$$

## 6. Antiderivatives of Exponential and Trigonometric Functions

To find the antiderivatives of common transcendental functions, we ask: "What function produces this expression when differentiated?"

**Exponential Function:**

- _Question:_ What function's derivative is $e^x$?
    
- _Answer:_ $e^x$.
    

$$\int e^x \, dx = e^x + C$$

**Cosine Function:**

- _Question:_ What function's derivative is $\cos x$?
    
- _Answer:_ $\sin x$.
    

$$\int \cos x \, dx = \sin x + C$$

**Sine Function (Watch the sign!):**

- _Question:_ What function's derivative is $\sin x$?
    
- _Answer:_ The derivative of $\cos x$ is $-\sin x$. To get a positive $\sin x$, we must start with $-\cos x$. Let's check: $\frac{d}{dx}(-\cos x) = -(-\sin x) = \sin x$.
    

$$\int \sin x \, dx = -\cos x + C$$

## 7. Antiderivatives and Initial Conditions

When we find an indefinite integral like $F(x) = x^2 + C$, we have found a general family of functions. Sometimes, we want to find one _specific_ function in that family. To find the exact value of $C$, we need one piece of extra information called an **initial condition** (often a specific point $(x_0, y_0)$ that the function passes through).

This forms a simple **differential equation**: a problem where we are given a derivative and an initial condition, and we must find the exact original function.

**Process:**

1. Find the general antiderivative (include the $+C$).
    
2. Substitute the given $x$ and $y$ values from the initial condition into the equation.
    
3. Solve algebraically for $C$.
    
4. Write the final specific function.
    

**Example:**

Find the function $F(x)$ if $F'(x) = 4x^3$ and $F(1) = 5$.

_Step 1: Find the general antiderivative._

$$F(x) = \int 4x^3 \, dx$$

$$F(x) = 4 \left( \frac{x^4}{4} \right) + C$$

$$F(x) = x^4 + C$$

_Step 2 & 3: Use the initial condition $F(1) = 5$ to find $C$._

We know that when $x = 1$, $F(x) = 5$.

$$5 = (1)^4 + C$$

$$5 = 1 + C$$

$$C = 4$$

_Step 4: Write the final function._

$$F(x) = x^4 + 4$$

_Note: If we ignored $C$ in step 1, our function would just be $x^4$, and $F(1)$ would equal 1, which violates our initial condition. The constant cannot be ignored!_

## 8. Geometric Interpretation

Geometrically, the equation $F'(x) = f(x)$ tells us that the given function $f(x)$ represents the **slope of the tangent line** to the graph of the antiderivative $F(x)$ at any point.

If you are looking at a graph of $f(x)$ and want to sketch what its antiderivative $F(x)$ looks like, you can use the rules of first derivatives we learned previously:

- Where $f(x) > 0$ (above the x-axis), the slope of $F(x)$ is positive, meaning $F(x)$ is **increasing**.
    
- Where $f(x) < 0$ (below the x-axis), the slope of $F(x)$ is negative, meaning $F(x)$ is **decreasing**.
    
- Where $f(x) = 0$ (crossing the x-axis), $F(x)$ has a slope of zero, meaning $F(x)$ has a **horizontal tangent** (a critical point—potentially a local maximum or minimum).
    
- If $f(x)$ changes sign from positive to negative, $F(x)$ changes from increasing to decreasing, creating a **local maximum**.
    

Because of the $+C$, $f(x)$ tells you the exact _shape_ of $F(x)$, but it tells you absolutely nothing about _how high or low_ the graph is placed on the y-axis.

## 9. Connection to Earlier Derivative Material

We can view the relationship between a function and its derivative as a bridge that we can now walk across in both directions.

- **Differentiation:** (Function $\longrightarrow$ Rate of Change). We start with a position function $s(t)$ and differentiate it to find the velocity $v(t)$. This is local analysis—measuring steepness at a single point.
    
- **Antidifferentiation:** (Rate of Change $\longrightarrow$ Function). We start with a velocity $v(t)$, and we "integrate" or accumulate that velocity to recover the position function $s(t)$.
    

This concept of recovering a whole function from its pieces of change is profound. It leads directly to the **Fundamental Theorem of Calculus**, which will formally bridge the gap between finding an antiderivative (the reverse of a slope) and calculating the area under a curve.

## 10. Worked Examples

**Example 1: Polynomials and Constants**

- **Problem:** Evaluate $\int (6x^2 - 4x + 7) \, dx$.
    
- **Goal:** Find the general antiderivative using linearity and the power rule.
    
- **Solution:**
    

$$\int 6x^2 \, dx - \int 4x \, dx + \int 7 \, dx$$

$$= 6\left(\frac{x^3}{3}\right) - 4\left(\frac{x^2}{2}\right) + 7x + C$$

$$= 2x^3 - 2x^2 + 7x + C$$

- **Verification:** $\frac{d}{dx}(2x^3 - 2x^2 + 7x + C) = 6x^2 - 4x + 7$. It matches.
    

**Example 2: Negative Exponents**

- **Problem:** Evaluate $\int \frac{3}{x^2} \, dx$.
    
- **Goal:** Rewrite as a power and apply the power rule.
    
- **Solution:** First, rewrite $\frac{1}{x^2}$ as $x^{-2}$.
    

$$\int 3x^{-2} \, dx = 3 \left( \frac{x^{-2+1}}{-2+1} \right) + C$$

$$= 3 \left( \frac{x^{-1}}{-1} \right) + C$$

$$= -3x^{-1} + C = -\frac{3}{x} + C$$

- **Verification:** $\frac{d}{dx}(-3x^{-1} + C) = (-3)(-1)x^{-2} = 3x^{-2} = \frac{3}{x^2}$. Matches.
    

**Example 3: $1/x$, Exponentials, and Trig**

- **Problem:** Evaluate $\int \left(\frac{5}{x} + 2e^x - \sin x\right) \, dx$.
    
- **Goal:** Identify the special rules for transcendental functions and $1/x$.
    
- **Solution:**
    
    The $5/x$ is $5 \cdot \frac{1}{x}$, which becomes $5 \ln\vert{}x\vert{}$.
    
    The $2e^x$ stays $2e^x$.
    
    The antiderivative of $-\sin x$ is $\cos x$ (because derivative of $\cos$ is $-\sin$).
    

$$5\ln\vert{}x\vert{} + 2e^x + \cos x + C$$

- **Verification:** $\frac{d}{dx}(5\ln\vert{}x\vert{} + 2e^x + \cos x + C) = \frac{5}{x} + 2e^x - \sin x$. Matches.
    

## 11. Common Mistakes

- **Forgetting $+C$:**
    
    - _Why it's wrong:_ Without $+C$, you are only writing down _one_ specific function, missing the infinitely many other functions that have the same derivative. It's an incomplete answer.
        
- **Applying the power rule to $1/x$:**
    
    - _Why it's wrong:_ Writing $\int x^{-1} dx = \frac{x^0}{0}$ results in division by zero. The correct answer is $\ln\vert{}x\vert{} + C$.
        
- **Getting the sign wrong for $\int \sin x \, dx$:**
    
    - _Why it's wrong:_ Students often remember that the derivative of $\sin x$ is positive $\cos x$, and mistakenly assume the integral of $\sin x$ is positive $\cos x$. Always check by differentiating: $\frac{d}{dx}(\cos x) = -\sin x$, which is the wrong sign! The correct integral is $-\cos x + C$.
        
- **Confusing $f(x)$ with its antiderivative $F(x)$:**
    
    - _Why it's wrong:_ When analyzing graphs, students will often look at $f(x)=0$ and think the antiderivative's _value_ is zero there. No—$f(x)=0$ means the antiderivative's _slope_ is zero.
        
- **Forgetting to apply an initial condition:**
    
    - _Why it's wrong:_ If the problem provides a point like $F(0)=5$, leaving $+C$ in your final answer means you didn't finish solving for the specific function requested.
        
- **Treating an indefinite integral as a number:**
    
    - _Why it's wrong:_ An indefinite integral $\int f(x) dx$ resolves to a _family of functions_ ($F(x)+C$), not a single numerical value. (Later, _definite_ integrals with limits will resolve to numbers representing area).
        

## 12. Deeper Conceptual Questions

**Why does $+C$ appear?**

Because differentiation calculates a rate of change, it ignores initial starting amounts. The derivative of a constant is zero. Therefore, when we go backward to recover the function from its rate of change, we have no way of knowing what that starting amount was. We use $+C$ to acknowledge this missing information.

**Why can two different functions have exactly the same derivative?**

If $F(x) = x^2$ and $G(x) = x^2 + 10$, their graphs are identical in shape; $G(x)$ is just shifted 10 units higher on the y-axis. Because they have the exact same shape, the steepness of their tangent lines at any $x$ is identical. Hence, their derivatives are the same.

**Why is $1/x$ different from other powers?**

The power rule generates $\frac{x^{n+1}}{n+1}$. As $n$ approaches $-1$, the denominator approaches $0$, causing the function to blow up to infinity. This signals a fundamental change in the behavior of the antiderivative, bridging the gap from rational functions into the realm of transcendental functions (the natural logarithm).

**How can I check whether an antiderivative is correct?**

Take the derivative of your answer. If you apply your differentiation rules correctly to your answer, you should get the original integrand perfectly. If you don't, your antiderivative is wrong.

## 13. Summary of Important Formulas

|**Integrand f(x)**|**Indefinite Integral ∫f(x)dx**|**When / Why it's used**|
|---|---|---|
|$x^n$ ($n \neq -1$)|$\frac{x^{n+1}}{n+1} + C$|The Power Rule (reverse of bringing the power down).|
|$\frac{1}{x}$|$\ln\Vert{}x\Vert{} + C$|The special case where $n = -1$ to avoid dividing by $0$.|
|$c$ (constant)|$cx + C$|Integrating a constant produces a linear function.|
|$e^x$|$e^x + C$|The exponential function is its own derivative and antiderivative.|
|$\cos x$|$\sin x + C$|Because $\frac{d}{dx}(\sin x) = \cos x$.|
|$\sin x$|$-\cos x + C$|Because $\frac{d}{dx}(\cos x) = -\sin x$, the negative is needed.|
|$c \cdot f(x)$|$c \int f(x)\,dx$|Constants can be pulled out of integrals.|
|$f(x) \pm g(x)$|$\int f(x)\,dx \pm \int g(x)\,dx$|Integrals can be split over addition and subtraction.|

## 14. Practice Problems

**Basic**

1. Evaluate $\int x^5 \, dx$.
    
2. Evaluate $\int 4 \, dx$.
    
3. Evaluate $\int \left(3x^2 + 2x\right) \, dx$.
    

**Intermediate**

4. Evaluate $\int \left(\frac{4}{x^3} - \frac{2}{x}\right) \, dx$.

5. Evaluate $\int \left(5e^x - 3\sin x\right) \, dx$.

6. Evaluate $\int \frac{x^2 + 1}{x} \, dx$. _(Hint: Simplify the fraction first)_.

**Challenging**

7. Find the specific function $F(x)$ given that $F'(x) = 2\cos x - 3$ and $F(0) = 4$.

8. A particle's velocity (rate of change of position) is given by $v(t) = \sqrt{t} + 2$. If the particle's initial position at $t=0$ is $s(0) = 5$, what is its position function $s(t)$?

### Answer Key & Solution Outlines

1. **$\frac{x^6}{6} + C$** (Power rule: add 1 to exponent, divide by 6).
    
2. **$4x + C$** (Antiderivative of a constant).
    
3. **$x^3 + x^2 + C$** (Power rule on each term: $3(x^3/3) + 2(x^2/2)$).
    
4. **$-\frac{2}{x^2} - 2\ln\vert{}x\vert{} + C$** (Rewrite as $4x^{-3} - 2/x$. Power rule gives $4x^{-2}/-2 = -2x^{-2}$. The second term is $-2\ln\vert{}x\vert{}$).
    
5. **$5e^x + 3\cos x + C$** (Antiderivative of $-\sin x$ is $\cos x$).
    
6. **$\frac{x^2}{2} + \ln\vert{}x\vert{} + C$** (Divide both terms by $x$ to get $\int (x + \frac{1}{x}) dx$. Then integrate).
    
7. **$F(x) = 2\sin x - 3x + 4$** (Integrate to get $F(x) = 2\sin x - 3x + C$. Substitute $F(0)=4 \Rightarrow 4 = 2(0) - 0 + C \Rightarrow C = 4$).
    
8. **$s(t) = \frac{2}{3}t^{3/2} + 2t + 5$** (Write $\sqrt{t}$ as $t^{1/2}$. Integrate: $s(t) = \frac{t^{3/2}}{3/2} + 2t + C = \frac{2}{3}t^{3/2} + 2t + C$. Substitute $t=0, s=5$ to find $C=5$).
    

## 15. Key Takeaways

- **Antidifferentiation is reverse differentiation:** It is the process of finding the original function $F(x)$ when given its rate of change $f(x)$.
    
- **The $+C$ is mandatory:** Because the derivative of any constant is zero, we lose information about vertical shifts when taking a derivative. We must include $+C$ to represent all possible original functions.
    
- **Indefinite Integral Notation:** The symbol $\int f(x) dx$ represents the entire family of antiderivatives of $f(x)$.
    
- **Power Rule:** $\int x^n dx = \frac{x^{n+1}}{n+1} + C$, which works for all powers _except_ $n = -1$.
    
- **The Logarithmic Exception:** When $n = -1$, the integral is $\int \frac{1}{x} dx = \ln\vert{}x\vert{} + C$. The absolute value ensures the domain is correct.
    
- **Trigonometric Traps:** Pay close attention to signs. $\int \sin x dx = -\cos x + C$, whereas $\int \cos x dx = \sin x + C$.
    
- **Initial Conditions resolve $+C$:** By plugging a known $(x,y)$ point into the general antiderivative, you can solve for $C$ and find one specific function.
    
- **Always check your work:** You can verify any antiderivative with 100% certainty by taking its derivative. If you don't get the original integrand back, try again.
    
- **Geometric connection:** The given function $f(x)$ describes the _slope_ of the antiderivative $F(x)$. Where $f(x)$ is positive, $F(x)$ is rising. Where $f(x)$ is zero, $F(x)$ flattens out.

[[Lecture9_Linear_&_Quadratic_approximations]]