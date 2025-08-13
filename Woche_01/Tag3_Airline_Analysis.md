Klar 👍
Hier ist dein vollständiges **Tag3\_Airline\_Analysis.md** als fertige Markdown-Datei, die du direkt in dein `MINT_learning` Repo legen kannst:

---

````markdown
# Tag 3 – Airline Passenger Analysis (1958–1960)

## 1. Datenbereinigung & Plot

Wir nutzen die CSV-Daten `airtravel.csv` mit monatlichen Passagierzahlen von 1958–1960.  
Die Originalspalten enthielten Leerzeichen und Anführungszeichen, die wir entfernt haben:

```python
df.columns = df.columns.str.strip().str.replace('"', '')
````

**Plot mit Monatsnamen:**

```python
for year in ['1958', '1959', '1960']:
    plt.plot(df['Month'], df[year], marker='o', label=year)
plt.xlabel("Month")
plt.ylabel("Passengers")
plt.legend()
plt.show()
```

---

## 2. Korrelation zwischen den Jahren

Korrelation misst den linearen Zusammenhang zwischen zwei Variablen.
Formel (Pearson-Korrelation):

$$
r = \frac{\text{Cov}(X,Y)}{\sigma_X \cdot \sigma_Y}
$$

```python
df[['1958', '1959', '1960']].corr()
```

**Ergebnis:** Werte \~0.99 → Die Jahresverläufe sind fast identisch.

---

## 3. Wachstumsrate pro Jahr

Formel:

$$
\text{Wachstumsrate} = \frac{\text{Ø Jahr X} - \text{Ø Jahr X-1}}{\text{Ø Jahr X-1}} \times 100
$$

```python
growth_59 = ((df['1959'].mean() - df['1958'].mean()) / df['1958'].mean()) * 100
growth_60 = ((df['1960'].mean() - df['1959'].mean()) / df['1959'].mean()) * 100
```

**Beispielergebnis:**

* 1958 → 1959: +6,83%
* 1959 → 1960: +6,20%

---

## 4. Trendlinie

Wir verwenden **lineare Regression** mit `numpy.polyfit`:

$$
y = m \cdot x + b
$$

```python
all_years = pd.concat([df['1958'], df['1959'], df['1960']]).values
x = np.arange(len(all_years))
m, b = np.polyfit(x, all_years, 1)
trend = m * x + b
```

Die Trendlinie zeigt einen klaren, positiven Anstieg.

---

## 5. AI-Teil – Vorhersage für 1961

Wir nutzen `LinearRegression` aus scikit-learn, um die Passagierzahlen für 1961 vorherzusagen.

```python
from sklearn.linear_model import LinearRegression
import numpy as np

# Trainingsdaten (Monatsindex, Passagiere)
X = np.arange(len(all_years)).reshape(-1, 1)
y = all_years

model = LinearRegression()
model.fit(X, y)

# Prognose für die nächsten 12 Monate (1961)
future_X = np.arange(len(all_years), len(all_years) + 12).reshape(-1, 1)
predictions_1961 = model.predict(future_X)

# Ausgabe
months = df['Month']
pred_df = pd.DataFrame({'Month': months, '1961_Predicted': predictions_1961})
print(pred_df)

# Plot mit Prognose
plt.figure(figsize=(10,6))
for year in ['1958', '1959', '1960']:
    plt.plot(df['Month'], df[year], marker='o', label=year)
plt.plot(months, predictions_1961, 'g--', marker='o', label='1961 (Predicted)')
plt.xlabel("Month")
plt.ylabel("Passengers")
plt.legend()
plt.show()
```

**Interpretation:**
Das Modell sagt für 1961 in jedem Monat höhere Passagierzahlen voraus, da es den positiven Trend aus 1958–1960 extrapoliert.

---

## Lernziele heute

* Daten bereinigen und sinnvoll visualisieren
* Korrelation interpretieren
* Wachstumsraten berechnen
* Trendlinien erstellen
* Einfaches ML-Modell zur Vorhersage einsetzen

```

---

Willst du, dass ich dir gleich den **Git Commit Text** für diese Datei mitgebe,  
damit du sie direkt sauber zu GitHub pushen kannst?
```
