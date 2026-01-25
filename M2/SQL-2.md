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

## Create tables

```sql
use toydb;

CREATE TABLE Drinkers (
    name  VARCHAR(50) PRIMARY KEY,
    addr  VARCHAR(50),
    phone VARCHAR(20)
);

CREATE TABLE Beers (
    name VARCHAR(50) PRIMARY KEY,
    manf VARCHAR(50)
);

CREATE TABLE Bars (
    name     VARCHAR(50) PRIMARY KEY,
    addr     VARCHAR(50),
    license  INTEGER,
    openDate DATE
);

CREATE TABLE Likes (
    drinker VARCHAR(50),
    beer    VARCHAR(50),
    PRIMARY KEY (drinker, beer),
    FOREIGN KEY (drinker) REFERENCES Drinkers(name),
    FOREIGN KEY (beer)    REFERENCES Beers(name)
);

CREATE TABLE Sells (
    bar   VARCHAR(50),
    beer  VARCHAR(50),
    price DECIMAL(4,2) CHECK (price > 0),
    PRIMARY KEY (bar, beer),
    FOREIGN KEY (bar)  REFERENCES Bars(name),
    FOREIGN KEY (beer) REFERENCES Beers(name)
);

CREATE TABLE Frequents (
    drinker VARCHAR(50),
    bar     VARCHAR(50),
    PRIMARY KEY (drinker, bar),
    FOREIGN KEY (drinker) REFERENCES Drinkers(name),
    FOREIGN KEY (bar)     REFERENCES Bars(name)
);
```

## Fill the tables with data

```sql
INSERT INTO Bars (name, addr, license, openDate) VALUES
('Australia Hotel', 'The Rocks', 123456, '1940-12-01'),
('Coogee Bay Hotel', 'Coogee', 966500, '1980-08-31'),
('Lord Nelson', 'The Rocks', 123888, '1920-11-11'),
('Marble Bar', 'Sydney', 122123, '2001-04-01'),
('Regent Hotel', 'Kingsford', 987654, '2000-02-29'),
('Rose Bay Hotel', 'Rose Bay', 966410, '2000-08-31'),
('Royal Hotel', 'Randwick', 938500, '1986-06-26');

INSERT INTO Drinkers (name, addr, phone) VALUES
('Adam', 'Randwick', '9385-4444'),
('Gernot', 'Newtown', '9415-3378'),
('John', 'Clovelly', '9665-1234'),
('Justin', 'Mosman', '9845-4321'),
('Marie', 'Rose Bay', '9371-2126'),
('Adrian', 'Redfern', '9371-1244');

INSERT INTO Beers (name, manf) VALUES
('80/-', 'Caledonian'),
('Bigfoot Barley Wine', 'Sierra Nevada'),
('Burragorang Bock', 'George IV'),
('Crown Lager', 'Carlton'),
('Fosters Lager', 'Carlton'),
('New', 'Toohey’s'),
('Old', 'Toohey’s'),
('Old Admiral', 'Lord Nelson'),
('Pale Ale', 'Sierra Nevada'),
('Premium Lager', 'Cascade'),
('Red', 'Toohey’s'),
('Sheaf Stout', 'Toohey’s'),
('Sparkling Ale', 'Cooper’s'),
('Stout', 'Cooper’s'),
('Three Sheets', 'Lord Nelson'),
('Victoria Bitter', 'Carlton');

INSERT INTO Likes (drinker, beer) VALUES
('Adam', 'Crown Lager'),
('Adam', 'Fosters Lager'),
('Adam', 'New'),
('Gernot', 'Premium Lager'),
('Gernot', 'Sparkling Ale'),
('John', '80/-'),
('John', 'Bigfoot Barley Wine'),
('John', 'Pale Ale'),
('John', 'Three Sheets'),
('Justin', 'Sparkling Ale'),
('Justin', 'Victoria Bitter');

INSERT INTO Sells (bar, beer, price) VALUES
('Australia Hotel', 'Burragorang Bock', 3.50),
('Australia Hotel', 'Old Admiral', 3.75),
('Australia Hotel', 'Three Sheets', 3.75),
('Coogee Bay Hotel', 'New', 2.30),
('Coogee Bay Hotel', 'Old', 2.50),
('Coogee Bay Hotel', 'Sparkling Ale', 2.80),
('Coogee Bay Hotel', 'Victoria Bitter', 2.30),
('Marble Bar', 'New', 2.80),
('Marble Bar', 'Old', 2.80),
('Marble Bar', 'Victoria Bitter', 2.80),
('Regent Hotel', 'Victoria Bitter', 2.20),
('Royal Hotel', 'Victoria Bitter', 2.30);

INSERT INTO Frequents (drinker, bar) VALUES
('Adam', 'Coogee Bay Hotel'),
('Gernot', 'Lord Nelson'),
('John', 'Coogee Bay Hotel'),
('John', 'Lord Nelson'),
('John', 'Australia Hotel'),
('Justin', 'Regent Hotel'),
('Justin', 'Marble Bar'),
('Marie', 'Rose Bay Hotel');
```


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

