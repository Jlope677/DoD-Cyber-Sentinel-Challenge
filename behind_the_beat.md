# 🎧 Challenge: Behind the Beat – Metadata Hidden Flag

**Category:** Forensics /
**Points:** 75


---

## 🧠 Challenge Summary

Agents intercepted an audio file named `message.mp3`. When played, the file emits a single tone, providing no useful auditory clues. However, threat intel suggested that the flag might be embedded within the metadata of the MP3 file rather than its audio stream.

📎 Provided file: `message.mp3`

---

## 🛠 Tools Used

| Tool     | Purpose                                      |
| -------- | -------------------------------------------- |
| Python   | Running the extraction script                |
| Mutagen  | Python library for working with MP3 metadata |
| Terminal | Script execution and navigation              |

---

## 🪜 Step-by-Step Process

### 🔹 Step 1: Install Mutagen

Install the Python `mutagen` package if it's not already available:

```bash
pip install mutagen
```

---

### 🔹 Step 2: Run Metadata Extraction Script

Create a file named `extract_metadata.py` and paste the following code:

```python
from mutagen.mp3 import MP3
from mutagen.id3 import ID3

# Load the MP3 file
file_path = "message.mp3"
audio = MP3(file_path, ID3=ID3)

# Print all metadata tags
print("Metadata found in the MP3 file:\n")
for tag in audio.tags.keys():
    print(f"{tag}: {audio.tags[tag]}")
```

Execute the script:

```bash
python extract_metadata.py
```

---

### 🔹 Step 3: Analyze the Output

The output returned the following tags:

```
TENC: C1{metadata_tells_more}
TSSE: Lavf61.7.100
```

✅ The flag was stored in the `TENC` (Encoded By) metadata field.

---

## 🏁 Final Flag

```
C1{metadata_tells_more}
```

---

## 📌 Key Takeaways

- Always inspect file metadata during forensic or CTF investigations.
- Tools like Mutagen allow fast and reliable metadata extraction from audio files.
- Flags may be hidden in non-obvious areas like tags or encodings rather than content itself.

---

