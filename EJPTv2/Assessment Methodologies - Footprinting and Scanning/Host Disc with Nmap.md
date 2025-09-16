# Host Discovery with Nmap 

Host discovery = first step of active scanning.  
Goal = find out which computers (hosts) are turned on and responding before we scan them further.

---

## Why We Do Host Discovery
- **Saves time:** We don't scan dead IPs.
- **Smaller attack surface:** Focus only on live hosts.
- **Safer:** We avoid creating too much network noise.

---

## The Most Important Nmap Commands

### 1. Basic Ping Scan
```bash
nmap -sn 192.168.1.0/24
```

- `-sn` = "ping scan only" (no port scan)
    
- Checks who is online in the network range.
    

---

### 2. Skip Ping and Scan Anyway

`nmap -Pn 192.168.1.0/24`

- Treats all IPs as alive.
    
- Use if ICMP ping is blocked by firewall.
    
- **Downside:** Will try to scan even dead hosts → slower.
    

---

### 3. ICMP Echo Requests

`nmap -sn -PE 192.168.1.0/24`

- Classic ping sweep using ICMP Echo.
    
- Works only if ICMP is allowed.
    

---

### 4. TCP SYN Ping

`nmap -sn -PS22,80,443 192.168.1.0/24`

- Sends SYN packet to ports 22, 80, 443.
    
- If reply comes back → host is alive.
    
- Good when ICMP is blocked.
    

---

### 5. TCP ACK Ping

`nmap -sn -PA80,443 192.168.1.0/24`

- Sends ACK packet.
    
- RST response = host alive.
    
- Works through some firewalls.
    

---

### 6. UDP Ping

`nmap -sn -PU53 192.168.1.0/24`

- Sends UDP packet (example to port 53).
    
- If host replies, it is alive.
    
- Good for finding DNS or SNMP devices.
    

---

### 7. ARP Scan (Local Network)

`sudo nmap -sn 192.168.1.0/24`

- When scanning your own LAN, Nmap uses ARP requests automatically (if run with `sudo`).
    
- Most reliable method for local networks.
    

---

## Tips for Beginners

- **Run as root (`sudo`)** for best results (ARP scanning needs it).
    
- If `-sn` shows no results, try `-PS` or `-PA` to bypass ICMP blocking.
    
- Use `-oG` to save live hosts into a file for next steps:
    

`nmap -sn -oG livehosts.txt 192.168.1.0/24 grep Up livehosts.txt | cut -d " " -f 2 > targets.txt`

- Then use `targets.txt` for port scanning.
    

---

## Cheat Sheet: When to Use What

|**Technique**|**Use When**|**Good For**|
|---|---|---|
|`-sn`|No filtering present|Fast discovery|
|`-Pn`|ICMP blocked|Force scan all IPs|
|`-PE`|Normal network|Simple ping sweep|
|`-PS`|ICMP blocked|More stealth, SYN scan|
|`-PA`|ICMP blocked|Works through some firewalls|
|`-PU`|Find DNS/SNMP|UDP-based services|
|ARP Scan|Local subnet|Most accurate on LAN|

---

## Beginner Workflow

1. **Local LAN:**  
    Use ARP scan:
    
    `sudo nmap -sn 192.168.1.0/24`
    
2. **Remote Network:**  
    Try normal ping sweep:
    
    `nmap -sn 10.10.10.0/24`
    
3. **If no replies:**  
    Try SYN ping:
    
    `nmap -sn -PS22,80,443 10.10.10.0/24`
    
4. **Still nothing:**  
    Force scanning:
    
    `nmap -Pn 10.10.10.0/24`
    

---

## Key Takeaways

- Start simple (ICMP ping), escalate to SYN/ACK if blocked.
    
- Always run with `sudo` for best accuracy on LAN.
    
- Save discovered hosts to a file for later scans.
    
- Don't over-scan — focus on hosts that reply.

---
---
---
---
---
# Nmap Usage Cheat Sheet (Beginner Notes)

Nmap = most popular tool for network scanning and host discovery.  
These commands cover **common real-world use cases**.

---

## 1. Scan an IP Range
```bash
nmap 192.168.1.1-50
````

- Scans from `.1` to `.50` in the subnet `192.168.1.x`.
    
- Good when you only want to check a small range.
    

---

## 2. Scan Multiple Specific IPs

```bash
nmap 192.168.1.10 192.168.1.20 192.168.1.30
```

- Separate IPs with a space.
    
- Useful if you know the exact hosts to check.
    

---

## 3. Scan a Whole Subnet

```bash
nmap 192.168.1.0/24
```

- `/24` = 256 IPs (192.168.1.0 – 192.168.1.255).
    
- Most common way to scan a full network.
    

---

## 4. Scan IPs from a Text File

```bash
nmap -iL targets.txt
```

- `targets.txt` contains one IP per line.
    
- Perfect for scanning a pre-collected list of hosts.
    

---

## 5. Scan Hosts That Block ICMP

```bash
nmap -Pn 192.168.1.0/24
```

- `-Pn` = Treat all hosts as alive (skip ping).
    
- Slower because Nmap scans every IP regardless of response.
    

---

## 6. Do a Quick Host Discovery Only

```bash
nmap -sn 192.168.1.0/24
```

- `-sn` = Ping scan only (no port scan).
    
- Finds live hosts quickly.
    

---

## 7. Scan Top 100 Ports (Fast)

```bash
nmap --top-ports 100 192.168.1.10
```

- Scans the most common 100 ports.
    
- Faster than full 65535 port scan.
    

---

## 8. Scan All 65535 Ports

```bash
nmap -p- 192.168.1.10
```

- `-p-` = Scan every possible port (1-65535).
    
- Takes more time but finds all open ports.
    

---

## 9. Detect Service Versions

```bash
nmap -sV 192.168.1.10
```

- Tries to identify the version of software running on each open port.
    
- Example output: `Apache httpd 2.4.49`
    

---

## 10. Detect Operating System

```bash
sudo nmap -O 192.168.1.10
```

- Attempts OS fingerprinting.
    
- Works best with root privileges and open ports.
    

---

## 11. Combine Host Discovery + Service Detection

```bash
sudo nmap -sn -sV 192.168.1.0/24
```

- Finds live hosts, then runs version detection on them.
    

---

## 12. Save Output to a File

```bash
nmap -oN output.txt 192.168.1.0/24
nmap -oG output.gnmap 192.168.1.0/24
```

- `-oN` = Normal text output
    
- `-oG` = Greppable output (easy to parse with `grep`)
    

---

## 13. Randomize Host Order (Stealth)

```bash
nmap --randomize-hosts 192.168.1.0/24
```

- Avoids scanning in sequential order (less suspicious).
    

---

## 14. Scan with Custom Timing

```bash
nmap -T4 192.168.1.0/24
```

- `-T4` = Faster scanning (good for internal labs).
    
- `-T2` or `-T3` = Slower and stealthier (good for production networks).
    

---

## 15. Save Live Hosts to a File

```bash
nmap -sn -oG live.txt 192.168.1.0/24
grep Up live.txt | awk '{print $2}' > hosts.txt
```

- First command = run ping scan
    
- Second command = extract IPs of live hosts into `hosts.txt`
    
- Use `hosts.txt` for targeted scans:
    

```bash
nmap -iL hosts.txt -sV
```

---

## 16. Specific Port Scans

```bash
nmap -p 22,80,443 192.168.1.10
```

- Scans only specified ports.
    
- Useful for quick checks.
    

---

## 17. Aggressive Scan (Do All in One)

```bash
sudo nmap -A 192.168.1.10
```

- Enables:
    
    - OS detection
        
    - Version detection
        
    - Script scanning
        
    - Traceroute
        
- Good for deep enumeration (but noisy).
    

---

## Key Tips for Beginners

- Always start with **host discovery** (`-sn`) before scanning ports.
    
- Run with **sudo** for more accurate results (enables OS detection, ARP scans).
    
- Save output (`-oN`, `-oG`) so you don’t have to rescan later.
    
- Use **small port ranges** for faster testing, **full scans** for thorough work.
    
- If scanning a production network, use slower timing (`-T2`) to avoid detection.
    
--- 
# TCP SYN Ping (Host Discovery)

TCP SYN Ping is a technique used in **host discovery** to find live systems when ICMP (ping) is blocked or filtered.

---

## What It Is
- Instead of sending ICMP Echo Requests, Nmap sends a **TCP SYN packet** to a specific port.
- If the target host replies with **SYN/ACK**, it means the host is alive.
- If the target replies with **RST** or no response, the host may be down or filtered.

---

## Why Use TCP SYN Ping
- Many firewalls block ICMP pings (ping sweeps).
- TCP SYN ping often gets through because it looks like a normal connection attempt.
- Stealthier than full connection because it doesn’t complete the handshake (no ACK sent back).

---

## Command Example
```bash
nmap -sn -PS22,80,443 192.168.1.0/24
-sn → Host discovery only (no port scan)

-PS → TCP SYN ping

22,80,443 → Ports to probe (SSH, HTTP, HTTPS)

How It Works (3-Way Handshake Concept)
Your machine: Sends SYN (start handshake)

Target host: Replies with SYN/ACK if port open

Nmap: Does not send final ACK (handshake incomplete)
→ This confirms host is alive without fully connecting.

When to Use
When ICMP ping sweeps return no results (possibly blocked).

When scanning remote networks where firewalls drop ping.

When you want to blend in with normal web/SSH traffic.

Advantages
Works even if ICMP is blocked.

Less noisy than a full connect scan.

Can target multiple common ports to maximize host detection.

Disadvantages
Requires at least one open or unfiltered port on the target.

May still trigger IDS/IPS alerts.

If target ports are closed/filtered, host may appear "down" even if alive.

Tips for Beginners
Use common ports like 22 (SSH), 80 (HTTP), 443 (HTTPS) — these are most likely open.

Combine with other discovery techniques for better accuracy:

bash
Copy code
nmap -sn -PE -PS22,80,443 192.168.1.0/24
Run with sudo for more reliable results.

Quick Summary Table
Response	Meaning
SYN/ACK	Host alive, port open
RST	Host alive, port closed
No reply	Host down or port filtered