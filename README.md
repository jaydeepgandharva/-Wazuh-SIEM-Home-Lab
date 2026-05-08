# 🛡️ Wazuh SIEM Home Lab

A hands-on cybersecurity home lab simulating enterprise SOC (Security Operations Center) operations using **Wazuh v4.14.4** as the SIEM platform. This project covers vulnerability assessment, CIS compliance benchmarking, real-time threat detection, and a live SSH brute-force attack simulation from Kali Linux.

---

## 📸 Lab Highlights

| Metric | Result |
|---|---|
| 🔴 Total CVEs Found | 791 (120 Critical) |
| 📋 CIS Benchmark Score | 26% — 346 failures documented |
| 🔥 Brute-Force Detections | **54,403 authentication failures** caught live |
| 🎯 MITRE ATT&CK Mapped | T1531 — Impact (Account Access Removal) |
| 📊 Compliance Frameworks Triggered | PCI DSS · HIPAA · GDPR · NIST 800-53 |

---

## 🏗️ Lab Architecture

```
Windows PC (winJD) — 192.168.1.234
        ↓ Wazuh Agent (port 1514)
Wazuh Server VM — 192.168.1.160
(Amazon Linux 2023 · VirtualBox Bridged Adapter)
        ↓ Dashboard at https://192.168.1.160
Browser — Wazuh Dashboard

Kali Linux VM — 192.168.1.11
        → SSH Brute Force Attack → 192.168.1.234:22
```

### Tools & Technologies

| Tool | Version | Purpose |
|---|---|---|
| Wazuh SIEM | v4.14.4 | Security monitoring, alerting, compliance |
| Windows 11 Pro | 10.0.26200 | Monitored endpoint (agent) |
| Kali Linux | Latest | Attack simulation platform |
| Hydra | v9.6 | SSH brute-force tool |
| VirtualBox | 7.0.10 | VM hypervisor |
| rockyou.txt | 14.3M passwords | Brute-force wordlist |

---

## 📁 Project Structure

```
wazuh-siem-home-lab/
├── README.md
├── docs/
│   ├── 01_Lab_Overview.md              ← Architecture, setup, CIS & vulnerability results
│   ├── 02_SSH_Brute_Force_Attack.md    ← Full attack documentation & Wazuh detections
│   ├── 03_Errors_and_Troubleshooting.md ← 5 errors with root cause & fixes
│   └── 04_Next_Steps.md                ← Roadmap: GitHub, CIS fixes, certs
└── screenshots/
    ├── 01_wazuh_dashboard_agent_active.png
    ├── 02_vulnerability_scan_791_cves.png
    ├── 03_cis_benchmark_26percent.png
    ├── 04_brute_force_spike_14271_failures.png
    └── 05_alert_detail_54403_firedtimes.png
```

---

## 🔬 What Was Done

### 1. 🖥️ SIEM Deployment
- Deployed Wazuh OVA on VirtualBox with Bridged Adapter networking
- Installed Wazuh agent on Windows 11 PC via PowerShell
- Confirmed agent active with real-time log shipping to server

### 2. 🔍 Vulnerability Assessment
- Wazuh scanned all installed packages against the National Vulnerability Database (NVD)
- **791 total CVEs** identified — 120 Critical, 334 High, 321 Medium
- Top finding: Mozilla Firefox with **593 vulnerabilities**
- Other vulnerable packages: Python 3.8.6 (EOL), Node.js, MySQL 8.1, VirtualBox 7.0.10

### 3. 📋 CIS Benchmark Compliance
- Ran **CIS Microsoft Windows 11 Enterprise Benchmark v3.0.0** (482 controls)
- Baseline score: **26%** — 346 failed controls across 16 categories
- Top failure categories: Encryption (30), Password Policy (27), Audit Logging (23), Network Security (20)
- Key risks: LLMNR enabled (Responder attack vector), SMB signing not enforced, BitLocker not configured

### 4. 🔴 SSH Brute-Force Attack Simulation

**Attack Setup:**
- Installed OpenSSH Server on Windows 11 via PowerShell
- Opened port 22 via Windows Firewall rule
- Confirmed SSH listening: `TCP 0.0.0.0:22 LISTENING`

**Attack Execution (Kali Linux):**
```bash
hydra -l "Jaydeep" -P /usr/share/wordlists/rockyou.txt ssh://192.168.1.234 -V -t 4
```

**Wazuh Detection Results:**
- **14,271 authentication failure events** in 10-minute window
- Rule 60122 fired **54,403 times** total
- Event ID 4625 captured on every single Hydra attempt
- Alert spike visible on dashboard at exact moment attack launched

**MITRE ATT&CK:** `T1531 — Account Access Removal (Impact)`

**Compliance Violations Auto-Flagged:**
| Framework | Controls |
|---|---|
| PCI DSS | 10.2.4, 10.2.5 |
| HIPAA | 164.312.b |
| GDPR | IV_35.7.d, IV_32.2 |
| NIST 800-53 | AU.14, AC.7 |

### 5. 🔒 Post-Lab Cleanup
- SSH service stopped and disabled
- Firewall rules removed
- Port 22 verified closed

---

## ⚠️ Troubleshooting & Errors

Five real errors were encountered and resolved during the lab. Full details in [`docs/03_Errors_and_Troubleshooting.md`](docs/03_Errors_and_Troubleshooting.md).

| Error | Root Cause | Fix |
|---|---|---|
| `Cannot find service 'sshd'` | SSH not installed yet | Install via `Add-WindowsCapability` first |
| OpenSSH stuck at `Staged` | Install didn't fully complete | Re-run install command |
| Wazuh API Offline after reboot | Services don't auto-start | `systemctl restart` all 3 services |
| `/etc/netplan` not found | VM runs Amazon Linux, not Ubuntu | Identified OS with `cat /etc/os-release` |
| Firewall rule not found | Rule created with different name | Filter by pattern with `-like "*SSH*"` |

---

## 🗺️ Roadmap

- [x] Wazuh SIEM deployed
- [x] Windows agent active
- [x] Vulnerability scan complete
- [x] CIS benchmark baseline established
- [x] SSH brute-force simulation + detection
- [ ] CIS score improved from 26% → 50%+
- [ ] Active Response configured (auto-block attacker IPs)
- [ ] Kali Linux agent added (bilateral monitoring)
- [ ] Suricata IDS integrated (network-level detection)
- [ ] GitHub portfolio complete

---

## 💼 Skills Demonstrated

`Wazuh SIEM` `Vulnerability Assessment` `CIS Benchmarks` `MITRE ATT&CK` `Penetration Testing`
`Kali Linux` `Hydra` `Windows Event Logs` `PowerShell` `Incident Response`
`PCI DSS` `HIPAA` `GDPR` `NIST 800-53` `VirtualBox Networking` `Amazon Linux`

---

## 📄 Documentation

| Document | Description |
|---|---|
| [01 — Lab Overview](docs/01_Lab_Overview.md) | Full setup guide, commands, CIS & vuln results |
| [02 — SSH Brute Force Attack](docs/02_SSH_Brute_Force_Attack.md) | Complete attack documentation |
| [03 — Errors & Troubleshooting](docs/03_Errors_and_Troubleshooting.md) | All errors with root cause and fix |
| [04 — Next Steps](docs/04_Next_Steps.md) | Roadmap for lab improvements |

---

## ⚠️ Disclaimer

This lab was conducted in a **controlled private home network** environment. The brute-force attack was performed exclusively against a personally owned machine. Never perform these techniques against systems you do not own or have explicit written permission to test.

---

*Built by Jaydeep Gandharva | Charlotte, NC | May 2026*
