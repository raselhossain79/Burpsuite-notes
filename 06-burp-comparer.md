
# 🔍 Burp Suite — Comparer

## What is Comparer?

Comparer allows you to compare two HTTP requests or responses side-by-side, highlighting the differences. It is useful for identifying subtle changes between responses that indicate a vulnerability.

```
Response A vs Response B → Highlighted differences
```

---

## How to Send Data to Comparer

- Right-click any request/response in Proxy, Repeater, or Intruder → **Send to Comparer**
- Or paste text directly in Comparer tab

---

## Interface

Two panels side by side — paste or send data to each.

- **Words** button — compare word by word
- **Bytes** button — compare byte by byte

Differences highlighted in color:
- Modified content
- Added content
- Removed content

---

## Real-World Scenarios

### Scenario 1 — Username Enumeration

Two login responses:

**Response A** (invalid username):
```
HTTP/1.1 200 OK
Content-Length: 1756
"User not found"
```

**Response B** (valid username, wrong password):
```
HTTP/1.1 200 OK
Content-Length: 1823
"Incorrect password"
```

Send both to Comparer → See exact difference → Confirms username enumeration is possible.

---

### Scenario 2 — Detecting SQL Injection

**Response A** — normal: `GET /product?id=1`
**Response B** — injected: `GET /product?id=1'`

Send both to Comparer → If responses differ → injection point confirmed.

---

### Scenario 3 — Privilege Escalation Check

**Response A** — normal user accessing `/admin`
**Response B** — admin user accessing `/admin`

Compare → If responses are identical → access control is broken.

---

### Scenario 4 — Session Token Analysis

Compare two session tokens from different logins:

```
Token 1: eyJhbGciOiJub25lIn0.eyJ1c2VyIjoiYWRtaW4ifQ.
Token 2: eyJhbGciOiJub25lIn0.eyJ1c2VyIjoidXNlciJ9.
```

Comparer shows exactly which bytes changed — helps understand token structure.

---

## Practice

| Lab | Exercise |
|-----|---------|
| DVWA | Compare login responses for valid vs invalid users |
| PortSwigger | Username enumeration via response comparison |
| Any app | Compare admin vs normal user responses |
