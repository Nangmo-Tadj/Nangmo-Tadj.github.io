---
title: "1 · Pourquoi la DFT, et l'idée de Kohn–Sham"
date: 2026-04-01
description: "Du problème à N corps insoluble à un jeu d'équations à une particule que l'on sait résoudre."
summary: "Partie 1 de la série « DFT depuis zéro » : le saut théorique qui rend possibles les calculs de structure électronique."
tags: ["dft", "physique-computationnelle", "theorie"]
categories: ["projets"]
weight: 1
showTableOfContents: true
draft: true   # sample content — not published until you say so
---

{{< katex >}}

_Chapitre d'exemple — rempli de vraie théorie et de vrai code pour montrer le rendu d'une page de
manuscrit. Remplacez le texte par vos propres explications à mesure que la série avance._

## Le mur : le problème à N corps

Pour \(N\) électrons en interaction, l'équation de Schrödinger indépendante du temps s'écrit

$$
\left[ -\frac{1}{2}\sum_{i=1}^{N}\nabla_i^2 +
\sum_{i} v_\text{ext}(\mathbf{r}_i) +
\sum_{i<j}\frac{1}{|\mathbf{r}_i-\mathbf{r}_j|} \right]
\Psi = E\,\Psi .
$$

La fonction d'onde \(\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N)\) vit dans un espace à \(3N\)
dimensions. La stocker, même sur une grille grossière, devient impossible au-delà de quelques
électrons : c'est le « mur exponentiel ».

## L'idée de Hohenberg–Kohn

Hohenberg et Kohn ont montré que l'énergie de l'état fondamental est une **fonctionnelle de la
densité électronique** \(n(\mathbf{r})\) — une fonction de trois variables seulement — et non de
la fonction d'onde à \(3N\) dimensions :

$$
E[n] = T_s[n] + \int v_\text{ext}(\mathbf{r})\,n(\mathbf{r})\,d\mathbf{r} +
E_\text{H}[n] + E_\text{xc}[n].
$$

## L'astuce de Kohn–Sham

Kohn et Sham remplacent le système en interaction par un système fictif **sans interaction**
possédant la *même densité*. Chaque orbitale \(\psi_i\) obéit alors à une équation à une
particule :

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i = \varepsilon_i\,\psi_i,
\qquad
n(\mathbf{r}) = \sum_{i}^{\text{occ}} |\psi_i(\mathbf{r})|^2 .
$$

Le potentiel effectif relie le tout et dépend de la densité elle-même :

$$
v_\text{eff}(\mathbf{r}) = v_\text{ext}(\mathbf{r}) +
\underbrace{\int \frac{n(\mathbf{r}')}{|\mathbf{r}-\mathbf{r}'|}\,d\mathbf{r}'}_{\text{Hartree}} +
\underbrace{\frac{\delta E_\text{xc}[n]}{\delta n(\mathbf{r})}}_{\text{échange–corrélation}} .
$$

Comme \(v_\text{eff}\) dépend de \(n\), et que \(n\) dépend des \(\psi_i\) que l'on cherche, la
résolution doit être **auto-cohérente**.

## La boucle auto-cohérente, en pseudo-code

```python
def scf(grille, v_ext, n_electrons, tol=1e-6, max_iter=100, melange=0.3):
    """Squelette d'une boucle de champ auto-cohérent de Kohn–Sham."""
    n = estimation_initiale(grille, n_electrons)

    for etape in range(max_iter):
        v_hartree = resoudre_poisson(grille, n)     # partie 3 de la série
        v_xc      = potentiel_lda(n)                # partie 4
        v_eff     = v_ext + v_hartree + v_xc

        H = matrice_cinetique(grille) + diag(v_eff) # partie 2
        eps, psi = eigh(H)                          # plus basses paires propres

        n_new = densite_depuis_orbitales(psi, n_electrons)
        if converge(n, n_new, tol):
            print(f"Convergé en {etape} itérations")
            break

        n = (1 - melange) * n + melange * n_new     # mélange linéaire simple (partie 5)

    return eps, psi, n
```

Chaque fonction ci-dessus est un chapitre de la série. Ensuite, nous rendons
`matrice_cinetique` concrète en construisant le laplacien sur une grille en espace réel.

> **La suite :** [Partie 2 — Discrétiser l'espace](../) _(lien d'exemple ; créez la page puis
> pointez ce lien dessus)_.

## Références

_Exemple — Hohenberg & Kohn (1964) ; Kohn & Sham (1965) ; et un bon manuel comme Martin,
*Electronic Structure*._
