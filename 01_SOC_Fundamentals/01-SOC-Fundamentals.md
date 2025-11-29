# Module 01 — Introduction to the SOC

## 🎯 Learning Objectives
- Understand the SOC structure and basic operation.
- Learn the main tools used in a SOC (SIEM, Log Management, EDR, SOAR, Threat Intelligence).
- Understand the role and responsibilities of a SOC analyst and other related roles.
- Identify common mistakes SOC analysts make.

## 🔑 Key Concepts

### What is a SOC?
- A Security Operation Center (SOC) is a facility where the security team continuously monitors and analyzes an organization's security. The primary purpose is to detect, analyze, and respond to cybersecurity incidents using people, processes, and technology.

### SOC Models
- **In-house SOC:** Built and maintained by the organization; requires budget for continuity.
- **Virtual SOC:** Team works remotely without a permanent facility.
- **Co-Managed SOC:** Internal staff working with an external MSSP; coordination is key.
- **Command SOC:** Oversees smaller SOCs across a large region (e.g., large telcos or defense agencies).

### People, Process, and Technology
- **People:** Trained personnel familiar with alerts and attack scenarios who can adapt and research new attack types.
- **Processes:** Align with standards (NIST, PCI, HIPAA) and require strict standardization to avoid omissions.
- **Technology:** Tools for testing, detection, prevention, and analysis; choose solutions that fit the team's needs and budget.

## 🧩 SOC Roles
- **SOC Analyst:** Level 1/2/3; classifies alerts, finds root causes, and advises on remediation.
- **Incident Responder:** Performs initial assessment of breaches and threat detection.
- **Threat Hunter:** Proactively searches for threats and vulnerabilities using manual and automated techniques to find APTs and sophisticated attacks.
- **Security Engineer:** Maintains SIEM and SOC product infrastructure and integrates systems (e.g., SIEM with SOAR).
- **SOC Manager:** Handles operations management such as budgeting, strategy, and staff coordination.

## 🛠️ SOC Analyst Responsibilities
- First investigator for suspected threats; escalates to supervisors when needed.
- Daily tasks: review SIEM alerts, determine if they are true threats or false positives, and use EDR, Log Management, and SOAR to support investigations.

## 🧠 Required Skills (from the module)
- **Operating Systems:** Know normal Windows/Linux behavior to identify anomalies.
- **Network:** Confirm whether devices are contacting suspicious IPs/URLs and search for potential data leaks on the network.
- **Malware Analysis:** Determine malware purpose, identify command & control (C2) addresses, and check whether devices communicate with them.

## 🔎 SIEM and the Analyst Relationship
- **What is SIEM?:** A solution that centralizes security information and event logging in real time; used to detect threats via rules and filtering.
- **Example alert:** 20 incorrect password attempts in 10 seconds — a rule exceeding a threshold triggers an alert.
- **Relationship:** Analysts typically investigate alerts; other teams are often responsible for rule development and correlation.
- **LetsDefend workflow:** View alerts in the Main Channel, click "Take Ownership" to move the alert to the Investigation Channel; alert details (hostname, IP, file hashes) are visible for investigation.

### Quick Tip
- False positives occur; a good analyst identifies them (for example, a rule that triggers on the keyword "union" may alert on legitimate Google searches) and provides feedback to the SIEM team to improve rules.

## 📚 Log Management
- **What is Log Management?:** A platform that centralizes logs (web, OS, firewall, proxy, EDR, etc.) so analysts can query all sources in one place.
- **Purpose:** Help analysts determine communication with an address (e.g., "letsdefend.io") and view details of that communication across devices.
- **Module example:** After an alert indicating data leak from LetsDefendHost to IP 122[.]194[.]229[.]59, search that IP in Log Management to find other devices that communicated with it.

## 🛡️ EDR (Endpoint Detection and Response)
- **Definition (from the text):** An integrated endpoint security solution combining continuous monitoring and collection of endpoint data with rules-based automated response and analysis.
- **Examples mentioned:** CarbonBlack, SentinelOne, FireEye HX.
- **Highlighted features:** Search for IOCs across hosts, view Browser History / Network Connections / Process List, connect to the host for live investigation, and use containment to isolate a device while still allowing communication only with the EDR center.
- **Module example:** Search for MD5 hash `ac596d282e2f9b1501d66fce5a451f00` in EDR to see if the file exists or executes on other hosts.

## 🔁 SOAR (Security Orchestration, Automation and Response)
- **What it is:** A platform that integrates security tools and automates tasks (e.g., automatically querying VirusTotal for a SIEM-detected source IP).
- **Benefits mentioned:** Saves time through workflows, centralizes tools, and provides playbooks that guide investigations and ensure team consistency.
- **Products listed:** Splunk Phantom, IBM Resilient, Logsign, Demisto.
- **LetsDefend mapping:** "Case Management" functions like SOAR; opening a case shows an automatically assigned playbook to investigate the associated SIEM alert.

## 🔗 Threat Intelligence Feed
- **Definition:** Third-party data (hashes, C2 domains/IPs, etc.) used to inform investigations.
- **Sources mentioned:** VirusTotal, Talos Intelligence.
- **Important points from the module:**
-  - If a search returns no data, do not assume the artifact is benign; perform static/dynamic file analysis.
-  - IP addresses can change ownership; an IP previously used for malicious activity may host benign content later.

## ⚠️ Common Mistakes Highlighted
- **Over-reliance on VirusTotal:** A negative or clean result on VirusTotal does not guarantee safety; use VT as supporting information only.
- **Hasty sandbox analysis:** Some malware detect sandboxes or delay execution; a 3–4 minute sandbox run may be insufficient.
- **Inadequate log analysis:** Always search Log Management for related communications across devices after an alert.
- **Overlooking VirusTotal dates:** Cached VT results may be outdated; prefer fresh queries when possible.

## ✅ Module Conclusion
- This module introduces SOC structure, roles, core tools (SIEM, Log Management, EDR, SOAR, Threat Intel), and basic best practices. It emphasizes completing hands-on exercises in "Monitoring", "Log Management", and "Endpoint Security" on LetsDefend.

---
