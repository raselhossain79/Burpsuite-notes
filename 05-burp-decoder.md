
# 🔐 Burp Suite — Decoder

## What is Decoder?

Decoder is a tool for encoding and decoding data. Web applications use various encoding methods to transfer data — Decoder lets you convert between them instantly.

It is essential for:
- Reading encoded tokens and cookies
- Crafting encoded payloads
- Bypassing input filters via encoding
- Understanding obfuscated data

---

## Opening Decoder

- Decoder tab in Burp
- Or right-click any value in Proxy/Repeater → **Send to Decoder**

---

## Supported Encoding Types

| Encoding | Used In |
|----------|---------|
| URL encoding | Query strings, form data |
| HTML encoding | Web page content |
| Base64 | Tokens, cookies, file transfers |
| Hex | Binary data, hashes |
| ASCII | Character codes |
| Gzip | Compressed data |

---

## URL Encoding

Converts special characters to `%XX` format.

```
Space → %20
/ → %2F
= → %3D
& → %26
' → %27
< → %3C
```

**Decode:**
Paste `admin%27+OR+%271%27%3D%271` → Decode as URL → `admin' OR '1'='1`

**Encode:**
Paste `admin' OR '1'='1` → Encode as URL → `admin%27+OR+%271%27%3D%271`

**Why important:**
WAFs block `'` but may allow `%27` — same character, different representation.

---

## Base64 Encoding

Encodes binary data as text. Common in cookies, JWT tokens, API keys.

```
Plaintext: {"user":"admin","role":"user"}
Base64:    eyJ1c2VyIjoiYWRtaW4iLCJyb2xlIjoidXNlciJ9
```

**Decode example:**

1. Copy cookie value from Proxy
2. Paste in Decoder
3. Decode as Base64
4. See plaintext — may reveal user data, role, session info

**Encode modified data:**

1. Modify decoded value: change `"role":"user"` → `"role":"admin"`
2. Encode as Base64
3. Replace cookie in Repeater → test privilege escalation

---

## HTML Encoding

Converts characters to HTML entities:

```
< → &lt;
> → &gt;
" → &quot;
' → &#x27;
```

**XSS filter bypass:**

If `<script>` is blocked, try HTML encoded:
```
&lt;script&gt;alert(1)&lt;/script&gt;
```

Some filters block the raw characters but not entities — browser renders them the same.

---

## Hex Encoding

Converts to hexadecimal representation:

```
A → 41
admin → 61646d696e
' → 27
```

**SQL injection filter bypass:**

Some WAFs block `UNION SELECT` as text — hex encode it:
```
0x554e494f4e2053454c454354
```

MySQL accepts hex strings directly in queries.

---

## Multi-Step Decoding

Sometimes data is encoded multiple times:

```
Original → Base64 → URL encode → double encoded
```

In Decoder, chain multiple operations:
1. Decode URL
2. Then decode Base64
3. See final plaintext

---

## Real-World Scenarios

### Scenario 1 — Cookie Analysis

1. Intercept request in Proxy
2. Copy cookie value
3. Send to Decoder → Decode as Base64
4. Check if it reveals session data, user role, or predictable pattern

---

### Scenario 2 — JWT Token Analysis

JWT tokens have 3 Base64 parts separated by `.`:

```
eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiYWRtaW4ifQ.SIGNATURE
     Header              Payload              Signature
```

1. Split by `.`
2. Decode each part as Base64
3. Header: `{"alg":"HS256"}`
4. Payload: `{"user":"admin"}`
5. Try changing `"alg":"none"` to bypass signature check

---

### Scenario 3 — WAF Bypass via Encoding

WAF blocks: `<script>alert(1)</script>`

Try in Decoder — encode differently:
```
URL:    %3Cscript%3Ealert(1)%3C%2Fscript%3E
HTML:   &lt;script&gt;alert(1)&lt;/script&gt;
Double: %253Cscript%253E (double URL encoded)
```

Test each encoded version in Repeater — one may bypass the WAF.

---

### Scenario 4 — Hidden Data in Parameters

Parameter value: `dXNlcj1hZG1pbg==`

1. Decode as Base64 → `user=admin`
2. Modify: encode `user=superadmin` as Base64 → `dXNlcj1zdXBlcmFkbWlu`
3. Replace in Repeater → test

---

## Smart Decode

Decoder has **Smart decode** option — automatically detects and applies the right decoding type.

Paste unknown encoded value → **Smart decode** → Burp tries common formats.

---

## Practice

| Lab | Exercise |
|-----|---------|
| DVWA | Decode session cookie |
| PortSwigger | JWT token manipulation |
| Any HTTPS site | Decode Base64 cookies |
| PortSwigger WAF bypass labs | Encode payloads to bypass filters |
