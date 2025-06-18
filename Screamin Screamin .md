# 😱 Screamin Screamin — CTF Write-Up

**Category:** Recon\
**Points:** 200

---

## 🎯 Challenge Summary

We received intel that Juche Jaguar exposed a network stream on host `34.85.185.78`, operating somewhere between TCP ports `5000–10000`. Our objective was to:

- Identify the correct RTSP port.
- Brute-force the correct stream name.
- Connect and extract the flag from the stream.

---

## 🧰 Tools Used

| Tool              | Purpose                          |
| ----------------- | -------------------------------- |
| `nmap`            | Port scanning and RTSP detection |
| `ffprobe`         | Silent RTSP probing              |
| `bash`            | Brute-force scripting            |
| `ffplay` or `VLC` | Stream viewing and playback      |

---

## 🪜 Step-by-Step Breakdown

### 🔎 Step 1: Scan for the RTSP Port

Used `nmap` to scan TCP ports 5000–10000 for RTSP services:

```bash
nmap -p 5000-10000 --script rtsp-methods -T4 34.85.185.78
```

📌 **Result:** Port `8774` was found open and likely running RTSP.

---

### 📦 Step 2: Install `ffprobe`

On Kali Linux, `ffprobe` may not be installed by default.

To fix:

```bash
echo 'deb http://http.kali.org/kali kali-rolling main non-free contrib' | sudo tee /etc/apt/sources.list
sudo apt update
sudo apt install ffmpeg
```

---

### 🚪 Step 3: Confirm RTSP Server

Tested the RTSP service using `ffprobe`:

```bash
ffprobe rtsp://34.85.185.78:8774/
```

📌 **Output:** `404 Not Found` — confirms RTSP is active, but stream name is required.

---

### 🧠 Step 4: Brute Force the Stream Name

Created a `streams.txt` list:

```text
stream
stream1
live
live.sdp
media
video
test
camera
cam
broadcast
```

Wrote a brute force script `brute_rtsp.sh`:

```bash
#!/bin/bash
HOST="34.85.185.78"
PORT="8774"

while IFS= read -r STREAM; do
  URL="rtsp://$HOST:$PORT/$STREAM"
  echo "[*] Trying: $URL"
  ffprobe -v error "$URL" > /dev/null 2>&1
  if [ $? -eq 0 ]; then
    echo "✅ SUCCESS: Stream found at $URL"
    break
  fi
done < streams.txt
```

Run the script:

```bash
chmod +x brute_rtsp.sh
./brute_rtsp.sh
```

📌 **Success:** Stream found at `/stream`

---
![Capture (1)](https://github.com/user-attachments/assets/19e582e9-46de-40c1-8c1e-871e8b14a250)

### 🎥 Step 5: View the Stream and Capture Flag

Used `ffplay` to open the stream:

```bash
ffplay rtsp://34.85.185.78:8774/stream
```

💡 **Flag appeared in the video stream:**

```
C1{RTSP_you_found_me}
```

---
![screamin screamin](https://github.com/user-attachments/assets/caf67cc4-564b-47a9-87cd-dfa1e5950b93)

## 🧠 Key Takeaways

- RTSP often doesn't advertise stream names — brute-force required.
- `ffprobe` is silent and efficient for probing stream validity.
- Custom Kali installs may require fixing package keys and repos.
- Combining network scanning and media tools = effective enumeration strategy.

---

## 🏁 Final Flag

```
C1{RTSP_you_found_me}
```

---

