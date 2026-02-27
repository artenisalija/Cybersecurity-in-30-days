# 🦈 Day 20 - Wireshark for Malware Analysis

---

## 🧭 Where I Started

I knew malware did *something* on the network — beaconing, exfiltrating data, phoning home — but I had no idea how to actually observe it in action. I decided to pick up Wireshark as my window into that world. This README is the guide I wish I'd had when I started.

---

## 🏗️ Step 1: Setting Up a Safe Environment

The first thing I learned (the hard way, mentally at least) is that you should **never** run malware on your real machine. I set up an isolated VM using VirtualBox with host-only networking so any malware I analyzed couldn't reach the real internet or my host system.

My setup checklist before every analysis session:
- Spin up a clean VM snapshot
- Set networking to host-only or isolated mode
- **Start Wireshark before executing the sample** — I missed early traffic the first few times by launching it too late

---

## 🎣 Step 2: Learning Capture Filters

Once I started capturing, I quickly got overwhelmed by noise. I learned that **capture filters** limit what Wireshark records in the first place. A few I use regularly:

```
host 192.168.1.10          # traffic to/from my malware VM
port 80 or port 443        # focus on web traffic only
not arp                    # cut out the ARP chatter
```

Capture filters use BPF (Berkeley Packet Filter) syntax, which took me a little while to get comfortable with — but once it clicked, it made captures much more manageable.

---

## 🔍 Step 3: Mastering Display Filters

This is where I spent most of my time. Display filters are applied *after* capture and are incredibly powerful for hunting through traffic. Here are the ones that became second nature to me:

```
dns                          # all DNS queries — my first stop every time
http                         # plaintext HTTP requests/responses
tcp.flags.syn == 1           # spot connection attempts and port scanning
ip.addr == 93.184.216.34     # zoom in on a suspicious IP
http.request.method == "POST" # look for data being sent outbound
```

The moment I started filtering for `dns` first, my analysis got so much faster. Malware almost always makes DNS lookups, and those domain names are often the most obvious red flag.

---

## 🚩 Step 4: Knowing What to Look For

After a few samples, I started developing an eye for suspicious patterns:

**DNS Queries** — I look for lookups to strange, algorithmically-generated domains (DGA). Something like `xkq72ms.top` at a regular interval is a huge red flag compared to normal traffic hitting `google.com`.

**Beaconing** — I learned to use `Statistics > Conversations` to spot repeated connections to the same external IP at suspiciously regular intervals. Malware often checks in with its C2 server on a timer.

**Unusual Ports** — Legitimate software rarely uses random high-numbered ports for outbound connections. I always flag those for investigation.

**Data Exfiltration** — Large outbound data transfers, especially to IPs I don't recognize, are worth investigating immediately.

**Raw TCP/IRC connections** — Older malware families love IRC-based C2. Seeing a raw TCP connection to port 6667 on an external IP was eye-opening the first time I spotted it.

---

## 🔧 Step 5: The Features I Use Most

**Follow TCP/UDP Stream** — Right-clicking a packet and selecting "Follow > TCP Stream" was a game changer. It reassembles the entire conversation so I can read exactly what was exchanged. I've seen C2 check-in messages, stolen credentials, and command responses this way.

**Statistics > Conversations** — Gives me a bird's-eye view of every unique connection. I sort by bytes to find the noisiest ones first.

**Statistics > DNS** — A summary of all DNS activity in the capture. Great for spotting DGA traffic at a glance.

**Export Objects** — Under `File > Export Objects`, I can extract files transferred over HTTP or SMB. I've pulled down malware droppers and staged payloads this way.

**Protocol Hierarchy** — `Statistics > Protocol Hierarchy` shows me what mix of protocols appeared. Seeing something unexpected in the hierarchy (like a lot of IRC or raw TCP) immediately draws my attention.

---

## 🔐 Step 6: Tackling Encrypted Traffic

This was a frustrating wall I hit early on. Most modern malware uses HTTPS/TLS, and Wireshark can't decrypt it without the session keys.

My workaround was to redirect traffic through **mitmproxy** running on the same isolated network. With a trusted cert installed in the VM, I could intercept and read the TLS traffic in plaintext. It's not perfect — some malware does certificate pinning — but it works a surprising amount of the time.

For browser-based traffic, I also learned about the `SSLKEYLOGFILE` environment variable, which logs TLS session keys that Wireshark can then use to decrypt.

---

## 🧰 Step 7: Tools That Made Wireshark Even Better

Wireshark doesn't work in isolation. I built a toolkit around it:

- **FakeNet-NG** — Simulates network services (DNS, HTTP, etc.) to trick malware into thinking it's reaching the internet. Pairs perfectly with Wireshark to capture the resulting traffic.
- **INetSim** — Similar to FakeNet, great for Linux-based analysis environments.
- **Zeek (formerly Bro)** — I use this for scripted analysis on saved `.pcap` files. It produces structured logs that are easier to query than raw packet data.
- **NetworkMiner** — Opened a whole new way of looking at pcaps for me. It automatically extracts hosts, files, credentials, and more from a capture.
- **VirusTotal / AbuseIPDB** — After identifying suspicious IPs or domains in Wireshark, I cross-reference them here to see if others have flagged them.

---

## 🔄 My Analysis Workflow (Today)

After going through enough samples, my process has settled into this:

1. **Snapshot the VM** and start Wireshark
2. **Execute the sample** and let it run for a few minutes
3. **Filter for `dns`** — scan all domain lookups first
4. **Check `Statistics > Conversations`** for beaconing or large transfers
5. **Follow streams** on anything suspicious
6. **Cross-reference IPs/domains** with threat intel
7. Save the `.pcap` for further analysis with Zeek or NetworkMiner

---

## 💡 Things I Wish I Knew Earlier

- Always start Wireshark *before* running the sample. You will miss things otherwise.
- DNS is almost always the first breadcrumb. Start there.
- Don't ignore UDP — some C2 channels use it to avoid TCP connection logging.
- A quiet capture can be as suspicious as a noisy one. Malware sometimes only phones home once.
- Save every `.pcap`. You'll want to re-examine them later with fresh eyes or new tools.

---

## 📚 What's Next for Me

- Getting deeper into TLS decryption techniques
- Learning to write custom Zeek scripts for automated IOC extraction
- Exploring JA3/JA3S fingerprinting for identifying malware TLS clients
- Building a proper lab with RITA for long-term traffic analysis

---

*This is a living document — I update it as I learn more. If you're on a similar journey, I hope it saves you some of the time I spent figuring this out.*