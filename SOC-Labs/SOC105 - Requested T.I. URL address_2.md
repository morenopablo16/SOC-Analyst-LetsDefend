# SOC105 - Requested T.I. URL address

Overview
- Alert: SOC105 — Requested T.I. URL address.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251128154914.png)

Investigation steps

1. **Analyze the URL**
Is the traffic legitimate or malicious?
`https://raw.githubusercontent.com/django/django/master/setup.py`

![](../98-assets/SOC_labs/Pasted%20image%2020251128155013.png)

As we can see, the URL points to a raw file in the official Django GitHub repository. This is a legitimate source for Django code.

## Artifacts / IOCs

- **Requested URL**: `https://raw.githubusercontent.com/django/django/master/setup.py`

## Case disposition
- **Status**: FALSE POSITIVE (No Malicious Activity Found)
- **Analysis notes**: The requested URL points to a legitimate file in the official Django GitHub repository. No malicious activity was detected.