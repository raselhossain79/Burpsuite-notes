
# ⚔️ Burp Suite — Intruder

## What is Intruder?

Intruder automates customized attacks against web applications. It takes a request, lets you mark positions (placeholders), and automatically replaces them with payloads from a list.

```
Request with §positions§ → Intruder → Try each payload → Analyze responses
```

Used for: brute force, fuzzing, enumeration, bypassing rate limits.

---

## ⚠️ Community Edition Throttling

Community edition throttles Intruder attacks — adds delay between requests. For fast attacks, Burp Pro is needed. For learning and manual testing, Community is fine.

---

## How to Send a Request to Intruder

- From Proxy Intercept: Right-click → **Send to Intruder** (Ctrl+I)
- From HTTP History: Right-click → Send to Intruder
- From Repeater: Right-click → Send to Intruder

---

## Intruder Tabs

| Tab | Purpose |
|-----|---------|
| Positions | Mark attack positions in request |
| Payloads | Configure what to insert |
| Options | Attack settings (headers, rate, grep) |
| Results | View all responses |

---

## Step 1 — Set Positions

In Positions tab, the request is shown with `§` markers.

Burp auto-marks some parameters. Clear them:
**Clear §** → manually select what you want to attack.

Select a value → **Add §** → wraps it in `§value§`

Example — mark password field:
```
username=admin&password=§test§
```

---

## Attack Types

| Type | Use Case |
|------|---------|
| Sniper | One position, one payload list — most common |
| Battering Ram | Same payload in all positions simultaneously |
| Pitchfork | Multiple positions, multiple lists — paired (user1:pass1, user2:pass2) |
| Cluster Bomb | Multiple positions, all combinations — username × password |

---

## Attack Type Details

### Sniper
One position, tries each payload one by one.

```
§password§
→ password1
→ password2
→ password3...
```

Use for: single field fuzzing, brute force one parameter.

---

### Cluster Bomb
Multiple positions, tries every combination.

```
§username§ + §password§
→ admin:password1
→ admin:password2
→ user:password1
→ user:password2...
```

Use for: username + password brute force when both are unknown.

---

### Pitchfork
Multiple positions, paired payloads.

```
List 1: admin, user, test
List 2: pass1, pass2, pass3

→ admin:pass1
→ user:pass2
→ test:pass3
```

Use for: testing known username:password pairs from a leaked credential list.

---

## Step 2 — Configure Payloads

**Payloads tab → Payload Sets**

### Payload Types

| Type | Description |
|------|-------------|
| Simple list | Load a wordlist file |
| Runtime file | Read from file during attack |
| Numbers | Generate number sequences |
| Dates | Generate date sequences |
| Brute forcer | Generate character combinations |
| Character substitution | Apply leet-speak rules (a→@, e→3) |

### Load a Wordlist

Payload Sets → Payload type: Simple list → Load → Select file

Common wordlists on Kali:
```
/usr/share/wordlists/rockyou.txt
/usr/share/wordlists/dirb/common.txt
/usr/share/seclists/Passwords/
/usr/share/seclists/Usernames/
```

---

## Step 3 — Configure Options

### Grep — Match (Find Success)

Add string that appears in successful response:

**Options → Grep - Match → Add**

Example: `Welcome` or `Dashboard` or `Login successful`

Intruder will flag responses containing this string.

### Grep — Extract

Extract data from responses (e.g., CSRF tokens):

**Options → Grep - Extract → Add**

Define the pattern around the token — Intruder extracts it from each response.

### Request Engine (Rate Control)

**Options → Request Engine**

| Setting | Purpose |
|---------|---------|
| Number of threads | Parallel requests (lower = stealthier) |
| Throttle | Delay between requests (ms) |
| Retry failed requests | Auto retry on error |

For WAF/rate limit bypass:
```
Threads: 1
Throttle: 2000ms (2 seconds between requests)
```

---

## Practical Examples

### Example 1 — Login Brute Force (DVWA)

1. Intercept DVWA login → Send to Intruder
2. Clear all positions
3. Mark password: `password=§test§`
4. Attack type: Sniper
5. Payload: Simple list → rockyou.txt
6. Options → Grep Match: add `Welcome to the password protected area`
7. Start Attack
8. Filter results by Grep match — find successful login

---

### Example 2 — Username + Password Brute Force

1. Mark both: `username=§admin§&password=§test§`
2. Attack type: Cluster Bomb
3. Payload Set 1: usernames list
4. Payload Set 2: passwords list
5. Start Attack → filter by response length or status code

---

### Example 3 — Directory Fuzzing

Find hidden endpoints:

1. Intercept: `GET /§test§ HTTP/1.1`
2. Attack type: Sniper
3. Payload: common.txt wordlist
4. Start Attack → look for 200 status codes

---

### Example 4 — CSRF Token Bypass

Modern forms have CSRF tokens that change every request. Hydra cannot handle this — Intruder can.

1. Intercept login request
2. Note CSRF token field: `csrf_token=§TOKEN§&username=admin&password=§PASS§`
3. Options → Grep Extract — extract token from login page response
4. Configure recursive grep to get fresh token for each request
5. Pitchfork attack — token from response + password from wordlist

---

### Example 5 — Rate Limit / WAF Bypass

**Method 1 — Slow down:**
```
Threads: 1
Throttle: 3000ms
```

**Method 2 — Header rotation:**
Options → Request Headers → Add:
```
X-Forwarded-For: §IP§
```
Use number payload to rotate IPs:
```
X-Forwarded-For: 1.1.1.1
X-Forwarded-For: 1.1.1.2
X-Forwarded-For: 1.1.1.3...
```

Some WAFs rate limit per IP — rotating header fakes different source IPs.

**Method 3 — User-Agent rotation:**
Add User-Agent as a position — rotate through browser User-Agents.

---

## Analyzing Results

| Column | What to look for |
|--------|----------------|
| Status | 200 = success, 302 = redirect (often login success) |
| Length | Different length = different response = something changed |
| Grep match | Flagged responses containing success string |

**Sort by Length** — successful login usually returns different length response than failed.

---

## Real-World Bug Bounty Scenario

Finding valid usernames via response difference:

```
POST /login
username=§admin§&password=wrongpass
```

- Existing user: "Wrong password" (length: 1823)
- Non-existing user: "User not found" (length: 1756)

Sort results by length — shorter responses = non-existing users, longer = valid usernames found.

---

## Practice

| Lab | Exercise |
|-----|---------|
| DVWA (Low) | Brute force login with Intruder |
| PortSwigger | Username enumeration lab |
| PortSwigger | Brute force with CSRF token lab |
| bWAPP | Directory fuzzing |
