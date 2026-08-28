---
title: "La force tranquille des tests de permutation"
date: 2026-02-10
description: "Quand les données refusent d'obéir à une loi de manuel, laissez les données définir l'hypothèse nulle."
summary: "Une introduction pratique aux tests de permutation — une façon peu exigeante en hypothèses d'obtenir des p-valeurs quand les formules classiques ne s'appliquent pas."
tags: ["statistiques", "tests-d-hypotheses", "python", "reproductibilite"]
categories: ["statistiques"]
showTableOfContents: true
draft: true   # sample content — not published until you say so
---

{{< katex >}}

_Article d'exemple — il montre le rendu d'un texte de statistiques avec des mathématiques, du
code et une figure._

## Le problème des hypothèses empruntées

Une grande partie des statistiques du quotidien repose sur l'idée qu'une quantité suit une loi
normale. Les vraies données expérimentales — petits échantillons, mesures asymétriques, valeurs
aberrantes bizarres — ne s'y plient souvent pas. Un **test de permutation** contourne
entièrement l'hypothèse : au lieu de comparer votre statistique à une loi théorique, vous
construisez la distribution nulle *à partir de vos propres données*, en mélangeant les
étiquettes.

## L'idée en une équation

Supposons que l'on mesure une statistique \(T_\text{obs}\) (par exemple la différence des
moyennes de deux groupes). Sous l'hypothèse nulle où les étiquettes de groupe ne veulent rien
dire, tous les ré-étiquetages sont également probables. La p-valeur est simplement la fraction
des ré-étiquetages qui produisent une statistique au moins aussi extrême :

$$
p = \frac{1}{N}\sum_{k=1}^{N} \mathbf{1}\!\left[\, |T_k| \ge |T_\text{obs}| \,\right].
$$

## En code

```python
import numpy as np

def test_permutation(a, b, n_perm=10_000, rng=None):
    """Test de permutation bilatéral pour une différence de moyennes."""
    rng = np.random.default_rng(rng)
    observe = a.mean() - b.mean()
    melange = np.concatenate([a, b])
    n_a = len(a)

    compte = 0
    for _ in range(n_perm):
        rng.shuffle(melange)
        diff = melange[:n_a].mean() - melange[n_a:].mean()
        if abs(diff) >= abs(observe):
            compte += 1
    return observe, (compte + 1) / (n_perm + 1)  # le +1 évite p = 0

# Exemple
rng = np.random.default_rng(42)
temoin    = rng.normal(0.0, 1.0, size=20)
traitement = rng.normal(0.6, 1.0, size=18)
obs, p = test_permutation(temoin, traitement, rng=rng)
print(f"différence observée = {obs:.3f},  p = {p:.4f}")
```

## Quand y recourir

- Petits échantillons, quand le théorème central limite n'a pas encore fait son effet.
- Statistiques sans loi nulle analytique propre (médianes, corrélations, scores maison).
- Chaque fois que vous préférez **ne pas** avoir à défendre une hypothèse de distribution devant
  un relecteur.

## Réserves

Les tests de permutation supposent l'**échangeabilité** sous l'hypothèse nulle — mélanger doit
représenter fidèlement « aucun effet ». C'est une hypothèse aussi, mais bien plus faible et
bien plus transparente.

---

_La suite (exemple) : les intervalles de confiance par bootstrap, et pourquoi ils ne sont pas
gratuits._
