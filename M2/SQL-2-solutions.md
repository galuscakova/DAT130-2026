# DAT130 – SQL 2: Solutions

---

## Solution 1 – Simple select

```sql
SELECT beer, price
FROM Sells
WHERE bar = 'Coogee Bay Hotel';
```

## Solution 2 - Query with selection

```sql
SELECT bar
FROM Sells
WHERE beer = 'Victoria Bitter'
  AND price < 2.5;
```

## Solution 3 – Join across relations

```sql
SELECT Likes.drinker AS drinker,
       Likes.beer AS beer,
       Beers.manf AS manufacturer
FROM Likes
JOIN Beers ON Likes.beer = Beers.name;
```

## Solution 4 – DISTINCT and join

```sql
SELECT DISTINCT Frequents.drinker AS drinker
FROM Frequents
JOIN Sells ON Frequents.bar = Sells.bar
WHERE Sells.beer = 'Sparkling Ale';
```

## Solution 5 – Attribute name ambiguity

```sql
SELECT Sells.bar,
       Sells.beer,
       Sells.price
FROM Sells
JOIN Frequents ON Sells.bar = Frequents.bar
WHERE Frequents.drinker = 'John';
```

## Solution 6 – Natural join

Using JOIN ... USING:

```sql
SELECT Bars.name AS bar,
       addr AS address,
       Drinkers.name AS drinker
FROM Bars
JOIN Drinkers USING (addr);
```

Equivalent version using JOIN ... ON:

```sql
SELECT Bars.name AS bar,
       Bars.addr AS address,
       Drinkers.name AS drinker
FROM Bars
JOIN Drinkers ON Bars.addr = Drinkers.addr;
```

## Solution 7 – Ordering results

```sql
SELECT Sells.beer,
       Sells.price,
       Beers.manf AS manufacturer
FROM Sells
JOIN Beers ON Sells.beer = Beers.name
WHERE Sells.bar = 'Australia Hotel'
ORDER BY Sells.price DESC;
```

## Solution 8 – Cross join

```sql
SELECT Bars.name AS bar,
       Drinkers.name AS drinker
FROM Bars
CROSS JOIN Drinkers
WHERE Drinkers.name = 'John';
```

## Solution 9 – Reasoning about joins
1. A CROSS JOIN produces a Cartesian product, which can result in very large result sets and poor performance.
2. A NATURAL JOIN is risky because it implicitly joins on all columns with the same name, which may change as the schema evolves.
3. Explicit join conditions improve readability, correctness, and maintainability, and avoid accidental joins.

## Solution 10 – Challenge

```sql
SELECT DISTINCT Likes.drinker AS drinker
FROM Likes
JOIN Sells ON Likes.beer = Sells.beer
JOIN Frequents ON Frequents.bar = Sells.bar
WHERE Likes.drinker = Frequents.drinker;
```
