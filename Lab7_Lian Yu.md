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
This report details the systematic exploitation of the "Lian Yu" machine on TryHackMe. The objective was to identify vulnerabilities, gain initial access via insecure services, and perform privilege escalation to achieve root access.
<img width="940" height="130" alt="image" src="https://github.com/user-attachments/assets/750b5d10-0f7d-4d1e-9780-921d674c34f8" />


---

### Step 1: Connectivity Check
The initial step in the penetration testing process involved verifying network connectivity between the attacker machine (running Kali Linux) and the target system. 

```bash
ping 10.49.176.128
```
<img width="868" height="319" alt="image" src="https://github.com/user-attachments/assets/88f91cd3-5bb1-4831-98dd-14d5f1e6e3bf" />
This was achieved by executing the ping command against the provided IP address to confirm that the target host was online, responsive, and fully booted.

---

### Step 2: Initial Reconnaissance
A comprehensive Nmap scan was performed to identify open ports and running services: nmap -sS -sV -A 10.49.176.128
-sS (TCP SYN Stealth Scan): To scan quickly and bypass some firewall logging.
-sV (Version Detection): To identify the specific versions of software running.
-A (Aggressive): To enable OS detection, script scanning, and traceroute.


```bash
nmap -sS -sV -A 10.49.176.128
```
<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/c7b99f38-a5b3-47a7-9e30-0d96de1d76e9" />

---

### Step 3: Web Exploration
Navigating to [http://10.49.176.128/](http://10.49.176.128/) revealed a static landing page. Initial inspection focused on identifying hidden clues within the UI or source code.

http://10.49.176.128/
<img width="940" height="416" alt="image" src="https://github.com/user-attachments/assets/b466af18-f5d4-4102-a508-7fc966263b24" />


---

### Step 4: Directory Brute Forcing
To uncover hidden directories and files that were not visible through normal browsing, the tool Gobuster was used to perform directory brute-forcing against the web server.

```bash
gobuster dir -u http://10.49.176.128 -w /usr/share/wordlists/dirb/common.txt
```
<img width="940" height="294" alt="image" src="https://github.com/user-attachments/assets/63417b28-2e04-4278-bb7f-cdd3961f6281" />


This process successfully revealed the existence of a hidden directory named /island, which indicated that additional content was available beyond the main landing page and warranted further investigation.

<img width="940" height="42" alt="image" src="https://github.com/user-attachments/assets/15c652a5-4512-4868-b130-48e98d91fbfe" />


---
### Step 5: Further Enumeration
Upon navigating to the /island directory using the browser,

<img width="940" height="233" alt="image" src="https://github.com/user-attachments/assets/4e0b356c-6451-4816-807d-b641d547dfe3" />

additional enumeration was conducted. By opening the browser’s Developer Tools (using the F12 key), a hidden keyword “vigilante” was discovered within the HTML comments.

<img width="493" height="207" alt="image" src="https://github.com/user-attachments/assets/303870df-585a-4ad3-b7fb-90cb0963c5fa" />

A subsequent Gobuster scan targeting the /island directory revealed another subdirectory named /2100
```bash
gobuster dir -u http://10.49.176.128/island -w /usr/share/wordlists/dirb/common.txt
```
<img width="940" height="43" alt="image" src="https://github.com/user-attachments/assets/e0978e3a-1588-4b00-b40a-d0ea9dbe7307" />

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/a1fcae00-ce2c-4613-b42b-15458b39cb47" />


---

### Step 6: Find `.ticket` File
During the enumeration of the /2100 directory, a file named green_arrow.ticket was discovered. This file appeared to contain an encoded string, suggesting that sensitive information had been intentionally obfuscated.
<img width="674" height="39" alt="image" src="https://github.com/user-attachments/assets/0effb781-ff41-466f-abc6-51a0cf9fadc5" />

green_arrow.ticket → decode credentials
<img width="940" height="358" alt="image" src="https://github.com/user-attachments/assets/3c17d1a5-40c2-4089-be3a-d56c6e2bdd42" />

After decoding the contents of the file using appropriate techniques, valid credentials were successfully retrieved.

---

### Step 7: FTP Access
```bash
ftp 10.49.176.128
```
<img width="642" height="381" alt="image" src="https://github.com/user-attachments/assets/68dbd736-bb63-40bb-b9d7-91af260da209" />
Upon successful login, three files were identified and transferred to the local machine Leave_me_alone.png, Queen_Gambit.png and aa.jpg

<img width="863" height="587" alt="image" src="https://github.com/user-attachments/assets/33c167e0-a5b8-48c5-8a26-19ef91a22b74" />

---

### Step 8: Download Files
```bash
get Leave_me_alone.png
get Queen_Gambit.png
get aa.jpg
```
<img width="940" height="182" alt="image" src="https://github.com/user-attachments/assets/c9dc125b-ef25-435b-94e8-235a7f72c3a4" />

All of these files were downloaded to the local Kali Linux machine for further analysis, as they potentially contained hidden or sensitive information relevant to the exploitation process.

---

### Step 9: Fix Corrupted Image

Upon inspecting the downloaded files, it was observed that the file Leave_me_alone.png was corrupted and could not be opened normally. To resolve this issue, a hex editor tool was used to manually examine and modify the file’s binary structure.

```bash
hexedit Leave_me_alone.png
```
<img width="940" height="412" alt="image" src="https://github.com/user-attachments/assets/614c83c9-0a68-4168-ab41-279a40dea0ee" />


After making this correction, the image was successfully restored and could be opened, revealing a hidden password embedded within the image.
<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/52e085c1-50fc-4f7b-8b10-c9446c101642" />


---

### Step 10: Steganography
Using the password obtained from the repaired image, further analysis was conducted using the steganography tool Steghide. This tool was used to extract hidden data embedded within the image files.

```bash
steghide extract -sf aa.jpg
```

```bash
unzip ss.zip
```

<img width="466" height="291" alt="image" src="https://github.com/user-attachments/assets/01c4f044-b6b1-4c12-8c81-828095bd08c5" />

Through this process, additional concealed information was uncovered, including credentials that were not visible through normal file inspection.By examining the extracted data using simple commands such as cat, valid SSH login credentials for a user named “slade” were successfully identified.

---

### Step 11: Privilege Escalation
With the newly obtained credentials, a secure shell connection was established to the target system using the command: ssh slade@10.49.176.128

```bash
ssh slade@10.49.176.128
sudo -l
```
<img width="940" height="690" alt="image" src="https://github.com/user-attachments/assets/129cc29a-225b-48c0-81fd-8901a49b1e38" />

After gaining access as the user “slade,” the next objective was to escalate privileges in order to obtain root-level access. This was initiated by executing the command sudo -l

<img width="940" height="525" alt="image" src="https://github.com/user-attachments/assets/4688e247-9808-449b-bb09-7b538ea4ade4" />


which lists all commands that the current user is allowed to run with elevated privileges. By analyzing the output of this command, potential misconfigurations or exploitable binaries were identified, which could be leveraged to gain root access. Through successful exploitation of these privileges, full administrative control over the system was achieved, thereby completing the objectives of the penetration testing exercise.

---

## 🛠️ Tools Used
- Nmap
- Gobuster
- FTP
- Hexedit
- Steghide
- SSH
