---

---
## Web Server Enumeration Using Metasploit — Detailed Notes

---

### 1. What is a Web Server?
A web server is software that listens on HTTP/HTTPS ports (usually 80 and 443) and serves web content such as HTML pages, images, scripts, or APIs to clients (browsers, scanners, curl, etc.).  
Common examples: Apache, Nginx, IIS, Lighttpd.

**Intuition:**  
Before enumeration, we must understand that the target exposes a service meant to be interacted with through HTTP requests. Identifying the server type and configuration helps predict potential misconfigurations and vulnerabilities.

---

### 2. What is HTTP and HTTPS?
- **HTTP (Hypertext Transfer Protocol):**  
  Unencrypted protocol used for communication between client and server over port **80**.

- **HTTPS (HTTP Secure):**  
  HTTP + TLS encryption. Uses SSL/TLS certificates and operates on port **443**.

**Intuition:**  
Understanding whether the server uses encryption impacts what scanning modules to use, how responses appear, and whether SSL-related enumeration is needed.

---

## 3. Lab Environment Setup

### Step | Action | Intuition
---|---|---
**1. Identify server IP** | `ifconfig` on Kali or given in lab | You must know the target address before scanning.
**2. Start PostgreSQL DB** | `systemctl start postgresql` | Metasploit stores loot, creds, and service info in the database.
**3. Launch Metasploit** | `msfconsole` | Main tool for automated enumeration.
**4. Create workspace** | `workspace -a web_enum` | Organizes results and avoids mixing different lab data.
**5. Set global RHOSTS/RHOST** | `setg RHOSTS <ip>`<br>`setg RHOST <ip>` | Saves time; all modules automatically target the same host.

---

## 4. Web Server Enumeration using HTTP Modules

### Step 1 — Identify HTTP Version
- `search type:auxiliary name:http`
- `use auxiliary/scanner/http/http_version`
- Check options
- Run

**Intuition:**  
`http_version` reveals the underlying web server software (Apache, Nginx, IIS), its version, and any interesting headers. This is often the first step in attacking web servers.

**Example Output:**  
`Server: Apache/2.4.18`

This provides a fingerprint for vulnerability research.

---

### Step 2 — Inspect HTTP Headers
- `search http_header`
- `use auxiliary/scanner/http/http_header`
- `show options`
- Run with default required fields

**Intuition:**  
HTTP headers leak useful information:
- `Content-Type`
- `Server`
- `X-Powered-By`
- `Cookie flags`
- `Security headers`

Your result included:  
`Content-Type: text/html`  
`Server: Apache/2.4.18`

This confirms technology stack and version.

---

## 5. Searching for Hidden/Restricted Directories

### robots.txt Enumeration
- Use module: `auxiliary/scanner/http/robots_txt`
- Run it

**Intuition:**  
Websites often list restricted or admin-only areas inside `robots.txt`. Attackers use this to find sensitive directories.

Your results:  
- `/data`  
- `/secure`

---

### Step 1 — Inspect `/data`
- `curl http://192.168.0.2/data`

**Intuition:**  
Directory listing enabled = misconfiguration → attacker can browse files directly.

### Step 2 — Inspect `/secure`
- `curl http://192.168.0.2/secure/`

**Result:** Unauthorized (401)

**Intuition:**  
This indicates authentication is required. Next step: brute force.

---

## 6. Directory Brute Forcing

### Directory Scanner
- `use auxiliary/scanner/http/dir_scanner`
- Set `PATH` if scanning a subdirectory
- Run

**Intuition:**  
This helps discover hidden directories that are not listed in robots.txt, useful for finding admin panels, backups, or misconfigurations.

---

### File Brute Forcing
- `use auxiliary/scanner/http/files_dir`
- Run

**Intuition:**  
Searches for common files like:
- `.php`, `.bak`, `.conf`, `.txt`, `.backup`, etc.  
File brute forcing reveals sensitive files and backup copies often left behind by developers.

You found:  
- Multiple files + 301 redirects (indicating directory moves or enforced slashes)

---

## 7. HTTP Login Brute Force

### Step 1 — Attempt login brute force
- `use auxiliary/scanner/http/http_login`
- Check options
- Unset previous incorrect file: `unset USERPASS_FILE`
- Run

**Intuition:**  
Try to break into `/secure` using default/common credentials.  
First attempt failed → need better wordlists.

---

### Step 2 — Use larger wordlists
- `set USER_FILE /usr/share/metasploit-framework/data/wordlists/namelist.txt`
- `set PASS_FILE /usr/share/metasploit-framework/data/wordlists/unix_passwords.txt`
- `set VERBOSE false`
- `run`

**Intuition:**  
A broader username/password search increases the chances of finding valid credentials.

---

## 8. Apache User Directory Enumeration

### Step | Action | Intuition
---|---|---
**Use module** | `use auxiliary/scanner/http/apache_userdir_enum` | Checks for `/~username` home directories on Apache servers. Useful to identify user accounts.
**Set wordlist** | `set USER_FILE /usr/.../common_users.txt` | Common usernames help identify valid accounts.
**Test a specific user** | `echo "rooty" > user.txt`<br>`set USER_FILE user.txt` | Quick tests for a single suspected username.

**Intuition:**  
Apache UserDir sometimes exposes personal directories like:  
`http://server/~alice/`  
If accessible, this leaks user info, files, or credentials.

---

## Final Summary

### What we achieved:
- Identified **Apache/2.4.18** web server
- Enumerated headers and configuration
- Found hidden folders (`/data`, `/secure`)
- Verified `/data` is publicly accessible
- Confirmed `/secure` requires authentication
- Performed directory and file brute forcing
- Attempted credential brute forcing on `/secure`
- Enumerated Apache user directories

### Next steps could include:
- Manual credential guessing using hints from discovered files
- Checking `/data` for sensitive files (config, backups)
- Testing vulnerabilities specific to Apache/2.4.18
- Trying SQLi, LFI/RFI, or upload bypass depending on application

---

If you want, I can also create a **flow diagram** or a **complete Obsidian vault section** combining FTP + Web Enum + Exploit.
