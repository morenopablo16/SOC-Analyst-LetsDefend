# 23 — Detecting Brute Force Attacks 🔓

## Learning Objectives 🎯

- Understand brute force attack types (online vs offline)
- Identify vulnerable protocols and services
- Detect brute force attacks using logs (SSH, HTTP, RDP)
- Implement prevention mechanisms (lockout, 2FA, captcha)
- Analyze Windows Event IDs for login activity

---

## 1. Brute Force Attack Overview 🔨

**Definition**: Trial and error method to find credentials, directories, or encryption keys.

**Duration**: Simple password (minutes) → Complex password (years)

---

## 2. Attack Types 📊

| Type | Connection | Target | Examples |
|------|------------|--------|----------|
| **Passive Online** | Indirect (same network) | Network traffic | MITM, Sniffing (hubs only) |
| **Active Online** | Direct to service | Live authentication | SSH, RDP, web login, databases |
| **Offline** | None | Captured hashes | Dictionary, Exhaustive, Rainbow tables |

### Offline Attack Methods

| Method | Description | Speed |
|--------|-------------|-------|
| **Dictionary** | Try passwords from wordlist | Fast (seconds-minutes) |
| **Exhaustive** | Try all character combinations | Slow (hours-years) |
| **Rainbow Tables** | Pre-calculated hashes → Compare | Very fast (instant match) |

---

## 3. Vulnerable Protocols/Services 🎯

| Protocol/Service | Attack Target |
|------------------|---------------|
| **Web Applications** | Login pages |
| **RDP** | Remote Desktop services |
| **SSH** | Secure Shell services |
| **Mail Servers** | Email login pages |
| **LDAP** | Directory services |
| **Databases** | MSSQL, MySQL, PostgreSQL, Oracle |
| **Web Directories** | Directory brute force |
| **DNS** | DNS record detection |

---

## 4. Brute Force Tools 🛠️

| Tool | Purpose | Platform |
|------|---------|----------|
| **Aircrack-ng** | WEP/WPA wireless cracking (40-512 bit keys) | Multi-platform |
| **John the Ripper** | Password cracking, weak password detection | Unix, Windows, OpenVMS (15 platforms) |
| **L0phtCrack** | Windows password cracking (rainbow tables, dictionaries) | Windows |
| **Hashcat** | 300+ hash algorithms, CPU/GPU/hardware accelerators | Linux |
| **Ncrack** | Network authentication cracking | Windows, Linux, BSD |
| **Hydra** | Parallelized login cracker, multiple protocols | Multi-platform |

---

## 5. Prevention Mechanisms 🛡️

| Control | Description | Effectiveness |
|---------|-------------|---------------|
| **Strong Passwords** | Min 8 chars, letters+numbers+symbols, unique per account | High |
| **Lock Policy** | Lock after N failed attempts → Admin unlock | High |
| **Progressive Delays** | Increase lockout time (5 min → 15 min → 1 hour) | Medium |
| **reCAPTCHA** | Bot prevention on login | High |
| **2FA** | SMS/email/token/push notification | **Very High** |

---

## 6. Detection Techniques 🔍

**SIEM Rule**: N failed logins in X minutes → Alert (e.g., 5+ failed in 5 minutes)

---

## 7. Detection Examples 📋

### 7.1. SSH Brute Force (Linux)

**Log File**: `/var/log/auth.log`

**Commands**:
```bash
# Failed attempts by user
grep "Failed password" auth.log | cut -d " " -f10 | sort | uniq -c | sort

# Failed attempts by IP
grep "Failed password" auth.log | cut -d " " -f12 | sort | uniq -c | sort

# Successful logins
grep "Accepted password" auth.log
```

**IOC Pattern**: Multiple failures from single IP → Success = Compromised account

---

### 7.2. HTTP Login Brute Force

**Log File**: Web server access.log

**Key Indicator**: Response size difference
- Failed login: Small response (~500 bytes)
- Successful login: Large response (~5000 bytes)

**Pattern**: Multiple POST requests → Same user → Different passwords → Single large response

---

### 7.3. Windows RDP Brute Force

**Tool**: Event Viewer → Security Logs

**Critical Event IDs**:

| Event ID | Description | Logon Type 10 = RDP |
|----------|-------------|---------------------|
| **4625** | Failed logon | Brute force attempt |
| **4624** | Successful logon | Compromise confirmed |

**Detection Process**:
1. Filter Event ID 4625 → Count per user/IP
2. Filter Event ID 4624 → Check if follows 4625 events
3. Pattern: Multiple 4625 → Single 4624 = **Successful brute force**

**Example**: User `LetsDefendTest` from IP `X.X.X.X`
- 10:15 PM - Event 4625 (Failed)
- 10:16 PM - Event 4625 (Failed)
- 10:16 PM - Event 4625 (Failed)
- 10:17 PM - Event 4625 (Failed)
- 10:17 PM - Event 4624 (Success) ← **Attack successful**

---

## Detection Cheat Sheet �

| Platform | Log Source | Key Indicator | Command/Filter |
|----------|------------|---------------|----------------|
| **Linux SSH** | `/var/log/auth.log` | Multiple "Failed password" → "Accepted password" | `grep "Failed password"` + `grep "Accepted password"` |
| **HTTP** | Web server access.log | Response size difference (small fail → large success) | Analyze POST request sizes |
| **Windows RDP** | Event Viewer Security | Event 4625 (multiple) → Event 4624 (Logon Type 10) | Filter 4625 + 4624 by user/time |

---

## SOC Focus Points 🎯

1. ✅ **2FA Deployment**: Most effective control, prioritize critical accounts
2. ✅ **Lock Policy**: 3-5 failed attempts → Account lock
3. ✅ **SIEM Rules**: Alert on N failed logins in X minutes (e.g., 5 in 5 min)
4. ✅ **Correlate Events**: Failed attempts + Success = Compromise (investigate immediately)
5. ✅ **Monitor High-Risk Services**: RDP, SSH, web apps (most targeted)
6. ✅ **Linux Analysis**: `/var/log/auth.log` → grep "Failed password" + "Accepted password"
7. ✅ **Windows Analysis**: Event 4625 (failed) → Event 4624 (success) correlation
8. ✅ **HTTP Analysis**: Response size difference (small fail → large success)
9. ✅ **Source IP Check**: Single IP = targeted, Multiple IPs = distributed
10. ✅ **Post-Compromise Activity**: Check lateral movement, data access after successful brute force

---

## Investigation Workflow ✅

**When Alert Triggers**:
1. Identify service (SSH/RDP/HTTP/etc.)
2. Count failed attempts + identify source IP(s)
3. Check if any attempts successful
4. **If successful**: Investigate post-login activity → Initiate IR
5. **If failed only**: Block source IP → Notify user

---

*Note: Module 23 covers brute force attack detection - a critical SOC analyst skill. Focus on correlating failed attempts with successful logins to identify compromised accounts. Always check post-compromise activity when attacks succeed.*
