# Day 17 – Hashcat

## Overview

Today I studied **Hashcat**, one of the most powerful password recovery tools used in cybersecurity and penetration testing. Hashcat is designed to perform fast and advanced password cracking using CPUs, GPUs, and other hardware accelerators.

I learned that Hashcat is commonly used in security testing, digital forensics, and auditing password strength. It is not used to “hack” systems randomly, but to test how resilient password hashes are against different attack strategies.

---

## What Hashcat Is

Hashcat is a command-line based password recovery tool that works by attempting to recover plaintext passwords from hashed values.

It supports:
- Hundreds of hashing algorithms
- GPU acceleration (very high speed)
- Multiple attack modes
- Advanced rule-based transformations

Hashcat does not break encryption. It attempts to guess passwords and compare their hashes to known hash values.

---

## Core Functionalities I Learned

### 1. Multiple Attack Modes

Hashcat supports several attack strategies:

- **Straight (Dictionary) Attack**
  - Uses a wordlist to test possible passwords.
  - Most common and efficient method.

- **Brute-Force Attack**
  - Tries all possible combinations within a defined character set.
  - Very resource-intensive.

- **Mask Attack**
  - Structured brute-force attack.
  - Allows defining patterns like:
    ```
    ?u?l?l?l?d?d
    ```
    (Uppercase, lowercase letters, digits, etc.)

- **Combinator Attack**
  - Combines two wordlists.

- **Hybrid Attack**
  - Dictionary + Mask combination.

---

### 2. GPU Acceleration

One of the most important things I learned is that Hashcat leverages GPU power instead of only CPU. This makes it significantly faster than traditional password cracking tools.

Performance depends heavily on:
- GPU model
- Hash algorithm type
- Wordlist size
- Attack mode

---

### 3. Supported Hash Types

Hashcat supports a wide range of hash algorithms, including:

- MD5
- SHA1
- SHA256
- SHA512
- NTLM
- bcrypt
- WPA/WPA2
- Many others

Each hash type has a specific mode number used when running Hashcat.

---

### 4. Rule-Based Attacks

Hashcat can modify words from a dictionary using rule files.

Examples of modifications:
- Capitalizing first letter
- Adding numbers at the end
- Replacing letters with symbols (e.g., a → @)

This significantly increases password guessing effectiveness without needing a massive wordlist.

---

### 5. Performance Monitoring

I learned that Hashcat provides real-time status output:

- Hashes cracked
- Speed (hashes per second)
- Time elapsed
- Estimated time remaining
- Hardware utilization

This makes it easier to analyze efficiency and adjust strategy.

---

### 6. Session Management

Hashcat supports:
- Pausing sessions
- Restoring interrupted sessions
- Saving cracked passwords
- Logging results

This is useful during long cracking sessions.

---

## Ethical Considerations

Today I reinforced the importance of using Hashcat responsibly.

It should only be used for:
- Authorized penetration testing
- Security audits
- Learning in lab environments
- Password strength testing

Using it against systems without permission is illegal and unethical.

---

## Key Takeaways

- Hashcat is extremely powerful and optimized for GPU-based password recovery.
- Attack mode selection is critical for efficiency.
- Rule-based attacks significantly improve success rates.
- Understanding hash types is essential before starting.
- Performance depends heavily on hardware.
- Ethical use is mandatory.

---

## Next Steps for Me

- Practice in controlled lab environments.
- Experiment with different attack modes.
- Learn to optimize wordlists.
- Study hash identification before attempting recovery.

---

**End of Today’s Learning Log**