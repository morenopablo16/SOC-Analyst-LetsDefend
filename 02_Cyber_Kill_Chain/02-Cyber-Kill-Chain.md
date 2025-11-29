# Module 02 — Introduction to the Cyber Kill Chain

## 🎯 Learning Objectives
- Understand what the Cyber Kill Chain is and its origin.
- Learn the seven stages of the Cyber Kill Chain and their sequence.
- Recognize attacker activities and recommended blue team/ SOC actions for each stage.

## 🔑 Overview

### What is Lockheed Martin?
- Lockheed Martin (est. 1995) is a security and aerospace corporation that researches, develops, designs, and produces advanced technological systems.

### What is the Cyber Kill Chain?
- The Cyber Kill Chain is a framework created by Lockheed Martin (2011) to model attacker behavior and the steps of a cyber attack. Attacks are modeled as seven sequential stages; failure at any stage prevents progression to the next.
- Official page: https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html

### Significance
- The model helps SOC analysts identify which stage an observed attacker action belongs to, evaluate severity, and find gaps in defenses so blue teams can prioritize mitigations.

## 🧭 Cyber Kill Chain — 7 Steps
1. Reconnaissance
2. Weaponization
3. Delivery
4. Exploitation
5. Installation
6. Command & Control (C2)
7. Actions on Objectives

Each step is described below with typical adversary actions and defender (blue team) measures mentioned in the module.

## 1 Reconnaissance
- Purpose: Attacker gathers information about the target to increase the attack surface.
- Techniques: Two subcategories — Passive Reconnaissance (collect info without interacting with the target; e.g., web archives) and Active Reconnaissance (interact with target to retrieve info such as server version from responses).

Adversary actions (examples):
- Obtain server/software version information
- Collect open-source information previously released about the target
- Harvest employee email addresses
- Gather internal/personal info via social networks
- Discover internet-connected devices and exposed servers
- Identify security vulnerabilities on internet-facing servers
- Enumerate organization's IP blocks and vendor relationships

Defender actions (examples):
- External pentests to detect information disclosure
- Use threat intelligence to learn of leaked org information
- Remove sensitive organizational documents from public internet
- Install and monitor perimeter security (firewalls) for internet-exposed areas
- Keep systems updated to reduce exploitability

## 2 Weaponization
- Purpose: Attacker uses reconnaissance data to prepare tools or build malware/exploits tailored to the target (e.g., phishing templates, exploit code).

Adversary actions (examples):
- Create malware
- Develop exploits
- Build malicious phishing content (email templates, malicious documents)
- Select the best tool for the planned attack

Defender actions (examples):
- Regularly check systems for known vulnerabilities
- Patch and apply security updates promptly
- Track known/new attack tools to detect their use against assets

## 3 Delivery
- Purpose: The attacker delivers the weapon to the victim — the first direct interaction with the victim (e.g., email, web uploads, USB).

Adversary actions (examples):
- Send malicious URLs via email or social media
- Attach malware to email
- Host malware on websites or upload directly to target servers
- Deliver via social media or physical media (USB)

Defender actions (examples):
- Treat URLs in emails cautiously and, where possible, open them in sandboxed environments
- Scan email attachments with antivirus solutions
- Use email security products and train users in secure handling of email content
- Monitor server access and logs continuously
- Use firewalls effectively and investigate anomalies

## 4 Exploitation
- Purpose: Attacker activates the delivered content so it runs on the victim system (exploits hardware/software vulnerabilities or runs malware). If exploitation fails, the attack halts.

Adversary actions (examples):
- Execute exploits targeting hardware or software vulnerabilities
- Run delivered malware

Defender actions (examples):
- Train users on safe file handling and suspicious indicators
- Continuously monitor asset security and detect anomalies
- Track published vulnerabilities and create monitoring rules for their exploitation
- Apply security updates promptly
- Monitor endpoints with EDR solutions
- Provide secure coding training and perform regular pentests and automated vulnerability scanning
- Enforce least privilege and proper authorization on assets

## 5 Installation
- Purpose: Attacker establishes persistence on the exploited system (install backdoors, web shells, scheduled tasks, services, or escalate privileges to maintain access).

Adversary actions (examples):
- Install malware or backdoors on the victim device
- Place web shells on web servers
- Add services, firewall rules, or scheduled tasks to ensure persistence
- Perform privilege escalation to gain highly authorized accounts

Defender actions (examples):
- Conduct threat hunting and assume compromise for detection
- Perform network security monitoring across assets
- Use EDR to detect configuration changes and malicious processes
- Restrict access to critical files and paths and monitor those accesses
- Allow admin privileges only when necessary and monitor their use
- Enforce executable signing to allow only signed binaries where applicable

## 6 Command & Control (C2)
- Purpose: Attacker configures and establishes communication between the victim and a C2 server so remote commands can be delivered and executed.

Adversary actions (examples):
- Configure C2 servers to communicate with victims
- Implement actions on victims to initiate C2 contact

Defender actions (examples):
- Monitor for possible C2 network traffic via network security monitoring
- Block known C2 IPs from threat intelligence via security products (firewalls)
- Detect presence of known C2 tools on systems

## 7 Actions on Objectives
- Purpose: Final stage where attacker carries out their intended goals (e.g., exfiltration, disruption, encryption) after earlier steps succeeded.

Adversary actions (examples):
- Encrypt files (ransomware)
- Exfiltrate critical data
- Delete or manipulate critical information
- Escalate privileges and move laterally to expand control
- Collect credentials and internal information

Defender actions (examples):
- Monitor systems continuously to detect malicious activity
- Take appropriate response actions once detection occurs
- Prevent data exfiltration (restrict/monitor external network access, use DLP)
- Restrict and monitor access to critical files/databases
- Detect unauthorized user access and anomalous network traffic

