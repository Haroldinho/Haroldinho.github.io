
# 📘 Causal Inference Summary: Metrics, Bias, and Estimation

## 🔍 Key Concepts & Definitions

### ✅ Selection Bias
- **Definition**: Difference between observed outcome difference and the true average treatment effect (ATE).
- **Formula**:  
  `Selection Bias = E[Y₁|D=1] - E[Y₀|D=0] - ATE`
- **Cause**: Non-random assignment or self-selection into treatment.
- **Example**: More motivated people select into job training, inflating its measured effect.

---

### 🎯 Intent-to-Treat (ITT)
- **Definition**: Effect of being assigned to treatment, regardless of actual compliance.
- **Formula**:  
  `ITT = E[Y | Z=1] - E[Y | Z=0]`
- **Use Case**: Keeps randomization intact; ideal for policy evaluations.
- **Example**: In a vaccine trial, compares all assigned, not just vaccinated.

---

### ✅ Treatment on the Treated (TOT or ATT)
- **Definition**: Effect of treatment on those who actually received it.
- **Formula**:  
  `TOT = E[Y₁ - Y₀ | D = 1]`
- **Use Case**: What the treatment did for actual takers.
- **Example**: Effect of a course on participants, not just assignees.

---

### ⚖️ Local Average Treatment Effect (LATE)
- **Definition**: Effect of treatment for **compliers** (those who only take treatment if assigned).
- **Formula**:  
  `LATE = ITT / [P(D=1|Z=1) - P(D=1|Z=0)]`
- **Use Case**: When using an instrument to deal with non-compliance.
- **Example**: Students who attend tutoring **only if** assigned.

---

### 🧩 Instrumental Variables (IV)
- **Definition**: A variable that affects treatment but not the outcome, except through treatment.
- **Requirements**:
  1. **Relevance**: Z affects D
  2. **Exclusion**: Z does not affect Y directly
- **Use Case**: Corrects for **unobserved confounders**.
- **Example**: Distance to college used as an instrument for education.

---

### ❌ Omitted Variable Bias (OVB)
- **Definition**: Bias in treatment effect estimates when a confounder is left out.
- **Formula** (linear case):  
  `Bias = β₂ × Corr(X₁, X₂)`
- **Example**: Not controlling for ability when estimating education's effect on income.

---

## 🧠 Additional Metrics

| Metric     | Measures                         | Population        | Random Assignment Needed? |
|------------|----------------------------------|-------------------|----------------------------|
| **ITT**    | Effect of treatment assignment   | All assigned      | ✅ Yes                     |
| **TOT/ATT**| Effect on treated individuals    | Treated only      | ❌ Not required            |
| **LATE**   | Effect on compliers              | Compliers         | ✅ Yes + IV                |
| **ATE**    | Effect on whole population       | Everyone          | ✅ Ideally                 |
| **ATU**    | Effect on untreated              | Untreated         | ❌ Not always available    |

---

## 🔁 Compliance Types (IV Context)
- **Compliers**: Take treatment only if assigned.
- **Always-takers**: Always take treatment.
- **Never-takers**: Never take it.
- **Defiers**: Do the opposite (usually ruled out).

---

## ⚠️ Monotonicity Assumption
- **Definition**: There are no **defiers**; treatment assignment doesn't decrease treatment probability.
- **Needed For**: Valid estimation of **LATE** with IVs.

---

## 🔧 Instrument vs. Control Variable

| Feature              | Control Variable                        | Instrumental Variable                |
|----------------------|------------------------------------------|--------------------------------------|
| **Controls for**     | Observed confounders                    | Unobserved confounders               |
| **Use Case**         | Regression, matching, stratification    | 2SLS, Wald Estimator, LATE           |
| **Affects Treatment?** | ✅ Yes                                | ✅ Yes                                |
| **Affects Outcome?** | ✅ Yes                                 | ❌ No (only via treatment)            |
| **Key Assumption**   | No unobserved confounders               | Relevance + Exclusion restriction    |

---

## 📊 Causal DAG

```
Z (Instrument)
|
v
D (Treatment) ----------> Y (Outcome)
 ^                          ^
 |                          |
 +------- U ---------------+
   (Unobserved Confounder)
```

- **Z → D**: Instrument affects treatment.
- **D → Y**: Treatment affects outcome.
- **U → D and Y**: Unobserved confounder creates bias.
- **IV Validity**: Requires Z has no direct path to Y.

---

## 🧠 Suggested Visuals and Simulations (Optional)
- Simulate ITT, TOT, and LATE in Python
- Create DAGs using `graphviz` or `DoWhy`
