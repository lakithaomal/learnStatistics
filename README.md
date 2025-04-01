# learnStatistics

## Hypothesis Testing 
Hypothesis testing is a statistical method used to make decisions or inferences about population parameters based on sample data. It involves testing an assumption (called the null hypothesis) against an alternative claim (called the alternative hypothesis) to determine if there is enough evidence to reject the assumption.

### Null Hypothesis 
An assumption that treats everything equal or similar.
  - Null hypothesis (H₀): The default or initial assumption (e.g., “There is no effect” or “The means are equal”).
  - Alternative hypothesis (H₁ or Hₐ): The claim you want to test (e.g., “There is an effect” or “The means are different”).

 Some Examples 
  - H_0: The new drug has no difference in effect compared to the standard.0
  - H_1: The new drug has a greater effect than the standard.

 
| **Decision**             | **H₀ is True**               | **H₁ is True (H₀ is False)**     |
|--------------------------|------------------------------|----------------------------------|
| **Fail to Reject H₀**    | ✅ **Correct Decision**       | ❌ **Type II Error**             |
| **Reject H₀**            | ❌ **Type I Error**           | ✅ **Correct Decision**          |

### P Value 
The P value is the probabilty of the null hypothesis being true. 

### Significance Level
  Common values: 0.05, 0.01, or 0.10. It represents the probability of rejecting the null hypothesis when it’s actually true (Type I error).

### Appropriate Test:
Based on the type of data and hypothesis (e.g., t-test, z-test, chi-square test, ANOVA). A value that tells how far the sample result is from the null hypothesis expectation. The probability of observing the sample data, or something more extreme, if the null hypothesis is true.

## One-sample proportion test :

You have a binary outcome (success/failure, yes/no, 1/0) and want to test if the observed proportion differs from a known or expected proportion.
  Suppose 60 out of 100 people prefer a new product. Is this proportion significantly different from 50%?
  ```
  from statsmodels.stats.proportion import proportions_ztest

  # Number of successes, number of observations
  successes = 60
  n = 100
  p0 = 0.5  # hypothesized proportion The null hypothesis is -  The true population proportion is equal to 0.5.
  
  stat, pval = proportions_ztest(count=successes, nobs=n, value=p0)
  print("Z-statistic:", stat)
  print("p-value:", pval)
  ```
## T Test
A t-test is a statistical test used to compare means. 
It answers questions like:
  - “Is this sample mean different from a known value?”
  - “Are these two groups different from each other?”

### One-sample t-test
👉 Compares the sample mean to a known value.
You measured the weights of 5 apples: [150, 155, 148, 152, 151]
You want to know if their average is different from 150g.

🧪 Hypotheses:
 - H₀ (Null): The true mean weight μ = 150g
 - H₁ (Alt): The true mean weight μ ≠ 150g (two-sided test)


```
import statsmodels.api as sm
import numpy as np

weights = np.array([150, 155, 148, 152, 151])
t_stat, p_value, df = sm.stats.ttest_1samp(weights, popmean=150)

print("One-sample t-test:")
print("H₀: μ = 150")
print("H₁: μ ≠ 150")
print("t-statistic:", t_stat)
print("p-value:", p_value)
print("degrees of freedom:", df)
```

### Two-Sample t-test (Independent)
📊 Scenario:

You have test scores from two different classes:
  - Class A: [85, 87, 90, 88, 86]
  - Class B: [78, 80, 75, 77, 79, 75, 77, 7]

🧪 Hypotheses:
	- H₀ (Null): The means of the two groups are equal → μ₁ = μ₂
	- H₁ (Alt): The means are different → μ₁ ≠ μ₂
 
```
class_a = np.array([85, 87, 90, 88, 86])
class_b = np.array([78, 80, 75, 77, 79, 75, 77, 7]) # Can be different sizes 

t_stat, p_value, df = sm.stats.ttest_ind(class_a, class_b)

print("\nTwo-sample t-test:")
print("H₀: μ₁ = μ₂")
print("H₁: μ₁ ≠ μ₂")
print("t-statistic:", t_stat)
print("p-value:", p_value)
print("degrees of freedom:", df)
```

### Paired t-test (Dependent)

📊 Scenario:

Blood pressure before and after treatment:
	- Before: [130, 128, 135, 133, 129]
	- After: [125, 124, 130, 128, 126]

🧪 Hypotheses:
	- H₀ (Null): There is no change in mean → μ_before = μ_after
	- H₁ (Alt): There is a change → μ_before ≠ μ_after

You could also use one-sided alternative if you expect a specific direction (e.g., decrease only).

```
before = np.array([130, 128, 135, 133, 129])
after = np.array([125, 124, 130, 128, 126])

t_stat, p_value, df = sm.stats.ttest_rel(before, after)

print("\nPaired t-test:")
print("H₀: μ_before = μ_after")
print("H₁: μ_before ≠ μ_after")
print("t-statistic:", t_stat)
print("p-value:", p_value)
print("degrees of freedom:", df)
```

| **Test**              | **Null Hypothesis (H₀)**                            |
|-----------------------|-----------------------------------------------------|
| One-sample t-test     | Mean of sample = known value (e.g., μ = 150)        |
| Two-sample t-test     | Means of two groups are equal (μ₁ = μ₂)             |
| Paired t-test         | Mean difference between pairs = 0 (μ_before = μ_after) |



# 📊 Chi-Square Test for Independence using `tips` Dataset

## ✅ What is the Chi-Square Test?

The **Chi-Square Test for Independence** checks if two categorical variables are **statistically associated** or **independent**.

---

## 📚 Dataset: `sns.load_dataset('tips')`

We’ll use the built-in **tips** dataset from Seaborn:

- **`sex`**: Male or Female
- **`smoker`**: Yes or No

---

## 🧪 Research Question

> Is there a **relationship** between a person's **gender** and whether they are a **smoker**?

---

## 🧪 Hypotheses

- **H₀ (Null)**:  
  Gender and smoking status are **independent** (no association)

- **H₁ (Alternative)**:  
  Gender and smoking status are **not independent** (there is an association)

---

## 🐍 Python Code

```python
import seaborn as sns
import pandas as pd
from scipy.stats import chi2_contingency

# Load the dataset
tips = sns.load_dataset('tips')

# Create a contingency table of 'sex' vs 'smoker'
table = pd.crosstab(tips['sex'], tips['smoker'])

print("Contingency Table:")
print(table)
# Contingency Table:
# smoker  No  Yes
# sex            
# Male    97   60
# Female  54   33

# Perform the Chi-Square Test of Independence
chi2, p, dof, expected = chi2_contingency(table)

# Print results
print("\nChi-square statistic:", chi2)
print("p-value:", p)
print("Degrees of freedom:", dof)
print("Expected frequencies:\n", expected)

 
 ### Making the Decision:
  If the p-value ≤ α, reject the null hypothesis (evidence supports the alternative).
  If the p-value > α, fail to reject the null hypothesis (not enough evidence to support the alternative).
