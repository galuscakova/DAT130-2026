## Part A – Set Operations

### Solution A1 – UNION vs UNION ALL

```sql
SELECT Drinker
FROM Likes
WHERE Beer = 'Sparkling Ale'
UNION
SELECT Drinker
FROM Frequents
WHERE Bar = 'Lord Nelson';
```

```sql
SELECT Drinker
FROM Likes
WHERE Beer = 'Sparkling Ale'
UNION ALL
SELECT Drinker
FROM Frequents
WHERE Bar = 'Lord Nelson';
```

---

### Solution A2 – INTERSECT

```sql
SELECT Drinker, Beer
FROM Likes
INTERSECT
SELECT Drinker, Beer
FROM Sells NATURAL JOIN Frequents;
```

---

### Solution A3 – EXCEPT

```sql
SELECT Beer
FROM Sells
WHERE Price < 3
EXCEPT
SELECT Beer
FROM Likes
WHERE Drinker = 'Justin';
```

---

## Part B – Embedded Queries

### Solution B1 – IN

```sql
SELECT Name, Manf
FROM Beers
WHERE Name IN (
    SELECT Beer
    FROM Likes
    WHERE Drinker = 'John'
);
```

---

### Solution B2 – Scalar vs relation result

```sql
SELECT Bar
FROM Sells
WHERE Beer = 'New'
  AND Price IN (
      SELECT Price
      FROM Sells
      WHERE Bar = 'Coogee Bay Hotel'
        AND Beer = 'Victoria Bitter'
  );
```

---

### Solution B3 – Correlated subquery

```sql
SELECT Name
FROM Drinkers d
WHERE EXISTS (
    SELECT *
    FROM Frequents f
    WHERE f.Drinker = d.Name
);
```

---

### Solution B4 – Minimum per group

```sql
SELECT Bar, Beer, Price
FROM Sells s1
WHERE Price IN (
    SELECT MIN(Price)
    FROM Sells s2
    WHERE s1.Bar = s2.Bar
);
```

---

## Part C – Aggregation

### Solution C1 – Average

```sql
SELECT AVG(Price)
FROM Sells
WHERE Bar = 'Australia Hotel';
```

---

### Solution C2 – DISTINCT and COUNT

```sql
SELECT COUNT(DISTINCT Bar)
FROM Sells
WHERE Price < 2.5;
```

---

### Solution C3 – Lowest price

```sql
SELECT Bar
FROM Sells
WHERE Beer = 'New'
  AND Price = (
      SELECT MIN(Price)
      FROM Sells
      WHERE Beer = 'New'
  );
```

---

## Part D – Partition (GROUP BY)

### Solution D1 – Counting per drinker

```sql
SELECT Drinker, COUNT(Beer) AS NbBeers
FROM Likes
GROUP BY Drinker;
```

---

### Solution D2 – GROUP BY with JOIN

```sql
SELECT d.Name, d.Addr, COUNT(l.Beer) AS NbBeers
FROM Drinkers d JOIN Likes l
ON d.Name = l.Drinker
GROUP BY d.Name, d.Addr;
```

---

### Solution D3 – HAVING

```sql
SELECT Bar,
       COUNT(DISTINCT s.Beer) AS NbBeers,
       COUNT(DISTINCT f.Drinker) AS NbDrinkers
FROM Sells s NATURAL JOIN Frequents f
GROUP BY Bar
HAVING COUNT(DISTINCT s.Beer) > 2;
```

---

## Part E – Empty Values (NULL)

### Solution E1 – Comparison with NULL

```sql
SELECT *
FROM R
WHERE Mark <> 90
   OR Mark IS NULL;
```

---

### Solution E2 – COUNT and NULL

* `COUNT(*)` counts all tuples, including those with `NULL` values.
* `COUNT(Mark)` counts only tuples where `Mark` is not `NULL`.
