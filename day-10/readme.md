# Day 10 Command Injection

## What is Command Injection?

Command injection is a security vulnerability that allows an attacker to execute arbitrary commands on a host operating system through a vulnerable application. It occurs when an application passes unsafe user-supplied data (forms, cookies, HTTP headers, etc.) to a system shell.

---

## How It Works

When an application constructs a system command using user input without proper sanitization, an attacker can inject additional commands using shell metacharacters such as:

| Character | Effect |
|-----------|--------|
| `;` | Executes commands sequentially |
| `&&` | Executes next command if previous succeeds |
| `\|\|` | Executes next command if previous fails |
| `\|` | Pipes output of one command to another |
| `` ` ` `` | Executes command within backticks (subshell) |
| `$()` | Command substitution |

### Example (Vulnerable Code)

A web app that pings a user-supplied IP address:

```python
import os
user_input = "8.8.8.8; rm -rf /tmp"
os.system("ping -c 1 " + user_input)
```

The shell sees: `ping -c 1 8.8.8.8; rm -rf /tmp` — and executes **both** commands.

---

## Types of Command Injection

- **In-band** — The attacker sees the output of the injected command directly in the application response.
- **Blind** — No output is returned, but the command still executes. Detected via time delays (e.g., `sleep 10`) or out-of-band channels.

---

## Impact

A successful command injection attack can lead to:

- Full system compromise
- Data exfiltration
- Privilege escalation
- Backdoor installation
- Lateral movement within a network

---

## Prevention

1. **Avoid system calls with user input** — Use built-in library functions instead of shell commands where possible.
2. **Input validation** — Whitelist expected input (e.g., only allow alphanumeric characters for a username).
3. **Parameterization** — Use APIs that separate the command from its arguments (e.g., `subprocess.run(["ping", "-c", "1", user_input])` in Python).
4. **Least privilege** — Run the application with the minimum permissions needed.
5. **Web Application Firewalls (WAF)** — Can help detect and block common injection patterns.
6. **Escape special characters** — If shell commands are unavoidable, sanitize and escape all metacharacters.

---

## Tools for Testing

- **Burp Suite** — Intercept and modify HTTP requests to test for injection points.
- **Commix** — Automated command injection exploitation tool.
- **Manual testing** — Try injecting payloads like `; whoami` or `&& id` in input fields.

---

## Resources

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security/os-command-injection)
- [HackTricks - Command Injection](https://book.hacktricks.xyz/pentesting-web/command-injection)

---

> **Note:** This knowledge is for educational and defensive security purposes only. Always obtain proper authorization before testing systems.