# Tag 2 – SQL Basics

## 1️⃣ Begriffe
| Begriff | Bedeutung |
|---------|-----------|
| **SELECT** | Wählt Spalten aus einer Tabelle aus |
| **FROM** | Gibt an, aus welcher Tabelle die Daten kommen |
| **WHERE** | Filtert Zeilen nach einer Bedingung |
| **JOIN** | Verknüpft zwei Tabellen miteinander |
| **GROUP BY** | Gruppiert Daten für Aggregationen |
| **ORDER BY** | Sortiert das Ergebnis |
| **SUM()** | Summiert Werte |
| **COUNT()** | Zählt Zeilen/Werte |

---

## 2️⃣ Tabellen anlegen

```sql
CREATE TABLE customers (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    country TEXT
);

CREATE TABLE orders (
    id INTEGER PRIMARY KEY,
    customer_id INTEGER,
    product TEXT,
    amount INTEGER,
    FOREIGN KEY (customer_id) REFERENCES customers(id)
);
```

---

## 3️⃣ Daten einfügen

```sql
INSERT INTO customers (name, country) VALUES
('Alice', 'Austria'),
('Bob', 'Germany'),
('Charlie', 'Austria');

INSERT INTO orders (customer_id, product, amount) VALUES
(1, 'Laptop', 1),
(1, 'Mouse', 2),
(2, 'Keyboard', 1),
(3, 'Monitor', 1),
(3, 'Mouse', 1);
```

---

## 4️⃣ Grundlegende Abfragen

**Alle Kunden:**
```sql
SELECT * FROM customers;
```

**Nur Name und Land:**
```sql
SELECT name, country FROM customers;
```

**Kunden aus Österreich:**
```sql
SELECT * FROM customers WHERE country = 'Austria';
```

---

## 5️⃣ JOIN – Tabellen verknüpfen

**Alle Bestellungen mit Kundendaten:**
```sql
SELECT customers.name, customers.country, orders.product, orders.amount
FROM orders
JOIN customers ON orders.customer_id = customers.id;
```

**Nur Bestellungen aus Österreich:**
```sql
SELECT customers.name, orders.product
FROM orders
JOIN customers ON orders.customer_id = customers.id
WHERE customers.country = 'Austria';
```

---

## 6️⃣ Sortieren

**Alphabetisch nach Kunden:**
```sql
SELECT customers.name, orders.product, orders.amount
FROM orders
JOIN customers ON orders.customer_id = customers.id
ORDER BY customers.name ASC;
```

---

## 7️⃣ Aggregatfunktionen

**SUM – Gesamtanzahl Produkte pro Kunde:**
```sql
SELECT customers.name, SUM(orders.amount) AS total_items
FROM orders
JOIN customers ON orders.customer_id = customers.id
GROUP BY customers.name;
```

**COUNT – Anzahl Bestellungen pro Kunde:**
```sql
SELECT customers.name, COUNT(orders.id) AS total_orders
FROM orders
JOIN customers ON orders.customer_id = customers.id
GROUP BY customers.name;
```

**SUM + COUNT kombiniert:**
```sql
SELECT customers.name,
       COUNT(orders.id) AS total_orders,
       SUM(orders.amount) AS total_items
FROM orders
JOIN customers ON orders.customer_id = customers.id
GROUP BY customers.name;
```

**Sortieren nach den meisten Produkten:**
```sql
SELECT customers.name, SUM(orders.amount) AS total_items
FROM orders
JOIN customers ON orders.customer_id = customers.id
GROUP BY customers.name
ORDER BY total_items DESC;
```

---

## 8️⃣ Physik- & AI-Bezug
- **Physik:** JOINs verbinden Messwerte mit Messquellen (z. B. Gerätetyp + Messergebnisse).
- **AI:** JOINs verknüpfen Trainingsdaten mit Labels oder Metadaten.

---

💡 **Merksatz des Tages:**
> „SQL ist die Sprache, mit der Daten anfangen, Geschichten zu erzählen.“
