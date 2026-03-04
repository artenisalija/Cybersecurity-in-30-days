# Day 24 📡 WiFi Hacking – Learning Notes
> Module: Cracking WiFi Security with Aircrack-ng and HCXTools

---

## 🔑 Key Terms

| Term | Meaning |
|------|---------|
| **SSID** | The network name you see when connecting to WiFi |
| **ESSID** | An SSID that may span multiple access points (e.g. a corporate network) |
| **BSSID** | The MAC (hardware) address of an access point |
| **WPA2-PSK** | WiFi secured by a single shared password for all users |
| **WPA2-EAP** | WiFi that authenticates via username/password sent to a RADIUS server |
| **RADIUS** | A server used to authenticate clients (not just for WiFi) |

---

## 🤝 The 4-Way Handshake

The core of WPA/WPA2 authentication is the **4-way handshake**. It allows both the client and the access point to prove they know the password — *without actually sending it to each other*.

- WPA and WPA2 use practically the same authentication method, so attacks work on both.
- Keys are derived from **both the ESSID and the password**, meaning the ESSID acts as a **salt**.
- This makes precomputed dictionary attacks harder — the key changes per access point even with the same password.

---

## 📜 A Brief History: WEP → WPA

- **WEP** (Wired Equivalent Privacy) was the old standard — now considered broken.
- WEP can be cracked by capturing enough packets and using statistical analysis.
- **WPA/WPA2** replaced WEP with much stronger authentication.

---

## 🛠️ The Aircrack-ng Suite

A collection of tools for auditing WiFi networks. Key tools for WPA attacks:

| Tool | Purpose |
|------|---------|
| `airmon-ng` | Enables/disables monitor mode on a NIC |
| `airodump-ng` | Captures packets and records nearby networks |
| `aireplay-ng` | Injects packets (e.g. deauth attacks to force a handshake) |
| `aircrack-ng` | Cracks the captured handshake using a wordlist |

> **Install:** Comes pre-installed on Kali Linux, or via `https://www.aircrack-ng.org/`

### Other tools in the suite:
`airdecap-ng`, `airtun-ng`, `packetforge-ng`, `airbase-ng`, `airdecloak-ng`, `airolib-ng`, `airserv-ng`, `buddy-ng`, `ivstools`, `easside-ng`, `tkiptun-ng`, `wesside-ng`

---

## 📶 Requirements for Capturing a Handshake

1. A **monitor mode-capable NIC** (not all wireless cards support this)
2. Optional but helpful: **Injection mode** — lets you send a deauth packet to force a client to reconnect, triggering the handshake instead of waiting

---

## 🔓 Cracking the Password

Once you have a captured handshake (`.cap` file), you can crack it:

- **Aircrack-ng** — CPU-based cracking using a wordlist
- **Hashcat** — GPU-accelerated cracking (convert capture to hashcat format first)
  - Use the **3.6+ hashcat output format** for compatibility with recent versions

### Practice Command (generate random passwords from rockyou):
```bash
head /usr/share/wordlists/rockyou.txt -n 10000 | shuf -n 5 -
```

### Practice Lab Setup:
- Create a hotspot on a phone/tablet
- Set a weak password (from `rockyou.txt`)
- Capture the handshake with `airodump-ng`
- Crack it with `aircrack-ng`

---

## 🧪 Practice Target Info

```
BSSID:  02:1A:11:FF:D9:BD
ESSID:  James Honor 8
```

---

## ⚠️ Ethics Note

This knowledge is intended for **ethical hacking and penetration testing only**. Always:
- Get **written permission** before testing any network
- Only practice on networks you **own or have authorisation to test**
- Use this knowledge to **defend**, not attack

---
