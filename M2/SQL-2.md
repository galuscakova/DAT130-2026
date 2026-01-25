# DAT130 – SQL 2: Joins and Advanced SELECT

## Context

These exercises are based on the **NewToy** database introduced in the lecture.

### Schema recap

- **Drinkers**(name, addr, phone)
- **Beers**(name, manf)
- **Bars**(name, addr, license, openDate)
- **Likes**(drinker, beer)
- **Sells**(bar, beer, price)
- **Frequents**(drinker, bar)

Assume that all foreign key constraints described in the slides hold.

---

## Exercise 1 – Simple join

Give the names of the beers sold by **Coogee Bay Hotel** together with their prices.

**Expected attributes**: `beer`, `price`

---

## Exercise 2 – Join with selection

Give the names of the bars that sell **Victoria Bitter** for less than 2.50.

**Expected attributes**: `bar`

---

## Exercise 3 – Join across relations

Give the name of each drinker together with the beers they like and the manufacturer of those beers.

**Expected attributes**: `drinker`, `beer`, `manufacturer`

---

## Exercise 4 – DISTINCT and join

Give the **distinct** names of drinkers who frequent a bar that sells **Sparkling Ale**.

**Expected attributes**: `drinker`

---

## Exercise 5 – Attribute name ambiguity

For each bar that **John** frequents, give:
- the bar name
- the beers sold at that bar
- the corresponding price

**Expected attributes**: `bar`, `beer`, `price`

---

## Exercise 6 – Natural join

Give the list of bars and drinkers that are located at the **same address**.

Use either `NATURAL JOIN` or `JOIN ... USING`.

**Expected attributes**: `bar`, `address`, `drinker`

---

## Exercise 7 – Ordering results

Give all beers sold by **Australia Hotel**, together with their price and manufacturer, ordered by price (highest first).

**Expected attributes**: `beer`, `price`, `manufacturer`

---

## Exercise 8 – Cross join

Give all possible combinations of **bars** and **drinkers named John**.

**Expected attributes**: `bar`, `drinker`

---

## Exercise 9 – Reasoning about joins (no SQL)

Answer the following questions in plain text:

1. Why is a `CROSS JOIN` usually dangerous in practice?
2. When is a `NATURAL JOIN` risky to use?
3. Why is it often preferable to use explicit join conditions (`JOIN ... ON`)?

---

## Exercise 10 – Challenge

Give the names of drinkers who like **at least one beer** that is sold at a bar they frequent.

**Expected attributes**: `drinker`

*Hint:* this requires joining **three relations**.

