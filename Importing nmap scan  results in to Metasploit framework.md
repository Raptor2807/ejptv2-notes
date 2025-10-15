- intiating a nmap scan using it inside the meta sploit framework
---

## Importing Nmap XML into Metasploit (msfconsole / msfdb)

1. Start msfconsole and ensure msf database is running (often `msfdb` or `systemctl start postgresql && msfconsole`).
    
2. Import XML:
    

`msfconsole msf > db_import /path/to/scan-full.xml`

- `db_import` accepts Nmap XML and several other formats. It parses hosts, ports, services, and NSE vuln script output.
    

3. Verify imported data:
    

`msf > hosts msf > services msf > vulns`

- `hosts` lists discovered hosts with IP, OS guess, and info.
    
- `services` lists ports, protocols, service names, product/version and host.
    
- `vulns` shows vulnerabilities discovered via NSE or other sources.
    

---

## Running Nmap from inside msfconsole

Use `db_nmap`. It runs the system `nmap` command and imports results automatically.

`msf > db_nmap -sV -p 1-65535 10.0.0.0/24`

Or with NSE scripts:

`msf > db_nmap -sV -p 80,443 --script vuln 10.0.1.0/24`

Notes:

- `db_nmap` forwards arguments to the installed `nmap`.
    
- Results are auto-imported into the current workspace.
    

---

## Workspaces (isolate engagements)

`msf > workspace -a clientA msf > workspace msf > workspace clientA`

Import/scan inside a workspace so data stays separated.