# Module 03 — MITRE ATT&CK Framework

## 🎯 Learning Objectives
- Understand what MITRE and the MITRE ATT&CK Framework are.
- Know the purpose and importance of the ATT&CK framework for SOC analysts.
- Learn the ATT&CK matrices, tactics, techniques/sub-techniques, mitigations, groups, and software components covered in the module.

## 🔑 Introduction
Cyber attackers have evolved alongside increasingly complex digital systems. To better understand and detect modern attacks, the MITRE ATT&CK framework models attack steps and details in a structured way. This module provides an entry-level theoretical overview intended to give SOC candidates a thorough understanding of MITRE ATT&CK.

## What is MITRE?
- MITRE (founded 1958, USA) is an organization that produces innovative solutions and serves as an independent adviser to advance national security and the public interest. Areas of work include Cybersecurity, Aerospace, AI/ML, Aviation & Transportation, Defense & Intelligence, Government Innovation, Health, Homeland Security, and Telecom.

## What is the MITRE ATT&CK Framework?
- ATT&CK (Adversarial Tactics, Techniques, and Common Knowledge) is a knowledge database framework introduced by MITRE in 2013 and continuously developed. It allows systematic analysis of cyber attacks by dividing attacks into stages and detailing methods used at each stage.

## Why ATT&CK is important for SOC Analysts
- ATT&CK helps SOC analysts map attacker actions to specific stages, develop detection and mitigation techniques, write in-depth reports, and archive attack details for future use. The framework provides a roadmap for anticipating and researching potential attacks.

## MITRE ATT&CK Matrix
- The ATT&CK Matrix is a visualization to classify and view attacker methods. Matrices can be customized and are used to visualize attacker behavior details.

### Types of Matrices
- Enterprise Matrix — covers a wide range of systems and is used mainly for large organizations. It includes 7 sub-matrices: PRE, Windows, macOS, Linux, Cloud, Network, Containers. (Enterprise Matrix link: https://attack.mitre.org/matrices/enterprise/)
- Mobile Matrix — focused on mobile devices (Android, iOS). (Mobile Matrix link: https://attack.mitre.org/matrices/mobile/)
- ICS Matrix — for industrial control systems security. (ICS Matrix link: https://attack.mitre.org/matrices/ics/)

## Tactics
- A tactic expresses the adversary's goal and groups attacker behaviors; tactics appear in the top row of the matrix. Tactics are generally general statements describing attacker intent.

### Enterprise Tactics (14)
- Reconnaissance; Resource Development; Initial Access; Execution; Persistence; Privilege Escalation; Defense Evasion; Credential Access; Discovery; Lateral Movement; Collection; Command and Control; Exfiltration; Impact.

### Mobile Tactics (14)
- Initial Access; Execution; Persistence; Privilege Escalation; Defense Evasion; Credential Access; Discovery; Lateral Movement; Collection; Command and Control; Exfiltration; Impact; Network Effects; Remote Service Effects.

### ICS Tactics (12)
- Initial Access; Execution; Persistence; Privilege Escalation; Evasion; Discovery; Lateral Movement; Collection; Command and Control; Inhibit Response Function; Impair Process Control; Impact.

## Techniques and Sub-Techniques
- Techniques and sub-techniques describe the specific methods attackers use to achieve tactical goals. Each technique/sub-technique belongs to a tactic. Some techniques have sub-techniques (indicated in the matrix by gray areas next to technique names).

### Counts (as of module preparation)
- Enterprise: Techniques 193, Sub-techniques 401 (see: https://attack.mitre.org/techniques/enterprise/)
- Mobile: Techniques 66, Sub-techniques 41 (see: https://attack.mitre.org/techniques/mobile/)
- ICS: Techniques 79, Sub-techniques 0 (see: https://attack.mitre.org/techniques/ics/)

## Procedures
- A procedure is an example of how a technique or sub-technique was implemented in practice (which tool/software was used). Procedures provide practical usage examples for techniques (e.g., OS Credential Dumping procedures).

## Mitigations
- Mitigations are measures and actions to respond to techniques in the matrix. Each mitigation has an ID, name, and description.

### Mitigation counts (as of module preparation)
- Enterprise Mitigations: 43 (https://attack.mitre.org/mitigations/enterprise/)
- Mobile Mitigations: 11 (https://attack.mitre.org/mitigations/mobile/)
- ICS Mitigations: 51 (https://attack.mitre.org/mitigations/ics/)

## Groups
- APT Groups are organized actors (sometimes state-supported) conducting targeted attacks. MITRE ATT&CK collects information about APTs, their techniques, and targets, which helps map group behavior.
- Groups count: 135 (see: https://attack.mitre.org/groups/). Each group entry includes a unique Group ID, Name, Description, and a Techniques column listing tools/techniques used by the group (example: Lazarus Group).

## Software
- The ATT&CK Framework catalogs software used by adversaries. Each software entry has a unique ID, name, and description and links to techniques and groups that used it (example: "3PARA RAT").
- Software count (at module prep): 718 (see: https://attack.mitre.org/software/)

