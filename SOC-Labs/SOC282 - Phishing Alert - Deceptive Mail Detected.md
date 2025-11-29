# Phishing Alert - Deceptive Mail Detected

Overview
- Alert: SOC282 — Phishing Alert - Deceptive Mail Detected.
Alert details
- Initial action: Take Ownership of the incident and create a case to begin investigation.

Investigation steps
1. **Parse Emails**
May, 13, 2024, 09:22 AM 
We received an alert indicating a phishing email was detected.

![](../98-assets/SOC_labs/Pasted%20image%2020251125172106.png)


![](../98-assets/SOC_labs/Pasted%20image%2020251125171226.png)

Lets parse the email to gather more information about the sender, recipient, subject, and any attachments or links included in the email.

From:
free@coffeeshooop.com
To:
Felix@letsdefend.io
Subject:
Free Coffee Voucher
Date:
May, 13, 2024, 09:22 AM
Action:
Allowed

2. **Analyze Attachments / Links**

    The email contains an attachment named "FreeCoffee.zip". We need to analyze this attachment to determine if it is malicious.
    Inside we can find a Coffee.exe file.
    Lets upload it to VirusTotal to see if it is malicious.

![](../98-assets/SOC_labs/Pasted%20image%2020251125171921.png)

As we can see the file is detected as malicious by multiple antivirus engines.

3. **Check if Mail was Delivered to User**

    We can see whether the e-mail is delivered by looking at the "device action" part of the alert details.
    In this case, the action is "Allowed", so the email was delivered to the recipient's mailbox.

4. **Delete Email from Recipient**
    Since the email was delivered and contains a malicious attachment, we need to delete the email from the recipient's mailbox to prevent any potential harm.

5. **Check if Someone Opened the Malicious File / URL**
    We need to check if the recipient opened the malicious attachment. We can do this by correlating SIEM/EDR logs with endpoint data using indicators from the attachment (e.g., file hash, file name).
    Using the recipient identity (Felix), we locate the endpoint: Felix.
    Evidence found:
    - Process activity: Coffee.exe was executed on Felix.


    ![](../98-assets/SOC_labs/Pasted%20image%2020251125173551.png)

- Command history shows execution of Coffee.exe.

6. **Containment**
    Since the malicious file was executed, we need to contain the affected endpoint. In this case, it is 'Felix' as we can see on the alert.

    
![](../98-assets/SOC_labs/Pasted%20image%2020251125173713.png)

So let's go to the endpoint and isolate it.
This also means that we have to escalate this to the Incident Response Team for further investigation.

## Artifacts / IOCs

- Sender Email: free@coffeeshooop.com
- Recipient Email: Felix@letsdefend.io
- Attachment: Coffee.exe
- File Hash: cd903ad2211cf7d166646d75e57fb866000f4a3b870b5ec759929be2fd81d334
## Case disposition

- Verdict: TRUE POSITIVE (malicious activity detected).
- Analysis notes: Phishing email with malicious attachment detected; attachment executed on recipient's endpoint; recommend isolating affected system and monitoring for further activity.