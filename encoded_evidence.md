# 🔍 Challenge: Encoded Evidence

**Category:** Reverse Engineering / Malware Analysis\
**Points:** 75\
**Solves:** 556

---

## 🧠 Challenge Summary

We were provided with a suspicious VBScript file (`invoice.vbs`). It appeared to be part of a malware delivery mechanism, pulling additional payloads from a remote source. Analysts suspected the flag was embedded in the content retrieved by this script.

📎 Provided file: `invoice.vbs`

---

## 🛠 Tools Used

| Tool                                              | Purpose                            |
| ------------------------------------------------- | ---------------------------------- |
| PowerShell                                        | Retrieve remote content via script |
| [Base64Decode.org](https://www.base64decode.org/) | Decode base64 payload              |
| Notepad or any text editor                        | View and inspect VBS file contents |

---

## 🪜 Step-by-Step Solution

### 🔹 Step 1: Inspect the Script

The VBScript contained a hardcoded URL pointing to Pastebin:

```vbscript
url = "https://pastebin.com/raw/eqkzMd2M"
objXML.Open "GET", url, False
objXML.Send
response = objXML.responseText
MsgBox response
```

This code fetches content from the Pastebin URL and displays it using a message box.

---

### 🔹 Step 2: Use PowerShell to Fetch the Remote Payload

To retrieve the actual payload (which was hidden at the Pastebin URL), we used PowerShell:

```powershell
Invoke-WebRequest -Uri "https://pastebin.com/raw/eqkzMd2M" -UseBasicParsing | Select-Object -ExpandProperty Content
```

✅ Result returned:

```
QzF7bjBfZDNidWdfbjBfcDR5bn0K
```

---

### 🔹 Step 3: Decode the Base64 Content

We then decoded this base64 string using [base64decode.org](https://www.base64decode.org/), though other tools like `base64 -d` or Python could also be used.

Decoded Output:

```
C1{n0_d3bug_n0_p4yn}
```

✅ This was the hidden flag!

---

## 🏁 Final Flag

```
C1{n0_d3bug_n0_p4yn}
```

---

## 📌 Key Takeaways

- Malware droppers often fetch remote payloads—inspect embedded URLs!
- VBScript can hide flags in external resources.
- Base64 remains a popular obfuscation technique in CTFs and real-world malware.

---

