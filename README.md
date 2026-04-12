# Burpsuite-notes# 🕷️ Burp Suite — Complete Notes

> Personal documentation of Burp Suite web application security testing tool.
> Covers every major feature with real-world scenarios, attack techniques, and practice lab references.

---

## 👤 About This Repo

Hands-on notes built while practicing web application penetration testing.
Not copy-pasted theory — actual techniques used in labs and real testing scenarios.

---

## 📂 Contents

| File | Topic |
|------|-------|
| [01 — Setup](./01-burp-setup.md) | Installation, CA certificate, browser config, scope |
| [02 — Proxy](./02-burp-proxy.md) | Intercept, modify, HTTP history, WAF bypass headers |
| [03 — Repeater](./03-burp-repeater.md) | Manual testing, SQLi, XSS, IDOR, session manipulation |
| [04 — Intruder](./04-burp-intruder.md) | Brute force, fuzzing, CSRF bypass, rate limit bypass |
| [05 — Decoder](./05-burp-decoder.md) | Base64, URL, HTML, Hex encoding — WAF bypass via encoding |
| [06 — Comparer](./06-burp-comparer.md) | Response comparison, username enumeration, access control |
| [07 — Scanner](./07-burp-scanner.md) | Passive/active scanning, security headers, manual alternatives |

---

## 🛠️ Tools & Stack

![Burp Suite](https://img.shields.io/badge/Burp_Suite-Community-FF6633?style=flat&logo=burpsuite&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=flat&logo=kalilinux&logoColor=white)
![DVWA](https://img.shields.io/badge/DVWA-Practice_Lab-green?style=flat)
![PortSwigger](https://img.shields.io/badge/PortSwigger-Web_Academy-orange?style=flat)

---

## 🎯 Key Features Covered

- **Proxy** — intercept and modify live HTTP/HTTPS traffic
- **Repeater** — manual vulnerability testing with full request control
- **Intruder** — automated attacks including brute force and fuzzing
- **Decoder** — encode/decode data, craft filter-bypass payloads
- **Comparer** — detect subtle response differences
- **Scanner** — automated vulnerability detection (Pro) + manual alternatives

---

## 🧠 Vulnerability Types Practiced

- SQL Injection (manual testing via Repeater)
- Cross-Site Scripting — XSS (reflected, stored)
- Insecure Direct Object Reference — IDOR
- Brute Force (Intruder — login, directory)
- CSRF Token Bypass
- Session/Cookie Manipulation
- WAF and Rate Limit Bypass
- Privilege Escalation via Parameter Tampering
- JWT Token Analysis

---

## 🔬 Practice Platforms

| Platform | URL | Used For |
|----------|-----|---------|
| DVWA | Local install | Basic vulnerability practice |
| bWAPP | Local install | Wide vulnerability coverage |
| PortSwigger Web Academy | https://portswigger.net/web-security | Structured labs, BSCP prep |
| HackTheBox | https://hackthebox.com | Real-world style challenges |
| TryHackMe | https://tryhackme.com | Beginner friendly rooms |

---

## ⚠️ Legal Notice

All techniques documented here are for **authorized security testing only**.
Never use these tools against systems you do not own or have explicit written permission to test.
Unauthorized testing is illegal.

---

## 🔗 Connect

- GitHub: [github.com/raselhossain79](https://github.com/raselhossain79)

---

> 💡 *"Understanding how attacks work is the foundation of building real defenses."*
