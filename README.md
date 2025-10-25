# Cybersecurity_task3_Prathee

# 🧩 Task 3: Perform a Basic Vulnerability Scan on Your PC

## 🎯 Objective
Use free vulnerability scanning tools to identify and analyze common security vulnerabilities present on a local machine.

---

## 🧰 Tools Used
- **Nessus Essentials** (Free vulnerability scanner)
  - Download: [https://www.tenable.com/products/nessus/nessus-essentials](https://www.tenable.com/products/nessus/nessus-essentials)

---

## ⚙️ Steps Performed

### 1️) Install Nessus Essentials
1. Registered with an email ID on the Nessus website to receive an **activation code**.
2. Downloaded and installed Nessus Essentials.
3. After installation, Nessus started as a **web service**.
4. Opened browser and visited:
   https://localhost:8834/
5. Entered the activation code to activate Nessus Essentials.
6. Waited for plugin configuration to complete (~15 minutes).

---

### 2️) Set Up Scan Target
- Configured the **scan target** as the **local machine IP** or **localhost** (127.0.0.1).

---

### 3️) Start a Full Vulnerability Scan
- Initiated a **Full System Scan** on the configured target.
- The scan duration was approximately **30–60 minutes**.

---

### 4️) Review the Report
- Examined the results for **vulnerabilities, their severity, and affected services**.
- Exported and documented the findings.

---

## 5) Summary of Vulnerabilities Found

| Severity | Vulnerability | Plugin ID | Description |
|-----------|----------------|------------|--------------|
| 🟠 Medium | SSL Certificate Cannot Be Trusted | 51192 | The SSL certificate presented by the service cannot be trusted (e.g., self-signed or expired). |
| 🟠 Medium | SMB Signing Not Required | 57608 | SMB signing is not enforced, which may allow man-in-the-middle (MITM) attacks. |
| 🔵 Info | Microsoft Windows NTLMSSP Authentication Request Remote Network Name Disclosure | 42410 | The system discloses its network name via NTLM authentication requests. |
| 🔵 Info | Microsoft Windows SMB NativeLanManager Remote System Information Disclosure | 10785 | SMB responses include information about the OS and system, which could help attackers profile the machine. |
| 🔵 Info | Additional DNS Hostnames | 46180 | The host is reachable via multiple DNS hostnames, which may assist in mapping your network. |

---

## 6) Fixes & Mitigations

### - SSL Certificate Cannot Be Trusted
**Issue:** SSL/TLS certificate is self-signed, expired, or untrusted.  
**Fix / Mitigation:**
- Replace with a certificate from a **trusted Certificate Authority (CA)**.
- Always use valid, up-to-date SSL certificates and enforce HTTPS.

**For Windows IIS:**
- Open **IIS Manager → Server Certificates → Create Domain Certificate**.

---

### - SMB Signing Not Required
**Issue:** SMB (Server Message Block) protocol allows unsigned communication → potential MITM attacks.  
**Fix / Mitigation (Windows):**
1. Open **Local Group Policy Editor** (`gpedit.msc`)
2. Navigate to:
   Computer Configuration → Windows Settings → Security Settings → Local Policies → Security Options
3. Enable:
- **Microsoft network client: Digitally sign communications (always)**
- **Microsoft network server: Digitally sign communications (always)**
4. Reboot the system.

**PowerShell Alternative:**
Set-SmbServerConfiguration -RequireSecuritySignature $true
Set-SmbClientConfiguration -RequireSecuritySignature $true

### - NTLMSSP Authentication Request Remote Network Name Disclosure
**Issue:** Windows may reveal its machine name during NTLM authentication.
**Fix / Mitigation:**
  Disable NTLM authentication if unnecessary:
  Local Security Policy → Local Policies → Security Options
  Set:
  Network security: Restrict NTLM: Outgoing NTLM traffic to remote servers → Deny all
  Prefer Kerberos or NTLMv2 for stronger authentication.
  Avoid connecting to untrusted networks.

### - SMB NativeLanManager Information Disclosure
**Issue:** SMB may reveal OS and system details to external requests.
**Fix / Mitigation:**
  Disable SMBv1:
  Disable-WindowsOptionalFeature -Online -FeatureName SMB1Protocol
  Enable SMB signing as shown above.
  Restrict SMB ports (TCP 139, 445) via the firewall for untrusted networks.

### - Additional DNS Hostnames
**Issue:** Multiple DNS aliases can help attackers map network structure.
**Fix / Mitigation:**
  Review and remove unnecessary DNS aliases (CNAMEs).
  Maintain consistent and updated DNS records.
  Use split-horizon DNS for internal vs external name resolution.

**Recommendations**
Keep your OS and all network services fully updated.
Disable unused protocols (Telnet, SMBv1, FTP, etc.).
Enable Windows Defender Firewall.
Regularly run Nessus or OpenVAS scans to ensure vulnerabilities remain fixed.Recommendations
