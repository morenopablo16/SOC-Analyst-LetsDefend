# SOC170 — Passwd Found in Requested URL - Possible LFI Attack

Overview
- Alert: SOC170 — Passwd Found in Requested URL - Possible LFI Attack.

Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.
Investigation steps

1. **Analyze the URL**
The url we can see on the ticket is the following one:
`https://172.16.17.13/?file=../../../../etc/passwd`
The alert is triggered because the requested URL contains a path traversal attempt to access the `/etc/passwd` file, which is a common target in Local File Inclusion (LFI) attacks.

![](../98-assets/SOC_labs/Pasted%20image%2020251125153458.png)


2. **Check the Source IP**

   Look up the source IP address (106.55.45.162) in Virus Total to see if it has been associated with any malicious activity.


![](../98-assets/SOC_labs/Pasted%20image%2020251125153522.png)

    As we can see this IP is flagged as malicious in Virus Total.

    Also we can confirm that this IP is not internal. So the attack is coming from outside.

    Internet → 106.55.45[.]162 → 172.16.17[.]13 (Our Web server)

3. **Follow the Playbook**

    Now we have to check if it is a Planned Test. After checking emails we didnt found anything so it was unplanned.

4. **Attack Success Verification**

![](../98-assets/SOC_labs/Pasted%20image%2020251125153705.png)

Luckily the attack was not successful we can see that the server returned a 500 HTTP code that means Internal Server Error. This indicates that the server was unable to process the request, likely due to security measures in place to prevent such attacks.

## Artifacts / IOCs
- Source IP: 106.55.45.162
- Target: / endpoint
- Payload: Path traversal attempt to access /etc/passwd

## Case disposition
- Verdict: TRUE POSITIVE (attack attempted but failed).
- Analysis notes: LFI attack attempt detected; server returned HTTP 500 for all requests; no successful file access; recommend monitoring for similar attempts from this IP.
