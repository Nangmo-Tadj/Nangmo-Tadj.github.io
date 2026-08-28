---
title: "DFT depuis zéro"
description: "Construire un code de théorie de la fonctionnelle de la densité à partir des premiers principes."
summary: "Un manuscrit en plusieurs parties qui développe un moteur DFT fonctionnel — théorie, discrétisation, auto-cohérence et code."
date: 2026-04-01
tags: ["dft", "physique-computationnelle", "python", "structure-electronique"]
categories: ["projets"]
showTableOfContents: true
---

Cette série construit un code de **théorie de la fonctionnelle de la densité** en partant de
zéro. Le but n'est pas de concurrencer un logiciel de production : c'est de rendre chaque étape
transparente, pour qu'à la fin vous puissiez re-dériver et ré-implémenter l'ensemble vous-même.

## Le plan

1. **[Pourquoi la DFT, et l'idée de Kohn–Sham](01-kohn-sham/)** — du problème à N corps à un jeu
   d'équations à une particule que l'on sait résoudre. _(brouillon d'exemple)_
2. **Discrétiser l'espace** — une grille en espace réel, le laplacien sous forme de matrice. _(à venir)_
3. **Le potentiel de Hartree** — résoudre l'équation de Poisson sur la grille. _(à venir)_
4. **Échange–corrélation** — la LDA, en code. _(à venir)_
5. **La boucle auto-cohérente** — mélange, convergence et énergie totale. _(à venir)_

## Prérequis

- De l'aisance en algèbre linéaire (problèmes aux valeurs propres) et en mécanique quantique de base.
- Python avec NumPy/SciPy. Aucune expérience préalable de la DFT n'est supposée.

## Code compagnon

_Exemple — lier ici le dépôt GitHub qui accompagne la série une fois qu'il sera public._
