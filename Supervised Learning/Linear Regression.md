# 📈 Linear Regression - Explained in Simple Terms

**Linear Regression** is a **Supervised Machine Learning algorithm** used to **predict a continuous numerical value** by learning the relationship between input variables and an output.

Think:

> **Past data → Find the trend → Draw the best-fit line → Predict future values**

---

## 🏠 Simple Example

Imagine you want to predict the **price of a house** based on its size.

| House Size (sq ft) |    Price |
| -----------------: | -------: |
|              1,000 | $200,000 |
|              1,500 | $300,000 |
|              2,000 | $400,000 |
|              2,500 | $500,000 |

You can see a pattern:

**Bigger house → Higher price**

Linear Regression learns this relationship and creates a **line of best fit**.

Then if you ask:

> 🏠 What might a **1,800 sq ft** house cost?

The model uses the learned line to make a prediction.

---

# 📐 The Core Idea

Linear Regression tries to find a straight line that fits the data as closely as possible.

genui{"inference_regression_ml_learning_block":{"type_id":"LEAST_SQUARE_REGRESSION"}}

The mathematical equation is:

**ŷ = b₀ + b₁x**

Where:

| Term   | Meaning             |
| ------ | ------------------- |
| **ŷ**  | Predicted value     |
| **x**  | Input feature       |
| **b₀** | Intercept           |
| **b₁** | Slope / coefficient |

### 🧠 Easy way to remember

Think of:

**Prediction = Starting Point + Effect of Input**

Suppose:

**House Price = $50,000 + $150 × Size**

Here:

* **$50,000** → intercept
* **$150** → coefficient
* **Size** → input feature
* **House Price** → predicted target

For a 2,000 sq ft house:

**Price = $50,000 + ($150 × 2,000) = $350,000**

---

# 📊 What is the Best-Fit Line?

Real-world data won't perfectly sit on one line.

```text
Price
  ↑
  |                 •
  |              •
  |           •
  |        •
  |     •
  |  •
  +----------------------→ Size
```

Linear Regression tries to find the line that passes **as close as possible to all observations**.

```text
Price
  ↑
  |                 •
  |              • /
  |           • /
  |        • /
  |     • /
  |  • /
  +----------------------→ Size
```

This is called the **line of best fit** or **regression line**.

---

# 🎯 How Does It Find the Best Line?

Imagine the actual house price is:

**$400,000**

but the model predicts:

**$380,000**

The difference is:

**Error = Actual − Predicted**

**= $400,000 − $380,000 = $20,000**

This difference is called a **residual**.

Linear Regression tries to choose the line where these errors are as small as possible.

A common method is **Ordinary Least Squares (OLS)**.

It minimizes the **sum of squared residuals**:

**Σ(Actual − Predicted)²**

### Why square the errors? 🤔

Because squaring:

* prevents positive and negative errors from cancelling each other
* penalizes large errors more heavily
* gives us a convenient mathematical optimization problem

---

# 🧩 Simple vs Multiple Linear Regression

### 1️⃣ Simple Linear Regression

Uses **one input feature**.

**House Price ← House Size**

Equation:

**ŷ = b₀ + b₁x**

### 2️⃣ Multiple Linear Regression

Uses **multiple input features**.

For example:

**House Price ← Size + Bedrooms + Age + Location Score**

The equation becomes:

**ŷ = b₀ + b₁x₁ + b₂x₂ + b₃x₃ + ... + bₙxₙ**

For example:

**Price = b₀ + b₁(Size) + b₂(Bedrooms) + b₃(Age)**

Each coefficient represents how that feature is associated with the prediction **while holding the other features constant**.

---

# 🔑 Understanding the Coefficient

Suppose:

**Salary = $30,000 + $5,000 × Experience**

Here **$5,000** is the coefficient.

It means:

> For every additional **1 year of experience**, predicted salary increases by **$5,000**, assuming everything else stays the same.

### Positive coefficient 📈

**b₁ > 0**

As X increases, Y tends to increase.

**Experience ↑ → Salary ↑**

### Negative coefficient 📉

**b₁ < 0**

As X increases, Y tends to decrease.

**Car Age ↑ → Resale Value ↓**

### Near-zero coefficient ➖

The feature may have little linear relationship with the target, though interpretation depends on the other variables in the model.

---

# ⚙️ End-to-End Flow

```text
Historical Data
      ↓
Choose Features + Target
      ↓
Split Train/Test Data
      ↓
Train Linear Regression
      ↓
Model learns coefficients
      ↓
Make Predictions
      ↓
Evaluate Errors
```

For example:

```text
Size ──────┐
Bedrooms ──┼──→ Linear Regression ──→ House Price
Age ───────┘
```

---

# 📏 How Do We Evaluate Linear Regression?

Common regression metrics include:

**MAE - Mean Absolute Error**
Average absolute difference between actual and predicted values.

**MSE - Mean Squared Error**
Average squared prediction error.

**RMSE - Root Mean Squared Error**
Square root of MSE; easier to interpret because it's in the target's original unit.

**R² - R-Squared**
Measures how much of the target's variation is explained by the model.

For example:

**R² = 0.80**

roughly means the model explains **80% of the variation in Y** in that dataset.

---

# ⚠️ Important Assumptions

Classical Linear Regression inference generally relies on assumptions such as:

**1. Linearity 📏** - The relationship between predictors and expected target is linear.

**2. Independence 🔗** - Observations/errors should not have problematic dependence.

**3. Homoscedasticity ⚖️** - Error variance should be roughly constant.

**4. Normality 🔔** - Residual normality matters especially for classical confidence intervals and hypothesis tests.

**5. Low multicollinearity 🧩** - Predictors shouldn't be excessively correlated with each other if you want stable coefficient interpretation.

A model can still make predictions when some assumptions aren't perfect, but violations can affect accuracy, uncertainty estimates, or coefficient interpretation.

---

# 🐍 Linear Regression in Python

Using `scikit-learn`:

```python
from sklearn.linear_model import LinearRegression

X = [[1000], [1500], [2000], [2500]]
y = [200000, 300000, 400000, 500000]

model = LinearRegression()
model.fit(X, y)

prediction = model.predict([[1800]])

print(prediction)
```

The model learns the relationship between **house size** and **price**, then predicts the price for **1,800 sq ft**.

---

# 🧠 Regression vs Classification

A very important distinction:

| Regression 📈              | Classification 🏷️        |
| -------------------------- | ------------------------- |
| Predicts a numerical value | Predicts a category/class |
| House price                | Spam / Not Spam           |
| Salary                     | Fraud / Not Fraud         |
| Temperature                | Cat / Dog                 |
| Sales revenue              | Churn / No Churn          |

**Linear Regression → continuous numerical prediction**

**Logistic Regression → classification**, despite having "Regression" in its name.

---

# 💡 Easy Analogy

Imagine several students:

```text
Study Hours → Exam Score

2 hours → 50
4 hours → 65
6 hours → 78
8 hours → 90
```

Linear Regression notices:

> 📈 More study hours generally correspond to higher scores.

It draws the best possible straight line through the observations.

Then:

```text
7 hours → ?
```

The model uses that line to estimate the score.

That's essentially **Linear Regression**.

---

## 🎯 Interview-ready definition

> **Linear Regression is a supervised learning algorithm that models a continuous target as a linear combination of one or more input features, typically estimating coefficients by minimizing the sum of squared residuals.**

### 🧠 Remember forever

**Linear Regression = Find the best-fitting straight line → understand relationships → predict continuous values.**
