# Cybersecurity Learning Journey

**Date:** February 8th, 2026  
**Duration:** 1 Day

---

## TryHackMe - Start Your Cybersecurity Journey

### 1. [Offensive Security](https://tryhackme.com/room/offensivesecurityintro)
- Understanding attacker methodologies
- Penetration testing basics
- Ethical hacking principles

### 2. [Defensive Security](https://tryhackme.com/room/defensivesecurityintro)
- Security monitoring and incident response
- Threat detection techniques
- Security controls and best practices

### 3. [Search Skills](https://tryhackme.com/room/searchskills)
- Advanced search operators (`-site:`, `-"phrase"`, `filetype:`)
- OSINT techniques
- Essential tools: Shodan, VirusTotal, Have I Been Pwned, CVE Details, Exploit Database

---

## [Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentals)

### Core Commands
- `ls`, `cd`, `cat`, `pwd`, `grep`

### Operators
- `&` (background), `&&` (chain), `>` (redirect), `>>` (append)

---

## [OverTheWire Bandit - Levels 0-11](https://overthewire.org/wargames/bandit/)

### Skills Covered
- **File Operations:** Basic reading, special characters, spaces, hidden files
- **File Type Identification:** Using `file` command
- **Advanced Search:** `find` with multiple criteria
- **Text Processing:** `grep`, `awk`, `sort`, `uniq`
- **Binary Analysis:** `strings` command
- **Encoding:** Base64 decoding

### Key Commands Used
```bash
find / -type f -user <user> -group <group> -size <size> 2>/dev/null
grep 'pattern' file.txt
sort | uniq -u
strings binary | grep "text"
base64 -d file.txt
```

---

## Progress

✅ Offensive Security Introduction  
✅ Defensive Security Introduction  
✅ Search Skills  
✅ Linux Fundamentals Part 1  
✅ Bandit Levels 0-11