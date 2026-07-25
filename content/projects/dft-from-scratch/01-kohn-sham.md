---
title: "1 · Why DFT, and the Kohn–Sham idea"
date: 2026-04-01
description: "From the intractable many-body problem to a solvable set of single-particle equations."
summary: "Part 1 of the DFT-from-scratch series: the theoretical leap that makes electronic-structure calculations possible."
tags: ["dft", "computational-physics", "theory"]
categories: ["projects"]
weight: 1
showTableOfContents: true
---

{{< katex >}}

_Placeholder chapter — full of real theory and code so you can see how a manuscript page
renders. Replace the prose with your own explanations as you develop the series._

## The wall: the many-body problem

For \(N\) interacting electrons, the time-independent Schrödinger equation is

$$
\left[ -\frac{1}{2}\sum_{i=1}^{N}\nabla_i^2
  + \sum_{i} v_\text{ext}(\mathbf{r}_i)
  + \sum_{i<j}\frac{1}{|\mathbf{r}_i-\mathbf{r}_j|} \right]
  \Psi = E\,\Psi .
$$

The wavefunction \(\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N)\) lives in a \(3N\)-dimensional space.
Storing it on even a coarse grid is hopeless for more than a few electrons — this is the
"exponential wall."

## The Hohenberg–Kohn insight

Hohenberg and Kohn showed that the ground-state energy is a **functional of the electron
density** \(n(\mathbf{r})\) — a function of just three variables — not of the full \(3N\)-dimensional
wavefunction:

$$
E[n] = T_s[n] + \int v_\text{ext}(\mathbf{r})\,n(\mathbf{r})\,d\mathbf{r}
       + E_\text{H}[n] + E_\text{xc}[n].
$$

## The Kohn–Sham trick

Kohn and Sham replaced the interacting system with a fictitious **non-interacting** one that
has the *same density*. Each orbital \(\psi_i\) obeys a single-particle equation:

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i = \varepsilon_i\,\psi_i,
\qquad
n(\mathbf{r}) = \sum_{i}^{\text{occ}} |\psi_i(\mathbf{r})|^2 .
$$

The effective potential ties everything together and depends on the density itself:

$$
v_\text{eff}(\mathbf{r}) = v_\text{ext}(\mathbf{r})
  + \underbrace{\int \frac{n(\mathbf{r}')}{|\mathbf{r}-\mathbf{r}'|}\,d\mathbf{r}'}_{\text{Hartree}}
  + \underbrace{\frac{\delta E_\text{xc}[n]}{\delta n(\mathbf{r})}}_{\text{exchange–correlation}} .
$$

Because \(v_\text{eff}\) depends on \(n\), and \(n\) depends on the \(\psi_i\) we're solving for, this
must be solved **self-consistently**.

## The self-consistency loop, in pseudocode

```python
def scf(grid, v_ext, n_electrons, tol=1e-6, max_iter=100, mix=0.3):
    """Skeleton of a Kohn–Sham self-consistent field loop."""
    n = initial_guess(grid, n_electrons)

    for step in range(max_iter):
        v_hartree = solve_poisson(grid, n)          # part 3 of the series
        v_xc      = lda_potential(n)                # part 4
        v_eff     = v_ext + v_hartree + v_xc

        H = kinetic_matrix(grid) + diag(v_eff)      # part 2
        eps, psi = eigh(H)                          # lowest eigenpairs

        n_new = density_from_orbitals(psi, n_electrons)
        if converged(n, n_new, tol):
            print(f"Converged in {step} iterations")
            break

        n = (1 - mix) * n + mix * n_new             # simple linear mixing (part 5)

    return eps, psi, n
```

Every function above is a chapter in this series. Next, we make `kinetic_matrix` concrete by
building the Laplacian on a real-space grid.

> **Coming next:** [Part 2 — Discretizing space](../) _(placeholder link; add the page and
> point this at it)_.

## References

_Placeholder — Hohenberg & Kohn (1964); Kohn & Sham (1965); and a good textbook such as
Martin, *Electronic Structure*._
