
# 🔁 Burp Suite — Repeater

## What is Repeater?

Repeater allows you to manually send and resend HTTP requests as many times as you want — modifying them each time and observing the server response. It is the most used tool for manual vulnerability testing.

```
Captured Request → Repeater → Modify → Send → Analyze Response → Modify → Send again...
```

---

## How to Open a Request in Repeater

**Method 1 — From Proxy Intercept:**
- Intercept a request → Right-click → **Send to Repeater** (or Ctrl+R)

**Method 2 — From HTTP History:**
- Proxy → HTTP History → Right-click any request → Send to Repeater

**Method 3 — Paste manually:**
- Repeater tab → Paste raw HTTP request

---

## Interface

| Panel | Content |
|-------|---------|
| Left top | Request — editable |
| Right top | Response — from server |
| Bottom | Rendered response (visual) |

**Controls:**
- **Send** — send current request
- **Cancel** — stop current request
- **< >** — navigate request history
- **+** — new tab

---

## Basic Usage

1. Send request to Repeater
2. Modify any part of the request
3. Click **Send**
4. Analyze response on the right
5. Modify again → Send again

---

## Real-World Testing Scenarios

### Scenario 1 — SQL Injection Testing

Original request:
```
GET /product?id=1 HTTP/1.1
Host: target.com
```

Test in Repeater:
```
GET /product?id=1' HTTP/1.1        → Check for SQL error
GET /product?id=1 AND 1=1-- HTTP/1.1   → Should return normal
GET /product?id=1 AND 1=2-- HTTP/1.1   → Should return different/empty
```

Observe response differences to confirm injection.

---

### Scenario 2 — Authentication Bypass

Original login:
```
POST /login HTTP/1.1

username=admin&password=wrongpass
```

Test:
```
username=admin'--&password=anything
username=' OR '1'='1'--&password=x
username=admin&password=' OR '1'='1
```

---

### Scenario 3 — IDOR (Insecure Direct Object Reference)

Original request accessing your own profile:
```
GET /user/profile?id=1001 HTTP/1.1
```

Test in Repeater — change ID:
```
GET /user/profile?id=1000 HTTP/1.1
GET /user/profile?id=1002 HTTP/1.1
```

If another user's data returns — IDOR vulnerability found.

---

### Scenario 4 — XSS Testing

Original search request:
```
GET /search?q=laptop HTTP/1.1
```

Test payloads:
```
GET /search?q=<script>alert(1)</script> HTTP/1.1
GET /search?q=<img src=x onerror=alert(1)> HTTP/1.1
```

Check if payload appears unescaped in response.

---

### Scenario 5 — Header Injection / WAF Bypass

Add headers to bypass WAF or test server behavior:

```
X-Forwarded-For: 127.0.0.1
X-Real-IP: 192.168.1.1
X-Custom-IP-Authorization: 127.0.0.1
```

Some applications grant admin access based on IP headers.

---

### Scenario 6 — Cookie/Session Manipulation

```
Cookie: session=eyJ1c2VyIjoiYWRtaW4ifQ==
```

1. Decode in Burp Decoder — see if it's Base64 JSON
2. Modify: change `"user":"admin"` to `"user":"superadmin"`
3. Re-encode → paste back into Repeater
4. Send — check if elevated access granted

---

### Scenario 7 — HTTP Method Testing

Some endpoints behave differently with different methods:

```
GET /api/user/1     → Returns user data
DELETE /api/user/1  → May delete user if not protected
PUT /api/user/1     → May update user
```

In Repeater — change method and observe response.

---

## Response Analysis

What to look for in responses:

| Response | Meaning |
|----------|---------|
| 200 OK | Request succeeded |
| 302 Redirect | Often used after login |
| 403 Forbidden | Access denied — may be bypassable |
| 500 Internal Server Error | May indicate SQL injection or code error |
| Different content length | Parameter may be affecting behavior |

**Compare responses:**
- Normal request response vs modified request response
- If responses differ — something is happening

---

## Repeater History

Every sent request is saved in Repeater history — use `< >` arrows to go back to previous requests.

Useful for:
- Comparing different payloads
- Returning to a working state
- Documenting findings

---

## Tips for Efficient Testing

- Open multiple Repeater tabs for different endpoints
- Label tabs — right-click tab → Rename
- Use **Render** tab to visually see response
- Copy successful requests to document findings

---

## Practice

| Lab | Exercise |
|-----|---------|
| PortSwigger SQLi labs | Test SQL injection manually in Repeater |
| DVWA (Low security) | IDOR — change user ID |
| PortSwigger IDOR labs | Access other users' data |
| bWAPP | XSS testing — observe raw response |
