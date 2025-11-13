# Lab Environment

In this lab environment, you will be provided with GUI access to a Kali machine. The target machine will be accessible at **demo.ine.local**.

**Objective:** Your task is to perform FTP enumeration with Metasploit.

# Tools

The best tools for this lab are:

- Metasploit Framework
- FTP client
---
### FTP Banner Analysis

| Component | Explanation |
|----------|-------------|
| `220` | FTP status code meaning the service is ready to accept a new connection. |
| `ProFTPD` | The FTP server software running on the target. |
| `1.3.5a` | The version of ProFTPD. Useful for identifying known vulnerabilities. |
| `(AttackDefense-FTP)` | Custom server identifier indicating the lab/target environment. |
| `[::ffff:192.141.214.3]` | IPv4-mapped IPv6 format showing the actual IP address of the FTP server. |
| **Overall Meaning** | The target is running ProFTPD 1.3.5a and is actively accepting FTP connections. This version is known to have potential vulnerabilities (e.g., `mod_copy`). |

---
## FTP Enumeration & Brute-Force Attempts — Step-by-Step Notes

| Step | Command / Observation | Intuition (Why We Did This) |
|------|------------------------|------------------------------|
| **1. Check target reachability** | `ping -c4 demo.ine.local` | Verify the host is alive and reachable over the network. Low latency indicates the machine is active and responsive. |
| **2. Basic port scan** | `nmap demo.ine.local` | Quickly identify open ports. Only port **21 (FTP)** appeared open, indicating a focused attack surface. |
| **3. Service & version detection** | `nmap -sC -sV demo.ine.local` | Run default scripts + version scan to learn which FTP server is running. Output shows **ProFTPD 1.3.5a**, a version known for potential vulnerabilities (e.g., `mod_copy`). |
| **4. Start Metasploit** | `msfconsole` | Use MSF’s enumeration and exploitation modules for FTP. |
| **5. Enumerate FTP banner via MSF** | `use auxiliary/scanner/ftp/ftp_version`<br>`set RHOSTS demo.ine.local`<br>`run` | Confirm and display the FTP server banner. The banner revealed: `220 ProFTPD 1.3.5a Server (AttackDefense-FTP)`. Helps validate Nmap results and detect modules relevant to this version. |
| **6. Attempt FTP brute-force login** | `use auxiliary/scanner/ftp/ftp_login` | Begin checking for weak or common FTP credentials. This is important before attempting ProFTPD-specific exploitation because some modules require authentication. |
| **7. Load username and password wordlists** | `set USER_FILE /usr/.../common_users.txt`<br>`set PASS_FILE /usr/.../unix_passwords.txt` | Provide a list of possible usernames and passwords. Used for brute forcing login access over FTP. |
| **8. Set target host** | `set RHOSTS demo.ine.local` | Mandatory: select which host the module attacks. |
| **9. Run brute-force attack** | `run` | Try combinations of usernames and passwords. Results showed **no valid login** yet, meaning FTP login brute-force was unsuccessful with provided lists. |

---

### Additional Notes for Your Report

- The banner and `-sV` scan confirm **ProFTPD 1.3.5a**, which is vulnerable in certain configurations (notably `mod_copy`).  
- If brute-force fails, you typically move to **version-based exploitation**, e.g.:
  - `exploit/unix/ftp/proftpd_modcopy_exec`  
  - Manual `SITE CPFR` / `SITE CPTO` check

I can prepare the next Obsidian table for **ProFTPD exploitation phase** when you proceed.
