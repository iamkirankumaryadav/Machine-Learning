# 🎛️ Hyperparameters

**Hyperparameters** are **settings we choose to control how a Machine Learning model learns or how complex it can become**.

In simple terms:

> 🎛️ **Hyperparameters = Settings you configure for the learning process.**

The model generally **does not learn these values directly from the training data**. We choose or tune them.

---

## 🍳 Simple Analogy: Cooking

Imagine you're baking a cake. 🎂

You have:

```text
Ingredients → Data
Recipe      → Algorithm
Oven        → Training Process
Cake        → Trained Model
```

Before cooking, you decide:

```text
Temperature → 180°C
Cooking Time → 30 minutes
```

These are like **hyperparameters** because they control **how the cooking happens**.

Machine Learning works similarly:

```text
Learning Rate = 0.01
Tree Depth = 5
Number of Trees = 100
K = 5
Regularization Strength = 1.0
```

These settings influence how the model learns and behaves.

---

# 🧠 Hyperparameters vs Parameters

This is the most important distinction.

### 🤖 Parameters

**Parameters are learned by the model during training.**

For Linear Regression:

**ŷ = b₀ + b₁x**

The model learns:

```text
b₀ → Intercept
b₁ → Coefficient
```

These are **parameters**.

You don't normally tell the model:

> "Set the coefficient to 42."

The training algorithm finds suitable values from the data.

---

### 🎛️ Hyperparameters

**Hyperparameters are settings used to configure the model or training process.**

Examples:

```text
Learning Rate
Regularization Strength
Tree Depth
Number of Trees
Number of Neighbors
Batch Size
```

These are usually selected **before or during the model-development process**, often with validation or cross-validation.

---

# 🆚 Parameters vs Hyperparameters

|                                   | 🤖 Parameters            | 🎛️ Hyperparameters                |
| --------------------------------- | ------------------------ | ---------------------------------- |
| Learned from training data?       | ✅ Yes                    | ❌ Usually no                       |
| Who determines them?              | Model/training algorithm | Developer/tuning process           |
| Example                           | Weights, coefficients    | Learning rate, tree depth          |
| Changed during ordinary training? | ✅ Yes                    | Usually fixed for one training run |
| Purpose                           | Make predictions         | Control model/training behavior    |

### 🧠 Memory Trick

> **Parameters = Model learns them**

> **Hyperparameters = We configure/tune them**

---

# 🏠 Linear Regression Example

Suppose:

**House Price = b₀ + b₁(Size) + b₂(Bedrooms)**

Training might learn:

```text
b₀ = 50,000
b₁ = 200
b₂ = 15,000
```

These are:

> 🤖 **Parameters**

But if we use Ridge Regression:

```python
Ridge(alpha=1.0)
```

`alpha` controls regularization strength.

That's a:

> 🎛️ **Hyperparameter**

The model learns the coefficients **given the alpha we chose**.

---

# 📚 Common Hyperparameters

Different algorithms have different hyperparameters.

### 🛡️ Ridge / Lasso

```text
alpha
```

Controls **regularization strength**.

```text
Small alpha
    ↓
Weak Regularization

Large alpha
    ↓
Strong Regularization
```

---

### 🌳 Decision Tree

Common hyperparameters include:

```text
max_depth
min_samples_split
min_samples_leaf
```

For example:

```python
DecisionTreeClassifier(max_depth=5)
```

`max_depth=5` tells the tree:

> 🌳 "Don't grow beyond five levels."

This helps control model complexity and overfitting.

---

### 🌲 Random Forest

Common hyperparameters:

```text
n_estimators
max_depth
max_features
min_samples_split
```

For example:

```python
RandomForestClassifier(
    n_estimators=200,
    max_depth=10
)
```

Here:

**200** → Number of trees 🌲🌲🌲
**10** → Maximum depth of each tree

---

### 👥 K-Nearest Neighbors

One important hyperparameter is:

```text
n_neighbors = K
```

For example:

```python
KNeighborsClassifier(n_neighbors=5)
```

Meaning:

> "Look at the **5 nearest neighbors** when making a classification."

---

### 🧠 Neural Networks

Common hyperparameters include:

```text
Learning Rate
Batch Size
Number of Layers
Number of Neurons
Dropout Rate
Weight Decay
Number of Epochs
```

For example:

```text
Learning Rate = 0.001
Batch Size = 32
Epochs = 100
```

These settings can have a major impact on training.

---

# 🎛️ Learning Rate Is a Hyperparameter

Remember Gradient Descent:

**New Parameter = Old Parameter − Learning Rate × Gradient**

The model learns its **weights**.

But we choose the **learning rate**.

So:

```text
Weights
   ↓
Parameters 🤖


Learning Rate
   ↓
Hyperparameter 🎛️
```

### Too small

```text
Learning Rate = 0.000001

Training
🐌🐌🐌🐌🐌
```

Training may progress very slowly.

### Too large

```text
Learning Rate = 10

Minimum 🎯
   ↙     ↘
 ●         ●
   ↖     ↙
```

The optimizer may overshoot or become unstable.

### Appropriate value

```text
Start
  ↓
 ●
   ↘
     ●
       ↘
         ●
           🎯
```

Training converges efficiently.

---

# 🔧 What Is Hyperparameter Tuning?

Now the natural question is:

> **How do we know which hyperparameter values are best?**

We **experiment with different values and evaluate them on validation data**.

This process is called:

> 🔧 **Hyperparameter Tuning**

Suppose you're tuning Ridge:

```text
alpha = 0.01
alpha = 0.1
alpha = 1
alpha = 10
alpha = 100
```

You train separate models:

```text
alpha = 0.01 → RMSE = 24.5
alpha = 0.1  → RMSE = 21.2
alpha = 1    → RMSE = 18.7 ⭐
alpha = 10   → RMSE = 20.4
alpha = 100  → RMSE = 29.8
```

Based on this validation experiment:

```text
Best alpha → 1
```

So you choose it and later evaluate the final model on untouched test data.

---

# 🔍 How Do We Find Good Hyperparameters?

Three common approaches:

### 1️⃣ Grid Search 🔲

Define possible values:

```text
Learning Rate:
[0.001, 0.01, 0.1]

Batch Size:
[16, 32, 64]
```

Grid Search systematically tries the combinations.

```text
0.001 + 16
0.001 + 32
0.001 + 64

0.01  + 16
0.01  + 32
0.01  + 64

...
```

> **Grid Search = Try every specified combination**

---

### 2️⃣ Random Search 🎲

Instead of trying every combination, randomly sample configurations from specified ranges/distributions.

> **Random Search = Try a selected set of random combinations**

This can be much more efficient when there are many hyperparameters.

---

### 3️⃣ Bayesian Optimization 🧠

Uses results from previous trials to decide which configuration looks promising to try next.

Conceptually:

```text
Try Configuration
       ↓
Observe Performance
       ↓
Learn from Result
       ↓
Choose Promising Next Configuration
       ↓
Repeat
```

Tools/frameworks such as Optuna can support this style of tuning.

---

# 🔄 Hyperparameters + Cross-Validation

We often combine hyperparameter tuning with **cross-validation**.

```text
Hyperparameter Candidates
          ↓
    Cross-Validation
          ↓
Compare Average Performance
          ↓
Choose Best Configuration
          ↓
Train Final Model
          ↓
Evaluate on Test Set 🎯
```

This reduces the risk of choosing a hyperparameter just because it happened to perform well on one particular validation split.

---

# ⚠️ Don't Tune Using the Test Set

This is important.

Your test set should represent:

> **"Data the model-development process hasn't used."**

If you repeatedly choose hyperparameters based on test performance:

```text
Test Data
    ↓
Tune Hyperparameters
    ↓
Try Again
    ↓
Tune Again
```

you are indirectly **overfitting to the test set**. 🚨

A cleaner workflow is:

```text
Training Data
     ↓
Train Parameters


Validation / Cross-Validation
     ↓
Tune Hyperparameters


Test Data
     ↓
Final Evaluation
```

---

# 🔗 How Everything Connects

You've now learned several concepts that fit together:

```text
Dataset
   ↓
Feature Engineering
   ↓
Choose Algorithm
   ↓
Set Hyperparameters 🎛️
   ↓
Train Model
   ↓
Learn Parameters 🤖
   ↓
Predictions
   ↓
Residuals
   ↓
Regression Metrics
   ↓
Tune Hyperparameters
   ↓
Reduce Bias / Variance
   ↓
Better Generalization 🎯
```

For example:

```text
Ridge Regression
       ↓
Hyperparameter: alpha
       ↓
Controls Regularization
       ↓
Controls Model Complexity
       ↓
Affects Bias & Variance
       ↓
Affects Test Performance
```

---

# 🧠 Easy Analogy to Remember Forever

Imagine driving a car. 🚗

The **engine learns nothing**-but you configure how the car behaves:

```text
Driving Mode
Suspension Setting
Cruise Speed
Traction Setting
```

Think of these as **hyperparameters**.

Meanwhile, values that the system learns/adapts internally during operation are more like **parameters**.

So:

> 🎛️ **Hyperparameters control how learning happens.**

> 🤖 **Parameters are what the model learns.**

---

## 🎯 Interview-Ready Definition

> **Hyperparameters are configuration values that control a model's architecture, complexity, or training process and are not learned directly as model parameters from the training data. They are typically selected using validation techniques such as cross-validation and hyperparameter tuning.**

### ⭐ Quick Cheat Sheet

**Parameters** → Learned by model 🤖
**Hyperparameters** → Configured/tuned 🎛️
**Grid Search** → Try specified combinations 🔲
**Random Search** → Sample combinations 🎲
**Cross-Validation** → Compare configurations reliably 🔄
**Goal** → Better generalization on unseen data 🎯
