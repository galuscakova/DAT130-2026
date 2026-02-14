When the instructions say Session A / Session B
Open:
File → New Query Tab

Each tab = one independent database session.

Also, first turn off autommit in MySQL (uncheck Query->Auto-Commit Transactions)

## Setup 


```sql
USE banklab;

CREATE TABLE accounts (
    id INT PRIMARY KEY,
    owner VARCHAR(50),
    balance INT
) ENGINE=InnoDB;

INSERT INTO accounts VALUES
(1,'Alice',1000),
(2,'Bob',500),
(3,'Charlie',700);
```

## Part A – Basic transactions

### Task A.1 – Transfer money safely

Transfer 200 from Alice to Bob using ONE transaction.

Steps:\
START TRANSACTION\
subtract from Alice\
add to Bob\
COMMIT

Check balances before and after.

### Task A.2 – Undo changes

Repeat Task 1.1 but use ROLLBACK instead of COMMIT.

What happens to the balances?

### Task A.3 – Crash simulation

Run:

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
ROLLBACK;
```

Why is this safer than running UPDATE without a transaction?

## Part B – Savepoints (Game shop)

Add inventory:

```sql
CREATE TABLE inventory (
    item VARCHAR(50) PRIMARY KEY,
    price INT,
    stock INT
);

INSERT INTO inventory VALUES
('Sword',300,5),
('Shield',200,5),
('Potion',50,10);
```

Alice has 1000 coins.

### Task B.1 – Partial rollback

Inside ONE transaction:
- buy Sword
- buy Shield
- SAVEPOINT
- buy Potion
- rollback only the potion
- commit

What is Alice’s final balance?

## Part C – Isolation levels

IMPORTANT:
Open TWO query tabs in Workbench.

Tab 1 = Session A\
Tab 2 = Session B

### Task C.1 – Dirty read

**Session A**

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
START TRANSACTION;
UPDATE accounts SET balance = 0 WHERE id = 1;
```

(DO NOT commit yet)

**Session B**

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT balance FROM accounts WHERE id = 1;
```

**Session A**

```sql
ROLLBACK;
```

Questions:
• What value did Session B see?
• Did it actually exist?

### Task C.2 – Read committed

Repeat Task C.1 using:

```sql
READ COMMITTED
```

Can Session B still see uncommitted data?

### Task C.3 – Non-repeatable read

**Session A**

```sql
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
SELECT balance FROM accounts WHERE id = 2;
```

**Session B**

```sql
UPDATE accounts SET balance = 999 WHERE id = 2;
COMMIT;
```

**Session A**

```sql
SELECT balance FROM accounts WHERE id = 2;
```

Did the value change inside the same transaction?

## Part D – Locking

### Task D.1 – Prevent lost updates

**Session A**

```sql
START TRANSACTION;
SELECT balance FROM accounts WHERE id = 3 FOR UPDATE;
```

**Session B**

```sql
START TRANSACTION;
UPDATE accounts SET balance = balance + 100 WHERE id = 3;
```

What happens? Does Session B wait?

Now finish Session A:

```sql
COMMIT;
```

What happens to Session B?

## Part E – Thinking questions

Answer briefly:
- What does Atomicity guarantee?
- When should you use SAVEPOINT?
- Which isolation level is MySQL’s default?
- Why is SERIALIZABLE slower?
- Give two real systems that require transactions.
