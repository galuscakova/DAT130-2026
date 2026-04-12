# Elasticsearch Practical Exercises 

## 1. Setup 

1. Go to: [https://cloud.elastic.co](https://cloud.elastic.co)
2. Sign up for a free account
3. Go to **Developer Tools**

You can now run Elasticsearch queries directly in the browser.

---

## 2. Exercise 1 – Create an Index

Create an index called `books`:

```json
PUT books
```

---

## 3. Exercise 2 – Insert Documents

Add some books:

```json
POST books/_doc
{
  "title": "The Hobbit",
  "author": "J.R.R. Tolkien",
  "year": 1937,
  "genre": "fantasy"
}
```

```json
POST books/_doc
{
  "title": "The Lord of the Rings",
  "author": "J.R.R. Tolkien",
  "year": 1954,
  "genre": "fantasy"
}
```

```json
POST books/_doc
{
  "title": "A Brief History of Time",
  "author": "Stephen Hawking",
  "year": 1988,
  "genre": "science"
}
```

---

## 4. Exercise 3 – Retrieve All Documents

```json
GET books/_search
```

---

## 5. Exercise 4 – Simple Search

Find books with the word "hobbit":

```json
GET books/_search
{
  "query": {
    "match": {
      "title": "hobbit"
    }
  }
}
```

👉 This is **full-text search**, not exact matching.

---

## 6. Why Information Retrieval (IR) Matters

Elasticsearch is based on **Information Retrieval principles**.

### Key Idea: Inverted Index

Instead of scanning all documents, Elasticsearch builds an index like:

```
"hobbit" → [doc1]
"lord"   → [doc2]
"time"   → [doc3]
```

This makes search fast and scalable.

---

### Exact Match vs Full-Text Search

#### SQL-style thinking (exact match)

```sql
SELECT * FROM books WHERE title = 'The Hobbit';
```

👉 Only matches exact string.

---

#### Elasticsearch (IR-based)

```json
GET books/_search
{
  "query": {
    "match": {
      "title": "hobbit"
    }
  }
}
```

👉 Matches:

* "The Hobbit"
* "Hobbit Adventures" (if exists)

Because text is:

* tokenized
* normalized (lowercased)

---

## 7. Example – Tokenization

Search for:

```json
{
  "query": {
    "match": {
      "title": "lord rings"
    }
  }
}
```

👉 Internally becomes tokens:

```
["lord", "rings"]
```

Documents containing both terms score higher.

---

## 8. Example – Boolean Retrieval

```json
GET books/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "title": "lord" } },
        { "match": { "title": "rings" } }
      ]
    }
  }
}
```

👉 Equivalent to:

```
lord AND rings
```

---

## 9. Example – Ranking

Search:

```json
GET books/_search
{
  "query": {
    "match": {
      "title": "of"
    }
  }
}
```

👉 Results are ranked using:

* term frequency (TF)
* inverse document frequency (IDF)

👉 Documents with more relevant terms appear **higher**.

---

## 10. Exercise – Filter Search

Find fantasy books after 1940:

```json
GET books/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "genre": "fantasy" } }
      ],
      "filter": [
        { "range": { "year": { "gt": 1940 } } }
      ]
    }
  }
}
```

## 11. Cleanup (Optional)

Delete the index:

```json
DELETE books
```
