# 🛠️ Burp Suite — Setup & Configuration

## What is Burp Suite?

Burp Suite is an integrated platform for web application security testing. It acts as a **proxy** between your browser and the target web server — allowing you to intercept, inspect, and modify all HTTP/HTTPS traffic.

```
Browser → Burp Suite (proxy) → Web Server
```

It is the industry standard tool for web application penetration testing — used in CEH, BSCP, OSCP, and real-world bug bounty hunting.

---

## Editions

| Edition | Price | Key Difference |
|---------|-------|---------------|
| Community | Free | No active scanner, throttled Intruder |
| Professional | ~$449/year | Full scanner, unlimited Intruder, all features |

> Community edition is enough for learning and most manual testing.

---

## Installation

Pre-installed on Kali Linux. Launch:

```bash
burpsuite
```

Or from Applications → Web Application Analysis → Burp Suite

Manual install (any OS):
- Download from: https://portswigger.net/burp/releases
- Requires Java (JRE 11+)

---

## Initial Setup

### Step 1 — Create Temporary Project

On launch:
- Select **Temporary project**
- Use Burp defaults
- Click Start Burp

> Save projects only in Pro edition. Community edition uses temporary projects.

---

### Step 2 — Configure Proxy Listener

Burp listens on `127.0.0.1:8080` by default.

Verify: **Proxy → Options → Proxy Listeners**

| Setting | Default Value |
|---------|--------------|
| Bind port | 8080 |
| Bind address | 127.0.0.1 |

Change port if 8080 is in use:
- Click the listener → Edit → Change port

---

### Step 3 — Configure Browser Proxy

**Firefox (recommended):**

Settings → Network Settings → Manual proxy:

| Field | Value |
|-------|-------|
| HTTP Proxy | 127.0.0.1 |
| Port | 8080 |
| Also use for HTTPS | ✅ Check |

> Use Firefox exclusively for Burp testing — keep your main browser clean.

**Faster method — FoxyProxy extension:**
- Install FoxyProxy in Firefox
- Add profile: 127.0.0.1:8080
- Toggle on/off with one click

---

### Step 4 — Install Burp CA Certificate (HTTPS)

Without this, HTTPS sites show certificate errors and Burp cannot intercept them.

**Install process:**

1. Make sure Burp is running and browser proxy is configured
2. In Firefox, go to: `http://burpsuite`
3. Click **CA Certificate** → Download `cacert.der`
4. Firefox → Settings → Privacy & Security → Certificates → View Certificates
5. Authorities tab → Import → Select `cacert.der`
6. Check: **Trust this CA to identify websites** → OK

**Verify:**
- Visit any HTTPS site
- No certificate warning = installed correctly
- Burp should show the intercepted HTTPS traffic

---

### Step 5 — Test Setup

1. Enable intercept: **Proxy → Intercept → Intercept is ON**
2. Visit any website in browser
3. Request should appear in Burp → Forward it
4. If traffic appears — setup is complete ✅

---

## Interface Overview

| Tab | Purpose |
|-----|---------|
| Proxy | Intercept and modify live traffic |
| Repeater | Manually resend and modify requests |
| Intruder | Automated attacks (brute force, fuzzing) |
| Scanner | Automated vulnerability detection (Pro) |
| Decoder | Encode/decode data (Base64, URL, HTML etc.) |
| Comparer | Compare two requests or responses |
| Logger | Full log of all traffic |
| Dashboard | Overview of scans and activity |

---

## Useful Settings

### Increase Display Font
User options → Display → Increase font size for readability

### Dark Theme
User options → Display → Theme → Dark

### Scope Configuration
Prevents Burp from capturing traffic from unrelated sites:

Target → Scope → Add target domain

Then in Proxy → Options:
- **And URL is in target scope** → only intercept in-scope traffic

---

## ⚠️ Important Notes

- Never run Burp against targets without written authorization
- Always use a dedicated browser profile for testing
- Remove CA certificate from browser after testing session if using shared machine
- Community Intruder is throttled — Pro is needed for fast attacks

---

## Practice Labs

| Platform | URL | Notes |
|----------|-----|-------|
| DVWA | Local install | HTTP, good for basics |
| bWAPP | Local install | Wide vulnerability coverage |
| PortSwigger Web Academy | https://portswigger.net/web-security | Free, best structured labs |
| HackTheBox | https://hackthebox.com | Real-world style |
| TryHackMe | https://tryhackme.com | Beginner friendly |

