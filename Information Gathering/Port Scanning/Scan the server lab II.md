  Task : In this lab environment, you will use Nmap to identify ports used by Bind DNS, TFTP, and SNMP servers. No prior setup is required. Clear instructions are provided for scan and port identification. The target machine will be accessible at **demo.ine.local**.

**Objective:** This lab covers the process of performing port scanning and service detection with Nmap.

The objectives of this lab are to:

- Identify the port running a Bind DNS server.
- Identify the port running a TFTP server.
- Identify the port running the SNMP server.

---
---
---


# Summary

Identify ports running BIND DNS, TFTP, and SNMP on `demo.ine.local` using Nmap. Confirm reachability. Perform TCP and UDP scans. Use service-detection and scripts. Note discrepancies between expected and captured terminal output.

# Objectives

- Identify port running BIND DNS server.
    
- Identify port running TFTP server.
    
- Identify port running SNMP server.
    
- Practice port scanning and service detection with Nmap.
    

# Environment

- Target: `demo.ine.local` (192.228.226.3)
    
- Date (from terminal): 2025-09-26
    
- Tools: `nmap`, `ping`, `tftp`, `snmpwalk`
    

# Steps and evidence

## 1. Reachability

**Command**

```bash
ping -c 4 demo.ine.local
```

**Observed**  
Host reachable. 4 packets transmitted, 4 received. Low latency.

## 2. Targeted TCP scan for BIND

**Command**

```bash
nmap demo.ine.local -p 177 -A
```

**Observed**

```
177/tcp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
|_  bind.version: 9.10.3-P4-Ubuntu
```

**Conclusion**  
BIND DNS detected on **TCP 177**. Version `9.10.3-P4-Ubuntu` reported.

## 3. UDP port range scan

**Command**

```bash
nmap demo.ine.local -p 1-250 -sU
```

**Observed**

```
134/udp open|filtered
177/udp open|filtered
234/udp open|filtered
```

**Conclusion**  
Three notable UDP ports. Need targeted service detection.

## 4. UDP service detection on candidate ports

**Command**

```bash
nmap demo.ine.local -p 134,177,234 -sUV
```

**Observed**

```
177/udp open  domain  ISC BIND 9.10.3-P4 (Ubuntu Linux)
234/udp open  snmp    SNMPv1 server; net-snmp SNMPv3 server (public)
134/udp open|filtered ingres-net
```

**Conclusion**

- **177/udp** also reports DNS/BIND.
    
- **234/udp** reports SNMP (net-snmp).
    
- **134/udp** remains unresolved by this scan.
    

## 5. Script scan for UDP/134

**Command**

```bash
nmap demo.ine.local -p 134 -sUV --script=discovery
```

**Observed**  
No definitive service identified for UDP/134. Host scripts returned DNS-related and discovery outputs but not a service fingerprint.

## 6. TFTP client attempt (interactive)

**Command**

```bash
tftp demo.ine.local
# user input aborted; commands returned Invalid command on exit/clear/quit
```

**Observed**  
The provided terminal log shows no successful TFTP session. The lab explanation claims `tftp demo.ine.local 134` produced a console. That is not present in the supplied logs.

# Findings (concise)

- **BIND DNS:** confirmed on **port 177** (TCP and UDP reported). Evidence: targeted nmap scans. Version: `ISC BIND 9.10.3-P4-Ubuntu`.
    
- **SNMP:** **port 234/udp** reported as SNMP (net-snmp). Evidence: `nmap -sUV` output.
    
- **TFTP:** **port 134/udp** is the likely candidate by elimination but **not confirmed** in supplied logs. Nmap showed `open|filtered` for 134 and script scans returned no service fingerprint. The `tftp` interactive attempt in the logs did not show a successful connection.
    

# Verification steps (recommended)

1. Probe TFTP on port 134 with application-level tools.
    

```bash
# nmap tftp script
nmap -sU -p 134 --script tftp-enum demo.ine.local
# or use the tftp client to connect to specific port
tftp demo.ine.local 134
# then at tftp prompt try: get <filename> or put <testfile>
```

2. Confirm SNMP access and community string (common `public`):
    

```bash
snmpwalk -v1 -c public demo.ine.local:234
```

3. Re-run targeted nmap scans with verbosity and timing adjustments if needed:
    

```bash
nmap -sU -p 134,234 -sV -vv --reason demo.ine.local
nmap -sT -p 177 -sV -vv --reason demo.ine.local
```

# Caveats and notes

- UDP scans are inherently less reliable than TCP scans. `open|filtered` means no response or filtered by firewall. Use protocol-specific clients and increase timeouts.
    
- Nmap service detection on non-standard ports can be wrong. Cross-check with native protocol tools.
    
- The supplied `tftp` log shows aborted interaction. Do not mark TFTP as confirmed without further proof.
    

# Conclusion

- DNS (BIND) confirmed on **177**.
    
- SNMP likely on **234/udp**.
    
- TFTP **inferred** on **134/udp** but requires verification.
    

# Quick command list

```bash
ping -c 4 demo.ine.local
nmap demo.ine.local -p 177 -A
nmap demo.ine.local -p 1-250 -sU
nmap demo.ine.local -p 134,177,234 -sUV
nmap demo.ine.local -p 134 -sUV --script=discovery
tftp demo.ine.local 134
snmpwalk -v1 -c public demo.ine.local:234
nmap -sU -p 134 --script tftp-enum demo.ine.local
```

# Next actions

- Run the verification steps and update this note with definitive confirmation of TFTP.
    
- If confirmation is needed now, run `snmpwalk` and `tftp` commands listed above and paste outputs into this note.