# 🔐 PortSwigger Lab - Password Reset Functionality Vulnerability

## 🧠 Day X Learning

Today I learned how password reset mechanisms can be vulnerable due to:

* Improper validation
* Trusting user-controlled input
* Missing authorization checks

---

## 🔍 Step 1: Understanding the Lab

The lab provides:

* A **Forgot Password** functionality
* Goal: Reset Carlos’s password and login

---

## ⚔️ Step 2: Intercept Request (Burp Suite)

Used:

* Burp Proxy
* Intercepted password reset request

Example:

```http id="a1kz9v"
POST /forgot-password HTTP/1.1

username=wiener
```

---

## 🧠 Step 3: Analyze Request

Observed:

* Username is sent directly in request
* No strong validation mechanism

👉 Key Question:

> Can we change the username?

---

## ⚔️ Step 4: Exploit the Flaw

Modified request:

```http id="t5r7nc"
POST /forgot-password

username=carlos
```

👉 This triggers reset process for Carlos

---

## 🔥 Step 5: Reset Password

During password reset step:

Modified request:

```http id="q2w9xf"
POST /reset-password

username=carlos
new-password=12345
```

👉 Password changed without proper authorization

---

## 🚀 Step 6: Login as Carlos

Used credentials:

```text id="p8k1lo"
Username: carlos
Password: 12345
```

---

## 🏁 Result

Successfully:

* Reset victim’s password
* Logged into Carlos’s account
* Completed the lab

---

## 🧠 Key Learnings

* Never trust user input in sensitive operations
* Password reset must be tied to secure tokens
* Authorization must be enforced server-side

---

## 🔐 Vulnerability

* Broken password reset logic
* Missing user verification
* Insecure direct object reference (IDOR-like behavior)

---

## 🛡️ Prevention

* Use secure, unique reset tokens
* Validate token on server-side
* Do not allow direct username manipulation
* Implement expiration for reset links

---

## 🔥 Real Insight

> If attackers can control identity in reset flow
> they can take over any account

---
