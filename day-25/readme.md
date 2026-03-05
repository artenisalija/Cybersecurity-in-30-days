# Day 25 🛡️ Malware Classification — What I Learned Today

## Overview

Today I worked through the **TryHackMe: Malware Classification** room. Here's a breakdown of what I picked up.

---

## Malware Types

I learned that malware isn't just one thing — it comes in distinct categories, each with its own purpose and behaviour:

- **Adware** — bombards users with pop-up ads, often running silently in the background
- **Spyware** — secretly monitors activity, capturing keystrokes and sending data to attackers
- **Ransomware** — encrypts files and demands payment to restore access
- **Wiper** — destroys data entirely by overwriting it, no ransom involved
- **C2 (Command & Control) Malware** — gives attackers remote access to issue commands and move files
- **Data Stealer** — collects sensitive files and exfiltrates them to external servers
- **Keylogger** — records every keystroke, including passwords and banking credentials
- **Cryptominer** — hijacks CPU resources to mine cryptocurrency without the user's knowledge

Being able to classify malware correctly matters because it directly shapes how you respond — whether that's isolating a machine, investigating data theft, or tracing lateral movement.

---

## Real-World Malware Families

I also went through some real case studies, which helped me connect the theory to actual incidents:

| Malware | Type | What it does |
|---|---|---|
| **Pegasus** | Spyware | Exploits zero-click vulnerabilities to silently monitor calls, messages, and location on mobile devices |
| **Akira** | Ransomware | Encrypts files *and* threatens to leak stolen data if payment isn't made |
| **Shamoon** | Wiper | Overwrote files with junk data, taking down entire energy company networks for weeks |
| **Agent Tesla** | Data Stealer | Delivered via phishing, captures keystrokes, screenshots, and browser credentials |
| **RedLine Stealer** | Keylogger / Data Stealer | Steals browser cookies, keystrokes, and crypto wallet info |
| **QakBot** | C2 RAT | Started as a banking trojan, evolved into a full RAT used to deploy ransomware like Black Basta |

---

## Binary vs. Script-Based Malware

One of the more practical things I learned today was the difference between how malware is *delivered*:

### Binary Malware (`.exe`, `.dll`)
- Compiled files that run on a compatible OS
- Often disguised with fake icons or misleading extensions (e.g. `invoice.pdf.exe`)
- Can be identified by **checksums (MD5)** — the binary stays the same even if renamed
- Leaves identifiable byte patterns that antivirus engines can detect

### Script-Based Malware (`.ps1`, `.vbs`, `.bat`, `.js`)
- Written in scripting languages, easier for attackers to modify on the fly
- Often runs **in memory** to avoid leaving files on disk
- Common red flags I now know to look for:
  - Downloads or executes files from the internet
  - Disables security tools or modifies system settings
  - Uses encoded/obfuscated content
  - Unexpectedly launches PowerShell or CMD

I also looked at a real obfuscated sample of **LummaStealer** — a C-based stealer sold as Malware-as-a-Service (MaaS). It used a huge base64-encoded variable that got decrypted and executed in memory via a .NET assembly, leaving no trace on disk. That was eye-opening.

---

## Key Takeaways

- Classifying malware correctly helps SOC analysts choose the right response
- The same malware can belong to multiple categories (e.g. RedLine is both a keylogger and data stealer)
- **PowerShell** is the most commonly abused tool for script-based malware delivery
- Malware families group related threats together — knowing one helps you defend against the whole family
- Script-based malware is flexible and stealthy; binary malware is stable and detectable by signature

---

*Room: [TryHackMe — Malware Classification](https://tryhackme.com/room/malwareclassification)*