# 🛡️ Cybersecurity 0→100 Challenge — Complete

> **30 days. Red team. Blue team. Root.**  
> From zero to hands-on cybersecurity practitioner.

---

## 📋 Table of Contents

- [Challenge Overview](#challenge-overview)
- [Week 1 — Linux Mastery & Network Fundamentals](#week-1--linux-mastery--network-fundamentals)
- [Week 2 — Exploitation Fundamentals](#week-2--exploitation-fundamentals)
- [Week 3 — Privilege Escalation & Blue Team](#week-3--privilege-escalation--blue-team)
- [Week 4 — Advanced Topics & Real-World Skills](#week-4--advanced-topics--real-world-skills)
- [Tools Mastered](#tools-mastered)
- [Key Deliverables](#key-deliverables)
- [Certifications & Next Steps](#certifications--next-steps)

---

## Challenge Overview

| | |
|---|---|
| **Duration** | 30 Days |
| **Focus** | Red Team & Blue Team |
| **Labs Completed** | OverTheWire, VulnHub, DVWA, Juice Shop, HackTheBox, CyberDefenders, LetsDefend |
| **Machines Rooted** | 5+ VulnHub machines |
| **Reports Written** | 4 full penetration test reports |
| **Tools Built** | 6 custom Python security tools |

---

## Week 1 — Linux Mastery & Network Fundamentals

**Days 1–7** | *Building the foundation every hacker needs*

### ✅ Day 1 — Advanced Linux & Bash Scripting
- Mastered advanced file operations, `awk`, `sed`, `cut`, process management, and job control
- Completed OverTheWire Bandit Levels 0–10
- **Built:** File parser, system info gatherer, and log analyzer scripts

### ✅ Day 2 — Linux Permissions & SSH Hardening
- Deep-dived into SUID, SGID, sticky bit, user/group management, and SSH configuration
- Completed Bandit Levels 11–15
- **Built:** SSH hardening checklist; documented privilege escalation via misconfigured permissions

### ✅ Day 3 — Networking Fundamentals & Packet Analysis
- Studied OSI model, TCP/IP stack, subnetting, common ports, and protocol analysis
- Captured and analyzed 5 protocol types with Wireshark
- **Documented:** Traffic findings with annotated screenshots

### ✅ Day 4 — Active Reconnaissance & Network Scanning
- Mastered all Nmap scan types: SYN, UDP, version detection, OS fingerprinting, scripting engine
- Completed OverTheWire Natas Levels 0–5
- **Built:** Comprehensive lab network map + enumeration automation script

### ✅ Day 5 — Passive Reconnaissance & OSINT
- Practiced Google dorking, Shodan, theHarvester, WHOIS, and DNS enumeration
- **Delivered:** Full OSINT report on a public target + custom Google dork list

### ✅ Day 6 — Vulnerability Scanning & Analysis
- Used OpenVAS and Nikto to scan VulnHub machines (Kioptrix, Mr. Robot, Basic Pentesting 1)
- **Delivered:** Full vulnerability assessment report with risk ratings and remediation recommendations

### ✅ Day 7 — Web Application Recon & Directory Enumeration
- Configured Burp Suite, ran Gobuster/Dirbuster, analyzed robots.txt and sitemaps
- **Delivered:** Complete attack surface map of 2 web apps, including hidden endpoints

---

## Week 2 — Exploitation Fundamentals

**Days 8–14** | *From theory to shells*

### ✅ Day 8 — SQL Injection
- Exploited error-based, union-based, and blind SQLi in DVWA and OverTheWire Natas
- Used `sqlmap` for automation and manual techniques for depth
- **Built:** Detection regex; extracted full databases across 3 injection contexts

### ✅ Day 9 — Cross-Site Scripting (XSS)
- Exploited reflected, stored, and DOM-based XSS in DVWA, Google XSS Game, and Juice Shop
- Researched and applied filter bypass techniques
- **Built:** Payload cheatsheet with bypass methods for each XSS type

### ✅ Day 10 — Command Injection & File Inclusion
- Exploited OS command injection, LFI, RFI, path traversal, and log poisoning
- Achieved RCE through LFI chaining
- **Documented:** 5 distinct filter bypass methods

### ✅ Day 11 — Authentication & Session Attacks
- Performed brute force, credential stuffing, session hijacking, and JWT exploitation
- Researched 2FA bypass techniques
- **Built:** Password spraying script; bypassed authentication 3 different ways

### ✅ Day 12 — File Upload Vulnerabilities & Webshells
- Bypassed file upload restrictions using 5 methods: extension spoofing, magic bytes, double extensions, null bytes, MIME type manipulation
- Deployed functional PHP webshells and reverse shells
- **Built:** PHP reverse shell; documented full exploitation chain

### ✅ Day 13 — First Full Machine (VulnHub)
- Rooted **Kioptrix Level 1** using full OSCP-style methodology
- Chained recon → exploitation → privilege escalation
- **Delivered:** Professional pentest report with executive summary, technical findings, and PoC

### ✅ Day 14 — Metasploit Framework
- Exploited 5 services on Metasploitable 2 using MSFconsole
- Generated custom payloads with `msfvenom`
- **Delivered:** Full post-exploitation documentation including persistence and loot

---

## Week 3 — Privilege Escalation & Blue Team

**Days 15–21** | *From user to SYSTEM. Then detecting yourself.*

### ✅ Day 15 — Linux Privilege Escalation
- Escalated via SUID binaries, sudo misconfigurations, cron jobs, PATH hijacking, and kernel exploits
- Used LinPEAS for enumeration
- **Built:** Custom privesc enumeration checklist; documented 5 escalation vectors

### ✅ Day 16 — Windows Privilege Escalation
- Exploited unquoted service paths, DLL hijacking, scheduled tasks, and token impersonation
- Used WinPEAS for automated enumeration
- **Built:** PowerShell-based automated Windows enumeration script

### ✅ Day 17 — Password Cracking & Hash Analysis
- Cracked 10 hash types using John the Ripper and Hashcat
- Created custom wordlists and mutation rules
- **Analyzed:** Cracking speed benchmarks across attack modes

### ✅ Day 18 — Active Directory Basics & Attacks
- Set up a Windows Server AD lab; studied Kerberos and NTLM authentication
- Used PowerView and BloodHound for enumeration and attack path mapping
- **Delivered:** Domain trust relationship map with identified escalation paths

### ✅ Day 19 — Log Analysis & SIEM Basics
- Analyzed Linux `/var/log` and Windows Event Logs
- Worked with Splunk BOTS dataset and LetsDefend challenges
- **Built:** 5 functional Splunk queries; constructed attack timeline from logs

### ✅ Day 20 — Network Traffic Analysis & IDS
- Analyzed malicious PCAPs from malware-traffic-analysis.net
- Wrote custom Snort rules to detect attacks
- **Documented:** IOCs from 3 malicious PCAP samples

### ✅ Day 21 — Intermediate VulnHub Machine
- Rooted **Mr. Robot** — chained web exploitation, LFI, and Linux privesc
- **Delivered:** Full pentest report with attack chain diagram and per-vulnerability remediation

---

## Week 4 — Advanced Topics & Real-World Skills

**Days 22–30** | *The skills that separate professionals*

### ✅ Day 22 — WAF Bypass Techniques
- Detected WAFs and bypassed ModSecurity using encoding, case modification, and comment injection
- **Built:** Bypass payload collection for SQLi and XSS

### ✅ Day 23 — API Security Testing
- Tested Juice Shop and crAPI for IDOR, broken auth, rate limiting bypass, and excessive data exposure
- **Delivered:** API testing methodology document + vulnerability findings

### ✅ Day 24 — Wireless Security & Evil Twin Attacks
- Captured and cracked WPA2 handshakes on own network using aircrack-ng suite
- Studied deauth attacks and evil twin setup
- **Delivered:** Wireless attack methodology + detection strategy

### ✅ Day 25 — Malware Analysis & Reverse Engineering
- Performed static and dynamic analysis on 3 samples using REMnux and Ghidra
- **Documented:** IOCs, behavioral analysis, and C2 infrastructure for each sample

### ✅ Day 26 — Threat Hunting & Digital Forensics
- Applied MITRE ATT&CK framework to real datasets
- Completed CyberDefenders forensics challenges and LetsDefend scenarios
- **Delivered:** Threat hunt hypothesis + findings report

### ✅ Day 27 — Social Engineering & Physical Security
- Built phishing campaign infrastructure using SET
- Analyzed real-world phishing emails for TTPs
- **Delivered:** Awareness training content + detection methodology

### ✅ Day 28 — Building Custom Security Tools
- **Built 3 functional Python tools:**
  - `portscanner.py` — multithreaded port scanner
  - `vulnscan.py` — automated vulnerability scanner
  - `recon.py` — automated passive reconnaissance tool

### ✅ Day 29 — Advanced VulnHub Machine
- Completed a challenging multi-stage machine (Stapler/Breach/SickOS)
- Chained multiple vulnerabilities, performed lateral movement and multiple privesc steps
- **Delivered:** Professional-grade pentest report with full attack narrative

### ✅ Day 30 — Capstone & Portfolio
- Organized all scripts, reports, and write-ups into this repository
- Wrote blog posts for 3 best findings
- **You are here.**

---

## Tools Mastered

**Reconnaissance**
`nmap` · `theHarvester` · `Shodan` · `Gobuster` · `Dirbuster` · `Burp Suite` · `Recon-ng`

**Exploitation**
`Metasploit` · `msfvenom` · `sqlmap` · `SET` · `aircrack-ng` · `Hydra` · `Nikto`

**Post-Exploitation & Privesc**
`LinPEAS` · `WinPEAS` · `PowerView` · `BloodHound` · `Mimikatz`

**Blue Team & Analysis**
`Wireshark` · `Snort` · `Splunk` · `Ghidra` · `REMnux` · `Volatility`

**Scripting & Automation**
`Bash` · `Python` · `PowerShell`

---

## Key Deliverables

| Deliverable | Description |
|---|---|
| `reports/` | 4 full penetration test reports (professional format) |
| `scripts/` | 6+ custom security tools and automation scripts |
| `writeups/` | Machine write-ups for all VulnHub completions |
| `cheatsheets/` | XSS payloads, bypass techniques, privesc checklists |
| `pcap-analysis/` | Annotated malicious PCAP findings with IOCs |
| `blog/` | 3 published technical write-ups |

---

## Certifications & Next Steps

This challenge was built as preparation and proof of hands-on skills for:

- [ ] **eJPT** — eLearnSecurity Junior Penetration Tester
- [ ] **CompTIA Security+**
- [ ] **OSCP** — Offensive Security Certified Professional
- [ ] **CEH** — Certified Ethical Hacker

**Continue leveling up:**
- [HackTheBox](https://hackthebox.com) — ongoing machine practice
- [TryHackMe](https://tryhackme.com) — structured learning paths
- [PortSwigger Web Academy](https://portswigger.net/web-security) — advanced web exploitation
- [PentesterLab](https://pentesterlab.com) — code-level vulnerability understanding

---

> *"The quieter you become, the more you are able to hear."* — Kali Linux

**Challenge completed.** 🎯