# fping (Fast Ping)

## What is fping?
- **fping** is a command-line tool similar to `ping` but designed for **ping sweeps** and **batch testing**.
- Unlike `ping`, which checks one host at a time, **fping can check many hosts very quickly**.
- Useful for network mapping and finding live hosts.

---

## Key Features
- Pings multiple IPs in parallel (much faster than ping).
- Can read IPs from a file or generate a range automatically.
- Can display only hosts that are alive (quiet mode).
- Designed for scripting and automation.

---

## Basic Syntax
```bash
fping [options] [host/IP/range]
## Common Options

|**Option**|**Description**|
|---|---|
|`-a`|Show only alive (responding) hosts|
|`-u`|Show only unreachable (non-responding) hosts|
|`-g`|Generate a range of IP addresses to ping|
|`-f <file>`|Read list of hosts/IPs from a file|
|`-q`|Quiet mode (no per-ping output, just summary)|
|`-c <count>`|Send N pings per host (like ping count)|
```
---

## Examples

### 1. Ping a Single Host

`fping 192.168.1.1`

### 2. Ping Sweep an Entire Subnet

`fping -a -g 192.168.1.0/24`

- `-a` = show only alive hosts
    
- `-g` = generate all IPs in subnet
    

### 3. Ping IPs from a File

`fping -a -f hosts.txt`

- Reads IP addresses/hostnames line by line from `hosts.txt`
    
- Shows which ones are alive
    

### 4. Show Only Dead Hosts

`fping -u -g 192.168.1.0/24`

---

## Why Pentesters Use fping

- **Speed:** Can check thousands of hosts quickly.
    
- **Automation:** Works well in scripts.
    
- **Stealth:** Less noisy than full Nmap scan (just sends ICMP Echo).
    

---

## Things to Watch For

- Requires root privileges for raw socket use on some systems.
    
- Can trigger IDS/IPS alerts if used aggressively on monitored networks.
    
- If ICMP is blocked, combine with Nmap `-Pn` or TCP SYN scans.