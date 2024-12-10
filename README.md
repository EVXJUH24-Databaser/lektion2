---
author: Lektion 2
date: MMMM dd, YYYY
paging: "%d / %d"
---

# Teams lektion 2

## Agenda

1. Frågor och repetition
2. Repetition av grundläggande SQL (övningar 26 - 35)
3. Genomgång av
   1. Constraints
   2. Funktioner
4. Introduktion till PostgreSQL i Java
5. Quiz frågor
6. Övning med handledning

---

# Fråga

Kan man lämna in uppgift 1 på Omniway redan nu, eller är det med fördel att vänta?

# Svar

Det går bra att lämna in. Titta igenom alla svar först och om du känner dig nöjd kan du lämna in.

---

# Fråga

På uppgift 7 i inlämningsuppgiften på Omniway, är tanken att man ska i text beskriva exemplen eller ge ett exempel på hur en kod kan se ut?

# Svar

Ge exempel med SQL kod på hur det kan se ut.

---

# Constraints

Extra krav, begränsningar och funktionalitet som kan appliceras på kolumner.

- `NOT NULL`
  - Värde kan inte vara null och måste få ett värde vid insert
- `UNIQUE`
  - Värde måste vara unikt och får inte finnas (för kolumnen) i tabell redan
- `CHECK`
  - Ser till att värde stämmer med vissa villkor
- `DEFAULT`
  - Kolumn får "default" värde om inget specificeras
- `PRIMARY KEY`
  - Identifierande kolumn, ofta `serial` och `uuid`
- `REFERENCES` (FOREIGN KEY)
  - Refererande kolumn (till en primary key)
- `SERIAL`
  - Tekniskt sätt inte en constraint, men har extra funktionalitet
  - `BIGINT` datatyp i bakgrunden
  - Ökar automatiskt (increment)

---

# Constraints exempel

```sql
CREATE TABLE user_accounts (
  id SERIAL PRIMARY KEY,
  username TEXT UNIQUE NOT NULL,
  email TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  age INT CHECK (age >= 18 AND age <= 120),
  created_at TIMESTAMP DEFAULT current_timestamp,
  online BOOL DEFAULT false,
  manager_id INT REFERENCES user_accounts(id) DEFAULT NULL
);
```

---

# SQL Funktioner

- Matematiska funktioner
- Sträng funktioner
- Tid och datum funktioner
- Aggregate funktioner
- GROUP BY, ORDER BY, HAVING

---

# Matematiska funktioner

- Enkla (+, -, *, /)
- Bitwise (AND, NOT, XOR, SHIFT)
- Funktioner (abs, exp, floor, log, sqrt)

<https://www.postgresql.org/docs/current/functions-math.html>

---

# Exempel på matematiska funktioner

```sql
CREATE TABLE products (price DECIMAL(10, 2));

SELECT price * 1.08 - 5 FROM products;
SELECT sqrt(price) FROM products;
SELECT abs(-price * 50) FROM products;
```

---

# Sträng funktioner

- Concatenation (||)
- Längd på sträng (char_length)
- Omvandla små eller stora bokstäver (lower & upper)
- Substring
- Trim (ltrim, btrim)

Många av dessa funktioner kan och bör göras med kod istället.

<https://www.postgresql.org/docs/current/functions-string.html>

---

# Exempel på sträng funktioner

```sql
CREATE TABLE employees (
    name VARCHAR(255),
    place VARCHAR(255)
);

SELECT 'Welcome to ' || place || ', ' || name || '.' FROM employees;
SELECT place FROM employees WHERE char_length(name) > 3;
SELECT upper(name) FROM employees;
```

---

# Datum och tid funktioner

- Addition och subtraktion (dagar, intervaller m.m)
- Ålder (age)
- Nuvarande tid (current_time, current_date)
- Truncate (date_trunc)
- Extrahera (extract)

<https://www.postgresql.org/docs/current/functions-datetime.html>

---

# Exempel på datum funktioner

```sql
CREATE TABLE appointments (
    start_date DATE,
    end_date DATE
);

INSERT INTO appointments (start_date, end_date) VALUES (current_date, '2020-05-14');
SELECT * FROM appointments WHERE start_date - 30 > '2024-10-01';
SELECT EXTRACT(DAY FROM start_date) FROM appointments WHERE end_date BETWEEN '2019-12-31' AND '2020-01-31';
```

---

# Aggregate funktioner

- Sum
- Count
- Min & max
- Avg

<https://www.postgresql.org/docs/current/functions-aggregate.html>

---

# Exempel på aggregate funktioner

```sql
CREATE TABLE salaries (
    employee_name TEXT,
    salary DECIMAL(10, 2),
    department VARCHAR(255)
);

SELECT avg(salary) FROM salaries;
SELECT count(*) FROM salaries;
SELECT sum(salary) FROM salaries;
```

---

# Andra funktioner

```sql
-- GROUP BY: Lägg alla med samma kolumner i en grupp
SELECT department, avg(salary) FROM salaries GROUP BY department;

-- ORDER BY: Sortera baserat på kolumn
SELECT employee_name, salary FROM salaries ORDER BY salary DESC;

-- HAVING: Inkludera endast grupper som matchar villkor
SELECT department, avg(salary) FROM salaries GROUP BY department HAVING avg(salary) > 30000;
```

<https://www.postgresql.org/docs/current/queries-table-expressions.html#QUERIES-GROUP>

---

# Quiz frågor

- Vad kan jämföras med rader och kolumner i Java?
- Vad gör `select name, power from heroes where power > 9000`?
- Vad saknas: `INSERT ? ?`?
- Vad saknas: `DELETE ? ?`?
- Vilket nyckelord används för att lägga till villkor i queries?
- Vilket nyckelord används för att slå ihop två villkor?
- Vad är skillnaden mellan `LIKE` och `=` i WHERE-villkor?
- Vad gör `DISTINCT` i en SELECT?
- Vilken constraint används för att förhindra negativa nummer?
- Vilket Docker kommando används för att stoppa en container?
- Vad är kommandot för att skapa en ny tabell?
- Vad är `MAX` för typ av funktion?
- Vad gör `SERIAL`?
- Vad gör `UNIQUE`?
- Vad är det för skillnad på `NOT NULL` och `CHECK`?
- Hur många constraints kan en kolumn ha?
