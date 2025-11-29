# 20 — VirusTotal for SOC Analysts 🔎

## Learning Objectives 🎯

- Analyze files, URLs, hashes, IPs, and domains on VirusTotal
- Interpret detection results and avoid common mistakes
- Use Relations, Behavior, and Community tabs effectively
- Search for IOCs and understand historical context

---

## What is VirusTotal? 🎯

**Definition**: Service aggregating multiple antivirus solutions for querying/analyzing malware, IPs, domains, URLs.

**Owner**: Acquired by Google (2012)

**Access**: Free tier (sufficient for SOC analysts) + Paid tier

**⚠️ CRITICAL**: Uploaded files can be downloaded by premium users → **Don't upload files with sensitive/confidential data**

---

## File Analysis 📄

**URL Example**: https://www.virustotal.com/gui/file/415ba65e21e8de9196462b10dd17ab81d75b3e315759ecced5ea8f5812000c1b

### Detection Section

**Purpose**: Shows how many vendors detected file as malicious.

**Example**: `42/58` = 42 out of 58 security vendors flagged as malicious

**Tags**: Classification info (e.g., "macro", "obfuscated", "trojan")

---

### Details Section

**Basic Properties**: File hash (MD5, SHA1, SHA256), size, type

**History** (Critical for SOC):
- **First Submission**: When file first uploaded to VT
- **Last Analysis**: Most recent scan

**SOC Insight**:
- File analyzed **before your submission** → Malware NOT written specifically for your org (likely campaign targeting multiple orgs)
- File **never analyzed** → Possible targeted attack OR new malware variant

---

### Relations Tab 🔗

**Purpose**: Shows domains, IPs, URLs, other files the malware communicates with.

**Use Cases**:
- Check C2 (Command & Control) servers
- Find related malware samples
- Detect suspicious communication patterns

**Detections Score**: Shows how many vendors flagged related entity as malicious

**⚠️ Limitation**: Modern malware adapts behavior per environment → Relations list may be **incomplete** (malware might not exhibit same behavior in VT sandbox)

---

### Behavior Tab 🔬

**Purpose**: Lists activities performed by file in sandbox.

**Activities Tracked**:
- Network connections (HTTP/HTTPS requests)
- DNS queries
- File operations (read/write/delete)
- Registry modifications
- Process creation/injection

**Multiple Vendors**: Select different sandbox vendors (section 1) to see varied results.

**⚠️ Critical Limitation**: 
- Malware without active C2 → May not activate
- Environment-aware malware → May detect sandbox and stay dormant
- **Solution**: Check old analysis reports when C2 was active

---

### Community Tab 💬

**Purpose**: User comments about file.

**Value**: 
- How file was obtained
- Analysis tips
- Undetected behaviors
- Campaign context

**Recommendation**: Always check - sometimes contains critical insights from other analysts.

---

## URL Analysis 🌐

**Example**: `thuening[.]de[/]cgi-bin/uo9wm/` (malicious - do NOT visit)

**VT Link**: https://www.virustotal.com/gui/url/2bcbc32b84d5d2f6ca77e99232134947377302e7eeee77555672e57f81cd9428

### Links Tab

**Purpose**: Shows external links from scanned URL.

**Example**: `letsdefend.io` → Links to social media accounts

**SOC Use Case**: 
- URL itself may be clean
- BUT links to malicious sites → Investigation continues
- Check all linked destinations

---

## Searching for IOCs 🔍

**Purpose**: Search hashes, IPs, domains from investigations.

### Hash Search

**Example**: SHA256 `415ba65e21e8de9196462b10dd17ab81d75b3e315759ecced5ea8f5812000c1b`

**Result**: Historical analysis + file details

---

### IP Address Search

**Example**: `70.121.172.89`

**Shows**:
- IP reputation
- Detection score
- **Relations Tab**: Files communicating with this IP

**Reverse Investigation**: 
- Upload file → See IPs in Relations
- Search IP → See files in Relations
- Cross-reference for campaign mapping

---

### Domain Search

**Similar to IP**: Shows reputation, related files, historical analysis.

---

## Common Mistakes to AVOID ❌

### Mistake 1: Old Analysis Results 🕐

**Scenario**:
1. Attacker creates clean URL `letsdefend.io/file`
2. Uploads to VT → Gets clean results
3. Replaces URL content with malicious payload
4. SOC analyst searches URL → Sees **old clean results** → Assumes safe ❌

**VT Behavior**: Shows cached results for speed (e.g., "Last analysis: 1 month ago")

**Solution**: **ALWAYS click "Reanalyse"** button to scan current content

**Critical**: Attackers exploit this to bypass detection!

---

### Mistake 2: Detection Tags (False Positives) 🏷️

**Scenario**: Setup files often flagged as "Adware"

**Why**: Legitimate software includes ads in installer → AV engines flag as "Adware"

**Example**: WinRAR setup file shows detections on VT but is legitimate

**Rule**: `10/52` detection ≠ always malicious

**Solution**: 
1. Check **detection tags** (Adware vs Trojan vs Ransomware)
2. Verify file source (official website?)
3. Check Community tab for context
4. Consider file purpose (installer, utility, etc.)

---

## VirusTotal Workflow for SOC 🔄

### File Investigation

```
1. Upload file (or search hash)
   ↓
2. Check Detection score (X/Y vendors)
   ↓
3. Review Tags (macro? obfuscated? trojan?)
   ↓
4. Details → History (first seen? targeted attack?)
   ↓
5. Relations → C2 IPs/domains? Related malware?
   ↓
6. Behavior → Network activity? File drops? Registry?
   ↓
7. Community → Other analyst insights?
```

---

### URL Investigation

```
1. Search URL in VT
   ↓
2. Click "Reanalyse" (avoid old results!)
   ↓
3. Check Detection score
   ↓
4. Links tab → External destinations?
   ↓
5. Relations → Related files/IPs?
   ↓
6. Community → Known phishing campaign?
```

---

### IOC Enrichment

```
1. Receive IOC (hash/IP/domain) from alert
   ↓
2. Search in VT
   ↓
3. Check reputation + detection score
   ↓
4. Relations tab → Map campaign (related IPs, files, domains)
   ↓
5. Historical data → Timeline of activity
   ↓
6. Document findings in investigation notes
```

---

## Key Sections Summary 📋

| Section | Purpose | Key Insight |
|---------|---------|-------------|
| **Detection** | Vendor consensus | `42/58` = high confidence malicious |
| **Details** | File metadata + history | First seen date = targeted vs campaign |
| **Relations** | C2 infrastructure | Map attacker infrastructure |
| **Behavior** | Sandbox activities | Network, files, registry, processes |
| **Community** | Analyst comments | Real-world context, tips |
| **Links** (URLs) | External destinations | Clean URL → Malicious link? |

---

## Best Practices 🎯

### DO ✅

1. ✅ **Always "Reanalyse"**: Get current results, not cached
2. ✅ **Check History**: First seen date = campaign scope
3. ✅ **Review Tags**: Understand detection type (Adware vs Trojan)
4. ✅ **Explore Relations**: Map full attack infrastructure
5. ✅ **Read Community**: Other analysts may have key details
6. ✅ **Cross-reference IOCs**: Hash → IP → Domain → Related files
7. ✅ **Consider Context**: Setup file with Adware tag may be legitimate

### DON'T ❌

8. ❌ **Trust old results**: Attacker may have swapped content
9. ❌ **Upload sensitive files**: Premium users can download
10. ❌ **Assume incomplete**: Relations/Behavior may not show full picture (sandbox evasion)
11. ❌ **Ignore low detections**: `2/58` could still be targeted malware
12. ❌ **Skip Behavior tab**: Network activity = C2 indicators

---

## SOC Use Cases 📊

### Use Case 1: Phishing Email Attachment

**Step 1**: Extract attachment hash
**Step 2**: Search hash in VT
**Step 3**: Check History → First seen 3 months ago = campaign (not targeted)
**Step 4**: Relations → Find C2 domains
**Step 5**: Block C2 domains at firewall/proxy

---

### Use Case 2: Suspicious URL in Alert

**Step 1**: Search URL in VT
**Step 2**: Click "Reanalyse" (critical!)
**Step 3**: Check detection: `15/90` vendors flag malicious
**Step 4**: Links tab → URL redirects to known malware dropper
**Step 5**: Containment → Block URL, isolate affected endpoint

---

### Use Case 3: Unknown Hash from Endpoint

**Step 1**: Search hash in VT
**Step 2**: No previous analysis → Upload file (if not sensitive)
**Step 3**: Behavior tab → File makes DNS queries to suspicious domain
**Step 4**: Relations → 5 related malware samples (same campaign)
**Step 5**: Hunt → Search for related hashes across environment

---

## Integration with SOC Workflow 🔗

### SIEM Alert → VT Enrichment

```
SIEM Alert (suspicious file executed)
   ↓
Extract hash from alert
   ↓
Search hash in VT API (automated)
   ↓
Enrich alert with:
   - Detection score
   - First seen date
   - C2 IPs/domains
   - Behavior summary
   ↓
Prioritize based on VT results
```

### Threat Intelligence

**VT as Intel Source**:
- New malware samples uploaded daily
- Community insights on campaigns
- Relations = attacker infrastructure mapping
- Historical data = trend analysis

---

## Quick Reference 📋

| Task | VT Section | Key Action |
|------|------------|------------|
| **Is file malicious?** | Detection | Check score + tags |
| **Targeted attack?** | Details → History | First seen date |
| **Find C2 servers** | Relations | Check contacted IPs/domains |
| **Understand behavior** | Behavior | Network, files, registry |
| **Campaign context** | Community | Read analyst comments |
| **URL leads where?** | Links | Check external destinations |
| **Search IOC** | Search bar | Hash/IP/Domain/URL |
| **Avoid old results** | Any section | Click "Reanalyse" |

---

## SOC Focus Points 🎯

1. ✅ **"Reanalyse" is MANDATORY**: Old results = attacker trap
2. ✅ **History = Campaign Indicator**: First seen before you = not targeted
3. ✅ **Relations = Infrastructure Map**: Find all C2 domains/IPs
4. ✅ **Behavior = True Intent**: Network activity reveals C2 communication
5. ✅ **Community = Analyst Network**: Learn from others' investigations
6. ✅ **Tags = Context**: Adware ≠ Trojan (severity differs)
7. ✅ **Don't Upload Sensitive**: Premium users can download your files
8. ✅ **Cross-Reference IOCs**: Hash → File, IP → Files, Domain → Files
9. ✅ **Sandbox Limitations**: Malware may not activate (check old reports)
10. ✅ **Detection Score ≠ Absolute**: Low score can still be malicious (zero-day, targeted)

---

*Note: Module 20 emphasizes practical VirusTotal usage for SOC analysts - file/URL analysis, IOC enrichment, and critical mistakes to avoid (old results, false positive tags). Essential daily tool for malware triage and threat intelligence.*
