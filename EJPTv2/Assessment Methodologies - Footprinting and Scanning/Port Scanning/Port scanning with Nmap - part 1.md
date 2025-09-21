- this is the next step in natural progression after host discovery with nmap
### default nmap port scan
- it means if you are a root user, then nmap will perform a port scan using TCP-SYN.
- It scans 1000 well known ports, if we don't specify the ports to scan or a port range to scan
	- Response 
		- RST : means port is closed
		- SYNACK : means port is open
```
nmap IP
```
### Nmap fast scanning
- scans 100 top ports
```
nmap -Pn -F IP
```


## while dealing with windows system if we see a port which is filtered -> it means that you're dealing with windows firewall
---
---
---
---

### **1. Basic Scan**

`nmap demo.ine.local`

- **What it does:** Performs default scan of top 1000 TCP ports using ping discovery.
    
- **Why:** Quick way to check host availability and common services.
    
- **Logic:** If host responds, confirms reachability and checks common ports first (efficient starting point).
    
- **Result:** Host is up, all 1000 ports closed.
    

---

### **2. Disable Host Discovery**

`nmap -Pn demo.ine.local`

- **What it does:** Treats host as online, skips ICMP/ping discovery.
    
- **Why:** Some hosts block ping, so normal scan may falsely show host as down.
    
- **Logic:** Directly attempts to connect to ports without checking if host replies to ping.
    
- **Result:** Same closed ports output → confirms host was already pingable.
    

---

### **3. Port Scan Syntax Practice**

`nmap -Pn -p80,443,3389 demo.ine.local`

- **What it does:** Scans only ports 80, 443, 3389.
    
- **Why:** Narrow scan to specific services (HTTP, HTTPS, RDP) instead of full port sweep.
    
- **Logic:** Reduces scan time, useful for targeted testing.
    
- **Result:** All three ports closed → no web server or RDP on target.
    

---

### **4. Top 100 Ports Scan**

`nmap -Pn -F demo.ine.local`

- **What it does:** Fast scan (`-F`) of top 100 ports.
    
- **Why:** Quick check for most-used services before running full range scan.
    
- **Logic:** Saves time on large networks, catches >90% of common services.
    
- **Result:** All top 100 ports closed → target likely uses high-numbered/non-standard ports.
    

---

### **5. Full Range Scan**

`nmap -Pn -p0-65530 demo.ine.local`

- **What it does:** Scans almost all ports (0–65530).
    
- **Why:** Enumerate _every_ listening service, including non-standard ports.
    
- **Logic:** After common ports failed, full sweep reveals hidden services.
    
- **Result:**
    
    - 6421/tcp → nim-wan
        
    - 41288/tcp → unknown
        
    - 55413/tcp → unknown  
        ⇒ These are entry points for further investigation.
        

---

### **6. All Ports Shortcut**

`nmap -Pn -p- demo.ine.local`

- **What it does:** Shortcut for scanning all 65535 ports.
    
- **Why:** Ensures nothing is missed (cover edge cases like 65535/tcp).
    
- **Logic:** Similar to range scan but simpler syntax.
    
- **Result:** Same open ports found → confirms no additional services.
    

---

### **7. Localhost Scan**

`nmap 127.0.0.1`

- **What it does:** Scans local machine’s loopback interface.
    
- **Why:** Check for services exposed only locally.
    
- **Logic:** Some services listen only on localhost, not on external interfaces.
    
- **Result:** 3389/tcp open → RDP is running locally.
    

---

### **Key Takeaways**

- **Stepwise approach:** Start with light scan → expand scope only if needed.
    
- **Ping vs. No-Ping:** Use `-Pn` when ICMP filtering suspected.
    
- **Targeted vs. Full Scan:** Specific ports for speed, full sweep for completeness.
    
- **Validation:** Repeat scans with different flags to confirm findings.
    
- **Analysis:** Open ports = potential attack surface; closed ports = reduced exposure.