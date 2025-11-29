# SOC169 — Possible IDOR Attack Detected
Overview
- Alert: SOC169 — Possible IDOR Attack Detected.

Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.
Investigation steps
1. **Analyze the URL**

The url we can see on the tickes is the following one:
`https://172.16.17.15/get_user_info/`

We cant see the full URL so we will need to check the logs to see the full URL with the parameters.

![](../98-assets/SOC_labs/Pasted%20image%2020251125152525.png)

Here we see that the attacker tried to access different user IDs by changing the "user_id" parameter in the URL.
2. **Check the Source IP**

   Look up the source IP address (134.209.118.137) in Virus Total to see if it has been associated with any malicious activity.

   ![](../98-assets/SOC_labs/Pasted%20image%2020251125152606.png)

    As we can see this IP is flagged as malicious in Virus Total.

3. **Follow the Playbook**

    Now we have to check if it is a Planned Test. After checking emails we didnt found anything so it was unplanned.

4. **Attack Success Verification**

As we can see on the logs the attacker was able to access different user IDs and got a 200 HTTP code that means OK.

![](../98-assets/SOC_labs/Pasted%20image%2020251125152731.png)

5. **What Is the Direction of Traffic?**

    The logs show the malicious IP was located in United States so we can confirm this was not internal whatsoever.

    134.209.118[.]137 (United States) → 172.16.17[.]15 (Our Web server)
6. **Containment**
    Since the attack was successful we can contain the web server. In this case is 'WebServer1005' as we can on the alert

    ![](../98-assets/SOC_labs/Pasted%20image%2020251125152837.png)

So lets go to the endpoint and isolate it.

This also means that we have to escalate this to the Incident Response Team for further investigation.

## Artifacts / IOCs
- Source IP: 134.209.118.137
- Target: /get_user_info endpoint.
- Payload: user_id parameter manipulation

## Case disposition
- Verdict: TRUE POSITIVE (attack successful).
- Analysis notes: IDOR attack detected; attacker accessed multiple user IDs successfully; recommend containment and escalation to Incident Response Team.
