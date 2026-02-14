## Part A

### A.1

```sql
-- check balances before
SELECT * FROM accounts;

START TRANSACTION;

UPDATE accounts
SET balance = balance - 200
WHERE id = 1;

UPDATE accounts
SET balance = balance + 200
WHERE id = 2;

COMMIT;

SELECT * FROM accounts
```

### A.2

```sql
-- check balances before
SELECT * FROM accounts;

START TRANSACTION;

UPDATE accounts
SET balance = balance - 200
WHERE id = 1;

UPDATE accounts
SET balance = balance + 200
WHERE id = 2;

ROLLBACK;

SELECT * FROM accounts;
```

### A.3

Without transactions:
• partial updates stay
• database becomes inconsistent

Transactions guarantee atomicity.

## Part B

### B.1

```sql
START TRANSACTION;

UPDATE accounts SET balance = balance - 300 WHERE id = 1; -- sword
UPDATE accounts SET balance = balance - 200 WHERE id = 1; -- shield

SAVEPOINT after_shield;

UPDATE accounts SET balance = balance - 50 WHERE id = 1; -- potion

ROLLBACK TO after_shield;

COMMIT;
```

Balance:
1000 − 300 − 200 = 500

Potion purchase undone.

## Part C

### C.1

Session B sees balance = 0.

After rollback → value never really existed.

This is a dirty read.

### C.2

Session B sees original balance only.

Dirty reads prevented.

### C.3

