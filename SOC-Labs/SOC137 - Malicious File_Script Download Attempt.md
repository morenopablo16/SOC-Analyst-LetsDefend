# SOC137 - Malicious File/Script Download Attempt

Overview

- Alert: SOC137 — Malicious File/Script Download Attempt.

Alert details

![](../98-assets/SOC_labs/Pasted%20image%2020251128161928.png)

Investigation steps

1. **Analyze the hash**

If we analyze the hash `08d4fd5032b8b24072bdff43932630d4200f68404d7e12ffeeda2364c8158873` on Virus Total we can see that there are multiple reports indicating malicious activity associated with this hash.


![](../98-assets/SOC_labs/Pasted%20image%2020251128162101.png)

2. **Check Logs**


![](../98-assets/SOC_labs/Pasted%20image%2020251128162259.png)

In the logs we can see that there are some weird requests.
`http://ueba6ka.club/images/DVeUkINudhi79z0c_2Bv/hcMjSPUhHNICgDZ2eJc/uPkHWXvBmVjikkuyor3cnx/gi3BCr71LrlRP/OOmOS8_2/Bl7A_2Fjz2BM4Pth4RZbBKn/1OVxs19bE9/6wtE2QLgVO1DP1TCF/SoIEOJUXIYbo/RtuJbNDFWW5/V.avi`

3. **Check the URL**

![](../98-assets/SOC_labs/Pasted%20image%2020251128162357.png)

If we check that URL on Virus Total we can see that there are multiple reports indicating malicious activity associated with this URL.

Lets check the endpoint `NicolasPRD` to see if there is any request to that URL.

![](../98-assets/SOC_labs/Pasted%20image%2020251128162211.png)

We can see that there is some suspicious activity on the processes called `ERROR` and also on the history some ofuscated commands.

4. **Containment**
Since we have found potential malicious activity we have to contain the `NicolasPRD` endpoint.

![](../98-assets/SOC_labs/Pasted%20image%2020251128162623.png)

## Artifacts / IOCs
- **File Hash**: `f2d0c66b801244c059f636d08a474079`
- **Malicious URL**: `http://ueba6ka.club/images/DVeUkINudhi79z0c_2Bv/hcMjSPUhHNICgDZ2eJc/uPkHWXvBmVjikkuyor3cnx/gi3BCr71LrlRP/OOmOS8_2/Bl7A_2Fjz2BM4Pth4RZbBKn/1OVxs19bE9/6wtE2QLgVO1DP1TCF/SoIEOJUXIYbo/RtuJbNDFWW5/V.avi`

## Case disposition
- **Status**: TRUE POSITIVE (Malicious Activity Detected)
- **Analysis notes**: The file hash and URL are associated with known malicious activity. Appropriate containment measures have been taken to mitigate the risk.