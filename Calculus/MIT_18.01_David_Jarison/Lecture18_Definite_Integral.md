
### 1. Motivation: Why Do We Need Definite Integrals?

Geometry provides us with straightforward formulas to find the areas of standard shapes. We know that the area of a rectangle is $A = bh$, a triangle is $A = \frac{1}{2}bh$, and a circle is $A = \pi r^2$. These shapes all have one thing in common: their boundaries are either straight lines or simple, highly symmetric curves.

But what happens when we need to find the area under a more complex, general curved function?

Imagine trying to find the area under the curve $y = x^2$ between $x = 0$ and $x = 2$. Bounded by the $x$-axis on the bottom, the vertical line $x = 2$ on the right, and the parabola $y = x^2$ on top, this shape is not a polygon, nor is it a circle. Elementary geometry simply does not have a formula for this.

**The basic idea:** If we cannot calculate the exact area directly, we can _approximate_ it using shapes we do understand. We can slice the region into many thin, vertical rectangles. By calculating the area of each rectangle ($base \times height$) and adding them all up, we get an approximation of the total area.

Because the top of the curve is slanted or curved, the flat tops of the rectangles will either overshoot or undershoot the true curve. Therefore, a finite number of rectangles only gives an approximation, not the exact answer. To get a better approximation, we just need to make the rectangles thinner and use more of them.

### 2. Riemann-Sum Idea

Let us carefully develop this rectangle approximation, which is known mathematically as a **Riemann sum**.

Suppose we want to find the area under a function $f(x)$ from $x = a$ to $x = b$. We divide the interval $[a, b]$ into $n$ smaller subintervals (the bases of our rectangles).

If we make all the rectangles the same width, the width of each rectangle is:

$$\Delta x = \frac{b - a}{n}$$

For each interval, we need to decide how tall the rectangle should be. We pick a specific sample point $x_i^*$ inside the $i$-th interval. We plug this point into our function to find the height: $h = f(x_i^*)$.

The approximate total area is the sum of the areas of all $n$ rectangles:

$$\text{Approximate Area} = \sum_{i=1}^{n} f(x_i^*) \Delta x$$

Let us break down every single component of this expression so it is not just a meaningless string of symbols:

- $n$: The total number of rectangles we are using.
    
- $\Delta x$: The width of one individual rectangle.
    
- $x_i^*$: The specific $x$-coordinate we choose within the $i$-th rectangle to evaluate the height. (This could be the left edge, right edge, or midpoint of the base).
    
- $f(x_i^*)$: The height of the $i$-th rectangle, determined by the function's value at $x_i^*$.
    
- $f(x_i^*)\Delta x$: The area of the $i$-th single rectangle ($\text{height} \times \text{width}$).
    
- $\sum_{i=1}^{n}$: The summation symbol (capital Greek letter Sigma). It tells us to add up the areas of all the rectangles from the first one ($i=1$) to the last one ($n$).
    

Geometrically, this formula is simply a set of instructions: "Calculate $height \times width$ for the first rectangle, do it for the second, do it for the third... all the way to the $n$-th rectangle, and add those numbers together."

### 3. From Approximation to Exact Area

An approximation is good, but calculus is about finding the exact answer.

What happens to our approximation if we use more and more rectangles? As the number of rectangles $n$ gets larger, they must become thinner to fit within the same interval $[a, b]$. Thus, as $n \to \infty$ (number of rectangles goes to infinity), $\Delta x \to 0$ (the width of each rectangle shrinks to zero).

As the rectangles get thinner, the jagged, stepped top edge of our rectangles fits the smooth curve of the function more and more tightly. The overshoots and undershoots shrink away.

We do not literally draw or add up an infinite number of physical rectangles—that is impossible. Instead, we use the mathematical idea of a **limit**. We determine the value that the sum approaches as $n$ grows infinitely large.

This limit is the formal definition of the exact area, which we call the **definite integral**:

$$\boxed{ \int_a^b f(x)\,dx = \lim_{n\to\infty} \sum_{i=1}^{n}f(x_i^*)\Delta x }$$

Because the limit eliminates the "error" (the gaps and overlaps of the rectangles), it yields an absolutely exact, perfect value for the area under the curve.

### 4. Why Is It Called an Integral?

The integral symbol $\int$ looks like a stretched-out letter "S". This is highly intentional. It was introduced by Gottfried Wilhelm Leibniz, one of the founders of calculus, and it stands for the Latin word _Summa_ (sum).

The difference between $\sum$ and $\int$ is conceptually profound:

- $\sum$ represents a **discrete, finite sum**. You are adding up distinct, separate chunks (like 10 or 100 rectangles).
    
- $\int$ represents a **continuous accumulation**. It is the theoretical result of adding together an infinite number of infinitely small contributions.
    

When you see the integral sign $\int_a^b$, your brain should translate it to: _"The continuous accumulation of..."_ from starting point $a$ to ending point $b$.

### 5. What Does $dx$ Actually Mean?

In the notation $\int_a^b f(x)\,dx$, the $dx$ is often treated by beginners as a mere punctuation mark indicating the end of the integral. This is a mistake. The $dx$ has a deep geometric and algebraic meaning.

Look back at the Riemann sum limit:

$$\lim_{n\to\infty} \sum_{i=1}^{n} f(x_i^*) \Delta x$$

As the limit is applied, the notation transforms:

- The sum $\sum$ transforms into the integral sign $\int$.
    
- The sample height $f(x_i^*)$ becomes the continuous function $f(x)$.
    
- **The finite width $\Delta x$ transforms into the infinitesimal width $dx$.**
    

Therefore, $dx$ represents an infinitely small slice of the $x$-axis. Just as $f(x_i^*)\Delta x$ is $height \times width$ for a finite rectangle, $f(x)\,dx$ conceptually represents $height \times width$ for an infinitely thin "rectangle."

Furthermore, $dx$ tells us the variable of integration. It specifies that we are slicing along the $x$-axis. If we had $\int f(t)\,dt$, it would mean we are slicing along the $t$-axis (usually representing time).

### 6. Definite Integral vs Indefinite Integral

It is crucial to distinguish between two concepts that share similar notation:

**1. The Indefinite Integral:**

$$\int f(x)\,dx = F(x) + C$$

- **What it is:** A family of functions. It asks the question, "What functions have $f(x)$ as their derivative?"
    
- **Why $+C$?** Because the derivative of a constant is zero, any two antiderivatives differ by a constant. The $+C$ represents this entire family.
    

**2. The Definite Integral:**

$$\int_a^b f(x)\,dx = \text{A Number}$$

- **What it is:** A specific numerical value representing accumulated area (or accumulation of a quantity) between limits $a$ and $b$.
    
- **Why no $+C$?** Because a definite integral computes a specific, measurable difference (as we will see with the Fundamental Theorem). The $+C$ cancels out when we evaluate the difference between the endpoints.
    

### 7. Signed Area

Until now, we have assumed $f(x)$ is positive (the graph is above the $x$-axis). If $f(x) \ge 0$, the definite integral gives the exact, ordinary geometric area.

But what if the graph dips below the $x$-axis? If $f(x) < 0$, then the "height" of our rectangles $f(x)$ is a negative number, while the width $dx$ is still positive. Therefore, $f(x)\,dx$ produces a negative value.

Because of this, the definite integral computes **signed area** (or net area):

$$\boxed{ \text{Signed Area} = (\text{Area above } x\text{-axis}) - (\text{Area below } x\text{-axis}) }$$

**Example:** Think about walking. If $v(t)$ is your velocity, walking forward means $v(t) > 0$ and walking backward means $v(t) < 0$. The integral of velocity gives your _net displacement_ (how far you are from where you started). If you walk 5 meters forward (area above = 5) and 3 meters backward (area below = 3), your net displacement is 2 meters. This convention is incredibly useful for physics and economics, where negative accumulation means losing a quantity (like losing money, or moving backward).

### 8. Basic Properties of Definite Integrals

Because the definite integral is fundamentally a limit of sums, it inherits several logical properties.

**1. Same endpoints**

$$\int_a^a f(x)\,dx = 0$$

- **Why?** Geometrically, the interval $[a, a]$ has a width of 0. A region with zero width has zero area.
    

**2. Reversing the limits**

$$\boxed{ \int_b^a f(x)\,dx = -\int_a^b f(x)\,dx }$$

- **Why?** If we integrate from $b$ to $a$ (where $b > a$), we are moving backwards along the $x$-axis. This makes our step size $\Delta x$ negative. A positive height multiplied by a negative width gives a negative area. Reversing direction reverses the sign.
    

**3. Splitting an interval**

$$\boxed{ \int_a^b f(x)\,dx = \int_a^c f(x)\,dx + \int_c^b f(x)\,dx }$$

- **Why?** This is simple geometric addition. The area from $a$ to $b$ is the area from $a$ to $c$ plus the area from $c$ to $b$. You are just slicing the region into two large pieces and adding them together.
    

**4. Constant multiple**

$$\int_a^b cf(x)\,dx = c\int_a^b f(x)\,dx$$

- **Why?** If you stretch a function vertically by a factor of $c$, you stretch the height of every single rectangle by $c$. You can factor this $c$ out of the sum.
    

**5. Sum of functions**

$$\int_a^b (f(x) + g(x))\,dx = \int_a^b f(x)\,dx + \int_a^b g(x)\,dx$$

- **Why?** The area under the sum of two curves is exactly the sum of their individual areas. In the Riemann sum, $(f(x) + g(x))\Delta x = f(x)\Delta x + g(x)\Delta x$.
    

### 9. Connection to Antiderivatives

Calculating limits of Riemann sums by hand is mathematically brutal. Fortunately, there is a shortcut that connects area to antiderivatives.

Suppose $F(x)$ is an antiderivative of $f(x)$, meaning $F'(x) = f(x)$.

The **Fundamental Theorem of Calculus (Part 2)** tells us that we can evaluate the definite integral using this antiderivative:

$$\boxed{ \int_a^b f(x)\,dx = F(b) - F(a) }$$

_(Often written as $[F(x)]_a^b$ or $F(x) \Big\vert{}_a^b$)_

This is not just a trick to memorize; it is a profound connection. Look at the conceptual chain we have built:

1. **Tiny rectangle sums:** We want to find area by adding up many $f(x)\Delta x$.
    
2. **Definite integral:** We take the limit to get exactly $\int_a^b f(x)\,dx$.
    
3. **Accumulation function:** We recognize this integral represents a running total of area.
    
4. **Antiderivative:** We discover (as proved in the next section) that the rate of change of area is the function itself, linking area to the antiderivative $F$.
    
5. **$F(b) - F(a)$:** We use the antiderivative to compute the exact total accumulation without ever physically drawing a rectangle.
    

### 10. Why Does $F(b) - F(a)$ Give the Same Result as the Rectangle Sum?

Why does this magical theorem work? Let us construct the reasoning.

Imagine an "area accumulator" function, $A(x)$, which represents the accumulated area under $f(t)$ from a fixed starting point $a$ up to a variable point $x$:

$$A(x) = \int_a^x f(t)\,dt$$

_(We use $t$ as a dummy variable inside the integral so we don't confuse it with the upper limit $x$.)_

What happens if we move $x$ a tiny bit to the right, by an amount $h$? The new area $A(x+h)$ is the old area $A(x)$ plus a new thin sliver of area.

The area of that new sliver is approximately a rectangle of height $f(x)$ and width $h$.

$$A(x+h) - A(x) \approx f(x) \cdot h$$

Divide by $h$:

$$\frac{A(x+h) - A(x)}{h} \approx f(x)$$

Take the limit as $h \to 0$. The left side is precisely the definition of the derivative!

$$A'(x) = f(x)$$

This means that **the rate at which area accumulates is exactly equal to the height of the curve**. Because $A'(x) = f(x)$, $A(x)$ must be an antiderivative of $f(x)$.

Let $F(x)$ be any general antiderivative of $f(x)$. Since $A(x)$ and $F(x)$ have the same derivative, they can only differ by a constant $C$:

$$A(x) = F(x) + C$$

To find $C$, what is $A(a)$? The area from $a$ to $a$ is 0. So:

$$A(a) = 0 \implies F(a) + C = 0 \implies C = -F(a)$$

Substitute this back:

$$A(x) = F(x) - F(a)$$

Finally, to find the total area from $a$ to $b$, we want $A(b)$. Setting $x=b$ gives:

$$A(b) = F(b) - F(a)$$

This proves why evaluating the antiderivative at the endpoints gives the exact same result as the limit of the infinite rectangle sum!

### 11. Worked Example: Area Under a Curve

Let's return to our motivation problem: Find the area under $f(x) = x^2$ from $x=0$ to $x=2$.

**1. The Rectangle Interpretation:**

We are looking for $\int_0^2 x^2 \,dx$, which is the limit of adding up infinitely many rectangles of height $(x_i^*)^2$ and width $dx$.

**2. Evaluating using the Antiderivative:**

We need an antiderivative of $x^2$. Using the power rule backwards:

$$F(x) = \frac{x^3}{3}$$

_(We don't need $+C$ because it will cancel out)._

Now, apply the Fundamental Theorem:

$$\int_0^2 x^2 \,dx = F(2) - F(0)$$

$$= \left( \frac{2^3}{3} \right) - \left( \frac{0^3}{3} \right)$$

$$= \frac{8}{3} - 0 = \frac{8}{3}$$

The exact area is $\frac{8}{3}$ (or about $2.667$). The rectangle approximations, if carried out to an infinite limit, converge perfectly to this rational number.

### 12. Worked Example: Function Below the $x$-axis

Let $f(x) = x - 2$ on the interval $[0, 1]$.

Notice that on this interval, the $y$-values are negative (e.g., at $x=0$, $y=-2$).

Calculate the integral:

$$\int_0^1 (x - 2) \,dx$$

Antiderivative $F(x) = \frac{1}{2}x^2 - 2x$. Evaluate from 0 to 1:

$$= \left( \frac{1}{2}(1)^2 - 2(1) \right) - \left( \frac{1}{2}(0)^2 - 2(0) \right)$$

$$= \left( \frac{1}{2} - 2 \right) - (0) = -1.5$$

Why is the answer negative? Because the integral measures **signed area**, and this entire region sits below the $x$-axis.

If a geometry problem asks for the **ordinary geometric area** between the curve and the $x$-axis, area cannot be negative. You must take the absolute value: $\vert{}-1.5\vert{} = 1.5$.

### 13. Worked Example: Graph Crossing the $x$-axis

Let $f(x) = 2x$ on the interval $[-2, 2]$.

Evaluate the integral:

$$\int_{-2}^2 2x \,dx = x^2 \Big\vert{}_{-2}^2 = (2)^2 - (-2)^2 = 4 - 4 = 0$$

The definite integral is 0. But clearly, there is geometric space between the graph and the $x$-axis! The graph is a line passing through the origin. From $x=-2$ to $0$, it forms a triangle below the axis (area = 4, so signed contribution is -4). From $x=0$ to $2$, it forms a triangle above the axis (area = 4, signed contribution +4).

$-4 + 4 = 0$.

If you want the **total geometric area**, you must split the integral where the function crosses the axis (at $x=0$):

$$\text{Total Area} = \left\vert{} \int_{-2}^0 2x\,dx \right\vert{} + \left\vert{} \int_0^2 2x\,dx \right\vert{}$$

$$= \vert{}-4\vert{} + \vert{}4\vert{} = 4 + 4 = 8$$

### 14. Accumulation Functions

As seen in the FTC explanation, the function

$$A(x) = \int_a^x f(t)\,dt$$

is an accumulation function. As $x$ moves to the right, $A(x)$ accumulates more "area" from the curve $f(t)$.

Because $A'(x) = f(x)$, we can read the behavior of the accumulation function directly from the original graph:

- If $f(x) > 0$, then $A'(x) > 0$. This means $A(x)$ is **increasing** (accumulating positive area).
    
- If $f(x) < 0$, then $A'(x) < 0$. This means $A(x)$ is **decreasing** (accumulating negative area, eating away at the total).
    
- If $f(x) = 0$, then $A'(x) = 0$. $A(x)$ has a local maximum, minimum, or plateau (it has temporarily stopped accumulating).
    

This perfectly connects integration back to your previous knowledge of first derivatives and increasing/decreasing functions.

### 15. Units and Physical Interpretation

Integration makes perfect dimensional sense. Recall that $f(x)\,dx$ is a multiplication. Therefore, the units of the integral are the units of $f(x)$ multiplied by the units of $x$.

For example, suppose $v(t)$ is the velocity of a car in meters per second (m/s), and $t$ is time in seconds (s).

- $v(t)$ has units of m/s.
    
- $dt$ has units of s.
    
- The product $v(t)\,dt$ has units of $\frac{\text{m}}{\text{s}} \cdot \text{s} = \text{m}$ (meters).
    

So, $\int v(t)\,dt$ calculates total meters traveled (distance/displacement). Integration is fundamentally an accumulation operation.

Other examples:

- **Flow rate (gallons/minute)** integrated over **time (minutes)** $\to$ Volume accumulated (gallons).
    
- **Power (Joules/second)** integrated over **time (seconds)** $\to$ Total energy used (Joules).
    
- **Linear Density (kg/meter)** integrated over **length (meters)** $\to$ Total mass of a rod (kg).
    

### 16. Common Misconceptions

- **"The integral is always the area above the $x$-axis."**
    
    - _Wrong._ The integral calculates _signed_ area. Areas below the axis are treated as negative. To get purely positive geometric area, you must integrate the absolute value of the function.
        
- **"A definite integral needs $+C$."**
    
    - _Wrong._ The $+C$ represents an unknown starting value for a family of antiderivatives. In a definite integral, $F(b) - F(a)$ subtracts the $+C$ out entirely. The answer is just a number.
        
- **"The bounds are just numbers you substitute into the original function."**
    
    - _Wrong._ You substitute the bounds into the _antiderivative_ $F(x)$, not the original function $f(x)$.
        
- **"$dx$ is meaningless notation."**
    
    - _Wrong._ It represents the infinitesimal width of the rectangles, derived from $\Delta x$. It is essential for knowing what variable you are multiplying by and accumulating over.
        
- **"An integral and an antiderivative are exactly the same thing."**
    
    - _Wrong._ An antiderivative is a function. A definite integral is a number (the limit of a Riemann sum). The Fundamental Theorem simply reveals that we can _use_ an antiderivative to _calculate_ a definite integral.
        
- **"The integral is defined only because we know the antiderivative."**
    
    - _Wrong._ The integral is defined completely independently as the limit of a Riemann sum of rectangles. Many functions (like $e^{-x^2}$) have definite integrals that represent perfect areas, even though we cannot write down a simple algebraic formula for their antiderivative.
        
- **"More rectangles means the answer is automatically exact."**
    
    - _Wrong._ More rectangles means a _better approximation_. It only becomes exact at the limit as $n \to \infty$.
        
- **"If the integral is negative, the area is negative."**
    
    - _Wrong._ "Area" geometrically is always positive. A negative integral just means the region lies predominantly _below_ the $x$-axis.
        

### 17. Deep Conceptual Questions

**Why do thin rectangles approximate a curved region?**

Because locally, over a very tiny width, any continuous curve looks relatively flat. A flat-topped rectangle is a good match for a very short segment of the curve.

**Why do the rectangles become exact in the limit?**

As the width approaches zero, the discrepancy between the flat top of the rectangle and the slanted curve approaches zero faster than the area itself. The error vanishes.

**Why does $dx$ appear?**

It is the continuous, infinitesimal counterpart to the discrete, finite width $\Delta x$. It completes the $height \times width$ formula in the continuous domain.

**Why does reversing the bounds change the sign?**

Integrating from $b$ to $a$ (where $b > a$) means slicing the interval backwards. The width of our slices, $dx$, becomes negative. $height \times (-width) = -area$.

**Why does splitting an integral work?**

Because area is additive. The space from $x=0$ to $x=5$ is just the space from $x=0$ to $x=2$ plus the space from $x=2$ to $x=5$.

**Why does the definite integral give a number while the indefinite integral gives a function?**

The definite integral adds up specific numerical slivers between two fixed points. The indefinite integral asks a general algebraic question: "what function's derivative is this?"

**Why can we calculate a rectangle-based definition using an antiderivative?**

Because the rate at which area accumulates under a curve ($A'(x)$) is exactly equal to the height of the curve ($f(x)$). Since the area's derivative is the function, the area must be the function's antiderivative.

**What is the difference between "area" and "accumulation"?**

Area is purely geometric (always positive). Accumulation is algebraic/physical; it can go up or down (like adding money to a bank account and then withdrawing it). The integral strictly does accumulation (signed area).

### 18. Connections to Previous and Future Topics

This lecture connects the two halves of calculus.

- Earlier, you learned that the derivative finds the instantaneous rate of change (tangent line slope).
    
- Now, you see the integral finds the accumulation of a quantity (area under the curve).
    

These seem like totally different geometric problems (slopes vs. areas). The brilliance of calculus is realizing they are opposites:

$$\boxed{ \text{Derivative} = \text{instantaneous rate} }$$

$$\boxed{ \text{Integral} = \text{accumulation of a rate} }$$

$$\boxed{ \text{Fundamental Theorem} = \text{these two operations are inverses} }$$

In future lectures, integration will be used to find volumes of 3D solids, lengths of curves, centers of mass, and work done in physics—all by setting up an integral as a limit of a sum of tiny pieces.

### 19. Formula Sheet

|**Concept**|**Formula**|**Explanation**|
|---|---|---|
|**Riemann Sum**|$\sum_{i=1}^{n} f(x_i^*) \Delta x$|Approximation of area using $n$ rectangles of width $\Delta x$ and height $f(x_i^*)$.|
|**Definite Integral (Limit Definition)**|$\int_a^b f(x)\,dx = \lim_{n\to\infty} \sum_{i=1}^{n}f(x_i^*)\Delta x$|The exact signed area, found by taking the limit as the number of rectangles goes to infinity.|
|**Fundamental Theorem of Calculus (FTC 2)**|$\int_a^b f(x)\,dx = F(b) - F(a)$|Computes the definite integral by subtracting the antiderivative $F$ evaluated at the bounds.|
|**Reverse Bounds Property**|$\int_b^a f(x)\,dx = -\int_a^b f(x)\,dx$|Integrating backward makes $dx$ negative, flipping the sign of the area.|
|**Splitting Property**|$\int_a^b f(x)\,dx = \int_a^c f(x)\,dx + \int_c^b f(x)\,dx$|The total area is the sum of adjacent sub-areas.|
|**Accumulation Function**|$A(x) = \int_a^x f(t)\,dt$|A function representing the area accumulated under $f(t)$ from a fixed start $a$ up to variable $x$.|
|**FTC 1**|$A'(x) = \frac{d}{dx} \int_a^x f(t)\,dt = f(x)$|The rate of change of the accumulated area is the height of the curve itself.|

### 20. Practice Problems

**Basic**

1. Evaluate the definite integral: $\int_1^3 (3x^2) \,dx$.
    
2. Suppose $\int_0^5 f(x)\,dx = 10$. What is $\int_5^0 f(x)\,dx$?
    
3. Translate the following limit into a definite integral: $\lim_{n \to \infty} \sum_{i=1}^{n} \cos(x_i^*) \Delta x$ on the interval $[0, \pi]$.
    

**Intermediate**

4. Evaluate $\int_0^\pi \sin(x) \,dx$. Does this represent the geometric area between the curve and the $x$-axis? Why?

5. Find the total geometric area bounded by $y = x^3$, the $x$-axis, $x = -1$, and $x = 1$. (Hint: Be careful about signed area!)

6. Using properties of integrals, if $\int_2^8 f(x)\,dx = 15$ and $\int_5^8 f(x)\,dx = 6$, find $\int_2^5 f(x)\,dx$.

**Challenging**

7. Let $v(t) = t^2 - 4t + 3$ be the velocity of a particle in m/s. Find the net displacement from $t=0$ to $t=4$, and then find the _total distance traveled_ from $t=0$ to $t=4$. (Note the difference in definitions).

8. Use the Fundamental Theorem of Calculus to find the derivative of $g(x) = \int_2^x \sqrt{1+t^3}\,dt$.

#### Answer Key (Outlines)

1. Antiderivative $F(x) = x^3$. $F(3) - F(1) = 27 - 1 = 26$.
    
2. By the reverse bounds property, it is $-10$.
    
3. $\int_0^\pi \cos(x) \,dx$.
    
4. $F(x) = -\cos(x)$. $F(\pi) - F(0) = (-\cos(\pi)) - (-\cos(0)) = 1 - (-1) = 2$. Yes, it represents geometric area because $\sin(x) \ge 0$ on $[0, \pi]$.
    
5. Curve crosses $x$-axis at 0. Split integral: $\int_{-1}^0 x^3 dx = -1/4$. $\int_0^1 x^3 dx = 1/4$. The total geometric area is $\vert{}-1/4\vert{} + \vert{}1/4\vert{} = 1/2$.
    
6. $\int_2^8 = \int_2^5 + \int_5^8 \implies 15 = \int_2^5 + 6 \implies \int_2^5 f(x)\,dx = 9$.
    
7. Displacement = $\int_0^4 (t^2 - 4t + 3) \,dt = [t^3/3 - 2t^2 + 3t]_0^4 = 4/3$.
    
    Distance = total area. Curve crosses axis at $t=1$ and $t=3$. Calculate areas of $[0,1]$, $[1,3]$, and $[3,4]$, take absolute values, and add. Distance = 4.
    
8. By FTC Part 1, the derivative of the accumulation function is the inside function evaluated at $x$. $g'(x) = \sqrt{1+x^3}$.
    

### 21. Final Summary: Key Takeaways

1. A definite integral calculates the accumulation of a quantity, typically visualized as the area under a curve.
    
2. The definite integral is fundamentally the limit of adding many tiny contributions (Riemann sums).
    
3. Geometrically, those tiny contributions can be viewed as the areas of very thin rectangles: $f(x_i^*)\Delta x$.
    
4. As the number of rectangles goes to infinity ($n \to \infty$), the width of each approaches zero ($\Delta x \to 0$), and the approximation becomes exact.
    
5. The notation $\int$ acts as a continuous sum, and $dx$ represents the infinitesimal width.
    
6. Unlike an indefinite integral (which gives a family of functions $+C$), a definite integral evaluates to a specific number.
    
7. Definite integrals calculate **signed area**; area below the $x$-axis counts as negative accumulation.
    
8. To find purely geometric total area, you must find where the function crosses the $x$-axis and split the integral, taking absolute values of the negative portions.
    
9. Integrals are highly logical: you can split them over intervals, reverse the limits (which flips the sign), and factor out constants.
    
10. The dimensional units of an integral are the units of the $y$-axis multiplied by the units of the $x$-axis (e.g., velocity $\times$ time = distance).
    
11. An accumulation function $A(x)$ tracks the running total of accumulated area. Its derivative is the curve itself: $A'(x) = f(x)$.
    
12. **The Fundamental Theorem of Calculus tells us that this accumulation can be computed using an antiderivative, which is why evaluating $F(b) - F(a)$ gives the exact same result as the limiting rectangle sum.**