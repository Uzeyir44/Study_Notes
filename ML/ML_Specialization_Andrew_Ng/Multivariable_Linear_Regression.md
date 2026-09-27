
### Main Objective of the Lesson

The core objective of this lesson is to extend the linear regression model from handling a single input variable (feature) to handling multiple input variables simultaneously. By the end of this note, you will understand how to construct a mathematical model that combines several distinct pieces of information to produce a single, accurate numeric prediction.

### Why Single-Variable Linear Regression Is Often Insufficient

In single-variable linear regression, we attempt to predict a target variable $y$ using only one input feature $x$. For instance, we might try to predict a home's price using only its size in square feet:

```
[ Size (x) ] --------------------> [ Predicted Price (y) ]
```

While size is important, real-world outcomes are rarely governed by a single factor. Two houses with the exact same square footage might sell for vastly different prices if one is brand new and located in the center of a major city, while the other is 80 years old and located hours away from any urban hub.

If we force a model to look at only one feature, it suffers from severe limitations:

- **Underfitting:** The model misses crucial information, leading to high errors in its predictions.
    
- **Oversimplification:** It treats fundamentally different items as identical simply because they match on a single metric.
    

### How Multiple Features Improve Prediction Accuracy

Multiple Linear Regression resolves this limitation by giving the model access to a richer set of inputs:

```
[ Size (x₁) ] -------------\
[ Bedrooms (x₂) ] ----------\
[ Age (x₃) ] ----------------+---> [ Predicted Price (y) ]
[ Distance to Center (x₄) ] -/
```

By considering multiple features simultaneously, the model can:

1. **Isolate specific relationships:** It can learn how much size matters _while holding age and location constant_.
    
2. **Balance opposing factors:** It can recognize that a small house in a prime location might be worth as much as a large house in a distant suburb.
    
3. **Reduce overall prediction error:** Access to more relevant data allows the cost function to reach a lower minimum value.
    

## Motivation

### Real-World Examples Where Several Features Influence the Prediction

In almost every domain where predictive modeling is applied, multiple features are required to capture reality:

- **Healthcare (Predicting Patient Blood Pressure):** Age, weight, daily sodium intake, exercise hours per week, and resting heart rate.
    
- **Automotive (Predicting Fuel Efficiency):** Vehicle weight, engine horsepower, aerodynamic drag coefficient, and tire pressure.
    
- **Finance (Predicting Credit Risk):** Annual income, total existing debt, credit score, length of employment, and payment history.
    

### Example: House Price Prediction Using Multiple Characteristics

Consider four distinct properties in a real estate database:

1. A $1\text{,}500 \text{ sq ft}$ house with $3$ bedrooms, built $5$ years ago, $2 \text{ miles}$ from the city center.
    
2. A $1\text{,}500 \text{ sq ft}$ house with $2$ bedrooms, built $50$ years ago, $15 \text{ miles}$ from the city center.
    
3. A $2\text{,}800 \text{ sq ft}$ house with $4$ bedrooms, built $1$ year ago, $8 \text{ miles}$ from the city center.
    
4. A $900 \text{ sq ft}$ house with $1$ bedroom, built $20$ years ago, $0.5 \text{ miles}$ from the city center.
    

If a model only evaluated square footage, Properties 1 and 2 would receive the exact same price prediction, despite Property 1 being significantly newer and much closer to the city. Multiple Linear Regression allows us to assign unique weights to size, bedrooms, age, and distance, producing far more realistic estimates.

### Why Machine Learning Models Benefit from More Information

Machine learning models learn patterns by mapping inputs to outputs. Supplying additional relevant features increases the information density of the dataset. Mathematically, extra features allow the learning algorithm to operate in a higher-dimensional decision space, creating a nuanced mathematical function that fits the true underlying distribution of the data much more closely.

## Review of Single-Variable Linear Regression

Before introducing multiple features, let us recap the foundational building blocks from single-variable linear regression.

### Summary of Core Components

- **Feature ($x$):** The single input variable used to make a prediction (e.g., size in square feet).
    
- **Target ($y$):** The actual output or ground-truth label we are trying to predict (e.g., actual selling price).
    
- **Weight ($w$):** The parameter that controls the scale or impact of the feature $x$ on the output. It represents the slope of the regression line.
    
- **Bias ($b$):** The base parameter representing the output value when the input $x$ is equal to zero. It represents the $y$-intercept of the regression line.
    
- **Prediction Function:**
    
    $$f_{w,b}(x) = wx + b$$
    
    This function takes a single number $x$, multiplies it by the weight $w$, and adds the bias $b$ to calculate a predicted output $\hat{y}$.
    
- **Cost Function (Mean Squared Error):**
    
    $$J(w,b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{w,b}(x^{(i)}) - y^{(i)} \right)^2$$
    
    This measures the average squared difference between what our model predicted ($f_{w,b}(x^{(i)})$) and what the actual targets ($y^{(i)}$) were across all $m$ training examples.
    
- **Gradient Descent:** An optimization algorithm that iteratively adjusts parameters $w$ and $b$ in the direction of steepest descent to minimize the cost function $J(w,b)$:
    
    $$w = w - \alpha \frac{\partial J(w,b)}{\partial w}$$
    
    $$b = b - \alpha \frac{\partial J(w,b)}{\partial b}$$
    

### Extension to Multiple Linear Regression

Multiple linear regression **does not replace** these foundational concepts; it simply expands them:

- Instead of **one weight** ($w$), we will have **a list of weights** ($w_1, w_2, \dots, w_n$), one for each feature.
    
- Instead of **one feature** ($x$), we will have **a list of features** ($x_1, x_2, \dots, x_n$).
    
- The **bias** ($b$) remains a single scalar value representing the baseline offset.
    
- The **cost function** and **gradient descent algorithm** maintain their structural logic, but now update all $n$ weights simultaneously alongside the bias.
    

## What is Multiple Linear Regression?

### Definition

**Multiple Linear Regression** is a supervised learning algorithm that models the linear relationship between two or more input features and a continuous target variable.

### Core Intuition

Imagine you are estimating the value of a used car. You start with a base value in your head—say, $\$5\text{,}000$. This is your **bias ($b$)**.

Next, you look at individual characteristics and adjust your estimate:

- **Odometer Reading:** Subtract $\$0.05$ for every mile driven.
    
- **Engine Size:** Add $\$1\text{,}000$ for every liter of displacement.
    
- **Previous Owners:** Subtract $\$500$ for every prior owner.
    

Your final mental calculation looks like this:

$$\text{Estimated Price} = \$5\text{,}000 + (-0.05 \times \text{miles}) + (1000 \times \text{engine size}) + (-500 \times \text{owners})$$

Multiple linear regression automates this exact process. The algorithm searches through your historical training data to find the optimal numbers to use for the baseline value and each individual modifier.

### Difference Between One Feature and Many Features

|**Aspect**|**Single Variable Regression**|**Multiple Linear Regression**|
|---|---|---|
|**Number of Features ($n$)**|$n = 1$|$n > 1$|
|**Parameters**|1 weight ($w$), 1 bias ($b$)|$n$ weights ($w_1, \dots, w_n$), 1 bias ($b$)|
|**Mathematical Shape**|A 2D line|A 3D plane or higher-dimensional hyperplane|
|**Input Representation**|Scalar number $x$|Feature vector $\mathbf{x}$|

### Why Each Feature Receives Its Own Weight

Every feature measures something different, and different features have different units and levels of importance:

- **Feature units differ:** Square footage might range from $500$ to $5\text{,}000$, whereas the number of bedrooms ranges from $1$ to $5$. If both used the same weight, a $1$-unit change in bedrooms would have the exact same effect as a $1$-unit change in square feet!
    
- **Feature impacts differ:** An extra square foot adds a moderate amount of value, whereas an extra bathroom might add a substantial lump sum. Giving each feature its own dedicated weight ($w_j$) allows the model to scale and weigh every feature independently.
    

## Dataset Representation

To build machine learning models effectively, we must organize data into standard mathematical structures.

### Vocabulary & Notation

- $m$: The total number of **training examples** (the number of rows in your dataset).
    
- $n$: The total number of **features** (the number of input columns in your dataset).
    
- $x_j$: The $j^{\text{th}}$ feature in a generic data entry ($j$ ranges from $1$ to $n$).
    
- $\mathbf{x}^{(i)}$: The **feature vector** containing all the input values for the $i^{\text{th}}$ training example (the $i^{\text{th}}$ row).
    
- $x_j^{(i)}$: The specific value of the $j^{\text{th}}$ feature in the $i^{\text{th}}$ training example (row $i$, column $j$).
    
- $y^{(i)}$: The actual target value (ground-truth output) for the $i^{\text{th}}$ training example.
    

> **Notation Check:** The superscript $(i)$ in parentheses refers to the **index of the training example (row)**. It is NOT an exponent. The subscript $j$ refers to the **index of the feature (column)**.

### Layout of a Dataset

A dataset with $m$ rows and $n$ input columns is structured as follows:

|**Example Index**|**Size (x1​)**|**Bedrooms (x2​)**|**Age (x3​)**|**Distance (x4​)**|**Price (y) [Target]**|
|---|---|---|---|---|---|
|**Example 1 ($i=1$)**|$1800$|$3$|$10$|$5$|$\$400\text{,}000$|
|**Example 2 ($i=2$)**|$1200$|$2$|$25$|$12$|$\$250\text{,}000$|
|**Example 3 ($i=3$)**|$2500$|$4$|$2$|$3$|$\$620\text{,}000$|

From this table:

- $m = 3$ (there are 3 rows/examples).
    
- $n = 4$ (there are 4 features).
    
- $x_1^{(2)} = 1200$ (Row 2, Column 1: Size of 2nd house).
    
- $x_3^{(1)} = 10$ (Row 1, Column 3: Age of 1st house).
    
- $y^{(3)} = 620000$ (Target price of 3rd house).
    
- $\mathbf{x}^{(2)} = \begin{bmatrix} 1200 & 2 & 25 & 12 \end{bmatrix}$ (Entire feature vector for 2nd house).
    

## Matrix Representation

Instead of dealing with lists of isolated numbers, linear algebra lets us pack our entire dataset into organized mathematical grids called **matrices** and **vectors**.

### The Feature Matrix ($X$)

The **Feature Matrix** (denoted by a capital letter $X$) is a rectangular grid that collects all input features across every single training example.

$$X = \begin{bmatrix} x_1^{(1)} & x_2^{(1)} & \cdots & x_n^{(1)} \\ x_1^{(2)} & x_2^{(2)} & \cdots & x_n^{(2)} \\ \vdots & \vdots & \ddots & \vdots \\ x_1^{(m)} & x_2^{(m)} & \cdots & x_n^{(m)} \end{bmatrix}$$

#### Structural Rules of $X$:

- **Matrix Dimensions:** The matrix has $m$ rows and $n$ columns, written as an $(m \times n)$ matrix.
    
- **Every Row is a Training Example:** Row $i$ is equal to $(\mathbf{x}^{(i)})^T$, representing all features for example $i$.
    
- **Every Column is a Specific Feature:** Column $j$ contains the values of feature $j$ across all $m$ training examples.
    

### The Target Vector ($\mathbf{y}$)

The actual output values $y^{(1)}, y^{(2)}, \dots, y^{(m)}$ are arranged into a single column vector $\mathbf{y}$ of size $(m \times 1)$:

$$\mathbf{y} = \begin{bmatrix} y^{(1)} \\ y^{(2)} \\ \vdots \\ y^{(m)} \end{bmatrix}$$

### Concrete Numerical Example

Let us take a small dataset with $m=3$ homes and $n=2$ features (Size in sq ft, Bedrooms):

- House 1: $1500$ sq ft, $3$ beds, Price = $\$300\text{,}000$
    
- House 2: $2000$ sq ft, $4$ beds, Price = $\$400\text{,}000$
    
- House 3: $1100$ sq ft, $2$ beds, Price = $\$210\text{,}000$
    

We represent this dataset in matrix notation as:

$$X = \begin{bmatrix} 1500 & 3 \\ 2000 & 4 \\ 1100 & 2 \end{bmatrix}_{3 \times 2}, \quad \mathbf{y} = \begin{bmatrix} 300000 \\ 400000 \\ 210000 \end{bmatrix}_{3 \times 1}$$

## The Prediction Function

### Plain English Explanation

To calculate a prediction for a single house using multiple features, we multiply each individual feature value by its corresponding weight parameter, sum all those products together, and add a single baseline offset (the bias).

### Mathematical Equation

For a single training example with $n$ features, the prediction function is written as:

$$f_{w_1, w_2, \dots, w_n, b}(x_1, x_2, \dots, x_n) = w_1 x_1 + w_2 x_2 + \dots + w_n x_n + b$$

Using vector notation, we compress this entire equation into a compact form:

$$f_{\mathbf{w},b}(\mathbf{x}) = \mathbf{w}^T \mathbf{x} + b$$

### Detailed Breakdown of Every Symbol

- $f_{\mathbf{w},b}(\mathbf{x})$: The estimated output (also denoted as $\hat{y}$) produced by the model given the input features $\mathbf{x}$ and parameters $\mathbf{w}$ and $b$.
    
- $\mathbf{w}$: The **weight vector**, a column vector of dimension $(n \times 1)$ containing $n$ real numbers:
    
    $$\mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \\ \vdots \\ w_n \end{bmatrix}$$
    
- $\mathbf{w}^T$: The **transpose** of the weight vector, turning the column vector into a row vector of dimension $(1 \times n)$:
    
    $$\mathbf{w}^T = \begin{bmatrix} w_1 & w_2 & \dots & w_n \end{bmatrix}$$
    
- $\mathbf{x}$: The **feature vector** for a single example, stored as a column vector of dimension $(n \times 1)$:
    
    $$\mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix}$$
    
- $\mathbf{w}^T \mathbf{x}$: The **dot product** between the weight vector and the feature vector, which yields a single scalar value.
    
- $b$: The **bias parameter**, a single scalar number representing the base prediction when all features $\mathbf{x}$ equal zero.
    

### Step-by-Step Prediction Process

When evaluating a single input vector $\mathbf{x} = \begin{bmatrix} 1500 & 3 & 10 \end{bmatrix}^T$:

1. **Retrieve parameters:** Suppose our trained model has $\mathbf{w} = \begin{bmatrix} 0.1 & 20 & -1.5 \end{bmatrix}^T$ and $b = 80$.
    
2. **Pair features with weights:**
    
    - Feature 1 ($1500$) $\times$ Weight 1 ($0.1$) $= 150$
        
    - Feature 2 ($3$) $\times$ Weight 2 ($20$) $= 60$
        
    - Feature 3 ($10$) $\times$ Weight 3 ($-1.5$) $= -15$
        
3. **Sum products:** $150 + 60 + (-15) = 195$.
    
4. **Add bias ($b$):** $195 + 80 = 275$.
    
5. **Final Output:** The predicted value $f_{\mathbf{w},b}(\mathbf{x}) = 275$.
    

## Understanding the Dot Product

### Intuitive Explanation

In linear algebra, a **dot product** takes two sequences of numbers of equal length, multiplies corresponding entries together one by one, and sums up all the resulting products into a single number.

In machine learning, you can think of the dot product as a **weighted voting mechanism**:

- The **feature vector ($\mathbf{x}$)** represents _how much of each quality_ an item possesses.
    
- The **weight vector ($\mathbf{w}$)** represents _how much value or importance_ your model assigns to each of those qualities.
    
- The **dot product ($\mathbf{w}^T \mathbf{x}$)** calculates the _total score_ produced by combining all features according to their importance.
    

### Algebraic Definition

Given two vectors $\mathbf{w}$ and $\mathbf{x}$ with $n$ elements each:

$$\mathbf{w}^T \mathbf{x} = \sum_{j=1}^{n} w_j x_j = w_1 x_1 + w_2 x_2 + \dots + w_n x_n$$

### Why Every Feature Contributes Separately

Because addition is commutative and associative, the total prediction is formed by simply adding up the isolated contributions of each feature:

$$\text{Total Prediction} = \underbrace{(w_1 x_1)}_{\text{Contribution 1}} + \underbrace{(w_2 x_2)}_{\text{Contribution 2}} + \dots + \underbrace{(w_n x_n)}_{\text{Contribution } n} + \underbrace{b}_{\text{Baseline}}$$

This separation means that changing feature $x_1$ changes the final prediction by $w_1 \times \Delta x_1$, regardless of the current values of $x_2$ or $x_3$.

### What Weight Signs and Magnitudes Mean

- **Positive Weight ($w_j > 0$):** As $x_j$ increases, the prediction $\hat{y}$ **increases**. (e.g., larger house size increases house price).
    
- **Negative Weight ($w_j < 0$):** As $x_j$ increases, the prediction $\hat{y}$ **decreases**. (e.g., higher house age decreases house price).
    
- **Weight of Zero ($w_j = 0$):** The feature $x_j$ has **zero effect** on the prediction.
    
- **Large Magnitude ($\vert{}w_j\vert{}$ is large):** Small changes in $x_j$ lead to dramatic swings in the prediction.
    

### Numerical Example of a Dot Product

Let $\mathbf{w} = \begin{bmatrix} 2.5 \\ -0.8 \\ 0.0 \\ 4.1 \end{bmatrix}$ and $\mathbf{x} = \begin{bmatrix} 10 \\ 5 \\ 100 \\ 2 \end{bmatrix}$.

$$\mathbf{w}^T \mathbf{x} = (2.5 \times 10) + (-0.8 \times 5) + (0.0 \times 100) + (4.1 \times 2)$$

$$\mathbf{w}^T \mathbf{x} = 25 - 4.0 + 0 + 8.2 = 29.2$$

## Visual Intuition

### 1. One Feature vs. Multiple Features

#### Single-Feature Case ($n=1$)

With one feature $x_1$ and target $y$, we plot data on a 2D graph. The model fits a **1D line** through the data space.

```
       y (Price)
       ^
       |          /  (Regression Line: y = w1*x1 + b)
       |        * /
       |       / *
       |     * /
       |    / *
       |  * /
       +-------------------> x1 (Size)
```

#### Two-Feature Case ($n=2$)

With two features ($x_1, x_2$) and target $y$, data lives in a 3D coordinate system. The model fits a **2D flat surface (a plane)** through the data points.

```
          y (Price)
          ^
          |      /-----------/  <-- Fitting Plane: y = w1*x1 + w2*x2 + b
          |     /   *       /
          |    /     *     /
          |   /___________/
          |  * 
          +--------------------> x1 (Size)
         /
        /
       v  x2 (Bedrooms)
```

### 2. High-Dimensional Data & The Concept of a Hyperplane

What happens when we have $n=4, 10, \text{ or } 1000$ features?

- We cannot visually display more than 3 dimensions on a screen.
    
- However, the mathematical geometry scales completely seamlessly!
    

In general geometry:

- In 2D space ($n=1$ feature + target), a linear model is a **Line** (1-dimensional object).
    
- In 3D space ($n=2$ features + target), a linear model is a **Plane** (2-dimensional object).
    
- In $n\text{D}$ space ($n$ features + target), a linear model is called a **Hyperplane** (an $(n)$-dimensional flat surface embedded within an $(n+1)$-dimensional space).
    

You do not need to visualize higher dimensions mentally. Simply remember that a **hyperplane** is the higher-dimensional equivalent of a flat sheet or line—it represents a smooth, flat boundary without curves or bends.

## Cost Function for Multiple Linear Regression

To train our model, we need a mathematical formula that measures how well (or poorly) our current weights $\mathbf{w}$ and bias $b$ are performing across the entire dataset.

### Formula

The Cost Function $J(\mathbf{w}, b)$ uses the **Mean Squared Error (MSE)** formulation:

$$J(\mathbf{w},b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right)^2$$

Substituting the dot product form of $f_{\mathbf{w},b}(\mathbf{x}^{(i)})$:

$$J(\mathbf{w},b) = \frac{1}{2m} \sum_{i=1}^{m} \left( (\mathbf{w}^T \mathbf{x}^{(i)} + b) - y^{(i)} \right)^2$$

### Symbol Explanation

- $J(\mathbf{w},b)$: The cost (error value) associated with a specific choice of parameters $\mathbf{w}$ and $b$. Lower values mean a better fit.
    
- $m$: The total number of training examples in the dataset.
    
- $\frac{1}{2m}$: Scaled factor where $m$ averages the squared errors across examples, and the factor of $2$ in the denominator cancels out a 2 during calculus differentiation.
    
- $\sum_{i=1}^{m}$: The summation symbol, instructing us to compute the error for each example from $i=1$ up to $i=m$ and add them all together.
    
- $\mathbf{x}^{(i)}$: The feature vector of the $i^{\text{th}}$ training example.
    
- $\mathbf{w}^T \mathbf{x}^{(i)} + b$: The model's prediction for example $i$.
    
- $y^{(i)}$: The true target value for example $i$.
    
- $(\dots)^2$: The squaring operation. Squaring ensures that positive errors and negative errors do not cancel each other out, and penalizes larger errors much more heavily than smaller errors.
    

### What Changed from Single Variable Regression?

- **Structurally, the cost function is identical!** It measures the average squared vertical distance between predictions and targets.
    
- **The only internal change** is how predictions are made: instead of computing $w x^{(i)} + b$, we now compute the vector dot product $\mathbf{w}^T \mathbf{x}^{(i)} + b$.
    

## Gradient Descent with Multiple Variables

### Intuition

Gradient Descent is an optimization algorithm that finds parameter values that minimize the cost function $J(\mathbf{w},b)$. Imagine standing on a multi-dimensional surface (a bowl-shaped function) while blindfolded. To reach the lowest point (minimum cost), you feel the slope of the terrain under your feet in every direction and take a small step downward.

Since we now have $n$ weights ($w_1, w_2, \dots, w_n$) and $1$ bias ($b$), we must evaluate the slope (slope is measured using **partial derivatives**) along every individual parameter direction simultaneously.

### Simultaneous Update Equations

For every step of gradient descent, we update all parameters at the exact same moment:

$$\text{Repeat until convergence \{} \quad$$

$$w_j = w_j - \alpha \frac{\partial J(\mathbf{w},b)}{\partial w_j} \quad \text{for } j = 1, 2, \dots, n$$

$$b = b - \alpha \frac{\partial J(\mathbf{w},b)}{\partial b}$$

$$\quad \}$$

### Calculated Partial Derivatives

When we evaluate the calculus derivatives of $J(\mathbf{w},b)$, the update rules expand into the following concrete formulas:

$$w_j = w_j - \alpha \frac{1}{m} \sum_{i=1}^{m} \left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right) x_j^{(i)} \quad \text{for } j = 1, \dots, n$$

$$b = b - \alpha \frac{1}{m} \sum_{i=1}^{m} \left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right)$$

### Complete Breakdown of Symbols

- $\alpha$ (Alpha): The **learning rate**, a small positive scalar constant (e.g., $0.01$) that dictates how large a step we take during each iteration.
    
- $\left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right)$: The **prediction error** for example $i$ (Predicted value minus Actual value).
    
- $x_j^{(i)}$: The actual numeric value of feature $j$ for example $i$. This acts as a scaling term: features with larger magnitude inputs generate larger gradients for their corresponding weights.
    
- $\frac{1}{m} \sum_{i=1}^{m}$: Averages the error-feature product across all $m$ training examples.
    

## Worked Numerical Example

Let us work through a complete numeric calculation by hand using a tiny, synthetic dataset.

### Dataset ($m=2$ examples, $n=2$ features)

- **Example 1 ($i=1$):** Size $x_1^{(1)} = 1$, Bedrooms $x_2^{(1)} = 2$. Target Price $y^{(1)} = 300$ (in thousands).
    
- **Example 2 ($i=2$):** Size $x_1^{(2)} = 2$, Bedrooms $x_2^{(2)} = 1$. Target Price $y^{(2)} = 350$ (in thousands).
    

### Initial Parameter Guess

Let's assume our model starts with initial weights and bias set to:

$$\mathbf{w} = \begin{bmatrix} w_1 \\ w_2 \end{bmatrix} = \begin{bmatrix} 50 \\ 30 \end{bmatrix}, \quad b = 100$$

### Step 1: Manual Prediction for Both Examples

#### Prediction for Example 1 ($\mathbf{x}^{(1)} = \begin{bmatrix} 1 & 2 \end{bmatrix}^T$):

$$f_{\mathbf{w},b}(\mathbf{x}^{(1)}) = w_1 x_1^{(1)} + w_2 x_2^{(1)} + b$$

$$f_{\mathbf{w},b}(\mathbf{x}^{(1)}) = (50 \times 1) + (30 \times 2) + 100 = 50 + 60 + 100 = 210$$

#### Prediction for Example 2 ($\mathbf{x}^{(2)} = \begin{bmatrix} 2 & 1 \end{bmatrix}^T$):

$$f_{\mathbf{w},b}(\mathbf{x}^{(2)}) = w_1 x_1^{(2)} + w_2 x_2^{(2)} + b$$

$$f_{\mathbf{w},b}(\mathbf{x}^{(2)}) = (50 \times 2) + (30 \times 1) + 100 = 100 + 30 + 100 = 230$$

### Step 2: Manual Cost Calculation ($J(\mathbf{w}, b)$)

1. **Calculate Errors:**
    
    - Error 1: $f_{\mathbf{w},b}(\mathbf{x}^{(1)}) - y^{(1)} = 210 - 300 = -90$
        
    - Error 2: $f_{\mathbf{w},b}(\mathbf{x}^{(2)}) - y^{(2)} = 230 - 350 = -120$
        
2. **Square the Errors:**
    
    - Squared Error 1: $(-90)^2 = 8100$
        
    - Squared Error 2: $(-120)^2 = 14400$
        
3. **Sum and Scale by $\frac{1}{2m}$ ($m=2$, so $\frac{1}{2m} = \frac{1}{4} = 0.25$):**
    
    $$J(\mathbf{w},b) = \frac{1}{4} \times (8100 + 14400) = \frac{1}{4} \times (22500) = 5625$$
    

The current baseline total cost is **$5625$**.

### Step 3: Compute One Step of Gradient Descent

Let learning rate $\alpha = 0.1$.

#### 1. Derivative for $w_1$:

$$\frac{\partial J}{\partial w_1} = \frac{1}{m} \sum_{i=1}^{m} \text{Error}^{(i)} \cdot x_1^{(i)}$$

$$\frac{\partial J}{\partial w_1} = \frac{1}{2} \left( (-90 \times 1) + (-120 \times 2) \right) = \frac{1}{2} (-90 - 240) = \frac{-330}{2} = -165$$

#### 2. Derivative for $w_2$:

$$\frac{\partial J}{\partial w_2} = \frac{1}{m} \sum_{i=1}^{m} \text{Error}^{(i)} \cdot x_2^{(i)}$$

$$\frac{\partial J}{\partial w_2} = \frac{1}{2} \left( (-90 \times 2) + (-120 \times 1) \right) = \frac{1}{2} (-180 - 120) = \frac{-300}{2} = -150$$

#### 3. Derivative for $b$:

$$\frac{\partial J}{\partial b} = \frac{1}{m} \sum_{i=1}^{m} \text{Error}^{(i)}$$

$$\frac{\partial J}{\partial b} = \frac{1}{2} \left( -90 + (-120) \right) = \frac{-210}{2} = -105$$

#### 4. Update Parameters:

$$w_1 = w_1 - \alpha \left(\frac{\partial J}{\partial w_1}\right) = 50 - 0.1(-165) = 50 + 16.5 = 66.5$$

$$w_2 = w_2 - \alpha \left(\frac{\partial J}{\partial w_2}\right) = 30 - 0.1(-150) = 30 + 15.0 = 45.0$$

$$b = b - \alpha \left(\frac{\partial J}{\partial b}\right) = 100 - 0.1(-105) = 100 + 10.5 = 110.5$$

**Result after 1 step:** All parameters ($w_1, w_2, b$) increased because our predictions were too low (errors were negative). Higher weights will increase predictions during the next iteration, successfully driving down total cost!

## Practical Example: Real Estate Prediction

Let us analyze a fully trained model predicting home prices based on 4 concrete features:

$$f_{\mathbf{w},b}(\mathbf{x}) = 0.15 x_1 + 45.0 x_2 - 2.5 x_3 - 8.0 x_4 + 120$$

Where:

- $x_1 = \text{Size in square feet}$
    
- $x_2 = \text{Number of bedrooms}$
    
- $x_3 = \text{Age of home in years}$
    
- $x_4 = \text{Distance to city center in miles}$
    
- Target $y = \text{Price in thousands of dollars}$
    

### Interpreting the Weight Parameters

1. **$w_1 = 0.15$ ($+\$150$ per sq ft):** Holding all other factors equal, adding $1$ extra square foot increases the home's value by $\$0.15$ thousand ($\$150$).
    
2. **$w_2 = 45.0$ ($+\$45\text{,}000$ per bedroom):** Holding size, age, and distance constant, adding an extra bedroom adds $\$45\text{,}000$ in value.
    
3. **$w_3 = -2.5$ ($-\$2\text{,}500$ per year of age):** For every year a house gets older, its value drops by $\$2\text{,}500$.
    
4. **$w_4 = -8.0$ ($-\$8\text{,}000$ per mile of distance):** For every mile further away from the city center, the price decreases by $\$8\text{,}000$.
    
5. **$b = 120$ ($\$120\text{,}000$ base offset):** The baseline value of a theoretical house where size, bedrooms, age, and distance are all $0$.
    

## Connection to Linear Algebra

If you are currently taking an introductory course in **Linear Algebra**, here is how this machine learning lesson directly connects to your coursework:

### 1. Vector Spaces & Dimensions

In Linear Algebra, an $n$-dimensional vector space $\mathbb{R}^n$ represents an ordered tuple of $n$ real numbers. In Machine Learning:

- Every single feature vector $\mathbf{x}^{(i)}$ is simply a point in an $n$-dimensional vector space $\mathbb{R}^n$.
    
- The weight vector $\mathbf{w}$ is also a vector in $\mathbb{R}^n$.
    

### 2. Linear Combinations & Dot Products

The core equation of Multiple Linear Regression is a **linear combination** of the components of $\mathbf{x}$, plus a constant offset:

$$\mathbf{w}^T \mathbf{x} = \langle \mathbf{w}, \mathbf{x} \rangle$$

The dot product measures geometric alignment. If vectors point in similar directions, their dot product is large and positive.

### 3. Matrix-Vector Multiplication for Mass Predictions

Suppose we want to calculate predictions for ALL $m$ training examples at the exact same time instead of using a slow computer `for`-loop.

Using the Feature Matrix $X$ of size $(m \times n)$ and Weight Vector $\mathbf{w}$ of size $(n \times 1)$:

$$\mathbf{\hat{y}} = X \mathbf{w} + b$$

Let's inspect the dimensions:

$$(m \times 1) = (m \times n) \times (n \times 1) + (1 \times 1)$$

Row $i$ of the resulting column vector $\mathbf{\hat{y}}$ will contain $\mathbf{w}^T \mathbf{x}^{(i)} + b$. Expressing code as matrix operations is called **vectorization**, and it runs hundreds of times faster on modern CPU/GPU hardware.

## Connection to Calculus

If you are currently taking a course in **Multivariable Calculus**, here is how the optimization logic aligns with your mathematical training:

### 1. Partial Derivatives

Because our cost function $J(w_1, w_2, \dots, w_n, b)$ depends on multiple independent variables, we cannot use a simple derivative $\frac{dJ}{dw}$. Instead, we compute **partial derivatives** ($\frac{\partial J}{\partial w_j}$), which measure the rate of change of $J$ with respect to $w_j$ while holding all other parameters fixed.

### 2. The Gradient Vector

The **Gradient** of $J$ (written as $\nabla J$) is a vector collecting all first-order partial derivatives:

$$\nabla_{\mathbf{w}} J = \begin{bmatrix} \frac{\partial J}{\partial w_1} \\ \frac{\partial J}{\partial w_2} \\ \vdots \\ \frac{\partial J}{\partial w_n} \end{bmatrix}$$

A fundamental theorem of multivariable calculus states that **the gradient vector $\nabla J$ points in the direction of steepest local ascent** on a multivariable function surface.

### 3. Gradient Descent as Vector Subtraction

Because $\nabla J$ points uphill toward maximum error, taking the negative of the gradient ($-\nabla J$) guarantees we step downhill in the direction of steepest _descent_:

$$\mathbf{w}_{\text{new}} = \mathbf{w}_{\text{old}} - \alpha \nabla_{\mathbf{w}} J$$

Gradient descent is simply vector subtraction applied repeatedly in multivariable calculus space!

## Advantages of Multiple Linear Regression

1. **Significantly Higher Predictive Accuracy:** By incorporating all available relevant inputs, errors are reduced compared to single-feature models.
    
2. **Interpretability & Transparency:** Unlike "black-box" models (e.g., deep neural networks), linear regression parameters are completely transparent. You can inspect $w_j$ to understand precisely how much weight the model places on each factor.
    
3. **Computational Efficiency:** Training via gradient descent or analytical solutions is exceptionally fast and scales well to millions of rows.
    

## Limitations

1. **Sensitivity to Feature Scaling:** If $x_1$ ranges from $0$ to $1$ and $x_2$ ranges from $0$ to $1\text{,}000\text{,}000$, gradient descent will oscillate wildly and converge extremely slowly. _(Note: This introduces the critical need for Feature Scaling techniques like Z-score normalization)._
    
2. **Multicollinearity (Correlated Features):** If two input features are strongly correlated (e.g., house size in sq ft and house size in sq meters), the model struggles to isolate individual weights, causing parameters to become unstable.
    
3. **Assumption of Linearity:** Multiple linear regression assumes the relationship between features and the target is additive and straight. If the real-world process involves complex curves or non-linear interactions, basic linear regression will underfit.
    

## Key Terminology

- **Multiple Linear Regression:** A linear approach to modeling the relationship between multiple scalar input variables ($x_1, \dots, x_n$) and a continuous output variable ($y$).
    
- **Feature ($x_j$):** An individual measurable property or variable of a domain process.
    
- **Feature Vector ($\mathbf{x}$):** An ordered tuple containing all $n$ features for a single training example.
    
- **Weight Vector ($\mathbf{w}$):** A vector of $n$ learnable parameters where $w_j$ dictates the influence of feature $x_j$.
    
- **Target ($y$):** The ground-truth value we are attempting to predict.
    
- **Dot Product ($\mathbf{w}^T \mathbf{x}$):** The sum of the pairwise products of two equal-length vectors.
    
- **Feature Matrix ($X$):** An $(m \times n)$ grid storing all input features across all $m$ training examples.
    
- **Training Example:** A single row in a dataset containing known input features and its true target output.
    
- **Parameter:** Internal learnable values ($\mathbf{w}$ and $b$) optimized by the model during training.
    
- **Prediction ($\hat{y}$ or $f_{\mathbf{w},b}(\mathbf{x})$):** The output numerical value estimated by the model.
    
- **Gradient:** A vector collecting the partial derivatives of a function with respect to all parameters, pointing in the direction of maximum increase.
    
- **Cost Function ($J(\mathbf{w},b)$):** A function measuring total model prediction error across a dataset.
    

## Common Misconceptions

### Misconception 1: "Adding more features always makes a model better."

- **Correction:** Adding _irrelevant_ or noisy features can cause **overfitting**, where the model fits random noise in the training set rather than the true underlying pattern, degrading its performance on new, unseen test data.
    

### Misconception 2: "In matrix $X$, rows are features and columns are training examples."

- **Correction:** It is standard convention in machine learning that **rows are training examples ($m$)** and **columns are individual features ($n$)**.
    

### Misconception 3: "Each weight is trained independently on its own feature."

- **Correction:** All weights are optimized **jointly**. When calculating the derivative for $w_1$, the residual error term $\left(f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)}\right)$ relies on predictions built using ALL current weights and the bias.
    

### Misconception 4: "The dot product $\mathbf{w}^T \mathbf{x}$ results in another vector."

- **Correction:** The dot product of two vectors of identical length always outputs a **single scalar number**, not a vector.
    

## Summary (Key Takeaways)

1. Single-variable linear regression uses $1$ feature; multiple linear regression extends this to $n$ features.
    
2. The prediction function is $f_{\mathbf{w},b}(\mathbf{x}) = w_1 x_1 + w_2 x_2 + \dots + w_n x_n + b$.
    
3. Using linear algebra, the prediction formula is compactly written as $f_{\mathbf{w},b}(\mathbf{x}) = \mathbf{w}^T \mathbf{x} + b$.
    
4. The weight vector $\mathbf{w}$ has dimension $(n \times 1)$, representing $n$ individual weights.
    
5. The bias $b$ remains a single scalar baseline constant.
    
6. A dataset is stored in a Feature Matrix $X$ of size $(m \times n)$, where $m$ is the number of rows (examples) and $n$ is the number of columns (features).
    
7. $x_j^{(i)}$ represents the numerical value of feature column $j$ for training example row $i$.
    
8. Geometrically, $n=1$ feature forms a line, $n=2$ features form a flat plane, and $n \ge 3$ features form an $n$-dimensional hyperplane.
    
9. The dot product acts as a weighted sum, scaling each input feature by its respective feature weight.
    
10. A positive weight parameter means increasing that feature increases the target prediction.
    
11. A negative weight parameter means increasing that feature decreases the target prediction.
    
12. The Cost Function $J(\mathbf{w},b) = \frac{1}{2m} \sum_{i=1}^{m} \left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right)^2$ measures total Mean Squared Error across all $m$ examples.
    
13. The structure of the cost function is identical to single-variable regression; only the internal prediction calculation expands.
    
14. Gradient Descent updates all $n$ weights and the bias simultaneously in each step.
    
15. The update rule for weight $w_j$ is $w_j = w_j - \alpha \frac{1}{m} \sum_{i=1}^{m} \left( f_{\mathbf{w},b}(\mathbf{x}^{(i)}) - y^{(i)} \right) x_j^{(i)}$.
    
16. Vectorization uses matrix multiplication ($X \mathbf{w} + b$) to compute predictions for all $m$ examples instantly without using explicit loops.
    
17. In multivariable calculus terms, Gradient Descent subtracts the scaled gradient vector ($\alpha \nabla J$) from parameter vector $\mathbf{w}$.
    

## Revision Questions

### Conceptual Questions

1. Why is single-variable linear regression usually insufficient for complex real-world predictions?
    
2. What is the geometric difference between the prediction space of a model with 1 feature versus a model with 2 features?
    
3. Define what a hyperplane means in the context of machine learning.
    
4. What do $m$ and $n$ represent in dataset notation?
    
5. Explain the notation difference between $x_2^{(5)}$ and $(x_2)^5$.
    
6. What is the dimension of the feature matrix $X$?
    
7. What is the dimension of the target vector $\mathbf{y}$?
    
8. Explain the mathematical difference between a row vector and a column vector.
    
9. What operation turns a column vector into a row vector?
    
10. Write out the multi-variable linear regression hypothesis function using summation ($\sum$) notation.
    
11. Write out the multi-variable linear regression hypothesis function using vector dot product notation.
    
12. What does a negative weight $w_j < 0$ signify about the relationship between feature $x_j$ and target $y$?
    
13. If a feature $x_j$ has a weight of $w_j = 0$, how does changing $x_j$ affect the prediction?
    
14. Why do we square the prediction errors inside the cost function $J(\mathbf{w},b)$?
    
15. What role does the learning rate $\alpha$ play during parameter updates in gradient descent?
    
16. Why are all parameters ($w_1, \dots, w_n, b$) updated simultaneously during gradient descent rather than one by one?
    
17. What is vectorization, and why is it preferred over `for`-loops in computer implementation?
    
18. How does gradient descent know whether to increase or decrease a specific weight $w_j$?
    
19. Explain why having inputs on completely different numerical scales (e.g., $0.01$ vs $1\text{,}000\text{,}000$) causes issues for gradient descent.
    
20. What is multicollinearity and why does it make weight interpretation difficult?
    

### Numerical Calculation Problems

1. Compute the dot product $\mathbf{w}^T \mathbf{x}$ where $\mathbf{w} = \begin{bmatrix} 3 \\ -2 \\ 0.5 \end{bmatrix}$ and $\mathbf{x} = \begin{bmatrix} 4 \\ 1 \\ 10 \end{bmatrix}$.
    
2. Given parameters $\mathbf{w} = \begin{bmatrix} 0.2 \\ 5.0 \end{bmatrix}$ and $b = 50$, calculate the predicted price $f_{\mathbf{w},b}(\mathbf{x})$ for a house with feature vector $\mathbf{x} = \begin{bmatrix} 2000 \\ 3 \end{bmatrix}$.
    
3. You have a dataset with $m=1$ example: $\mathbf{x}^{(1)} = \begin{bmatrix} 2 \\ 4 \end{bmatrix}$, actual target $y^{(1)} = 20$. If current weights are $\mathbf{w} = \begin{bmatrix} 1 \\ 2 \end{bmatrix}$ and $b = 5$:
    
    - Calculate the prediction $f_{\mathbf{w},b}(\mathbf{x}^{(1)})$.
        
    - Calculate the prediction error $(f - y)$.
        
    - Calculate the cost $J(\mathbf{w},b)$.
        
4. For the problem above, compute the partial derivative $\frac{\partial J}{\partial w_1}$ for the single example ($m=1$).
    
5. Given a feature matrix $X$ of shape $(100 \times 5)$ and a weight vector $\mathbf{w}$ of shape $(5 \times 1)$:
    
    - What is the dimension of the product $X \mathbf{w}$?
        
    - How many total individual numbers are contained within matrix $X$?
        

### Reasoning Questions

1. Suppose feature $x_1$ is house size in square feet and $x_2$ is house size in square meters. Why is it problematic to include both features in a multiple linear regression model at the same time?
    
2. If a model predicts house prices and yields $w_1 = 200$ for square footage when measured in square feet, what would happen to the magnitude of $w_1$ if we changed the measurement unit of feature $x_1$ to square yards ($1 \text{ sq yd} = 9 \text{ sq ft}$)?
    
3. Why does the partial derivative formula for weight $w_j$ contain the extra multiplication term $x_j^{(i)}$, while the derivative for bias $b$ does not?
    
4. A student runs gradient descent and notices the cost function $J(\mathbf{w},b)$ grows larger and explodes to infinity after every step instead of decreasing. What is causing this, and how can it be fixed?
    
5. Imagine a dataset where feature $x_3$ is completely random noise (e.g., lottery numbers). What numerical value should gradient descent ideally drive weight $w_3$ toward over time, and why?

[[Gradient_Descent]]
[[Feature_Scalling]]

[[NumPy_Essentials_for_ML]]