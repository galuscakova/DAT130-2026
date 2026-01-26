We will use the tables created in [SQL-2](https://github.com/galuscakova/DAT130-2026/blob/cb7cddafd624c0a3cfc1d4fe35d6f234e43b2883/M2/SQL-2.md).

The schema is as follows:
```sql
Drinkers(Name, Addr)
Bars(Name, Addr)
Beers(Name, Manf)
Likes(Drinker, Beer)
Frequents(Drinker, Bar)
Sells(Bar, Beer, Price)
```


## Part A – Set Operations

### Exercise A1 – UNION vs UNION ALL

Give the names of drinkers who either:
* like the beer Sparkling Ale, or
* frequent the bar Lord Nelson.

1. Write a query using UNION.
2. Compare it with query UNION ALL.

### Exercise A2 – INTERSECT

Give all (Drinker, Beer) pairs such that the drinker likes a beer and frequents at least one bar that sells that beer.

### Exercise A3 – EXCEPT

Give the beers that are sold for less than $3 and are not liked by Justin.

## Part B – Embedded Queries

### Exercise B1 – IN (Non‑correlated subquery)

Give the name and manufacturer of beers that are liked by John.

### Exercise B2 – IN (Non‑correlated subquery)

Give the bars that sell New at the same price as Victoria Bitter is sold at Coogee Bay Hotel.

### Exercise B3 – Correlated subquery

Give the names of drinkers who frequent at least two bars.

### Exercise B4 – Minimum per group

For each bar, give the cheapest beer it sells and its price.

## Part C – Aggregation

### Exercise C1 – Simple aggregation

What is the average price of beers sold at Australia Hotel?

### Exercise C2 – DISTINCT and COUNT

How many different bars sell at least one beer for less than $2.50?

### Exercise C3 – Aggregation with join

Which bar(s) sell the beer New for the lowest price?

## Part D – Partition (GROUP BY)

### Exercise D1 – Counting per drinker

For each drinker, give the number of beers they like.

Expected output columns: Drinker, NbBeers

### Exercise D2 – GROUP BY with JOIN

For each drinker, give their name, address, and the number of beers they like.

### Exercise D3 – HAVING

For each bar selling more than two different beers, give:

* the bar name
* the number of different beers it sells
* the number of different drinkers who frequent it

## Part E – Empty Values (NULL)

Assume the relation:
```sql
R(StuId, Mark, Course)
```

Assume that some tuples may have Mark = NULL.

### Exercise E1 – Comparison with NULL

Give all tuples corresponding to students who got a mark different from 90, including students who did not receive a mark.

### Exercise E2 – COUNT and NULL

Explain the difference between the following two queries:

```sql
SELECT COUNT(*) FROM R;
```

and

```sql
SELECT COUNT(Mark) FROM R;
```
