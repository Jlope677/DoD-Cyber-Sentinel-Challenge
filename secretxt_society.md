# 🔓 Secret.txt Society

**Category:** Web Security\
**Points:** 75

---

### 🧠 Description

Our team suspected that a Juche Jaguar developer accidentally left something interesting behind on a public site. The challenge hinted that bots were told to ignore something – which means the `robots.txt` file might hold a clue. Our task was to explore the structure of the site and uncover what was hidden from crawlers.

---

### 🔍 Step-by-Step Solution

#### 1. Initial Recon – Accessing the Site
![secret1](https://github.com/user-attachments/assets/c8e59f1f-6dd2-4be0-af34-513a309f5566)


URL provided:

```
https://juche.msoidentity.com/
```

The main page mentioned an update to the site's `robots.txt` file – a common place for hidden paths.

#### 2. Check robots.txt

Navigated to:
```
https://juche.msoidentity.com/robots.txt
```

Found the following:
# Friendly Crawler Guidelines for North Torbia
User-agent: *
Disallow: /juchejaguar/   # Absolutely nothing valuable in here. Promise.
```

#### 3. Access the Disallowed Path

Visited:

```
https://juche.msoidentity.com/juchejaguar/
```

The page revealed a congratulatory message and the flag.

---

### 🏁 Flag

```
C1{r0b0ts_arent_4lways_p0lit3}
```

---

### 🧰 Tools Used

- Web Browser
- Common knowledge of `robots.txt`

### 📝 Takeaways

- Always check `robots.txt` for hidden paths – especially when hinted.
- Don’t trust comments that say “nothing valuable here” 😏

