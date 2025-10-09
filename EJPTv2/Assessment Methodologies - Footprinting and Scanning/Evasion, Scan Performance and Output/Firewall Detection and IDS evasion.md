
- **`-sA` — ACK scan**
  - Purpose: check whether ports are being filtered by a firewall.  
  - Behavior: sends TCP ACK packets.  
    - `RST` response → probe reached host (unfiltered for that probe).  
    - No response → likely filtered (packets dropped).  
  - Example:
    ```bash
    nmap -sA -p 1-1000 --reason 10.4.27.83
    ```

- **IDS evasion & spoofing (lab only)**
  - _Do not use against systems without written permission._

  - **Fragmentation**
    - Splits packets into fragments to alter signatures.
    - Example:
      ```bash
      nmap -Pn -sS -sV -f 10.4.27.83
      ```

  - **MTU (with fragmentation)**
    - Force smaller MTU to produce more fragments.
    - Example:
      ```bash
      nmap -Pn -sS -sV -f --mtu 32 10.4.27.83
      ```

  - **TTL manipulation**
    - Change IPv4 TTL to affect perceived hop distance.
    - Example:
      ```bash
      nmap --ttl 64 -sS 10.4.27.83
      ```

  - **Packet padding (`--data-length`)**
    - Append random bytes to probes to change payload length/fingerprint.
    - Example:
      ```bash
      nmap -Pn -sS -sV -p 445,3389 --data-length 200 10.4.27.23
      ```

  - **Decoys (`-D`)**
    - Mix decoy IPs with real traffic to confuse simple logs/analysts.
    - Decoy list is comma-separated. Include `ME` to show your real probe among decoys.
    - Example:
      ```bash
      nmap -Pn -sS -sC -p 445,3389 -f --data-length 200 -D  10.10.23.1,10.10.23.2,ME 10.4.27.23
      ```
    - Note: decoys do not change the actual source IP seen by upstream devices unless you spoof at the IP layer. Many networks drop spoofed packets.

  - **Source port (`-g`)**
    - Set source port to appear like traffic from a common service (e.g., DNS port 53).
    - Example:
      ```bash
      nmap -Pn -sS -sC -p 445,3389 -f --data-length 200 -g 53 -D 10.10.23.1,10.10.23.2,ME 10.4.27.23
      ```
    - Effect: probes use source port 53. Logs may show DNS-like origin port.

- **Quick warnings**
  - Modern IDS/IPS perform stateful correlation. Simple evasion often fails.  
  - Spoofing real gateway or third-party addresses can harm others and is illegal on shared networks.  
  - Always capture PCAP and IDS logs. Compare baseline vs evasion runs.

---

## Goals

- Understand how to detect firewalls and basic filtering.
    
- Learn low-impact Nmap techniques to test IDS/IPS behavior in a lab.
    
- Record signals to look for when an IDS/firewall reacts.
    

---



## Basic concepts (short)

- **Filtering**: device drops or blocks packets (firewall). Can be host-based or network.
    
- **Stateful vs stateless**: stateful tracks connections and will drop unexpected packets. Stateless may filter by rules without context.
    
- **IDS/IPS**: detects or prevents malicious traffic. IDS alerts. IPS may block or reset.
    
- **False positives**: evasive or crafted packets can cause unusual alerts.
    


---

## Short cheat sheet

- SYN scan: `-sS`
    
- ACK scan: `-sA`
    
- UDP scan: `-sU`
    
- Fragment packets: `-f`
    
- Slow scan: `--scan-delay`
    
- Rate cap: `--max-rate`
    
- Packet trace: `--packet-trace`
    

## Cleaned lecture notes (added)

### ACK scan (`-sA`)

- Purpose: detect whether packets are being filtered by a firewall rather than discover open services.
    
- Behaviour: sends TCP ACK packets to the specified ports. Responses tell you if a packet was dropped (filtered) or if a host responded with an RST (unfiltered / reachable).
    
- Example:
    

```bash
nmap -sA -p 22,80,443 --reason 10.4.27.83
```

### Evasion / spoofing primitives (lab only)

> Use only in an authorised lab. These techniques change packet shape or metadata. They may break connectivity or produce misleading logs.

#### Fragmentation (`-f`) and MTU (`--mtu`)

- `-f` splits packets into small fragments. This can evade simple signature matching.
    
- `--mtu <size>` forces a specific MTU for fragmentation. Smaller MTU increases fragment count.
    
- Example:
    

```bash
nmap -Pn -sS -sV -f 10.4.27.83
nmap -Pn -sS -sV -f --mtu 32 10.4.27.83
```

#### Packet padding (`--data-length`)

- Appends random bytes to each probe. Changes packet fingerprint and payload length.
    
- Example:
    

```bash
nmap -Pn -sS -sV -p445,3389 --data-length 200 10.4.27.23
```

#### TTL manipulation (`--ttl`)

- Sets the IPv4 TTL field. Useful to make packets appear to originate from different network hops.
    
- Example:
    

```bash
nmap --ttl 64 -sS 10.4.27.83
```

#### Decoys (`-D`) and spoofing source appearance

- `-D decoy1,decoy2,...,ME` mixes decoy IP addresses with your real traffic to confuse analysts and simple host-based logs.
    
- Last IP in the command is always the target. Decoys are listed as comma-separated values.
    
- Example with decoys and fragmentation:
    

```bash
nmap -Pn -sS -sC -p445,3389 -f --data-length 200 -D 10.10.23.1,10.10.23.2,ME 10.4.27.23
```

- Note: Decoying does not truly change source IP in packets seen by upstream devices unless you spoof IPs at the IP layer. Many environments will drop or block spoofed source IPs.
    

#### Source port change (`-g`)

- `-g <port>` sets the source port of packets you send. Useful to make traffic appear to come from common services (DNS 53, HTTP 80).
    
- Example:
    

```bash
nmap -Pn -sS -sC -p445,3389 -f --data-length 200 -g 53 -D 10.10.23.1,10.10.23.2,ME 10.4.27.23
```

This makes probes use source port 53 so logs may show the traffic as originating from a DNS source port.

### Practical notes and warnings

- Many evasions rely on altering packet headers. Upstream devices or the target may normalize or drop such traffic.
    
- Modern IDS/IPS use multiple features and stateful correlation. Simple evasion may not succeed.
    
- Spoofing real gateway IPs can incriminate others. Never spoof real third-party addresses on shared networks.
    
- Always capture PCAP and IDS logs before and after each test. Compare baseline to changed runs.
    

### Short examples (lab-ready)

- ACK scan to test filtering:
    

```bash
nmap -sA -p 1-1024 --reason 10.4.27.83
```

- Fragmented, padded SYN scan with decoys and source-port 53:
    

```bash
nmap -Pn -sS -p22,80 -f --data-length 120 -g 53 -D 10.10.23.1,10.10.23.2,ME 10.4.27.83
```

- Slow, low-noise polite scan:
    

```bash
nmap -sS -sV -p 1-1024 --scan-delay 200ms --max-rate 10 10.4.27.83
```

---
---
---

## Detection steps (beginner flow)

1. **Reachability**: `ping -c 3 <target>` and `traceroute <target>`.
    
2. **TCP ACK scan** (tests filtering vs port state):
    

```bash
nmap -sA -p 1-1000 --reason <target>
```

Interpretation:

- **RST returned**: likely host reachable and port unfiltered for that probe type.
    
- **No response / filtered**: packets dropped by filter.
    

3. **SYN scan** (default stealthy TCP discovery):
    

```bash
nmap -sS -p 1-1024 -Pn --reason <target>
```

4. **FIN/NULL/XMAS** (stateless probe differences):
    

```bash
nmap -sF -sN -sX -p 1-1024 --reason <target>
```

Different firewall types and stacks respond differently to these.

5. **UDP probes** (UDP often filtered):
    

```bash
nmap -sU -p 53,123,161 --reason <target>
```

6. **Use `--packet-trace`** to see sent/received packets:
    

```bash
nmap -sS -p 80 --packet-trace <target>
```

---

## IDS/IPS detection signals to record

- Unexpected TCP RSTs from an intermediate IP or the target.
    
- ICMP unreachable messages that change with different probes.
    
- Rate limiting of responses compared with baseline.
    
- Delayed responses when payloads change.
    
- Extra packets injected (RSTs, TCP resets) by inline devices.
    

Record timestamped logs and pcap for each test.

---

## Low-impact evasion techniques (lab only)

Use these to study IDS behaviour. Keep slow and small.

- **Slow the scan**: reduce speed so traffic looks normal.
    

```bash
nmap -sS -p 1-1024 --scan-delay 200ms --max-retries 2 <target>
```

- **Limit packet rate**:
    

```bash
nmap -sS -p 1-1024 --max-rate 10 <target>
```

- **Randomize order**:
    

```bash
nmap -sS --randomize-hosts --top-ports 100 <targets-list>
```

- **Fragment packets** (can alter simple signatures):
    

```bash
nmap -sS -f --data-length 24 -p 22 <target>
```

- **Use decoys** (only in lab):
    

```bash
nmap -sS -D decoy1,ME,decoy2 -p 22 <target>
```

- **Use different probe types**: `-sS`, `-sA`, `-sF`, `-sN`, `-sX` and `-sU`.
    

---

## Example, minimal polite scan (beginner)

```bash
nmap -sS -sV -p 22,80,443 --scan-delay 150ms --max-rate 20 --reason <target>
```

What to capture alongside:

- `tcpdump -w baseline.pcap` before running scan.
    
- IDS logs collected with timestamps.
    

---

## How to take notes in Obsidian (suggested structure)

- Use this file as a template note.
    
- Create child notes per target: `target-192.0.2.10.md`.
    
- Link scans and PCAPs with attachments or paths.
    
- Use YAML frontmatter for metadata: `target_ip`, `lab`, `date`, `permission`.
    

Example frontmatter for a target note:

```yaml
---
created: 2025-10-09
target_ip: 192.0.2.10
lab: home-lab
permission: written
tags: [nmap, experiment]
---
```

---

## Minimal logging template (copy into target note)

- Date:
    
- Command run:
    
- Nmap output (summary):
    
- PCAP filename:
    
- IDS logs observed:
    
- Interpretation:
    

---

## Further reading (lab)

- `man nmap` and `nmap --help`.
    
- Nmap book: official Nmap documentation and online guides.
    
