# SOC165 — Possible SQL Injection Payload Detected

Overview
- Alert: SOC165 — Possible SQL Injection Payload Detected.

![](../98-assets/SOC_labs/Pasted%20image%2020251125135623.png)

Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.

Investigation steps

1. **URL Decode**
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125135434.png)
   
   If we try to decode de actual URL we can see the SQL keywords used in the injection attempt ` OR 1 = 1 -- -`:
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125140314.png)
   
   Clearly, this is an attempt to bypass authentication using SQL Injection.

2. **VirusTotal Check**
   
   We can look also for that ip address on the Virus Total to see if there is any report related to that IP (167.99.169.17).
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125140716.png)

3. **Playbook Follow-up**
   
   Lets check the next steps on the playbook.
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125140417.png)
   
   We need to Check if it is a Planned Test show lets check if there is any email showing that this is a planned test. 
   But after checking emails we didnt found anything so it was unplanned.

4. **Attack Success Verification**
   
   Next task asked if the attack was successful.
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125141405.png)
   
   Lets check the logs to see what that IP have done:
   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125141156.png)
   
   If we check those logs we can see that the attacker have launched multiple requests to the same page with different SQL Injection payloads. But no successful login was achieved. All 500 HTTP codes indicate server errors.
   
   We can see that there is no successful login achieved. All 500 HTTP codes indicate server errors.

   
   ![](../98-assets/SOC_labs/Pasted%20image%2020251125141745.png)
   
   Since the attack was not successful we can answer No to that question. No further action is required at this time.

## Artifacts / IOCs

- **Attacker IP**: `167.99.169.17`
- **SQL Injection Payload**: ` ' OR 1=1 -- - `
- **Target Parameter**: `/search`
- **HTTP Status Codes**: 500 (Server Error)
- **Attack Pattern**: Multiple SQLi attempts with different payloads
- **Planned Test**: No (unplanned attack)

## Case disposition

- **Status**: TRUE POSITIVE (Attack Attempt Detected)
- **Severity**: Medium
- **Success**: No (Attack Failed)
- **Analysis**: After analyzing the logs we can see that there was an attempt to perform SQL Injection attack to bypass authentication. However, the attack was not successful since no successful login was achieved. All requests resulted in server errors (HTTP 500). No further action is required at this time, but monitoring for similar attempts is recommended.


