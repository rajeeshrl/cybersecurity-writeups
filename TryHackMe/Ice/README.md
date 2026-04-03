# TryHackMe Ice Walkthrough

## Overview
This room focuses on exploiting a vulnerable Icecast media server on a Windows machine.

---

## Key Steps

### 1. Reconnaissance
- Performed full port scan using Nmap
- Identified open ports including 8000 (Icecast service)

### 2. Exploitation
- Discovered Icecast vulnerability (CVE-2004-1561)
- Used Metasploit module:
  exploit/windows/http/icecast_header
- Gained Meterpreter session

### 3. Privilege Escalation
- Used local exploit suggester
- Escalated privileges using bypassuac_eventvwr

### 4. Post Exploitation
- Migrated to stable process (spoolsv.exe)
- Loaded kiwi (Mimikatz)
- Extracted credentials:
  Dark : Password01!

---

## Tools Used
- Nmap
- Metasploit
- Meterpreter
- Mimikatz (Kiwi)

---

## Key Learnings
- Importance of service enumeration
- Exploiting known vulnerabilities
- Privilege escalation techniques
- Credential dumping from memory

---

## Medium Writeup
Full detailed writeup:
https://medium.com/@rajeesh8214/tryhackme-ice-walkthrough-exploiting-a-vulnerable-icecast-server-1d07e71e86a2
