# Threat Intelligence & Incident Analysis: APT10 Attack on Biotech Research

## 📋 Project Overview
This repository contains a comprehensive cybersecurity incident analysis report investigating a targeted advanced persistent threat (APT) campaign against a biotechnology firm specializing in cancer research. The attack has been attributed to **APT10 (Stone Panda)**, a state-sponsored threat group known for intellectual property theft and trade secret exfiltration. 

The investigation documents the multi-stage attack lifecycle, correlates adversary behaviors with the **MITRE ATT&CK framework**, and outlines actionable mitigation and hardening strategies.

---

## 🔬 Attack Lifecycle & TTP Mapping

| Stage | Attack Step Description | Indicators of Compromise (IoCs) | MITRE ATT&CK Technique |
| :--- | :--- | :--- | :--- |
| **1. Initial Access** | Spearphishing email delivering a malicious Excel document with an embedded macro payload. | `financial_report_2024.xlsm` | **T1566.001** (Phishing: Spearphishing Attachment) |
| **2. Execution** | Execution of the macro script, triggering a PowerShell command to download and run a Remote Access Tool (RAT). | `hxxp://192.168.50.10/ratscript.ps1` | **T1059.001** (Command and Scripting Interpreter: PowerShell) |
| **3. Credential Access** | Harvesting administrator credentials from system memory using a credential dumping utility. | `C:\Windows\Temp\dump.dat` | **T1003** (OS Credential Dumping) |
| **4. Lateral Movement** | Leveraging compromised administrator credentials to gain unauthorized access to secure research servers via RDP. | Source: `192.168.100.20`<br>Destination: `10.30.40.50` | **T1021.001** (Remote Services: Remote Desktop Protocol) |
| **5. Exfiltration** | Compressing sensitive research data into a ZIP archive and exfiltrating it via encrypted C2 channels. | Destination Domain: `biotechsecure-update.com`<br>Port: `8443` (HTTPS) | **T1560** (Archive Collected Data)<br>**T1041** (Exfiltration Over C2 Channel) |

---

## 🛠️ Key Findings & Analysis

- **Entry Vector:** The campaign weaponized trusted communication vectors (spearphishing) targeting organizational personnel with tailored financial pretexts.
- **Payload Delivery:** PowerShell execution was abused "living off the land" to download secondary staging payloads directly into memory or disk staging directories.
- **Persistence & Movement:** Credential harvesting enabled seamless escalation and lateral traversal across internal secure subnets, bypassing perimeter defenses.
- **Data Theft:** Exfiltration was disguised via standard encrypted outbound ports (HTTPS/8443) to blend with legitimate administrative or update traffic.

---

## 🛡️ Mitigation & Remediation Recommendations

1. **Email Security Hardening:** Implement advanced email filtering, sandbox attachments, and enforce strict group policies blocking macros in Microsoft Office files downloaded from the internet.
2. **Endpoint Detection & Response (EDR):** Monitor and restrict suspicious PowerShell execution policies (e.g., restricted language mode) and alert on known credential dumping utility behaviors and memory access patterns.
3. **Network Segmentation & Access Control:** Enforce strict firewall rules between administrative subnets and research data servers, requiring Multi-Factor Authentication (MFA) for all RDP connections.
4. **Traffic Monitoring:** Deploy deep packet inspection (DPI) and DNS monitoring to detect anomalous outbound connections to suspicious domains (`biotechsecure-update.com`) or unauthorized ports.

---

## 📁 Repository Structure
