# SOC105 - Requested T.I. URL address

Overview
- Alert: SOC105 — Requested T.I. URL address.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251128154100.png)

The alert is triggered because the requested URL contains a domain that is listed in the Threat Intelligence (T.I.) feed as malicious.

First lets check the requested URL:

`https://bit.ly/TAPSCAN`

![](../98-assets/SOC_labs/Pasted%20image%2020251128154217.png)

If we analyze the URL we can see that it is a shortened URL using the bit.ly service. Shortened URLs are often used by attackers to hide the true destination of the link.
If we analyze the bit.ly link we can see that it redirects to `https://play.google.com/store/apps/details?id=pdf.tap.scanner&referrer=appmetrica_tracking_id=674968008912837528&ym_tracking_id=2762378484247110398`

If we analyze the final URL we can see that it is a link to a Google Play Store app called "Tap Scanner - PDF Scanner App, Scan to PDF".

Investigation steps

Lets mark the No malicious activity found in the playbook since we have not found any malicious activity yet.

![](../98-assets/SOC_labs/Pasted%20image%2020251128154502.png)


Since we have not found any malicious activity we can answer No to the question Was the attack successful?. No further action is required at this time.

## Artifacts / IOCs

- **Requested URL**: `https://bit.ly/TAPSCAN` (redirects to Google Play Store app)
- **Final URL**: `https://play.google.com/store/apps/details?id=pdf.tap
.scanner&referrer=appmetrica_tracking_id=674968008912837528&ym_tracking_id=2762378484247110398` 
## Case disposition
- **Status**: FALSE POSITIVE (No Malicious Activity Found)
- **Analysis notes**: The requested URL redirected to a legitimate Google Play Store app. No malicious activity was detected.
