---
title: "Research interests in depth"
date: 2026-01-15
description: "A longer look at the methods I work on."
tags: ["dft", "numerical-methods", "electronic-structure"]
categories: ["research"]
showTableOfContents: true
---

{{< katex >}}

_This is a placeholder research page. Use it as a template for describing a theme, method, or
project in detail — including the math you care about._

## Motivation

Electronic-structure theory lets us predict material properties from the laws of quantum
mechanics. The central object is the many-body Schrödinger equation,

$$
\hat{H}\,\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N) = E\,\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N),
$$

which is intractable to solve directly for more than a handful of electrons. Density
Functional Theory reframes the problem in terms of the electron density
\(n(\mathbf{r})\) instead of the full wavefunction, via the Kohn–Sham equations

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i(\mathbf{r})
  = \varepsilon_i\,\psi_i(\mathbf{r}).
$$

## Open questions I find exciting

- How far can simple functionals be pushed before they break?
- What is the right trade-off between accuracy and cost for large systems?

See the hands-on side of this in [**DFT from scratch**](/projects/dft-from-scratch/).
