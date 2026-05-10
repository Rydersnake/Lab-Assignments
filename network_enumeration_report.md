# Network Enumeration & Service Discovery Report

## Challenge 1 — NetBIOS Enumeration

### Command
```bash
nbtscan 192.168.56.0/24
```

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
