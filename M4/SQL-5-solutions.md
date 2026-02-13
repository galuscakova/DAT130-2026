
## Exercise A -- No indexes

### A.1

```sql
Explain SELECT * FROM users WHERE username = 'user1500';
```

Full table scan.

### A.2

```sql
Explain SELECT * FROM users WHERE country = 'Norway';
```

Full table scan.

## Exercise B -- Single column indexes

### B.1

```sql
CREATE INDEX idx_username ON users(username);
Explain SELECT * FROM users WHERE username = 'user1500';
```

Got from thousands of searches to just a few searches.

### B.2

```sql
CREATE INDEX idx_country ON users(country);
Explain SELECT * FROM users WHERE country = 'Norway';
```

Due to the cardinality, this index is not so helpful.

## Exercise C -- Composite indexes

```sql
SELECT * FROM orders 
WHERE user_id = 100 AND status = 'paid';
```

### C.2

```sql
CREATE INDEX idx_user_status ON orders(user_id, status);
EXPLAIN SELECT * FROM orders 
WHERE user_id = 100 AND status = 'paid';
```

## Exercise D -- Range queries

```sql
CREATE INDEX idx_amount ON orders(amount);
EXPLAIN SELECT * FROM orders
WHERE amount BETWEEN 4000 AND 4500;
```

## Exercise E -- FULLTEXT search

### E.1

```sql
EXPLAIN SELECT * from posts WHERE body LIKE '%performance%';
```

### E.2

```sql
CREATE FULLTEXT INDEX idx_post_text ON posts(title, body);
EXPLAIN SELECT *
FROM posts
WHERE MATCH(title,body) AGAINST('performance');
```

## Exercise F -- Index inspection

```sql
SHOW INDEX FROM orders;
```

## Exercise G -- Query optimizer hints

```sql
EXPLAIN SELECT * FROM orders USE INDEX(idx_amount)
WHERE amount > 4000;

SELECT * FROM orders FORCE INDEX(idx_amount)
WHERE amount > 4000;
```

## Exercise H -- Partitioning

```sql
CREATE TABLE orders_partitioned (
    id INT,
    user_id INT,
    amount DECIMAL(8,2),
    status VARCHAR(20),
    order_date DATE
)
PARTITION BY RANGE (YEAR(order_date)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION pmax VALUES LESS THAN MAXVALUE
);
```

Querying 2024:
Only one partition scanned → faster.
