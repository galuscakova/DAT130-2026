# DAT130 – NoSQL Practical Exercises (MongoDB & Neo4j)

---
# 🟩 Part A – MongoDB

## MongoDB Playground (no login, no install)

👉 Open this in your browser:  
https://mongoplayground.net/

---

## 1. Insert data

### 1.1 
Copy everything below into Mongo database:

```js 
[
  { name: "Alice", age: 22, course: "DAT130" },
  { name: "Bob", age: 24, course: "DAT130", hobbies: ["football", "gaming"] },
  { name: "Charlie", age: 23, course: "DAT200" }
]
```

Click Run ▶

## 2. Basic queries

### 2.1 Find all students
```js
db.collection.find()
```

### 2.2 Find DAT130 students
```js
db.collection.find({ course: "DAT130" });
```

### 2.3 Find students older than 22
```js
db.collection.find({ age: { $gt: 22 } });
```


# 🟦 Part B – Neo4j (Graph Database)

## Neo4j (graph database)

👉 https://neo4j.com/cloud/platform/aura-graph-database/

### 3.1. Create nodes
```js
CREATE (a:Student {name: "Alice", age: 22})
CREATE (b:Student {name: "Bob", age: 24})
CREATE (c:Course {code: "DAT130"})
```

### 3.2. Create relationships
```js
MATCH (a:Student {name: "Alice"}), (c:Course {code: "DAT130"})
CREATE (a)-[:ENROLLED_IN]->(c)

MATCH (b:Student {name: "Bob"}), (c:Course {code: "DAT130"})
CREATE (b)-[:ENROLLED_IN]->(c)
```
### 3.3. Query

Show graph
```js
MATCH (n) RETURN n
```

Find students in DAT130
```js
MATCH (s:Student)-[:ENROLLED_IN]->(c:Course {code: "DAT130"})
RETURN s
```

### 3.4. Add social connection

```js
MATCH (a:Student {name: "Alice"}), (b:Student {name: "Bob"})
CREATE (a)-[:FRIEND]->(b)
```

### 3.5. Query relationships

```js
MATCH (a:Student)-[:FRIEND]->(b)
RETURN a, b
```
