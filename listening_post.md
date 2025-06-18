# 🛰️ Listening Post – Audio Signal Decode Challenge

**Category:** Forensics / Signal Analysis\
**Points:** 150\
**Solves:** 267

---

## 🎯 Challenge Description

We intercepted a satellite radio broadcast likely intended for North Torbian operatives. The audio signal may contain an encoded flag. Two versions of the same recording were provided in `.wav` and `.mp3` formats.

---

## 🧩 Solution Overview

### 🔊 1. Analyze the Audio File

- Files provided: `radio.wav`, `radio.mp3`
- Used `radio.wav` for analysis (10 seconds, mono, 48kHz)
- Tools: `Python`, `matplotlib`, `scipy`

🧠 **Finding**: Audio consisted of structured tone bursts. Not Morse or voice.

---

### 🧪 2. Attempt Morse Code Decoding (Failed)

- Tool: [https://morsecode.world](https://morsecode.world/international/decoder/audio-decoder.html)
- Result: Decoding produced garbled output.

❌ **Conclusion**: Likely not Morse code.

---

### 📊 3. Visual Spectrogram Analysis

- Used Python and `matplotlib` to generate a spectrogram.
- Noted consistent tone blocks at defined frequencies.

✅ **Conclusion**: The pattern resembled DTMF (Dual-Tone Multi-Frequency) signals.

---

### 🔓 4. Decode DTMF Tones

- Tool used: [https://dtmf.netlify.app](https://dtmf.netlify.app)
- Uploaded `radio.wav`
- Output: Long binary sequence

```txt
0100001100110001011110110111001000110100011001000011000101101111
0101111101101011001100010110110001101100001100110110010001011111
0111010001101000001100110101111101110100001100000111001001100010
0011000101100001010111110111001101110100001101000111001001111101
```

---

### 🔡 5. Convert Binary to ASCII

Used Python to decode the binary string:

```python
binary_data = (
    "0100001100110001011110110111001000110100011001000011000101101111"
    "0101111101101011001100010110110001101100001100110110010001011111"
    "0111010001101000001100110101111101110100001100000111001001100010"
    "0011000101100001010111110111001101110100001101000111001001111101"
)

byte_chunks = [binary_data[i:i+8] for i in range(0, len(binary_data), 8)]
decoded_flag = ''.join([chr(int(byte, 2)) for byte in byte_chunks])
print(decoded_flag)
```

### 🏁 Flag:

```
C1{r4d1o_k1ll3d_th3_t0rb1a_st4r}
```

---

## 🧰 Tools Used

| Tool                                                                                | Purpose                             |
| ----------------------------------------------------------------------------------- | ----------------------------------- |
| Python (scipy, matplotlib)                                                          | Audio visualization and spectrogram |
| [morsecode.world](https://morsecode.world/international/decoder/audio-decoder.html) | Morse decoding attempt              |
| [dtmf.netlify.app](https://dtmf.netlify.app)                                        | DTMF-to-binary decoding             |
| Python script                                                                       | Binary to ASCII conversion          |

---

## ✅ Summary

This challenge required decoding structured tones embedded in an audio file. Although Morse decoding failed, spectrogram analysis revealed it was DTMF. Using an online DTMF decoder and a binary-to-ASCII Python script, we successfully extracted the flag.

---

