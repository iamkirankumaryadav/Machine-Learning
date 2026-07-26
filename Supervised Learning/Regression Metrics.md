# 📊 Regression Metrics — Explained in Simple Terms

**Regression metrics** tell us **how good or bad a regression model's predictions are**.

Suppose a model predicts house prices:

| 🏠 House | Actual Price | Predicted Price | Error |
| -------- | -----------: | --------------: | ----: |
| A        |        $100K |           $110K |  $10K |
| B        |        $200K |           $180K |  $20K |
| C        |        $300K |           $330K |  $30K |

Regression metrics answer:

> 🎯 **How far are our predictions from the actual values?**

The most important ones are:

**MAE → MSE → RMSE → R² → Adjusted R²**

---

# 1️⃣ MAE — Mean Absolute Error

### 📌 Simple meaning

**MAE = Average prediction mistake**

Take the difference between actual and predicted values, ignore whether the error is positive or negative, and calculate the average.

### Formula

**MAE = Average of |Actual − Predicted|**

Using our example:

```text
Errors:

$10K
$20K
$30K

MAE = (10 + 20 + 30) / 3
    = $20K
```

### 🧠 Interpretation

**MAE = $20K**

means:

> 🏠 On average, our model's predictions are off by about **$20,000**.

### 🎯 Goal

**Lower MAE = Better**

```text
MAE = 0      → Perfect 🏆
MAE = $10K   → Better
MAE = $50K   → Worse
```

### ⭐ Why MAE is useful

It is very easy to explain because it remains in the **same unit as the target**.

Predicting salary → MAE in dollars
Predicting temperature → MAE in degrees
Predicting sales → MAE in sales units

---

# 2️⃣ MSE — Mean Squared Error

### 📌 Simple meaning

MSE is similar to MAE, but it **squares every error before averaging**.

**MSE = Average of (Actual − Predicted)²**

Suppose the errors are:

```text
10
20
30
```

Square them:

```text
10² = 100
20² = 400
30² = 900
```

Then:

```text
MSE = (100 + 400 + 900) / 3

    = 466.67
```

### 🤔 Why square the errors?

Because large errors become **much more expensive**.

Compare:

```text
Error = 2   → Squared = 4

Error = 10  → Squared = 100

Error = 50  → Squared = 2500 🚨
```

So MSE strongly punishes models making **large mistakes**.

### 🎯 Goal

**Lower MSE = Better**

**MSE = 0 → Perfect**

### ⚠️ Problem with MSE

Its unit is squared.

If you're predicting dollars:

```text
Actual unit → $
MSE unit    → $²
```

That makes MSE less intuitive to explain to business stakeholders.

---

# 3️⃣ RMSE — Root Mean Squared Error

### 📌 Simple meaning

**RMSE is the square root of MSE.**

It keeps MSE's stronger penalty for large errors but converts the result back into the target's original unit.

### Formula

**RMSE = √MSE**

If:

```text
MSE = 400

RMSE = √400
     = 20
```

If you're predicting house prices:

> 🏠 RMSE = $20K

This tells us that prediction errors are roughly **$20,000 in magnitude**, with larger mistakes having extra influence because errors were squared first.

### 🎯 Goal

**Lower RMSE = Better**

---

# 🆚 MAE vs MSE vs RMSE

This is one of the most important comparisons to remember.

| Metric   | Meaning                | Large Errors     | Unit           |
| -------- | ---------------------- | ---------------- | -------------- |
| **MAE**  | Average absolute error | Normal penalty   | Same as target |
| **MSE**  | Average squared error  | Heavy penalty 🚨 | Squared        |
| **RMSE** | Root of MSE            | Heavy penalty 🚨 | Same as target |

### 🧠 Easy memory trick

**MAE → Average mistake**

**MSE → Punishes big mistakes**

**RMSE → Punishes big mistakes + easy to interpret**

---

# 4️⃣ R² — R-Squared

R² is different from MAE, MSE and RMSE.

Instead of asking:

> ❓ How large are my prediction errors?

R² asks:

> ❓ How much of the variation in the target does my model explain compared with simply predicting the mean?

### Example

Suppose:

**R² = 0.80**

A simple interpretation is:

> The model explains about **80% of the variation** in the target variable.

```text
R² = 1.00 → Perfect fit 🏆

R² = 0.80 → 80% explained

R² = 0.50 → 50% explained

R² = 0.00 → No improvement over predicting mean

R² < 0    → Worse than predicting mean 🚨
```

### 🎯 Goal

Generally:

**Higher R² = Better**

and **1 is the ideal value**.

But don't assume a high R² automatically means a good model. You should also inspect prediction errors, test-set performance, residuals, and whether the model makes sense for the problem.

---

# 5️⃣ Adjusted R²

Here's a problem with regular R².

Suppose we predict:

**House Price**

using:

```text
Size
Bedrooms
Location
Age
```

Now we add:

```text
Owner's favorite number 🤨
```

Even if that feature is useless, training-set **R² cannot decrease** when you add predictors.

That's a problem.

### 💡 Adjusted R² solves this

Adjusted R² considers:

**Model performance + Number of predictors**

It effectively asks:

> Did adding this new feature improve the model enough to justify the extra complexity?

If useful features are added:

```text
Adjusted R² ↑
```

If useless features are added:

```text
Adjusted R² may ↓
```

### 🧠 Easy memory

**R² = How much variation did the model explain?**

**Adjusted R² = How much did it explain while accounting for how many predictors it used?**

---

# 🎯 One Example to Understand Everything

Imagine you're predicting salaries.

Actual:

```text
$50K
$60K
$70K
$80K
```

Predicted:

```text
$52K
$58K
$75K
$76K
```

The metrics tell you different things:

```text
MAE
↓
"How much am I wrong on average?"


MSE
↓
"How bad are my errors if big mistakes
deserve extra punishment?"


RMSE
↓
"Same idea as MSE, but give the answer
back in dollars."


R²
↓
"How much variation in salary does
my model explain?"


Adjusted R²
↓
"Is that explanatory power still impressive
after accounting for the number of predictors?"
```

---

# 🔥 MAE vs RMSE — When Should I Use Which?

This is particularly useful in practical ML.

### Use MAE when:

You want an **easy-to-understand average error**, and large errors shouldn't receive disproportionate weight.

Example:

> "Our house price predictions are off by **$12,000 on average**."

### Use RMSE when:

**Large prediction errors are particularly undesirable**.

Imagine:

```text
Actual      Predicted      Error

$100K       $105K          $5K
$200K       $205K          $5K
$500K       $900K          $400K 🚨
```

RMSE reacts much more strongly to that **$400K error** than MAE.

---

# 📈 Where Does Linear Regression Fit?

Remember that Linear Regression finds a line that minimizes squared residuals.

genui{"inference_regression_ml_learning_block":{"type_id":"LEAST_SQUARE_REGRESSION"}}

Each vertical gap between an actual observation and the regression line is a **residual**.

```text
Actual value
     ●
     │ ← Residual/Error
     │
─────●──────── Regression Line
 Predicted
```

Ordinary Least Squares chooses coefficients to minimize the **sum of squared residuals**.

That's why squared-error metrics like **MSE and RMSE** connect naturally with Linear Regression.

---

# 🐍 Regression Metrics in Python

Using `scikit-learn`:

```python
from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    r2_score
)

y_actual = [100, 200, 300]
y_pred = [110, 180, 330]

mae = mean_absolute_error(y_actual, y_pred)
mse = mean_squared_error(y_actual, y_pred)
rmse = mse ** 0.5
r2 = r2_score(y_actual, y_pred)

print("MAE :", mae)
print("MSE :", mse)
print("RMSE:", rmse)
print("R²  :", r2)
```

---

# 🧠 Remember Forever

Think of a student predicting exam scores:

```text
                    Regression Metrics
                           │
        ┌──────────────────┼─────────────────┐
        ↓                  ↓                 ↓
      MAE                RMSE               R²
        │                  │                 │
 Average mistake      Big mistakes       How much
                      hurt more           variation
                                          explained
```

And then:

**MSE = RMSE before taking the square root**

**Adjusted R² = R² with a complexity penalty**

---

## 🎯 Interview Cheat Sheet

| Metric          | Simple Meaning                                     |   Best Value |
| --------------- | -------------------------------------------------- | -----------: |
| **MAE**         | Average absolute error                             |      **0** ↓ |
| **MSE**         | Average squared error                              |      **0** ↓ |
| **RMSE**        | Squared-error metric in original units             |      **0** ↓ |
| **R²**          | Proportion of variation explained vs mean baseline |      **1** ↑ |
| **Adjusted R²** | R² adjusted for number of predictors               | **Higher** ↑ |

### ⭐ One-line answer

> **MAE measures average absolute error, MSE penalizes large errors through squaring, RMSE expresses squared-error performance in the original unit, R² measures explained variation relative to a mean baseline, and Adjusted R² accounts for model complexity.**
