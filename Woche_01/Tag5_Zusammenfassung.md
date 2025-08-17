# 📊 Abschlussprojekt Woche 1 – Wahrscheinlichkeiten

## 1️⃣ Binomialverteilung
- Formel: P(X = k) = C(n, k) * p^k * (1-p)^(n-k)
- Beispiel: Münzwurf (10 Würfe, p=0.5)
- Plot der Wahrscheinlichkeiten
- Erwartungswert: E[X] = n * p
- Varianz: Var[X] = n * p * (1-p)

## 2️⃣ Poissonverteilung
- Formel: P(X = k) = (λ^k * e^(-λ)) / k!
- Beispiel: Anrufe im Callcenter (λ=4 pro Stunde)
- Plot der Wahrscheinlichkeiten
- Erwartungswert: E[X] = λ
- Varianz: Var[X] = λ

## 3️⃣ Vergleich
- Binomial: feste Anzahl Versuche, Erfolgswahrscheinlichkeit bekannt
- Poisson: seltene, zufällige Ereignisse pro Zeitintervall
- Grenzfall: Binomial(n groß, p klein) ≈ Poisson(λ = n*p)

## 4️⃣ Mini-Demo
- Simulation in Python (Binomial vs. Poisson)
- Erwartungswerte vergleichen
- Plots nebeneinander darstellen

## 5️⃣ Fazit
- Binomial: geeignet für **diskrete, begrenzte Experimente**
- Poisson: geeignet für **zufällige Ereignisse über Zeit/Raum**
- Beide wichtig in **Physik (Detektoren, Zerfälle)** & **AI (Anomalieerkennung, Warteschlangen)**
