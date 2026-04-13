# 🔐 Penetration Testing Lab Report   
## VPN Access, Network Scanning & Web Enumeration (Nmap + Gobuster)

---

## 1. Opening the VPN (PRWTillDawn)

Before any internal network testing could be performed, a VPN connection was established using the provided configuration file (**PRWTillDawn**).

### Purpose:
- The VPN acts as a secure tunnel into the internal lab network.
- It allows the attacker machine (Kali Linux) to access private IP ranges such as `10.150.150.0/24`.
- Without the VPN connection, the target system would not be reachable.

### Process Summary:
- The VPN configuration file was imported into the VPN client.
- The connection was initiated successfully.
- Once connected, the system received access to the internal lab network environment.

### Result:
✔ Secure connection established  
✔ Internal network access enabled  

<img width="642" height="511" alt="Step 1 Opening the VPN PRWTillDawn" src="https://github.com/user-attachments/assets/dc799063-7572-4da4-86b8-12f23755097a" />

---

## 2. Network Scanning Using Nmap

After successfully connecting to the VPN, the next step was to identify active hosts and open services within the network using **Nmap (Network Mapper)**.

### Purpose:
- Discover live hosts in the network.
- Identify open ports and running services.
- Gather information for further exploitation or enumeration.

### Methodology:
- A scan was performed on the target subnet.
- The tool checked for:
  - Open TCP ports
  - Service versions
  - Operating system fingerprints (if enabled)

### Output Interpretation:
- Open ports indicate active services running on the target machine.
- Each service may represent a potential attack surface (e.g., HTTP, FTP, SSH).

### Result:
✔ Target host identified  
✔ Open services discovered  
✔ Attack surface mapped  

<img width="644" height="508" alt="Step 2 Scanning the Nmap" src="https://github.com/user-attachments/assets/1f668b42-ee8b-4a03-972d-dde7e598b45c" />

---

## 3. Target Selection & Port Scanning (10.150.150.11)

From the list of discovered hosts, the target IP address **10.150.150.11** was selected for deeper analysis.

### Purpose:
- Perform a focused scan on a single host.
- Identify detailed service versions and exposed ports.
- Understand possible entry points into the system.

### Focus Areas:
- Common ports (HTTP, SSH, FTP, etc.)
- Service versions (to detect outdated or vulnerable software)
- Banner information (for fingerprinting services)

### Result:
✔ Active services confirmed on target  
✔ Ports and service details enumerated  
✔ Potential web service identified for further testing  

<img width="640" height="368" alt="Step 3 Pick one then do Port Scanning (10 150 150 11)" src="https://github.com/user-attachments/assets/9e48248b-1683-4851-a34d-457d6047a38d" />

---

## 4. Web Enumeration Using Gobuster

After confirming that a web service was running on the target, **Gobuster** was used to perform directory brute-forcing.

### Purpose:
- Discover hidden directories and files on the web server.
- Identify admin panels, uploads, backups, or sensitive endpoints.
- Expand attack surface beyond visible web pages.

### Methodology:
- A wordlist-based enumeration approach was used.
- Gobuster sent multiple HTTP requests to guess valid paths.
- Responses were analyzed based on HTTP status codes (e.g., 200, 301, 403).

### Expected Findings:
- `/admin`
- `/uploads`
- `/backup`
- `/login`
- Hidden configuration or vulnerable endpoints

### Result:
✔ Hidden directories discovered (if any)  
✔ Web application structure mapped  
✔ Potential entry points identified for further exploitation  

<img width="937" height="635" alt="Step 4 attack with Gobuster" src="https://github.com/user-attachments/assets/3ce6692e-e8bc-4975-bc8b-1bbf566790e9" />


---
