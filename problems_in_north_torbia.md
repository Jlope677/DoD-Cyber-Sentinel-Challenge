# 🕵️‍♀️ Problems in North TORbia — CTF Write-Up

**Category:** Web / Forensics\
**Points:** 150\


---

## 🎯 Challenge Summary

We were provided a ransom note that referenced a dark web .onion service. Although no actual encryption occurred, we were instructed to investigate the site and determine whether any hidden information could be recovered from the page.

📎 File provided: `note.txt`

> The note contained the following URL:

```
http://jjpwn5u6ozdmxjurfitt42hns3qovikeyhocx5b2byoxgupnuzd2vkid.onion/
```

---

## 🛠️ Tools Used

- Tor Browser
- Firefox / Chrome DevTools (Inspect Element)
- Basic HTML knowledge

---

## 🪜 Steps to Solve

### 🔹 Step 1: Open the .onion Site via Tor

We opened the ransom note's Tor URL in the Tor Browser:

```
http://jjpwn5u6ozdmxjurfitt42hns3qovikeyhocx5b2byoxgupnuzd2vkid.onion/
```

The page displayed a dark-themed submission form asking for:

- First Name
- Last Name
- Email
- Ransom Code

---

### 🔹 Step 2: Inspect the Source Code

Using the browser’s **DevTools > Inspector**, we searched the HTML form fields.

Hidden at the bottom of the form, we found:

```html
<input id="send" type="hidden" name="ransom_code" value="ransom@ethtrader-ai.com">
<input id="send_data" type="hidden" value="C1{h1dd3n_f13lds_0f_0n10ns}">
```

---

### 🔹 Step 3: Capture the Flag

The second hidden input field `send_data` contained the flag in plain text:

```
C1{h1dd3n_f13lds_0f_0n10ns}
```

✅ **This was the correct flag!**

---

## 🧠 Key Takeaways

- Always inspect HTML source code during CTF challenges.
- Don’t trust visible inputs—use DevTools to uncover hidden fields.
- Even when the surface appears simple, hidden clues may lie beneath.

---

## 🏁 Final Flag

```
C1{h1dd3n_f13lds_0f_0n10ns}
```

---

