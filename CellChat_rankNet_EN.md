# CellChat `rankNet()` and Paired Wilcoxon Test: Key Notes

This document summarizes important caveats when using **CellChat `rankNet(..., mode = "comparison")`**, with a focus on *information flow*, *relative information flow*, and the *paired Wilcoxon test*, to help avoid common misinterpretations.

---

## 1. What Is Information Flow?

* **Definition**:

  * For a given signaling pathway in a single condition,
  * the **sum of communication probabilities** across all sender → receiver cell group pairs.

* Corresponding fields in `rankNet()` output:

  * `contribution` → **raw information flow**
  * `contribution.scaled` → **scaled information flow (for visualization/ranking only)**

⚠️ `contribution.scaled` is **not a linear rescaling**. Different conditions can have different scaling factors; therefore, it **must not be interpreted as a fold change**.

---

## 2. What Does Relative Information Flow (Stacked Plot) Represent?

When `stacked = TRUE`:

* Each signaling pathway is divided into two segments (one per condition)
* **The two segments always sum to 1** (x-axis is fixed to 0–1)

Actual calculation (based on raw values):

```
Rel_A = raw_A / (raw_A + raw_B)
Rel_B = raw_B / (raw_A + raw_B)
```

### Important Behavioral Notes

* **Regardless of `show.raw = TRUE` or `FALSE`**
* `rankNet(..., stacked = TRUE)` **always uses raw values (`contribution`) to compute proportions**
* CellChat **does not provide an option** to compute stacked proportions using `contribution.scaled`

👉 As a result:

* Stacked plots from `gg1_raw` and `gg1_sc` will look identical

---

## 3. Why Does the Stacked Plot Look ~1:1 While the Non-stacked Plot Looks ~3:1?

This indicates that:

* **Raw information flow is nearly identical** between conditions (≈ 1:1)
* **Scaled information flow is amplified** due to different scaling factors across conditions

Conclusion:

> This discrepancy is not a computational error, but a **visual artifact caused by mixing raw and scaled values**.

---

## 4. What Is Paired in the Paired Wilcoxon Test in CellChat?

### The pairing unit is *not* the pathway itself, but:

> **The same signaling pathway × the same sender–receiver cell group pair**

For each pathway (e.g., NOTCH):

1. Identify sender–receiver cell pairs present in both conditions
2. Compare the communication probability of each matched cell pair across conditions
3. Perform a paired Wilcoxon signed-rank test across all matched pairs:

```r
wilcox.test(x, y, paired = TRUE)
```

---

## 5. Why Are Many Pathway Names Shown in Black?

When `do.stat = TRUE`, text color indicates statistical interpretation:

* **Non-black text**:

  * Enriched in one condition, and
  * the paired Wilcoxon test is significant (p < 0.05), or
  * the relative ratio exceeds the tolerance threshold (`tol`)

* **Black text**:

  * Differences between conditions are small, or
  * the paired Wilcoxon test is **not significant**

⚠️ Even if total or scaled information flow is large, inconsistent directions across individual cell-pair changes can easily lead to non-significant results.

---

## 6. Practical Recommendations (Strongly Advised)

### ✅ Figure Interpretation

* **Stacked (relative) plots**:

  * Interpret as **relative allocation of raw information flow**

* **Non-stacked plots**:

  * Explicitly state whether raw or scaled values are used

### ✅ Manuscript Writing

* **Do not** interpret `contribution.scaled` as a fold change
* Statistical testing should be described explicitly as:

> Paired Wilcoxon signed-rank test comparing matched sender–receiver cell pairs between conditions.

---

## 7. If Scaled-based Relative Information Is Needed

CellChat **does not natively support** scaled-based relative information. It must be computed manually:

```
scaled_rel_A = scaled_A / (scaled_A + scaled_B)
```

and visualized separately (e.g., using `ggplot2`).

---

## 8. One-sentence Summary

> In CellChat, relative information flow is always computed from raw information flow; scaled values are intended only for visualization and ranking, and should not be mixed with relative metrics or statistical testing in interpretation.
