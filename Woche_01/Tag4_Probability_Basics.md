# 📅 Tag 4 – Grundlagen der Wahrscheinlichkeit

## 1️⃣ Begriffe
| Begriff | Bedeutung |
|---------|-----------|
| **Wahrscheinlichkeit (P)** | Maß für die Sicherheit, dass ein Ereignis eintritt (zwischen 0 und 1) |
| **Zufallsvariable** | Variable, deren Wert vom Zufall abhängt |
| **Erwartungswert (E)** | Durchschnittlicher Wert einer Zufallsvariablen über viele Wiederholungen |
| **Varianz (Var)** | Maß für die Streuung der Werte um den Erwartungswert |
| **Standardabweichung (σ)** | Quadratwurzel der Varianz, beschreibt mittlere Abweichung |
| **Unabhängigkeit** | Zwei Ereignisse beeinflussen sich nicht |
| **Bedingte Wahrscheinlichkeit** | Wahrscheinlichkeit eines Ereignisses, gegeben dass ein anderes eingetreten ist |
| **Verteilung** | Beschreibung, wie Wahrscheinlichkeiten über mögliche Werte verteilt sind |

---

## 2️⃣ Einfache Wahrscheinlichkeitsrechnung
**Formel:**
\[ P(E) = \frac{\text{günstige Fälle}}{\text{mögliche Fälle}} \]

**Beispiel:** Würfelwurf – Wahrscheinlichkeit für eine „6“:
\[ P(6) = \frac{1}{6} \approx 0,1667 \]

**In Python:**
```python
favourable = 1
possible = 6
probability = favourable / possible
print(probability)
```

---

## 3️⃣ Erwartungswert & Varianz
**Formeln:**
\[ E(X) = \sum x_i \cdot P(x_i) \]
\[ Var(X) = \sum (x_i - E(X))^2 \cdot P(x_i) \]

**Beispiel:** Erwartungswert beim Würfeln:
\[ E(X) = \frac{1+2+3+4+5+6}{6} = 3,5 \]

**Python:**
```python
import numpy as np

values = np.array([1, 2, 3, 4, 5, 6])
prob = np.ones(6) / 6
E = np.sum(values * prob)
Var = np.sum((values - E)**2 * prob)
print("Erwartungswert:", E)
print("Varianz:", Var)
```

---

## 4️⃣ Wichtige Verteilungen

### **Binomialverteilung**
- Modelliert Anzahl an Erfolgen in fester Anzahl von Versuchen
- Parameter: **n** (Anzahl Versuche), **p** (Erfolgswahrscheinlichkeit)
```python
from scipy.stats import binom
import numpy as np

n, p = 10, 0.5
x = np.arange(0, n+1)
pmf = binom.pmf(x, n, p)
print("Wahrscheinlichkeit genau 5 Erfolge:", pmf[5])
```

### **Normalverteilung**
- Symmetrische Glockenkurve, viele natürliche Phänomene folgen ihr
```python
from scipy.stats import norm
import matplotlib.pyplot as plt
import numpy as np

mu, sigma = 0, 1
x = np.linspace(-4, 4, 100)
pdf = norm.pdf(x, mu, sigma)

plt.plot(x, pdf)
plt.title("Normalverteilung N(0,1)")
plt.grid(True)
plt.show()
```

### **Poissonverteilung**
- Modelliert Anzahl an Ereignissen in festem Zeitraum
```python
from scipy.stats import poisson
import numpy as np

lambda_val = 3
x = np.arange(0, 10)
pmf = poisson.pmf(x, lambda_val)
print("P(X=2):", pmf[2])
```

---

## 5️⃣ Physik- & AI-Bezug
- **Physik:** Fehlerwahrscheinlichkeiten bei Messungen, Zerfallsprozesse, Quantenphysik
- **AI:** Wahrscheinlichkeitsmodelle für Klassifikatoren, Bayes'sche Netze, Unsicherheitsabschätzung in Vorhersagen

---

💡 **Merksatz des Tages:**
> „Wahrscheinlichkeit ist das Werkzeug, um mit Unsicherheit zu rechnen.“
