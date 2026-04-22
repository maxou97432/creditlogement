# Duration effective Bâle II — Guide par type d'amortissement

## C'est quoi la duration au sens réglementaire ?

> **Question centrale :** *"En moyenne, à quelle date la banque récupère-t-elle son argent ?"*

Ce n'est **pas** la date de fin du prêt. C'est une moyenne pondérée dans le temps de tous les flux (capital + intérêts).

### Définition simple

```
         Σ (t × CF_t)       pour t allant de 1 à N
M  =  ──────────────────
          Σ CF_t
```

### Identités de sommation utilisées dans les preuves

On utilisera trois formules classiques :

```
(I)   Σ t            =  N(N+1)/2
      t=1 à N

(II)  Σ t²           =  N(N+1)(2N+1)/6
      t=1 à N

(III) Σ t(N - t + 1) =  N(N+1)(N+2)/6
      t=1 à N
```

> **Preuve de (III) :**
> ```
> Σ t(N+1-t) = (N+1)×Σt  -  Σt²
>            = (N+1)×N(N+1)/2  -  N(N+1)(2N+1)/6
>            = N(N+1)/6 × [ 3(N+1) - (2N+1) ]
>            = N(N+1)/6 × (N+2)
>            = N(N+1)(N+2)/6    ✓
> ```

**Règle réglementaire (Bâle II) :** après calcul, M est toujours clippé entre **1 an** et **5 ans**.

### Intuition clé

```
Flux concentrés au début  →  Duration COURTE  (ex: dégressif)
Flux répartis régulièrement →  Duration INTERMÉDIAIRE
Flux concentrés à la fin  →  Duration LONGUE   (ex: bullet)
```

---

## Type 3 — Progressif / Annuité constante

> Mensualité identique chaque mois. C'est le "prêt français" classique.

### Profil des flux

```
Période :   1       2       3       4
Flux    : [ 2160 ] [ 2160 ] [ 2160 ] [ 2160 ]
            ████    ████    ████    ████      ← flux identiques
```

### Calcul pas à pas

| Période | Flux | Poids (période × flux) |
|:---:|---:|---:|
| 1 | 2 160 € | 1 × 2 160 = **2 160** |
| 2 | 2 160 € | 2 × 2 160 = **4 320** |
| 3 | 2 160 € | 3 × 2 160 = **6 480** |
| 4 | 2 160 € | 4 × 2 160 = **8 640** |
| **Total** | **8 640 €** | **21 600** |

```
M  =  21 600 / 8 640  =  2.5 ans
```

### Formule générale

```
M  =  (N + 1) / 2
```

> Avec N = 4 ans  →  M = (4+1)/2 = **2.5 ans** ✅

### 📐 Preuve

```
Hypothèse : CF_t = a  (constante pour tout t)

Dénominateur :
  Σ CF_t  =  N × a

Numérateur :
  Σ t × CF_t  =  a × Σ t
              =  a × N(N+1)/2          [par identité (I)]

Donc :
  M  =  [ a × N(N+1)/2 ]  /  [ N × a ]
     =  (N+1)/2                         ✓
```

> ✅ Le `a` (annuité) et le taux `r` disparaissent complètement — la formule
> est **exacte** quel que soit le taux, car tous les flux sont identiques.

---

## Type 1 — Linéaire / Capital constant

> Même part de capital remboursée à chaque période. Les intérêts diminuent car l'encours baisse. La mensualité **décroît** dans le temps.

### Profil des flux

```
Période :   1       2       3       4
Flux    : [ 1400 ] [ 1300 ] [ 1200 ] [ 1100 ]
            ████    ███     ██      █         ← flux décroissants
```

### Calcul pas à pas (4 000 € sur 4 ans à 10%)

| Période | Capital | Intérêts | Flux total | Poids |
|:---:|---:|---:|---:|---:|
| 1 | 1 000 € | 400 € | **1 400 €** | 1 × 1 400 = **1 400** |
| 2 | 1 000 € | 300 € | **1 300 €** | 2 × 1 300 = **2 600** |
| 3 | 1 000 € | 200 € | **1 200 €** | 3 × 1 200 = **3 600** |
| 4 | 1 000 € | 100 € | **1 100 €** | 4 × 1 100 = **4 400** |
| **Total** | | | **5 000 €** | **12 000** |

```
M  =  12 000 / 5 000  =  2.4 ans
```

### Formule générale

```
         (N+1)       1 + r × (N+2)/3
M  =  ─────────  ×  ─────────────────
          2          1 + r × (N+1)/2
```

> Avec N = 4, r = 0.10 :
> M = 2.5 × (1.20 / 1.25) = 2.5 × 0.96 = **2.4 ans** ✅

### 📐 Preuve

```
Hypothèse :
  - Capital remboursé à chaque période : K/N
  - Encours en début de période t      : K × (N - t + 1) / N
  - Intérêts en période t              : K × r × (N - t + 1) / N
  - Flux total en t :

    CF_t  =  K/N  +  K×r×(N-t+1)/N
          =  (K/N) × [ 1 + r×(N-t+1) ]

Dénominateur :
  Σ CF_t  =  K/N × [ Σ 1  +  r × Σ(N-t+1) ]
           =  K/N × [ N  +  r × N(N+1)/2 ]       [Σ(N-t+1) = Σt par symétrie = N(N+1)/2]
           =  K × [ 1  +  r(N+1)/2 ]

Numérateur :
  Σ t × CF_t  =  K/N × [ Σt  +  r × Σ t(N-t+1) ]
              =  K/N × [ N(N+1)/2  +  r × N(N+1)(N+2)/6 ]  [par (I) et (III)]
              =  K(N+1)/2 × [ 1  +  r(N+2)/3 ]

Donc :
  M  =  K(N+1)/2 × [1 + r(N+2)/3]
        ─────────────────────────────
             K × [1 + r(N+1)/2]

     =  (N+1)/2  ×  [1 + r(N+2)/3] / [1 + r(N+1)/2]     ✓
```

> 💡 Si r = 0 : le ratio vaut 1, et M = (N+1)/2 — identique au type 3.

---

## Type 4 — Bullet / In fine

> Seuls les intérêts sont payés périodiquement. Le capital est remboursé **en une seule fois** à la fin.

### Profil des flux

```
Période :   1      2      3      4
Capital :   0      0      0    100
Intérêts:  10     10     10     10
            ─      ─      ─    ████  ← capital concentré à la fin
```

### Calcul pas à pas (100 € sur 4 ans à 10%)

| Période | Flux | Poids |
|:---:|---:|---:|
| 1 | 10 € | 1 × 10 = **10** |
| 2 | 10 € | 2 × 10 = **20** |
| 3 | 10 € | 3 × 10 = **30** |
| 4 | 110 € | 4 × 110 = **440** |
| **Total** | **140 €** | **500** |

```
M  =  500 / 140  ≈  3.57 ans
```

### Formule générale

```
       N × (1 + r × (N+1)/2)
M  =  ────────────────────────
              1 + N × r
```

> Avec N = 4, r = 0.10 :
> M = 4 × 1.25 / 1.40 = 5 / 1.40 = **3.57 ans** ✅

### 📐 Preuve

```
Hypothèse :
  - Périodes 1 à N-1 : intérêts seuls    →  CF_t = K × r
  - Période N        : capital + intérêts →  CF_N = K × (1 + r)

Dénominateur :
  Σ CF_t  =  K×r × (N-1)  +  K×(1+r)
           =  Kr×N - Kr + K + Kr
           =  K × (1 + Nr)

Numérateur :
  Σ t × CF_t  =  K×r × Σ t       (de t=1 à N-1)   +   N × K×(1+r)
              =  K×r × (N-1)N/2                    +   NK × (1+r)
              =  NK × [ r(N-1)/2  +  1+r ]
              =  NK × [ 1  +  r×(N-1)/2  +  r ]
              =  NK × [ 1  +  r×(N+1)/2 ]

Donc :
  M  =  NK × [1 + r(N+1)/2]
        ─────────────────────
            K × (1 + Nr)

     =  N × [1 + r(N+1)/2] / (1 + Nr)              ✓
```

> 💡 Si r = 0 : M = N×1/1 = N — tout le cash arrive en T=N.

---

## Type 2 — Dégressif (capital décroissant)

> La part de capital remboursée **diminue** chaque période. Plus de capital est remboursé en début de prêt → duration courte.

### Pourquoi pas de formule exacte ?

La loi de décroissance du capital n'est pas précisée dans les codes de votre base.
Sans connaître le tableau d'amortissement exact, on ne peut pas calculer la somme analytiquement.

### Approximation utilisée

```
M  ≈  N / 3
```

> ⚠️ C'est la seule approximation du lot. Si vous disposez du profil exact de remboursement, on peut calculer M précisément ligne par ligne.

---

## Récapitulatif — Comparaison sur N = 10 ans, r = 2%

| Code | Type | Duration M estimée |
|:---:|---|:---:|
| **4** | Bullet / In fine | **9.17 ans** → clippé à **5** |
| **3** | Progressif (annuité constante) | **5.5 ans** → clippé à **5** |
| **1** | Linéaire (capital constant) | **5.39 ans** → clippé à **5** |
| **2** | Dégressif | **3.33 ans** |
| **9** | Autre (conservateur) | **10 ans** → clippé à **5** |

### Visualisation intuitive

```
  Court ◄─────────────────────────────────────► Long
                                                 
  [Dégressif]  [Linéaire] [Progressif]  [Bullet]
      N/3      (N+1)/2     (N+1)/2          N
      ↑                                    ↑
   Capital                             Capital
  surtout                             surtout
  au début                             à la fin
```

---

## Le clip réglementaire final

```
M_final = max(1, min(5, M_calculé))
```

| Exemple | M calculé | M final |
|---|---:|---:|
| Prêt 6 mois, type 3 | 0.29 an | **1.0 an** (plancher) |
| Prêt 3 ans, type 1 | 1.8 an | **1.8 an** |
| Prêt 30 ans, type 4 | 28.5 ans | **5.0 ans** (plafond) |
