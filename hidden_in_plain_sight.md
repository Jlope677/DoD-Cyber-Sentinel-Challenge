# 🕵️ Hidden in Plain Sight

**Category:** Forensics\
**Points:** 75

---

### 🧠 Description

Analysts recovered a suspicious image from a threat actor’s social media account. At first glance, it looks like an innocent selfie – but insider reports suggest that a flag might be hiding in the image metadata. Can you extract it?

---

### 🔍 Step-by-Step Solution

#### 1. Use a Metadata Inspection Tool

The challenge hints that the flag is embedded within metadata or raw strings. An easy-to-use and effective tool is:

[https://29a.ch/photo-forensics/#strings](https://29a.ch/photo-forensics/#strings)

#### 2. Upload the File

Upload the provided image file (`selfie.png`) into the tool.

#### 3. Switch to the Strings Tab

Navigate to the `Strings` tab at the top. This displays embedded ASCII strings from the image file.

#### 4. Search for the Flag

Use `Ctrl + F` and search for `C1{` or scroll through manually.

#### 5. Locate the Flag

Found the following string:

```
C1{smile_youre_flagged}
```

---

### 🏁 Flag

```
C1{smile_youre_flagged}
```

---

### 🧰 Tools Used

- [29a.ch Photo Forensics](https://29a.ch/photo-forensics/#strings)
- Browser (for file upload)

### 📝 Takeaways

- Metadata often hides useful artifacts.
- Online forensics tools are great for CTFs when you don’t want to install anything.
- Always explore the strings inside image files!

