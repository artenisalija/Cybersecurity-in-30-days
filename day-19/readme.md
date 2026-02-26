# Day 19 - ryHackMe: Intro to Log Analysis — Walkthrough

A task-by-task walkthrough of the TryHackMe "Intro to Log Analysis" room, covering log investigation theory, detection engineering, command-line tools, regex, CyberChef, and Yara/Sigma.

> **Note:** Try to solve the room yourself first. Use this only if you're stuck.

---

## Table of Contents

- [Task 3 — Investigation Theory](#task-3--investigation-theory)
- [Task 4 — Detection Engineering](#task-4--detection-engineering)
- [Task 5 — Automated vs. Manual Analysis](#task-5--automated-vs-manual-analysis)
- [Task 6 — Log Analysis Tools: Command Line](#task-6--log-analysis-tools-command-line)
- [Task 7 — Log Analysis Tools: Regular Expressions](#task-7--log-analysis-tools-regular-expressions)
- [Task 8 — Log Analysis Tools: CyberChef](#task-8--log-analysis-tools-cyberchef)
- [Task 9 — Log Analysis Tools: Yara and Sigma](#task-9--log-analysis-tools-yara-and-sigma)

---

## Task 3 — Investigation Theory

Covers timelines, data visualisation, and threat intel concepts.

| # | Question | Answer |
|---|----------|--------|
| Q1 | What's the term for a consolidated chronological view of logged events from diverse sources? | `Super Timeline` |
| Q2 | Which threat intelligence indicator would `5b31f93c09ad1d065c0491b764d04933` and `763f8bdbc98d105a8e82f36157e98bbe` be classified as? | `File Hashes` |

---

## Task 4 — Detection Engineering

Covers common Linux log file locations, suspicious behaviour patterns, and attack signatures.

| # | Question | Answer |
|---|----------|--------|
| Q1 | What is the default file path for HTTP request logs on an Nginx server? | `/var/log/nginx/access.log` |
| Q2 | A log entry containing `%2E%2E%2F%2E%2E%2Fproc%2Fself%2Fenviron` was identified. What kind of attack might this infer? | `Path Traversal` |

**Tip:** `%2E` = `.` and `%2F` = `/` in URL encoding. Decoded: `../../proc/self/environ` — a classic directory traversal attempt.

---

## Task 5 — Automated vs. Manual Analysis

| # | Question | Answer |
|---|----------|--------|
| Q1 | A log file is processed by a tool which returns an output. What form of analysis is this? | `Automated` |
| Q2 | An analyst opens a log file and searches for events. What form of analysis is this? | `Manual` |

---

## Task 6 — Log Analysis Tools: Command Line

Working with `apache.log` using command-line tools (`cut`, `awk`, `grep`, `sort`, `uniq`, `wc`).

| # | Question | Answer |
|---|----------|--------|
| Q1 | Use `cut` on `apache.log` to return only URLs. What is the flag in one of the unique entries? | `c701d43cc5a3acb9b5b04db7f1be94f6` |
| Q2 | How many total HTTP 200 responses were logged in `apache.log`? | `52` |
| Q3 | Which IP address generated the most traffic? | `145.76.33.201` |
| Q4 | What is the complete timestamp of the entry where `110.122.65.76` accessed `/login.php`? | `31/Jul/2023:12:34:40 +0000` |

### Key Commands

```bash
# Q1 — Extract URLs (field 7)
cut -d ' ' -f 7 apache.log

# Q2 — Count HTTP 200 responses
awk '$9 == 200' apache.log | wc -l

# Q3 — Find IP with most traffic
cut -d ' ' -f 1 apache.log | sort -n -r | uniq -c

# Q4 — Find specific IP + URL combo
grep -E '110.122.65.76.*/login.php' apache.log
```

---

## Task 7 — Log Analysis Tools: Regular Expressions

| # | Question | Answer |
|---|----------|--------|
| Q1 | How would you modify the grep pattern to match blog posts with an ID between 22–26? | `post=2[2-6]` |
| Q2 | What is the name of the filter plugin used in Logstash to parse unstructured log data? | `Grok` |

```bash
# Modified grep command
grep -E 'post=2[2-6]' apache-ex2.log
```

---

## Task 8 — Log Analysis Tools: CyberChef

Working with `access.log` and `encodedflag.txt` in [CyberChef](https://gchq.github.io/CyberChef/).

| # | Question | Answer |
|---|----------|--------|
| Q1 | Use regex to list all IPs. What is the full IP beginning in 212? | `212.14.17.145` |
| Q2 | A request was encoded in Base64. What is the decoded value? | `THM{CYBERCHEF_WIZARD}` |
| Q3 | Decode `encodedflag.txt` and extract by MAC address. What is the extracted value? | `08-2E-9A-4B-7F-61` |

### Useful Regex Patterns

```
# IPv4 starting with 212
\b212\.([0-9]{1,3}\.){2}[0-9]{1,3}\b

# Find Base64-encoded HTTP requests
(POST|GET) /.*=
```

---

## Task 9 — Log Analysis Tools: Yara and Sigma

| # | Question | Answer |
|---|----------|--------|
| Q1 | What languages does Sigma use? | `YAML` |
| Q2 | What keyword denotes the "title" of a Sigma rule? | `title` |
| Q3 | What keyword denotes the "name" of a rule in YARA? | `rule` |

---

## Tools Used

- **Linux CLI** — `cut`, `awk`, `grep`, `sort`, `uniq`, `wc`
- **CyberChef** — Base64 decoding, regex extraction
- **Yara** — Pattern-based malware detection rules
- **Sigma** — Generic SIEM rule format (YAML)
