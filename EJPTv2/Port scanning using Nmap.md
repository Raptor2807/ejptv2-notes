- # Port Scanning with Nmap — Notes

---

## What is Port Scanning?
Port scanning is the process of probing a host to discover **open, closed, or filtered ports** and the services running on them.  
Nmap (Network Mapper) is the most widely used tool for this purpose.

---

## Basic Scan
```bash
nmap <target>
Reason: Performs a default scan of the 1000 most common ports using TCP connect or SYN scan depending on privileges.

Types of Scans
1. TCP Connect Scan
bash
Copy code
nmap -sT <target>
Establishes a full TCP connection.

Works without root privileges.

Slower and more detectable.

2. SYN Scan (Half-open)
bash
Copy code
nmap -sS <target>
Sends SYN packets and waits for SYN/ACK.

Faster, stealthier, requires root.

Most commonly used.

3. UDP Scan
bash
Copy code
nmap -sU <target>
Probes UDP ports.

Slower because UDP has no handshake.

Useful for services like DNS, SNMP, TFTP.

4. Version Detection
bash
Copy code
nmap -sV <target>
Identifies service versions running on open ports.

Useful for vulnerability assessment.

5. Service & Script Scan
bash
Copy code
nmap -sC <target>
Runs default Nmap Scripting Engine (NSE) scripts.

Provides extra info like SSL details, HTTP titles, etc.

6. Aggressive Scan
bash
Copy code
nmap -A <target>
Combines -sV, -sC, OS detection, and traceroute.

Very informative, but noisy.

Scanning Specific Ports
bash
Copy code
nmap -p 22,80,443 <target>
nmap -p- <target>
-p specifies ports.

-p- scans all 65535 ports.

Stealth and Timing Options
-T0 to -T5: timing templates (slowest to fastest).

-Pn: skip host discovery, assume target is up.

--min-rate <number>: send packets at minimum rate.

Output Options
bash
Copy code
nmap -oN result.txt <target>    # Normal output
nmap -oG result.gnmap <target> # Greppable output
nmap -oX result.xml <target>   # XML output
Key Takeaways
Use SYN scan (-sS) for speed and stealth.

Use version detection (-sV) to identify running services.

Use NSE scripts (-sC or --script) for deeper insights.

Always adjust timing to avoid detection or performance issues.

