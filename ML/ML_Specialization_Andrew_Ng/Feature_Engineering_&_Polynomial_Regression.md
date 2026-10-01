
## 1. Why Feature Engineering?

In a standard linear regression model with a single feature, the relationship is defined as:

$$f_{w,b}(x) = wx + b$$

When working with real-world problems, the original features (the raw data you collect) are not always the most effective way to represent the underlying patterns.

Imagine you are trying to predict the price of a house. Your dataset provides two features:

- $x_1 = \text{width of the lot}$
    
- $x_2 = \text{length of the lot}$
    

A standard multiple linear regression model would look like this:

$$f_{w,b}(x) = w_1x_1 + w_2x_2 + b$$

While width and length influence the price, buyers primarily care about the **total area** of the lot. The model above can only add the width and length together (weighted by $w_1$ and $w_2$). It cannot multiply them.

To give the model the information it actually needs, we can create a new feature by multiplying the existing ones:

$$x_3 = x_1x_2$$

Now, we can feed this new representation into our model:

$$f_{w,b}(x) = w_1x_1 + w_2x_2 + w_3x_3 + b$$

- **What is a feature?** A measurable property of the input data (e.g., width).
    
- **What is an engineered/derived feature?** A new property created by mathematically transforming or combining original features (e.g., area = width × length).
    
- **Why does this not change the basic algorithm?** To the gradient descent algorithm, $x_3$ is just a number. It does not know that $x_3$ was created by multiplying $x_1$ and $x_2$. It simply sees a third column of data and optimizes a third weight ($w_3$) for it.
    

## 2. Feature Engineering and the Meaning of "Linear"

This is one of the most important conceptual leaps in machine learning: distinguishing between what changes in the inputs versus what changes in the model's parameters.

You might wonder why a model using $x_3 = x_1x_2$ or $x^2$ is still called **Linear Regression**. The term "linear" in linear regression refers to being **linear in the parameters** (the weights), not necessarily linear in the original inputs.

Consider this model:

$$f(x) = w_1x + w_2x^2 + b$$

This model produces a curve, meaning it is non-linear with respect to the original input $x$. However, it is **linear with respect to the parameters $w_1$ and $w_2$**.

To understand the difference, compare it to this hypothetical equation:

$$f(x) = w_1^2x + b$$

In this second equation, the parameter $w_1$ is squared. This is fundamentally different and breaks the linear regression machinery, because the relationship between the weight and the output is no longer a simple flat multiplier.

**The Rule to Remember:**

If you can write the model's prediction as a sum of weights multiplied by features, plus a bias—regardless of how mathematically complex those features are—it is a linear model. The algorithm is just solving for $w$, and $w$ always acts as a simple, un-exponentiated scaling factor for whatever data it is attached to.

## 3. Polynomial Regression

Polynomial regression is a specific type of feature engineering motivated by the need to fit non-linear relationships.

If your data is curved (e.g., housing prices that flatten out at high square footages), a straight line will fail to capture the pattern:

$$f(x) = w_1x + b$$

To allow the model to bend, we can engineer new features by taking the original feature $x$ and raising it to higher powers:

$$f(x) = w_1x + w_2x^2 + b$$

$$f(x) = w_1x + w_2x^2 + w_3x^3 + b$$

Conceptually:

- Adding $x^2$ allows the model to form a parabola (one curve/bend).
    
- Adding $x^3$ allows the model to form an S-shape (two bends).
    

**The Mathematical Trick:**

We can use the exact same multiple linear regression machinery we already learned by mapping these polynomial terms to standard numbered features:

$$x_1 = x$$

$$x_2 = x^2$$

$$x_3 = x^3$$

Substituting these into our multiple linear regression formula yields:

$$f_{w,b}(x) = w_1x_1 + w_2x_2 + w_3x_3 + b$$

Because we have mapped the problem back to the standard format, **gradient descent does not fundamentally change**. The algorithm simply updates $w_1$, $w_2$, and $w_3$ based on the errors, entirely blind to the fact that $x_2$ and $x_3$ are exponents of $x_1$.

## 4. Step-by-Step Numerical Example

To prove that polynomial regression is just multiple linear regression in disguise, let's work through a calculation.

Suppose our original input is:

$$x = 2$$

We engineer a degree-3 polynomial representation. Our features become:

- $x_1 = x = 2$
    
- $x_2 = x^2 = 4$
    
- $x_3 = x^3 = 8$
    

Suppose gradient descent has already found the following parameters:

- $w_1 = 3$
    
- $w_2 = 2$
    
- $w_3 = -0.5$
    
- $b = 4$
    

Now we calculate the prediction step-by-step using $f(x) = w_1x_1 + w_2x_2 + w_3x_3 + b$:

1. Term 1: $3 \times 2 = 6$
    
2. Term 2: $2 \times 4 = 8$
    
3. Term 3: $-0.5 \times 8 = -4$
    
4. Bias: $4$
    

Add them together:

$$f(2) = 6 + 8 - 4 + 4 = 14$$

The model successfully evaluated a cubic function using basic dot-product arithmetic.

## 5. Polynomial Regression and Feature Scaling

When you use polynomial regression, feature scaling goes from being "helpful" to **absolutely mandatory**.

Imagine a house size is $x = 100$ (in square meters). If we use a degree-3 polynomial, our features are:

- $x_1 = 100$
    
- $x_2 = 10,000$
    
- $x_3 = 1,000,000$
    

These features have drastically different scales. To understand why this breaks gradient descent, look at the partial derivative used to update the weights:

$$\frac{\partial J}{\partial w_j} = \frac{1}{m} \sum_{i=1}^m (f_{w,b}(x^{(i)}) - y^{(i)}) x_j^{(i)}$$

The gradient (the size of the step we take) for a specific weight $w_j$ is directly multiplied by its corresponding feature value $x_j^{(i)}$.

- For $w_1$, the error is multiplied by 100.
    
- For $w_3$, the exact same error is multiplied by 1,000,000.
    

If you use a single learning rate $\alpha$ for all parameters, a step size that is perfectly safe for $w_1$ will cause $w_3$ to explode and diverge toward infinity because its gradient is artificially inflated by the massive feature values. Feature scaling brings all features down to a comparable range (usually between -1 and 1), ensuring the gradients are balanced and descent is smooth.

## 6. Mean Normalization / Standardization Connection

To scale polynomial features, we apply the exact same standardization techniques learned previously (Z-score normalization).

Crucially, **each engineered feature is treated as completely independent and gets its own statistics.** We do not scale $x_2$ using the mean of $x_1$.

For each column $j$ in our engineered dataset, we calculate its mean $\mu_j$ and standard deviation $\sigma_j$, then apply:

$$x_{j,\text{scaled}} = \frac{x_j - \mu_j}{\sigma_j}$$

- $x_1$ is scaled using $\mu_1$ and $\sigma_1$.
    
- $x_2$ is scaled using $\mu_2$ (the mean of the squared values) and $\sigma_2$ (the standard dev of the squared values).
    
- $x_3$ is scaled using $\mu_3$ and $\sigma_3$.
    

**Critical Implementation Rule:**

You must calculate $\mu_j$ and $\sigma_j$ strictly across your **training dataset**. When a new example arrives later for prediction, you must engineer its polynomial features, and then scale those features using the saved $\mu_j$ and $\sigma_j$ from the training phase. Do not recalculate the mean and standard deviation on new data.

## 7. Choosing Polynomial Degree

Adding polynomial features gives the model more flexibility, but more flexibility is not always better.

- **Degree 1 ($x$):** A straight line. If the data is inherently curved, this model will systematically miss the data points. It is **underfitting** (too simple).
    
- **Degree 2 ($x + x^2$):** A parabola. This often provides an appropriate, smooth curve that captures the general trend of the data without overreacting to individual quirks.
    
- **Degree 15 ($x + x^2 + \dots + x^{15}$):** A highly complex curve. It has so much flexibility that it will contort itself to pass perfectly through every single training data point, including the random noise. It will perform terribly on new, unseen data. This is **overfitting** (overly complicated).
    

As the degree increases, we add more weights ($w_1, w_2 \dots w_{15}$). We are fundamentally increasing the model's capacity to memorize the training data at the expense of its ability to generalize.

## 8. Feature Engineering vs. Polynomial Regression

It is important not to confuse the general strategy with the specific technique.

**Feature Engineering** is the broad, creative process of designing new inputs from existing data to make the learning algorithm's job easier.

- Examples: Multiplying width by length ($x_1x_2$), taking the ratio of debt to income ($\frac{x_1}{x_2}$), or extracting the month from a date.
    

**Polynomial Regression** is one specific mathematical flavor of feature engineering where the engineered features are strictly exponents of the original features ($x, x^2, x^3$). It is a tool inside the feature engineering toolbox.

## 9. Connection to Multiple Linear Regression

The progression of concepts you have learned forms a single, unified pipeline. Visually, the mathematical evolution looks like this:

1. **Ordinary linear regression:**
    

$$f(x) = wx + b$$

2. **Multiple linear regression (handling multiple distinct raw inputs):**
    

$$f(x_1, x_2) = w_1x_1 + w_2x_2 + b$$

3. **Feature engineering (creating a better representation, like area):**
    

$$x_3 = x_1x_2$$

4. **Multiple linear regression with the engineered feature:**
    

$$f(x_1, x_2, x_3) = w_1x_1 + w_2x_2 + w_3x_3 + b$$

5. **Polynomial regression (using exponents as the engineered features):**
    

$$x_2 = x^2, \quad x_3 = x^3$$

$$f(x) = w_1x + w_2x^2 + w_3x^3 + b$$

At every step, the final mathematical form remains a sum of weights multiplied by features.

## 10. NumPy Implementation

In Python, the transition to polynomial regression simply involves manipulating the input matrix `X` before feeding it into your existing algorithm.

Python

```
import numpy as np

# Assume X is a 1D array of original features (m examples)
X = np.array([1, 2, 3, 4]) 

# Create polynomial features up to degree 3
# np.column_stack takes 1D arrays and stacks them as columns in a 2D matrix
X_poly = np.column_stack((X, X**2, X**3))

print(X_poly)
# Output:
# [[ 1  1  1]
#  [ 2  4  8]
#  [ 3  9 27]
#  [ 4 16 64]]
```

Mathematically, we just transformed an $m \times 1$ vector into an $m \times 3$ matrix.

Once `X_poly` is created (and scaled), we pass it into our standard vectorized gradient descent update rule:

$$\mathbf{w} \leftarrow \mathbf{w} - \alpha \frac{1}{m} X^T(X\mathbf{w}+b-\mathbf{y})$$

**What changed:** The values inside the $X$ matrix, and consequently the number of elements in the $\mathbf{w}$ vector.

**What didn't change:** The gradient descent formula itself. The dot products work exactly the same way.

## 11. Important Shape Concepts

Understanding the dimensions (shapes) of your matrices is the best way to debug ML code. Let's trace how feature engineering alters shapes.

- $X \in \mathbb{R}^{m \times n}$ ($m$ examples, $n$ features)
    
- $w \in \mathbb{R}^n$ (one weight per feature)
    
- $y \in \mathbb{R}^m$ (one target label per example)
    

If we start with 2 original features ($x_1, x_2$), our initial matrix is:

$$X \in \mathbb{R}^{m \times 2}$$

And our weights are a vector of length 2.

If we engineer new features to capture non-linearities, such as $x_1^2$, $x_2^2$, and $x_1x_2$, we are adding 3 new columns to our data.

Our new matrix $X_{\text{new}}$ contains $[x_1, x_2, x_1^2, x_2^2, x_1x_2]$.

- The new shape of $X_{\text{new}}$ is $\mathbb{R}^{m \times 5}$.
    
- Because every column in $X$ requires a corresponding weight, the shape of the weight vector $w$ must automatically expand to $\mathbb{R}^5$.
    
- The target vector $y$ remains $\mathbb{R}^m$ because the number of training examples ($m$) has not changed.
    

## 12. Common Misconceptions

- **"Polynomial regression is a completely different algorithm from linear regression."**
    
    - _Correction:_ It is exactly the same algorithm. It just uses a dataset that has been mathematically expanded with exponents prior to training.
        
- **"If the model contains $x^2$, it is no longer a linear model."**
    
    - _Correction:_ It is non-linear in the input variables, but it is a linear model because it is **linear in the parameters** (the weights are just scaling multipliers).
        
- **"Feature engineering means manually changing the weights."**
    
    - _Correction:_ You never manually change weights. You change the input data features. The algorithm learns the optimal weights for the new features via gradient descent.
        
- **"We need a different gradient descent formula for polynomial regression."**
    
    - _Correction:_ The partial derivatives and update rules are identical. You just swap out the original $X$ matrix for the polynomial $X$ matrix.
        
- **"The model automatically knows which engineered features are useful."**
    
    - _Correction:_ The model doesn't "know" anything; if you give it a useless feature, gradient descent will try to drive that feature's weight $w_j$ toward zero, but providing too many useless features makes optimization harder and risks overfitting.
        
- **"Scaling all polynomial features using one mean and one standard deviation is always correct."**
    
    - _Correction:_ False. Every single column (feature) must have its own distinct $\mu_j$ and $\sigma_j$ calculated.
        
- **"Higher polynomial degree always means a better model."**
    
    - _Correction:_ Higher degrees reduce training error but eventually cause severe overfitting, making predictions on new data wildly inaccurate.
        

## 13. What I Should Actually Remember

- **Representation shift:** Feature engineering changes the data representation, not the underlying learning algorithm.
    
- **It's all Linear Regression:** Polynomial regression is simply multiple linear regression applied to engineered polynomial features.
    
- **Linearity:** "Linear model" means linear with respect to the learned parameters ($w$), not the inputs ($x$).
    
- **Scaling is critical:** Polynomial features explode in magnitude rapidly (e.g., $x$ vs $x^3$). Feature scaling is required to prevent gradients from diverging.
    
- **Independent statistics:** Each engineered feature gets its own distinct $\mu$ and $\sigma$ calculated from the training set, which must be stored and reused for future predictions.
    
- **The tradeoff:** Increasing model complexity (higher degree polynomials) allows fitting complex curves but invites overfitting to noise.
    

## 14. Practice Problems

Try to solve these on paper before checking the answers below.

**Problem 1:**

You have a model $f(x) = w_1x + w_2\sqrt{x} + b$. Is this model linear in its parameters? Why or why not?

**Problem 2:**

You are given two original features for a single training example: $x_1 = 3, x_2 = 4$. You decide to engineer features for a degree-2 polynomial, which includes the original features, their squares, and their product. List out the final feature vector you will pass to the model.

**Problem 3:**

You have a dataset with $m=500$ examples and $n=3$ original features ($x_1, x_2, x_3$). You engineer new features by adding all squared terms ($x_1^2, x_2^2, x_3^2$) and all pairwise products ($x_1x_2, x_1x_3, x_2x_3$). What is the exact shape of your new matrix $X$? What is the shape of your weight vector $w$?

**Problem 4:**

A model has been trained on a single feature $x$. The learned parameters are $w_1 = 2$, $w_2 = -0.5$, and $b = 1$. The model is a degree-2 polynomial regression model: $f(x) = w_1x + w_2x^2 + b$.

Calculate the model's prediction if the input is $x = 4$.

**Problem 5:**

You are training a degree-4 polynomial model on an input $x$. In your training data, $x$ ranges from 1 to 10. Without calculating the exact gradient, explain why gradient descent will struggle to optimize $w_4$ and $w_1$ simultaneously using a single learning rate $\alpha$ if you skip feature scaling.

**Problem 6:**

Using NumPy, write a one-line expression to create a matrix `X_engineered` that contains $x$, $x^2$, $x^3$, and $x^4$ given a 1D array `X = np.array([2, 3, 4])`.

  
  
  

### Answers to Practice Problems

**Answer 1:**

Yes, it is linear in its parameters. Even though $\sqrt{x}$ is a non-linear transformation of the input data, the parameters $w_1$ and $w_2$ simply act as flat scalar multipliers for those terms. It takes the form of a dot product.

**Answer 2:**

The new feature vector will have 5 terms: $[x_1, x_2, x_1^2, x_2^2, x_1x_2]$.

Calculating the values: $[3, 4, 9, 16, 12]$.

**Answer 3:**

The original matrix had 3 features. We added 3 squared terms and 3 pairwise product terms, making $3 + 3 + 3 = 9$ total features.

- The new shape of $X$ is $500 \times 9$.
    
- The shape of $w$ is a vector of length 9 (or $9 \times 1$).
    

**Answer 4:**

1. Map the features: $x_1 = 4$, $x_2 = 16$.
    
2. Apply the model: $f(4) = (2 \times 4) + (-0.5 \times 16) + 1$
    
3. Calculate: $f(4) = 8 - 8 + 1 = 1$.
    
    The final prediction is 1.
    

**Answer 5:**

Because $x$ reaches 10, $x_4$ ($x^4$) will reach 10,000. In the gradient descent update formula, the error is multiplied by the feature value to determine the gradient. The gradient for $w_4$ will be heavily inflated (roughly 1,000 times larger than the gradient for $w_1$ for the same error). A learning rate $\alpha$ small enough to keep $w_4$ from bouncing to infinity will make the update to $w_1$ so microscopically small that it will barely learn.

**Answer 6:**

Python

```
X_engineered = np.column_stack((X, X**2, X**3, X**4))
```


[[Feature_Scalling]]
[[Classification_&_Logistic_Regression]]