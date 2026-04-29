# 🔐 CompTIA Security+ SY0-701 Lab Portfolio

<div align="center">

![CompTIA Security+](https://img.shields.io/badge/CompTIA-Security%2B%20SY0--701-FF0000?style=for-the-badge&logo=comptia&logoColor=white)
![Passed](https://img.shields.io/badge/Exam-Passed-00C853?style=for-the-badge&logo=checkmarx&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Wireshark](https://img.shields.io/badge/Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white)

**Deborah Chidimma Chukwuezi | CompTIA Security+ Certified**

</div>

---

## 📋 About This Repository

This repository documents hands-on lab work aligned with all **5 domains** of the **CompTIA Security+ SY0-701** certification exam. Each lab demonstrates practical application of security concepts using industry-standard tools in a controlled lab environment.

**Lab Environment:**
- Virtualization: VMware Workstation / VirtualBox
- OS Platforms: Kali Linux, Windows Server 2022, Ubuntu 22.04
- Tools: Wireshark, Nmap, Metasploit Framework, Nessus Essentials, Burp Suite

---

## 📚 Domain Coverage

| Domain | Weight | Labs Completed | Score |
|:---|:---:|:---:|:---:|
| 1.0 General Security Concepts | 12% | 8/8 | ✅ |
| 2.0 Threats, Vulnerabilities & Mitigations | 22% | 12/12 | ✅ |
| 3.0 Security Architecture | 18% | 10/10 | ✅ |
| 4.0 Security Operations | 28% | 15/15 | ✅ |
| 5.0 Security Program Management & Oversight | 20% | 10/10 | ✅ |

---

## 🔴 Domain 1: General Security Concepts (12%)

### Lab 1.1 — Cryptography Implementation

**Objective:** Demonstrate understanding of cryptographic algorithms and their applications.

**Tools Used:** OpenSSL, GPG, Windows CertMgr

**Lab Tasks Completed:**
- Generated RSA 2048-bit and 4096-bit key pairs using OpenSSL
- Created and signed X.509 digital certificates
- Implemented AES-256 file encryption/decryption
- Configured SHA-256 hashing for file integrity verification
- Set up asymmetric encryption for secure email using GPG

**Key Commands Used:**
```bash
# Generate RSA key pair
openssl genrsa -out private.key 4096
openssl rsa -in private.key -pubout -out public.key

# Create self-signed certificate
openssl req -new -x509 -key private.key -out cert.crt -days 365

# Encrypt file with AES-256
openssl enc -aes-256-cbc -salt -in plaintext.txt -out encrypted.bin -k SecureP@ss!

# Generate file hash
sha256sum important_file.txt
```

**Key Findings:**
- AES-256 provides 256-bit symmetric encryption — industry standard for data at rest
- PKI infrastructure requires proper certificate chain validation
- Hash collisions impossible with SHA-256 for practical purposes

---

### Lab 1.2 — PKI and Certificate Management

**Objective:** Build and manage a Public Key Infrastructure environment.

**Tasks Completed:**
- Deployed a Windows CA (Certificate Authority) hierarchy
- Issued server certificates for HTTPS
- Implemented CRL (Certificate Revocation List) and OCSP
- Configured certificate pinning

---

## 🟠 Domain 2: Threats, Vulnerabilities & Mitigations (22%)

### Lab 2.1 — Network Reconnaissance with Nmap

**Objective:** Perform ethical network scanning and reconnaissance.

**Tools:** Nmap 7.94, Wireshark 4.2

**Lab Tasks Completed:**
- Performed host discovery using ICMP and TCP ping sweeps
- Conducted service/version detection (-sV flag)
- Identified operating systems using OS fingerprinting (-O flag)
- Used NSE (Nmap Scripting Engine) for vulnerability detection
- Analyzed traffic captures in Wireshark

**Nmap Scan Results (Lab Environment):**
```
Nmap scan report for 192.168.1.0/24
Host: 192.168.1.10 (Windows Server 2022)
PORT     STATE  SERVICE    VERSION
22/tcp   closed ssh
80/tcp   open   http       Apache httpd 2.4.54
135/tcp  open   msrpc      Microsoft Windows RPC
443/tcp  open   ssl/https  Apache httpd 2.4.54
445/tcp  open   microsoft-ds
3389/tcp open   ms-wbt-server Microsoft Terminal Services
OS Details: Windows Server 2022 Datacenter
```

**Vulnerability Identified:** RDP exposed on 3389 — Remediation: Network segmentation, NLA required, MFA enforced

---

### Lab 2.2 — Social Engineering & Phishing Analysis

**Objective:** Identify and analyze phishing attempts and social engineering attacks.

**Tools:** Email header analyzer, VirusTotal, PhishTool

**Phishing Email Analysis — Sample Headers:**
```
From: "IT Department" <it-support@companyname-helpdesk.com>
Reply-To: attacker@malicious-domain.ru
X-Originating-IP: 185.220.101.45
Authentication-Results: spf=fail; dkim=none; dmarc=fail
```

**Red Flags Identified:**
1. Domain mismatch — "companyname-helpdesk.com" vs actual domain
2. SPF/DKIM/DMARC all failing — email not from legitimate server
3. Originating IP from known Tor exit node
4. Urgency language: "Your account will be locked in 24 hours"
5. Suspicious URL redirect to credential harvesting page

**Mitigation Applied:** 
- Email filtering rules updated in Exchange
- Security awareness training initiated
- DMARC policy set to "reject"

---

### Lab 2.3 — Vulnerability Assessment with Nessus

**Objective:** Perform a vulnerability scan and produce a remediation report.

**Tools:** Nessus Essentials 10.7, Lab Windows Server 2019

**Scan Summary Results:**

| Severity | Count | Examples |
|:---:|:---:|:---|
| Critical | 3 | MS17-010 EternalBlue, BlueKeep RDP, Log4Shell |
| High | 8 | Outdated OpenSSL, TLS 1.0 enabled, Weak cipher suites |
| Medium | 15 | Self-signed cert, Default credentials, Info disclosure |
| Low | 12 | Banner grabbing, ICMP timestamp, HTTP methods |
| Info | 47 | Open ports, service versions, OS detection |

**Remediation Actions Taken:**
- MS17-010: Applied KB4012212 patch — Critical resolved
- TLS 1.0 disabled, TLS 1.3 enforced — High resolved
- Default credentials changed on all services — Medium resolved

---

### Lab 2.4 — Malware Analysis (Static & Dynamic)

**Objective:** Analyze malware samples in a sandboxed environment.

**Tools:** Any.run sandbox, Ghidra, VirusTotal, PEStudio

**Sample Analysis (WannaCry Variant — Sandboxed):**

Static Analysis:
- File Type: PE32 executable (Windows)
- File Size: 3.4 MB
- SHA256: 24d004a104d4d54034dbcffc2a4b19a11f39008a575aa614ea04703480b1022c
- Packed: Yes (UPX 3.94)
- Suspicious imports: CreateRemoteThread, VirtualAllocEx, WriteProcessMemory

Dynamic Analysis:
- Attempts to contact C2 server: wanna1.onion
- Creates shadow copies deletion: vssadmin delete shadows /all /quiet
- Enumerates network shares for lateral movement
- Drops ransom note: @Please_Read_Me@.txt

**Indicators of Compromise (IOCs):**
```
File: tasksche.exe (SHA256: 24d004a104...)
Registry: HKLM\SOFTWARE\WanaCrypt0r
Network: Port 445 scanning, Tor connections
Process: cmd.exe spawned from ransomware binary
```

---

## 🟢 Domain 3: Security Architecture (18%)

### Lab 3.1 — Network Segmentation & Firewall Configuration

**Objective:** Design and implement network segmentation using VLANs and firewall rules.

**Network Design Implemented:**
```
Internet
    │
[Firewall/Router - pfSense]
    │
    ├── DMZ (VLAN 10) - 10.0.10.0/24
    │   ├── Web Server
    │   └── Mail Gateway
    │
    ├── Corporate (VLAN 20) - 10.0.20.0/24
    │   ├── Workstations
    │   └── File Servers
    │
    ├── Management (VLAN 30) - 10.0.30.0/24
    │   └── Domain Controllers, Network Mgmt
    │
    └── IoT (VLAN 40) - 10.0.40.0/24
        └── Isolated IoT devices
```

**Firewall Rules Applied:**
- DMZ → Internet: Allow HTTP/HTTPS only
- Internet → DMZ: Allow 80, 443 inbound only
- Corporate → DMZ: Allow all (internal access)
- IoT → Corporate: DENY all (isolated)
- Management → All VLANs: Allow all (admin access with MFA)

---

### Lab 3.2 — Cloud Security Configuration (AWS)

**Objective:** Implement security controls in AWS cloud environment.

**AWS Security Controls Implemented:**
- IAM: Created roles with least-privilege policies
- S3 Bucket: Disabled public access, enabled versioning and MFA delete
- CloudTrail: Enabled in all regions, logs sent to S3 with KMS encryption
- Security Groups: Configured to deny-all default, allow-specific rules
- GuardDuty: Enabled for threat detection
- Config: Enabled for compliance monitoring
- VPC Flow Logs: Enabled for network traffic analysis

---

## 🔵 Domain 4: Security Operations (28%)

### Lab 4.1 — SIEM Implementation & Log Analysis (Splunk)

**Objective:** Configure SIEM, create alerts, and investigate security events.

**Splunk Configuration:**
- Deployed Splunk Enterprise 9.1
- Configured data inputs from Windows Event Logs, Syslog, Apache logs
- Created custom dashboards for security monitoring
- Built correlation rules for threat detection

**Sample SPL Queries Used:**
```
# Failed login attempts
index=windows EventCode=4625 | stats count by src_ip, user | where count > 5

# Brute force detection
index=windows EventCode=4625
| bucket _time span=1m
| stats count by _time, src_ip
| where count > 10

# Privilege escalation detection
index=windows EventCode IN (4672, 4728, 4732, 4756)
| table _time, user, dest, EventCode, message

# Lateral movement via SMB
index=network dest_port=445
| stats dc(dest) as unique_dest by src_ip
| where unique_dest > 10
```

**Alerts Created:**
- Brute Force Attack: >10 failed logins in 1 minute from same IP
- Privilege Escalation: Admin group membership changes
- Data Exfiltration: Large outbound data transfer (>100MB)
- Malware C2: Connections to known malicious IPs

---

### Lab 4.2 — Incident Response Simulation

**Objective:** Execute incident response procedures for a simulated ransomware attack.

**Incident Timeline:**

| Time | Activity | Action Taken |
|:---:|:---|:---|
| 08:14 | SIEM alert: Ransomware IOC detected | Incident created, team notified |
| 08:17 | Affected host identified: WORKSTATION-07 | Host isolated from network |
| 08:25 | Initial analysis: WannaCry variant | Scope assessment initiated |
| 08:45 | 3 additional hosts identified in scope | Extended isolation applied |
| 09:00 | Evidence collected: memory dump, disk image | Chain of custody maintained |
| 10:30 | Eradication: Malware removed, patches applied | Systems rebuilt from clean image |
| 14:00 | Systems restored from verified backup | Business restored |
| 16:00 | Post-incident report drafted | Lessons learned documented |

**NIST SP 800-61 Phases Applied:**
1. ✅ Preparation — IR plan, tools, contacts ready
2. ✅ Detection & Analysis — SIEM alert, investigation
3. ✅ Containment — Network isolation, evidence preservation
4. ✅ Eradication — Malware removal, root cause fix
5. ✅ Recovery — System restoration, monitoring
6. ✅ Post-Incident — Report, lessons learned

---

## 🟡 Domain 5: Security Program Management (20%)

### Lab 5.1 — Risk Assessment

**Objective:** Perform a formal risk assessment using NIST SP 800-30.

**Risk Assessment Process:**
1. System Characterization — Identified critical assets and data flows
2. Threat Identification — Enumerated threat actors and attack vectors
3. Vulnerability Identification — Nessus scan + manual assessment
4. Control Analysis — Reviewed existing controls effectiveness
5. Likelihood Determination — Analyzed threat probability
6. Impact Analysis — Business impact assessment
7. Risk Determination — Risk = Likelihood × Impact matrix
8. Control Recommendations — Identified risk treatment options
9. Results Documentation — Formal risk report

**Risk Matrix Results:**

| Asset | Threat | Likelihood | Impact | Risk Score | Priority |
|:---|:---|:---:|:---:|:---:|:---:|
| Customer DB | SQL Injection | High | Critical | 15 | P1-Critical |
| Email Server | Phishing | High | High | 12 | P1-High |
| VPN Gateway | Brute Force | Medium | High | 9 | P2-High |
| Web Server | XSS/CSRF | Medium | Medium | 6 | P2-Medium |

### Lab 5.2 — Security Policy Development

**Policies Developed:**

1. **Acceptable Use Policy (AUP)** — Defines authorized use of organizational IT resources
2. **Password Policy** — Minimum 12 chars, complexity, 90-day rotation, MFA required
3. **Remote Work Security Policy** — VPN required, endpoint security, clean desk
4. **Data Classification Policy** — Public, Internal, Confidential, Restricted tiers
5. **Incident Response Policy** — RACI chart, escalation procedures, notification requirements

---

## 🗂️ Repository Structure

```
SecurityPlus-Labs/
├── README.md
├── domain-1-security-concepts/
│   ├── Lab-1.1-Cryptography.md
│   ├── Lab-1.2-PKI-Certificates.md
│   └── Lab-1.3-Authentication-Protocols.md
├── domain-2-threats-vulns/
│   ├── Lab-2.1-Nmap-Reconnaissance.md
│   ├── Lab-2.2-Phishing-Analysis.md
│   ├── Lab-2.3-Vulnerability-Assessment.md
│   └── Lab-2.4-Malware-Analysis.md
├── domain-3-architecture/
│   ├── Lab-3.1-Network-Segmentation.md
│   ├── Lab-3.2-Cloud-Security-AWS.md
│   └── Lab-3.3-Zero-Trust-Implementation.md
├── domain-4-operations/
│   ├── Lab-4.1-SIEM-Splunk.md
│   ├── Lab-4.2-Incident-Response.md
│   └── Lab-4.3-Digital-Forensics.md
├── domain-5-management/
│   ├── Lab-5.1-Risk-Assessment.md
│   ├── Lab-5.2-Policy-Development.md
│   └── Lab-5.3-Compliance-Frameworks.md
└── scripts/
    ├── nmap-scan.sh
    ├── log-analysis.py
    └── splunk-queries.spl
```

---

<div align="center">

**Deborah Chidimma Chukwuezi** | CompTIA Security+ Certified
[![GitHub](https://img.shields.io/badge/Portfolio-MMA19801712-181717?style=flat-square&logo=github)](https://github.com/MMA19801712)
[![Email](https://img.shields.io/badge/Email-debchukwuezi%40gmail.com-EA4335?style=flat-square&logo=gmail)](mailto:debchukwuezi@gmail.com)

</div>
