Nmap has several built-in scripts that can be used for SMB (Server Message Block) reconnaissance on Windows networks. These scripts are part of the Nmap Scripting Engine (NSE) and can be very useful for gathering information about SMB shares, users, and vulnerabilities.

# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Your task is to fingerprint the service using the tools available on the Kali machine and run Nmap scripts to enumerate the Windows target machine's SMB service.

1. Identify SMB Protocol Dialects
2. Find SMB security level information
3. Enumerate active sessions, shares, Windows users, domains, services, etc.

**The following username and password may be used to access the service:**

| Username | Password | | administrator | smbserver_771 |

# Tools

- Nmap
---
SMB (Server Message Block) is a **network file sharing protocol** used mainly by **Windows systems** to let computers share files, printers, and other resources over a network.

Key points:

- **Purpose:** Enables file and printer sharing, remote management, and interprocess communication.
    
- **Ports:** Uses TCP **445** (modern) and **139** (older NetBIOS transport).
    
- **Operation:** A client requests access to a shared file or printer on a server; the server responds based on permissions.
    
- **Versions (dialects):**
    
    - **SMB1** (CIFS) – old and insecure.
        
    - **SMB2** – improved performance and security (Windows Vista+).
        
    - **SMB3.x** – adds encryption and better integrity (Windows 8+/Server 2012+).
        
- **Security features:** Authentication (NTLM or Kerberos), message signing, optional encryption.
    
- **Common vulnerabilities:** SMBv1 (EternalBlue, MS17-010), weak signing, guest access.
    

In penetration testing, SMB is important because:

- It often reveals **usernames, shares, and OS info**.
    
- It can be exploited for **remote code execution or lateral movement**.
---
## Summary

Start unauthenticated. Verify service and dialects. Check security posture. Enumerate sessions and shares anonymously. Authenticate to expand findings. Enumerate users, groups, domains, services, and list directories. Save outputs for reporting.

---

## Commands + one-line purpose + intuition

1. `nmap -p445 -Pn -sV --script smb-protocols demo.ine.local`  
    Purpose: list supported SMB dialects.  
    Intuition: identifies legacy support (SMBv1) and modern features (SMB2/3). Legacy = exploit risk.
    
2. `nmap -p445 --script smb-security-mode demo.ine.local`  
    Purpose: check signing, auth level, challenge/response.  
    Intuition: disabled signing or guest fallback increases relay/MITM risk.
    
3. `nmap -p445 --script smb-enum-sessions demo.ine.local`  
    Purpose: discover currently logged users and sessions (anonymous).  
    Intuition: exposed usernames are targets for brute-force and social engineering.
    
4. `nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: repeat sessions with creds for full details.  
    Intuition: authenticated queries show source hosts and timestamps.
    
5. `nmap -p445 --script smb-enum-shares demo.ine.local`  
    Purpose: list shares visible to anonymous user.  
    Intuition: reveals public read/write exposure without credentials.
    
6. `nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: enumerate shares with admin creds.  
    Intuition: reveals hidden admin shares (ADMIN$, C$, D$), paths, and RW access.
    
7. `nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: list accounts and attributes.  
    Intuition: find non-expiring passwords, guest/no-password accounts, and RIDs.
    
8. `nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: collect server stats (failed logins, files opened).  
    Intuition: failed logins point to brute-force attempts or misconfig.
    
9. `nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: extract domain/workgroup info and password policy.  
    Intuition: password policy and lockout determine brute-force feasibility.
    
10. `nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: enumerate groups and membership.  
    Intuition: identifies users with admin/RDP privileges.
    
11. `nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: list Windows services via SMB RPC.  
    Intuition: services like Print Spooler or cloud agents indicate attack vectors or cloud image.
    
12. `nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`  
    Purpose: list shares and preview directory trees/files.  
    Intuition: quick harvest of config files, creds, backups, driver files without mounting.
    

---

## Example observations (paste outputs under each header in your note)

- Dialects: `NT LM 0.12 (SMBv1)`, `2.x`, `3.x`
    
- Signing: `disabled`
    
- Sessions: `bob` logged in; `ADMINISTRATOR` active from your IP
    
- Shares (guest): `C (READ)`, `Documents (READ)`, `IPC$ (READ/WRITE)`
    
- Shares (admin): `ADMIN$, C$, D$` RW; `print$` drivers path exposed
    
- Users: `Administrator`, `bob` (admin), `Guest` (no password)
    
- Groups: `Administrators` includes `bob`
    
- Services: `Spooler`, `AmazonSSMAgent`, `Ec2Config`
    
- Server stats: `34 failed logins`, `7 permission errors`, `35 files opened`
    

---

## Intuition / sequencing rationale

- **Minimal surface first**. Run non-auth probes to see public exposure. This avoids unnecessary auth noise.
    
- **Check posture**. Protocols and signing determine viable attacks. SMBv1 or disabled signing raise priority.
    
- **Enumerate sessions and shares anonymously**. Find usernames and public data.
    
- **Authenticate** only when you need expanded info and have credentials. Authenticated scans reveal hidden shares, full user lists, groups, services, and file trees.
    
- **Follow data to action**. If `bob` is local admin and RDP enabled, target credential access or lateral movement. If Print Spooler present, consider spooler-related exploit paths (version-dependent). If admin shares writable, consider file retrieval or service manipulation.
    

---

## Reporting checklist (tick when added to report)

-  Raw Nmap outputs saved (`-oN`/`-oA`)
    
-  Dialects and SMBv1 presence documented
    
-  Message signing state documented
    
-  Anonymous share/session findings documented
    
-  Authenticated findings documented (shares, users, groups)
    
-  Potential high-risk vectors listed (SMBv1, admin user in RDP group, Print Spooler, writable ADMIN$)
    
-  Recommended next steps noted (mount shares, harvest files, test specific CVEs if authorized)
---

# SMB lab — steps, commands, observed output, explanation, intuition

---

## Quick summary

Start GUI Kali. Verify target is alive. Fingerprint SMB. Check security posture. Enumerate sessions, shares, users, groups, domains, services. Escalate from anonymous to authenticated enumeration. Save outputs.

---

## Step 0 — open lab

**Action**: Open the lab link and access the Kali VM GUI.  
**Why**: Provides the attacker/client platform for scanning and authenticated enumeration.

---

## Step 1 — ping target

**Command**

`ping -c 5 demo.ine.local`

**Observed**: 5 packets sent, 5 received. Host is alive.  
**Explanation**: Basic reachability check. Confirms network path and latency.  
**Intuition**: Fast check before heavier scans. If ping fails, use `-Pn` with Nmap.

---

## Step 2 — quick Nmap host discovery

**Command**

`nmap demo.ine.local`

**Observed**: Multiple open ports; SMB (445) present.  
**Explanation**: Default TCP scan lists open services for follow-up.  
**Intuition**: Identify services to prioritise. SMB on 445 becomes next focus.

---

## Step 3 — SMB protocol dialects

**Command**

`nmap -p445 -Pn -sV --script smb-protocols demo.ine.local`

**Observed (your output)**:

- `445/tcp open microsoft-ds`
    
- OS: Windows Server 2008 R2 - 2012
    
- Dialects: `NT LM 0.12 (SMBv1)`; `2:x` (SMB2); `3:x` (SMB3)  
    **Explanation**: `smb-protocols` lists supported SMB dialects. SMBv1 present plus SMB2/3.  
    **Intuition**: Dialects define available features and weaknesses. SMBv1 is legacy and high-risk. SMB3 indicates modern features like encryption may exist.
    

---

## Step 4 — SMB security posture

**Command**

`nmap -p445 --script smb-security-mode demo.ine.local`

**Observed (your output)**:

- `account_used: guest`
    
- `authentication_level: user`
    
- `challenge_response: supported`
    
- `message_signing: disabled (dangerous)`  
    **Explanation**: Script checks signing, auth fallback, and challenge/response support.  
    **Intuition**: Disabled signing + guest fallback increases relay/MITM and anonymous info-leak risk.
    

Reference: [https://nmap.org/nsedoc/scripts/smb-security-mode.html](https://nmap.org/nsedoc/scripts/smb-security-mode.html)

---

## Step 5 — anonymous session enumeration

**Command**

`nmap -p445 --script smb-enum-sessions demo.ine.local`

**Observed**: `WIN-OMCNBKR66MN\bob` listed as logged in.  
**Explanation**: Anonymous query returned active logged-in user.  
**Intuition**: Guest/anonymous access can reveal usernames. Use as enumeration seed for password attacks or social engineering.

---

## Step 6 — authenticated session enumeration

**Command**

`nmap -p445 --script smb-enum-sessions --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- `bob` since `2025-10-21T13:45:20`
    
- `ADMINISTRATOR` connected from `\\10.10.41.2` (your host)  
    **Explanation**: Authenticated run gives timestamps and source host for sessions.  
    **Intuition**: Authenticated detail helps map active accounts and session origins. Use creds only when necessary and authorized.
    

---

## Step 7 — enumerate shares (anonymous)

**Command**

`nmap -p445 --script smb-enum-shares demo.ine.local`

**Observed (anonymous)**:

- `C` read, `Documents` read, `Downloads` read, `IPC$` READ/WRITE, hidden admin shares no access.  
    **Explanation**: Lists shares and current access for anonymous account.  
    **Intuition**: `IPC$` RW and readable shares indicate misconfig or lax ACLs. IPC$ allows null-session style enumeration of names and shares.
    

---

## Step 8 — enumerate shares (authenticated)

**Command**

`nmap -p445 --script smb-enum-shares --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed (authenticated)**:

- `ADMIN$`, `C$`, `D$` present and **READ/WRITE** for admin.
    
- `Documents`, `Downloads` READ; `print$` READ/WRITE.  
    **Explanation**: Authenticated enumeration reveals hidden admin shares and actual paths.  
    **Intuition**: Admin share RW means full filesystem access. Look for creds, configs, backups, or sensitive data.
    

---

## Step 9 — list folders (smb-ls)

**Command**

`nmap -p445 --script smb-enum-shares,smb-ls --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- `ADMIN$` shows `ADFS` locales.
    
- `C`/`C$` show Program Files and `Amazon` entries.
    
- `print$` contains driver/color profiles.  
    **Explanation**: `smb-ls` previews directory contents without mounting.  
    **Intuition**: Quick harvest of useful files. Presence of Amazon/EC2 tooling indicates cloud image.
    

---

## Step 10 — enumerate users

**Command**

`nmap -p445 --script smb-enum-users --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- `Administrator` (RID 500), `bob` (RID 1010), `Guest` (RID 501, password not required)  
    **Explanation**: Lists local accounts and flags like password policies.  
    **Intuition**: Accounts with non-expiring or no-password flags are weak points. `bob` is a target since he is active and appears privileged.
    

---

## Step 11 — server statistics

**Command**

`nmap -p445 --script smb-server-stats --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- Server stats since `2025-10-21T13:45:14`
    
- `94766 bytes sent`, `80638 bytes received`
    
- `34 failed logins`, `7 permission errors`, `35 files opened`  
    **Explanation**: Provides operational stats and failed login counts.  
    **Intuition**: High failed logins may indicate brute-force attempts or misconfigured automation.
    

---

## Step 12 — enumerate domains

**Command**

`nmap -p445 --script smb-enum-domains --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- Domain: `WIN-OMCNBKR66MN`
    
- Password policy: max age 42 days, complexity required, account lockout disabled  
    **Explanation**: Extracts domain/workgroup metadata and policy.  
    **Intuition**: Lockout disabled eases password spraying. Complexity required still affects brute-force strategy.
    

---

## Step 13 — enumerate groups

**Command**

`nmap -p445 --script smb-enum-groups --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- `Builtin\Administrators`: Administrator, **bob**
    
- `Remote Desktop Users`: bob  
    **Explanation**: Maps group membership and privilege distribution.  
    **Intuition**: `bob` in Administrators and RDP group is high-value. Compromise of `bob` yields full control.
    

---

## Step 14 — enumerate services

**Command**

`nmap -p445 --script smb-enum-services --script-args smbusername=administrator,smbpassword=smbserver_771 demo.ine.local`

**Observed**:

- `AmazonSSMAgent`, `AWSLiteAgent`, `Ec2Config`, `DiagTrack`, `MSDTC`, `Spooler`  
    **Explanation**: Lists services and states discovered via SMB RPC.  
    **Intuition**: Print Spooler historically exploitable. AWS/EC2 agents indicate cloud instance; adjust follow-up actions accordingly.
    

---

## Conclusion / combined intuition

- Sequence: reachability → service discovery → protocol fingerprint → posture check → anonymous enumeration → authenticated enumeration → deep listing.
    
- High-leverage findings: SMBv1 enabled, message signing disabled, `IPC$` RW for anonymous, `bob` is a local admin + RDP member, admin shares are RW, Print Spooler present, account lockout disabled, presence of AWS agents.
    
- Attack surface summary: credential harvesting and RDP targeting for `bob`; file collection from admin shares; potential relay/MITM if network permits; spooler-based exploits where version allows; tailored cloud-specific steps if EC2 image.