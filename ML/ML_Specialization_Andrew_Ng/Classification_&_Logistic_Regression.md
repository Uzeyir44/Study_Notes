
## 1. Why Classification?

**Classification** is the task of predicting a discrete category or class, whereas **regression** predicts a continuous number.

In **binary classification**, there are only two possible outcomes. We typically represent these classes as $y \in \{0, 1\}$.

- **Examples:** Email (Spam = 1, Not Spam = 0), Medical Diagnosis (Malignant = 1, Benign = 0), Image Recognition (Cat = 1, Not Cat = 0).
    

**Why is Ordinary Linear Regression Not Ideal for Classification?**

In linear regression, our prediction $f_{\mathbf{w},b}(\mathbf{x}) = \mathbf{w}^T\mathbf{x} + b$ can output any value from $-\infty$ to $+\infty$.

If we want to predict a probability (which must strictly be between 0 and 1), outputting a value like $-2.5$ or $14.2$ makes no sense.

Furthermore, applying a simple threshold to linear regression (e.g., if output $\ge 0.5$, predict 1) is highly fragile.

- **Numerical Example:** Imagine plotting tumor size ($x$) against malignancy ($y \in \{0, 1\}$). A linear regression line might fit nicely through small benign tumors and medium malignant ones, with a threshold crossing exactly at $x = 5$cm. But if we add a massive malignant tumor ($x = 20$cm) to our dataset, the linear regression line will tilt heavily to minimize the squared error for that massive outlier. This tilt shifts the threshold far to the right, suddenly causing medium malignant tumors to be misclassified as benign.
    

We need a model that squashes predictions between 0 and 1, explicitly representing **probabilities**, so that extreme outliers don't warp the decision boundary.

## 2. From Linear Regression to Logistic Regression

Let's start with our familiar linear model:

$$z = \mathbf{w}^T\mathbf{x} + b$$

**What $z$ represents:** This is a raw, unbounded linear score. It can take any value from $-\infty$ to $+\infty$. Because it is unbounded, $z$ itself cannot directly represent a probability. We need a function to convert this raw score into a valid probability $p$ between 0 and 1.

**The First-Principles Derivation of the Sigmoid Function:**

How do we mathematically map an unbounded domain $(-\infty, +\infty)$ to a bounded one $(0, 1)$? We use the concepts of odds and logarithms.

1. **Probability ($p$):** The chance of an event happening. Range: $[0, 1]$.
    
2. **Odds:** The ratio of the probability of an event happening to it not happening.
    

$$\text{Odds} = \frac{p}{1-p}$$

```
If \(p = 0.8\), the odds are \(0.8/0.2 = 4\) (or "4 to 1"). Range: \([0, +\infty)\).
```

3. **Log-Odds (Logit):** If we take the natural logarithm of the odds, we stretch the range to encompass all real numbers.
    

$$\text{Log-Odds} = \ln\left(\frac{p}{1-p}\right)$$

```
Range: \((-\infty, +\infty)\).
```

**The Modeling Assumption:**

Because our raw linear score $z = \mathbf{w}^T\mathbf{x} + b$ also ranges from $(-\infty, +\infty)$, it is mathematically natural to equate it to the log-odds. _This is the core assumption defining logistic regression:_

$$\ln\left(\frac{p}{1-p}\right) = \mathbf{w}^T\mathbf{x} + b = z$$

**Solving for $p$ (The Derived Result):**

Now, let's algebraically isolate $p$ to find our prediction function.

First, exponentiate both sides to remove the natural log:

$$\frac{p}{1-p} = e^z$$

Multiply both sides by $(1-p)$:

$$p = e^z (1 - p)$$

$$p = e^z - p e^z$$

Move all terms containing $p$ to the left side:

$$p + p e^z = e^z$$

Factor out $p$:

$$p(1 + e^z) = e^z$$

Divide to isolate $p$:

$$p = \frac{e^z}{1 + e^z}$$

Finally, multiply the numerator and denominator by $e^{-z}$:

$$p = \frac{1}{1 + e^{-z}}$$

This derived formula is the **sigmoid (or logistic) function**, denoted as $\sigma(z)$.

_Crucial distinction:_ The sigmoid formula isn't just a random curve we picked; it is the strict algebraic result of **assuming** the log-odds of our classes follow a linear model.

## 3. Interpreting the Sigmoid Function

The sigmoid function $\sigma(z) = \frac{1}{1 + e^{-z}}$ squashes any real number $z$ into the range $(0, 1)$.

- **When $z$ is very negative (e.g., -100):** $e^{-(-100)} = e^{100}$, which is massive. $1 / (1 + \text{massive}) \approx 0$.
    
- **When $z = 0$:** $e^0 = 1$. $\sigma(0) = 1 / (1 + 1) = 0.5$.
    
- **When $z$ is very positive (e.g., +100):** $e^{-100} \approx 0$. $\sigma(100) = 1 / (1 + 0) = 1$.
    

It approaches 0 and 1 asymptotically but never actually touches them, which perfectly matches a probabilistic interpretation (we are rarely 100% mathematically certain).

**Numerical Table:**

|**z (Raw Score)**|**σ(z) (Probability)**|**Interpretation**|
|---|---|---|
|-5|0.0067|Extremely unlikely to be class 1 (< 1%)|
|-2|0.1192|Unlikely to be class 1 (~12%)|
|0|0.5000|Perfectly uncertain (50/50 chance)|
|2|0.8808|Likely to be class 1 (~88%)|
|5|0.9933|Extremely likely to be class 1 (> 99%)|

Notice the most critical relationship for classification:

$$z = 0 \iff p = 0.5$$

## 4. Logistic Regression Prediction

The complete prediction pipeline for a single input vector $\mathbf{x}$ is:

$\mathbf{x} \xrightarrow{\text{linear model}} \mathbf{w}^T\mathbf{x} + b \xrightarrow{\text{sigmoid}} \text{predicted probability } p$

We denote the final output as $f_{\mathbf{w},b}(\mathbf{x}) = \sigma(\mathbf{w}^T\mathbf{x} + b)$.

**Interpretation:**

If $f_{\mathbf{w},b}(\mathbf{x}) = 0.82$, we interpret this as the model estimating an 82% probability that $y = 1$, given the features $\mathbf{x}$ and the parameters $\mathbf{w}, b$.

Formally: $P(y=1 \vert{} \mathbf{x}; \mathbf{w}, b) = 0.82$.

**Probability vs. Class:**

- **Predicted Probability:** The raw decimal output (e.g., 0.82).
    
- **Predicted Class:** The final categorical decision (0 or 1) obtained by applying a threshold.
    
    Typically, we use a threshold of 0.5:
    
    If $f_{\mathbf{w},b}(\mathbf{x}) \ge 0.5 \implies$ predict class $y=1$
    
    If $f_{\mathbf{w},b}(\mathbf{x}) < 0.5 \implies$ predict class $y=0$
    

## 5. Decision Boundary

The decision boundary is the line (or surface) separating where the model predicts $y=1$ from where it predicts $y=0$. Do not memorize it; derive it.

Assuming a threshold of 0.5, the boundary exists exactly where the model is uncertain:

1. **Start at the threshold:** $p = 0.5$
    
2. **Substitute the model:** $\sigma(z) = 0.5$
    
3. **Find the required $z$:** From our table earlier, $\sigma(z) = 0.5$ exactly when $z = 0$.
    
4. **Substitute the linear definition of $z$:** $\mathbf{w}^T\mathbf{x} + b = 0$.
    

This equation, $\mathbf{w}^T\mathbf{x} + b = 0$, mathematically defines the decision boundary!

- **One feature:** $w_1x_1 + b = 0$ is just a single point on a number line.
    
- **Two features:** $w_1x_1 + w_2x_2 + b = 0$ is a line in a 2D plane.
    
- **Higher dimensions:** It becomes a flat hyperplane.
    

**On either side of the boundary:**

- If $\mathbf{w}^T\mathbf{x} + b > 0$, we predict $y=1$.
    
- If $\mathbf{w}^T\mathbf{x} + b < 0$, we predict $y=0$.
    

**2-Feature Numerical Example:**

Suppose $w_1 = 1, w_2 = -1, b = -3$.

The decision boundary is: $1x_1 - 1x_2 - 3 = 0$.

Solving for $x_2$, we get the line: $x_2 = x_1 - 3$.

Everything above this line results in $z < 0$ (predict 0). Everything below results in $z > 0$ (predict 1).

_Geometrically:_ The weight vector $\mathbf{w} = [w_1, w_2]$ always points strictly orthogonal (perpendicular) to the decision boundary, pointing in the direction where probabilities increase toward 1.

## 6. Why We Cannot Simply Use the Linear Regression Cost Function

For linear regression, we used the Mean Squared Error (MSE) cost function:

$J(\mathbf{w},b) = \frac{1}{2m} \sum (f_{\mathbf{w},b}(\mathbf{x}) - y)^2$

If we use MSE for logistic regression, $f_{\mathbf{w},b}(\mathbf{x})$ is no longer a straight line; it contains the non-linear sigmoid function $\sigma(\mathbf{w}^T\mathbf{x} + b)$.

When you plug a squiggly sigmoid into a squared function, the resulting cost function surface becomes **non-convex**.

A non-convex surface has many "valleys" (local minima). If you drop a marble (gradient descent) onto this surface, it might get stuck in a shallow valley instead of finding the true lowest point (global minimum). We need a cost function that forms a single, smooth, bowl-like (convex) shape.

## 7. Logistic Loss / Cost Function

To get a convex cost surface, we use the **Logistic Loss** (also called Binary Cross-Entropy).

For a single training example $(x, y)$, the loss is defined as:

$$Loss(f_{\mathbf{w},b}(\mathbf{x}), y) = -y \log(f_{\mathbf{w},b}(\mathbf{x})) - (1-y) \log(1-f_{\mathbf{w},b}(\mathbf{x}))$$

_Note: In machine learning, $\log$ almost always means natural logarithm $\ln$._

Where does this come from? It's designed to heavily penalize confidently wrong predictions, and because $y$ can only be exactly 0 or exactly 1, this equation acts as an elegant mathematical switch.

### Case 1: When the true label $y = 1$

Substitute $y=1$ into the loss function. The second term $-(1-1)\log(\dots)$ becomes 0 and vanishes.

$$Loss = -\log(f_{\mathbf{w},b}(\mathbf{x}))$$

- **Confident & Correct:** If prediction $f \approx 1$, then $-\log(1) = 0$. Loss is zero.
    
- **Confident & Wrong:** If prediction $f \approx 0.001$, then $-\log(0.001)$ becomes a massive positive number.
    
    _Intuition:_ The logarithm heavily penalizes the model, pushing the loss toward infinity if it predicts a 0% chance for an event that actually happened!
    

### Case 2: When the true label $y = 0$

Substitute $y=0$. The first term $-0\log(\dots)$ vanishes.

$$Loss = -\log(1 - f_{\mathbf{w},b}(\mathbf{x}))$$

- **Confident & Correct:** If prediction $f \approx 0$, then $-\log(1 - 0) = -\log(1) = 0$. Loss is zero.
    
- **Confident & Wrong:** If prediction $f \approx 0.999$, then $-\log(1 - 0.999) = -\log(0.001) \to \text{massive penalty}$.
    

**The Full Cost Function $J(\mathbf{w},b)$:**

The cost is just the average of the individual losses across all $m$ training examples:

$$J(\mathbf{w},b) = \frac{1}{m} \sum_{i=1}^{m} \left[ -y^{(i)} \log\left(f_{\mathbf{w},b}(\mathbf{x}^{(i)})\right) - (1-y^{(i)}) \log\left(1-f_{\mathbf{w},b}(\mathbf{x}^{(i)})\right) \right]$$

We average them because we want the model's parameters to perform well on the dataset as a whole, not just on a single point.

## 8. Deriving the Logistic Regression Gradient

To run gradient descent, we need the partial derivative of the cost $J$ with respect to each weight $w_j$. We find this using the calculus **chain rule**.

_Definitions:_

1. $J = -y \ln(f) - (1-y)\ln(1-f)$ _(focusing on one example for simplicity)_
    
2. $f = \sigma(z) = \frac{1}{1+e^{-z}}$
    
3. $z = \mathbf{w}^T\mathbf{x} + b = w_1x_1 + \dots + w_jx_j + \dots + b$
    

_The Chain Rule:_

$$\frac{\partial J}{\partial w_j} = \frac{\partial J}{\partial f} \cdot \frac{\partial f}{\partial z} \cdot \frac{\partial z}{\partial w_j}$$

_Step 1: Derivative of Loss with respect to $f$_

$$\frac{\partial J}{\partial f} = -\frac{y}{f} + \frac{1-y}{1-f} = \frac{-y(1-f) + f(1-y)}{f(1-f)} = \frac{-y + yf + f - fy}{f(1-f)} = \frac{f - y}{f(1-f)}$$

_Step 2: Derivative of Sigmoid with respect to $z$_

(A known property of the sigmoid derivative is $\sigma'(z) = \sigma(z)(1-\sigma(z))$)

$$\frac{\partial f}{\partial z} = f(1-f)$$

_Step 3: Derivative of $z$ with respect to $w_j$_

$$\frac{\partial z}{\partial w_j} = x_j$$

_Multiplying them together:_

$$\frac{\partial J}{\partial w_j} = \left[ \frac{f - y}{f(1-f)} \right] \cdot \left[ f(1-f) \right] \cdot \left[ x_j \right]$$

Notice how elegantly the $f(1-f)$ terms cancel out!

**The Final Gradient Result (Averaged over $m$ examples):**

$$\frac{\partial J}{\partial w_j} = \frac{1}{m} \sum_{i=1}^{m} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)}) x_j^{(i)}$$

$$\frac{\partial J}{\partial b} = \frac{1}{m} \sum_{i=1}^{m} (f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)})$$

**The Beautiful Connection to Linear Regression:**

Look at that formula. **It is identical in structure to the gradient for linear regression!**

- Linear Regression Error = (Linear Prediction) - (Target)
    
- Logistic Regression Error = (Predicted Probability) - (Target)
    

Even though the prediction function $f$ became highly non-linear via the sigmoid, the logistic loss function perfectly "undoes" the complexity during the derivative step, leaving us with a beautiful, simple gradient.

## 9. Gradient Descent for Logistic Regression

The update rules are exactly the same as linear regression:

$$w_j := w_j - \alpha \frac{\partial J}{\partial w_j}$$

$$b := b - \alpha \frac{\partial J}{\partial b}$$

**Explanation of terms:**

$w_j, b$: The parameters we are optimizing.

$\frac{\partial J}{\partial w_j}$: The gradient (slope). It tells us which direction makes the cost increase.

- **Minus sign ($-$)**: We subtract the gradient because we want to move _opposite_ to the slope to find the valley floor (minimization).
    

$\alpha$ (Learning Rate): Controls the step size. If $\alpha$ is too small, gradient descent takes too long. If $\alpha$ is too large, it can overshoot the minimum and diverge.

**What changes from your linear regression code?**

Almost nothing conceptually! You still calculate predictions, calculate errors, calculate gradients, and update.

- _Changes_: The prediction calculation now wraps $z$ in `sigmoid()`, and the cost function tracks Logistic Loss instead of MSE.
    
- _Stays the same_: The parameter update logic and gradient loops are structurally identical.
    

## 10. Logistic Regression in Vector/Matrix Form

To train efficiently, we avoid `for` loops by using matrix multiplication.

Let $\mathbf{X}$ be our feature matrix of shape $(m, n)$ — $m$ examples, $n$ features.

Let $\mathbf{w}$ be our weight vector of shape $(n, 1)$.

**Vectorized Prediction:**

$$\mathbf{z} = \mathbf{X}\mathbf{w} + b$$

- $\mathbf{X}\mathbf{w}$ is an $(m, 1)$ vector containing the raw scores for all $m$ examples simultaneously.
    

$$\mathbf{f} = \sigma(\mathbf{z})$$

- $\mathbf{f}$ is an $(m, 1)$ vector of all predicted probabilities.
    

**Vectorized Gradient:**

In code, the loop $\frac{1}{m} \sum (f^{(i)} - y^{(i)}) x_j^{(i)}$ becomes a simple dot product:

$$\text{error} = \mathbf{f} - \mathbf{y} \quad \text{...shape } (m, 1)$$

$$\nabla \mathbf{w} = \frac{1}{m} \mathbf{X}^T \cdot \text{error} \quad \text{...shape } (n, 1)$$

**NumPy Equivalents:**

- `z = X @ w + b` replaces the nested loops calculating $w_1x_1 + \dots + b$ for every row.
    
- `X.T @ error` replaces iterating over every training example $i$ and every feature $j$ to calculate the gradient sum. Mathematically, it multiplies every feature column by the errors and sums them instantly.
    
- `np.sum(error) / m` replaces the loop to find the bias gradient.
    

## 11. Complete Mental Model

**Training Pipeline:**

`X, y` (Data)

↓

`z = X @ w + b` (Linear combination)

↓

`sigmoid(z)` (Squash to 0-1)

↓

`predicted probabilities f`

↓

`logistic loss` (Compare f to true y)

↓

`average cost J` (Scalar value to monitor)

↓

`gradient` (X.T @ (f - y) / m)

↓

`update w, b` (w = w - alpha * grad)

↓

`repeat` (Loop until cost stops decreasing)

**Prediction Pipeline (In Production):**

`new x` (Unseen data)

↓

`w^T x + b` (Calculate raw score z)

↓

`sigmoid(z)` (Squash)

↓

`probability` (e.g., 0.85)

↓

`threshold` (Is p >= 0.5?)

↓

`class` (Predict 1)

## 12. Common Misconceptions

- **"Sigmoid itself is the probability."** -> False. Sigmoid is just a math function. It _outputs_ a probability only because we pass our linear log-odds model through it.
    
- **"The equation $\log(p/(1-p)) = \mathbf{w}^T\mathbf{x} + b$ is an identity."** -> False. It is a _modeling assumption_. We are choosing to assume the data's log-odds have a linear relationship.
    
- **"Logistic regression is just linear regression with a sigmoid added."** -> Conceptually close, but mathematically, changing the prediction to a sigmoid forced us to invent an entirely new cost function (Logistic Loss) to maintain convexity.
    
- **"The decision boundary is where the sigmoid becomes undefined."** -> False. The sigmoid is defined everywhere. The decision boundary is where the sigmoid outputs exactly 0.5 (which is where $z=0$).
    
- **"0.5 is mathematically the only possible classification threshold."** -> False. 0.5 is symmetric and standard, but in real life (e.g., detecting cancer), you might lower the threshold to 0.1 to be extremely cautious, trading more false positives for fewer false negatives.
    
- **"The weights represent probabilities."** -> False. The weights $\mathbf{w}$ represent how much one unit change in a feature affects the _log-odds_, not the probability directly.
    
- **"Logistic regression can only work with one feature."** -> False. Just like multiple linear regression, it scales to $n$ features via vectors.
    
- **"Classification means the model directly outputs 0 or 1."** -> False. The _model_ outputs a continuous probability between 0 and 1. We, the engineers, force it to 0 or 1 at the very end by applying a threshold.
    

## 13. Connection to Previous Weeks

**What is Reused:**

- **The Linear Engine:** $z = \mathbf{w}^T\mathbf{x} + b$ is identical.
    
- **Vectorization:** `X @ w` is exactly the same.
    
- **Feature Scaling (Z-score normalization):** Still crucial because gradient descent still struggles with elliptical contours.
    
- **Feature Engineering/Polynomials:** We can still create $x_1^2, x_1x_2$, etc., to create _non-linear decision boundaries_ (e.g., a circle) while keeping the model linear with respect to $\mathbf{w}$.
    

**What Changes:**

- **Output Transformation:**
    
    - _Linear:_ prediction = $z$
        
    - _Logistic:_ prediction = $\sigma(z)$
        
- **Cost Function:** MSE $\rightarrow$ Logistic Loss.
    
- **Interpretation:** Continuous target value $\rightarrow$ Probability of a class.
    

## 14. Worked Numerical Example

**1. Setup:**

- Features: $\mathbf{x} = [2, 3]$
    
- True Class: $y = 1$
    
- Current parameters: $\mathbf{w} = [0.5, -0.5]$, $b = 0.5$
    
- Learning Rate: $\alpha = 0.1$
    

**2. Calculate z:**

$z = \mathbf{w}^T\mathbf{x} + b = (0.5)(2) + (-0.5)(3) + 0.5 = 1 - 1.5 + 0.5 = 0$

**3. Calculate sigmoid(z):**

$p = \sigma(0) = \frac{1}{1 + e^0} = \frac{1}{2} = 0.5$

**4. Interpret & Predict Class:**

The model gives exactly a 50% probability of class 1.

Using threshold $\ge 0.5$, we predict Class 1.

**5. Calculate Loss:**

Since $y=1$, Loss $= -\log(p) = -\log(0.5) \approx 0.693$.

**6. Calculate Gradient Contribution:**

Error = $p - y = 0.5 - 1 = -0.5$

$\frac{\partial J}{\partial w_1} = \text{Error} \times x_1 = -0.5 \times 2 = -1.0$

$\frac{\partial J}{\partial w_2} = \text{Error} \times x_2 = -0.5 \times 3 = -1.5$

$\frac{\partial J}{\partial b} = \text{Error} = -0.5$

**7. Update Parameters:**

$w_1 := 0.5 - (0.1)(-1.0) = 0.5 + 0.1 = 0.6$

$w_2 := -0.5 - (0.1)(-1.5) = -0.5 + 0.15 = -0.35$

$b := 0.5 - (0.1)(-0.5) = 0.5 + 0.05 = 0.55$

_(Notice: the weights shifted in a direction that will make $z$ larger next time, which will increase the probability from 0.5 closer to the true label 1.)_

## 15. Key Formulas

- **Linear Combination:** $z = \mathbf{w}^T\mathbf{x} + b$
    
    _(The raw unbounded score, same as linear regression.)_
    
- **Sigmoid Function:** $\sigma(z) = \frac{1}{1 + e^{-z}}$
    
    _(Squashes any real number into the range $(0,1)$.)_
    
- **Log-odds (Logit) Definition:** $\text{logit}(p) = \log\left(\frac{p}{1-p}\right)$
    
    _(Mathematical definition linking probability to a $(-\infty, \infty)$ range.)_
    
- **Modeling Assumption:** $\log\left(\frac{p}{1-p}\right) = \mathbf{w}^T\mathbf{x} + b$
    
    _(The core assumption defining logistic regression: log-odds are linear.)_
    
- **Prediction Function:** $f_{\mathbf{w},b}(\mathbf{x}) = P(y=1) = \sigma(\mathbf{w}^T\mathbf{x} + b)$
    
    _(The algebraic result of the modeling assumption solved for $p$.)_
    
- **Decision Boundary:** $\mathbf{w}^T\mathbf{x} + b = 0$
    
    _(The threshold where the model is 50/50 uncertain.)_
    
- **Logistic Loss:** $L(f,y) = -y \log(f) - (1-y)\log(1-f)$
    
    _(The convex penalty function for a single training example.)_
    
- **Gradient w.r.t weights:** $\frac{\partial J}{\partial w_j} = \frac{1}{m} \sum (f_{\mathbf{w},b}(\mathbf{x}) - y) x_j$
    
    _(The slope of the cost surface indicating how to adjust $w_j$.)_
    
- **Gradient w.r.t bias:** $\frac{\partial J}{\partial b} = \frac{1}{m} \sum (f_{\mathbf{w},b}(\mathbf{x}) - y)$
    
    _(The slope of the cost surface indicating how to adjust $b$.)_
    
- **Gradient Descent:** $w_j := w_j - \alpha \frac{\partial J}{\partial w_j}$
    
    _(The step taking parameters down the cost slope toward the minimum.)_
    

## 16. Practice Questions

1. Why can't a linear model directly represent probabilities? What bounds does it violate?
    
2. Why do we use the concept of odds, and why do we take the natural logarithm of those odds?
    
3. Derive the sigmoid function algebraically starting from the assumption: $\log(\frac{p}{1-p}) = z$.
    
4. Why is predicting $p = 0.5$ mathematically equivalent to $z = 0$?
    
5. Derive the decision boundary formula. If $w_1 = 2, w_2 = -2, b = 0$, what does the boundary look like on a graph?
    
6. Calculate the sigmoid of $z = -3, z = 0, z = 3$ (roughly). What does it tell you?
    
7. Calculate the logistic loss for a prediction of $f = 0.9$ when the true class is $y = 0$. Now do it for $f = 0.1$. Explain the difference.
    
8. Why does substituting a sigmoid prediction into a Mean Squared Error cost function cause problems for gradient descent?
    
9. Explain the intuition of how the chain rule cancels out the complex parts of the sigmoid derivative to yield a simple gradient formula.
    
10. Perform one gradient descent update by hand using $x = [1, 2], y=0, w=[0, 0], b=0, \alpha=0.1$.
    
11. What is the fundamental difference between a probability prediction and a class prediction?
    
12. Conceptually, what changes when you convert your existing multiple linear regression code to logistic regression code? What stays the same?

[[Feature_Engineering_&_Polynomial_Regression]]
[[Decision_Boundary]]