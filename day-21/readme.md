# Day 21 TryHackMe — Intro to Detection Engineering Walkthrough

> **Room URL:** https://tryhackme.com/room/introtodetectionengineering

---

## Task 1 — Introduction

Detection engineering is an important role and task for a security analyst. It involves developing processes that guide analysts to identify threats, detect them through rules and processes, and fine-tune the process as the landscape changes.

**Learning Objectives:**
- Understand what Detection Engineering is
- Understand the Detection Engineering Lifecycle
- Identify various frameworks used in Detection Engineering

---

## Task 2 — What is Detection Engineering?

Detection engineering is the **continuous process of building and operating threat intelligence analytics** to identify potentially malicious activity or misconfigurations that may affect your environment.

### Detection Types

| Type | Description |
|------|-------------|
| **Configuration Detection** | Uses knowledge of the known environment to identify misalignments in network, asset, or identity configurations |
| **Modelling** | Defines baseline operations and flags any deviations from normal behaviour |
| **Indicator Detection** | Uses IOCs (Indicators of Compromise) derived from investigations against malicious events |
| **Threat Behaviour Detection** | Focuses on adversary TTPs (Tactics, Techniques & Procedures) regardless of specific indicators |

### Detection as Code (DaC)

DaC is a structured approach incorporating software engineering best practices into detection processes. Key benefits include:

- **Version Control** — Track changes to detection rules and processes
- **Automation workflows** — CI/CD pipeline for quick production delivery
- **Customisable Detections** — Vendor-agnostic using Sigma and YARA
- **Test-Driven Development** — Identify blind spots and false positives early
- **Team Collaboration** — Eliminates isolation between security teams
- **Code Reusability** — Reuse detection patterns across different detections

### ✅ Task 2 Answers

| Question | Answer |
|----------|--------|
| Which detection type focuses on misalignments within the current infrastructure? | `Configuration` |
| Which detection approach involves building an asset or activity baseline profile? | `Modelling` |
| Which type of detection integrates with defensive playbooks? | `Threat Behaviour` |

---

## Task 3 — Detection Engineering Methodologies

The detection engineering lifecycle follows these key phases:

1. **Detection Gap Analysis** — Identify areas for improvement through reactive (incident review) or proactive (ATT&CK framework) threat modelling
2. **Datasource Identification & Log Collection** — Map relevant data sources to identified threats
3. **Baseline Creation** — Define normal behaviour across devices and systems (high-level and technical baselines)
4. **Log Collection** — Aggregate logs using network sensors and host tools like Sysmon
5. **Rule Writing** — Write and test detection rules (Snort for network, YARA for files, Sigma for logs)
6. **Deployment, Automation & Tuning** — Deploy to production and continuously refine detections

---

## Task 4 — Detection Engineering Frameworks 1

### MITRE ATT&CK & CAR
- **ATT&CK** — Maps adversarial actions across platforms (Windows, macOS, Linux, Mobile)
- **CAR (Cyber Analytics Repository)** — Detects and prioritises adversary behaviours based on ATT&CK

### Pyramid of Pain
Illustrates how difficult it is for adversaries to change their TTPs when each layer is detected. Higher layers = more pain for the attacker.

### Cyber Kill Chain (Lockheed Martin)
Seven phases of a cyberattack:
1. Reconnaissance
2. Weaponisation
3. Delivery
4. Exploitation
5. Installation
6. Command & Control
7. Actions on Objectives

### Unified Kill Chain
An improved version combining the Cyber Kill Chain with MITRE ATT&CK, expanding to **18 phases** to cover every element of a cyberattack.

### ✅ Task 4 Answers

| Question | Answer |
|----------|--------|
| Which framework looks at how to make it difficult for an adversary to change their approach when detected? | `Pyramid of Pain` |
| What is the improved Cyber Kill Chain framework called? | `The Unified Kill Chain` |
| How many phases are in the improved kill chain? | `18` |

---

## Task 5 — Detection Engineering Frameworks 2

### ADS (Alerting and Detection Strategy) Framework
Developed by Palantir to address alert fatigue. Detection engineers must follow this strict flow before publishing rules:

| Stage | Description |
|-------|-------------|
| **Goal** | Intended reasons for the alert and behaviour to detect |
| **Categorisation** | Mapping to MITRE ATT&CK TTPs |
| **Strategy Abstract** | Top-level description of the detection strategy |
| **Technical Context** | Technical environment details for analysts |
| **Blind Spots & Assumptions** | Where the strategy may fail or be bypassed |
| **False Positives** | Known non-malicious triggers |
| **Validation** | Steps to produce a true-positive and verify the alert |
| **Priority** | Alerting levels and criteria |
| **Response** | Triage and investigation guidance |

### Detection Maturity Level (DML) Model
Developed by Ryan Stillions in 2014. Nine levels (0–8) assessing an organisation's ability to apply threat intelligence to detection:

| Level | Name | Description |
|-------|------|-------------|
| DML-8 | Goals | Detect adversary motives |
| DML-7 | Strategy | Detect adversary intentions |
| DML-6 | Tactics | Detect tactics without knowing specific techniques |
| DML-5 | Techniques | Detect specific APT techniques |
| DML-4 | Procedures | Detect sequences of adversary events |
| DML-3 | Tools | Detect adversarial tooling |
| DML-2 | Host & Network Artefacts | Gather IOCs and artefacts |
| DML-1 | Atomic Indicators | Use threat intel feeds (IPs, domains) |
| DML-0 | None | No detection processes established |

---

## Task 6 — Detection Detective

**Scenario:** Develop a detection strategy for changes made to privileged and administrative groups/accounts in Active Directory.

### ADS Framework Answers

- **Categorisation:** `Account Manipulation` — monitoring changes to high-privilege user accounts (e.g., Domain Admins)
- **Strategy Abstract:** Collect Windows Event Logs to detect changes to administrative groups in Active Directory
- **Response:** Validate the group modified → validate the user added → validate the user making the change

### 🚩 Flag

```
THM{Sup3r-D3t3ct1v3}
```

