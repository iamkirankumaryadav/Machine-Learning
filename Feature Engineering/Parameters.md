# ⚙️ Parameters

**Parameters** are the **values a Machine Learning model learns automatically from training data**.

In simple terms:

> 🧠 **Parameters = What the model learns during training.**

You provide the **data**, and the training process figures out the parameter values that help make good predictions.

---

## 🏠 Simple Example

Suppose we want to predict a house price using its size.

After training, the model learns:

> **House Price = $50,000 + $200 × House Size**

Here:

* **$50,000** → Intercept
* **$200** → Coefficient

Both are **parameters** because the model learned them from the training data.

You didn't manually say:

> "Use exactly $200 per square foot."

The model found that value during training. 🤖

---

# 📐 Parameters in Linear Regression

Remember the Linear Regression equation:

**ŷ = b₀ + b₁x**

Where:

**x** → Input feature
**ŷ** → Prediction
**b₀** → Intercept ⚙️
**b₁** → Coefficient ⚙️

The model needs to figure out good values for:

**b₀ and b₁**

These are the model's **parameters**.

Suppose training produces:

```text
b₀ = 50
b₁ = 10
```

Then the learned model becomes:

**ŷ = 50 + 10x**

For `x = 5`:

**ŷ = 50 + 10(5) = 100**

---

# 🧠 What Does "Learn" Actually Mean?

Imagine the model starts with poor parameter values:

```text
Coefficient = 2
Intercept   = 5
```

Predictions are poor.

```text
Parameters
    ↓
Prediction
    ↓
Compare with Actual
    ↓
Loss 🚨
```

The training algorithm adjusts the parameters:

```text
Coefficient

2 → 4 → 6 → 8 → 9.5 → 10
```

As the parameters improve:

```text
Prediction Error
       ↓
      Loss
       ↓
Gets Smaller 📉
```

Eventually, the model finds parameters that minimize or approximately minimize its training objective.

So:

> 🎯 **Training a model largely means finding good parameter values.**

---

# 🔄 Connection With Gradient Descent

This connects directly to Gradient Descent.

Remember:

> **Gradient Descent adjusts parameters to reduce the loss.**

```text
Initial Parameters
       ↓
Make Prediction
       ↓
Calculate Loss
       ↓
Calculate Gradient
       ↓
Update Parameters ⚙️
       ↓
Make Prediction Again
       ↓
Repeat 🔄
```

For example:

```text
Weight = 5
   ↓
Loss = 100

Weight = 7
   ↓
Loss = 50

Weight = 9
   ↓
Loss = 15

Weight = 10
   ↓
Loss = 5 🎯
```

The **weight** being adjusted is a parameter.

One nuance: not every model uses Gradient Descent. Some algorithms learn their parameters using other optimization methods.

---

# ⚙️ Parameters in Different ML Models

Different algorithms have different types of learned parameters.

| Model                  | Examples of learned parameters                |
| ---------------------- | --------------------------------------------- |
| 📈 Linear Regression   | Coefficients, intercept                       |
| 📊 Logistic Regression | Coefficients, intercept                       |
| 🧠 Neural Network      | Weights, biases                               |
| 🎯 SVM                 | Support-vector-related coefficients/intercept |
| 📦 Naive Bayes         | Class probabilities, distribution parameters  |

For tree models, you'll often hear the learned splits/tree structure described as the **fitted model state**, rather than using "parameters" in exactly the same mathematical sense as regression coefficients.

---

# 🧠 Neural Network Example

Neural Networks can contain millions or billions of parameters.

Imagine one neuron:

```text
Input x₁ ── Weight w₁ ──┐
                        │
Input x₂ ── Weight w₂ ──┼──→ Neuron → Output
                        │
Input x₃ ── Weight w₃ ──┘
                 +
               Bias b
```

Here:

**w₁, w₂, w₃** → Weights
**b** → Bias

These are **parameters**.

During training:

```text
Weights + Biases
       ↓
Prediction
       ↓
Loss
       ↓
Backpropagation
       ↓
Gradients
       ↓
Optimizer
       ↓
Update Weights + Biases
       ↓
Repeat 🔄
```

So when you hear:

> **"A model has 7 billion parameters"**

it roughly means the model contains **7 billion learned numerical values**, primarily weights, plus other learned values depending on the architecture.

---

# 🎛️ Parameters vs Hyperparameters

This is the most important distinction.

### ⚙️ Parameters

**Learned during training**

Examples:

```text
Coefficient
Intercept
Weight
Bias
```

### 🎛️ Hyperparameters

**Configured/tuned by us or an automated tuning process**

Examples:

```text
Learning Rate
Batch Size
Tree Depth
Number of Trees
Regularization Strength
```

So:

|                             | ⚙️ Parameter          | 🎛️ Hyperparameter         |
| --------------------------- | --------------------- | -------------------------- |
| Learned from training data? | ✅ Yes                 | ❌ Not directly             |
| Determined by               | Training algorithm    | Developer/tuning process   |
| Example                     | Weight                | Learning rate              |
| Changes during training?    | Usually ✅             | Usually fixed within a run |
| Purpose                     | Defines learned model | Controls model/training    |

### 🧠 Memory Trick

> **Parameter = Model learns it**

> **Hyperparameter = We configure/tune it**

---

# 🛡️ Connection With Ridge & Lasso

Suppose Linear Regression learns:

```text
Size coefficient       = 200
Bedrooms coefficient   = 30,000
Age coefficient        = -2,000
```

These coefficients are **parameters**.

Ridge and Lasso add regularization that influences how these parameters are learned.

### Ridge

```text
Large Parameters
      ↓
L2 Penalty
      ↓
Shrink Parameters
```

### Lasso

```text
Parameters
     ↓
L1 Penalty
     ↓
Shrink Parameters
     ↓
Some may become 0 ✂️
```

But:

```text
alpha = 1.0
```

is a **hyperparameter** controlling how strong that regularization is.

So:

> ⚙️ **Coefficient = Parameter**

> 🎛️ **Alpha = Hyperparameter**

---

# 🚗 Easy Analogy

Imagine you're learning to drive a car.

Before practice, an instructor chooses:

```text
Training duration
Practice route
Difficulty
```

Think of these as **hyperparameters** - settings controlling the learning process.

As you practice, you learn:

```text
How much steering is needed
How much braking is needed
How much acceleration is needed
```

Think of these learned behaviors as **parameters**.

So:

> 🎛️ **Hyperparameters = How learning is configured**

> ⚙️ **Parameters = What gets learned**

---

# 🔗 Putting Everything Together

```text
Training Data
     ↓
Algorithm
     +
Hyperparameters 🎛️
     ↓
Training
     ↓
Optimization
     ↓
Learn Parameters ⚙️
     ↓
Trained Model 🤖
     ↓
Predictions
```

For Linear Regression:

```text
House Data
    ↓
Linear Regression
    ↓
Training
    ↓
Learn
b₀, b₁, b₂...
    ↓
Trained Model
    ↓
Predict House Price 🏠
```

---

## 🎯 Interview-Ready Definition

> **Parameters are internal values learned by a machine learning model from training data during the fitting process, such as coefficients and intercepts in Linear Regression or weights and biases in Neural Networks.**

### 🧠 Remember Forever

**Parameters → Learned ⚙️**

**Hyperparameters → Configured/Tuned 🎛️**

**Linear Regression → Coefficients + Intercept**

**Neural Networks → Weights + Biases**

**Training → Process of learning good parameters**

> ⭐ **Parameters are the knowledge captured by the model during training.**
