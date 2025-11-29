# 18 — Splunk 🔍

## Learning Objectives 🎯

- Understand Splunk as SIEM platform
- Master search syntax and operators
- Create reports, alerts, and dashboards
- Manage users and roles

---

## What is Splunk? 🎯

**Definition**: Data platform for enterprise observability, unified security, and custom applications.

**Popularity**: One of most popular SIEM solutions in cybersecurity industry.

**License**: After 60 days, convert to perpetual free license.

**Key Ports**:
- **9997**: Forwarders → Indexer
- **8000**: Web interface (Search)
- **8089**: splunkd / Deployment server

**Documentation**: https://docs.splunk.com/Documentation/Splunk/latest/Installation/Systemrequirements

---

## Installation (Brief) 📦

**Windows**: Download MSI → Install → Set admin credentials → Access `https://127.0.0.1:8000`

**Linux CLI**: 
```bash
cd /opt && sudo wget [link] && sudo tar xvzf splunk-*.tgz
sudo /opt/splunk/bin/splunk start --accept-license
sudo /opt/splunk/bin/splunk enable boot-start
```

**Forwarder Setup**: Install on clients → Configure indexer `<IP>:9997` → Verify: Settings → Forwarder Management

---

## Adding Data to Splunk 📥

**Method 1 - Forwarder**: Settings → Add Data → Forward → Select logs → Create index → Enable receiver (port 9997)

**Method 2 - Upload**: Settings → Add Data → Upload → Select file → Choose index → Start Searching

---

## Splunk Search Fundamentals 🔍

### Search Syntax Rules

| Rule | Example | Result |
|------|---------|--------|
| **Field names** = case sensitive | `Username` vs `username` | Different fields |
| **Field values** = NOT case sensitive | `admin` vs `ADMIN` | Same value |
| **Wildcard** = `*` | `Username=Je*` | Jean, Jeanne, etc. |
| **Operators** | `AND`, `OR`, `NOT` | Boolean logic |

---

### Date Selection 🕐

**Options**:
- **Presets**: Today, last week, last 24h
- **Relative**: X minutes/hours/days ago
- **Real-time**: Live search
- **Date Range**: Between specific dates/times

---

### Search Modes

| Mode | When to Use |
|------|-------------|
| **Smart** | Default (most common) |
| **Fast** | Quick results, less detail |
| **Verbose** | All field/event data |

---

### Search Examples

**Example 1**: Find username containing "Je"
```
Username=Je*
```

**Example 2**: Logins on specific computer
```
EventCode=4624 AND ComputerName=computer1
```

**Example 3**: Logins excluding domain controller
```
EventCode=4624 NOT ComputerName=domaincontroller
```

**Example 4**: Failed admin logins
```
source="WinEventLog:*" index="winlog_clients" EventCode=4625 AND Account_Name=Admin*
```

---

### Fields Panel (Left Side)

**Purpose**: Show all available fields in search results.

**Usage**: Click field → See statistics (top values, count, etc.)

---

## Reports 📊

**Definition**: Saved search results (scheduled or on-demand).

### Creating a Report

**Example Use Case**: Failed admin login attempts

**Search**:
```
source="WinEventLog:*" index="winlog_clients" EventCode=4625 AND Account_Name=Admin*
```

**Steps**:
1. Test search → Save As → **Report**
2. Give title + description
3. Save → View

---

### Scheduling Reports

**Access**: Search App → Reports → Select report → Edit → Edit Schedule

**Configuration**:
- Schedule: Daily at 08:00 AM
- Time range: Yesterday's data
- Trigger actions: Email, run script, etc.

**Use Case**: Daily report of failed admin logins

---

### Managing Reports

**Location**: Search App → Reports

**Actions**: Edit, Delete, Schedule, Share, Clone

---

## Alerts 🚨

**Definition**: Saved searches triggering when conditions met.

**Types**:
- **Scheduled**: Run at specific times
- **Real-time**: Continuous monitoring (⚠️ server load!)

---

### Creating an Alert

**Example**: Alert on 35+ failed logins

**Search**: Same as report example
```
source="WinEventLog:*" index="winlog_clients" EventCode=4625 AND Account_Name=Admin*
```

**Steps**:
1. Search → Save As → **Alert**
2. Configure:
   - **Alert Type**: Scheduled or Real-time
   - **Trigger Condition**: "Number of results > 35"
   - **Trigger Actions**: Email, webhook, script
3. Save

**Difference from Reports**: Alerts trigger actions automatically when conditions met

---

## Dashboards 📈

**Definition**: Visual panels displaying multiple searches/reports.

**Use Cases**:
- **SOC L1**: Operational metrics for analysts
- **Management**: High-level KPIs for executives
- **Customer**: Relevant security metrics

---

### Creating a Dashboard

**Example**: SOC L1 Dashboard

**Steps**:
1. Search → Save As → **New Dashboard**
2. Dashboard title: "SOC L1"
3. **Share**: Permissions (App or All Apps)
4. Panel title + description
5. Save

**Adding More Panels**: Dashboard → Edit → Add Panel (search, visualization, etc.)

---

## System Management ⚙️

### Health Status Check

**Access**: Click username (top right) → Health status

**Panels**:
- **Left**: Status of each Splunk component (green/yellow/red)
- **Right**: Signal descriptions

**Monitoring**: CPU, memory, disk, indexing performance

---

### User Management 👥

**Location**: Settings → Users

**Best Practice**: 
- Create separate admin account
- Use built-in `admin` only in emergencies
- Monitor `admin` logins (red flag if used)

**User Requirements**: Every user must have a role

---

### Role Management 🔐

**Location**: Settings → Roles

**Default Roles**: admin, power, user

**Role Permissions**:
- Splunk server access
- Search capabilities
- Data visibility (which events user can see)
- Index access

**Creating Roles**: Add New → Define permissions → Save

---

### Password Management 🔑

**Location**: Settings → Password Management

**Default Requirement**: 8 characters minimum (no complexity)

**Best Practice**: Enforce complexity policies (modify in password management settings)

---

## Search Best Practices 🎯

### Optimize Searches

1. ✅ **Specify index**: `index="winlog_clients"` (faster than searching all)
2. ✅ **Use time range**: Narrow to relevant period
3. ✅ **Field extraction**: Reference specific fields vs free text
4. ✅ **Wildcard placement**: `Username=Je*` faster than `*Jean*`

---

### Field Syntax

| Syntax | Example | Use Case |
|--------|---------|----------|
| **Exact match** | `EventCode=4624` | Specific event |
| **Wildcard** | `Username=Admin*` | Pattern matching |
| **AND** | `EventCode=4625 AND ComputerName=DC01` | Multiple conditions |
| **OR** | `EventCode=4624 OR EventCode=4625` | Either condition |
| **NOT** | `EventCode=4624 NOT Username=system` | Exclude values |

---

### Common Event Codes (Windows)

| EventCode | Description | Use Case |
|-----------|-------------|----------|
| **4624** | Successful logon | Track user logins |
| **4625** | Failed logon | Detect brute force |
| **4720** | User account created | Monitor account creation |
| **4672** | Admin privileges assigned | Track privilege escalation |

---

## Quick Reference 📋

| Feature | Purpose | Access |
|---------|---------|--------|
| **Search** | Ad-hoc queries | Search App → Search bar |
| **Reports** | Saved searches (scheduled/on-demand) | Save As → Report |
| **Alerts** | Automated triggers on conditions | Save As → Alert |
| **Dashboards** | Visual panels for multiple metrics | Save As → Dashboard |
| **Forwarders** | Send logs to indexer | Settings → Forwarder Management |
| **Indexes** | Log storage locations | Settings → Indexes |
| **Users** | Access management | Settings → Users |
| **Roles** | Permission management | Settings → Roles |

---

## SOC Focus Points 🎯

1. ✅ **Master Search Syntax**: Operators (AND/OR/NOT) + wildcards = efficient hunting
2. ✅ **Use Indexes**: Specify `index="name"` for faster searches
3. ✅ **Schedule Reports**: Daily/weekly reports for recurring analysis
4. ✅ **Create Alerts**: Automate detection (brute force, admin creation, etc.)
5. ✅ **Build Dashboards**: Operational visibility for SOC team
6. ✅ **Time Range Matters**: Narrow searches to relevant periods
7. ✅ **Field-Based Searches**: `EventCode=4625` faster than free text
8. ✅ **Save Search History**: Reuse successful queries
9. ✅ **Monitor Forwarders**: Settings → Forwarder Management (check connectivity)
10. ✅ **Role-Based Access**: Limit data visibility per analyst role

---

*Note: Module 18 focuses on practical Splunk usage for SOC analysts - search syntax, reports, alerts, and dashboards are core daily activities. Installation is simplified as it's less critical for analysts.*
