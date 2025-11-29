# SOC168 — Whoami Command Detected in Request Body

Overview
- Alert: SOC168 — Whoami Command Detected in Request Body.
Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.
Investigation steps
1. **Analyze the Request Body**
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125145850.png)
   
   The alert is triggered because the request body contains the "whoami" command. This command is often used by attackers to gather information about the current user context on a compromised system.

2. **Check the Source IP**

    Look up the source IP address (61.177.172.87) in Virus Total to see if it has been associated with any malicious activity.

![](../98-assets/SOC_labs/Pasted%20image%2020251125150138.png)

   As we can see this IP is flagged as malicious in Virus Total.
3. **Follow the Playbook**

    Now we have to check if it is a Planned Test. After checking emails we didnt found anything so it was unplanned.

4. **Attack Success Verification**

Lets check the logs to see what that IP have done:

![](../98-assets/SOC_labs/Pasted%20image%2020251125150249.png)

We can see that the attack was successful we run the following commands and got a 200 HTTP code that means OK.
ls
whoami
uname
passwd
shadow

5. **What Is the Direction of Traffic?**

    The logs show the malicious IP was located in China so we can confirm this was not internal whatsoever.

    61.177.172[.]87 (China) → 172.16.17[.]16 (Our Web server)

6. **Containment**

    Since the attack was successful we can contain the web server. In this case is 'WebServer1004' as we can on the alert
    

![](../98-assets/SOC_labs/Pasted%20image%2020251125150738.png)

So lets go to the endpoint and isolate it.
This also means that we have to escalate this to the Incident Response Team for further investigation.


## Artifacts / IOCs
- Source IP: 61.177.172.87
- Target: /api/execute endpoint.
- Payload: whoami, ls, uname, passwd, shadow
## Case disposition
- Verdict: TRUE POSITIVE (attack successful).
- Analysis notes: Command injection attempt detected; multiple commands executed successfully; recommend isolating affected system and monitoring for further activity.
