# SOC167 — LS Command Detected in Requested URL

Overview
- Alert: SOC167 — LS Command Detected in Requested URL.

Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.


Investigation steps

1. **Analyze the URL**

![](../98-assets/SOC_labs/Pasted%20image%2020251125151340.png)

Here we can see the following URL: `https://letsdefend.io/blog/?s=skills`
The alert is triggered because the requested URL contains the "ls" command. This command is often used by attackers to list directory contents on a compromised system.

We can see that the alert might be a false positive because the "ls" command is part of the word "skills" in the URL parameter.

2. **Check the Source IP**

   Look up the source IP address (172.16.17.46) in Virus Total to see if it has been associated with any malicious activity.
![](../98-assets/SOC_labs/Pasted%20image%2020251125151500.png)

    This IP is not flagged as malicious in Virus Total.

3. **Follow the Playbook**

After checking the logs we can see that there is no malicious activity from that IP.

![](../98-assets/SOC_labs/Pasted%20image%2020251125151710.png)

We can also check if there is any other log from that IP, there is indeed another log but its a normal GET request.

## Artifacts / IOCs
- Source IP: 172.16.17.46
- Target: /blog endpoint
- Payload: ls command in URL parameter "s=skills"
## Case disposition
- Verdict: FALSE POSITIVE (no attack detected).
- Analysis notes: The presence of "ls" in the URL parameter is part of a legitimate search query ("skills"); no evidence of malicious activity found; recommend monitoring for similar patterns but no immediate action required.