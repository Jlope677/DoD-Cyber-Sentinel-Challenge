# 🔐 Packet Whisperer – PCAP Analysis

**Category:** Forensics / Network Analysis\
**Points:** 75


---

## 🕵️ Challenge Summary

Our Blue Team intercepted a packet capture (`login.pcap`) that included unencrypted HTTP traffic. The objective was to inspect the capture and determine whether login credentials had been exposed in plain text.

📎 Provided file: `login.pcap`

---

## 🧰 Tools Used

| Tool      | Purpose                         |
| --------- | ------------------------------- |
| Wireshark | Analyze and inspect .pcap files |

---

## 🪜 Step-by-Step Analysis

### 1️⃣ Open the PCAP File

Open Wireshark and load the packet capture file:

```bash
wireshark login.pcap
```

---

### 2️⃣ Apply an HTTP Filter

To narrow down the view to relevant login activity, apply the following display filter:

```wireshark
http.request.method == "POST"
```

This shows HTTP POST requests—commonly used to submit forms.

---

### 3️⃣ Locate the POST Data

Select the filtered packet (e.g., `POST /login`). In the **Packet Details Pane**:

- Expand the **Hypertext Transfer Protocol** section.
- Scroll to:

```
HTML Form URL Encoded: application/x-www-form-urlencoded
```

There, you'll see:

```
Form item: "username" = "ironpotatoadmin"
Form item: "password" = "C1{maybe_TLS_would_be_nice}"
```

✅ Wireshark automatically decoded the form data—no manual decoding was needed.

---

## 🏁 Final Flag

```
C1{maybe_TLS_would_be_nice}
```

---
![wireshark1](https://github.com/user-attachments/assets/6d2a81a6-3076-4744-b71e-1afd0863b910)

## 📌 Key Takeaways

- Never send sensitive data like passwords over HTTP—always use HTTPS.
- Wireshark makes it easy to inspect form submissions in cleartext.
- Even headers and cookies can be exposed without encryption.

---

