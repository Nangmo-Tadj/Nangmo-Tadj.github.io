---
title: "Discrétiser l'espace"
summary: "Transformer le laplacien en matrice sur une grille en espace réel."
duration: "20 min"
weight: 3
---

{{< katex >}}

{{< lead >}}
Pour résoudre numériquement les équations de Kohn–Sham, on place l'espace sur une grille et on
transforme les dérivées en matrices. Cette leçon construit l'opérateur d'énergie cinétique.
{{< /lead >}}

## Différences finies

Sur une grille 1-D uniforme de pas \(h\), la dérivée seconde s'écrit avec le classique stencil à
trois points :

$$
\frac{d^2\psi}{dx^2}\bigg|_{x_i} \approx
\frac{\psi_{i-1} - 2\psi_i + \psi_{i+1}}{h^2}.
$$

C'est une matrice tridiagonale — peu coûteuse à construire comme à diagonaliser.

```python
import numpy as np

def laplacien_1d(n, h):
    diagonale = -2.0 * np.ones(n)
    hors_diag =  1.0 * np.ones(n - 1)
    return (np.diag(diagonale) + np.diag(hors_diag, 1) + np.diag(hors_diag, -1)) / h**2

# opérateur d'énergie cinétique T = -1/2 ∇²
def cinetique(n, h):
    return -0.5 * laplacien_1d(n, h)
```

{{< alert >}}
**À essayer.** Diagonalisez `cinetique(200, 0.05)` pour une particule dans une boîte et comparez
les plus basses valeurs propres à \(\varepsilon_k = \tfrac{1}{2}(k\pi/L)^2\). L'accord doit tenir
sur quelques chiffres.
{{< /alert >}}

Le terme cinétique est fait. Ensuite, nous ajouterons les potentiels et fermerons la boucle
auto-cohérente.

{{< pdf src="/docs/dft-cheatsheet.pdf" title="Fiche de synthèse" desc="Gardez les équations clés à portée" >}}
