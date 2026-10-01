
## 1. Start From Logistic Regression

Before analyzing the boundary, we must establish the foundations of the logistic regression model.

**Definition (The Linear Score):**

First, we compute a raw, unbounded linear score $z$ from our input features $\mathbf{x}$, weights $\mathbf{w}$, and bias $b$:

$$z = \mathbf{w}^T \mathbf{x} + b$$

**Definition (The Sigmoid Transformation):**

Because $z$ can range from $-\infty$ to $+\infty$, we pass it through the sigmoid function to squash it into a valid probability range between 0 and 1:

$$\sigma(z) = \frac{1}{1 + e^{-z}}$$

**Definition (The Model Output):**

The final output of our model is therefore:

$$f_{\mathbf{w},b}(\mathbf{x}) = \sigma(\mathbf{w}^T \mathbf{x} + b)$$

**Interpretation:**

$z$ represents the "confidence score" of the model. Large positive values mean high confidence in Class 1; large negative values mean high confidence in Class 0.

$\sigma(z)$ transforms this score into a percentage.

- **Probability:** We interpret $f_{\mathbf{w},b}(\mathbf{x})$ as the estimated probability that the target $y = 1$ given the input $\mathbf{x}$.
    

_Example:_ If $z = 1.386$, then $\sigma(1.386) \approx 0.8$. We interpret this as: "There is an 80% probability that this example belongs to Class 1."

## 2. From Probability to Class

While the model outputs a probability, practical applications usually require a hard decision (e.g., "Spam" or "Not Spam"). We must convert the probability into a class prediction.

**The Standard Classification Rule:**

- If $f_{\mathbf{w},b}(\mathbf{x}) \geq 0.5 \rightarrow$ predict Class 1
    
- If $f_{\mathbf{w},b}(\mathbf{x}) < 0.5 \rightarrow$ predict Class 0
    

**What does this threshold mean?**

The value 0.5 represents a state of perfect indifference. If the probability of Class 1 is exactly 50%, the model is essentially tossing a coin. By choosing 0.5 as our threshold, we are saying: _"Whichever class has the higher probability is the class I will predict."_

**Is 0.5 mathematically mandatory?**

No. It is simply a common default choice for symmetric problems. If predicting a false positive is highly dangerous (like approving a fraudulent transaction), you might raise the threshold to 0.9. Changing the threshold directly changes _where_ the model switches its prediction, which in turn moves the decision boundary.

## 3. Derive the Decision Boundary From First Principles

The decision boundary is not a magical formula to memorize. It is a **derived result** that naturally emerges from asking one question: _Where exactly does the model switch from predicting Class 0 to predicting Class 1?_

**Derivation:**

1. **Definition of the boundary:** The boundary exists exactly where the model is indifferent between the two classes. Given a threshold of 0.5, this occurs when:
    

$$f_{\mathbf{w},b}(\mathbf{x}) = 0.5$$

2. **Substitute the sigmoid function:**
    

$$\frac{1}{1 + e^{-z}} = 0.5$$

3. **Solve for $z$ algebraically:**
    
    Multiply both sides by $(1 + e^{-z})$:
    

$$1 = 0.5(1 + e^{-z})$$

Divide by 0.5:

$$2 = 1 + e^{-z}$$

Subtract 1:

$$1 = e^{-z}$$

Take the natural logarithm ($\ln$) of both sides. Since $\ln(1) = 0$:

$$0 = -z$$

$$z = 0$$

4. **Substitute the definition of $z$:**
    
    Since $z = \mathbf{w}^T \mathbf{x} + b$, we substitute this back in to get the final equation:
    

$$\mathbf{w}^T \mathbf{x} + b = 0$$

**Summary of the logical chain:**

Classification threshold (0.5) $\rightarrow$ Sigmoid output = 0.5 $\rightarrow$ Sigmoid input = 0 $\rightarrow$ Linear model = 0.

## 4. Why Does $\sigma(z) = 0.5$ Correspond to $z = 0$?

Let's look at the mathematics of the sigmoid function independently to make this relationship intuitive.

If we plug $z = 0$ into the sigmoid function:

$$\sigma(0) = \frac{1}{1 + e^0} = \frac{1}{1 + 1} = \frac{1}{2} = 0.5$$

Because $e^{-z}$ shrinks as $z$ gets larger and grows as $z$ gets more negative, we can observe three clear states:

- If $z > 0 \rightarrow e^{-z} < 1 \rightarrow \text{denominator} < 2 \rightarrow \sigma(z) > 0.5$
    
- If $z < 0 \rightarrow e^{-z} > 1 \rightarrow \text{denominator} > 2 \rightarrow \sigma(z) < 0.5$
    
- If $z = 0 \rightarrow e^{-z} = 1 \rightarrow \text{denominator} = 2 \rightarrow \sigma(z) = 0.5$
    

**Crucial Insight:**

If our threshold is 0.5, we do not actually need to calculate the sigmoid function to determine the predicted class! We can simply look at the sign of $z$:

- $z \geq 0 \rightarrow$ predict Class 1
    
- $z < 0 \rightarrow$ predict Class 0
    

## 5. Geometric Meaning in 2D

To visualize what $\mathbf{w}^T \mathbf{x} + b = 0$ actually is, let's restrict our feature space to two dimensions: $\mathbf{x} = [x_1, x_2]$.

The vector equation expands to:

$$w_1 x_1 + w_2 x_2 + b = 0$$

Let's rearrange this algebraically to solve for $x_2$ (the vertical axis on a standard 2D plot):

$$w_2 x_2 = -w_1 x_1 - b$$

$$x_2 = -\left(\frac{w_1}{w_2}\right)x_1 - \frac{b}{w_2}$$

This is the standard equation of a straight line ($y = mx + c$)!

- **Slope ($m$):** $-\frac{w_1}{w_2}$
    
- **Intercept ($c$):** $-\frac{b}{w_2}$
    

This line divides the 2D feature space into two regions. One side corresponds to Class 1 (where $z > 0$), and the other side corresponds to Class 0 (where $z < 0$).

### Concrete Example:

Let's assume our learned parameters are:

- $w_1 = 2$
    
- $w_2 = 1$
    
- $b = -3$
    

The decision boundary equation is:

$$2x_1 + x_2 - 3 = 0$$

Rearranging for $x_2$:

$$x_2 = 3 - 2x_1$$

If we plug in a point above this line, say $x_1=2, x_2=2$:

$z = 2(2) + 1(2) - 3 = 3$ (Positive, Class 1)

If we plug in a point below this line, say $x_1=0, x_2=0$:

$z = 2(0) + 1(0) - 3 = -3$ (Negative, Class 0)

## 6. Why Is the Weight Vector Important Geometrically?

Let's look closely at the relationship between the weight vector $\mathbf{w} = [w_1, w_2]$ and the decision boundary.

**Geometric Fact:** The vector $\mathbf{w}$ is perpendicular (normal) to the decision boundary line.

**Why? (The dot product intuition):**

The equation of the boundary is $\mathbf{w}^T \mathbf{x} + b = 0$.

Imagine two different points, $\mathbf{x}_A$ and $\mathbf{x}_B$, that both sit exactly on the decision boundary. Because they are on the boundary, we know:

1. $\mathbf{w}^T \mathbf{x}_A + b = 0$
    
2. $\mathbf{w}^T \mathbf{x}_B + b = 0$
    

If we subtract the second equation from the first, the $b$'s cancel out:

$$\mathbf{w}^T \mathbf{x}_A - \mathbf{w}^T \mathbf{x}_B = 0$$

$$\mathbf{w}^T (\mathbf{x}_A - \mathbf{x}_B) = 0$$

The term $(\mathbf{x}_A - \mathbf{x}_B)$ is a vector pointing _along_ the decision boundary (from point B to point A). The dot product of $\mathbf{w}$ and this boundary vector is 0. In linear algebra, if the dot product of two vectors is 0, they are perpendicular (orthogonal) at exactly 90 degrees.

**Intuition:** $\mathbf{w}$ represents the direction of steepest ascent for the score $z$. Moving in the exact direction of $\mathbf{w}$ increases $z$ (and thus the probability of Class 1) faster than moving in any other direction. The boundary is the line where the score is stagnant at exactly 0.

## 7. Understanding the Two Sides of the Boundary

Because the boundary is a line (or surface) in space, it carves the universe into three sets of points.

|**Region in Space**|**Linear Score (z)**|**Probability (σ(z))**|**Prediction (Threshold=0.5)**|
|---|---|---|---|
|On the "positive" side of boundary|$w^T x + b > 0$|$p > 0.5$|**Class 1**|
|Exactly ON the boundary|$w^T x + b = 0$|$p = 0.5$|**Indifferent (50/50)**|
|On the "negative" side of boundary|$w^T x + b < 0$|$p < 0.5$|**Class 0**|

## 8. What Happens With More Than Two Features?

The beauty of linear algebra is that the equation $\mathbf{w}^T \mathbf{x} + b = 0$ works identically no matter how many features we have. The geometric shape of the boundary simply scales up in dimension.

- **1 Feature ($x_1$):** $w_1 x_1 + b = 0$. The boundary is a single **point** (a threshold on a number line).
    
- **2 Features ($x_1, x_2$):** $w_1 x_1 + w_2 x_2 + b = 0$. The boundary is a **line** on a 2D plane.
    
- **3 Features ($x_1, x_2, x_3$):** $w_1 x_1 + w_2 x_2 + w_3 x_3 + b = 0$. The boundary is a flat **plane** in a 3D room.
    

$n$ Features: $\mathbf{w}^T \mathbf{x} + b = 0$. The boundary is a **hyperplane**.

A hyperplane is simply a flat subspace that has one dimension less than the feature space containing it. It acts as a perfect, flat divider cutting the $n$-dimensional space in half.

## 9. Decision Boundary vs Sigmoid Curve

These two concepts are frequently confused, but they live in entirely different conceptual spaces.

1. **The Sigmoid Curve ($p = \sigma(z)$):** This is an 'S' shaped curve. It lives in a 2D graph where the horizontal axis is the score $z$, and the vertical axis is the probability $p$. It shows how probability changes relative to $z$.
    
2. **The Decision Boundary ($\mathbf{w}^T \mathbf{x} + b = 0$):** This lives entirely in the **input feature space** (e.g., the map of $x_1$ vs $x_2$). It is the "line in the sand" drawn across your actual data points.
    

_Analogy:_ The decision boundary is the border between two countries on a map. The sigmoid curve is a chart showing how your geographical altitude changes as you walk across that border.

## 10. Decision Boundary Is Not Necessarily the Same as "Where Data Changes"

A common mistake is looking at a scatter plot of data and drawing a line by hand between the red and blue dots, then calling that "the decision boundary."

- **Training Examples:** The actual data points.
    
- **Learned Model:** The specific values of $\mathbf{w}$ and $b$ discovered by Gradient Descent.
    
- **Decision Boundary:** The rigid mathematical line dictated exclusively by $\mathbf{w}$ and $b$.
    

While gradient descent tries to find $\mathbf{w}$ and $b$ that separate the data well, the final boundary is dictated strictly by the math. If the data is messy or overlapping, the boundary will just slice straight through the middle of the mess. It doesn't curve around individual data points on its own.

## 11. Nonlinear Decision Boundaries

What if the data cannot be separated by a straight line? We can use **Polynomial Feature Engineering** (from Week 2).

Suppose we start with two features, $x_1$ and $x_2$. We can manually create new, engineered features by squaring them or multiplying them: $x_1^2, x_2^2, x_1 x_2$.

Our linear score $z$ now looks like this:

$$z = w_1 x_1^2 + w_2 x_2^2 + w_3 x_1 x_2 + b$$

The decision boundary (where $z = 0$) becomes:

$$w_1 x_1^2 + w_2 x_2^2 + w_3 x_1 x_2 + b = 0$$

**Concrete Example (A Circle):**

Imagine gradient descent finds the following weights: $w_1 = 1$, $w_2 = 1$, $w_3 = 0$, $b = -4$.

The boundary is:

$$1x_1^2 + 1x_2^2 + 0 - 4 = 0$$

$$x_1^2 + x_2^2 = 4$$

In geometry, $x^2 + y^2 = r^2$ is the equation of a circle centered at the origin with radius $r$.

Our decision boundary is a perfect circle of radius 2 in the original feature space!

- Inside the circle: $x_1^2 + x_2^2 - 4 < 0 \rightarrow z < 0 \rightarrow$ predict Class 0.
    
- Outside the circle: $x_1^2 + x_2^2 - 4 > 0 \rightarrow z > 0 \rightarrow$ predict Class 1.
    

## 12. Important Distinction: Linear Model vs Linear Decision Boundary

How can Logistic Regression create a circle if it is a "linear" model?

**Definition:** A model is called "linear" if the output $z$ is a linear combination of the **parameters (weights)**.

Notice in the equation $z = w_1 x_1^2 + w_2 x_2^2 + b$, neither $w_1$, $w_2$, nor $b$ are squared or multiplied together. The model is strictly linear with respect to $\mathbf{w}$.

However, the decision boundary exists in the **original feature space** ($x_1, x_2$). Because we squared the inputs before feeding them to the model, the resulting boundary drawn in the original space is a curve.

Therefore: A model that is linear in its parameters can easily produce a nonlinear decision boundary in its original features via polynomial feature engineering.

## 13. Changing the Classification Threshold

What if we want to be very confident before predicting Class 1? We might change our rule to:

_Predict Class 1 only if $p \geq 0.7$_.

Where is the decision boundary now? It is no longer at $z=0$.

1. The boundary is where $p = 0.7$.
    
2. $\sigma(z) = 0.7$
    
3. $\frac{1}{1 + e^{-z}} = 0.7$
    
4. $1 + e^{-z} = \frac{1}{0.7}$
    
5. $e^{-z} = \frac{1 - 0.7}{0.7} = \frac{0.3}{0.7}$
    
6. $-z = \ln\left(\frac{0.3}{0.7}\right)$
    
7. $z = \ln\left(\frac{0.7}{0.3}\right) \approx 0.847$
    

**The General Rule for Threshold $t$:**

If threshold $= t$, then the boundary is where:

$$z = \ln\left(\frac{t}{1 - t}\right)$$

Therefore, the new decision boundary equation is:

$$\mathbf{w}^T \mathbf{x} + b = \ln\left(\frac{t}{1 - t}\right)$$

**Conceptually:** Increasing the threshold shifts the boundary line away from Class 1, shrinking the area of the map where Class 1 is predicted. The line stays parallel to the original boundary; it just slides over.

## 14. Worked Example From Start to Finish

Let's tie it all together with a concrete, numerical scenario.

- **Weights:** $\mathbf{w} = [2, 1]$
    
- **Bias:** $b = -3$
    
- **Threshold:** $0.5$ (meaning boundary is where $z=0$)
    
- **Boundary Equation:** $2x_1 + x_2 - 3 = 0$ (or $x_2 = 3 - 2x_1$)
    

Let's test several points in feature space:

|**Point (x1​,x2​)**|**Calculate z=2x1​+x2​−3**|**Sigmoid p=1/(1+e−z)**|**Predict**|**Location vs Boundary**|
|---|---|---|---|---|
|**(0, 0)**|$2(0) + 1(0) - 3 = -3$|$1 / (1 + e^3) \approx 0.047$ (4.7%)|**Class 0**|Below line ($z < 0$)|
|**(1, 0)**|$2(1) + 1(0) - 3 = -1$|$1 / (1 + e^1) \approx 0.269$ (26.9%)|**Class 0**|Below line ($z < 0$)|
|**(1, 1)**|$2(1) + 1(1) - 3 = 0$|$1 / (1 + e^0) = 0.5$ (50%)|**Indifferent**|**Exactly ON line** ($z = 0$)|
|**(0, 3)**|$2(0) + 1(3) - 3 = 0$|$1 / (1 + e^0) = 0.5$ (50%)|**Indifferent**|**Exactly ON line** ($z = 0$)|
|**(2, 1)**|$2(2) + 1(1) - 3 = 2$|$1 / (1 + e^{-2}) \approx 0.881$ (88.1%)|**Class 1**|Above line ($z > 0$)|

Notice how points exactly on the line ($1,1$ and $0,3$) produce exactly $z=0$ and $p=0.5$. The numerical predictions perfectly align with the geometry of the line $x_2 = 3 - 2x_1$.

## 15. Connection to Linear Algebra

In vector notation, the boundary $\mathbf{w}^T \mathbf{x} + b = 0$ tells a clear geometric story:

$\mathbf{x}$ is a point in feature space.

$\mathbf{w}^T \mathbf{x}$ is the dot product. It measures the "projection" of the point $\mathbf{x}$ onto the weight vector $\mathbf{w}$.

$\mathbf{w}$ is the **normal vector**. It strictly controls the _angle/orientation_ of the decision boundary. Changing the weights rotates the line.

$b$ is the **translation term**. It shifts the boundary toward or away from the origin without rotating it. If $b=0$, the decision boundary is guaranteed to pass perfectly through the origin $(0,0)$, because $\mathbf{w}^T [0, 0] + 0 = 0$.

## 16. Common Misconceptions

- **"The decision boundary is where sigmoid = 0."** $\rightarrow$ FALSE. The sigmoid function can never equal 0. The boundary is where the _input_ to the sigmoid ($z$) equals 0, making the sigmoid output 0.5.
    
- **"The decision boundary is the sigmoid curve."** $\rightarrow$ FALSE. The sigmoid curve charts probability against $z$. The decision boundary is a line/plane separating the actual input features (like height vs. weight).
    
- **"The decision boundary is always a straight line."** $\rightarrow$ FALSE. While $\mathbf{w}^T \mathbf{x} + b = 0$ is mathematically linear, polynomial feature engineering allows it to form curves, circles, and complex shapes in the original feature space.
    
- **"0.5 is mathematically the only possible threshold."** $\rightarrow$ FALSE. 0.5 is just a convenient convention for balanced confidence. You can set it to anything between 0 and 1.
    
- **"w is parallel to the decision boundary."** $\rightarrow$ FALSE. The weight vector $\mathbf{w}$ is exactly perpendicular (orthogonal) to the boundary.
    
- **"b determines the slope."** $\rightarrow$ FALSE. The ratio of the weights ($-w_1/w_2$) determines the slope. $b$ only shifts the line up and down (translation).
    
- **"The decision boundary is manually chosen from the graph."** $\rightarrow$ FALSE. It is mathematically rigid and determined entirely by the learned values of $\mathbf{w}$ and $b$.
    
- **"Every training example must lie on one side with no mistakes."** $\rightarrow$ FALSE. Logistic regression draws the _best possible_ line to minimize cost. If data is overlapping, the boundary will naturally place some examples on the "wrong" side (misclassifications).
    
- **"If a model has a nonlinear decision boundary, it is no longer logistic regression."** $\rightarrow$ FALSE. If you apply polynomial features and then run logistic regression, the underlying math engine is still strictly logistic regression.
    

## 17. Big Picture

The process of deciding a class flows entirely downstream from the features to the boundary:

**Features** $\mathbf{x}$

$\downarrow$ (Multiply by weights and add bias)

**Linear Score** $z = \mathbf{w}^T \mathbf{x} + b$

$\downarrow$ (Squash through function)

**Sigmoid Output** $p = \sigma(z)$

$\downarrow$ (Apply a rule, usually 0.5)

**Classification Threshold**

$\downarrow$ (Find where the threshold is exactly met)

**Decision Boundary** $\mathbf{w}^T \mathbf{x} + b = 0$

$\downarrow$ (Check which side of the boundary $\mathbf{x}$ falls on)

**Class 0 or Class 1**

_Every step is an automatic mathematical consequence of the step before it._

## 18. Key Takeaways

1. **The Central Insight:** The equation $\mathbf{w}^T \mathbf{x} + b = 0$ is not a random rule; it naturally appears when we ask "Where is the model exactly 50% sure?" because $\sigma(0) = 0.5$.
    
2. The score $z = \mathbf{w}^T \mathbf{x} + b$ contains all the information needed to classify; if threshold is 0.5, positive $z$ means Class 1, negative $z$ means Class 0.
    
3. In 2D, the boundary is a line. In 3D, a plane. In $n$D, a hyperplane.
    
4. The weight vector $\mathbf{w}$ is always perpendicular to the decision boundary.
    
5. $\mathbf{w}$ controls the rotation of the boundary; $b$ controls its shift/offset from the origin.
    
6. The boundary lives in the feature space (your $X$ axes), unlike the sigmoid curve which lives in the probability space (your $Y$ axis).
    
7. A linear model can produce complex, nonlinear boundaries (like circles) by using polynomial feature engineering ($x^2$, $x_1x_2$).
    
8. The boundary is a rigid mathematical construct dictated by $\mathbf{w}$ and $b$; it does not "bend" to accommodate data points unless you add polynomial features.
    
9. Changing the classification threshold away from 0.5 simply shifts the decision boundary parallel to its original position.

[[Classification_&_Logistic_Regression]]