# 📅 Tag 3 – Airline Analysis (Linear Regression & SARIMA)

## 1️⃣ Begriffe
| Begriff | Bedeutung |
|---------|-----------|
| **Zeitreihe (Time Series)** | Datenpunkte, die in zeitlicher Reihenfolge gesammelt werden |
| **Trend** | Langfristige Aufwärts- oder Abwärtsbewegung der Daten |
| **Saisonalität** | Wiederholende Muster innerhalb eines festen Zeitraums |
| **Linear Regression** | Einfaches Modell zur Vorhersage durch eine Gerade |
| **SARIMA** | Zeitreihenmodell, das Trend + Saisonalität kombiniert |
| **Forecast** | Vorhersage zukünftiger Werte basierend auf historischen Daten |
| **Konfidenzintervall** | Bereich, in dem der wahre Wert mit einer bestimmten Wahrscheinlichkeit liegt |

---

## 2️⃣ Daten laden & bereinigen
```python
import pandas as pd

url = "https://people.sc.fsu.edu/~jburkardt/data/csv/airtravel.csv"
df = pd.read_csv(url)

# Spaltennamen bereinigen
df.columns = df.columns.str.strip().str.replace('"', '')

print(df.head())
```
📌 **Ergebnis:** Tabelle mit Monaten und Passagierzahlen für 1958, 1959 und 1960.

---

## 3️⃣ Linear Regression – Trendmodell ohne Saisonalität
```python
import numpy as np
from sklearn.linear_model import LinearRegression

all_years = pd.concat([df['1958'], df['1959'], df['1960']]).values
X = np.arange(len(all_years)).reshape(-1, 1)
y = all_years

lr_model = LinearRegression()
lr_model.fit(X, y)

future_X = np.arange(len(all_years), len(all_years) + 12).reshape(-1, 1)
predictions_1961 = lr_model.predict(future_X)
```

**Plot:**
```python
import matplotlib.pyplot as plt

months = df['Month']
for year in ['1958', '1959', '1960']:
    plt.plot(df['Month'], df[year], marker='o', label=year)

plt.plot(months, predictions_1961, 'g--', marker='o', label='1961 (LR Predicted)')
plt.xlabel("Month")
plt.ylabel("Passengers")
plt.title("Airline Passengers 1958-1960 + 1961 Forecast (Linear Regression)")
plt.legend()
plt.grid(True)
plt.show()
```
📌 **Vorteil:** Einfaches Modell, erkennt Trend  
📌 **Nachteil:** Ignoriert saisonale Schwankungen

---

## 4️⃣ SARIMA – Modell mit Saisonalität
```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

# Daten in lange Form bringen
df_long = pd.melt(df, id_vars=["Month"], value_vars=['1958', '1959', '1960'],
                  var_name="Year", value_name="Passengers")
df_long['Date'] = pd.to_datetime(df_long['Month'] + " " + df_long['Year'])
df_long = df_long.sort_values("Date").set_index('Date')

ts = df_long['Passengers']

# SARIMA-Modell erstellen
sarima_model = SARIMAX(ts, order=(1,1,1), seasonal_order=(1,1,1,12))
sarima_results = sarima_model.fit(disp=False)

# Prognose für 1961
sarima_forecast = sarima_results.get_forecast(steps=12)
sarima_pred = sarima_forecast.predicted_mean
sarima_conf = sarima_forecast.conf_int()
```

**Plot:**
```python
plt.figure(figsize=(10,6))
plt.plot(ts, label="Historische Daten")
plt.plot(sarima_pred, label="Vorhersage 1961 (SARIMA)", color='green')
plt.fill_between(sarima_conf.index,
                 sarima_conf.iloc[:,0],
                 sarima_conf.iloc[:,1], color='pink', alpha=0.3)
plt.xlabel("Datum")
plt.ylabel("Passagiere")
plt.title("Airline Passengers Forecast mit Saisonalität (SARIMA)")
plt.legend()
plt.grid(True)
plt.show()
```
📌 **Vorteil:** Modelliert Trend **und** wiederkehrende Muster  
📌 **Nachteil:** Komplexer als einfache Regression

---

## 5️⃣ Ergebnisvergleich
| Modell               | Trend | Saisonalität | Genauigkeit bei Wellen |
|----------------------|-------|--------------|------------------------|
| Linear Regression    | ✅    | ❌           | Niedrig                 |
| SARIMA               | ✅    | ✅           | Hoch                    |

---

## 6️⃣ Physik- & AI-Bezug
- **Physik:** Vorhersage von Messwerten (z. B. Temperatur, Energieverbrauch)  
- **AI:** Zeitreihenmodelle werden in **Predictive Maintenance**, **Finanzprognosen** und **Supply Chain Optimierung** eingesetzt

---

💡 **Merksatz des Tages:**
> „Wer Saisonalität ignoriert, sieht nur die halbe Wahrheit der Daten.“
