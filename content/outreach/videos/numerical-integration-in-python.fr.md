---
title: "Intégration numérique en Python, depuis zéro"
date: 2026-03-05
description: "Trapèzes, Simpson et Gauss–Legendre — codés et comparés."
summary: "Une vidéo qui construit trois règles de quadrature classiques en Python et mesure la vitesse à laquelle leur erreur diminue."
tags: ["python", "methodes-numeriques", "video", "tutoriel"]
categories: ["programmation"]
showTableOfContents: true
---

_Page vidéo d'exemple — remplacez l'identifiant d'intégration ci-dessous par celui de votre
propre vidéo._

## Regarder

{{< youtube dQw4w9WgXcQ >}}

_(Le shortcode `youtube` ci-dessus prend l'identifiant de la vidéo dans l'URL,
par exemple `https://youtube.com/watch?v=**dQw4w9WgXcQ**`.)_

## Au programme

1. La **règle des trapèzes** et pourquoi son erreur se comporte en $O(h^2)$.
2. La **règle de Simpson** — une idée de plus, deux ordres de précision de plus : $O(h^4)$.
3. La quadrature de **Gauss–Legendre**, et pourquoi bien placer les nœuds vaut mieux que d'en ajouter.

## Code de départ de la vidéo

```python
import numpy as np

def trapezes(f, a, b, n):
    x = np.linspace(a, b, n + 1)
    y = f(x)
    h = (b - a) / n
    return h * (y[0] / 2 + y[1:-1].sum() + y[-1] / 2)

# Converge vers la valeur exacte 2.0 quand n augmente
print(trapezes(np.sin, 0, np.pi, 1000))  # ≈ 1.9999983...
```

## Ressources

- Code complet : _(exemple — liez un dépôt GitHub ou un Gist)_
- Manuscrit associé : [DFT depuis zéro](/fr/research/projects/dft-from-scratch/) utilise ces
  idées pour intégrer la densité électronique.
