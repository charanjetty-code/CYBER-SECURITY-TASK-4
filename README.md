# 🛡️ Task 4: Malware Analysis & System Hardening

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Kali%20Linux-blue?style=for-the-badge&logo=kali-linux" alt="Kali Linux" />
  <img src="https://img.shields.io/badge/Role-Cybersecurity%20Analyst%20Intern-red?style=for-the-badge" alt="Role" />
  <img src="https://img.shields.io/badge/Organization-ApexPlanet%20Software-orange?style=for-the-badge" alt="ApexPlanet" />
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge" alt="Status" />
</p>

---

## 👨‍💻 Intern Profile
* **Name:** Jetty Charan
* **Role:** Cybersecurity Analyst Intern
* **Organization:** ApexPlanet Software Pvt. Ltd.
* **Task Module:** Task 4 — Malware Analysis & System Hardening

---

## 📌 Executive Summary
This repository contains the technical documentation, execution logs, and verified screenshots for **Task 4** of the ApexPlanet Cybersecurity Internship. This task encompasses two critical sub-disciplines of cybersecurity:

1. **Malware Analysis (Static & Dynamic):** Examining binary characteristics, file hashes, embedded strings, and real-time execution tracing (`strace`).
2. **System Hardening (Defensive Security):** Implementing strict host firewall policies (`ufw`), applying security patch management, and auditing active background services to reduce the attack surface.

---

## 🗂️ Repository Architecture

```text
Task-4-System-Security/
│
├── README.md                           <-- Main Project Documentation
└── screenshots/
    ├── 01_phishing_analysis.png        <-- Email Header / Artifact Analysis
    ├── 02_static_analysis.png          <-- Static Analysis Output (hashes, file, strings)
    ├── 03_dynamic_analysis.png         <-- Dynamic Analysis Output (strace)
    ├── 04_firewall_config.png          <-- UFW Active Rules & Policy Output
    ├── 05_patch_management.png         <-- System Update & Upgrade Verification
    └── 06_services_audit.png           <-- Network Listening Ports Audit (ss -tulpn)

🔬 Phase 1: Malware Analysis
Overview
Static analysis inspects binary file properties without executing the executable file, providing initial threat intelligence safely.

# 1. Determine file type and ELF architecture
file /bin/ls

# 2. Generate cryptographic hashes for integrity and threat intelligence checks
md5sum /bin/ls
sha256sum /bin/ls

# 3. Extract printable character strings to inspect system dependencies
strings /bin/ls | head -n 20


Analysis Matrix:PropertyValue / ResultSecurity SignificanceFile Architecture64-bit ELF Executable, Dynamically LinkedConfirms execution target and library dependence.Integrity ChecksMD5 & SHA-256 HashesUsed for signature identification against known malware databases (e.g., VirusTotal).Embedded StringsLibrary imports, error messagesReveals potential hardcoded credentials, C2 domains, or APIs.

Overview
Dynamic analysis monitors the live runtime execution behavior of a process to capture file interactions, system calls, and process creation.

Command Executed:
Bash
# Trace open, openat, and execve system calls during execution
strace -e trace=open,openat,execve /bin/ls > /dev/null


Execution Findings:
Process Spawning (execve): Verified executable initiation in isolated user context.

File System Operations (openat): Captured system library queries (e.g., /etc/ld.so.cache, /usr/lib/x86_64-linux-gnu/).

Process Exit Code: Clean termination with status code 0, confirming expected non-malicious runtime execution.

Screenshot Reference: See screenshots/03_dynamic_analysis.png

🛡️ Phase 2: System Hardening
Overview
Configuring firewall rules minimizes unauthorized network access while ensuring vital system administration channels remain functional.

Commands Executed:
Bash
# 1. Set default security policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 2. Enable SSH administration port
sudo ufw allow 22/tcp

# 3. Enable firewall daemon & verify status
sudo ufw enable
sudo ufw status verbose
Active Firewall Policy:
Status: Active

Incoming Default: Deny (Blocks unsolicited inbound access)

Outgoing Default: Allow (Permits outbound user request traffic)

Allowed Port: 22/tcp (Encrypted SSH management interface)

Screenshot Reference: See screenshots/04_firewall_config.png

Overview
Regular patch management ensures operating system dependencies and installed packages are secured against known vulnerability exposures (CVEs).

Commands Executed:
Bash
# Update repository package index and install upgrades
sudo apt update && sudo apt upgrade -y

# Verify pending updates status
sudo apt-get --just-print upgrade
Verification Result:
Confirmed 0 upgraded, 0 newly installed, 0 to remove, establishing that all security updates are fully applied to the operating environment.

Screenshot Reference: See screenshots/05_patch_management.png

Overview
Auditing listening network sockets ensures that unnecessary daemon processes are not leaving exposed entry points for remote attackers.

Command Executed:
Bash
# Audit listening TCP/UDP network sockets and active processes
sudo ss -tulpn
Security Impact:
Auditing active listening sockets allows system administrators to identify and disable unapproved or redundant services (e.g., stopping unneeded HTTP/FTP daemons using systemctl stop <service>).

Screenshot Reference: See screenshots/06_services_audit.png
