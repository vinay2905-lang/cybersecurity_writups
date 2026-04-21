# 💉 PortSwigger Lab - SQL Injection in WHERE Clause

## 🧠 Day 16 Learning

Today I learned how SQL Injection in a **WHERE clause** can be used to bypass filters and retrieve hidden data.

Key concepts:

* SQL query manipulation
* Boolean-based injection
* Comment operator usage
* Input trust issues

---

## 🔍 Step 1: Understanding the Application

The website had a filter parameter similar to:

```text id="vtx2o0"
?category=Gifts
```

Selecting categories changed displayed products.

This suggested the backend query might be:

```sql id="mbrb3l"
SELECT * FROM products WHERE category='Gifts' AND released=1;
```

---

## ⚠️ Vulnerability

User-controlled input was inserted directly into SQL query without proper sanitization.

That means the `category` parameter could be manipulated.

---

## ⚔️ Step 2: Test for SQL Injection

Used a single quote:

```text id="zv3dgs"
'
```

This often helps detect SQL parsing issues.

---

## ⚔️ Step 3: Exploit Payload

Injected:

```text id="r2v9k0"
%27+or+1=1--
```

Decoded version:

```sql id="1u2r9k"
' OR 1=1--
```

---

## 🧠 How It Works

Original query:

```sql id="j2ke5f"
SELECT * FROM products WHERE category='Pets'
```

Injected query becomes:

```sql id="8yxj0w"
SELECT * FROM products WHERE category='' OR 1=1--'
```

---

## 🔥 Why This Works

* `'` closes the original string
* `OR 1=1` is always TRUE
* `--` comments out the rest of query

Result:

👉 Database returns all rows instead of filtered rows.

---

## 🚀 Step 4: Result

The page displayed:

* Hidden products
* Unreleased items
* All categories / expanded content

Lab solved successfully.

---

## 🏁 Final Result

Successfully exploited:

* SQL Injection in WHERE clause
* Authentication/filter bypass logic
* Data disclosure via boolean condition

---

## 🧠 Key Learnings

* Never trust URL parameters
* WHERE clause injection can expose all records
* `OR 1=1` is a classic but powerful test
* Comments (`--`) are used to ignore trailing SQL syntax

---

## 🔐 Vulnerability Type

* SQL Injection
* Improper input sanitization
* Sensitive data exposure

---

## 🛡️ Prevention

* Use parameterized queries / prepared statements
* Input validation
* Least-privilege DB permissions
* Error handling without SQL leaks

---

## 🔥 Real Insight

> Even a simple product filter can become a data leak if user input is directly inserted into SQL.

---
