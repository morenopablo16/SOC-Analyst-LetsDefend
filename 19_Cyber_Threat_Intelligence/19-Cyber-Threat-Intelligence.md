# 19 — Cyber Threat Intelligence (CTI) 🕵️

## Learning Objectives 🎯

- Understand CTI lifecycle and types
- Determine organizational attack surface
- Gather threat intelligence from multiple sources
- Integrate CTI with SOC tools (SIEM, SOAR, EDR, Firewall)

---

## What is CTI? 🎯

**Definition**: Cyber security discipline that processes data from multiple sources to produce actionable intelligence against cyber attacks.

**Goal**: Understand attacker TTPs (Techniques, Tactics, Procedures) to minimize damage.

**Process**: Collect data (IOCs, etc.) → Process → Create information → Match with organization → Produce intelligence

---

## CTI Lifecycle 🔄

### 1. Planning and Direction

**Purpose**: Define intelligence requirements and consumers.

**Key Questions**:

| Question | Intelligence Impact |
|----------|---------------------|
| **Have SOC team?** | YES = technical details<br>NO = executive summaries |
| **Attack history?** | High attacks = frequent updates needed |
| **Target: Org or individuals?** | Org = EASM focus<br>Individuals = DRP focus |
| **Industry-wide attacks?** | YES = need industry-based intelligence |

---

### 2. Information Gathering

**Sources**:

| Source Type | Examples |
|-------------|----------|
| **Dark Web** | Hacker forums, ransomware blogs, black markets |
| **Chatters** | Telegram, ICQ, IRC, Discord |
| **Code Repos** | GitHub, GitLab, Bitbucket |
| **Public Tools** | Shodan, VirusTotal, Alienvault, Malwarebazaar |
| **Internal** | SIEM, IDS/IPS, Firewalls, Honeypots |
| **File Sharing** | Anonfiles, Mediafire, WeTransfer |
| **Public Buckets** | Amazon S3, Azure Blob, Google Cloud |

---

### 3. Processing

**Purpose**: Filter data, remove false positives, apply rules, correlate.

**Output**: Clean information ready for analysis.

---

### 4. Analysis and Production

**Purpose**: Interpret information → Produce consumable intelligence.

**Output**: Reports tailored to consumer (technical team vs executives).

---

### 5. Dissemination and Feedback

**Purpose**: Deliver intelligence via appropriate channels, collect feedback to improve.

**Example**: If `blogspot.com` marked malicious (not just `letsdefend.blogspot.com`), causes false positives → Feedback needed to refine.

---

## Types of CTI 📊

| Type | Consumer | Purpose | Example |
|------|----------|---------|---------|
| **Technical** | SOC Analyst, IR | IOC-based rulesets, block lists | Malicious IPs, hashes, phishing domains |
| **Tactical** | SOC Manager | Understand attacker TTPs | "What vulnerabilities does attacker X use?" |
| **Operational** | Security Manager, Threat Hunter | Focused TTP hunting | Deep dive on specific attacker/campaign |
| **Strategic** | Executives (C-level) | Long-term planning | Budgeting, product purchases, risk assessment |

---

## Attack Surface Determination 🎯

**Purpose**: Know all organizational assets to defend them.

**Concept**: Extended Threat Intelligence (XTI) = CTI + Attack Surface

### Attack Surface Components

| Asset Type | How to Find | Tools |
|------------|-------------|-------|
| **Related Domains** | Reverse whois, co-hosted, redirects | host.io, whoxy.com, viewdns.info |
| **Subdomains** | Passive DNS, brute force | SecurityTrails, Sublist3r, Aquatone, Assetfinder |
| **Websites** | HTTP/HTTPS requests to domains | httpx, httprobe |
| **Login Pages** | Parse HTML for "login", forms, "username/password" | Custom Python scripts (requests, BeautifulSoup) |
| **Technologies** | CMS detection, libraries, frameworks | Wappalyzer, Whatruns, BuiltWith, Whatcms |
| **IP Addresses** | Resolve domains, A records | dig, nslookup, DNS tools |
| **IP Blocks** | Whois, BGP lookups | Shodan (`org:"Company"`), bgp.he.net |
| **DNS Records** | A, MX, NS, TXT records | dig, dnslytics.com |
| **Employee Emails** | LinkedIn scraping | SalesQL, RocketReach, Apollo, ContactOut (Chrome extensions) |
| **Network Apps/OS** | Port scanning, service detection | Shodan, Nmap, passive scanning |
| **BIN Numbers** (banks) | Public BIN databases | bincheck.io, freebinchecker.com |
| **Swift Codes** (banks) | Public Swift databases | wise.com, bank.codes, theswiftcodes.com |
| **SSL Certificates** | Certificate transparency logs | Censys, crt.sh |

---

## Threat Intelligence Sources 🔍

### Shodan

**Purpose**: Search engine for internet-connected systems.

**Use Cases**: Find open ports, vulnerable systems by country/org/service.

**Alternatives**: BinaryEdge, Zoomeye, Censys

**Example**: `org:"Abanca" port:21` → Find all Abanca FTP servers

---

### IOC Providers

**Purpose**: Collect malicious IPs, domains, hashes, C2s.

**Sources**: Alienvault, Malwarebazaar, Abuse.ch, Malshare, Anyrun, VirusTotal, Hybrid-Analysis, Urlscan, Phishunt, Spamhaus

**Rule**: Collect from as many sources as possible, apply whitelist to reduce false positives.

---

### Hacker Forums

**Purpose**: Early warning - attackers share plans/targets before campaigns.

**Intel**: Attack direction, targets, methods, attacker identity.

**Example**: Sales of hacked system access → Remediation before worse actors buy.

---

### Ransomware Blogs

**Purpose**: Track ransomware victims and group activities (especially post-Covid 2020+).

**Popular Groups**: Lockbit, Conti, Revil, Hive, Babuk

**Access**: Tor Browser (`.onion` sites)

**Example List**: `http://ransomwr3tsydeii4q43vazm7wofla5ujdajquitomtd47cxjtfgwyyd.onion/`

---

### Black Markets

**Purpose**: Detect stolen credentials, RDP access, credit cards matching your attack surface.

**Sold**: Credit cards, stealer logs, RDP access, prepaid accounts

**Limitation**: No API - requires custom scraping scripts.

---

### Chatters (Telegram, ICQ, IRC, Discord)

**Purpose**: Monitor threat actor communications, sensitive data sharing, attack prep.

**Intel**: Credit card sales, account sales, company access sales.

---

### Code Repositories (GitHub, GitLab, Bitbucket)

**Purpose**: Find leaked credentials, API keys, database passwords, configs.

**Method**: Dork searches (e.g., `"password" "abanca.com"`)

**Risk**: Attackers find and exploit same leaks.

---

### File Share Sites

**Purpose**: Detect breached data shared anonymously.

**Sites**: Anonfiles, Mediafire, Uploadfiles, WeTransfer, File.io

**Method**: Google Dorks on indexed files (less costly than brute force).

---

### Public Buckets

**Purpose**: Find misconfigured cloud storage with sensitive data.

**Services**: Amazon S3, Azure Blob, Google Cloud Storage

**Method**: Brute force bucket names with org wordlist.

---

### Honeypots

**Purpose**: Trap attackers, collect attacker IPs/tactics.

**Examples**: Kippo, Cowrie, Glastopf, Nodepot, ElasticHoney

**Output**: Attacker IOCs for use in own systems.

---

### SIEM/IDS/IPS/Firewalls

**Purpose**: Internal attack logs = intelligence source.

**Intel**: Blocked attacker IPs, malicious file hashes, attack patterns.

**Value**: Organization-specific threat landscape.

---

## Data Interpretation 🧠

**Challenge**: Complex, large data from multiple sources.

**Solution**: Clean, classify, label, map to attack surface.

### Data Cleaning Process

1. **Whitelist** legitimate data (Microsoft app hashes, known-good IPs)
2. **Apply filters** to remove false positives
3. **Classify** by type (IP, domain, hash, email)
4. **Label** by severity/category
5. **Map** to attack surface (match collected data with org assets)

**Example**: Microsoft app hash in intelligence → Mark all org instances malicious → Business disruption ❌

---

## Using Threat Intelligence 🛡️

### 1. External Attack Surface Management (EASM)

**Purpose**: Manage outward-facing assets, detect vulnerabilities.

#### EASM Alerts

| Alert | Action |
|-------|--------|
| **New Asset Detected** | Verify belongs to org, authorized creation |
| **Domain/DNS Info Changed** | Compare old vs new, verify authorized change |
| **DNS Zone Transfer** | Check DNS records, verify need |
| **Internal IP Exposed** | Contact DNS POC, investigate misconfiguration |
| **Critical Open Port** | Close/filter unused, update services if needed |
| **SMTP Open Relay** | Contact mail server POC, fix config |
| **SPF/DMARC Missing** | Configure email security records |
| **SSL Expired/Revoked** | Renew immediately (data in clear text!) |
| **Suspicious Redirect** | Check destination, potential breach |
| **Subdomain Takeover** | Contact DNS team, immediate remediation |
| **Website Status Changed** | Investigate downtime/error, fix root cause |
| **Vulnerability Detected** | Apply patches/fixes immediately |

---

### 2. Digital Risk Protection (DRP)

**Purpose**: Protect brand reputation, executives, detect fraud.

#### DRP Alerts

| Alert | Action |
|-------|--------|
| **Phishing Domain** | Investigate in safe environment, contact registrar for takedown |
| **Rogue Mobile App** | Analyze APK, takedown if malicious |
| **IP Reputation Loss** | Investigate blacklist reason, remove from torrent/IOC feeds |
| **Fake Social Media** | Review account, request closure if fraudulent |
| **Botnet (Black Market)** | Reset user password, isolate/investigate if employee |
| **Deep/Dark Web Mention** | Analyze threat, tighten security before attack |
| **IM Platform Threat** | Analyze conversation context, take preventive action |
| **Stolen Credit Card** | Inform fraud team, cancel card |
| **Code Repo Leak** | Delete if own repo, request takedown if external |
| **Malware w/ Company Info** | Analyze file, investigate intent |
| **Employee/VIP Credential** | Immediate password reset |

---

### 3. Cyber Threat Intelligence (CTI)

**Purpose**: General cyber world awareness, malicious campaigns, offensive IPs.

**Integration**: Feed into SIEM, SOAR, EDR, Firewall for automated protection.

**Limitation**: CTI alone insufficient - must combine with corporate feeds (EASM + DRP).

---

## CTI + SOC Integration 🔗

### Integration Benefits

| SOC Product | CTI Integration Benefit |
|-------------|-------------------------|
| **SIEM** | Reduce false positives in collected logs |
| **SOAR** | Better quality outputs (less noise from SIEM) |
| **EDR** | Detect endpoint risks from user web traffic |
| **Firewall** | Block malicious IPs before alert occurs |

**Example**: Firewall + CTI → Block malicious IP before it reaches internal systems → No SOC alert needed (threat eliminated at perimeter).

---

## Quick Reference 📋

| CTI Component | Key Point |
|---------------|-----------|
| **Lifecycle** | Plan → Gather → Process → Analyze → Disseminate + Feedback |
| **Types** | Technical (SOC), Tactical (Managers), Operational (Hunters), Strategic (Execs) |
| **XTI** | CTI + Attack Surface (EASM + DRP + CTI) |
| **EASM** | Manage external assets, vulnerability detection |
| **DRP** | Brand protection, fraud detection, executive protection |
| **Sources** | Wide range: Dark web, Shodan, GitHub, honeypots, internal logs |
| **Integration** | Feed CTI into SIEM/SOAR/EDR/Firewall for automation |

---

## SOC Focus Points 🎯

1. ✅ **Wide Sources**: More sources = better intelligence (reduce blindspots)
2. ✅ **Whitelist Critical**: Prevent false positives (Microsoft apps, known-good IPs)
3. ✅ **Map Attack Surface**: Know all org assets before defending
4. ✅ **Monitor Attack Surface**: Continuous monitoring for changes/vulnerabilities
5. ✅ **EASM Priority**: Critical ports, SSL expiry, subdomain takeover = high risk
6. ✅ **DRP Priority**: Phishing domains, credential leaks, exec protection = brand/fraud risk
7. ✅ **Integrate with SOC**: CTI feeds into SIEM/SOAR/EDR/Firewall = automated defense
8. ✅ **Ransomware Blogs**: Post-2020 critical source for victim tracking
9. ✅ **Dark Web Monitoring**: Early warning of attacks (forums/markets)
10. ✅ **Feedback Loop**: Improve intelligence quality based on false positive reports

---
