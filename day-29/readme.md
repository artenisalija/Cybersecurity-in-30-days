# Day 29: TryHackMe — Cyber Kill Chain Room Writeup

## Overview

Today I completed the **Cyber Kill Chain** room on TryHackMe. This room walked me through the Lockheed Martin Cyber Kill Chain® framework — a model originally built on a military concept that breaks down a cyberattack into structured phases. The core idea is that an adversary must successfully complete **every** phase of the chain to achieve their goal, which means defenders only need to break one link to stop the attack.

---

## What I Learned

### Phase 1 — Reconnaissance
The attacker (nicknamed "Megatron" in the scenario) starts by gathering as much information as possible about the target. This is called **OSINT (Open-Source Intelligence)**. I learned about tools like:

- **theHarvester** — collects emails, subdomains, IPs, and URLs from public sources
- **Hunter.io** — finds contact info associated with a domain
- **OSINT Framework** — a web-based interface that organizes OSINT tools by category *(this was the answer to the "web-based interface" question)*

I also learned that **email harvesting** is the process of obtaining email addresses from public or paid services, often used to set up phishing attacks.

---

### Phase 2 — Weaponization
Here the attacker builds or buys a payload. I learned the key definitions:

- **Malware** — software designed to damage, disrupt, or gain unauthorized access
- **Exploit** — code that takes advantage of a vulnerability
- **Payload** — the malicious code the attacker runs on the system

Attackers often embed malicious **macros** (a group of commands that automate tasks) inside Microsoft Office documents to trick victims into executing code.

---

### Phase 3 — Delivery
This phase is about how the payload reaches the victim. I learned about three main delivery methods:

1. **Phishing / Spearphishing** — crafting fake emails targeting specific individuals
2. **USB Drop Attacks** — planting infected drives in public places
3. **Watering Hole Attack** — compromising a website that the target group frequently visits, then silently delivering malware through a **drive-by download**

---

### Phase 4 — Exploitation
Once delivered, the attacker exploits a vulnerability to gain access. I learned about the **Zero-day exploit** — an attack that targets an unknown vulnerability that software vendors haven't had any time (zero days) to patch or detect.

Other exploitation methods include:
- Victim clicking a malicious link or attachment
- Exploiting hardware or human vulnerabilities
- Server-based vulnerability exploitation

---

### Phase 5 — Installation
After gaining access, the attacker needs to maintain it. I learned about **persistence** techniques:

- **Web shells** — malicious scripts (`.php`, `.asp`, `.jsp`, etc.) planted on a web server to allow remote access
- **Backdoors** — installed via tools like Meterpreter (a Metasploit payload)
- **Modifying Windows services** or **Registry Run Keys** to execute payloads on startup
- **Timestomping** — modifying file timestamps to hide malicious activity from forensic investigators

---

### Phase 6 — Command & Control (C2)
The attacker establishes a communication channel back to the compromised machine. I learned that modern C2 channels blend into normal traffic using:

- **HTTP/HTTPS (ports 80/443)** — mimics legitimate web traffic
- **DNS Tunneling** — the infected machine makes constant DNS requests to an attacker-controlled server, also called **DNS Tunneling**

---

### Phase 7 — Actions on Objectives (Exfiltration)
The final phase — the attacker achieves their goal. This can include:

- Stealing credentials
- Privilege escalation
- Lateral movement through the network
- Exfiltrating sensitive data
- Deleting **Shadow Copies** (a Microsoft Windows technology that creates backup snapshots of files/volumes) to prevent recovery
- Overwriting or corrupting data

---

## Practical — Target Data Breach (2013)

I applied the Kill Chain to the real-world **Target data breach of 2013**, one of the largest in history (~40 million card accounts compromised). I mapped the following techniques to each phase:

| Kill Chain Phase     | Technique Used                   |
|----------------------|----------------------------------|
| Weaponization        | PowerShell                       |
| Delivery             | Spearphishing Attachment         |
| Exploitation         | Exploit Public-Facing Application|
| Installation         | Dynamic Linker Hijacking         |
| Command & Control    | Fallback Channels                |
| Exfiltration         | Data from Local System           |

---

## Key Takeaways

- The Cyber Kill Chain is a powerful mental model for understanding attacker behavior and identifying where defenses can be strengthened.
- It was designed in **2011** and hasn't been updated since, meaning it has gaps — particularly around **insider threats**.
- It's best used **alongside** other frameworks like **MITRE ATT&CK** and the **Unified Kill Chain** for a more complete defensive picture.
- As a defender, I only need to **break one link** in the chain to stop an attack — that's the real power of this framework.

---

*Completed on TryHackMe | Cyber Kill Chain Room*