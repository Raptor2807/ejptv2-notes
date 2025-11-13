Host confirmed: ICMP blocked, nmap -Pn found HFS 2.3, SMB/NetBIOS, MS-RPC and RDP; scan imported into Metasploit DB.

````markdown
---
title: "Lab: Host Discovery & MSF Import — demo.ine.local"
tags: [lab, nmap, metasploit, enumeration, HFS, SMB, RDP]
date: 2025-11-11
---

# Objective
- Confirm host availability despite ICMP blocking.
- Enumerate open services.
- Import scan results into Metasploit database for tracking and follow-up.

# Quick summary
Host `demo.ine.local` (10.2.22.71) blocks ICMP. `nmap -Pn` shows HTTP (HttpFileServer 2.3), SMB/NetBIOS, MS-RPC, and RDP. Nmap XML exported and imported into Metasploit DB successfully.

# Commands & outputs

## Ping (ICMP blocked)
```bash
ping -c 4 demo.ine.local
# 4 packets transmitted, 0 received, 100% packet loss
````

## Initial nmap (host appears down due to probes blocked)

```bash
nmap demo.ine.local
# Note: Host seems down. If it is really up, but blocking our ping probes, try -Pn
```

## Nmap with service/version detection, no-ping, export to XML

```bash
nmap -sV -Pn -oX my.xml demo.ine.local
# Host is up (0.0030s latency)
# PORT      STATE SERVICE            VERSION
# 80/tcp    open  http               HttpFileServer httpd 2.3
# 135/tcp   open  msrpc              Microsoft Windows RPC
# 139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
# 445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
# 3389/tcp  open  ssl/ms-wbt-server? (RDP)
# 49154/tcp open  msrpc
# 49155/tcp open  msrpc
# Service Info: OSs: Windows Server 2008 R2 - 2012
```

## Start DB and Metasploit import

```bash
service postgresql start

msfconsole
# msf6> db_status
# [*] Connected to msf. Connection type: postgresql.
msf6> db_import my.xml
# [*] Successfully imported /root/my.xml
msf6> hosts
# Shows 10.2.22.71 demo.ine.local
msf6> services
# Lists ports and services found by nmap
```

# Observations

- ICMP is filtered. Use `-Pn` for TCP discovery.
    
- HttpFileServer 2.3 may be vulnerable to file upload/exec in some versions. Treat exploit attempts as lab-authorized only.
    
- SMB/NetBIOS and MSRPC indicate Windows attack surface for enumeration and credential-based attacks.
    
- RDP is reachable. Check for NLA and authentication controls.
    
- High TCP ephemeral RPC ports suggest RPC endpoint mapping possible.
    

# Recommended next steps (authorized lab testing only)

1. **HTTP (port 80, HFS 2.3)**
    
    - Manual browse to the host. Check `/`, `/webdav`, directory listings.
        
    - Use `curl`, `dirb`/`gobuster` for directories. Verify upload capability with a safe test file.
        
    - Research HFS 2.3 advisories before exploit attempts.
        
2. **SMB / NetBIOS / MSRPC (139, 445, 135, 49154, 49155)**
    
    - `enum4linux -a demo.ine.local`
        
    - `smbclient -L //demo.ine.local`
        
    - `smbmap -H demo.ine.local`
        
    - `rpcclient` and `rpcdump` for RPC enumeration.
        
3. **RDP (3389)**
    
    - `nmap --script rdp-enum-encryption -p 3389 demo.ine.local`
        
    - Check if NLA is required. If credentials available, test RDP with `xfreerdp` in controlled manner.
        
4. **Metasploit workflow**
    
    - Use `hosts`, `services`, and `vulns` commands to list imported assets.
        
    - Run targeted auxiliary modules for SMB and HTTP enumeration.
        
    - Track credentials and sessions in the DB.
        
5. **Documentation**
    
    - Save `my.xml` and msf DB snapshot. Log timestamps and full command outputs.
        

# Risks / rules

- Do not perform brute force or exploitative actions unless the lab explicitly permits them.
    
- Avoid actions that risk DoS. Rate-limit scanners and brute force tools.
    

# Checklist

-  Manual browse and lightweight HTTP enumeration
    
-  Run directory brute force on port 80 (`gobuster` / `dirb`)
    
-  Enumerate SMB shares and permissions
    
-  Enumerate RPC endpoints
    
-  Check RDP NLA and crypto params
    
-  Import any new scans into Metasploit DB
    

# Notes / artifacts

- Nmap XML: `my.xml`
    
- Metasploit DB: connection verified (`db_status` shows postgres)
    
- Timestamp of scan: `2025-11-11 19:16 IST`
    

# Example quick commands

```bash
gobuster dir -u http://demo.ine.local -w /usr/share/wordlists/dirb/common.txt
enum4linux -a demo.ine.local
smbclient -L //demo.ine.local -N
nmap -p 445 --script smb-enum-shares.nse demo.ine.local
msfconsole -q
# in msf:
# msf6> db_import /root/my.xml
# msf6> hosts; services
```

```
End of note.
```