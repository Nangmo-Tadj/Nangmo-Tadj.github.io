---
title: "The Kohn–Sham idea"
summary: "How an impossible many-body problem becomes a solvable set of single-particle equations."
duration: "25 min"
weight: 2
draft: true   # sample content — not published until you say so
---

{{< katex >}}

{{< lead >}}
The many-electron Schrödinger equation is hopeless to solve directly. Kohn and Sham found a way
around it — and it's the reason DFT works at all.
{{< /lead >}}

## The wall

For \(N\) interacting electrons the Hamiltonian couples every particle to every other one:

$$
\hat{H} = -\frac{1}{2}\sum_{i=1}^{N}\nabla_i^2 +
\sum_{i} v_\text{ext}(\mathbf{r}_i) +
\sum_{i<j}\frac{1}{|\mathbf{r}_i-\mathbf{r}_j|}.
$$

The wavefunction lives in \(3N\) dimensions — an exponential wall.

## The trick

Replace the interacting system with a **non-interacting** one that has the *same density*.
Each orbital then obeys a simple single-particle equation:

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i = \varepsilon_i\,\psi_i,
\qquad
n(\mathbf{r}) = \sum_i^{\text{occ}} |\psi_i(\mathbf{r})|^2 .
$$

{{< alert >}}
**Key point.** The effective potential depends on the density, which depends on the orbitals we're
solving for. That circularity is why DFT is solved **self-consistently**.
{{< /alert >}}

## In code

The whole method is a loop. Here's its skeleton — we fill in each piece over the next lessons:

```python
def scf(grid, v_ext, n_electrons, tol=1e-6, max_iter=100, mix=0.3):
    n = initial_guess(grid, n_electrons)
    for step in range(max_iter):
        v_eff = v_ext + hartree(grid, n) + v_xc(n)   # lessons 3–4
        H = kinetic(grid) + diag(v_eff)              # lesson 2 (next)
        eps, psi = eigh(H)
        n_new = density(psi, n_electrons)
        if converged(n, n_new, tol):
            return eps, psi, n_new
        n = (1 - mix) * n + mix * n_new              # simple mixing
    raise RuntimeError("SCF did not converge")
```

In the next lesson we make `kinetic(grid)` concrete by discretising the Laplacian on a grid.
