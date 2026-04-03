# TryHackMe Blue

## Overview
Exploiting SMB vulnerability (MS17-010) using EternalBlue.

## Steps

### Reconnaissance
- Used Nmap to identify SMB service (port 445)

### Exploitation
- Used Metasploit:
  exploit/windows/smb/ms17_010_eternalblue
- Gained Meterpreter session

### Post Exploitation
- Escalated privileges
- Gained system access

## Key Learning
- Understanding SMB vulnerabilities
- Exploiting EternalBlue
- Importance of patching systems
