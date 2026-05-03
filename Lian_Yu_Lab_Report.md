# 🧪 Lab 6 Report – Lian Yu (TryHackMe)

## 📌 Overview
This report details the systematic exploitation of the Lian Yu machine on TryHackMe.
The objective was to identify vulnerabilities, gain initial access via insecure services, and perform privilege escalation to achieve root access.

---

## 📊 Summary Table

| Step | Phase | Action / Tool | Key Findings / Outcome |
|------|------|--------------|------------------------|
| 1 | Connectivity | `ping` | Confirmed target (10.49.176.128) is active |
| 2 | Reconnaissance | `nmap -sS -sV -A` | Found open ports (SSH, HTTP, FTP) |
| 3 | Web Enumeration | Browser | Static landing page discovered |
| 4 | Directory Busting | `gobuster` | Found `/island` |
| 5 | Deep Enumeration | DevTools + Gobuster | Found keyword vigilante, directory `/2100` |
| 6 | Data Retrieval | `.ticket` file | Found green_arrow.ticket, decoded credentials |
| 7 | Initial Access | `ftp` | Accessed files (images) |
| 8 | File Repair | `hexedit` | Fixed corrupted PNG file |
| 9 | Data Analysis | Image Viewer | Extracted hidden password |
| 10 | Steganography | `steghide` | Retrieved SSH credentials |
| 11 | Privilege Escalation | `ssh`, `sudo -l` | Root access achieved |

---

## 🚀 Step-by-Step Walkthrough

### Step 0: Start the Room
Start the target machine on TryHackMe.

---

### Step 1: Connectivity Check
```bash
ping 10.49.176.128
```

---

### Step 2: Initial Reconnaissance
```bash
nmap -sS -sV -A 10.49.176.128
```

---

### Step 3: Web Exploration
http://10.49.176.128/

---

### Step 4: Directory Brute Forcing
```bash
gobuster dir -u http://10.49.176.128 -w /usr/share/wordlists/dirb/common.txt
```

---

### Step 5: Further Enumeration
```bash
gobuster dir -u http://10.49.176.128/island -w /usr/share/wordlists/dirb/common.txt
```

---

### Step 6: Find `.ticket` File
green_arrow.ticket → decode credentials

---

### Step 7: FTP Access
```bash
ftp 10.49.176.128
```

---

### Step 8: Download Files
```bash
get Leave_me_alone.png
get Queen_Gambit.png
get aa.jpg
```

---

### Step 9: Fix Corrupted Image
```bash
hexedit Leave_me_alone.png
```

---

### Step 10: Steganography
```bash
steghide extract -sf Queen_Gambit.png
cat <extracted_file>
```

---

### Step 11: Privilege Escalation
```bash
ssh slade@10.49.176.128
sudo -l
```

---

## ✅ Conclusion
The system was successfully compromised and root access achieved.

---

## 🛠️ Tools Used
- Nmap
- Gobuster
- FTP
- Hexedit
- Steghide
- SSH
