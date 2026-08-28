---
title: "L'idée de Kohn–Sham"
summary: "Comment un problème à N corps insoluble devient un jeu d'équations à une particule."
duration: "25 min"
weight: 2
---

{{< katex >}}

{{< lead >}}
L'équation de Schrödinger à plusieurs électrons est désespérée à résoudre directement. Kohn et
Sham ont trouvé un contournement — et c'est la raison pour laquelle la DFT fonctionne.
{{< /lead >}}

## Le mur

Pour \(N\) électrons en interaction, le hamiltonien couple chaque particule à toutes les autres :

$$
\hat{H} = -\frac{1}{2}\sum_{i=1}^{N}\nabla_i^2 +
\sum_{i} v_\text{ext}(\mathbf{r}_i) +
\sum_{i<j}\frac{1}{|\mathbf{r}_i-\mathbf{r}_j|}.
$$

La fonction d'onde vit en \(3N\) dimensions — un mur exponentiel.

## L'astuce

Remplacer le système en interaction par un système **sans interaction** ayant la *même densité*.
Chaque orbitale obéit alors à une équation à une particule très simple :

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i = \varepsilon_i\,\psi_i,
\qquad
n(\mathbf{r}) = \sum_i^{\text{occ}} |\psi_i(\mathbf{r})|^2 .
$$

{{< alert >}}
**Point clé.** Le potentiel effectif dépend de la densité, qui dépend des orbitales que l'on
cherche. Cette circularité est la raison pour laquelle la DFT se résout de manière
**auto-cohérente**.
{{< /alert >}}

## En code

Toute la méthode est une boucle. Voici son squelette — nous remplissons chaque pièce au fil des
leçons :

```python
def scf(grille, v_ext, n_electrons, tol=1e-6, max_iter=100, melange=0.3):
    n = estimation_initiale(grille, n_electrons)
    for etape in range(max_iter):
        v_eff = v_ext + hartree(grille, n) + v_xc(n)  # leçons 3–4
        H = cinetique(grille) + diag(v_eff)           # leçon 2 (la suivante)
        eps, psi = eigh(H)
        n_new = densite(psi, n_electrons)
        if converge(n, n_new, tol):
            return eps, psi, n_new
        n = (1 - melange) * n + melange * n_new       # mélange simple
    raise RuntimeError("La boucle SCF n'a pas convergé")
```

Dans la leçon suivante, nous rendons `cinetique(grille)` concrète en discrétisant le laplacien
sur une grille.
