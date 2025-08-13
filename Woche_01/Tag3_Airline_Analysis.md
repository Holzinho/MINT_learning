Perfekt 👍
Hier ist deine fertige **`Tag3_Airline_Analysis.md`** mit allen Erklärungen, Code und Kontext, sodass du sie direkt ins GitHub-Repo legen kannst.

---

```markdown
# 📅 Tag 3 – Airline Analysis mit Linear Regression & SARIMA

Heute kombinieren wir Statistik mit Machine Learning und Zeitreihenanalyse.  
Wir analysieren die berühmten **Airline Passenger Daten** (1958–1960) und erstellen Vorhersagen für 1961 –  
einmal **ohne Saisonalität** (Linear Regression) und einmal **mit Saisonalität** (SARIMA).

---

## 1️⃣ Datenquelle & Vorbereitung

Wir nutzen eine öffentliche CSV-Datei:  
```

[https://people.sc.fsu.edu/\~jburkardt/data/csv/airtravel.csv](https://people.sc.fsu.edu/~jburkardt/data/csv/airtravel.csv)

````

### Laden & Bereinigen
```python
import pandas as pd

url = "https://people.sc.fsu.edu/~jburkardt/data/csv/airtravel.csv"
df = pd.read_csv(url)

# Spaltennamen bereinigen
df.columns = df.columns.str.strip().str.replace('"', '')

print(df.head())
````

**Ergebnis:** Tabelle mit `Month`, `1958`, `1959`, `1960` und Passagierzahlen.

---

## 2️⃣ Linear Regression – Einfaches Trendmodell

### Idee

* Wir setzen alle Monate als **X-Werte** (0–35)
* Passagierzahlen sind **y-Werte**
* Das Modell sucht eine **Gerade** `y = m*x + b`, die den Trend beschreibt.

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

### Plot

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

---

## 3️⃣ SARIMA – Saisonalität berücksichtigen

### Warum SARIMA?

* Linear Regression erkennt **keine saisonalen Wellen**
* SARIMA modelliert **Trend + Saisonalität** gleichzeitig
* Perfekt für Zeitreihen mit regelmäßigen Mustern (Flüge, Verkäufe, Energieverbrauch)

### Daten in Zeitreihe umwandeln

```python
df_long = pd.melt(df, id_vars=["Month"], value_vars=['1958', '1959', '1960'],
                  var_name="Year", value_name="Passengers")

df_long['Date'] = pd.to_datetime(df_long['Month'] + " " + df_long['Year'])
df_long = df_long.sort_values("Date")
df_long.set_index('Date', inplace=True)

ts = df_long['Passengers']
```

### SARIMA-Modell

```python
from statsmodels.tsa.statespace.sarimax import SARIMAX

sarima_model = SARIMAX(ts, order=(1,1,1), seasonal_order=(1,1,1,12))
sarima_results = sarima_model.fit(disp=False)

sarima_forecast = sarima_results.get_forecast(steps=12)
sarima_pred = sarima_forecast.predicted_mean
sarima_conf = sarima_forecast.conf_int()
```

### Plot

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

---

## 4️⃣ Ergebnisvergleich

| Modell            | Trend | Saisonalität | Genauigkeit bei Wellen |
| ----------------- | ----- | ------------ | ---------------------- |
| Linear Regression | ✅     | ❌            | Niedrig                |
| SARIMA            | ✅     | ✅            | Hoch                   |

---

## 5️⃣ AI-Teil: Warum das wichtig ist

* **MINT-Berufe** brauchen oft **Prognosen** (Produktion, Nachfrage, Energie)
* Reine Trendmodelle (LR) sind zu simpel für komplexe Muster
* Zeitreihenmodelle wie **SARIMA** oder **Prophet** sind industrieweit Standard
* Dein Wissen hier ist **direkt beruflich nutzbar**

---

✍ **Zusammenfassung:**
Heute hast du gelernt, wie man **Machine Learning** und **klassische Statistik** kombiniert:

1. **Daten laden & bereinigen**
2. **Trend erkennen (LR)**
3. **Saisonalität modellieren (SARIMA)**
4. **Vorhersagen realistisch machen**

Nächster Schritt: Prophet-Modell testen und vergleichen.

```

---

Willst du, dass ich dir gleich noch die passende **Git-Commit-Message** schreibe,  
damit du das sauber ins Repo pushen kannst?
```
