
---

## Overview

- Open-source penetration testing and exploitation framework.
    
- Automates stages of a penetration test.
    
- Large database of public, tested exploits.
    
- Modular. Easy to extend.
    
- Source code available on GitHub. Maintained by Rapid7.
    

---

## History

- **2003**: Created by HD Moore (originally in Perl).
    
- **2007**: Rewritten in Ruby.
    
- **2009**: Acquired by Rapid7.
    
- **2019**: Metasploit 5.0 released.
    
- **2020**: Metasploit 6.0 released.
    

---

## Editions

- **Metasploit Framework** (community, free).
    
- **Metasploit Pro / Express** (commercial).
    

---

## Key Terms

- **Interface**: Way to interact with MSF (console, GUI, CLI).
    
- **Module**: Reusable code piece (exploit, auxiliary, payload).
    
- **Vulnerability**: Weakness that can be abused.
    
- **Exploit**: Module that triggers a vulnerability.
    
- **Payload**: Code delivered to target after exploitation.
    
- **Listener**: Waits for inbound connections (handlers).
    

---

## Interfaces

- **msfconsole**: Primary all-in-one console.
    
- **msfcli**: Deprecated CLI. Functionality available in msfconsole.
    
- **Community Edition**: Web GUI.
    
- **Armitage**: Java GUI for visualization and automation.
    

---

## Architecture

- **Modules** execute tasks. Stored under `modules/`.
    
- **Libraries** provide common functionality.
    

### Module types

- **Exploit**: triggers vuln.
    
- **Payload**: executed on successful exploit.
    
- **Auxiliary**: scanning, enumeration, brute-force.
    
- **Encoder**: obfuscates payload to evade AV.
    
- **NOPS**: padding for payload reliability.
    

### Payload styles

- **Non-staged**: sent complete with exploit.
    
- **Staged**: stager establishes channel, then stage downloads and runs.
    

---

## Meterpreter

- In-memory advanced payload.
    
- Interactive command interpreter.
    
- File system access, process control, extensions (Kiwi/ Mimikatz), pivoting.
    
- Harder to detect due to in-memory execution.
    

---

## File locations (Linux)

- Framework modules: `/usr/share/metasploit-framework/modules`
    
- User modules: `~/.msf4/modules`
    

---

## Using MSF in a pentest (PTES mapping)

- **Recon / Enumeration**: auxiliary modules, nmap imports.
    
- **Vulnerability scanning**: auxiliary modules, Nessus integration.
    
- **Exploitation**: exploit modules + payloads.
    
- **Post-exploitation**: meterpreter, post modules (privilege escalation, persistence, data exfiltration).
    

---

## Quick MSFconsole checklist

- `search` to find modules.
    
- `use <module>` to select.
    
- `show options` to view required variables.
    
- `set RHOSTS <ip>` and `set LHOST <ip>` as needed.
    
- `exploit` or `run` to execute.
    
- `sessions -l` to list sessions.
    
- `workspace -a <name>` to organize targets.
    

---

## Further reading

- Official docs on Rapid7 and Metasploit GitHub.
    

_End of note._