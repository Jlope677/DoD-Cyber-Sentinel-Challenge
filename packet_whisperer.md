# 🔐 Packet Whisperer – PCAP Credential Leak Write-Up

**Category:** Forensics / Network Analysis\
**Points:** 75\
**Solves:** 648

---

## 🕵️ Challenge Summary

Our Blue Team intercepted a PCAP file (`login.pcap`) containing unencrypted HTTP traffic. It was suspected that a user exposed their login credentials during a session. The task was to review the traffic and identify the password transmitted during login.

📎 Provided file: `login.pcap`

---

## 🧰 Tools Used

| Tool        | Purpose                          |
| ----------- | -------------------------------- |
| Wireshark   | Packet inspection and filtering  |
| URL decoder | Decoding URL-encoded credentials |

---

## 🪜 Step-by-Step Analysis

### 🔹 Step 1: Open the PCAP

Launch Wireshark and open the file:

```bash
wireshark login.pcap
```

---

### 🔹 Step 2: Filter HTTP POST Requests

To isolate the traffic where credentials might be passed:

```wireshark
http.request.method == "POST"
```

This displays HTTP POST transmissions, where form submissions usually occur.

---

### 🔹 Step 3: Inspect the Packet

Select a POST request in the list. In the **Packet Details Pane**, expand the **Hypertext Transfer Protocol** section.

Look for:

```
Line-based text data: application/x-www-form-urlencoded
```

You’ll find a line like this:

```ini
username=ironpotatoadmin&password=C1%7Bmaybe_TLS_would_be_nice%7D
```

---

### 🔹 Step 4: Decode the Password

The password is URL-encoded. Decode:

- `%7B` → `{`
- `%7D` → `}`

✅ **Decoded Flag:**

```
C1{maybe_TLS_would_be_nice}
```

---

## 🧠 Key Takeaways

- Never transmit credentials over unencrypted HTTP.
- Use HTTPS to protect login data from packet capture and inspection.
- Network captures like `.pcap` files can reveal plaintext credentials if TLS is not in place.
- Always inspect POST data under `application/x-www-form-urlencoded` when analyzing login attempts.

---

## 🏁 Final Flag

```
C1{maybe_TLS_would_be_nice}
```

---

