---
title: "DFT from scratch"
description: "Building a Density Functional Theory code from first principles."
summary: "A multi-part manuscript that develops a working DFT engine — theory, discretization, self-consistency, and code."
date: 2026-04-01
tags: ["dft", "computational-physics", "python", "electronic-structure"]
categories: ["projects"]
showTableOfContents: true
---

This series builds a **Density Functional Theory** code from the ground up. The goal isn't to
race a production package — it's to make every step transparent, so that by the end you could
re-derive and re-implement the whole thing yourself.

## The plan

1. **[Why DFT, and the Kohn–Sham idea](01-kohn-sham/)** — from the many-body problem to a
   tractable set of single-particle equations. _(placeholder draft)_
2. **Discretizing space** — a real-space grid, the Laplacian as a matrix. _(coming soon)_
3. **The Hartree potential** — solving Poisson's equation on the grid. _(coming soon)_
4. **Exchange–correlation** — the LDA, in code. _(coming soon)_
5. **The self-consistency loop** — mixing, convergence, and total energy. _(coming soon)_

## Prerequisites

- Comfort with linear algebra (eigenvalue problems) and basic quantum mechanics.
- Python with NumPy/SciPy. No prior DFT experience assumed.

## Companion code

_Placeholder — link the GitHub repository that accompanies this series once it's public._
