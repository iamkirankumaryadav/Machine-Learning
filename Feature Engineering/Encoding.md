# 🔤 Encoding - Explained in Simple Terms

In Machine Learning, **encoding** means converting **categorical/text data into numerical data** so an ML model can understand and use it.

> 🧠 **Encoding = Convert categories → numbers**

For example:

```text
City

Bengaluru
Mumbai
Delhi
```

Most traditional ML algorithms cannot directly perform calculations on these words. So we encode them:

```text
Bengaluru → numerical representation
Mumbai    → numerical representation
Delhi     → numerical representation
```

---

# 🧑‍🏫 Simple Analogy

Imagine a teacher records attendance as:

```text
Present
Absent
```

A computer could represent it as:

```text
Present → 1
Absent  → 0
```

Nothing about the meaning changed. We simply created a **machine-friendly representation**.

That's encoding.

---

# 🎯 Why Do We Need Encoding?

Suppose we want to predict salary:

| Experience | Education  | Salary |
| ---------: | ---------- | -----: |
|    2 years | Bachelor's |   $60K |
|    5 years | Master's   |   $90K |
|    8 years | PhD        |  $120K |

`Experience` is already numerical.

But:

```text
Bachelor's
Master's
PhD
```

are categories.

Before many ML models can use them:

```text
Categorical Data
       ↓
    Encoding
       ↓
Numerical Representation
       ↓
     ML Model
       ↓
   Prediction
```

---

# 📦 Two Types of Categorical Data

Before choosing an encoding technique, ask:

> **Do these categories have a meaningful order?**

## 1️⃣ Nominal Data - No Order

Categories have **no natural ranking**.

Examples:

```text
City
──────
Bengaluru
Mumbai
Delhi
```

```text
Color
─────
Red
Blue
Green
```

There is no meaningful relationship like:

```text
Mumbai > Bengaluru ❌
Red > Green ❌
```

Think:

> 🏷️ **Nominal = Names without ranking**

---

## 2️⃣ Ordinal Data - Has Order

Categories have a meaningful ranking.

Example:

```text
Customer Satisfaction

Poor
Average
Good
Excellent
```

Here:

```text
Poor < Average < Good < Excellent
```

Another example:

```text
Education Level

High School
Bachelor's
Master's
PhD
```

The categories have a meaningful order, although the "distance" between adjacent levels isn't necessarily equal.

Think:

> 🪜 **Ordinal = Categories with an order**

---

# 🧩 Common Encoding Techniques

The most important techniques to understand are:

```text
Categorical Encoding
        │
   ┌────┼────────────┬─────────────┐
   ↓    ↓            ↓             ↓
Label  Ordinal    One-Hot       Target
Encoding Encoding  Encoding      Encoding
```

Let's understand each.

---

# 1️⃣ Label Encoding 🏷️

**Label Encoding assigns a number to each category.**

Suppose:

```text
Department

HR
Finance
IT
```

Label Encoding might produce:

```text
HR      → 0
Finance → 1
IT      → 2
```

Simple.

### ⚠️ The Problem

The model might interpret:

```text
IT > Finance > HR
```

or even treat the numeric gaps as meaningful.

But these departments have **no natural order**.

Therefore, arbitrary integer encoding of nominal **input features** can be problematic, especially for models that interpret numeric magnitude or distance.

### Where is Label Encoding commonly useful?

For encoding a **target label**:

```text
Spam     → 1
Not Spam → 0
```

or:

```text
Cat → 0
Dog → 1
```

Here the numbers are identifiers for the target classes rather than a claim that one class is "greater."

---

# 2️⃣ Ordinal Encoding 🪜

Use **Ordinal Encoding** when categories have a meaningful order.

Example:

```text
Size

Small
Medium
Large
```

We can intentionally encode:

```text
Small  → 0
Medium → 1
Large  → 2
```

because:

```text
Small < Medium < Large
```

Another example:

```text
Satisfaction

Poor      → 1
Average   → 2
Good      → 3
Excellent → 4
```

### 🧠 Memory Trick

> 🪜 **Ordinal Encoding = Ranking**

---

# 3️⃣ One-Hot Encoding 🔥

This is one of the most important techniques.

Use it when categories have **no meaningful order**.

Suppose:

```text
City

Bengaluru
Mumbai
Delhi
```

Instead of:

```text
Bengaluru → 0
Mumbai    → 1
Delhi     → 2
```

we create a separate binary column for each category:

| City      | Bengaluru | Mumbai | Delhi |
| --------- | --------: | -----: | ----: |
| Bengaluru |         1 |      0 |     0 |
| Mumbai    |         0 |      1 |     0 |
| Delhi     |         0 |      0 |     1 |

So:

```text
Bengaluru → [1, 0, 0]
Mumbai    → [0, 1, 0]
Delhi     → [0, 0, 1]
```

Now the model doesn't see an artificial ranking between the cities.

> 🔥 **One category → One binary indicator column**

---

# 🆚 Label vs Ordinal vs One-Hot Encoding

| Technique           | Best suited for      | Example                          |
| ------------------- | -------------------- | -------------------------------- |
| 🏷️ Label Encoding  | Often target labels  | Spam → 1, Not Spam → 0           |
| 🪜 Ordinal Encoding | Ordered categories   | Small → 0, Medium → 1, Large → 2 |
| 🔥 One-Hot Encoding | Unordered categories | Red → [1,0,0]                    |

The easiest rule:

```text
Does the feature have a meaningful order?
               │
         ┌─────┴─────┐
        YES           NO
         ↓             ↓
     Ordinal       One-Hot
     Encoding      Encoding
```

For the **target variable**, integer/label encoding is commonly fine.

---

# 4️⃣ Target Encoding 🎯

Suppose we want to predict whether a customer buys a product.

We have:

| City      | Purchase |
| --------- | -------: |
| Bengaluru |        1 |
| Bengaluru |        1 |
| Bengaluru |        0 |
| Mumbai    |        0 |
| Mumbai    |        0 |

Instead of creating many binary columns, Target Encoding represents each category using information derived from the **target**.

For example:

```text
Bengaluru → Purchase Rate ≈ 0.67
Mumbai    → Purchase Rate = 0.00
```

Then:

```text
City
  ↓
Target Statistics
  ↓
Numerical Feature
```

This can be useful when a feature has **many categories**.

For example:

```text
Postal Code → 5,000 categories
Product ID  → 50,000 categories
```

One-Hot Encoding could create thousands of columns.

Target Encoding can produce just one numerical feature.

### 🚨 Data Leakage Risk

Target Encoding uses the **target**, so it must be implemented carefully using training-only or out-of-fold statistics.

Otherwise the model can accidentally get information about the answer it is supposed to predict.

---

# 💥 High Cardinality

**Cardinality** means the number of unique categories.

### Low Cardinality

```text
Gender → 3 categories
Plan   → 4 categories
Region → 5 categories
```

One-Hot Encoding is often manageable.

### High Cardinality

```text
Customer ID → 1,000,000
Product ID  → 100,000
Postal Code → 20,000
```

One-hot encoding could create:

```text
Product_1
Product_2
Product_3
...
Product_100000
```

😵 That's a huge feature space.

Depending on the problem, alternatives include:

**Target Encoding, Frequency/Count Encoding, Hashing, learned embeddings**, or reconsidering whether an identifier should be a feature at all.

---

# 5️⃣ Frequency / Count Encoding 🔢

Another useful technique.

Suppose:

```text
City

Mumbai
Mumbai
Mumbai
Delhi
Delhi
Bengaluru
```

Counts:

```text
Mumbai    → 3
Delhi     → 2
Bengaluru → 1
```

Count Encoding replaces the category with its frequency:

```text
Mumbai    → 3
Delhi     → 2
Bengaluru → 1
```

Frequency Encoding could instead use proportions:

```text
Mumbai    → 3/6 = 0.50
Delhi     → 2/6 = 0.33
Bengaluru → 1/6 = 0.17
```

Useful for some high-cardinality features, although categories with identical frequencies become indistinguishable through this feature alone.

---

# 🤖 What About Modern AI and Embeddings?

Traditional ML often turns categories into numbers using:

```text
One-Hot Encoding
Ordinal Encoding
Target Encoding
```

Deep Learning and modern AI frequently use **embeddings**.

Suppose:

```text
King
Queen
Apple
Mango
```

Instead of simple labels:

```text
King  → 1
Queen → 2
Apple → 3
Mango → 4
```

an embedding represents each item using a **dense numerical vector**:

```text
King  → [0.72, -0.14, 0.91, ...]
Queen → [0.69, -0.11, 0.88, ...]
```

These vectors can learn meaningful relationships between items.

This idea is fundamental to:

**LLMs, RAG, recommendation systems, NLP, vector databases, and neural networks.**

But conceptually:

> 🔢 **Encoding = represent information numerically**
> 🧠 **Embedding = learned dense numerical representation**

---

# ⚠️ Encoding and Data Leakage

Just like Scaling and SMOTE, preprocessing should respect the train/test boundary.

Don't learn encoding information from the full dataset:

```text
Full Dataset
     ↓
Fit Encoder ❌
     ↓
Train/Test Split
```

Instead:

```text
Dataset
   ↓
Train/Test Split
   ↓
 ┌─────────────┬──────────────┐
 ↓             ↓
Train          Test
 ↓
Fit Encoder
 ↓
Transform Train
               ↓
         Transform Test
         using SAME Encoder
```

This matters especially for **target encoding** and any encoding that learns statistics from data.

---

# 🐍 Encoding in Scikit-Learn

### 🔥 One-Hot Encoding

```python
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder(handle_unknown="ignore")

X_train_encoded = encoder.fit_transform(X_train)
X_test_encoded = encoder.transform(X_test)
```

`handle_unknown="ignore"` is useful when the test/production data contains a category that wasn't present during training.

---

### 🪜 Ordinal Encoding

```python
from sklearn.preprocessing import OrdinalEncoder

encoder = OrdinalEncoder(
    categories=[["Poor", "Average", "Good", "Excellent"]]
)

X_train_encoded = encoder.fit_transform(X_train)
X_test_encoded = encoder.transform(X_test)
```

Here we explicitly define the correct order.

---

### 🏷️ Target Labels

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

y_train_encoded = encoder.fit_transform(y_train)
```

`LabelEncoder` is primarily intended for the target `y`, not ordinary input columns `X`.

# 🧠 Big Picture

```text
                    CATEGORICAL DATA
                          │
                       Encoding
                          │
              ┌───────────┴───────────┐
              ↓                       ↓
           Nominal                  Ordinal
          No Order                  Ordered
              │                       │
              ↓                       ↓
          One-Hot                 Ordinal
          Encoding                Encoding

High Cardinality
       ↓
Target / Frequency / Hashing / Embeddings
```

# 📝 Quick Revision

> 🔤 **Encoding = Convert categorical information into numerical representations.**

Remember:

**🏷️ Label Encoding** → Commonly encode target classes
**🪜 Ordinal Encoding** → Categories have an order
**🔥 One-Hot Encoding** → Categories have no order
**🎯 Target Encoding** → Encode using target statistics; watch for leakage
**🔢 Frequency Encoding** → Encode using occurrence frequency
**🧠 Embeddings** → Learned dense vector representations

### 🧠 Memory Trick

Imagine an ML model says:

> 🤖 **"I can't calculate with Bengaluru, Mumbai, Small, Medium, or Large. Give me useful numerical representations."**

Encoding acts as the **translator**:

```text
Human Categories
       ↓
    Encoding
       ↓
Machine-Friendly Numbers
       ↓
     ML Model
```

> **Encoding = Translator between categorical data and Machine Learning. 🔤 → 🔢**
