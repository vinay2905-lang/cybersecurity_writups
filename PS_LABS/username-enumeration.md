# Username Enumeration via Different Responses

## 🧠 Concept

Some applications reveal different error messages for:

* Invalid username
* Valid username but incorrect password

This leads to **username enumeration**.

---

## 🔴 Vulnerability

The application returns different responses:

* "Invalid username"
* "Incorrect password"

This allows attackers to identify valid usernames.

---

## ⚔️ Steps to Exploit

1. Intercept login request using Burp Suite

2. Send request to Repeater

3. Test with random username:

   ```
   username=random123&password=test
   ```

4. Observe response → "Invalid username"

5. Test with candidate usernames:

   ```
   username=albuquerque&password=test
   ```

6. Observe response → "Incorrect password"

7. Confirm valid username

---

## 🚀 Password Attack

1. Send request to Intruder
2. Select password field as payload
3. Use wordlist
4. Identify correct password via:

   * Status code change (302)
   * Response length difference

---

## 🎯 Key Learning

* Small differences in responses can leak critical information
* Always analyze:

  * Response message
  * Response length
  * Status code

---

## 🔐 Real-World Impact

Attackers can:

1. Identify valid usernames
2. Perform targeted brute-force attacks

---

## 🛡️ Prevention

* Use generic error messages:

  ```
  Invalid username or password
  ```
* Avoid revealing authentication state
