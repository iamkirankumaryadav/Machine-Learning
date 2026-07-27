# 🔄 Cross-Validation

**Cross-Validation** is a technique used to check:

> 🎯 **"Will my Machine Learning model perform well on data it has never seen before?"**

Instead of evaluating a model using just **one train/validation split**, Cross-Validation trains and validates the model **multiple times using different portions of the data**.

In simple terms:

> **Split → Train → Validate → Rotate → Repeat → Average the results**

---

## 🎓 Simple Analogy

Imagine a student has 100 questions to prepare for an exam.

If the student practices 80 questions and tests themselves on only the same 20 questions:

```text
80 Questions → Practice 📚
20 Questions → Test 📝
```

Maybe those 20 happened to be unusually easy or difficult.

So instead, we rotate which questions are used for testing:

```text
Test 1 → Questions 1–20
Test 2 → Questions 21–40
Test 3 → Questions 41–60
Test 4 → Questions 61–80
Test 5 → Questions 81–100
```

Now every question gets a chance to be part of the test.

That's the basic idea behind **Cross-Validation**. 🔄

---

# 🤔 Why Do We Need Cross-Validation?

Suppose you have:

**1,000 observations**

You make one split:

```text
800 → Training
200 → Validation
```

and get:

**RMSE = 15**

Great! 🎯

But what if you randomly split the data differently?

```text
800 → Training
200 → Validation
```

Now:

**RMSE = 27** 😬

Another split:

**RMSE = 19**

So which number represents the model?

That's the problem.

A single validation split can depend heavily on **which observations happened to land in the validation set**.

Cross-Validation gives us a more reliable picture.

---

# ⭐ K-Fold Cross-Validation

The most common technique is:

> **K-Fold Cross-Validation**

"K" simply means:

> **How many parts should we divide the data into?**

Suppose:

**K = 5**

We divide the dataset into **5 folds**:

```text
Dataset

┌────┬────┬────┬────┬────┐
│ F1 │ F2 │ F3 │ F4 │ F5 │
└────┴────┴────┴────┴────┘
```

Then train and validate **5 times**.

### 🔄 Round 1

```text
F1 → Validation 📝

F2
F3  → Training 📚
F4
F5
```

### 🔄 Round 2

```text
F1
F2 → Validation 📝
F3  → Training 📚
F4
F5
```

Continue until every fold has been used as validation once.

So:

```text
Round 1 → F1 validates
Round 2 → F2 validates
Round 3 → F3 validates
Round 4 → F4 validates
Round 5 → F5 validates
```

That means:

> **5-Fold Cross-Validation trains 5 models.**

---

# 📊 What Happens at the End?

Suppose the validation RMSE from each fold is:

```text
Fold 1 → 20
Fold 2 → 18
Fold 3 → 22
Fold 4 → 19
Fold 5 → 21
```

Calculate the average:

**Average RMSE = 20**

Now we have a more stable estimate of model performance than relying on just one split.

We can also look at the variation across folds:

```text
20, 19, 21, 20, 20
```

Very consistent. ✅

Versus:

```text
10, 14, 22, 35, 41
```

Highly variable. 🚨

That could indicate the model's performance depends strongly on which data it receives.

---

# 🎯 Why Is It Called "Cross"-Validation?

Because different portions of the dataset **take turns being the validation data**.

```text
Fold 1 → Validate
   ↓
Fold 2 → Validate
   ↓
Fold 3 → Validate
   ↓
Fold 4 → Validate
   ↓
Fold 5 → Validate
```

The validation role moves across the dataset.

Hence:

> 🔄 **Cross-Validation**

---

# 🆚 Train/Test Split vs Cross-Validation

### Normal Holdout Split

```text
Dataset
   ↓
┌──────────────┬──────┐
│    Train     │ Test │
│     80%      │ 20%  │
└──────────────┴──────┘

Train Once
Evaluate Once
```

Simple and fast, but performance depends on that particular split.

### K-Fold Cross-Validation

```text
Dataset
   ↓
F1 F2 F3 F4 F5

Train → Validate
Train → Validate
Train → Validate
Train → Validate
Train → Validate

        ↓
Average Performance
```

More computationally expensive, but usually gives a more robust estimate during model development.

---

# 🎛️ Cross-Validation + Hyperparameter Tuning

This is one of the most important practical uses.

Suppose we're tuning **Ridge Regression**.

We want to find the best:

**alpha**

Candidates:

```text
0.01
0.1
1
10
100
```

Instead of evaluating each alpha on one validation split:

```text
alpha = 0.01
      ↓
5-Fold CV
      ↓
Average RMSE = 25


alpha = 0.1
      ↓
5-Fold CV
      ↓
Average RMSE = 21


alpha = 1
      ↓
5-Fold CV
      ↓
Average RMSE = 18 ⭐


alpha = 10
      ↓
5-Fold CV
      ↓
Average RMSE = 20
```

We might choose:

> 🎯 **alpha = 1**

This is how concepts such as **Grid Search + Cross-Validation** work together.

---

# 🔍 Common Types of Cross-Validation

### 1️⃣ K-Fold Cross-Validation ⭐

Divide data into K parts and rotate the validation fold.

Common choices:

**K = 5** or **K = 10**

Useful for many standard ML problems.

---

### 2️⃣ Stratified K-Fold ⚖️

Especially useful for **classification**, particularly with imbalanced classes.

Suppose:

```text
Fraud     → 10%
Not Fraud → 90%
```

Regular splitting could accidentally produce folds with very different class distributions.

Stratified K-Fold tries to maintain approximately:

```text
Fold 1 → 10% Fraud / 90% Normal
Fold 2 → 10% Fraud / 90% Normal
Fold 3 → 10% Fraud / 90% Normal
...
```

> **Stratified = Preserve class proportions**

---

### 3️⃣ Leave-One-Out Cross-Validation

If you have 100 observations:

```text
99 → Training
1  → Validation
```

Repeat **100 times**, so every observation is used as validation once.

This uses almost all data for training each time, but can be computationally expensive and isn't always the best statistical choice.

---

### 4️⃣ Time-Series Cross-Validation ⏰

Time-series data requires special handling because **future information shouldn't be used to predict the past**.

Suppose:

```text
2022 → 2023 → 2024 → 2025 → 2026
```

We might evaluate progressively:

```text
Train: 2022
Validate: 2023

Train: 2022–2023
Validate: 2024

Train: 2022–2024
Validate: 2025

Train: 2022–2025
Validate: 2026
```

This respects time order.

Random K-Fold is often inappropriate for time-dependent data.

---

# ⚠️ Cross-Validation and the Test Set

A very important distinction:

Cross-Validation is usually used during **model development**.

The **test set should remain untouched** until final evaluation.

A clean workflow looks like:

```text
Complete Dataset
       ↓
┌──────────────────┬──────────┐
│ Development Data │ Test Set │
└──────────────────┴──────────┘
         ↓
   Cross-Validation
         ↓
Select Model
+
Tune Hyperparameters
         ↓
Train Final Model
         ↓
Evaluate Once
         ↓
      Test Set 🎯
```

Why?

Because if you repeatedly use the test set to choose models or hyperparameters, the development process can indirectly **overfit to the test set**.

---

# 🚨 Avoid Data Leakage During Cross-Validation

Suppose you standardize the entire dataset **before** Cross-Validation.

```text
Entire Dataset
      ↓
StandardScaler
      ↓
Cross-Validation
```

🚨 Information from validation folds can influence the scaling parameters.

Instead:

```text
Fold
 ↓
Training Portion
 ↓
Fit StandardScaler
 ↓
Train Model

Validation Portion
 ↓
Use same fitted scaler
 ↓
Evaluate
```

In `scikit-learn`, a **Pipeline** is an excellent way to handle this correctly.

---

# 🐍 Cross-Validation in Python

Using `scikit-learn`:

```python
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LinearRegression

model = LinearRegression()

scores = cross_val_score(
    model,
    X,
    y,
    cv=5,
    scoring="neg_mean_squared_error"
)

print(scores)
```

`cv=5` means:

> **Use 5-Fold Cross-Validation.**

For tuning Ridge:

```python
from sklearn.linear_model import Ridge
from sklearn.model_selection import GridSearchCV

model = Ridge()

params = {
    "alpha": [0.01, 0.1, 1, 10, 100]
}

search = GridSearchCV(
    model,
    params,
    cv=5
)

search.fit(X, y)

print(search.best_params_)
```

Now the hyperparameter is selected using Cross-Validation. 🎯

---

# 🔗 Connecting Your Concepts

Cross-Validation ties together many of the concepts you've covered:

```text
Features
   ↓
Feature Engineering
   ↓
Choose Model
   ↓
Hyperparameters
   ↓
Cross-Validation 🔄
   ↓
Evaluate Metrics
   ↓
Tune Hyperparameters
   ↓
Control Bias & Variance
   ↓
Choose Best Model
   ↓
Final Test
   ↓
Generalization 🎯
```

For Ridge:

```text
Ridge Regression
      ↓
Try different alpha values
      ↓
5-Fold Cross-Validation
      ↓
Compare average RMSE
      ↓
Choose best alpha
      ↓
Final Model
```

---

# 🧠 Remember Forever

Imagine you want to judge a cricket player. 🏏

Would you judge them based on **one match**?

Probably not.

Instead:

```text
Match 1 → Score
Match 2 → Score
Match 3 → Score
Match 4 → Score
Match 5 → Score
             ↓
     Overall Performance
```

Similarly:

```text
One Split
   ↓
One Performance Score
   ❌


Multiple Folds
   ↓
Multiple Scores
   ↓
Average + Variation
   ✅
```

> **Cross-Validation = Don't judge your model on just one validation split.**

## 🎯 Interview-Ready Definition

> **Cross-Validation is a model evaluation technique where the training data is repeatedly divided into training and validation subsets, allowing the model to be evaluated across multiple splits to estimate how well it generalizes to unseen data.**

### ⭐ Quick Cheat Sheet
- **K-Fold** → Rotate K validation folds 🔄
- **5-Fold** → Train/evaluate 5 times
- **Stratified K-Fold** → Preserve class proportions ⚖️
- **Time-Series CV** → Preserve chronological order ⏰
- **GridSearchCV** → Hyperparameter tuning + CV 🎛️
- **Test Set** → Keep untouched for final evaluation 🎯

### 🧠 One-line memory trick

> **Cross-Validation = Train and validate multiple times on different splits, then combine the scores to judge the model more reliably.**
