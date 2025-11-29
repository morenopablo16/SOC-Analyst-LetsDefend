# SOC138 - Detected Suspicious Xls File

Overview
- Alert: SOC138 — Detected Suspicious Xls File.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251126103627.png)

We can see that the alert is triggered because a suspicious XLS file was detected. The file has characteristics commonly associated with malicious documents, such as embedded macros or unusual file structures.

Investigation steps

![](../98-assets/SOC_labs/Pasted%20image%2020251126104002.png)

If we dump the file on Virus total we can see that multiple antivirus engines flag this file as malicious.

1. **Define Threat Indicator**

  We have to choose the type from:
  Unknown or unexpected outgoing internet traffic
  Anti-virus programs malfunctioning or disabled for unknown reasons
  Unknown or unexpected services and applications configured to launch at startup

 So if we launch this xls file in Any.Run we can see that it tries to connect to multiple IPs and URLs that are suspicious.

 ![](../98-assets/SOC_labs/Pasted%20image%2020251126111544.png)

2. **Check if the malware has been quarantined**

  We need to go to the endpoint where the file was detected and check if the antivirus has quarantined the file.
  
![](../98-assets/SOC_labs/Pasted%20image%2020251126111012.png)

The malware isn’t quarantined because it was executed and the ‘Device Action’ is set to ‘Allowed.
Also after all we know the malware has malicious intent since is flaged by multiple antivirus engines in VirusTotal.
3. **Determine if someone requested the C2 address**

    We need to check if the malware has contacted the C2 address.
	We can do this by checking the network logs for any connections to the IP addresses or URLs associated with the malware.

![](../98-assets/SOC_labs/Pasted%20image%2020251126111729.png)

WE can see that Sofia made some connections to suspicious IPs and URLs (177.53.143.89)

4. **Containment**

   Since we have evidence that the malware has contacted the C2 address, we need to contain the incident.
   We can do this by isolating the affected endpoint from the network to prevent further communication with the C2 server.

![](../98-assets/SOC_labs/Pasted%20image%2020251126111851.png)


## Artifacts / IOCs
- **File Hash**: `7bcd31bd41686c32663c7cabf42b18c50399e3b3b4533fc2ff002d9f2e058813`
- **Suspicious IP Address**: `177.53.143.89`

## Case disposition
- **Status**: TRUE POSITIVE (Malicious Activity Detected)
- **Analysis notes**: Suspicious XLS file detected and confirmed as malicious via VirusTotal; evidence of C2 communication found; endpoint isolated to prevent further compromise.