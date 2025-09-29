- the objective here is to take all what we have learnt about the target hosts that are online and start to learn about what they're being used for by means of the ports and what services they are running. and in addition to that what OS they're running.
- This information will be useful while we do threat modelling and check for vulnerabilities.
-  
---
# Nmap — Host discovery, port scan, service & OS detection

**Summary:** Steps to discover hosts, scan ports, identify services and OS. Example commands and interpretation notes included.

  

---

  

## Metadata

```yaml

tags: [pentest, nmap, host-discovery, port-scan, service-detection, os-detection]

source: course lecture transcript

```

  

---

  

## 00:00–00:40 — Initial setup & host discovery

- Run `ifconfig` (or `ip a`) to find your assigned interface and subnet mask.

- Determine CIDR / subnet from interface (e.g., `eth1` output).

- Host discovery: use Nmap ping scan to find active hosts on subnet.

```bash

# Example (replace with your interface IP/subnet)

ifconfig eth1

nmap -sn 192.168.1.0/24

```

- Output shows gateway, target host(s) and your Kali machine. Copy target IP(s) for next steps.

  

---

  

## 00:40–03:00 — Port scanning

- Start with default Nmap TCP top-1000 scan.

```bash

nmap <target-ip>

```

- If top-1000 ports are closed, scan full TCP range.

```bash

# Full TCP ports with faster timing template T4

nmap -p- -T4 <target-ip>

```

- Recommendation: scan all TCP ports and UDP where appropriate.

  

---

  

## 03:00–05:00 — Service version detection

- Run service/version detection to identify services and versions.

```bash

# Service/version detection

nmap -sV <target-ip>

```

- Lecture example findings:

  - `mongodb` found on port `6421`, version `2.6.10`.

  - `memcache` reported (port number in transcript was unclear; verify actual port in your scan results).

- Use these service/version results for vulnerability mapping and exploitation attempts later.

  

---

  

## 05:35–07:00 — OS detection

- Combine service detection with OS detection.

```bash

# OS detection + service version

nmap -O -sV <target-ip>

```

- If initial results are inconclusive use aggressive OS guessing:

```bash

nmap -O --osscan-guess -sV <target-ip>

```

- Output items to check:

  - TCP/IP fingerprint

  - MAC vendor

  - OS kernel/version guesses (nmap gives probabilities)

  

---

  

## 07:25–09:30 — Increasing detection intensity

- Increase service version detection intensity if needed.

```bash

# Version intensity from 0 (light) to 8 (aggressive)

nmap -sV --version-intensity 8 <target-ip>

```

- Trade-off: higher intensity improves detection accuracy at the cost of longer scan time and noisiness.

  

---

  

## Interpretation notes / caveats

- Closed ports on top-1000 do not mean target has no services. Always follow up with full-port scans.

- Nmap gives kernel or OS-version guesses. It may identify kernel version but not Linux distribution.

- Accurate distribution identification often requires additional enumeration beyond nmap (NSE scripts, banner grabs, authenticated enumeration).

- Use Nmap Scripting Engine (NSE) for further enumeration. (Next lecture topic.)

  

---

  

## Practical checklist

1. `ifconfig` / `ip a` — identify interface and CIDR.

2. `nmap -sn <subnet>` — host discovery.

3. `nmap <target-ip>` — quick top-1000 TCP scan.

4. If no results: `nmap -p- -T4 <target-ip>` — full TCP scan.

5. `nmap -sV <target-ip>` — service/version detection.

6. `nmap -O -sV <target-ip>` — OS detection.

7. If needed:

   - `nmap -O --osscan-guess -sV <target-ip>` — aggressive OS guesses.

   - `nmap -sV --version-intensity 9 <target-ip>` — aggressive version detection.

8. Record services and versions. Map to known vulnerabilities. Plan enumeration/exploitation.

  

---

  

## Next steps (from lecture)

- Use Nmap Scripting Engine (NSE) for deeper detection and to try to identify Linux distribution and exact kernel.

- Move to targeted enumeration and vulnerability verification for identified services.

  

---

  

## Quick copyable command block

```bash

# Host discovery

nmap -sn 192.168.1.0/24

  

# Full TCP port scan with timing

nmap -p- -T4 <target-ip>

  

# Service/version detection

nmap -sV <target-ip>

  

# OS detection + service detection

nmap -O -sV <target-ip>

  

# Aggressive OS guessing

nmap -O --osscan-guess -sV <target-ip>

  

# Aggressive version detection

nmap -sV --version-intensity 9 <target-ip>

```