---
title: "Axes de recherche en détail"
date: 2026-01-15
description: "Un regard plus long sur les méthodes sur lesquelles je travaille."
tags: ["dft", "methodes-numeriques", "structure-electronique"]
categories: ["recherche"]
showTableOfContents: true
---

{{< katex >}}

_Page d'exemple. Elle sert de gabarit pour décrire en détail un thème, une méthode ou un
projet — y compris les mathématiques qui comptent._

## Motivation

La théorie de la structure électronique permet de prédire les propriétés des matériaux à partir
des lois de la mécanique quantique. L'objet central est l'équation de Schrödinger à N corps,

$$
\hat{H}\,\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N) = E\,\Psi(\mathbf{r}_1,\dots,\mathbf{r}_N),
$$

impossible à résoudre directement au-delà de quelques électrons. La théorie de la fonctionnelle
de la densité (DFT) reformule le problème en fonction de la densité électronique
\(n(\mathbf{r})\) plutôt que de la fonction d'onde complète, via les équations de Kohn–Sham

$$
\left[ -\tfrac{1}{2}\nabla^2 + v_\text{eff}(\mathbf{r}) \right]\psi_i(\mathbf{r})
  = \varepsilon_i\,\psi_i(\mathbf{r}).
$$

## Les questions ouvertes qui m'intéressent

- Jusqu'où peut-on pousser des fonctionnelles simples avant qu'elles ne cassent ?
- Quel est le bon compromis entre précision et coût pour les grands systèmes ?

Le versant pratique se trouve dans [**DFT depuis zéro**](/fr/research/projects/dft-from-scratch/).
