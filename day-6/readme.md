# Day 6: Network Enumeration with Nmap - Hack The Box

## Overview
This document covers network enumeration techniques using Nmap for the Hack The Box Academy module on network scanning and reconnaissance.

## What is Nmap?
Nmap (Network Mapper) is a powerful open-source tool for network discovery and security auditing. It uses TCP/IP protocols to identify hosts, services, and potential vulnerabilities.

---

## Basic Concepts

### SYN Scan
- **SYN** = Short for "synchronize"
- Part of the TCP (Transmission Control Protocol) three-way handshake
- Default scan type when running Nmap as root

### Scanning Localhost
```bash
sudo nmap -sS localhost
```

---

## Host Discovery

### ICMP (Internet Control Message Protocol)
The most effective method for host discovery uses ICMP echo requests to determine if targets are alive.

### Scan Network Range
```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```

**Example:**
```bash
sudo nmap 220.158.183.14/24 -sn -oA tnet | grep for | cut -d" " -f5
```

### Scan Multiple IPs
```bash
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20 | grep for | cut -d" " -f5
```

**Output:**
```
10.129.2.18
10.129.2.19
10.129.2.20
```

### Scan IP Range
```bash
sudo nmap -sn -oA tnet 10.129.2.18-20 | grep for | cut -d" " -f5
```

### Scan from File
Use the `-iL` flag to perform scans against targets in a provided hosts list:
```bash
sudo nmap -iL hosts.lst
```

---

## Single Host Scanning

### Basic Host Check
```bash
sudo nmap 10.129.2.18 -sn -oA host
```

### With Packet Tracing
```bash
sudo nmap 10.129.2.49 -sn -oA host -PE --packet-trace
```

### Check Why Host is Alive
```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --reason
```

### Disable ARP Ping
To use ICMP echo requests instead of ARP:
```bash
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

**Sample Output:**
```
SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28]
RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28]
```

**Note:** TTL Value = 128 (typically indicates Windows system)

---

## Port Scanning

### TCP Port Scanning

#### Default Behavior
- **As root:** SYN scan (`-sS`) - default
- **As user:** TCP connect scan (`-sT`) - default
- **Default ports:** Top 1000 TCP ports

#### Scan Top 10 Ports
```bash
sudo nmap 10.129.2.210 --top-ports=10
```

#### Define Specific Ports
```bash
# Individual ports
sudo nmap -p 22,25,80,139,445 <target>

# Port range
sudo nmap -p 22-445 <target>

# All ports
sudo nmap -p- <target>

# Fast scan (top 100 ports)
sudo nmap -F <target>
```

#### Trace Packets
Disable ICMP, DNS, and ARP for cleaner output:
```bash
sudo nmap 10.129.2.210 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

**Sample Output:**
```
CONN (0.0385s) TCP localhost > 10.129.2.28:443 => Operation now in progress
CONN (0.0396s) TCP localhost > 10.129.2.28:443 => Connected

PORT    STATE SERVICE REASON
443/tcp open  https   syn-ack
```

---

## Filtered Ports

### Understanding Filtered Ports
Ports may be filtered due to firewall rules that:
- **Drop** packets (no response)
- **Reject** packets (ICMP response)

### Dropped Packets Example
```bash
sudo nmap 10.129.2.28 -p 139 --packet-trace -n --disable-arp-ping -Pn
```
- Nmap sends 2 SYN packets (retry)
- Scan takes ~2.11s (vs ~0.05s for open ports)
- No response = filtered

### Rejected Packets Example
```bash
sudo nmap 10.129.2.28 -p 445 --packet-trace -n --disable-arp-ping -Pn
```
- Firewall actively rejects connection
- ICMP response received

---

## UDP Port Scanning

### Why UDP is Different
- UDP is stateless (no three-way handshake)
- No acknowledgment received
- Much slower than TCP scans
- Often forgotten by administrators

### Basic UDP Scan
```bash
sudo nmap 10.129.2.28 -F -sU
```

### UDP Scan with Packet Trace
```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason
```

### Understanding UDP Responses
- **ICMP error code 3** (port unreachable) = port is **closed**
- **No response** or other ICMP = port is **open|filtered**

**Closed Port Example:**
```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 100 --reason
```

**Open/Filtered Port Example:**
```bash
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 138 --reason
```

---

## Service and Version Detection

### Version Scan
Get detailed information about services:
```bash
sudo nmap 10.129.2.28 -Pn -n --disable-arp-ping --packet-trace -p 445 --reason -sV
```

This identifies:
- Service versions
- Service names
- Additional details about the target

---

## Hack The Box Challenges

### Challenge 1: Find All TCP Ports
**Question:** Find all TCP ports on your target. Submit the total number of found TCP ports as the answer.

**Solution:**
```bash
nmap 10.129.178.201
```

### Challenge 2: Enumerate Hostname
**Question:** Enumerate the hostname of your target and submit it as the answer (case-sensitive).

**Solution:**
```bash
sudo nmap -sV 10.129.178.201
```

**Answer:** `NIX-NMAP-DEFAULT`

### Challenge 3: Full TCP Scan with HTML Report
**Question:** Perform a full TCP port scan on your target and create an HTML report. Submit the number of the highest port as the answer.

**Solution:**
```bash
nmap 10.129.2.49 -oA target
```

**Answer:** `31337`

---

## Key Nmap Flags Reference

| Flag | Description |
|------|-------------|
| `-sS` | SYN scan (stealth scan) |
| `-sT` | TCP connect scan |
| `-sU` | UDP scan |
| `-sV` | Version detection |
| `-sn` | Ping scan (no port scan) |
| `-Pn` | Skip host discovery |
| `-n` | No DNS resolution |
| `-p` | Specify ports |
| `-p-` | All ports |
| `-F` | Fast scan (top 100 ports) |
| `--top-ports=N` | Scan top N ports |
| `-oA` | Output all formats |
| `--packet-trace` | Show packets sent/received |
| `--disable-arp-ping` | Disable ARP ping |
| `--reason` | Show reason for port state |
| `--max-retries` | Set retry rate |
| `-iL` | Input from file |

---

## Best Practices

1. **Always use sudo** for SYN scans (requires raw socket permissions)
2. **Disable unnecessary features** when troubleshooting (ARP, DNS, ICMP)
3. **Use packet tracing** to understand what's happening
4. **Save outputs** using `-oA` for later reference
5. **Start with host discovery** before port scanning
6. **Remember UDP scanning** is slower but important
7. **Check filtered ports** - they might reveal firewall rules

---

## Notes

- **TTL = 128** typically indicates a Windows system
- **TTL = 64** typically indicates a Linux/Unix system
- Default retry rate: `--max-retries=1`
- UDP scans are significantly slower than TCP scans
- Firewall rules can drop or reject packets differently

---

## Conclusion

Nmap is an essential tool for network reconnaissance and security assessments. Understanding the different scan types, flags, and how to interpret results is crucial for effective network enumeration.

**Day 6 Complete! 🎯**