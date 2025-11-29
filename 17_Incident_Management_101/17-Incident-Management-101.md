# 17 — Incident Management 101 🎫

## Learning Objectives 🎯

- Understand incident management workflow and key definitions
- Learn how Incident Management Systems (IMS) work
- Follow playbooks for consistent alert investigation
- Distinguish True Positive from False Positive alerts

---

## What is a Cyber Incident? 🚨

**NCSC Definition**: Breach of system security policy affecting integrity/availability OR unauthorized access/attempted access (per Computer Misuse Act).

**SOC Flow**: SIEM collects data → Creates alerts → Sends to **Incident Management System** (IMS) → SOC analyst investigates

**Prerequisite**: SIEM 101 training (https://app.letsdefend.io/training/lessons/siem-101)

---

## Key Definitions 📚

| Term | Definition | Example |
|------|------------|---------|
| **Alert** | SIEM-generated notification requiring investigation | "SQL Injection detected" |
| **Event** | Observable system occurrence (user login, firewall block, email sent) | Firewall blocking connection |
| **Incident** | Security policy violation (NIST SP 800-61) | Confirmed malware infection |
| **True Positive** ✅ | Real threat correctly detected | `src=' OR 1=1` → Actual SQLi attack |
| **False Positive** ❌ | False alarm, no real threat | `q=FC+Union+Berlin` → Sports team, not SQLi |

**TP Example**: URL contains `' OR 1=1` → SQLi alert → TRUE POSITIVE (real attack)
**FP Example**: URL contains "Union" (team name) → SQLi alert → FALSE POSITIVE (benign search)

---

## Incident Management Systems (IMS) 🖥️

**Purpose**: Central platform for alert investigation and documentation (SOC spends most time here).

**Examples**: TheHive (open source), LetsDefend Case Management

### IMS Workflow

```
[SIEM] → [IMS] ← [Threat Intel + SOAR]
           ↓
      [SOC Analyst]
```

**Process**: Alert → Create ticket → Enrich (Threat Intel) → Investigate → Respond (SOAR) → Close

### Integration Benefits

| Integration | Benefit | Without Integration |
|-------------|---------|---------------------|
| **Threat Intel** | Auto reputation check (domain → VirusTotal) | Manual VirusTotal lookup |
| **SOAR** | One-click block (domain via proxy) | Manual firewall/proxy config |
| **SIEM** | Full event context | Manual log searches |

---

## Case/Alert Naming 📝

**Format**: `EventID: {ID} - [{Alert Name}]`

**Example**: `EventID: 115 - [SQL Injection Detected]`

**Benefits**: Quick search by ID or name, instant context, statistics extraction

**Optional Fields**: Alert Category, Event Source, Description

---

## Playbooks 📖

**Definition**: Step-by-step workflows for consistent alert analysis.

**Why Critical**:
1. **Guidance**: Tell analysts what to do (especially juniors)
2. **Consistency**: All analysts follow same standards (e.g., always check C2 access)
3. **Efficiency**: Reduce investigation time

**LetsDefend**: Auto-assigns playbook → Follow steps → Verify analysis with questions

---

## SOC Analyst Workflow 🔍

**Reality**: Most alerts = False Positives → Tune rules constantly with SIEM team

### Investigation Steps (LetsDefend)

```
1. Choose Alert (High severity first in production)
   ↓
2. Take Ownership (prevent duplicate work)
   ↓
3. Create Case (IMS opens case in Investigation Channel)
   ↓
4. Follow Playbook (step-by-step investigation)
   ↓
5. Make Decision (TP or FP?)
   ↓
6. Close Alert (document: classification + explanation + actions)
   ↓
7. Review Results (check Editor Note/Community Walkthrough)
```

**Key**: Take Ownership prevents team duplication (10 alerts → you claim EventID 63 → others take remaining 9)

---

## Key Takeaways 📋

| Concept | Key Point |
|---------|-----------|
| **IMS** | Central investigation platform (most time spent here) |
| **Playbooks** | Mandatory for consistency + quality |
| **Integrations** | Threat Intel + SOAR = speed + automation |
| **Alert Reality** | Most = false positives → tune with SIEM team |
| **Ownership** | Claim alerts before investigating (team coordination) |
| **Analysis Goal** | Determine TP or FP with clear evidence |

---

## SOC Focus Points 🎯

1. ✅ **IMS = Most Time Spent**: Master the platform for efficiency
2. ✅ **Playbooks = Mandatory**: Follow for consistent quality
3. ✅ **False Positives = Majority**: Develop tuning feedback loop with SIEM team
4. ✅ **Threat Intel Integration**: Auto-checks save investigation time
5. ✅ **SOAR Integration**: One-click response actions (block domain, isolate host)
6. ✅ **Meaningful Titles**: `EventID: {ID} - [{Name}]` format enables quick searches
7. ✅ **Severity Priority**: High/Critical alerts first (real world)
8. ✅ **Take Ownership**: Claim before investigating (team coordination)
9. ✅ **Document Everything**: TP/FP decision + reasoning + actions taken
10. ✅ **Learn from Closed Alerts**: Review walkthrough to improve skills

---

*Note: Module 17 Incident Management 101 covers the complete alert investigation workflow from SIEM to case closure, emphasizing playbook usage and IMS platform efficiency for SOC analysts.*
