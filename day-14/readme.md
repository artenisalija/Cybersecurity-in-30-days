# day 14 My Metasploit Notes

## Introduction

The Metasploit Framework is a set of tools I can use for information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While I primarily use it in the penetration testing domain, it's also useful for vulnerability research and exploit development.

The main components I work with are:

- **msfconsole** — The main command-line interface I interact with
- **Modules** — Supporting modules like exploits, scanners, and payloads
- **Tools** — Stand-alone tools such as `msfvenom`, `pattern_create`, and `pattern_offset`

---

## Main Components

When I launch Metasploit with `msfconsole`, I interact with modules — small components built to perform specific tasks like exploiting vulnerabilities, scanning targets, or brute-forcing credentials.

Before working with modules, here are three key concepts I keep in mind:

- **Exploit** — A piece of code I use to take advantage of a vulnerability on the target system
- **Vulnerability** — A design, coding, or logic flaw on the target system that I can exploit to disclose information or run code
- **Payload** — The code that actually runs on the target system after the exploit takes effect

### Module Categories

| Module | Purpose |
|--------|---------|
| **Auxiliary** | Supporting modules I use for scanning, crawling, and fuzzing |
| **Encoders** | Encode exploits and payloads to help evade signature-based AV |
| **Evasion** | More direct attempts at bypassing antivirus software |
| **Exploits** | Exploit code organized by target system |
| **NOPs** | No-operation instructions (0x90) I use as buffers for consistent payload sizes |
| **Payloads** | Code that runs on the target system |

### Q&A

- **What takes advantage of a flaw on the target system?** → Exploit
- **What runs on the target system to achieve my goal?** → Payload
- **What are self-contained payloads called?** → Singles
- **Is `windows/x64/pingback_reverse_tcp` a single or staged payload?** → Singles

---

## Using msfconsole

I launch the console with:

```bash
msfconsole
```

To search for a module (e.g., Apache-related ones):

```bash
search apache
```

The `auxiliary/scanner/ssh/ssh_login` module was provided by **todb**.

---

## Working with Modules

Once I've selected a module with `use <module_name>`, I set its parameters using:

```bash
set PARAMETER_NAME VALUE
```

I always run `show options` first to see what parameters are required.

### Common Commands I Use

| Task | Command |
|------|---------|
| Set a local port | `set LPORT 6666` |
| Set a global RHOSTS value | `setg RHOSTS 10.10.19.23` |
| Clear a set payload | `unset PAYLOAD` |
| Start the exploitation phase | `exploit` |