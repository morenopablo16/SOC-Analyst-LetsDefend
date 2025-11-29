# SOC146 — Phishing Mail Detected — Excel 4.0 Macros Walkthrough

Overview
- Alert: SOC146 — Phishing Mail Detected (Excel 4.0 Macros).


Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.
- Attachment type: Excel macro (Excel 4.0) with two DLLs.

Investigation steps

1. Parse emails
- Search Email Logs to find email details and attachments.
- Findings: Email contained attachments that extract into 3 files (1 .xls, 2 .dll).

2. Analyze attachments / VirusTotal
- Extracted files found malicious on VirusTotal:
  - 1 xls (malicious macro)



![](../98-assets/SOC_labs/Pasted%20image%2020251124155536.png)
  - iroto.dll (malicious)

![](../98-assets/SOC_labs/Pasted%20image%2020251124155544.png)

  - iroto1.dll (malicious)

![](../98-assets/SOC_labs/Pasted%20image%2020251124155557.png)


- Conclusion: Attachment is malicious.

3. Are there attachments or URLs in the email?
- Yes — confirmed via Email Logs.

4. Check if mail was delivered to user
- Delivered. Device Action = Allowed (seen in alert details).

5. Delete email from recipient
- Remove the malicious mail from the recipient mailbox.

6. Check if someone opened the malicious file / URL
- Correlate SIEM/EDR with endpoint data using indicators from the macro (URLs/IP).
- Using recipient identity (Lars) locate endpoint: LarsPRD.
- Evidence found:
  - Browser History on LarsPRD contains contacted URLs matching macro activity and alert timestamp.

![](../98-assets/SOC_labs/Pasted%20image%2020251124155932.png)


  - Command history shows registration of malicious DLLs.

![](../98-assets/SOC_labs/Pasted%20image%2020251124155946.png)

  - Process activity: Excel spawned regsvr32.exe which registered the DLLs.

![](../98-assets/SOC_labs/Pasted%20image%2020251124160116.png)


- Conclusion: Malicious file was opened.

Containment
- Isolate the infected endpoint (use EDR/network controls as available).
- Add discovered artifacts/IOCs to perimeter/EDR/AV blacklists.


![](../98-assets/SOC_labs/Pasted%20image%2020251124160146.png)


Artifacts / IOCs (add to case)
- hxxps://royalpalm.sparkblue.lk/vCNhYrq3Yg8/dot.html
- hxxps://nws.visionconsulting.ro/N1G1KCXA/dot.html
- 8e6fbefcbac2a1967941fa692c82c3ca — malicious dll hash
- e03bde4862d4d93ac2ceed85abf50b18 — malicious dll hash
- b775cd8be83696ca37b2fe00bcb40574 — malicious macro file hash

Analysis notes
- User Lars received a phishing email containing a malicious macro.
- Macro executed and performed network communication consistent with malware.
- Process evidence: Excel spawned regsvr32.exe; DLLs registered; network callbacks observed.

Case disposition
- Verdict: TRUE POSITIVE — confirmed malicious activity.
- Close alert and document summary and IOC additions in the case.
