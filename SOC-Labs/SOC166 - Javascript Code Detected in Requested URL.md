# SOC166 — Javascript Code Detected in Requested URL

Overview
- Alert: SOC166 — Javascript Code Detected in Requested URL.

Alert details


![](../98-assets/SOC_labs/Pasted%20image%2020251125142805.png)

We can see that the alert is triggered because the requested URL contains Javascript code. This is a common technique used in Cross-Site Scripting (XSS) attacks.

Investigation steps

1. **Analyze the URL**
Is the traffic legitimate or malicious?
`https://172.16.17.17/search/?q=<$script>javascript:$alert(1)<$/script>`


![](../98-assets/SOC_labs/Pasted%20image%2020251125142955.png)

As we can see, the URL contains a script tag with a JavaScript alert function. This is a clear indication of a potential XSS attack.

2. **Check the Source IP**
   
   Look up the source IP address (112.85.42.13) in Virus Total to see if it has been associated with any malicious activity.


![](../98-assets/SOC_labs/Pasted%20image%2020251125143303.png)
   
   As we can see this IP is flagged as malicious in Virus Total.

3. **Follow the Playbook**
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125143401.png)
   
   Now we have to check if it is a Planned Test. After checking emails we didnt found anything so it was unplanned.

4. **Attack Success Verification**
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125143827.png)
   
   After some tries he gets a 302 HTTP code that means redirection. This could indicate that the attack was successful but if we check the HTTP response size we can see that is 0 bytes. This means that the server didnt return any content. So we can conclude that the attack was not successful.

5. **Containment**
   
   Since the attack was not successful we can answer No to that question. No further action is required at this time.

## Artifacts / IOCs

- **Source IP**: `112.85.42.13` (Flagged as malicious in VirusTotal)
- **Target**: `/search` endpoint
- **Payload**: `<$script>javascript:$alert(1)<$/script>`
- **Attack Type**: Cross-Site Scripting (XSS)
- **HTTP Status**: 302 (Redirect with 0 bytes response)
- **Planned Test**: No (unplanned attack)

## Case disposition

- **Status**: TRUE POSITIVE (Attack Attempt Detected)
- **Severity**: Medium
- **Success**: No (Attack Failed)
- **Analysis**: XSS attempt detected; all requests resulted in HTTP 302 with 0 byte responses; no successful exploitation; recommend monitoring for similar attempts from this IP.
