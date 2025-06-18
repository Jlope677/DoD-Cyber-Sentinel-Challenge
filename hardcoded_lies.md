# 🔐 Challenge: Hardcoded Lies – Configuration Extraction

**Category:** Malware / Reverse Engineering\
**Points:** 75


---

## 🧠 Challenge Summary

We were given a suspicious binary named `hardcodedlies`. While the executable doesn’t print anything when run, threat intel suspected a hardcoded configuration string—likely the flag—was embedded inside.

> Note: The binary was not actual malware, as clarified in the challenge.

📎 Provided file: `hardcodedlies`

---

## 🛠 Tools Used

| Tool      | Purpose                                      |
| --------- | -------------------------------------------- |
| `strings` | Extract human-readable content from binaries |
| Terminal  | Navigate and read binary outputs             |

---

## 🪜 Step-by-Step Process

### 🔹 Step 1: (Optional) Identify File Type

Use the `file` command to inspect what kind of binary we're dealing with:

```bash
file hardcodedlies
```

This can indicate whether it's an ELF, Mach-O, or PE executable—useful for advanced reverse engineering.

---

### 🔹 Step 2: Extract Strings from the Binary

Use the `strings` command to dump readable text from the binary:

```bash
strings hardcodedlies | less
```

This lists embedded human-readable content, including debug strings, hardcoded paths, and sometimes flags.

---

### 🔹 Step 3: Search for Suspicious Content

Scroll through the output of `strings`. In this case, the flag was clearly embedded in the output:

```
C1{h4rdc0ded_but_0verlooked}
```

As seen in the screenshot, this was surrounded by system library paths and internal symbols.
![hardcodelies1](https://github.com/user-attachments/assets/005c5d4f-fb85-4ae1-aea0-a744466a7fa9)

---

## 🏁 Final Flag

```
C1{h4rdc0ded_but_0verlooked}
```

---

## 📌 Key Takeaways

- `strings` is a fast and effective way to inspect binaries for readable secrets.
- It only works for non-obfuscated or plaintext content—reverse engineering tools are needed for more advanced analysis.
- Always inspect binaries statically before attempting execution.

---

