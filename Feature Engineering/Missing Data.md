# 🧩 Missing Data - Explained in Simple Terms

**Missing Data** means some values are **not available** in a dataset.

For example:

| Name |         Age |      Salary | City        |
| ---- | ----------: | ----------: | ----------- |
| Alex |          28 |        $70K | New York    |
| Sam  | **Missing** |        $85K | Chicago     |
| Maya |          32 | **Missing** | Boston      |
| John |          40 |        $95K | **Missing** |

Those empty values are **missing data**.

> 🧠 **Missing Data = Information that should exist in the dataset but is unavailable.**

You may see missing values represented as:

```text
NaN
None
NULL
NA
?
""
```

---

# 🏫 Simple Analogy

Imagine a class survey:

```text
Name     Age     Score

Alex      20      85
Sam       21      ?
Maya      ?       92
John      22      88
```

Sam forgot to enter the score.

Maya forgot to enter the age.

Those blanks are **missing values**.

Machine Learning models usually need us to decide what to do with them before training.

---

# 🤔 Why Does Data Go Missing?

Missing values happen for many reasons:

```text
👤 User didn't provide information
🖥️ System failed to record it
📡 Sensor stopped working
🔗 Data was lost during integration
⌨️ Data-entry error
📋 Question wasn't applicable
🔒 Information wasn't collected
```

For example, an optional **annual income** question may be skipped more often by certain groups.

That matters because sometimes:

> 💡 **Why the value is missing can itself contain information.**

---

# 🎯 Why Is Missing Data a Problem?

Suppose we're predicting house prices:

```text
Area      = 2,000 sq ft
Bedrooms  = 3
Age       = ?
Location  = New York
```

The model expects:

```text
Area + Bedrooms + Age + Location
                 ↑
              Missing
```

Depending on the algorithm, it may:

```text
❌ Fail during training/prediction

or

⚠️ Produce misleading results if missingness
   is handled incorrectly
```

Missing data can also reduce the amount of usable information and introduce **bias** if the missing values aren't random.

---

# 🧠 Three Types of Missing Data

A very important concept is understanding **why** values are missing.

The classic categories are:

```text
Missing Data
     │
 ┌───┼────┐
 ↓   ↓    ↓
MCAR MAR MNAR
```

---

## 1️⃣ MCAR - Missing Completely At Random 🎲

The missing value has **no systematic relationship** with the data.

Example:

A sensor randomly loses a few readings because of a temporary network failure.

```text
Temperature

25
26
?
24
27
```

The missing reading wasn't caused by the temperature itself or another measured characteristic.

> 🎲 **MCAR = Missing randomly for unrelated reasons.**

This is the least problematic type statistically.

---

# 2️⃣ MAR - Missing At Random 🔗

The probability of missingness depends on **other information we have observed**, rather than the missing value itself after accounting for those variables.

Example:

Suppose younger customers are less likely to report income.

```text
Age      Income

22       Missing
24       Missing
45       $100K
50       $120K
```

Income missingness appears related to:

```text
Age → observed
```

So information from Age may help us model or impute Income.

> 🔗 **MAR = Missingness can be explained by other observed variables.**

Despite its name, MAR doesn't simply mean "random."

---

# 3️⃣ MNAR - Missing Not At Random 🚨

The missingness is related to the **missing value itself** or another unobserved factor, even after considering the observed data.

Example:

Suppose people with very high incomes are more likely to avoid reporting their income.

```text
Income

$60K
$80K
?
?
```

The reason Income is missing is related to the Income value itself.

> 🚨 **MNAR = The missingness itself is informative.**

MNAR is particularly difficult because ordinary imputation can introduce substantial bias.

---

# 🧠 MCAR vs MAR vs MNAR

| Type        | Missingness related to         | Simple Example                      |
| ----------- | ------------------------------ | ----------------------------------- |
| 🎲 **MCAR** | Nothing systematic             | Random sensor failure               |
| 🔗 **MAR**  | Other observed data            | Income missingness related to age   |
| 🚨 **MNAR** | Missing/unobserved information | High earners avoid reporting income |

### Memory Trick

> **MCAR → Completely random**
> **MAR → Explainable using observed data**
> **MNAR → Missingness itself carries hidden information**

---

# 🔍 How Do We Find Missing Values?

Before fixing anything, understand the extent and pattern.

In Pandas:

```python
df.isnull()
```

or:

```python
df.isna()
```

To count missing values per column:

```python
df.isna().sum()
```

Example:

```text
Age        20
Salary    150
City        5
Gender      0
```

To calculate percentages:

```python
df.isna().mean() * 100
```

Example:

```text
Age        2%
Salary    15%
City       0.5%
Gender     0%
```

This helps determine how serious the problem is.

---

# 🛠️ How Do We Handle Missing Data?

There are two broad choices:

```text
Missing Data
      │
 ┌────┴─────┐
 ↓          ↓
Delete     Impute
```

But the correct decision depends on **why data is missing, how much is missing, and what the feature represents**.

---

# 1️⃣ Delete Rows 🗑️

Suppose:

```text
100,000 rows
     ↓
Only 50 rows contain missing values
```

If those rows are not systematically different, removing them might have little impact.

```python
df.dropna()
```

### ⚠️ Problem

Imagine:

```text
1,000 rows

400 contain missing values
```

Deleting them means throwing away:

```text
40% of your dataset 😵
```

And if missingness isn't random, deletion can introduce bias.

> 🧠 **Delete rows only when the information loss and bias are acceptable.**

---

# 2️⃣ Delete a Column 🗑️

Suppose:

```text
Customer_ID      → 0% missing
Age              → 2% missing
Salary           → 5% missing
Optional_Field   → 95% missing
```

A feature with **95% missing data** may provide little useful information.

It might make sense to remove it.

But:

> ⚠️ There is no universal rule such as "more than 50% missing → delete."

A highly missing feature could still be valuable.

---

# 3️⃣ Mean Imputation 📊

Replace missing numerical values with the **mean**.

Example:

```text
Salary:

$50K
$60K
?
$70K
```

Mean:

[
\frac{50+60+70}{3}=60
]

So:

```text
$50K
$60K
$60K ← Imputed
$70K
```

### When useful?

For relatively symmetric numerical data without problematic outliers, mean imputation can be a simple baseline.

### ⚠️ Limitation

Mean imputation can reduce variability and distort relationships between features.

---

# 4️⃣ Median Imputation 🎯

Suppose:

```text
Salary:

$50K
$55K
$60K
$65K
$1M
?
```

The `$1M` outlier pulls the mean upward.

The median is much less affected.

So:

```text
Missing → Median
```

can be more appropriate for **skewed numerical data or data with outliers**.

> 🧠 **Mean → Symmetric-ish data**
> **Median → Skewed/outlier-heavy data**

---

# 5️⃣ Mode Imputation 🏷️

The **mode** is the most frequent value.

Suppose:

```text
City:

New York
Chicago
New York
?
Boston
New York
```

Most common:

```text
New York
```

So:

```text
? → New York
```

Mode imputation is commonly used as a simple baseline for **categorical features**.

---

# 📊 Mean vs Median vs Mode

| Technique     | Common Use                   |
| ------------- | ---------------------------- |
| 📊 **Mean**   | Numerical, roughly symmetric |
| 🎯 **Median** | Numerical, skewed/outliers   |
| 🏷️ **Mode**  | Categorical                  |

Memory:

> **Mean → Average**
> **Median → Middle**
> **Mode → Most common**

---

# 6️⃣ Constant / Special-Category Imputation

Sometimes instead of guessing the missing value, explicitly represent it as missing.

For categorical data:

```text
City

New York
Chicago
Missing
Boston
```

You could encode:

```text
Missing → "Unknown"
```

For numerical features, a constant can sometimes be used together with a missing indicator.

This can be useful when:

> **The fact that a value is missing may itself be predictive.**

---

# 7️⃣ Forward Fill & Backward Fill ⏱️

These techniques are often used with **ordered/time-series data**, when appropriate.

Suppose:

```text
Date        Temperature

Monday        25
Tuesday       26
Wednesday      ?
Thursday      28
```

### Forward Fill

Use the previous observation:

```text
Wednesday → 26
```

```python
df.ffill()
```

### Backward Fill

Use the next observation:

```text
Wednesday → 28
```

```python
df.bfill()
```

⚠️ These methods only make sense when the ordering and domain justify carrying values across observations.

---

# 8️⃣ Interpolation 📈

Instead of simply copying the previous/next value, estimate something between nearby observations.

Example:

```text
Monday       20
Tuesday       ?
Wednesday    30
```

Linear interpolation could estimate:

```text
Tuesday → 25
```

Useful in some:

```text
📈 Time Series
🌡️ Sensor Data
📊 Sequential Measurements
```

when the underlying process supports interpolation.

---

# 9️⃣ Model-Based Imputation 🤖

We can use other features to predict the missing value.

Suppose Salary is missing:

```text
Age        → 35
Experience → 10 years
Education  → Master's
Salary     → ?
```

Instead of simply using the overall mean:

```text
Mean Salary → $75K
```

a model can use:

```text
Age
Experience
Education
     ↓
Imputation Model
     ↓
Estimated Salary
```

Techniques include:

```text
KNN Imputer
Iterative Imputer
Regression-based methods
Multiple Imputation
```

These can capture relationships between variables, but they require more care and validation.

---

# 🚩 Missing Indicator

Sometimes create another feature:

```text
Salary      Salary_Missing

$80K              0
Missing           1
$90K              0
Missing           1
```

Then impute Salary while retaining:

```text
Salary_Missing = 1
```

The model can potentially learn that **missingness itself carries useful information**.

---

# 🚨 Critical Rule: Avoid Data Leakage

Suppose we're using median imputation.

Don't calculate the median using the entire dataset:

```text
Full Dataset
     ↓
Calculate Median ❌
     ↓
Train/Test Split
```

The test set has influenced preprocessing.

Instead:

```text
Dataset
    ↓
Train/Test Split
    ↓
 ┌──────────────┬───────────────┐
 ↓              ↓
Train           Test
 ↓
Fit Imputer
 ↓
Transform Train
                ↓
         Transform Test
         using SAME Imputer
```

> 🎯 **Fit preprocessing on training data only.**

This is the same principle we saw with **Scaling, Encoding, and SMOTE**.

---

# 🐍 Pandas Examples

### Find Missing Values

```python
df.isna().sum()
```

### Delete Rows

```python
df.dropna()
```

### Mean

```python
df["Age"] = df["Age"].fillna(df["Age"].mean())
```

### Median

```python
df["Salary"] = df["Salary"].fillna(df["Salary"].median())
```

### Mode

```python
df["City"] = df["City"].fillna(df["City"].mode()[0])
```

These are useful for exploration, but for an ML pipeline, fitting imputers on training data is safer.

---

# 🤖 Scikit-Learn

For production-style ML preprocessing:

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="median")

X_train_imputed = imputer.fit_transform(X_train)
X_test_imputed = imputer.transform(X_test)
```

Remember:

```text
Training → fit_transform()
Testing  → transform()
```

---

# 🎯 Which Method Should I Use?

A useful starting framework:

```text
                   Missing Value
                        │
              ┌─────────┴─────────┐
              ↓                   ↓
          Numerical           Categorical
              │                   │
       ┌──────┴──────┐            ↓
       ↓             ↓        Mode / Unknown
  Symmetric       Skewed
       ↓             ↓
     Mean          Median
```

For ordered/time-series data:

```text
Forward Fill
Backward Fill
Interpolation
```

For more complex relationships:

```text
KNN / Iterative / Model-Based Imputation
```

But always consider **why the value is missing** before selecting a method.

---

# ⚠️ Common Mistakes

**❌ Automatically deleting every row containing NaN**

You may lose valuable data.

**❌ Always replacing numerical missing values with the mean**

The distribution may be skewed or contain outliers.

**❌ Treating missing as zero**

```text
Income = Missing
```

doesn't necessarily mean:

```text
Income = $0
```

Zero is a real value; missing means **unknown/unavailable**.

**❌ Imputing before train/test split**

This can cause data leakage.

**❌ Ignoring the cause of missingness**

Sometimes the missingness itself is important.

# 🧠 Big Picture

```text
                     MISSING DATA
                          │
                    Why missing?
                          │
              ┌───────────┼───────────┐
              ↓           ↓           ↓
             MCAR        MAR         MNAR
                          │
                    Choose Treatment
                          │
        ┌─────────────────┼─────────────────┐
        ↓                 ↓                 ↓
      Delete            Impute          Advanced
                          │                 │
               ┌──────────┼───────┐      KNN
               ↓          ↓       ↓      Model
              Mean      Median   Mode    Interpolation
```

# 📝 Quick Revision

> 🧩 **Missing Data = Values that are unavailable in a dataset.**

Remember:

**🎲 MCAR** → Missing completely at random
**🔗 MAR** → Missingness related to observed variables
**🚨 MNAR** → Missingness related to missing/unobserved information

For handling:

**📊 Mean** → Numerical, roughly symmetric
**🎯 Median** → Numerical, skewed/outliers
**🏷️ Mode** → Categorical
**❓ Unknown category** → Preserve missingness explicitly
**⏱️ Forward/Backward Fill** → Ordered/time-series data when justified
**📈 Interpolation** → Estimate between nearby observations
**🤖 KNN/Model-based** → More advanced imputation

### 🧠 Memory Trick

Imagine a school attendance sheet:

```text
Alex → 90
Sam  → ?
Maya → 85
John → 95
```

Before replacing `?`, ask:

> 🔍 **Why is Sam's score missing?**

Then decide whether to:

> 🗑️ **Delete it**
> 🧩 **Fill it**
> 🤖 **Estimate it**
> 🚩 **Mark it as missing**

That decision-making process is **Missing Data Handling**.
