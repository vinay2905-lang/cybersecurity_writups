# 🔐 PortSwigger Lab - Username Enumeration via Different Responses

## 🧠 Day X Learning

Today I learned how authentication systems can leak information through:

* Different error messages
* Response differences
* Login behavior

---

## 🔍 Step 1: Understanding the Lab

The lab contains a login page where:

* Invalid username → different response
* Valid username → different response

👉 This allows **username enumeration**

---

## ⚔️ Step 2: Capture Request (Burp Suite)

Used:

* Burp Proxy
* Intercepted login request

Example request:

```http id="q3z8b5"
POST /login HTTP/1.1

username=test&password=test123
```

Sent request to **Repeater**

---

## 🧠 Step 3: Baseline Response

Tested with random username:

```text id="u8e0lx"
username=random123
```

👉 Response:

* “Invalid username”
* Response length noted

---

## 🔎 Step 4: Username Enumeration

Tested multiple usernames:

```text id="9y1e1b"
administrator
admin
carlos
albuquerque
```

---

## 🎯 Observation

Found difference:

| Username    | Response           |
| ----------- | ------------------ |
| random123   | Invalid username   |
| albuquerque | Incorrect password |

---

## 🚀 Result

👉 Identified valid username:

```text id="p7cxxw"
albuquerque
```

---

## ⚔️ Step 5: Password Brute Force (Burp Intruder)

Sent request to **Intruder**

### Configuration:

* Attack type: Sniper
* Payload: Password list

---

## 🔍 Step 6: Analyze Results

Checked:

* Status codes
* Response length

---

## 🎯 Success Indicator

Found response with:

```text id="o3h5aq"
HTTP 302 Found
```

👉 Indicates successful login

---

## 🚀 Step 7: Login Success

Redirected to:

```text id="6m4fxk"
/my-account?id=albuquerque
```

---

## 🏁 Result

Successfully:

* Enumerated username
* Brute-forced password
* Logged into account

---

## 🧠 Key Learnings

* Small response differences = big vulnerability
* Always compare:

  * Response text
  * Length
  * Status code
* Enumeration before brute force is efficient

---

## 🔐 Vulnerabilities

* Information leakage via error messages
* Weak authentication logic

---

## 🛡️ Prevention

* Use generic error messages
* Implement rate limiting
* Add account lockout
* Use strong authentication mechanisms

---

## 🔥 Real Insight

> Attackers don’t guess blindly
> They first find valid users, then target them

---
