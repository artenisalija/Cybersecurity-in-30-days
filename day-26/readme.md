# Day 27: Threat Hunting Introduction — TryHackMe Learning Journal

## What I Learned

Working through the **Threat Hunting Introduction** module on TryHackMe gave me a solid baseline understanding of what threat hunting actually is, how it differs from incident response, and why organisations need it as a core part of their security posture.

---

## Core Concept: Threat Hunting vs. Incident Response

One of the first things that clicked for me was the contrast between threat hunting and incident response. I used to think of them as similar, but they're fundamentally different in their orientation:

- **Incident Response** is *reactive* — a team waits for an alert, triages it, analyses it, and then responds once enough evidence points to malicious activity.
- **Threat Hunting** is *proactive* — there's no trigger. I'm the one going out to look for threats, not waiting for the system to tell me something's wrong.

What I found most valuable here is understanding *why* threat hunting exists alongside IR. Even with solid detection systems in place, advanced threats and well-executed red team exercises can slip through undetected. Threat hunting is how I fill that gap.

---

## The Threat Hunting Mindset

I learned that every hunt needs to start with a strong basis — it can't just be aimless searching. The foundation is **Threat Intelligence**, which shapes what I'm looking for and how I look for it.

There are two main flavours of intel I can draw from:

- **Unique/Internal Intelligence** — IOCs from previous intrusions within my own organisation. These are gold because they're actual traces of real adversaries that have targeted *me* specifically.
- **Threat Intelligence Feeds** — For organisations that can't develop their own intel, external feeds like MISP or Recorded Future provide a practical starting point.

The key insight: understanding a threat lets me know *how* to hunt for it.

---

## The Threat Hunting Process

I now have a clearer picture of what actually drives a hunt. The main starting points I learned to use are:

- **Known Relevant Malware** — Identifying threat actors that might target my organisation and hunting for traces of their toolkits. Resources like theZoo and Trend Micro's Threat Encyclopedia are useful here.
- **Attack Residues** — If I suspect an attack has already happened, looking for the leftovers is a practical first step.
- **Known Vulnerabilities** — Actively hunting for unpatched vulnerabilities in assets in use, and checking for signs they've already been exploited. This also means staying sharp on zero-day announcements.
- **Attack Signatures and IOCs** — Turning threat subjects into specific, actionable identifiers I can recognise.
- **Patterns of Activity** — Using frameworks like the **MITRE ATT&CK Matrix** to characterise adversary behaviour and build hunt hypotheses around it.

---

## Practical Application: MITRE ATT&CK Navigator

The hands-on section had me working with the MITRE ATT&CK Navigator, comparing techniques across three threats: **WannaCry**, **Conficker**, and **Stuxnet**. A few things I took away:

- The **Discovery** tactic had the most highlighted techniques across the threats.
- All three shared **Exploitation of Remote Services** as a common technique.
- WannaCry and Conficker both used **Inhibit System Recovery**.
- Stuxnet and Conficker shared **6 techniques** in common (score of 2 + 4).

This exercise drove home how powerful the ATT&CK matrix is for visualising overlap between threat actors — a technique I'll keep using in future hunts.

---

## Why It Matters: The Goals of Threat Hunting

Going through the goals section helped me understand the *why* behind all of this:

1. **Proactively finding threats** — Malicious actors are a constant. Finding them before they complete their objectives is always the goal.
2. **Minimising dwell time** — The longer an attacker is undetected, the more damage they can do to confidentiality, integrity, and availability. Reducing dwell time directly reduces impact.
3. **Developing better detection** — Every hunt feeds back into improving continuous monitoring. I'm not just catching threats — I'm making the detection pipeline smarter over time.

---

## Key Takeaways

- Threat hunting is **intelligence-driven** and **proactive** — it's not waiting, it's seeking.
- Strong **Threat Intelligence**, whether internal or from feeds, is what separates a focused hunt from a guessing game.
- The **MITRE ATT&CK Matrix** is an essential tool for mapping adversary behaviour and identifying patterns.
- Every hunt should ultimately improve the organisation's **security posture** and detection capabilities — it's a continuous cycle, not a one-off exercise.

---

*Module completed as part of TryHackMe's Threat Hunting learning path.*