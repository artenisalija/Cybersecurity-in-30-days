# Day 22: TryHackMe WAF Introduction

I completed the [TryHackMe WAF: Introduction](https://tryhackme.com/room/wafintroduction) room, which walked me through the basics of Web Application Firewalls — what they are, how they work, and how they differ from regular firewalls. Here's a breakdown of what I learned, step by step.

---

## Section 1 — Firewall Fundamentals

**What I learned:** Not all firewalls are equal. Stateless firewalls can be tricked using the ACK flag, because they don't track the state of a connection — they just check rules on individual packets. Stateful firewalls, on the other hand, track active connections, making them harder to bypass.

- **Q: Which flag can attackers misuse to bypass a stateless firewall?**
  **A: ACK**

- **Q: What type of firewall tracks active connections?**
  **A: Stateful**

---

## Section 2 — What Makes a WAF Different

**What I learned:** A WAF operates at Layer 7 (the Application Layer), meaning it actually understands HTTP traffic — not just packets. This lets it do things like block SQL injection attempts in a web form. One powerful concept I picked up here is *virtual patching* — you can mitigate a vulnerability at the WAF level without touching the actual application source code.

- **Q: Which layer does a WAF primarily inspect?**
  **A: Layer 7**

- **Q: What do you call mitigating vulnerabilities without source code changes?**
  **A: Virtual patching**

- **Q: What HTTP status code do you get if you attempt an SQLi against the target?**
  **A: 403 Forbidden**

---

## Section 3 — Detection Methods

**What I learned:** There are two main ways a WAF detects attacks. Signature-based detection matches traffic against known attack patterns — it's reliable but can't catch what it hasn't seen before. Behavioural detection fills that gap: it can identify zero-day attacks that signature-based detection would miss, because it looks for anomalous behaviour rather than known patterns.

- **Q: Which detection method uses known attack patterns?**
  **A: Signature-Based**

- **Q: What kind of attacks can behavioural detection catch that signature-based cannot?**
  **A: Zero-day**

---

## Section 4 — OWASP Core Rule Set (CRS)

**What I learned:** OWASP CRS is a widely used ruleset for WAFs. I learned that it uses a library called *libinjection* specifically to detect SQL injection. I also learned about transformations — for example, `urlDecodeUni` handles double URL encoding, which attackers use to sneak past filters. Rules have a scoring system: when a critical rule triggers, it raises the *Anomaly Score*, and if that score crosses a threshold, the request gets blocked.

I also logged into a live dashboard and traced a real rule trigger (rule 932160 — a path traversal attempt) back to its exact URI.

- **Q: Which library does OWASP CRS use to detect SQLi?**
  **A: libinjection**

- **Q: What transformation handles double URL encoding?**
  **A: urlDecodeUni**

- **Q: What score is increased when a critical rule triggers?**
  **A: Anomaly Score**

- **Q: What URI triggered rule 932160 at 2025-11-12 15:38:00?**
  **A: `/?parameter=../../../etc/passwd`**

---

## Section 5 — WAF Limitations

**What I learned:** A WAF can't inspect what it can't see. If HTTPS traffic is not terminated at the WAF, the encryption makes the payload invisible — the WAF just sees an encrypted blob and can't do anything with it. This is a key deployment consideration.

- **Q: What prevents WAFs from inspecting traffic if not terminated at the WAF?**
  **A: Encryption**

---

## Section 6 — WAFs in a Defence Strategy

**What I learned:** A WAF is not a silver bullet. The guiding principle for using one correctly is *defence in depth* — layering multiple security controls so that no single point of failure can compromise the whole system.

- **Q: What principle should guide WAF use in a defence strategy?**
  **A: Defence in depth**

---

## Summary

This room gave me a solid foundation in WAFs — from how they differ from traditional firewalls, to how they detect attacks, to how OWASP CRS works under the hood. The hands-on dashboard task made the scoring/anomaly system click for me in a way that just reading about it wouldn't have.