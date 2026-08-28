---
title: "The quiet power of permutation tests"
date: 2026-02-10
description: "When your data won't obey a textbook distribution, let the data define the null."
summary: "A worked introduction to permutation testing — an assumption-light way to get p-values when the textbook formulas don't apply."
tags: ["statistics", "hypothesis-testing", "python", "reproducibility"]
categories: ["statistics"]
showTableOfContents: true
draft: true   # sample content — not published until you say so
---

{{< katex >}}

_Placeholder post — here to show how a statistics essay looks with math, code, and a figure._

## The problem with borrowed assumptions

A lot of everyday statistics leans on the assumption that some quantity is normally
distributed. Real experimental data — small samples, skewed measurements, weird outliers —
often isn't. A **permutation test** sidesteps the assumption entirely: instead of comparing
your statistic to a theoretical distribution, you build the null distribution *from your own
data* by shuffling labels.

## The idea in one equation

Suppose we measure a statistic \(T_\text{obs}\) (say, the difference in group means). Under the
null hypothesis that the group labels are meaningless, every relabeling is equally likely. The
p-value is simply the fraction of relabelings that produce a statistic at least as extreme:

$$
p = \frac{1}{N}\sum_{k=1}^{N} \mathbf{1}\!\left[\, |T_k| \ge |T_\text{obs}| \,\right].
$$

## In code

```python
import numpy as np

def permutation_test(a, b, n_perm=10_000, rng=None):
    """Two-sided permutation test for a difference in means."""
    rng = np.random.default_rng(rng)
    observed = a.mean() - b.mean()
    pooled = np.concatenate([a, b])
    n_a = len(a)

    count = 0
    for _ in range(n_perm):
        rng.shuffle(pooled)
        diff = pooled[:n_a].mean() - pooled[n_a:].mean()
        if abs(diff) >= abs(observed):
            count += 1
    return observed, (count + 1) / (n_perm + 1)  # +1 avoids p = 0

# Example
rng = np.random.default_rng(42)
control   = rng.normal(0.0, 1.0, size=20)
treatment = rng.normal(0.6, 1.0, size=18)
obs, p = permutation_test(control, treatment, rng=rng)
print(f"observed diff = {obs:.3f},  p = {p:.4f}")
```

## When to reach for it

- Small samples where the Central Limit Theorem hasn't kicked in.
- Statistics with no clean analytic null (medians, correlations, custom scores).
- Any time you'd rather **not** defend a distributional assumption to a referee.

## Caveats

Permutation tests assume **exchangeability** under the null — shuffling has to be a fair
representation of "no effect." That's an assumption too, just a much weaker and more
transparent one.

---

_Next up (placeholder): bootstrap confidence intervals, and why they're not a free lunch._
