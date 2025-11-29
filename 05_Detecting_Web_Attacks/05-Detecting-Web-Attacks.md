# Module 05 — Detecting Web Attacks

## 🎯 Learning Objectives
- Understand why web attacks are prevalent (75% of cyber attacks target web applications).
- Learn common web attack types: SQL Injection, XSS, Command Injection, IDOR, RFI/LFI, File Upload.
- Know HTTP request/response structure and key headers.
- Recognize detection methods for each attack type covered in the module.

## 🔑 Introduction
- Web applications (Google, Facebook, YouTube, etc.) serve as the primary internet interface for many organizations and are heavily targeted by attackers.
- According to Acunetix research, 75% of all cyber attacks occur at the web application level.
- Attackers exploit web apps to gain access to devices, steal data, or cause service disruptions.

## 🛡️ OWASP
- The Open Worldwide Application Security Project (OWASP) is a non-profit focused on improving software security.
- OWASP Top Ten (2021): lists the 10 most critical web application security risks (Broken Access Control, Cryptographic Failures, Injection, Insecure Design, Security Misconfiguration, Vulnerable and Outdated Components, Identification and Authentication Failures, Software and Data Integrity Failures, Security Logging and Monitoring Failures, Server-Side Request Forgery).

## 🌐 How Web Applications Work
- Web apps communicate via HTTP (Hyper-Text Transfer Protocol) at Layer 7 of the OSI model.
- Communication flow: client requests a resource → server processes request → server sends HTTP response → client displays the resource.

### HTTP Requests
- Structure: Request Line (HTTP method + resource), Request Headers, Request Message Body.
- Key headers: Host, Cookie, User-Agent, Accept, Accept-Encoding, Accept-Language, Connection.
- Example: `GET /` requests the main page; cookies store session info; User-Agent contains browser/OS details.

### HTTP Responses
- Structure: Status Line (HTTP version + status code), Response Headers, Response Body.
- Status codes: 1xx (Informational), 2xx (Successful), 3xx (Redirection), 4xx (Client error), 5xx (Server error).
- Key headers: Date, Connection, Server, Last-Modified, Content-Type, Content-Length.
- Response Body: contains the requested resource (e.g., HTML).

## 🔍 Detecting SQL Injection (SQLi)
- SQLi occurs when unsanitized user data is included directly in SQL queries.

### Types of SQLi
- In-band (Classic): query and response on same channel (easier to exploit).
- Inferential (Blind): response not visible directly.
- Out-of-band: response via another channel (e.g., DNS).

### How SQLi Works (example from module)
- Login query: `SELECT * FROM users WHERE username = 'USERNAME' AND password = 'USER_PASSWORD'`
- Payload: `' OR 1=1 -- -` → query becomes `SELECT * FROM users WHERE username = '' OR 1=1` (always true → authentication bypass).

### What Attackers Gain
- Authentication bypass, command execution, data exfiltration, create/delete/update database entries.

### Detection Methods
- Check all user-supplied data (forms, headers like User-Agent).
- Look for SQL keywords: INSERT, SELECT, WHERE, UNION, CHR.
- Check special characters: apostrophes ('), dashes (-), parentheses.
- Familiarize with common SQLi payloads.

### Detecting Automated SQLi Tools (e.g., Sqlmap)
- Check User-Agent for tool names/versions.
- Monitor request frequency (automated tools send many requests per second).
- Look for tool names in payloads.
- Complex payloads may indicate automation.

### Detection Example (from module)
- Access log analysis: repeated requests to same page with % symbols (URL encoding).
- Decode URLs to reveal SQL keywords (UNION, SELECT, AND, CHR).
- High request frequency (>50 requests/second) and complex payloads suggest automated attack.
- Compare response sizes to infer success (significant differences may indicate successful exploitation).

## 🔍 Detecting Cross-Site Scripting (XSS)
- XSS is an injection vulnerability where malicious scripts execute in a user's browser due to unsanitized user input.

### Types of XSS
- Reflected (Non-Persistent): payload must be in the request; most common.
- Stored (Persistent): payload permanently stored; most dangerous.
- DOM Based: payload modifies DOM environment on client side.

### How XSS Works (example from module)
- Vulnerable code displays user input without sanitization.
- Payload: `<script>alert(1)</script>` triggers popup.
- Payload: `<script>window.location='https://google.com'</script>` redirects user.

### What Attackers Gain
- Steal session info, capture credentials, etc.

### Detection Methods
- Look for keywords: alert, script, prompt, console.log.
- Learn common XSS payloads.
- Check for special characters: `<`, `>`.

### Detection Example (from module)
- Access logs show searches on WordPress (`/blog/?s=`) with encoded XSS payloads.
- Decode URLs to reveal javascript-related keywords.
- Requests every 3–4 seconds and User-Agent (urllib library) indicate automated scanner.

## 🔍 Detecting Command Injection
- Command Injection occurs when unsanitized user data is passed directly to OS shell.

### How Command Injection Works (example from module)
- Vulnerable code: file copy operation using user-supplied filename.
- Payload: `letsdefend;ls;.txt` → OS executes three commands: `cp letsdefend`, `ls`, `.txt`.
- Attacker gains OS command execution; can create reverse shells or shutdown systems.

### What Attackers Gain
- Execute OS commands; full system compromise.

### Prevention
- Sanitize user data, limit user privileges, use virtualization.

### Detection Methods
- Check all request areas.
- Look for terminal keywords: dir, ls, cp, cat, type.
- Learn common command injection payloads (e.g., reverse shell commands).

### Detection Example (from module)
- HTTP request with bash command in User-Agent header: `() { :;}; echo "NS:" $(</etc/passwd)`.
- Exploits Shellshock vulnerability (2014); bash executes environment variables unintentionally.

## 🔍 Detecting Insecure Direct Object Reference (IDOR)
- IDOR (Broken Access Control) occurs when authorization checks are missing or improper; user can access objects belonging to others.

### How IDOR Works (example from module)
- URL: `https://letsdefend.io/get_user_information?id=1` retrieves user info.
- Attacker changes `id` parameter (e.g., `id=2`) to access other users' information.

### What Attackers Gain
- Steal personal info, access unauthorized documents, take unauthorized actions (delete/modify).

### Prevention
- Always verify request authorization; remove unnecessary parameters; use session info instead of user-supplied IDs.

### Detection Methods
- Check all parameters.
- Monitor for many requests to same page (brute-force pattern).
- Look for sequential patterns (id=1, id=2, id=3).

### Detection Example (from module)
- WordPress access logs show many requests to `wp-admin/user-edit.php?user_id=` with different user_id values.
- 15–16 requests in short timeframe; User-Agent shows "wfuzz/3.1.0" (automated tool).
- Response size analysis: most responses are 479 bytes; some 5691/5692 with redirect (302) suggest attack unsuccessful.

## 🔍 Detecting Local File Inclusion (LFI) & Remote File Inclusion (RFI)
- LFI: unsanitized user input includes a file from the same server.
- RFI: unsanitized user input includes a file from a remote server.

### How LFI & RFI Work (example from module)
- Vulnerable code uses user-supplied `language` parameter to include files.
- LFI payload: `/../../../../../../../../../etc/passwd%00` → reads `/etc/passwd`.
- RFI payload: attacker hosts malicious file on their server and injects URL.

### What Attackers Gain
- Execute code, disclose sensitive info, Denial of Service.

### Detection Methods
- Examine all request fields.
- Look for special characters: `/`, `.`, `\`.
- Familiarize with common LFI file names (e.g., `/etc/passwd`).
- Look for http/https in payloads (RFI indicator).

---

Note: these notes were prepared strictly from the Module 5 text you provided and include only content from that text.
