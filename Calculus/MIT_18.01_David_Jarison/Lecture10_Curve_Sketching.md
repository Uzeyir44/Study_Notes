
## 1. The Purpose of Curve Sketching

**What curve sketching means mathematically**

Curve sketching is the process of using algebraic analysis and calculus to determine the exact global and local behavior of a function, allowing us to draw its graph accurately without relying on software. It is the geometric realization of a function's analytical properties.

**Why graphing is not simply plotting points**

If you only calculate $y$-values for a few integer $x$-values and connect the dots, you are guessing the behavior between those points. The function might contain violent oscillations, hidden asymptotes, or sharp corners between $x=1$ and $x=2$ that a simple table of values will miss. Calculus removes the guesswork by telling us exactly what happens everywhere in the domain.

**The distribution of information**

To reconstruct a graph's shape, we interrogate the function at three different levels, each providing a unique layer of geometric truth:

1. **$f(x)$ (The Position)**: Tells us _where_ the graph is. It answers: Where is the function positive or negative? Where does it cross the axes ($x$- and $y$-intercepts)? What happens at the boundaries of the domain or as $x \to \pm\infty$ (asymptotes)?
    
2. **$f'(x)$ (The Direction)**: Tells us _where the graph is going_. It answers: Where is the function increasing (rising) or decreasing (falling)? Where are the local maxima (peaks) and minima (valleys)?
    
3. **$f''(x)$ (The Curvature)**: Tells us _how the direction is changing_. It answers: Where is the graph concave up (shaped like a cup) or concave down (shaped like a frown)? Where do the inflection points (where curvature flips) occur?
    

## 2. A Systematic Curve-Sketching Procedure

To avoid missing critical features, we use a logical hierarchy. We start with algebraic foundations before applying calculus.

1. **Determine the domain:** This is the most crucial step. You cannot evaluate derivatives or look for points where the function does not exist. The domain constraints every subsequent conclusion.
    
2. **Find the $x$- and $y$-intercepts:** Set $y=0$ to find $x$-intercepts (roots). Set $x=0$ to find the $y$-intercept.
    
3. **Check symmetry:** Is $f(-x) = f(x)$ (even, symmetric across $y$-axis) or $f(-x) = -f(x)$ (odd, symmetric across origin)? This halves your workload.
    
4. **Examine discontinuities and asymptotes:** Look for division by zero (vertical asymptotes) or limits as $x \to \pm\infty$ (horizontal asymptotes).
    
5. **Study end behavior:** What is $\lim_{x\to\infty} f(x)$ and $\lim_{x\to-\infty} f(x)$?
    
6. **Calculate the first derivative ($f'(x)$) and identify critical numbers:** Find where $f'(x) = 0$ or $f'(x)$ is undefined.
    
7. **Determine intervals of increase and decrease:** Build a sign chart for $f'(x)$ using test values between critical numbers.
    
8. **Classify local maxima and minima:** Apply the First Derivative Test.
    
9. **Calculate the second derivative ($f''(x)$):** Find where $f''(x) = 0$ or is undefined.
    
10. **Determine intervals of concavity:** Build a sign chart for $f''(x)$.
    
11. **Identify and verify possible inflection points:** Check if $f''(x)$ actually changes sign at the candidates found in step 9.
    
12. **Combine all findings into a coherent sketch:** Plot the key points (intercepts, extrema, inflection points) and connect them respecting the increasing/decreasing and concavity behaviors.
    

**Adaptability of the procedure:**

Do not treat this as a rigid checklist. For a polynomial, you can skip asymptotes entirely (step 4). For a rational function, finding $x$-intercepts of a high-degree numerator might be impossible by hand, so you might rely heavier on derivatives.

## 3. First Derivative and the Shape of the Graph

**Intuition and Mathematical Reasoning**

The derivative $f'(x)$ represents the slope of the tangent line to the curve at $x$.

- If a line has a positive slope, it rises from left to right. Therefore, if the tangent lines are constantly rising ($f'(x) > 0$), the function itself must be rising.
    
- Mathematically, this is guaranteed by the Mean Value Theorem. If $f'(x) > 0$ on an interval, then for any $a < b$, $f(b) - f(a) = f'(c)(b-a) > 0$, meaning $f(b) > f(a)$.
    

Thus:

$$f'(x) > 0 \implies f \text{ is increasing}$$

$$f'(x) < 0 \implies f \text{ is decreasing}$$

**Critical Numbers vs. Critical Points**

- A **critical number** is an $x$-value ($x = c$) in the domain of $f$ where either $f'(c) = 0$ (a horizontal tangent) or $f'(c)$ is undefined (a sharp corner, cusp, or vertical tangent).
    
- A **critical point** is the coordinate pair $(c, f(c))$ on the graph.
    
- _Crucial Distinction:_ You must have the $y$-value to plot a point.
    

**The First Derivative Test**

Not every critical number results in a local extremum. To classify them, we use a first-derivative sign chart to track how the slope changes _across_ the critical number $c$:

1. **$+$ to $-$**: The function rises, levels off, and falls. This is a **local maximum**.
    
2. **$-$ to $+$**: The function falls, levels off, and rises. This is a **local minimum**.
    
3. **No sign change ( $+$ to $+$ OR $-$ to $-$ )**: The function pauses but continues in the same direction. This is **neither** a maximum nor a minimum.
    

**Local vs. Absolute Extrema**

A _local_ maximum is the highest point in its immediate neighborhood. An _absolute_ (or global) maximum is the highest point on the entire domain. Absolute extrema often occur at the endpoints of a restricted domain (like $[a, b]$), which is why checking the domain boundary is essential.

## 4. Second Derivative and Concavity

**Geometric and Mathematical Meaning**

If $f'(x)$ tells us the slope, then the second derivative $f''(x)$ tells us _the rate of change of the slope_.

- If $f''(x) > 0$, then $f'(x)$ (the slope) is increasing. Geometrically, as you move left to right, the tangent lines rotate counterclockwise. The curve sits _above_ its tangent lines. This shape is **concave up** (like a cup).
    
- If $f''(x) < 0$, then $f'(x)$ is decreasing. The tangent lines rotate clockwise. The curve sits _below_ its tangent lines. This shape is **concave down** (like a frown).
    

**Sign Chart for $f''(x)$**

Just as we find critical numbers for $f'$ by setting $f'(x)=0$, we find candidate points for concavity changes by setting $f''(x) = 0$ or finding where $f''$ is undefined. We then test points between these candidates to see if $f''$ is positive or negative.

## 5. Inflection Points: Go Beyond the Memorized Rule

An **inflection point** is a point on the graph where the function is continuous and the concavity _changes_ (either from up to down, or down to up).

**Necessary vs. Sufficient Conditions**

- **Necessary:** For a smooth function, it is _necessary_ that $f''(c) = 0$ for $c$ to be an inflection point. (It can also happen where $f''(c)$ is undefined).
    
- **Sufficient:** $f''(c) = 0$ is _not sufficient_ to guarantee an inflection point. The concavity must actually change signs across $x = c$.
    

Let's look at specific examples to solidify this:

**Example A: A genuine inflection point**

$$f(x) = x^3 \implies f'(x) = 3x^2 \implies f''(x) = 6x$$

Set $f''(x) = 0 \implies 6x = 0 \implies x = 0$.

- Test $x = -1$: $f''(-1) = -6$ (Concave Down).
    
- Test $x = 1$: $f''(1) = 6$ (Concave Up).
    
    Because $f''$ changes sign, $(0,0)$ is a genuine inflection point.
    

**Example B: A critical point that is NOT an inflection point**

$$g(x) = x^4 \implies g'(x) = 4x^3 \implies g''(x) = 12x^2$$

Set $g''(x) = 0 \implies 12x^2 = 0 \implies x = 0$.

- Test $x = -1$: $g''(-1) = 12$ (Concave Up).
    
- Test $x = 1$: $g''(1) = 12$ (Concave Up).
    
    The concavity does _not_ change. The graph is always concave up. Therefore, $x=0$ is a local minimum, but it is **not** an inflection point.
    
    _(This is why the Second Derivative Test for extrema is inconclusive when $f''(c)=0$. It could be a minimum like $x^4$, a maximum like $-x^4$, or an inflection point like $x^3$.)_
    

**Example C: A stationary inflection point**

Let's revisit $h(x) = x^3$. We know $(0,0)$ is an inflection point. But notice $h'(0) = 3(0)^2 = 0$.

Because the first derivative is zero, the tangent line is completely horizontal at $x=0$. Yet, it is an inflection point, not a max or min. This is called a **stationary inflection point**. (Examples A and C use the same function, highlighting that $x=0$ is both a root of $f'$ and a root of $f''$).

**Detailed Analysis: $p(x) = x^5 - 5x^4$**

First derivatives:

$$p'(x) = 5x^4 - 20x^3 = 5x^3(x-4)$$

Critical numbers: $x = 0$ and $x = 4$.

Second derivative:

$$p''(x) = 20x^3 - 60x^2 = 20x^2(x-3)$$

Possible inflection points ($p''(x) = 0$): $x = 0$ and $x = 3$.

Let's look at the $p''$ sign chart across the intervals defined by $x=0$ and $x=3$:

- $x < 0$: $p''(-1) = 20(-1)^2(-1-3) = 20(1)(-4) = -80$ (Concave Down)
    
- $0 < x < 3$: $p''(1) = 20(1)^2(1-3) = 20(1)(-2) = -40$ (Concave Down)
    
- $x > 3$: $p''(4) = 20(4)^2(4-3) = 320(1) = 320$ (Concave Up)
    

Notice what happened at $x=0$. The factor $x^2$ in $p''(x) = 20x^2(x-3)$ is always positive. It acts as a "squared bounce" for the sign chart. The sign of $p''$ does not change across $x=0$, so $x=0$ is **not** an inflection point.

However, if we look at the first derivative $p'(x) = 5x^3(x-4)$, at $x=0$, the sign changes from positive (e.g., $p'(-1) = 5(-1)(-5) = +25$) to negative (e.g., $p'(1) = 5(1)(-3) = -15$).

Conclusion for $x=0$: It is a **local maximum** (and concave down the whole time).

Conclusion for $x=3$: It is an **inflection point** (changes from concave down to concave up).

## 6. How the First and Second Derivatives Work Together

The first derivative gives the direction; the second gives the "acceleration" or curvature of that movement.

|**f′(x)**|**f′′(x)**|**Behavior**|**Plain English Description**|
|---|---|---|---|
|$+$|$+$|Increasing, Concave Up|Rising and getting steeper (accelerating upward).|
|$+$|$-$|Increasing, Concave Down|Rising but leveling off (decelerating upward).|
|$-$|$+$|Decreasing, Concave Up|Falling but leveling off (decelerating downward).|
|$-$|$-$|Decreasing, Concave Down|Falling and getting steeper (accelerating downward).|

**Note:** A function can easily be decreasing while concave up (think of the left side of a parabola $y=x^2$). It is sliding down the bowl, but the slope is becoming _less negative_ (increasing mathematically), eventually hitting zero at the bottom.

## 7. Complete Worked Curve-Sketching Examples

### Example 1: A standard polynomial

**$f(x) = x^3 - 3x$**

1. **Domain**: $(-\infty, \infty)$ since it's a polynomial.
    
2. **Intercepts**:
    
    - $y$-int: $f(0) = 0 \implies (0,0)$.
        
    - $x$-int: $x^3 - 3x = 0 \implies x(x^2 - 3) = 0 \implies x = 0, \pm\sqrt{3}$.
        
3. **Symmetry**: $f(-x) = (-x)^3 - 3(-x) = -x^3 + 3x = -f(x)$. It is an **odd function** (symmetric about origin).
    
4. **First Derivative**: $f'(x) = 3x^2 - 3 = 3(x^2 - 1) = 3(x-1)(x+1)$.
    
5. **Critical Numbers**: $x = 1, -1$.
    
6. **$f'$ Sign Chart**:
    
    - $x < -1$: test $-2 \implies 3(-)(-)$ = $+$ (Inc)
        
    - $-1 < x < 1$: test $0 \implies 3(-)(+)$ = $-$ (Dec)
        
    - $x > 1$: test $2 \implies 3(+)(+)$ = $+$ (Inc)
        
7. **Local Extrema**:
    
    - Local max at $x = -1$. $y = f(-1) = 2 \implies (-1, 2)$.
        
    - Local min at $x = 1$. $y = f(1) = -2 \implies (1, -2)$.
        
8. **Second Derivative**: $f''(x) = 6x$.
    
9. **$f''$ Sign Chart**:
    
    - $x < 0$: test $-1 \implies -$ (Concave Down)
        
    - $x > 0$: test $1 \implies +$ (Concave Up)
        
10. **Inflection Points**: Concavity changes at $x=0$. $f(0) = 0$. IP is $(0,0)$.
    
11. **End Behavior**: $\lim_{x\to\infty} x^3-3x = \infty$, $\lim_{x\to-\infty} = -\infty$.
    
12. **Final Sketch Description**: The graph starts at $-\infty$, rises steeply (concave down) to a peak at $(-1,2)$, falls through the origin $(0,0)$ where it perfectly flips to concave up, hits a valley at $(1,-2)$, and then rises smoothly to $\infty$.
    

### Example 2: Stationary Inflection Point

**$f(x) = x^3 - 3x^2 + 3x$**

1. **Domain**: $(-\infty, \infty)$
    
2. **Intercepts**: $y$-int: $(0,0)$. $x$-int: $x(x^2-3x+3)=0 \implies x=0$ (quadratic has no real roots).
    
3. **First Derivative**: $f'(x) = 3x^2 - 6x + 3 = 3(x^2 - 2x + 1) = 3(x-1)^2$.
    
4. **Critical Numbers**: $x = 1$.
    
5. **$f'$ Sign Chart**:
    
    - $x < 1$: test $0 \implies 3(-1)^2 = +$ (Inc)
        
    - $x > 1$: test $2 \implies 3(1)^2 = +$ (Inc)
        
6. **Local Extrema**: No sign change. No local max or min!
    
7. **Second Derivative**: $f''(x) = 6x - 6 = 6(x-1)$.
    
8. **$f''$ Sign Chart**:
    
    - $x < 1$: test $0 \implies -$ (Concave Down)
        
    - $x > 1$: test $2 \implies +$ (Concave Up)
        
9. **Inflection Points**: Concavity changes at $x=1$. $f(1) = 1$. IP is $(1,1)$.
    
10. **Final Sketch Description**: The graph is always increasing. It rises from $-\infty$, bending downwards (concave down) until it reaches $(1,1)$. At exactly $(1,1)$, the tangent line goes completely horizontal for an instant, then the curve immediately starts bending upwards (concave up) and continues rising to $\infty$.
    

### Example 3: Rational function with asymptotes

**$f(x) = \frac{x}{x^2+1}$**

1. **Domain**: $(-\infty, \infty)$. (Denominator is never zero).
    
2. **Intercepts**: $y$-int: $(0,0)$. $x$-int: $(0,0)$.
    
3. **Symmetry**: $f(-x) = \frac{-x}{(-x)^2+1} = -f(x)$. Odd function.
    
4. **Asymptotes / End Behavior**: $\lim_{x\to\pm\infty} \frac{x}{x^2+1} = 0$. Horizontal asymptote at $y=0$.
    
5. **First Derivative**: (Quotient Rule)
    
    $$f'(x) = \frac{(1)(x^2+1) - (x)(2x)}{(x^2+1)^2} = \frac{1 - x^2}{(x^2+1)^2}$$
    
6. **Critical Numbers**: Numerator $= 0 \implies 1 - x^2 = 0 \implies x = \pm 1$.
    
7. **$f'$ Sign Chart**: (Note denominator is always positive)
    
    - $x < -1$: test $-2 \implies \frac{-}{+} = -$ (Dec)
        
    - $-1 < x < 1$: test $0 \implies \frac{+}{+} = +$ (Inc)
        
    - $x > 1$: test $2 \implies \frac{-}{+} = -$ (Dec)
        
8. **Local Extrema**: Local min at $x = -1, y = -1/2$. Local max at $x = 1, y = 1/2$.
    
9. **Second Derivative**:
    
    $$f''(x) = \frac{(-2x)(x^2+1)^2 - (1-x^2) \cdot 2(x^2+1)(2x)}{(x^2+1)^4}$$
    
    Factor out $(x^2+1)$:
    
    $$f''(x) = \frac{(-2x)(x^2+1) - 4x(1-x^2)}{(x^2+1)^3} = \frac{-2x^3 - 2x - 4x + 4x^3}{(x^2+1)^3} = \frac{2x^3 - 6x}{(x^2+1)^3} = \frac{2x(x^2-3)}{(x^2+1)^3}$$
    
10. **$f''$ Sign Chart**: Candidates where numerator $= 0 \implies x=0, \pm\sqrt{3}$.
    
    - $x < -\sqrt{3}$: test $-2 \implies \frac{(-)(+)}{+} = -$ (Concave Down)
        
    - $-\sqrt{3} < x < 0$: test $-1 \implies \frac{(-)(-)}{+} = +$ (Concave Up)
        
    - $0 < x < \sqrt{3}$: test $1 \implies \frac{(+)(-)}{+} = -$ (Concave Down)
        
    - $x > \sqrt{3}$: test $2 \implies \frac{(+)(+)}{+} = +$ (Concave Up)
        
11. **Inflection Points**: All three candidates are IPs. $(0,0)$, $(\sqrt{3}, \frac{\sqrt{3}}{4})$, and $(-\sqrt{3}, -\frac{\sqrt{3}}{4})$.
    
12. **Final Sketch Description**: Approaching from the left along $y=0$, the curve dips down (concave down) to a minimum at $x=-1$, flips concavity at $-\sqrt{3}$ on the way, passes through the origin where it flips to concave down, reaches a peak at $x=1$, flips to concave up at $\sqrt{3}$, and gradually flattens out toward $y=0$ as $x \to \infty$.
    

## 8. Sign Charts and Common Mistakes

To build a reliable sign chart:

- **Factor completely**: It is much easier to evaluate $3(x-1)(x+1)$ by looking at positive/negative signs than plugging numbers into $3x^2 - 3$.
    
- **Mark undefined points**: Vertical asymptotes must go on your sign chart! The derivative can change signs across an asymptote.
    
- **Test points systematically**: Choose easy integers inside every sub-interval.
    

**Common Errors & Corrections:**

- **Treating $f'(c)=0$ as proof of a maximum or minimum.**
    
    - _Why it's wrong:_ It could be a stationary inflection point (like $y=x^3$ at $x=0$).
        
    - _Correction:_ Always check the sign change on either side.
        
- **Treating $f''(c)=0$ as proof of an inflection point.**
    
    - _Why it's wrong:_ The function could just touch 0 concavity and bounce back (like $y=x^4$ at $x=0$).
        
    - _Correction:_ Verify the sign of $f''$ changes across $c$.
        
- **Confusing the graph of $f$ with the graphs of $f'$ and $f''$.**
    
    - _Why it's wrong:_ An $x$-intercept on $f'$ means a horizontal tangent on $f$. A peak on $f'$ means the _steepest slope_ on $f$ (often an inflection point).
        
- **Assuming concave up means increasing.**
    
    - _Why it's wrong:_ Concave up means the _slope_ is increasing. A parabola $y=x^2$ is concave up everywhere, but it is decreasing for $x < 0$.
        
- **Confusing critical numbers with inflection-point coordinates.**
    
    - _Why it's wrong:_ Critical numbers come from $f'$. Inflection points come from $f''$. Do not mix the candidates.
        
- **Forgetting to calculate the $y$-coordinate.**
    
    - _Why it's wrong:_ You cannot graph $x=2$. You must graph $(2, f(2))$. Always plug back into the **original** function $f(x)$ for coordinates.
        
- **Ignoring domain restrictions.**
    
    - _Why it's wrong:_ If $f(x) = \ln(x)$, checking $x=-1$ on your sign chart is mathematically invalid.
        

## 9. How to Reconstruct a Graph from Derivative Information

If you are handed only the sign charts for $f'$ and $f''$:

1. Determine the overall shape: Combine increasing/decreasing and concavity (using the table from section 6).
    
2. Identify peaks and valleys (from $f'$ sign changes).
    
3. Identify where the curve bends (from $f''$ sign changes).
    
4. **What is missing?** The vertical position.
    

Because the derivative of a constant is zero, $\frac{d}{dx}[f(x) + C] = f'(x)$. Therefore, the functions $f(x)$, $f(x)+5$, and $f(x)-10$ all have the exact same first and second derivatives. Without an initial point (an "initial condition"), derivative sign charts give you the exact _shape_ of the curve, but you do not know where to anchor it on the $y$-axis. Adding a constant shifts the graph vertically without changing its geometry.

## 10. Connections to Other Calculus Concepts

- **Tangent lines and local linear approximation**: Near a point $x=a$, the curve is approximated by its tangent line $L(x) = f(a) + f'(a)(x-a)$. $f'$ determines this line.
    
- **Second derivatives and quadratic approximation**: The second derivative accounts for the bending away from the tangent line. The Taylor polynomial $P_2(x) = f(a) + f'(a)(x-a) + \frac{f''(a)}{2}(x-a)^2$ perfectly captures the concavity.
    
- **Optimization**: Finding absolute extrema on a closed interval (often used in physics or economics) relies heavily on identifying critical numbers via $f'(x)=0$.
    

## 11. Conceptual Questions

Test your deeper understanding before moving on:

1. Can a function have a horizontal tangent without a local maximum or minimum?
    
2. Can an inflection point occur where $f'(x) \neq 0$?
    
3. Can $f''(x) = 0$ at a local maximum?
    
4. Can a function be decreasing and concave up simultaneously?
    
5. Can two functions have identical first and second derivatives but different graphs?
    
6. If $f'$ has a local maximum, what does that tell us about $f$?
    
7. Why do derivative sign charts provide more information than simply solving $f'=0$ and $f''=0$?
    
8. What information is lost when we examine derivatives without knowing the original function's vertical position?
    

9. **Yes.** A stationary inflection point, like $f(x)=x^3$ at $x=0$, has a horizontal tangent but is strictly increasing.
    
10. **Yes.** $f(x) = \sin(x)$ has an inflection point at $x=\pi$. Its slope there is $f'(\pi) = \cos(\pi) = -1 \neq 0$.
    
11. **Yes.** Consider $f(x) = -x^4$. The maximum is at $x=0$. $f''(0) = -12(0)^2 = 0$. The Second Derivative Test fails here, but the First Derivative Test proves it's a maximum.
    
12. **Yes.** Think of the left side of a regular parabola $y=x^2$ for $x < 0$. It is sliding downward (decreasing) but curved like a cup (concave up).
    
13. **Yes.** $f(x) = x^2$ and $g(x) = x^2 + 5$ have identical derivatives but are shifted vertically relative to one another.
    
14. **It tells us $f$ has an inflection point of maximum steepness.** If $f'$ has a maximum, the slope stops increasing and starts decreasing. This means $f''$ changes from positive to negative, which by definition is an inflection point.
    
15. **Because roots only tell you _where_ candidates are.** Sign charts tell you the behavior _between_ the roots. Without the signs, you don't know if a candidate is a max, min, inflection point, or nothing at all.
    
16. **The exact $y$-values (intercepts).** We know the shape perfectly, but we don't know if the entire graph sits above the $x$-axis, crosses it, or sits below it.
    

## 12. Practice Problems

**Basic:**

For $f(x) = x^2 - 4x + 3$: Find the intervals where the function is increasing/decreasing and its concavity.

**Intermediate:**

For $g(x) = x^4 - 2x^3$: Find all critical numbers, local extrema, and inflection points.

**Advanced:**

Sketch $h(x) = x^6 - 10x^4$. Prove that $x=0$ is a critical number where the second derivative is zero, but it is NOT an inflection point. Also find the genuine inflection points and local extrema.

**Basic Solution:**

- $f'(x) = 2x - 4$. Set $2x-4=0 \implies x=2$.
    
- Sign chart for $f'$: $x<2 \implies -$ (Dec). $x>2 \implies +$ (Inc).
    
- $f''(x) = 2$. Always positive.
    
- _Conclusion:_ Decreasing on $(-\infty, 2)$, Increasing on $(2, \infty)$. Concave up everywhere.
    

**Intermediate Solution:**

- $g'(x) = 4x^3 - 6x^2 = 2x^2(2x-3)$. Critical numbers: $x=0$, $x=3/2$.
    
- $f'$ Sign chart:
    
    - $x < 0 \implies (-)$.
        
    - $0 < x < 3/2 \implies (+)(-)=(-)$.
        
    - $x > 3/2 \implies (+)(+)=(+)$.
        
    - Local min at $x=3/2$. Neither max nor min at $x=0$.
        
- $g''(x) = 12x^2 - 12x = 12x(x-1)$. Candidates: $x=0, 1$.
    
- $f''$ Sign chart:
    
    - $x < 0 \implies (+)$.
        
    - $0 < x < 1 \implies (-)$.
        
    - $x > 1 \implies (+)$.
        
    - _Conclusion:_ Inflection points at $x=0$ and $x=1$. Local min at $x=3/2$.
        

**Advanced Solution:**

- $h'(x) = 6x^5 - 40x^3 = 2x^3(3x^2 - 20)$. Critical numbers: $x=0, \pm\sqrt{20/3}$.
    
- $h''(x) = 30x^4 - 120x^2 = 30x^2(x^2 - 4)$. Candidates: $x=0, \pm 2$.
    
- Look at $h''(x)$ around $x=0$. The factor $30x^2$ is always positive. The factor $(x^2 - 4)$ is negative for $x \in (-2, 2)$. Thus, $h''(x)$ is negative on both sides of $x=0$. Concavity does NOT change. $x=0$ is not an IP.
    
- Since $h'$ changes from $+$ to $-$ at $x=0$, it is a local maximum.
    
- $h''$ changes sign at $x=-2$ and $x=2$ (from positive to negative at $-2$, from negative to positive at $2$). These are genuine inflection points.
    

## 13. Final Summary and Formula Sheet

**Curve Sketching Workflow:**

1. Algebra first: Domain, Intercepts, Asymptotes.
    
2. First Derivative $f'$: Find roots/undefined points. Build sign chart. Find Inc/Dec and Extrema.
    
3. Second Derivative $f''$: Find roots/undefined points. Build sign chart. Find Concavity and Inflection Points.
    
4. Synthesize and draw.
    

**Derivative Interpretation Table**

|**Math**|**Geometric Reality**|
|---|---|
|$f'(x) > 0$|Curve is rising.|
|$f'(x) < 0$|Curve is falling.|
|$f''(x) > 0$|Curve is concave up ($\cup$).|
|$f''(x) < 0$|Curve is concave down ($\cap$).|
|$f'(c) = 0$|Tangent is horizontal (Critical Number).|
|$f'(c)$ changes sign|Local Extremum (Max or Min).|
|$f''(c)$ changes sign|Inflection Point.|

**Key Distinctions**

- **Critical Point:** Where $f'(x)=0$ or is undefined. (Candidates for extrema).
    
- **Stationary Point:** A critical point specifically where $f'(x)=0$.
    
- **Inflection Point:** Where concavity changes (requires $f''$ to flip signs).
    

**The Most Important Conceptual Lesson:**

Never trust a zero derivative implicitly. $f'(c)=0$ is merely a request for investigation, not a guarantee of a peak or valley. $f''(c)=0$ is a request for investigation, not a guarantee of an inflection point. **The sign chart (the behavior around the point) is the only true proof of the graph's geometry.**


[[Lecture9_Linear_&_Quadratic_approximations]]