- Used to increase or augment the amount of information we can get specifically with regards to service version detection as well as OS detection
- while performing port scanning you should never go for quick scans or top 1000 port scan, you should try to be as comprehensive as you can
- #### NSE
	- is a very powerful feature of nmap tool, it essentially allows users to write and share scripts to automate a wide variety of tasks
	- for example : port scanning, service version detection, vulnerability scanning, exploitation, etc.
	- can be found at usr/share/nmap/scripts/ directory 
	- NSE had scripts in various categories. when we specify the category to nmap it uses all the set of scripts in that category based on the ports that are open and the services which are running on those open ports.
	- It's not like the *NSE* will run all of the scripts regardless to whether or not those services exist.
	- ```
	  nmap -sS -sV -sC -p- -T4 192.168.225.21
	  ```

---
# Nmap NSE — quick reference

- Purpose: extend Nmap with Lua scripts for discovery, versioning, vuln detection, brute force, and exploitation checks.
    
- Location of scripts: `/usr/share/nmap/scripts/`
    
- Run by name, category, or wildcard: `--script <script|category|pattern>`.
    
- Common flags:
    
    - `-sS` TCP SYN scan
        
    - `-sU` UDP scan
        
    - `-sV` service/version detection
        
    - `-p` port(s)
        
    - `-Pn` skip host discovery
        
    - `-T4` aggressive timing
        
    - `-oN` normal output, `-oA` all formats
        
    - `--script-args` pass args to scripts
        
    - `--script-help <script>` show usage
        
    - `--script-trace` debug script execution
        

# NSE categories (short)

- `discovery` — enumerate hosts/services
    
- `auth` — authentication helpers
    
- `vuln` — vulnerability checks
    
- `brute` — password guessing
    
- `intrusive` — may disrupt target
    
- `safe` — low risk
    
- `default` — run with `-sC` or default scripts
    
- `malware`, `exploit`, etc. — use caution / legal consent
    

# Most commonly used scripts (practical list)

- Discovery / enumeration
    
    - `http-title` — page title
        
    - `http-enum` — common directories
        
    - `ssh-hostkey` — SSH host keys
        
    - `smtp-open-relay` — SMTP relay check
        
    - `dns-brute` — DNS brute force
        
    - `smb-enum-shares` / `smb-enum-users`
        
- Service/version/vuln
    
    - `vulners` — CVE lookup via vulnerability DB
        
    - `ssl-enum-ciphers` — TLS ciphers and versions
        
    - `http-vuln*` (e.g., `http-vuln-cve2017-5638`) — HTTP vulns
        
    - `smb-vuln-ms17-010` — EternalBlue check
        
    - `ftp-anon` — anonymous FTP
        
- Auth / brute
    
    - `http-brute` — web form brute force
        
    - `ftp-brute`, `ssh-brute` — credential guessing
        
- Misc / reconnaissance
    
    - `whois-domain`
        
    - `traceroute-geolocation`
        

# How to call scripts (examples)

```bash
# default scripts + service detection, top 1000 ports
nmap -sC -sV demo.ine.local -oN 01-default-scan.txt

# run a specific script
nmap -sV --script smb-enum-shares.nse -p445 demo.ine.local -oN 02-smb-shares.txt

# run all vuln category scripts
nmap -sV --script vuln demo.ine.local -oN 03-vuln-scan.txt

# run multiple scripts or patterns
nmap -sV --script "http-title,http-enum,ssl-enum-ciphers" demo.ine.local -oN 04-web-scan.txt

# use script args (example: provide credentials)
nmap --script smb-enum-users --script-args "smbuser=demo,smbpass=Passw0rd" -p445 demo.ine.local

# show script help
nmap --script-help smb-enum-shares.nse
```

# Script selection tips

- Use categories for broad checks: `--script vuln`, `--script safe`.
    
- Use commas for specific scripts.
    
- Use wildcards: `--script "http-vuln*"` runs all http-vuln scripts.
    
- Check `--script-help` for required `--script-args`.
    
- Use `--script-trace` for debugging script failures.
    

# Output and reporting

- Save canonical output: `-oA <basename>` writes `.nmap`, `.gnmap`, `.xml`.
    
- For evidence capture use `-oN` + timestamped filenames.
    
- Parse `*.xml` with tools (e.g., import to Burp, reporting tools).
    

# Lab: step-by-step (Kali GUI → terminal)

Checklist (perform in order):

-  Confirm network connectivity to target.
    
-  Initial host discovery.
    
-  Top ports quick scan.
    
-  Service/version detection.
    
-  Targeted NSE enumeration.
    
-  Vulnerability-focused NSE.
    
-  Brute/auth attempts only if authorized.
    
-  Save results and summarize findings.
    

Detailed steps:

1. Environment prep
    

```bash
# open terminal in Kali
ping -c 3 demo.ine.local
```

Expect ping replies. If not, use `-Pn` later.

2. Host discovery + quick scan
    

```bash
nmap -Pn -sS -T4 --top-ports 100 demo.ine.local -oN lab-step1-top100.txt
```

Goal: find open ports fast.

3. Service/version detection on discovered ports
    

```bash
# replace <ports> with comma list if needed, or use -p- for all
nmap -sV -sC -p- demo.ine.local -oN lab-step2-sv.txt
```

`-sC` runs default scripts.

4. Full port scan (if needed)
    

```bash
nmap -p- -T4 -sS demo.ine.local -oN lab-step3-allports.txt
```

5. Targeted NSE enumeration per service
    

- Web services (ports 80/443)
    

```bash
nmap -sV --script "http-enum,http-title,http-vuln*" -p80,443 demo.ine.local -oN lab-step4-http.txt
```

- SMB (445/139)
    

```bash
nmap -sV --script "smb-enum-shares,smb-enum-users,smb-vuln-ms17-010" -p139,445 demo.ine.local -oN lab-step4-smb.txt
```

- SSH
    

```bash
nmap -sV --script "ssh-hostkey,ssh-brute" -p22 demo.ine.local -oN lab-step4-ssh.txt
# only run ssh-brute if you have explicit permission
```

- SSL/TLS
    

```bash
nmap -sV --script ssl-enum-ciphers -p 443 demo.ine.local -oN lab-step4-ssl.txt
```

6. Vulnerability sweep
    

```bash
nmap -sV --script vuln demo.ine.local -oN lab-step5-vuln.txt
```

Interpretation: scripts will print CVE IDs and risk markers. Verify manually.

7. Use `vulners` for CVE lookup (if available)
    

```bash
nmap -sV --script vulners demo.ine.local -oN lab-step5-vulners.txt
```

8. Brute force (only if authorized)
    

```bash
nmap --script ssh-brute --script-args userdb=/path/users.txt,passdb=/path/pass.txt -p22 demo.ine.local -oN lab-step6-brute.txt
```

9. Debugging scripts
    

```bash
nmap --script smb-enum-shares --script-trace -p445 demo.ine.local
```

10. Export & summarize
    

```bash
nmap -oA lab-final demo.ine.local
# read lab-final.nmap and lab-final.xml for report generation
```

# Interpreting results (concise)

- `OPEN` ports = reachable services. Note service/version from `sV`.
    
- NSE script output lines often include:
    
    - script name header
        
    - findings (shares, titles, CVEs)
        
    - suggested mitigation or CVE links
        
- Cross-check CVEs manually before claiming exploitability.
    
- False positives occur. Confirm with service-specific tools (e.g., smbclient, curl, openssl).
    

# Safety, scope, and permissions

- Only scan targets you are authorized to test.
    
- `intrusive` scripts may crash services. Avoid on production.
    
- Record timestamps, commands run, and outputs for traceability.
    

# Useful quick commands to remember

```bash
# list scripts and categories
ls /usr/share/nmap/scripts/
grep -R "categories" /usr/share/nmap/scripts/*.nse

# script help
nmap --script-help <script>

# run default scripts + service detection and save all formats
nmap -sC -sV -oA scan-default demo.ine.local
```


---
## we can combine the service version detection scan and OS scan in one option by using _-A_ option in nmap