# 22 — IT Security Basis for Corporates 🏢

## Learning Objectives 🎯

- Evaluate corporate infrastructure protection level
- Implement inventory and asset management
- Establish backup strategies (3-2-1 and 3-2-1-1-0)
- Configure access control and patching policies
- Perform risk analysis and network segmentation

---

## 1. Inventory 📋

**Purpose**: Know all devices, applications, users, and security measures on network.

### Minimum Inventory Requirements

| Component | Details Needed |
|-----------|----------------|
| **Hardware** | Workstations, servers (models, specs) |
| **Software** | Installed applications + **exact versions** |
| **Last Report Date** | When inventory last updated |
| **EOL Status** | End-of-life equipment flagged |
| **Security Measures** | AV, EDR, patches applied |

---

### End-of-Life (EOL) Equipment

**Risk**: No security patches = vulnerable to exploits

**Action**: 
- Remove from network
- **Exception**: CISO risk acceptance (documented in inventory)
- Review regularly (each new vulnerability discovered)

---

### Secure Boot

**Purpose**: Computer boots only with manufacturer-approved software.

**Requirement**: Enable on **all compatible devices** (available 10+ years)

**Status Check**: Verify in inventory

---

### Software Management

#### Allowed Software

**Method**: GPO (Group Policy Object) or Intune

**Benefits**:
- Users get software without admin rights
- Reduces risk of downloading malicious programs
- Centralized software library

#### Forbidden Software

**Method**: AppLocker or Intune

**Block**:
- Unauthorized applications
- Versions with dangerous CVEs

**Alert**: Usage of forbidden app → Report to IT team

---

### Security Hardening

**Purpose**: Apply proven configuration standards.

**Levels**:
1. Station handover procedures (prerequisite checklist)
2. Audit of secure device configurations
3. Alerts on configuration modifications

**Tools**: Ansible, GPO, Intune

---

### Antivirus / EDR

**Requirement**: Deploy on **entire fleet** (prioritize critical devices)

**Critical**: Set up alert monitoring (AV/EDR without monitoring = blind protection)

---

## 2. Backups 🗄️

**Reality**: Backups = last line of defense against ransomware (not prevention)

**Risk**: Non-operational backup = business survival threat

---

### Rule 3-2-1 (Minimum Standard)

| Rule | Requirement | Purpose |
|------|-------------|---------|
| **3** | Three copies of data | Original + 2 backups (redundancy) |
| **2** | Two different media | Different datacenters, not same RAID |
| **1** | One offsite external | Outside building (fire protection) |

**Example**:
- Production data on server (1)
- Backup in same datacenter on different storage (2)
- Backup in different location/cloud (3) ✅

---

### Rule 3-2-1-1-0 (Critical Resources)

**Adds**:

| Rule | Requirement | Purpose |
|------|-------------|---------|
| **+1** | One offline copy | Not connected to network (ransomware protection) |
| **+0** | Zero restoration errors | Test backups regularly |

**Offline Copy**: Disconnected from network/IT infrastructure → Attacker can't reach it

**Zero Errors**: Test restores regularly (corrupted backup = useless backup)

---

### Minimum Storage Time

**Requirement**: 30 days of backup history

**Why**: Average time between intrusion and detection ≈ 30 days

**Use Case**: Restore to point before compromise

---

### Backup Tests

**Frequency**: Each server restored **at least once per year**

**Tracking**: Document test results in inventory

**Critical**: Backup without restore test = unknown reliability

---

## 3. Phishing Prevention 📧

**Reality**: Phishing = biggest initial attack vector (alongside vulnerable online servers)

### Antispam and Email Protection

**Questions**:
- ✅ All emails pass through spam filter?
- ✅ AV analyzes email attachments?
- ✅ Configuration reviewed and updated for new threats?

---

### Analysis Procedure

**Requirement**: Employees can contact someone to analyze suspicious emails

**Process**: Report suspicious email → IT/Security team analyzes → Feedback to user

---

### Phishing Drill

**Purpose**: Test employee awareness

**Note**: Often experienced as "attack on staff" → Approach with sensitivity

**Value**: Measure real-world readiness

---

## 4. Internet Browsing Protection 🌐

**Purpose**: Block unauthorized, suspicious, and malicious domains.

### DNS Filtering

**Block Categories**:
- +18 adult content
- **URL shorteners** (massively used for phishing) ⚠️
- Malicious domains
- Suspicious categories

**Action**: Monitor blocked sites employees try to visit (identifies training needs or legitimate business requirements)

---

### Centralized Browser Management

**Purpose**: Harden browser security settings

**Actions**:
- Reduce plug-in installation ability
- Disable automatic execution of content
- Enforce security policies (GPO/Intune)

---

## 5. Patching 🔧

**Goal**: Deploy patches to software and firmware as quickly as possible.

**Preference**: Enable automatic updates when possible

---

### SLA for Patching

**SLA Definition**: Service-Level Agreement = contract defining service standards and timelines

**Patching SLA Factors**:

| Factor | Impact on Priority |
|--------|-------------------|
| **CVSS Score** | High score = faster patching |
| **Internet-Facing** | Public servers = critical priority |
| **0-day Exploit** | Known exploitation = immediate patching |
| **Asset Criticality** | Business-critical = higher priority |

**Example**: Internet-facing server with CVSS 9.8 0-day = **immediate patching**

---

## 6. Access Control 🔐

**Principle**: Only authorized users get **least privileges necessary**

**Reality**: No magic bullet - adapt step by step

---

### Password & MFA

**Basic**: Password policy in "Default Domain Policy" (Windows)

**Better**: Stronger authentication mechanisms
- Biometrics
- One-time passwords (OTP)
- Application tokens

**Best**: Multi-factor Authentication (MFA)
- SMS or authenticator app
- **Priority**: Privileged users first → Extend to all users

---

### Zero Trust Approach

**Actions**:
1. Identify and disable unused accounts
2. Eliminate shared accounts
3. Remove unnecessary privileges
4. Enforce strong password policies

**Rule**: Trust no one by default, verify everyone

---

### Audit of Account Usage

**Monitor**:
- Access attempts outside business hours
- Access from unusual locations
- Anomalous user behavior

**Privilege Rule**: **Max 15%** of accounts with "domain administrator" privilege

**Why**: Over-privileged environment = larger attack surface

---

## 7. Risk Analysis 🎯

**Purpose**: Prioritize resource allocation and investments.

### Return to Service (RTO/RPO)

**Question**: Which resources should recover **first** after incident?

**Analysis Outputs**:
- Recovery priority order
- Estimated disruption impact
- Allowable downtime per service

**Use**: Budget planning, DR (Disaster Recovery) planning

---

### Review of Connected Networks

**Reality**: Every connection = potential risk

**Questions**:

| Connection Type | Key Questions |
|----------------|---------------|
| **Partner Networks** | Same security standards? Patching speed? Default certs? |
| **Other Entities** | Different security requirements? |
| **Online Solutions** | Configuration aligns with security needs? |

**Action**: Document risk acceptance for each connection

---

## 8. Network Security 🌐

**Purpose**: Monitor incoming/outgoing Internet traffic.

### PCAP (Packet Capture)

**Method**: Use SPAN ports on network equipment

**Benefits**:
- Detect abnormal behavior (protocol spikes, unusual destinations)
- Post-mortem analysis after compromise
- Baseline network activity

---

### Network Segmentation

**Purpose**: Divide network into smaller parts → Reduce attack surface

**Benefits**:
- Improved performance
- Limited attack range
- Contain breaches

**Tools**: VLAN, PVLAN

**Examples**:
- ❌ Accounting station accessing network equipment admin interfaces
- ✅ Remote office service on dedicated network only

---

### Review of Blocked Flows

**Purpose**: Identify why traffic gets blocked by segmentation policy

**Discoveries**:
- Compromised workstations
- Unknown applications (not in inventory)
- Misconfigured services

**Action**: Investigate each blocked flow, update inventory/rules

---

### Alert on Abnormal Network Usage

**Purpose**: Generate alerts for unusual patterns

**Security Gains**: Detect compromises

**Operational Gains**: Identify weak points (e.g., backup tools saturating network)

---

## Quick Reference 📋

| Area | Key Requirement | Critical Action |
|------|-----------------|-----------------|
| **Inventory** | Hardware, software, versions, EOL status | Remove EOL equipment |
| **Backups** | 3-2-1 minimum, 3-2-1-1-0 for critical | Test restores yearly |
| **Phishing** | Spam filter, AV on attachments | Analysis procedure + drills |
| **Browsing** | DNS filtering (block URL shorteners) | Monitor blocked sites |
| **Patching** | SLA based on CVSS/0-day/exposure | Prioritize internet-facing |
| **Access** | MFA, least privilege, max 15% domain admins | Audit anomalous access |
| **Risk** | Recovery priority, partner security review | Document risk acceptance |
| **Network** | PCAP, segmentation, blocked flow review | Alert on abnormal usage |

---

## SOC Focus Points 🎯

### Inventory

1. ✅ **EOL Equipment**: Remove or document CISO risk acceptance
2. ✅ **Secure Boot**: Enable on all compatible devices
3. ✅ **Software Control**: Allowed list (GPO) + Blocked list (AppLocker)
4. ✅ **EDR Deployment**: Monitor alerts (deployed EDR without monitoring = wasted investment)

### Backups

5. ✅ **3-2-1 Rule**: Minimum standard for all data
6. ✅ **3-2-1-1-0 Rule**: Critical resources (offline copy + zero errors)
7. ✅ **30-Day Retention**: Match average dwell time
8. ✅ **Test Annually**: Each server restored at least once/year

### Defense

9. ✅ **DNS Filtering**: Block URL shorteners (phishing vector)
10. ✅ **MFA Priority**: Privileged users first, then all users
11. ✅ **Domain Admin Limit**: Max 15% of accounts
12. ✅ **Patching SLA**: Internet-facing + high CVSS = immediate

### Network

13. ✅ **PCAP Collection**: SPAN ports for post-mortem analysis
14. ✅ **Segmentation**: Admin interfaces NOT accessible from user stations
15. ✅ **Blocked Flow Review**: Investigate every blocked connection
16. ✅ **Baseline Monitoring**: Alert on deviations from normal

---

## Corporate Security Checklist ✅

### Prevention

- [ ] Complete inventory (hardware, software, versions, EOL)
- [ ] Secure Boot enabled on all devices
- [ ] Allowed/forbidden software lists enforced
- [ ] Security hardening via GPO/Intune/Ansible
- [ ] AV/EDR deployed with alert monitoring
- [ ] Backups follow 3-2-1 (minimum) or 3-2-1-1-0 (critical)
- [ ] Backup restores tested annually
- [ ] Spam filter and email AV enabled
- [ ] DNS filtering (including URL shorteners)
- [ ] Centralized browser security management
- [ ] Patching SLA defined (by CVSS/exposure/0-day)
- [ ] MFA deployed (prioritize privileged users)
- [ ] Password policy enforced
- [ ] Max 15% domain admin accounts

### Detection

- [ ] User activity auditing (anomalous access)
- [ ] PCAP collection on network
- [ ] Network segmentation (VLAN/PVLAN)
- [ ] Blocked flow review process
- [ ] Alerts on abnormal network usage

### Response

- [ ] Recovery priority defined (RTO/RPO)
- [ ] Partner network security reviewed
- [ ] Risk acceptance documented
- [ ] Phishing analysis procedure established
- [ ] Phishing drill program

---
