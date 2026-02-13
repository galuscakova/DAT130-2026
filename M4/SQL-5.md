In this lab you will explore how indexes affect query performance in MySQL.

Run the following scripts first.

Create the tables:

```sql
USE indexToyDB;

-- Users table (Instagram style)
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50),
    email VARCHAR(100),
    country VARCHAR(50),
    created_at DATE
);

-- Posts table
CREATE TABLE posts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    title VARCHAR(200),
    body TEXT,
    likes INT,
    created_at DATE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- Orders table (shop example)
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    amount DECIMAL(8,2),
    status VARCHAR(20),
    order_date DATE,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

Insert test data:

```sql
SET SESSION cte_max_recursion_depth = 200000;

INSERT INTO users (username,email,country,created_at)
WITH RECURSIVE seq AS (
  SELECT 1 AS n
  UNION ALL
  SELECT n+1 FROM seq WHERE n < 20000
)
SELECT
  CONCAT('user', n),
  CONCAT('user', n, '@mail.com'),
  ELT(1 + FLOOR(RAND()*5),'Norway','Sweden','Denmark','Germany','France'),
  DATE('2020-01-01') + INTERVAL FLOOR(RAND()*365) DAY
FROM seq;

INSERT INTO posts (user_id,title,body,likes,created_at)
WITH RECURSIVE seq AS (
  SELECT 1 AS n
  UNION ALL
  SELECT n+1 FROM seq WHERE n < 10
)
SELECT
  FLOOR(1 + RAND()*20000),
  CONCAT('Post ', n),
  'Indexing improves performance',
  FLOOR(RAND()*500),
  DATE('2021-01-01') + INTERVAL FLOOR(RAND()*365) DAY
FROM seq;

INSERT INTO orders (user_id,amount,status,order_date)
WITH RECURSIVE seq AS (
  SELECT 1 AS n
  UNION ALL
  SELECT n+1 FROM seq WHERE n < 10
)
SELECT
  FLOOR(1 + RAND()*20000),
  ROUND(RAND()*1000,2),
  ELT(1 + FLOOR(RAND()*3),'paid','pending','cancelled'),
  DATE('2022-01-01') + INTERVAL FLOOR(RAND()*365) DAY
FROM seq;
```

## Exercise A -- No indexes

### A.1

Find a user by username:

```sql
SELECT * FROM users WHERE username = 'user15000';
```

Run EXPLAIN

How many rows are scanned?

### A.2

Find all users from Norway.

Run with EXPLAIN

Is an index used?

## Exercise B -- Single column indexes

### B.1

Create an index on username.

Run EXPLAIN again for A.1

What changed?

### B.2

Create an index on country.

Run A.2 again

Compare rows scanned before/after

## Exercise C -- Composite indexes

We often search:

```sql
SELECT * FROM orders 
WHERE user_id = 100 AND status = 'paid';
```

### C.1
Run EXPLAIN without index

### C.2
Create a composite index

Run EXPLAIN again

## Exercise D -- Range queries

Find expensive orders:

```sql
SELECT * FROM orders
WHERE amount BETWEEN 4000 AND 4500;
```

Create an index for this and test

## Exercise E -- FULLTEXT search

Search posts containing the word “performance”.

### E.1
Search for it using LIKE

### E.2

Create FULLTEXT index

Use MATCH ... AGAINST for searching

Compare speed

## Exercise F -- Index inspection

Use:

```sql
SHOW INDEX FROM users;
```

Explain:

- Cardinality
- Index_type
- Non_unique

## Exercise G -- Query optimizer hints

Force MySQL to use an index even if it doesn’t want to.

Use:

- USE INDEX
- FORCE INDEX

And run it on the orders table.

# Exercise H -- Partitioning

Create a partitioned copy of orders by year of order_date using RANGE partitioning.

Then run

```sql
EXPLAIN SELECT * FROM orders_partitioned
WHERE order_date >= '2024-01-01';
```

Does MySQL scan all partitions or only one?
