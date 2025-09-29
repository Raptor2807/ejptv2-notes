## Quick usage

`nmap [scan types] [options] <target(s)>`

## Targets / ranges

- Single host: `192.168.1.10`
    
- Hostname: `demo.ine.local`
    
- CIDR: `192.168.1.0/24`
    
- Range: `192.168.1.1-50`
    
- Multiple: `host1 host2 host3`
    
- From file: `-iL targets.txt`
    
- Exclude: `--exclude 192.168.1.5` / `--excludefile file.txt`
    

## Host discovery

- No ping: `-Pn`
    
- ICMP echo: `-PE`
    
- TCP SYN ping: `-PS22,80`
    
- TCP ACK ping: `-PA22,80`
    
- UDP ping: `-PU53,161`
    
- ARP: used automatically on LAN
    

## Port selection

- All TCP ports: `-p-` or `-p 1-65535`
    
- Specific: `-p22,80,443`
    
- Protocol-specific: `-p U:69,T:53`
    

## Common scan types

- SYN (fast, stealth): `-sS`
    
- Connect (no raw sockets): `-sT`
    
- UDP: `-sU`
    
- OS detection: `-O`
    
- Service/version: `-sV`
    
- Ping/host discovery only: `-sn`
    

## Nmap Scripting Engine (NSE)

- Default scripts: `--script=default`
    
- Specific scripts: `--script=http-enum,snmp-info`
    
- Categories: `--script "vuln or auth"`
    
- Script args: `--script-args 'user=foo,pass=bar'`
    

## Output formats

- Normal: `-oN file.txt`
    
- XML: `-oX file.xml`
    
- Grepable: `-oG file.gnmap`
    
- All: `-oA basename`
    
- Verbose / debug: `-v` / `-vv` / `-d`
    

## Timing and performance

- Timing templates: `-T0`..`-T5`
    
- Max retries: `--max-retries <N>`
    
- Host timeout: `--host-timeout <time>`
    
- Scan delay controls: `--scan-delay`, `--max-scan-delay`
    

## Evasion (use ethically)

- Fragment packets: `-f`
    
- MTU set: `--mtu <value>`
    
- Decoys: `-D decoy1,decoy2,ME`
    
- Spoof source IP: `-S <ip>` (requires privileges)
    
- Source port: `--source-port <port>`
    

## Privileges

- Raw SYN, OS detection, many NSE scripts require root. Use `sudo`.
    

## Interpreting results

- `open` = service listening and reachable.
    
- `closed` = reachable host, no service on port.
    
- `filtered` = packet filtered/dropped by firewall.
    
- `open|filtered` = common with UDP. Use NSE or retries.
    

## Useful examples (lab-focused)

- Quick top ports + service detection:
    

```
sudo nmap -sS -sV -T4 demo.ine.local
```

- Full TCP port scan (all ports):
    

```
sudo nmap -p- -sS -T4 -oN tcp_full.txt demo.ine.local
```

- Targeted UDP ports with version detection:
    

```
sudo nmap -sU -p 53,69,161 -sV -T4 -oN udp_targets.txt demo.ine.local
```

- Combined TCP+UDP scan with scripts (Bind/TFTP/SNMP):
    

```
sudo nmap -sS -sU -p T:1-1000,U:53,69,161 -sV --script=banner,dns-service-discovery,snmp-info,tftp-enum -oA lab_scan demo.ine.local
```

- Aggressive scan (OS, version, scripts, traceroute):
    

```
sudo nmap -A demo.ine.local
```

## NSE scripts useful for this lab

- `dns-service-discovery`
    
- `dns-zone-transfer`
    
- `tftp-enum`
    
- `snmp-info`
    
- `snmp-brute` (use with permission)
    

## Troubleshooting UDP ambiguity

1. Repeat `-sU` with `--max-retries` increased.
    
2. Use NSE relevant scripts.
    
3. Increase timing (`-T2` or `-T3` slower) and `--host-timeout`.
    

## Quick reference line (most-used)

```
sudo nmap -sS -p- -sV -O --script=default --traceroute -T4 -oA quick_all target
```

## Legal and safety

Only scan systems you own or have explicit permission to test.

---

_TIP_: Pin this note in Obsidian or add a `nmap` tag: `#tools/nmap` for quick access.