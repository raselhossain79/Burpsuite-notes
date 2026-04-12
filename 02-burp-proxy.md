# 🔄 Burp Suite — Proxy

## What is the Proxy?

The Proxy is the core of Burp Suite. It sits between your browser and the web server, capturing every HTTP/HTTPS request and response. You can inspect, modify, drop, or forward each one.

```
Browser → [Burp Proxy] → Web Server
              ↓
         Intercept / Modify / Forward
```

---

## Proxy Tabs

| Tab | Purpose |
|-----|---------|
| Intercept | Capture and hold requests/responses |
| HTTP History | Log of all traffic |
| WebSockets History | WebSocket traffic log |
| Options | Proxy configuration |

---

## Intercepting Requests

### Enable Intercept

**Proxy → Intercept → Intercept is ON**

Every browser request will now pause in Burp until you act on it.

### Intercept Actions

| Button | Action |
|--------|--------|
| Forward | Send request to server as-is |
| Drop | Block the request entirely |
| Action | Send to Repeater, Intruder, Scanner etc. |
| Intercept is ON/OFF | Toggle interception |

---

## Modifying Requests

When a request is intercepted, you can edit any part:

- **URL** — change endpoint
- **Headers** — add, remove, modify headers
- **Body** — change POST parameters
- **Cookies** — modify session tokens
- **Method** — change GET to POST etc.

Example — change a parameter:

```
Original: username=admin&password=test
Modified: username=admin'--&password=anything
```

Then click Forward — modified request goes to server.

---

## HTTP History

**Proxy → HTTP History**

Shows every request made through Burp — even when intercept is OFF.

Useful for:
- Reviewing all traffic after browsing
- Finding hidden endpoints
- Picking requests to send to Repeater/Intruder

**Filter traffic:**
- Filter bar at top — filter by URL, status code, method
- Right-click → Add to scope
- Right-click → Send to Repeater / Intruder

---

## Intercepting Responses

By default Burp only intercepts requests. To also intercept responses:

**Proxy → Options → Intercept Server Responses → Check "Intercept responses"**

Useful for:
- Modifying response content
- Removing security headers
- Changing redirect responses (302 → 200)

---

## Real-World Scenarios

### Scenario 1 — Bypass Client-Side Validation

A form validates input in JavaScript before sending. Burp bypasses this:

1. Submit form normally — JS validation passes
2. Intercept the actual HTTP request in Burp
3. Modify the value to anything you want
4. Forward — server receives modified value

> Client-side validation is never a security control — server must validate too.

---

### Scenario 2 — Modify Hidden Form Fields

```html
<input type="hidden" name="price" value="100">
```

1. Intercept the POST request
2. Change `price=100` to `price=1`
3. Forward — server may accept the modified price

---

### Scenario 3 — Cookie Manipulation

1. Browse to a site — Burp captures cookies
2. In HTTP History — right-click request → Send to Repeater
3. Modify cookie value
4. Send — check if server accepts it

---

### Scenario 4 — HTTPS Traffic Inspection

With CA certificate installed:
1. Visit HTTPS site
2. Burp decrypts, shows plaintext request/response
3. You can read and modify HTTPS traffic same as HTTP

---

## WAF / Firewall Bypass via Proxy

When a WAF blocks requests, modify headers to confuse it:

```
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
X-Originating-IP: 127.0.0.1
```

Some WAFs trust these headers and treat request as coming from localhost.

Also try:
- Change `Content-Type` header
- Add unusual headers
- Change HTTP version (HTTP/1.0 vs HTTP/1.1)

---

## Match and Replace (Automatic Modification)

Automatically modify every request/response matching a pattern — without manually intercepting each one.

**Proxy → Options → Match and Replace → Add**

Example — replace User-Agent automatically:

| Field | Value |
|-------|-------|
| Type | Request header |
| Match | User-Agent: .* |
| Replace | User-Agent: Mozilla/5.0 |

---

## Scope Control

Limit Burp to only capture traffic for your target:

**Target → Scope → Add**

```
Protocol: https
Host: targetsite.com
```

Then: **Proxy → Options → And URL is in target scope**

Now Burp only intercepts target traffic — browser works normally for other sites.

---

## Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Ctrl+F | Forward request |
| Ctrl+A | Select all |
| Ctrl+R | Send to Repeater |
| Ctrl+I | Send to Intruder |

---

## ⚠️ Important Notes

- Turn intercept OFF when not actively testing — otherwise browser freezes waiting for forwarded requests
- HTTP History captures everything even when intercept is OFF — useful for reviewing traffic after browsing
- Always set scope to avoid accidentally capturing sensitive data from other sites

---

## Practice

| Lab | Exercise |
|-----|---------|
| DVWA | Intercept login request, modify parameters |
| PortSwigger | SQL injection labs — modify requests manually |
| DVWA | Intercept file upload, change file extension |
| bWAPP | Modify hidden price fields |
