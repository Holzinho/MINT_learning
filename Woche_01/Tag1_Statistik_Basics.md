# Tag 1 – Statistik & Datenanalyse Basics

## 1️⃣ Begriffe
| Begriff | Bedeutung |
|---------|-----------|
| **Mittelwert (Mean)** | Durchschnitt aller Werte |
| **Median** | Mittlerer Wert einer sortierten Liste (robust gegen Ausreißer) |
| **Varianz** | Misst, wie stark die Werte um den Mittelwert streuen |
| **Standardabweichung (Std)** | Wurzel der Varianz – mittlere Abweichung vom Mittelwert in Originaleinheiten |
| **Korrelation** | Maß für den linearen Zusammenhang zwischen zwei Variablen (zwischen -1 und 1) |
| **Regressionslinie** | Gerade, die den bestmöglichen linearen Zusammenhang zwischen zwei Variablen beschreibt |

---

## 2️⃣ Formeln

**Mittelwert**
```
x̄ = (Σ xᵢ) / n
```

**Varianz (Stichprobe)**
```
σ² = Σ (xᵢ - x̄)² / (n - 1)
```

**Standardabweichung**
```
σ = √σ²
```

**Korrelation (Pearson)**
```
r = Σ (xᵢ - x̄)(yᵢ - ȳ) / [ √Σ (xᵢ - x̄)² · √Σ (yᵢ - ȳ)² ]
```

**Lineare Regression (Geradengleichung)**
```
y = m · x + b
```
- m = Steigung (Gewichtszuwachs pro Inch Größe)
- b = Achsenabschnitt (theoretisches Gewicht bei Größe 0)

---

## 3️⃣ Code-Snippets

**CSV aus Web laden**
```python
import pandas as pd
url = "https://people.sc.fsu.edu/~jburkardt/data/csv/hw_200.csv"
df = pd.read_csv(url)
df.head()
```

**Statistische Kennzahlen**
```python
df['Height(Inches)'].mean()
df['Height(Inches)'].median()
df['Height(Inches)'].var()
df['Height(Inches)'].std()
```

**Korrelation**
```python
df.corr()
df['Height(Inches)'].corr(df['Weight(Pounds)'])
```

**Scatterplot + Regressionslinie**
```python
import numpy as np
import matplotlib.pyplot as plt
x = df['Height(Inches)']
y = df['Weight(Pounds)']
m, b = np.polyfit(x, y, 1)
plt.scatter(x, y, alpha=0.5)
plt.plot(x, m*x + b, color='red')
plt.xlabel("Height (inches)")
plt.ylabel("Weight (pounds)")
plt.show()
```

---

## 4️⃣ Physik- & AI-Bezug
- **Physik:** Statistik hilft, Messreihen zu verstehen, Ausreißer zu erkennen und Fehlerquellen zu bewerten.
- **AI:** Vorverarbeitung von Daten, Erkennung von Mustern, Training von Vorhersagemodellen.

---

💡 **Merksatz des Tages:**
> „Statistik ist das Werkzeug, das Zahlen in Wissen verwandelt – in Physik, AI und allen MINT-Fächern.“
