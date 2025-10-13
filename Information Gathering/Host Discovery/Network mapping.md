# Network Mapping (Beginner-Friendly Notes)

- After collecting information about a target organization during the **passive information gathering** stage, a penetration tester moves to **active information gathering**.
- **Passive information gathering** = no direct interaction (Google search, WHOIS, OSINT).
- **Active information gathering** = sending packets to the target network to get responses (can be detected by firewalls).

### Tasks in Network Mapping
- **Discover hosts:** Find out which devices are alive and responding.
- **Perform port scanning:** Identify open ports and services.
- **Enumerate services and OS:** Gather details about software and operating systems in use.

**Key Point:**  
Every device on a network must have a **unique IP address**, so finding live IPs is the first step.

**Questions a Pentester Must Answer:**
- Which hosts in scope are online and reachable?
- What ports are open on those hosts?
- What services/OS are running that could be exploited?

---

## What is Network Mapping?

- **Definition:**  
  The process of **finding all devices, IP addresses, and services** in a network and understanding its layout.

### Why It’s Important
- Builds a **map** of the network for testing.
- Reveals **attack surface** — open ports, exposed services.
- Helps prioritize attacks (e.g., focus on critical servers, not printers).

---

## Example: Why Map a Network?

- Example scope: `200.200.0.0/16`
- A `/16` means **65,536 possible IP addresses** (huge network).
- First step:
  - Find which of those addresses are in use.
  - Focus scanning efforts only on live hosts (saves time).

---

## Network Mapping Objectives

- **Discovery of Live Hosts:**  
  Avoid wasting time scanning non-existent IPs.

- **Identification of Open Ports & Services:**  
  Example: If port 22 (SSH) is open, you might try password attacks.

- **Network Topology Mapping:**  
  Understand how devices connect (e.g., which subnets exist, where firewalls are).

- **OS Fingerprinting:**  
  Learn if a host is Windows/Linux — important for selecting exploits.

- **Service Version Detection:**  
  Example: Finding Apache 2.4.49 running → look up CVEs for that version.

- **Identifying Filtering & Security Measures:**  
  Detect firewalls, IDS/IPS so you know how stealthy you must be.

---

## Nmap (Network Mapper)

- **What it is:**  
  An open-source tool used by hackers and defenders for network scanning.

### Key Features
- **Host discovery:** Find live hosts.
- **Port scanning:** Discover open ports (attack surface).
- **Service version detection:** Find exact software running.
- **OS fingerprinting:** Identify operating system type/version.

### Who Uses It
- **Pentesters:** To find targets and vulnerabilities.
- **Admins:** To audit their network and check exposure.
- **Red/Blue Teams:** For attack and defense exercises.

---

## Nmap Functionality (Simplified)

- **Host Discovery:**  
  Like a "network-wide ping" to see who’s alive.

- **Port Scanning:**  
  Check which "doors" (ports) are open on each host.

- **Service Version Detection:**  
  Find out what is running (e.g., Apache 2.4.49 vs. Nginx 1.18).

- **OS Fingerprinting:**  
  Guess OS from network behavior (e.g., TTL, TCP flags).

---

## Host Discovery (Finding Live Hosts)

- Purpose: Only scan **active hosts** instead of entire IP range blindly.
- Your choice of technique depends on:
  - **Network type:** Local or remote
  - **Stealth:** Avoiding detection or not
  - **Goals:** Quick recon vs. full mapping

---

## Host Discovery Techniques

- **Ping Sweep (ICMP Echo):**  
  - Classic way: send ICMP Echo Request (`ping`) to many IPs.
  - If you get Echo Reply → host is online.

- **ARP Scanning:**  
  - Works only in local network (Layer 2).
  - Very reliable because ARP cannot be blocked easily.

- **TCP SYN Ping:**  
  - Sends a SYN packet to a port (like half of TCP handshake).
  - If SYN/ACK is received → host is alive.
  - More stealthy than ICMP.

- **UDP Ping:**  
  - Send UDP packet to closed port, expect ICMP "Port Unreachable".
  - If you get reply → host is alive.

- **TCP ACK Ping:**  
  - Send ACK packet. Host sends RST back if alive.
  - Bypasses some firewalls that drop SYN.

- **SYN-ACK Ping:**  
  - Send SYN-ACK; host replies with RST → confirms host is up.

---

## Choosing a Technique

- **ICMP Ping**
  - **Pros:** Very simple and fast.
  - **Cons:** Often blocked by firewalls → false negatives.

- **TCP SYN Ping**
  - **Pros:** Harder for firewalls to block (looks like normal traffic).
  - **Cons:** Might trigger intrusion detection alerts.

---



[[Ping Sweeps]]
