- also known as google hacking
- # Google Dorks

Google dorks are specialized search queries that use **Google search operators** to find information that is not easily visible through normal searches. They are often used in security testing, research, and digital investigations.

---

## Core Idea
Google indexes huge amounts of data. Many websites unintentionally expose sensitive files, directories, or misconfigured pages. By combining search operators, you can narrow results to reveal this hidden data.

---

## Common Operators
- `site:example.com` → search within one domain  
	- "site:*.ine.com" but this helps to find out sub domains for that particular domain as google has already indexed it
- `filetype:pdf` → return only a certain file type  
	- site:*.ine.com filetype:pdf admin
- `intitle:"index of"` → find directory listings  
	- site:*.ine.com intitle:admin
- `inurl:admin` → URLs containing "admin"  
	- site:*.ine.com inurl:forum
	- inurl:auth_user_file.txt
	- inurl:passwd.txt
- `cache:` → view Google’s cached copy of a page  
	- you can also use wayback machine. 
	- the older versions might leak some private info that might be useful such as email addr,etc
- `ext:` → same as filetype, for extensions  

---

## Example Dorks
- `site:gov filetype:xls "password"` → may find exposed spreadsheets with sensitive info  
- `intitle:"index of" "backup"` → may reveal open backup directories  
- `inurl:wp-admin` → find WordPress admin panels  

---

## Use Cases
- **Security researchers**: find misconfigurations to report  
- **Investigators**: locate publicly exposed documents  
- **OSINT analysts**: gather open-source intelligence  

---

## Warning
Accessing or exploiting sensitive data you are not authorized to is illegal.  
Safe use is for **research, learning, and improving security** only.


### Interesting thing
- intitle:index of
- this is a common vulnerablity in web servers, 
- there is a misconfiguration called directory listing
- it should be running on a production site because it allows users to view the contents of that directory

### Google hacking database
- # Google Hacking Database (GHDB)

The **Google Hacking Database (GHDB)** is a public repository of documented Google dorks.  
It was originally created by **Johnny Long** and is now maintained on [Exploit-DB](https://www.exploit-db.com/google-hacking-database).

---

## Purpose
- Collects and categorizes Google dorks that reveal sensitive data or misconfigurations  
- Helps penetration testers and researchers quickly find known search strings  
- Provides defenders with a reference to check what attackers might search for  

---

## Categories
Some common categories of dorks included in GHDB:

- **Files containing usernames or passwords**  
- **Sensitive directories**  
- **Vulnerable web servers**  
- **Error messages revealing software details**  
- **Network or device information** (routers, webcams, printers, etc.)  
- **Advisories and vulnerabilities**  

---

## Example Entries
- `filetype:xls inurl:"password"` → spreadsheets with login data  
- `intitle:"index of" "apache logs"` → exposed log directories  
- `inurl:login filetype:php` → login portals written in PHP  

---

## Usage Notes
- Useful for **penetration testers** during reconnaissance  
- Helps **defenders** audit exposure of sensitive information  
- Exploiting data without authorization is **illegal**  

---

## Reference
- [GHDB on Exploit-DB](https://www.exploit-db.com/google-hacking-database)
