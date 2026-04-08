# 2FA Bypass via Direct Access

## 🧠 Concept

Two-Factor Authentication (2FA) should be enforced before granting access to protected resources.

---

## 🔴 Vulnerability

The application does not verify whether the user has completed 2FA before allowing access to protected endpoints.

---

## ⚔️ Steps to Exploit

1. Login with valid credentials
2. Stop at 2FA verification step
3. Directly access:

   ```
   /my-account
   ```
4. Gain access without completing 2FA

---

## 🎯 Key Learning

* Authentication ≠ Authorization
* 2FA must be enforced server-side for all protected endpoints

---

## 🔐 Real-World Impact

Attackers with stolen credentials can:

* Bypass 2FA
* Access sensitive accounts

---

## 🛡️ Prevention

* Enforce 2FA completion before granting session access
* Validate authentication state on every request
