---
title: "Numerical integration in Python, from scratch"
date: 2026-03-05
description: "Trapezoid, Simpson, and Gauss–Legendre — coded and compared."
summary: "A screencast building three classic quadrature rules in Python and measuring how fast their error shrinks."
tags: ["python", "numerical-methods", "video", "tutorial"]
categories: ["programming"]
showTableOfContents: true
draft: true   # sample content — not published until you say so
---

_Placeholder video page — swap the embed ID below for your own upload._

## Watch

{{< youtube dQw4w9WgXcQ >}}

_(The `youtube` shortcode above takes the video ID from the URL,
e.g. `https://youtube.com/watch?v=**dQw4w9WgXcQ**`.)_

## What's covered

1. The **trapezoid rule** and why its error scales as $O(h^2)$.
2. **Simpson's rule** — one extra idea, two more orders of accuracy: $O(h^4)$.
3. **Gauss–Legendre** quadrature, and why placing the nodes cleverly beats adding more of them.

## Starter code from the video

```python
import numpy as np

def trapezoid(f, a, b, n):
    x = np.linspace(a, b, n + 1)
    y = f(x)
    h = (b - a) / n
    return h * (y[0] / 2 + y[1:-1].sum() + y[-1] / 2)

# Converges to the exact value 2.0 as n grows
print(trapezoid(np.sin, 0, np.pi, 1000))  # ≈ 1.9999983...
```

## Resources

- Full code: _(placeholder — link a GitHub repo or Gist)_
- Related manuscript: [DFT from scratch](/research/projects/dft-from-scratch/) uses these ideas for
  integrating the electron density.
