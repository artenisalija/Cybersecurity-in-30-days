# Day 11 🔐 Brute Force Attacks & Burp Suite — Learning Notes

## What is a Brute Force Attack?

A **brute force attack** is a trial-and-error method used to guess credentials (usernames, passwords, tokens) by systematically trying every possible combination until the correct one is found. It's one of the oldest and most straightforward attack techniques in cybersecurity.

There are a few common variations:

- **Simple Brute Force** — tries every possible combination of characters (e.g., `aaa`, `aab`, `aac`...). Effective but very slow.
- **Dictionary Attack** — uses a pre-made list of common or leaked passwords (wordlist) instead of random combinations. Much faster and more practical.
- **Credential Stuffing** — uses known username/password pairs from previous data breaches to try on other services.
- **Password Spraying** — tries one common password (e.g., `Password123`) against many different usernames to avoid account lockouts.

### Why Does It Work?

Brute force succeeds when applications:
- Don't limit the number of login attempts
- Don't implement CAPTCHA or multi-factor authentication
- Allow users to set weak or common passwords
- Return different responses for valid vs invalid usernames (user enumeration)

---

## Tools Used

- **Burp Suite** (Community or Pro) — a web security testing proxy that intercepts, inspects, and modifies HTTP requests between your browser and a web server.
- **Wordlist** — a text file with one password per line (e.g., `rockyou.txt`)

---

## Steps Followed in Burp Suite to Brute Force a Login Page

### Step 1 — Configure Your Browser Proxy
Set your browser to route traffic through Burp Suite's proxy, typically at `127.0.0.1:8080`. In Burp, go to **Proxy → Options** to confirm the listener is active.

### Step 2 — Intercept the Login Request
1. Turn **Intercept ON** in **Proxy → Intercept**
2. Navigate to the target login page in your browser
3. Enter any test credentials (e.g., `admin` / `test`) and submit the form
4. Burp Suite will capture the raw HTTP POST request

### Step 3 — Send to Intruder
Right-click on the intercepted request and select **"Send to Intruder"**.  
Then turn Intercept off so normal browsing resumes.

### Step 4 — Configure the Intruder Attack
Go to the **Intruder** tab:

1. **Positions sub-tab** — Burp highlights parameters automatically. Clear all highlights first (`Clear §`), then manually select the value you want to fuzz (e.g., the password field) and click **Add §**.  
   The marked parameter will look like: `password=§test§`

2. **Choose an Attack Type:**
   | Type | Use Case |
   |---|---|
   | **Sniper** | One payload list, one parameter at a time |
   | **Battering Ram** | Same payload inserted into all marked positions simultaneously |
   | **Pitchfork** | Multiple lists, one payload per position in parallel |
   | **Cluster Bomb** | Multiple lists, tries every combination (best for user + pass) |

3. **Payloads sub-tab** — Load your wordlist:
   - Set Payload type to **Simple list**
   - Click **Load** and select your wordlist file (e.g., `rockyou.txt`)

### Step 5 — Start the Attack
Click **"Start Attack"**. Burp will send a request for each payload and display the results in a table showing:
- Request number
- Payload used
- Status code
- Response length

### Step 6 — Identify the Successful Login
Look for an **anomaly** in the results — a response that differs from the rest:
- A different **HTTP status code** (e.g., `302 Found` instead of `200 OK`)
- A different **response length** (longer or shorter than failed attempts)
- A response containing words like `"Welcome"` or `"Dashboard"` when you inspect it

That anomaly usually indicates a successful login. The payload used in that request is the valid credential.

---

## Key Takeaways

- Brute force relies on the absence of rate limiting or account lockout policies
- Dictionary attacks are far more efficient than pure brute force in real scenarios
- Burp Suite's **Intruder** module is a powerful tool for automating and analyzing login attacks
- Always look for **response length differences** — it's often the clearest signal of a successful guess
- In production environments, always get **written authorization** before performing any security testing

---

## How to Defend Against Brute Force

- Implement **account lockout** after N failed attempts
- Use **CAPTCHA** on login forms
- Enforce **multi-factor authentication (MFA)**
- Monitor and alert on repeated failed login attempts
- Use **strong password policies** and check against known breached password lists

---

> ⚠️ **Disclaimer:** This document is for **educational purposes only**. Only perform security testing on systems you own or have explicit written permission to test. Unauthorized access to computer systems is illegal.