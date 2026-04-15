# 🔐 PortSwigger Lab - Broken Brute-Force Protection & Username Enumeration

## 🧠 Day 8 Learning

Today I learned how to bypass brute-force protection using:

* Header manipulation (X-Forwarded-For)
* Timing-based username enumeration
* Advanced Burp Intruder (Pitchfork attack)

---

## 🔍 Step 1: Understanding the Vulnerability

The application:

* Blocks IP after multiple failed login attempts ❌
* Trusts the `X-Forwarded-For` header

👉 This allows:

* IP spoofing
* Bypassing brute-force protection

---

## ⚔️ Step 2: Capture Login Request

Used Burp Suite to intercept login request:

```http
POST /login HTTP/1.1

username=test&password=test123
```

Sent request to **Intruder**

---

## ⚔️ Step 3: Bypass IP Blocking

Added header:

```http
X-Forwarded-For: 1
```

👉 This simulates different IP addresses for each request

---

## ⚔️ Step 4: Username Enumeration (Timing Attack)

### Intruder Configuration:

* Attack type: **Pitchfork**
* Payload positions:

  * X-Forwarded-For → §1§
  * Username → §2§
* Password set to very long string (100+ characters)

---

### Payload Setup:

#### Position 1 (IP spoofing):

* Type: Numbers
* Range: 1–100

#### Position 2 (Usernames):

* Username wordlist

---

## 🔍 Step 5: Analyze Results

Enabled columns:

* Response received
* Response completed

---

## 🎯 Observation

* Invalid usernames → consistent response time
* Valid username → **significantly higher response time**

👉 Identified valid username

---

## ⚔️ Step 6: Password Brute Force

Configured second Intruder attack:

```http
X-Forwarded-For: §1§
username=VALID_USERNAME&password=§2§
```

---

### Payload Setup:

* Position 1 → Numbers (1–100)
* Position 2 → Password list

---

## 🔍 Step 7: Identify Correct Password

Looked for:

```text
HTTP Status: 302 Found
```

👉 Indicates successful login

---

## 🚀 Step 8: Login

Used discovered credentials:

```text
Username: (valid username)
Password: (valid password)
```

Accessed:

```text
/my-account
```

---

## 🏁 Result

Successfully:

* Bypassed brute-force protection
* Enumerated valid username
* Brute-forced password
* Logged into user account

---

## 🧠 Key Learnings

* Never trust client-controlled headers
* Timing differences can leak sensitive data
* Brute-force protections are often flawed
* Intruder Pitchfork is powerful for multi-parameter attacks

---

## 🔐 Vulnerabilities

* Broken brute-force protection
* IP-based rate limiting bypass
* Timing side-channel attack

---

## 🛡️ Prevention

* Do not trust `X-Forwarded-For` blindly
* Implement server-side rate limiting
* Use CAPTCHA / MFA
* Normalize response times

---

## 🔥 Real Insight

> Even when error messages are hidden,
> attackers can still extract information using timing differences

---
