# 15 — Network Log Analysis (Part 1) 🌐

## Learning Objectives 🎯

- Analyze Netflow, Firewall, VPN, and Proxy logs
- Detect security threats through network logs
- Understand log structure and key fields

---

## 1. Netflow Analysis 📊

**Definition**: Protocol collecting IP traffic information (Cisco-developed, multi-vendor support).

### Flow Attributes

- Source/Destination IP & Port
- IP Protocol
- Interface Info
- IP Version (IPv4/IPv6)

### Key Benefits

- **Billing**: ISP usage tracking
- **Network Design**: Traffic analysis, capacity planning
- **Monitoring**: Port usage, traffic patterns
- **QoS**: Performance measurement
- **Security**: Anomaly detection, threat identification

### Security Use Cases

✅ Detect traffic spikes
✅ Identify data leaks
✅ Monitor unauthorized access
✅ Discover new/unknown IPs
✅ Analyze first-time connections to critical systems

---

## 2. Firewall Log Analysis 🔥

**Purpose**: Control network traffic based on security policies (Layer 3-7 inspection).

**NGFW**: Identifies applications regardless of port (e.g., blocks SSH on any port, not just 22).

### Sample Log & Key Fields

```
srcip=172.14.14.26 srcport=50495 srcintf="ACC-LAN" 
dstip=142.250.186.142 dstport=443 dstintf="Wan"
srccountry="Reserved" dstcountry="United States" 
action="accept" service="HTTPS" transip=89.145.185.195
duration=72 sentbyte=2518 rcvdbyte=49503
```

| Field | Description | Field | Description |
|-------|-------------|-------|-------------|
| **srcip/dstip** | Source/Dest IP | **action** | accept/deny/drop |
| **srcport/dstport** | Source/Dest Port | **transip** | NAT IP |
| **srccountry/dstcountry** | Geo location | **service** | Protocol (HTTPS) |

### Actions

- **accept**: Passed ✅
- **deny**: Blocked + notify source 🚫
- **drop**: Blocked silently 🔇
- **client/server-rst**: Connection terminated

### Detection Use Cases

✅ Port scanning
✅ IoC communication
✅ Lateral movement (LAN-to-LAN)
✅ Unauthorized access (LAN-to-WAN)
✅ Follow-up on IPS denies

---

## 3. VPN Log Analysis 🔐

**Purpose**: Remote access to internal systems. Publicly exposed = prime attack surface.

**Deployment**: Integrated in firewalls OR standalone appliances.

### Sample Log

```
type="event" subtype="vpn" action="tunnel-up" tunneltype="ssl-web"
remip=13.29.5.4 user="letsdefend-user" reason="login successfully"
```

**Key Fields**: remip (source IP), user (username), reason (success/fail)

**Tunnelip**: After successful connection, system assigns internal IP for VPN access. All subsequent traffic uses this tunnelip as source.

### Detection Use Cases

✅ Successful/failed attempts
✅ Brute-force attacks
✅ Geographic anomalies (wrong country)
✅ Time anomalies (off-hours access)
✅ Impossible travel (2 countries, short time)

---

## 4. Proxy Log Analysis 🔄

**Purpose**: Bridge between endpoint and internet. Controls web access based on URL/domain categories.

**Types**:
- **Transparent**: Target sees real source IP
- **Anonymous**: Target sees proxy IP only

**Popular**: Cisco Umbrella, Forcepoint, Check Point URL Filtering, Fortinet Secure Web Gateway

### Sample Log

```
srcip=192.168.209.142 srcintfrole="lan" dstip=54.20.21.189 
dstport=443 hostname="android.prod.cloud.netflix.com" 
profile="Wifi-Guest" action="blocked" 
url="https://android.prod.cloud.netflix.com/"
urlsource="Local URLfilter Block"
```

**Analysis**: 192.168.209.142 in "Wifi_Guest" group blocked from netflix (URL in block list).

### Detection Use Cases

✅ Connections to suspicious URLs/domains
✅ Infected system (C2 communication)
✅ Tunneling activities (data exfiltration)

### Advanced Example: Server Requesting Web

**Scenario**: Server (10.80.18.50) making POST to suspicious proxy "sentry-proxy.cargox.cc"
- Category 194 = "Extended Protection Suspicious Content"
- **Red Flag**: Servers shouldn't make web requests
- **Action**: Check source process via EDR/XDR

---

## Quick Reference 📋

| Log Type | Key Fields | Detects |
|----------|------------|---------|
| **Netflow** | Src/Dst IP:Port, Protocol | Traffic spikes, data leaks, new IPs |
| **Firewall** | Src/Dst IP:Port, Action | Port scans, IoC comms, lateral movement |
| **VPN** | RemIP, User, Reason | Brute-force, geo/time anomalies |
| **Proxy** | URL, Hostname, Action | Suspicious URLs, C2, tunneling |

---

## SOC Focus Points 🎯

### Netflow
1. **Baseline**: Know normal patterns first
2. **Port Analysis**: High ports internal→external = C2
3. **Volume Spikes**: Sudden increase = exfiltration
4. **First-Seen**: New IPs to critical systems

### Firewall
5. **Action Check**: Accept vs deny = reached target?
6. **Port Scanning**: Multiple denies, sequential ports
7. **NAT Tracking**: Use transip after NAT
8. **Correlate**: Match accepts with IPS/IDS

### VPN
9. **Failed→Success**: Multiple fails then success = compromise
10. **Impossible Travel**: 2 countries, short time
11. **Off-Hours**: Access outside business hours
12. **Tunnelip**: Follow VPN IP through firewall logs

### Proxy
13. **Server Web Access**: Servers shouldn't browse web
14. **Category Numbers**: Know your proxy categories
15. **Blocked Intent**: Blocks show attack attempt
16. **User-Agent**: Automated tools vs browsers

### General
17. **Correlate Logs**: Use multiple sources
18. **Time Zones**: Consistent across sources
19. **Context**: Server behavior ≠ workstation
20. **Chain**: Netflow→FW→Proxy→Web = full picture

---

*Part 2 covers: IDS/IPS, WAF, Web Server, DNS*
