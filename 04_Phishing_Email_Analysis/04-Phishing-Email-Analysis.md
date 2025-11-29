# Module 04 — Introduction to Phishing

## 🎯 Learning Objectives
- Understand what phishing is and why it is a common initial attack vector.
- Learn how phishing maps to the Cyber Kill Chain (Delivery phase).
- Know basic information-gathering and header analysis techniques used during phishing investigations.

## 🔑 What is Phishing?
- Phishing is an attack that aims to steal personal information by tricking users into clicking malicious links or running malicious files. Attackers use social-engineering lures (e.g., "you won a gift", "big discount", "account will be suspended") to prompt clicks.
- Phishing typically falls under the Delivery phase of the Cyber Kill Chain: the attacker transfers pre-arranged malicious content to victims.

## 🎯 Attack Purpose and Human Factor
- The real goal is to exploit the human element—the weakest link—so attackers often use phishing as an initial step to infiltrate systems, not only to directly steal credentials.

## 🔎 Information Gathering & Spoofing
- Email spoofing allows attackers to send emails that appear to come from someone else. SPF, DKIM, and DMARC are protocols designed to help determine whether a sender is legitimate; some email clients check these automatically, but use is not mandatory and can cause issues.
- Manual spoof checks: identify the SMTP address in the header, query the domain's SPF/DKIM/DMARC/MX records (tools like MxToolbox), and compare records to detect spoofing.
- Checking Whois records for the SMTP IP can help confirm whether a sending IP belongs to the claimed institution.
- Note: a non-spoofed sender doesn't guarantee safety—trusted accounts can be compromised and used to send malicious email.

## 📧 Email Traffic Analysis (gateway search parameters)
- Useful parameters to search on mail gateways: Sender address, SMTP IP, domain base (e.g., @letsdefend.io), different sender aliases, subject, recipient list, timestamps.
- Repeated delivery to the same recipients may indicate leaked addresses (e.g., posted on Pastebin). Tools such as Harvester (Kali) can find email addresses posted publicly.

## 🧾 Email Headers — Purpose & Important Fields
- The header contains sender, recipient, date, Return-Path, Reply-To, Received entries, etc., and is essential for tracking and analysis.
- Key header fields:
  - From: sender name and email
  - To: recipient and CC/BCC
  - Date: timestamp when sent
  - Subject: topic of the message
  - Return-Path / Reply-To: address where replies are sent
  - DKIM/DomainKey: email signature for authentication
  - Message-ID: unique identifier for each email
  - MIME-Version: indicates multipart content/attachments
  - Received: the chain of mail servers the message passed through (reverse chronological)
  - X-Spam-Status: spam score and classification

## 🔧 How to Access Headers
- Gmail: open the message → menu (three dots) → Download message → open `.eml` with a viewer/editor.
- Outlook: open message → File → Info → Properties → Internet headers.

## 🕵️ Email Header Analysis — Key Questions
- Was the email sent from the correct SMTP server? Check `Received` fields and compare the sending IP to the domain's MX records (e.g., via MxToolbox).
- Are the `From` and `Return-Path/Reply-To` the same? If not, the reply may go to a different address—suspicious but not definitive on its own; evaluate in context (attachments, URLs, content).

### Example findings described in the module
- The module's example showed an email from `ogunal@letsdefend.io` that actually originated from IP `101[.]99.94.116`; letsdefend.io uses Google MX servers, so the sending IP did not match—indicating spoofing.

## 🧪 Static Analysis
- HTML emails can hide malicious URLs behind seemingly harmless text/buttons; hovering reveals the true destination.
- New domains used briefly for phishing are suspicious. Check URLs in VirusTotal; be aware results may be cached—re-scan if the last analysis is old.
- Check SMTP IP reputation via Cisco Talos, VirusTotal, AbuseIPDB to identify blacklisted or suspicious senders.

## ⚙️ Dynamic Analysis
- Test URLs and files in sandbox environments (e.g., Browserling, VMRay, JoeSandbox, AnyRun, Hybrid Analysis/Falcon Sandbox) to avoid executing malware on your own system.
- Before visiting a link, remove or obfuscate sensitive query parameters (like user email) to avoid signalling valid users to the attacker.
- Malware may delay behavior; allow longer analysis durations when needed.
- Note: attackers may embed malware in images to avoid direct URL/file detection.

## 🔁 Additional Phishing Techniques
- Attackers abuse legitimate services to evade detection:
  - Cloud storage links (Google Drive, Microsoft) hosting malicious files.
  - Free subdomains (WordPress, Blogspot, Wix) where Whois info is not helpful and may appear legitimate.
  - Form applications (Google Forms) to collect credentials because domain appears legitimate.

---

Note: these notes were prepared strictly from the Module 4 text you provided and include only content from that text.
