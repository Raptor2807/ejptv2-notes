### Completing Skill Check Labs

Skill Check Labs are interactive, hands-on exercises designed to validate the knowledge and skills you’ve gained in this course through real-world scenarios. Each lab presents practical tasks that require you to apply what you’ve learned. Unlike other INE labs, solutions are not provided, challenging you to demonstrate your understanding and problem-solving abilities. Your performance is graded, allowing you to track progress and measure skill growth over time.

# Lab Environment

A website is accessible at **http://target.ine.local**. Perform reconnaissance and capture the following flags.

- **Flag 1:** This tells search engines what to and what not to avoid.
- **Flag 2:** What website is running on the target, and what is its version?
- **Flag 3:** Directory browsing might reveal where files are stored.
- **Flag 4:** An overlooked backup file in the webroot can be problematic if it reveals sensitive configuration details.
- **Flag 5:** Certain files may reveal something interesting when mirrored.

# Tools

- Firefox
- Curl
- HTTrack

---

### Note

In this lab, the flag will follow the format: FLAG1{MD5Hash} OR FL@G1{MD5Hash}. For example, FLAG1{0f4d0db3668dd58cabb9eb409657eaa8}. You need to submit only the MD5 hash string, excluding the braces. For instance: 0f4d0db3668dd58cabb9eb409657eaa8.

--- 
# Assessment Methodologies: Information Gathering CTF 1 — Full Notes

These notes summarize the CTF solution process and explain why each step was taken.

---

## Flag 1: robots.txt

- **Action**: Visited `http://target.ine.local/robots.txt`.  
- **Reason**: The hint mentioned instructions for search engines. `robots.txt` is designed to tell crawlers which paths to index or ignore. Often, sensitive directories are unintentionally revealed here.  
- **Result**: Found the flag inside `robots.txt`.  
- **Answer**: `8155d9126fb24f2092855fbca6bea649`

---

## Flag 2: Web server and version

- **Action**: Ran Nmap scan:
  ```bash
  nmap -sC -sV target.ine.local
- **Reason**:
    
    - `-sC` runs default NSE scripts for service probing.
        
    - `-sV` identifies service versions.  
        The hint required identifying the running service and version.
        
- **Result**: Nmap output revealed the target’s web service and version.
    
- **Answer**: `c1e39ae6855349758f63ec15cbe8429e`
    

---

## Flag 3: Directory browsing

- **Action**: Used DirB:
    
    `dirb http://target.ine.local`
    
- **Reason**: The hint referred to directory browsing. Directory brute-forcing reveals hidden paths and files.
    
- **Result**: Found `/wp-content/uploads/` with indexing enabled. Inside was `flag.txt`.
    
- **Answer**: `6ca785e9728243618484b02521a10f49`
    

---

## Flag 4: Backup file in webroot

- **Action**: Ran DirB with bigger wordlist and backup extensions:
    
    `dirb http://target.ine.local -w /usr/share/dirb/wordlists/big.txt -X .bak,.tar.gz,.zip,.sql,.bak.zip`
    
- **Reason**:
    
    - The hint mentioned a backup file.
        
    - `-w` specifies the larger wordlist.
        
    - `-X` tries common backup extensions.
        
- **Result**: Found `wp-config.bak`. Retrieved with:
    
    `curl http://target.ine.local/wp-config.bak`
    
    File contained the flag.
    
- **Answer**: `6912dbdbe9ff4c9f8e038a1b67fdb0a8`
    

---

## Flag 5: Mirroring files

- **Action**: Mirrored the site using HTTrack:
    
    `httrack http://target.ine.local -O target.html`
    
- **Reason**: The hint mentioned mirrored files. HTTrack downloads the entire site, including hidden or orphaned files.
    
- **Result**: Discovered `xmlrpc0db0.php` in mirrored content, which contained the flag.
    
- **Answer**: `8d93b2e60e7d46a395283f92c90c1a0d`
    

---

## Summary of Flags

- **Flag 1**: 8155d9126fb24f2092855fbca6bea649
    
- **Flag 2**: c1e39ae6855349758f63ec15cbe8429e
    
- **Flag 3**: 6ca785e9728243618484b02521a10f49
    
- **Flag 4**: 6912dbdbe9ff4c9f8e038a1b67fdb0a8
    
- **Flag 5**: 8d93b2e60e7d46a395283f92c90c1a0d
    

---

## Key Takeaways

- `robots.txt` can leak sensitive paths.
    
- Nmap is effective for service discovery and versioning.
    
- Directory brute-forcing helps uncover hidden resources.
    
- Backup files in webroots pose a serious security risk.
    
- Site mirroring can reveal hidden or orphaned files.
    

`Do you also want me to make a **compact “cheat sheet” version in MD** with just the commands + why, without the long explanations?`