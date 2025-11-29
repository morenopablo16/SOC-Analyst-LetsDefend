# SOC105 - Requested T.I. URL address

Overview
- Alert: SOC105 — Requested T.I. URL address.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251128155347.png)


Investigation steps

1. **Analyze the URL**
Is the traffic legitimate or malicious?

`http://115.99.150.132:56841/Mozi.m`

![](../98-assets/SOC_labs/Pasted%20image%2020251128155421.png)

This url is pointing to an IP address with a specific port (56841) and a file named "Mozi.m". The Mozi malware is known for its use in botnet activities, which raises suspicion about the legitimacy of this URL.

If we look for that IP address on Virus Total we can see that there are multiple reports indicating malicious activity associated with this IP.

2. **Check Logs**

Lets check the logs to see what that IP have done:

![](../98-assets/SOC_labs/Pasted%20image%2020251128155702.png)

We can see that there is a request from the internal IP to that addres but the Device Action is "Blocked". This indicates that the security systems in place have successfully prevented any potential malicious activity from occurring.

## Artifacts / IOCs
- **Requested URL**: `http://115.99.150.132:56841/Mozi.m`
- **IP Address**: `115.99.150.132`

## Case disposition
- **Status**: FALSE POSITIVE (No Malicious Activity Found)
- **Analysis notes**: The requested URL is associated with known malware, but the security systems successfully blocked any potential malicious activity. No further action is required at this time.