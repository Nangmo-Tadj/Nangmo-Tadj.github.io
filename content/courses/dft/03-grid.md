---
title: "Discretising space"
summary: "Turn the Laplacian into a matrix on a real-space grid."
duration: "20 min"
weight: 3
---

{{< katex >}}

{{< lead >}}
To solve the Kohn–Sham equations numerically we put space on a grid and turn derivatives into
matrices. This lesson builds the kinetic-energy operator.
{{< /lead >}}

## Finite differences

On a uniform 1-D grid with spacing \(h\), the second derivative has the classic three-point
stencil:

$$
\frac{d^2\psi}{dx^2}\bigg|_{x_i} \approx
\frac{\psi_{i-1} - 2\psi_i + \psi_{i+1}}{h^2}.
$$

That's a tridiagonal matrix — cheap to build and to diagonalise.

```python
import numpy as np

def laplacian_1d(n, h):
    main = -2.0 * np.ones(n)
    off  =  1.0 * np.ones(n - 1)
    return (np.diag(main) + np.diag(off, 1) + np.diag(off, -1)) / h**2

# kinetic energy operator T = -1/2 ∇²
def kinetic(n, h):
    return -0.5 * laplacian_1d(n, h)
```

{{< alert >}}
**Try it.** Diagonalise `kinetic(200, 0.05)` for a particle in a box and compare the lowest
eigenvalues to \(\varepsilon_k = \tfrac{1}{2}(k\pi/L)^2\). They should match to a few digits.
{{< /alert >}}

That's the kinetic term done. Next we'll add the potentials and close the self-consistency loop.

{{< pdf src="/docs/dft-cheatsheet.pdf" title="Cheat sheet" desc="Keep the key equations handy" >}}
