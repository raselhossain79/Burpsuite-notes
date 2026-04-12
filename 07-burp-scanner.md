
# 🔎 Burp Suite — Scanner

## What is Scanner?

Scanner is Burp Suite's automated vulnerability detection tool. It crawls and scans web applications for common vulnerabilities — SQL injection, XSS, SSRF, path traversal, and more.

> ⚠️ Scanner is only available in **Burp Suite Professional**. Community edition has no active scanner.

---

## Two Types of Scanning

| Type | Description |
|------|-------------|
| Passive Scan | Analyzes traffic passing through Proxy — no extra requests sent |
| Active Scan | Sends crafted requests to actively probe for vulnerabilities |

**Passive scan** runs automatically in Community edition (limited).
**Active scan** requires Pro.

---

## Starting a Scan (Pro)

### Method 1 — Scan from Proxy History

Proxy → HTTP History → Right-click request → **Scan**

### Method 2 — New Scan from Dashboard

Dashboard → New Scan → Enter URL → Configure → Start

### Method 3 — Scan specific request

Right-click in Repeater → Active Scan

---

## Scan Configuration

**Scan type:**
- Crawl and audit — discover and test everything
- Audit selected items — test specific requests only

**Audit coverage:**
- Fast — quick, less thorough
- Balanced — recommended
- Deep — thorough, slow

---

## What Scanner Detects

| Category | Examples |
|----------|---------|
| Injection | SQL injection, Command injection, LDAP injection |
| XSS | Reflected, Stored, DOM-based |
| Path Traversal | `../../../../etc/passwd` |
| SSRF | Server-side request forgery |
| XXE | XML external entity |
| Open Redirect | Redirect to external URL |
| Information Disclosure | Stack traces, debug info, version numbers |
| Broken Access Control | IDOR, privilege escalation |
| Security Misconfig | Default credentials, directory listing |

---

## Passive Scanning (Community)

Even without Pro, Burp passively analyzes traffic:

**Dashboard → Issue activity** — shows passive findings

Common passive findings:
- Password submitted over HTTP (not HTTPS)
- Sensitive data in URL parameters
- Cacheable HTTPS responses
- Missing security headers

---

## Security Headers Scanner

Check for missing security headers in any response:

Look for these in response headers:

| Header | Purpose |
|--------|---------|
| `Strict-Transport-Security` | Force HTTPS |
| `X-Content-Type-Options` | Prevent MIME sniffing |
| `X-Frame-Options` | Prevent clickjacking |
| `Content-Security-Policy` | Prevent XSS |
| `X-XSS-Protection` | Browser XSS filter |

Missing headers = findings to report in pentest.

---

## Real-World Workflow (Pro)

1. Set target scope
2. Browse application manually — Proxy captures all traffic
3. Right-click scope in Target → **Active Scan**
4. Review findings in Dashboard → Issue activity
5. Click each finding — see request/response proof
6. Verify manually in Repeater — confirm it is not false positive
7. Document confirmed findings

---

## Without Scanner (Community) — Manual Approach

Use Repeater + Intruder manually for what Scanner does automatically:

| Vulnerability | Manual Method |
|--------------|--------------|
| SQLi | Repeater — test `'`, `AND 1=1--` etc. |
| XSS | Repeater — test `<script>alert(1)</script>` |
| IDOR | Repeater — change ID parameters |
| Brute force | Intruder — wordlist attack |
| Directory fuzzing | Intruder — common.txt wordlist |

---

## PortSwigger Web Academy

The best free alternative to the scanner for learning:

- https://portswigger.net/web-security
- Structured labs for every vulnerability type
- Uses Burp Community + manual testing
- Same labs used in BSCP certification exam

---

## Practice

| Platform | Exercise |
|----------|---------|
| PortSwigger | All vulnerability labs — manual testing |
| DVWA | Manually find SQLi, XSS, IDOR |
| bWAPP | Wide vulnerability coverage |
| HackTheBox | Real-world style web challenges |
