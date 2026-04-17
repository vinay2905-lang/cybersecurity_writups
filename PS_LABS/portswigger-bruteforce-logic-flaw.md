# 🔐 PortSwigger Lab - Password Brute-Force Protection Logic Flaw

## 🧠 Day X Learning

Today I learned how to bypass brute-force protection by exploiting:

* Broken rate-limiting logic
* Counter reset flaws
* Request sequencing attacks
* Burp Intruder Pitchfork mode

---

## 🔍 Step 1: Understanding the Application Logic

The application implements a **soft lockout**:

* After **3 failed login attempts**
* Same IP must wait **1 minute**

👉 Intended to stop brute-force attacks.

---

## ⚠️ Vulnerability

The failed-attempt counter is reset when a **successful login** occurs from the same IP.

That means:

```text id="6wulxj"
2 failed attempts + 1 successful login = counter reset
```

👉 This allows unlimited guessing without triggering lockout.

---

## 🧠 Exploitation Strategy

Use this repeating pattern:

```text id="3ofx6n"
1. carlos : wrongpassword
2. carlos : wrongpassword
3. wiener : peter   (valid login)
```

Then repeat.

---

## 🔥 Why This Works

The successful login resets the backend counter before the third failed attempt can trigger lockout.

---

## ⚔️ Step 2: Prepare Payload Lists

Used Python to generate two synchronized payload files.

### Username List Pattern

```text id="mjlwmg"
carlos
carlos
wiener
carlos
carlos
wiener
...
```

### Password List Pattern

```text id="k06g5v"
candidate1
candidate2
peter
candidate3
candidate4
peter
...
```

👉 `peter` is the valid password for `wiener`

---

## ⚔️ Step 3: Burp Intruder Configuration

### Attack Type:

```text id="1sod3w"
Pitchfork
```

This matches:

* Username payload line 1 ↔ Password payload line 1
* Username payload line 2 ↔ Password payload line 2
* etc.

---

## ⚙️ Step 4: Resource Pool (Critical)

Created new resource pool:

```text id="7wl4sv"
Maximum concurrent requests = 1
```

👉 Requests must be sent one-by-one in exact order.

If parallel requests are used:

* Sequence breaks
* IP gets locked out

---

## ⚔️ Step 5: Start Attack

Launched Intruder with:

* Payload 1 → usernames list
* Payload 2 → passwords list

---

## 🔍 Step 6: Analyze Results

Monitored responses for:

```text id="3eqhqs"
HTTP 302 Found
```

on requests where username = `carlos`

---

## 🎯 Result

Valid credentials found:

```text id="ww1ovj"
Username: carlos
Password: matthew
```

---

## 🚀 Step 7: Login

Logged in using:

```text id="q4l16s"
carlos : matthew
```

Accessed user account page and solved the lab.

---

## 🏁 Final Result

Successfully:

* Bypassed brute-force lockout
* Exploited reset logic flaw
* Discovered victim password
* Logged into Carlos account

---

## 🧠 Key Learnings

* Security controls can fail due to logic mistakes
* Request order matters in some attacks
* Successful actions can unintentionally reset protections
* Intruder Pitchfork is powerful for synchronized payloads

---

## 🔐 Vulnerability Type

* Authentication logic flaw
* Broken brute-force protection
* Weak rate-limiting design

---

## 🛡️ Prevention

* Separate counters per username and IP
* Do not reset counters on unrelated successful logins
* Use account lockout with adaptive controls
* Add MFA / CAPTCHA

---

## 🔥 Real Insight

> Many protections look strong on paper,
> but fail because of poor backend logic.

---
