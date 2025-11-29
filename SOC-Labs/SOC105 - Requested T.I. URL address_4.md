# SOC105 - Requested T.I. URL address

Overview 
- Alert: SOC105 — Requested T.I. URL address.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251128160021.png)

Investigation steps

1. **Analyze the URL**

Is the traffic legitimate or malicious?

`https://pssd-ltdgroup.com`

![](../98-assets/SOC_labs/Pasted%20image%2020251128160103.png)

If we analyze the URL we can see that it points to a domain "pssd-ltdgroup.com".
If we look for that domain on Virus Total we can see that there are multiple reports indicating malicious activity associated with this domain.

2. **Check Logs**

Lets check the logs to see what that domain have done:

![](../98-assets/SOC_labs/Pasted%20image%2020251128160158.png)

We can see :
```
Main Process: Krankheitsmeldung_092020_07.xlsm
Request URL: https://pssd-ltdgroup.com
```
We can see that there is a request from the internal IP to that address in this case we dont have the Device Action column so we cant know if it was blocked or allowed.
This indicates that there is a potential risk of malicious activity from this domain.

3. **Containment**

Since we have found potential malicious activity we have to conatain the `BillPRD`endpoint.


![](../98-assets/SOC_labs/Pasted%20image%2020251128160531.png)

## Artifacts / IOCs
- **Requested URL**: `https://pssd-ltdgroup.com`
- **Ip**: `5.188.0.251`
- **Domain**: `pssd-ltdgroup.com`

## Case disposition
- **Status**: TRUE POSITIVE (Malicious Activity Detected)
- **Analysis notes**: The requested URL is associated with known malicious activity. Appropriate containment measures have been taken to mitigate the risk.
