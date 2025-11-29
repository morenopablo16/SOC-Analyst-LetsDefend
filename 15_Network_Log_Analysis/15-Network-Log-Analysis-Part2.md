# 15 — Network Log Analysis (Part 2) 🌐

## Learning Objectives 🎯

- Analyze IDS/IPS, WAF, Web Server, and DNS logs
- Detect web attacks and C2 communications
- Understand attack patterns and response codes

---

## 5. IDS/IPS Log Analysis 🛡️

**Purpose**: Inspects packet content (not just IP/port like firewall).

**Analogy**: Firewall = "red apples pass, yellow don't" | IDS/IPS = "checks for worms inside apple"

### IDS vs IPS

| Type | Action | Mode |
|------|--------|------|
| **IDS** | Detects only (alerts) | Passive |
| **IPS** | Detects + Prevents (blocks) | Active inline |

**Signatures**: Rules detecting known attacks (constantly updated). Examples: Snort, Suricata.

**Detects**: Log4j, vulnerability exploits, botnet, C2, post-scan activities

### Sample Log

```
severity="high" srcip=12.11.2.4 dstip=19.66.201.16 
action="detected" service="DNS" 
attack="DNS.Server.Label.Buffer.Overflow"
srcport=57673 dstport=53 direction="incoming"
```

**Key Fields**: severity, srcip/dstip, action (detected/blocked), attack name, direction

### Analysis Process

**Example**: DNS Buffer Overflow from 12.11.2.4 → 19.66.201.16:53
1. Check attack details (affects Tftpd32 DNS Server)
2. Is Tftpd32 running on port 53? If NO → not affected
3. Action = "detected" → Traffic reached destination (not blocked)

### Critical Checks

✅ **Direction**: Inbound or outbound?
✅ **Severity**: High/Critical = urgent, less false positive
✅ **Multiple Signatures**: Same src→dst = active attack
✅ **Service Match**: Is vulnerable service actually running?
✅ **Action**: Detected (reached target) vs Blocked (stopped)

### Detection Use Cases

Port scans, vulnerability scans, code injection, brute-force, DoS/DDoS, trojan, botnet

---

## 6. WAF Log Analysis 🌐

**Purpose**: Secures web applications (Firewall/IPS insufficient alone).

**Flow**: User → Internet → **WAF** → Web Server

**SSL Offload**: Decrypts HTTPS to inspect payload. WAF without this = cannot see HTTPS attacks.

**Popular**: Cloudflare, AWS WAF, F5, Imperva

### Sample Log

```
type=attack sub_type="SQL Injection" severity_level=High
action=Alert policy="Alert_Policy" src=19.6.150.138 dst=172.16.10.10
http_method=get http_url="?v=(SELECT CHR(113)||...)" 
http_host="app.letsdefend.io" attack_type="SQL Injection"
```

**Key Fields**: action (Alert/Block), http_url, attack_type, policy

### Analysis

**SQL Injection from 19.6.150.138 → 172.16.10.10:443**
- Action = "Alert" → Monitoring mode (not blocked)
- Request reached server
- **Check web server response code**:
  - 200 = Attack successful
  - 404 = Attack failed
  - 500 = Server error (DoS effect)

### Response Codes

| Range | Meaning | Security Impact |
|-------|---------|-----------------|
| **200-299** | Success | Attack may have succeeded |
| **400-499** | Client error | Attack likely failed |
| **500-599** | Server error | Caused error (DoS) |

### Detection Use Cases

SQLi, XSS, Code Injection, Directory Traversal, suspicious methods (PUT/DELETE)

---

## 7. Web Server Log Analysis 🖥️

**Popular**: IIS, Apache, Nginx

### Sample Log

```
71.16.45.142 - - [12/Dec/2021:09:24:42 +0200] 
"GET /?id=SELECT+*+FROM+users HTTP/1.1" 200 486 "-" "curl/7.72.0"
```

| Field | Value | Meaning |
|-------|-------|---------|
| **Source IP** | 71.16.45.142 | Requesting IP |
| **Method** | GET | HTTP method |
| **URL** | /?id=SELECT+*+FROM+users | Path (SQL injection!) |
| **Response** | 200 | Success |
| **User-Agent** | curl/7.72.0 | Automated tool |

⚠️ **Note**: POST/PUT data usually NOT in logs by default.

### Attack Patterns in URLs

- **SQLi**: SELECT, UNION, INSERT, DROP
- **XSS**: `<script>`, alert(), onerror=
- **Command Injection**: |, ;, &&, whoami
- **Directory Traversal**: ../../../etc/passwd

### Response Codes

- **200**: Success → Attack succeeded
- **404**: Not found → Attack failed
- **500**: Server error → Attack failed (DoS effect)

### User-Agent

**Automated Tools**: nikto, nessus, nmap, sqlmap, burp
**Real Users**: Mozilla, Chrome, Safari

⚠️ Can be spoofed - verify with other indicators.

### Example Analysis

**URL**: `sqli_1.php?title=%iron' union select 1,user(),3,4,5,6,7-- -`
**Response**: 200
**Result**: Database user "root@localhost" exposed
**Conclusion**: Attack successful

**Critical**: If attacked and behind FW/IPS/WAF → attack bypassed all defenses!

---

## 8. DNS Log Analysis 🔍

**Purpose**: Domain-IP resolution. Critical for investigating which domains systems queried.

### DNS Log Types

1. **DNS Server Records**: Audit events (add/delete/edit records) - Windows Event_ID 516
2. **DNS Queries**: Shows which systems queried which domains (NOT enabled by default)

**IOCs**: Indicators of Compromise - evidence from cybersecurity incidents.

### Sample Log

```json
{
  "source_ip": "192.168.4.76",
  "query": "testmyids.com",
  "qtype_name": "A"
}
```

**Key Fields**: source_ip, query (domain), qtype (A/AAAA/MX/TXT)

**Sources**: DNS server logs (`/var/log/querylog`) OR network capture (Zeek/Bro)

### Detection Use Cases

✅ First-time visited domains
✅ Long subdomains (tunneling)
✅ NX domains (non-existent, DGA)
✅ Domain IOC matches
✅ DNS over TLS/HTTPS detection

### Example 1: DNS Tunneling

**Pattern**: 192.168.10.12 queries random subdomains in 1 minute
**Assessment**: DNS tunneling for data exfiltration
**Action**: Check source process via EDR

### Example 2: Database Server → Cloud Services

**Logs**:
```
client 192.168.10.3: query: login.microsoftonline.com
client 192.168.10.3: query: onedrive.live.com
```
**Red Flag**: Oracle DB server querying OneDrive/Microsoft
**Possible**: Data exfiltration attempt

---

## Quick Reference: Part 2 Log Types

| Log Type | Key Fields | Primary Detection |
|----------|------------|-------------------|
| **IDS/IPS** | src_ip, dest_ip, signature, action | Known attack signatures |
| **WAF** | client_ip, uri, status, rule_id | Web app attacks (SQLi, XSS) |
| **Web Server** | client_ip, method, uri, status, user_agent | Attack patterns, suspicious agents |
| **DNS** | source_ip, query, qtype | Domain IOCs, tunneling, DGA |

---

## SOC Focus Points 🎯

1. ✅ **IDS/IPS alerts** → Check signature severity, validate true positive
2. ✅ **WAF 403 blocks** → Persistent attacker? Same IP in web server logs?
3. ✅ **Web 200 OK after attack pattern** → Successful SQLi/RCE? Investigate immediately
4. ✅ **curl/wget/powershell User-Agents** → Automated attacks or post-exploitation
5. ✅ **DNS queries from servers** → Database/app servers shouldn't query random domains
6. ✅ **Long DNS subdomains** → Potential tunneling for data exfiltration
7. ✅ **Domain IOC matches** → Check all internal IPs querying malicious domain
8. ✅ **Correlate single IP across logs** → Web attack + DNS query + IDS alert = incident
9. ✅ **Internal IP → cloud services** → OneDrive/Dropbox from non-workstations = exfiltration?
10. ✅ **NX domains** → DGA malware beaconing to C2

---

