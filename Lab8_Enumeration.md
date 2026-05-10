# Network Enumeration & Service Discovery Report

## Challenge 1 — NetBIOS Enumeration

### Command
```bash
nbtscan 192.168.56.0/24
```
<img width="642" height="143" alt="Challenge 1 - NetBIOS Enumeration" src="https://github.com/user-attachments/assets/b6527e43-f750-4a1c-8e40-f7da0494b35f" />

### Description
NetBIOS enumeration was performed using `nbtscan` against the target IP address `192.168.56.104`. The scan attempted to retrieve NetBIOS information such as:

- Hostname
- Workgroup details
- NetBIOS names

### Findings
The target responded to NetBIOS queries, confirming that NetBIOS services were enabled and accessible on the network.

---

## Challenge 2 — Fast Nmap Scan

### Command
```bash
nmap -F 192.168.56.104
```
<img width="625" height="507" alt="Challenge 2 — Fast Nmap Scan" src="https://github.com/user-attachments/assets/b458f198-e7eb-4e12-8ec9-74ee19d54064" />

### Description
A fast Nmap scan was performed to quickly identify commonly used open ports and services on the target machine.

### Findings
The scan identified multiple open ports and active services, including:

- FTP
- Telnet
- SMB
- HTTP

These exposed services indicate a relatively large attack surface.

---

## Challenge 5 — TTL OS Fingerprinting

### Command
```bash
ping 192.168.56.104
```
<img width="553" height="453" alt="Challenge 3 — TTL OS Fingerprinting" src="https://github.com/user-attachments/assets/0d67f907-b8df-4b68-a62c-2a608e119805" />

### Description
TTL (Time To Live) values from ICMP responses were analyzed to estimate the target operating system.

### Findings
The observed TTL value was:

```text
TTL = 64
```

This value commonly indicates that the target system is likely running a Linux operating system.

---

## Challenge 9 — FTP Banner Grabbing

### Command
```bash
nc 192.168.56.104 21
```
<img width="347" height="88" alt="Challenge 4 — FTP Banner" src="https://github.com/user-attachments/assets/7063a63c-a3e0-4008-90d0-6d49dcd94e37" />

### Description
Netcat was used to connect to the FTP service and retrieve the server banner.

### Findings
FTP banner grabbing revealed the FTP server version running on the target machine. Version disclosure may assist attackers in identifying known vulnerabilities associated with the FTP service.

---

## Challenge 10 — Anonymous FTP Login

### Command
```bash
ftp 192.168.56.104
```
<img width="445" height="294" alt="Challenge 5 — Anonymous FTP Login" src="https://github.com/user-attachments/assets/1190c48a-657d-4e7b-8d2e-2a20cea147f2" />

### Description
An FTP connection was established to test whether anonymous authentication was enabled.

### Findings
Anonymous FTP authentication was enabled on the target system, allowing unauthenticated users to access FTP resources.

Potential risks include:

- Unauthorized file access
- Information disclosure
- Upload of malicious files

---

## Challenge 7 — SMTP VRFY Enumeration

### Command
```bash
telnet 192.168.56.104 25
```
<img width="454" height="200" alt="Challenge 6 — SMTP VRFY" src="https://github.com/user-attachments/assets/e1236ce7-9635-4a6a-a863-963969f62ff3" />

### Description
SMTP enumeration was performed through the SMTP service to test support for user verification commands such as `VRFY`.

### Findings
The SMTP server revealed valid usernames.

This information could be useful for:

- Brute force attacks
- Password guessing
- User enumeration

---

## Challenge 11 — SMB NSE Enumeration

### Command
```bash
nmap --script smb-enum-shares.nse -p445 192.168.56.104
```
<img width="613" height="817" alt="Challenge 7 — SMB Enumeration" src="https://github.com/user-attachments/assets/87b4fa0d-44a7-4b2a-9d0b-8dfa9b32ee36" />

### Description
The Nmap NSE script `smb-enum-shares` was used to enumerate SMB shared folders and permissions.

### Findings
The script attempted to identify:

- Shared folders
- Access permissions
- Anonymous access rights

Misconfigured SMB shares may allow unauthorized users to access sensitive files.

---

## Challenge 12 — Enum4linux Enumeration

### Command
```bash
enum4linux 192.168.56.104
```
<img width="968" height="896" alt="Challenge 8 — Enum4linux" src="https://github.com/user-attachments/assets/beb14b5b-f0fc-478e-9350-9da81957b130" />
<img width="996" height="872" alt="Challenge 8 — Enum4linux (1)" src="https://github.com/user-attachments/assets/302efd13-981d-4218-92ee-a406f0e153e3" />
<img width="916" height="888" alt="Challenge 8 — Enum4linux (2)" src="https://github.com/user-attachments/assets/cfa92fa8-08b9-4a77-a022-cd54ab20e469" />
<img width="901" height="891" alt="Challenge 8 — Enum4linux (3)" src="https://github.com/user-attachments/assets/faa328b4-d3d4-432e-b12f-57e32c1ae9da" />
<img width="925" height="793" alt="Challenge 8 — Enum4linux (4)" src="https://github.com/user-attachments/assets/86dca64a-97df-4100-9357-8fa2d28dbdad" />



### Description
`enum4linux` was used to perform detailed enumeration against the SMB service.

### Findings
The tool successfully enumerated:

- Users
- Groups
- Shared folders
- Password policies
- NetBIOS information

This information can assist attackers in privilege escalation and lateral movement.

---

## Challenge 13 — NFS Exports Enumeration

### Command
```bash
showmount -e 192.168.56.104
```
<img width="342" height="85" alt="Challenge 9 — NFS Exports" src="https://github.com/user-attachments/assets/4fffff16-5725-4a96-8834-9eab512c300f" />

### Description
The `showmount` command was used to identify exported NFS shared directories.

### Findings
The target system exposed NFS shared directories that may allow unauthorized file access if permissions are misconfigured.

Potential risks include:

- Unauthorized data access
- File modification
- Sensitive information disclosure

---

## Challenge 16 — Service Version Detection

### Command
```bash
nmap -sV 192.168.56.104
```
<img width="911" height="606" alt="Challenge 10 — Version Detection" src="https://github.com/user-attachments/assets/5ce69457-218a-46cd-aa13-87f8e5c004b7" />

### Description
A detailed service version detection scan was performed to identify software versions and protocols running on the target system.

### Findings
The scan identified:

- Service versions
- Software details
- Protocol information

Detected services may include:

- vsFTPd
- OpenSSH
- Apache
- Samba

Version information can be used to identify publicly known vulnerabilities associated with outdated software.
