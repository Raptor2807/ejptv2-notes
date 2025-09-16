Here’s the **Ping Sweeps** section from your PDF, fully converted into Markdown with beginner-friendly explanations added:


# Ping Sweeps

## What is a Ping Sweep?
- A **ping sweep** is a network scanning technique used to find **live hosts** in a given IP address range.
- It sends multiple ICMP Echo Requests (ping messages) to different IPs and checks who replies.
- Hosts that reply are considered online.

---

## Purpose of Ping Sweeps
- Quickly identify which systems are reachable before running heavier scans.
- Saves time by excluding offline IPs from port scanning.
- Common first step in **network mapping**.

---

## How Ping Works
- Ping uses **ICMP (Internet Control Message Protocol)**.
- **ICMP Echo Request (Type 8, Code 0):** Sent by your machine to a target host.
- **ICMP Echo Reply (Type 0, Code 0):** Sent back by target if it is alive.

---

## Ping Sweep Workflow
1. **Choose a network range** (e.g., 192.168.1.0/24).
2. Send ICMP Echo Requests to each IP in the range.
3. Collect replies → IPs that respond are live.
4. Ignore IPs with no reply (but note they could be blocking ICMP).
# Note : if the target IP system is running windows, then it would never respond to ICMP echo requests
---

## Example Commands

### Single Host Ping
```bash
ping 192.168.1.10
````

Sends continuous ICMP Echo Requests until interrupted.

### Control the number of ICMP packets to be sent
```
ping -c 5 192.168.0.1
```
Sends 5 ICMP packets 

### Scanning an entire Subnet 
```
ping -b -c 1 subnet_addr / 10.0.2.0
```
### Ping Sweep Using Nmap

```bash
nmap -sn 192.168.1.0/24
```

- `-sn` = "ping scan only" (no port scanning).
    
- Lists all live hosts in the subnet.
    

### Ping Sweep Using fping

```bash
fping -a -g 192.168.1.0/24
```

- `-a` = show alive hosts only
    
- `-g` = generate IP range automatically
    

---

## ICMP Types Recap

- **Echo Request:** Type 8, Code 0
    
- **Echo Reply:** Type 0, Code 0
    

---

## Things to Remember

- **No Response ≠ Always Offline:**
    
    - Firewalls may block ICMP traffic.
        
    - Some systems are configured not to respond to pings for security reasons.
        
- Use other discovery methods (TCP SYN ping, ARP scan) if needed.
    

---

## Visualization

```
Pentester → ICMP Echo Request → Host A → ICMP Echo Reply (Alive)

Pentester → ICMP Echo Request → Host B → No Response (Offline/Filtered)
```

---

## Practical Tips for Beginners

- Always start with ping sweep to avoid wasting time on dead IPs.
    
- If nothing replies, try:
    
    ```bash
    nmap -Pn 192.168.1.0/24
    ```
    
    (`-Pn` treats all hosts as alive and directly attempts port scans).
    
- Combine with ARP scan for local network:
    
    ```bash
    sudo arp-scan 192.168.1.0/24
    ```
    


[[fping]] 