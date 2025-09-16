# DNS and DNS Zone Transfers

---

## DNS (Domain Name System)

### Definition
DNS is a hierarchical naming system that translates **domain names** (e.g., `example.com`) into **IP addresses** (e.g., `93.184.216.34`).

### Key Components
- **Domain name**: Human-readable address (e.g., `example.com`)  
- **DNS resolver**: Client-side service that queries DNS servers  
- **Root servers**: Store addresses of top-level domain (TLD) servers (`.com`, `.org`, etc.)  
- **Authoritative name servers**: Hold records for a specific domain  

### Common DNS Record Types
- `A` → Maps domain to IPv4 address  
- `AAAA` → Maps domain to IPv6 address  
- `CNAME` → Alias for another domain name  
- `MX` → Mail exchange server for emails  
- `NS` → Nameservers for a domain  
- `TXT` → Arbitrary text, often used for verification (e.g., SPF, DKIM)  
- `PTR` → Reverse lookup (IP → domain)  

---
# DNS Interrogation

---

## Definition
**DNS interrogation** is the process of gathering information about a target domain by querying its DNS records.  
It is commonly used in **reconnaissance** during penetration testing or attacks.

---

## Typical Data Collected
- **A / AAAA records** → IP addresses of hosts  
- **MX records** → mail servers  
- **NS records** → authoritative name servers  
- **TXT records** → SPF, DKIM, verification strings  
- **PTR records** → reverse lookups (IP → hostname)  
- **CNAME records** → aliases pointing to other domains  

---

## Methods
- Querying with tools like `nslookup` or `dig`  
- Attempting **zone transfers** if misconfigured  
- Performing reverse DNS sweeps to discover hosts  
- Enumerating subdomains via wordlists or OSINT sources  

---

## Security Risks
- Disclosure of internal hostnames  
- Identification of server roles (web, mail, VPN, etc.)  
- Discovery of potential attack vectors  

---

## Defense
- Restrict DNS zone transfers to authorized servers only  
- Limit unnecessary exposure of DNS records  
- Monitor DNS queries for enumeration attempts  

## DNS Zone Transfers

### Definition
A **zone transfer** is the process where a DNS server sends a copy of its zone file (all DNS records for a domain) to another DNS server.

### Types
- **AXFR (Full Zone Transfer)**: Entire zone file is copied  
- **IXFR (Incremental Zone Transfer)**: Only changes since the last update are copied  

### Purpose
- Synchronize records between **primary (master)** and **secondary (slave)** DNS servers  
- Provide redundancy and load balancing  

### Security Risk
- If zone transfers are not restricted, attackers can request them and obtain:
  - All hostnames and IPs in a domain  
  - Mail servers, subdomains, internal records  
- This is useful for **network mapping and reconnaissance**  

### Prevention
- Restrict zone transfers to specific authorized IPs (secondary DNS servers)  
- Use DNSSEC for integrity and authentication  
- Regularly audit DNS server configurations  

---

## Example Command
To test zone transfer with `dig`:
```bash
dig @ns1.example.com example.com AXFR


```dnsenum

```
- dnsenum can essentially enumerate the records that are publicly available
- it can also be used to perform DNS Zone transfer automatically
- it can be used to do DNS brute force and can be used to identify records more specifically subdomains
- in linux we can access the host file for local dns cache, under 
  sudo vim /etc/hosts

### dnsenum
```
dnsenum zonetransfer.me
dnsenum VERSION:1.3.1

-----   zonetransfer.me   -----


Host's addresses:
__________________

zonetransfer.me.                         6340     IN    A        5.196.105.14


Name Servers:
______________

nsztm1.digi.ninja.                       9939     IN    A        81.4.108.41
nsztm2.digi.ninja.                       9939     IN    A        5.196.105.10


Mail (MX) Servers:
___________________

ASPMX.L.GOOGLE.COM.                      0        IN    A        108.177.127.27
ALT1.ASPMX.L.GOOGLE.COM.                 0        IN    A        74.125.131.27
ALT2.ASPMX.L.GOOGLE.COM.                 0        IN    A        142.250.4.26
ASPMX2.GOOGLEMAIL.COM.                   31       IN    A        142.250.147.27
ASPMX5.GOOGLEMAIL.COM.                   293      IN    A        108.177.125.27
ASPMX4.GOOGLEMAIL.COM.                   293      IN    A        142.250.4.26
ASPMX3.GOOGLEMAIL.COM.                   293      IN    A        74.125.131.27


Trying Zone Transfers and getting Bind Versions:
_________________________________________________


Trying Zone Transfer for zonetransfer.me on nsztm1.digi.ninja ... 
zonetransfer.me.                         7200     IN    SOA               (
zonetransfer.me.                         7200     IN    DNSKEY            (
zonetransfer.me.                         301      IN    TXT               (
zonetransfer.me.                         7200     IN    MX                0
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    A        5.196.105.14
zonetransfer.me.                         7200     IN    NS       nsztm1.digi.ninja.
zonetransfer.me.                         7200     IN    NS       nsztm2.digi.ninja.
zonetransfer.me.                         7200     IN    CERT              (
zonetransfer.me.                         300      IN    HINFO        "Casio
_acme-challenge.zonetransfer.me.         301      IN    TXT               (
_sip._tcp.zonetransfer.me.               14000    IN    SRV               0
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200     IN    PTR      www.zonetransfer.me.
asfdbauthdns.zonetransfer.me.            7900     IN    AFSDB             1
asfdbbox.zonetransfer.me.                7200     IN    A         127.0.0.1
asfdbvolume.zonetransfer.me.             7800     IN    AFSDB             1
canberra-office.zonetransfer.me.         7200     IN    A        202.14.81.230
cmdexec.zonetransfer.me.                 300      IN    TXT              ";
contact.zonetransfer.me.                 2592000  IN    TXT               (
dc-office.zonetransfer.me.               7200     IN    A        143.228.181.132
deadbeef.zonetransfer.me.                7201     IN    AAAA     dead:beaf::
dr.zonetransfer.me.                      300      IN    LOC              53
DZC.zonetransfer.me.                     7200     IN    TXT         AbCdEfG
email.zonetransfer.me.                   2222     IN    NAPTR             (
email.zonetransfer.me.                   7200     IN    A        74.125.206.26
Hello.zonetransfer.me.                   7200     IN    TXT             "Hi
home.zonetransfer.me.                    7200     IN    A         127.0.0.1
Info.zonetransfer.me.                    7200     IN    TXT               (
internal.zonetransfer.me.                300      IN    NS       intns1.zonetransfer.me.
internal.zonetransfer.me.                300      IN    NS       intns2.zonetransfer.me.
intns1.zonetransfer.me.                  300      IN    A        81.4.108.41
intns2.zonetransfer.me.                  300      IN    A        167.88.42.94
office.zonetransfer.me.                  7200     IN    A        4.23.39.254
ipv6actnow.org.zonetransfer.me.          7200     IN    AAAA     2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.                     7200     IN    A        207.46.197.32
robinwood.zonetransfer.me.               302      IN    TXT          "Robin
rp.zonetransfer.me.                      321      IN    RP                (
sip.zonetransfer.me.                     3333     IN    NAPTR             (
sqli.zonetransfer.me.                    300      IN    TXT              "'
sshock.zonetransfer.me.                  7200     IN    TXT             "()
staging.zonetransfer.me.                 7200     IN    CNAME    www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301      IN    A         127.0.0.1
testing.zonetransfer.me.                 301      IN    CNAME    www.zonetransfer.me.
vpn.zonetransfer.me.                     4000     IN    A        174.36.59.154
www.zonetransfer.me.                     7200     IN    A        5.196.105.14
xss.zonetransfer.me.                     300      IN    TXT      "'><script>alert('Boo')</script>"

Trying Zone Transfer for zonetransfer.me on nsztm2.digi.ninja ... 
zonetransfer.me.                         7200     IN    SOA               (
zonetransfer.me.                         7200     IN    DNSKEY            (
zonetransfer.me.                         301      IN    TXT               (
zonetransfer.me.                         7200     IN    MX                0
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               10
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    MX               20
zonetransfer.me.                         7200     IN    A        5.196.105.14
zonetransfer.me.                         7200     IN    NS       nsztm1.digi.ninja.
zonetransfer.me.                         7200     IN    NS       nsztm2.digi.ninja.
zonetransfer.me.                         7200     IN    CERT              (
zonetransfer.me.                         300      IN    HINFO        "Casio
_acme-challenge.zonetransfer.me.         301      IN    TXT               (
_sip._tcp.zonetransfer.me.               14000    IN    SRV               0
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200     IN    PTR      www.zonetransfer.me.
asfdbauthdns.zonetransfer.me.            7900     IN    AFSDB             1
asfdbbox.zonetransfer.me.                7200     IN    A         127.0.0.1
asfdbvolume.zonetransfer.me.             7800     IN    AFSDB             1
canberra-office.zonetransfer.me.         7200     IN    A        202.14.81.230
cmdexec.zonetransfer.me.                 300      IN    TXT              ";
contact.zonetransfer.me.                 2592000  IN    TXT               (
dc-office.zonetransfer.me.               7200     IN    A        143.228.181.132
deadbeef.zonetransfer.me.                7201     IN    AAAA     dead:beaf::
dr.zonetransfer.me.                      300      IN    LOC              53
DZC.zonetransfer.me.                     7200     IN    TXT         AbCdEfG
email.zonetransfer.me.                   2222     IN    NAPTR             (
email.zonetransfer.me.                   7200     IN    A        74.125.206.26
Hello.zonetransfer.me.                   7200     IN    TXT             "Hi
home.zonetransfer.me.                    7200     IN    A         127.0.0.1
Info.zonetransfer.me.                    7200     IN    TXT               (
internal.zonetransfer.me.                300      IN    NS       intns1.zonetransfer.me.
internal.zonetransfer.me.                300      IN    NS       intns2.zonetransfer.me.
intns1.zonetransfer.me.                  300      IN    A        81.4.108.41
intns2.zonetransfer.me.                  300      IN    A        167.88.42.94
office.zonetransfer.me.                  7200     IN    A        4.23.39.254
ipv6actnow.org.zonetransfer.me.          7200     IN    AAAA     2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.                     7200     IN    A        207.46.197.32
robinwood.zonetransfer.me.               302      IN    TXT          "Robin
rp.zonetransfer.me.                      321      IN    RP                (
sip.zonetransfer.me.                     3333     IN    NAPTR             (
sqli.zonetransfer.me.                    300      IN    TXT              "'
sshock.zonetransfer.me.                  7200     IN    TXT             "()
staging.zonetransfer.me.                 7200     IN    CNAME    www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301      IN    A         127.0.0.1
testing.zonetransfer.me.                 301      IN    CNAME    www.zonetransfer.me.
vpn.zonetransfer.me.                     4000     IN    A        174.36.59.154
www.zonetransfer.me.                     7200     IN    A        5.196.105.14
xss.zonetransfer.me.                     300      IN    TXT      "'><script>alert('Boo')</script>"

                                                                                                                                                                                                                                           
Brute forcing with /usr/share/dnsenum/dns.txt:                                                                                                                                                                                             
_______________________________________________                                                                                                                                                                                            
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
zonetransfer.me class C netranges:                                                                                                                                                                                                         
___________________________________                                                                                                                                                                                                        
                                                                                                                                                                                                                                           
 4.23.39.0/24                                                                                                                                                                                                                              
 5.196.105.0/24
 74.125.206.0/24
 81.4.108.0/24
 143.228.181.0/24
 167.88.42.0/24
 174.36.59.0/24
 202.14.81.0/24
 207.46.197.0/24

                                                                                                                                                                                                                                           
Performing reverse lookup on 2304 ip addresses:                                                                                                                                                                                            
________________________________________________                                                                                                                                                                                           
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
0 results out of 2304 IP addresses.

                                                                                                                                                                                                                                           
zonetransfer.me ip blocks:                                                                                                                                                                                                                 
___________________________                                                                                                                                                                                                                
                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                           
done.
                                                                                                                                                                                                                                           
┌──(raptor㉿RAPTOR)-[~]
└─$ whatis dig             
dig (1)              - DNS lookup utility
                                                                                                                                                                                                                                           
┌──(raptor㉿RAPTOR)-[~]
└─$ whatis dnsenum
dnsenum (1)          - - multithread script to enumerate information on a domain and to discover non-contiguous IP blocks
                                                                               
```

### dig 
```
dig axfr @nsztm1.digi.ninja zonetransfer.me

; <<>> DiG 9.20.9-1-Debian <<>> axfr @nsztm1.digi.ninja zonetransfer.me
; (1 server found)
;; global options: +cmd
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.        7200    IN      DNSKEY  256 3 7 AwEAAapoL+InQBYx2oi3dI424+dEDFgnVW0cOINfCY3jLrngZxBsEur8 ByhMOQsxoIOYu/7b3c8tj2BwlQquqxZe79QHSW78fK7D+bP/8AosnBG5 K5gJXEvphEtJ9x8/X0Y971XaW9lLmtJ6h4AXsrbgTr2g9KOiPSIbvDPM W8qLMaQkTm89hvPc+NuzrOEOPNhoXs/iPM+SQzrvTBfr6y0w2yPtYYdW I1kN76OQBxh0xjIdlyT0QKiohKq2bybPROJO7K3NlDc8oaOZoXH5/RfL DQzxzXyYSV8fLwimUeulo7YA11I/AHQ7DsUsFu2S2vxGCyR8nmx9gYbN 4sBvTF2i5eM=
zonetransfer.me.        301     IN      TXT     "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.        7200    IN      MX      0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      MX      20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.        7200    IN      A       5.196.105.14
zonetransfer.me.        7200    IN      NS      nsztm1.digi.ninja.
zonetransfer.me.        7200    IN      NS      nsztm2.digi.ninja.
zonetransfer.me.        7200    IN      CERT    PKIX 0 0 MIIDvTCCAqUCFHh5BGzOrlYrXo5h90ipm0aDUEz9MA0GCSqGSIb3DQEB CwUAMIGaMQswCQYDVQQGEwJHQjEYMBYGA1UECAwPU291dGggWW9ya3No aXJlMRIwEAYDVQQHDAlTaGVmZmllbGQxEjAQBgNVBAoMCURpZ2luaW5q YTEQMA4GA1UECwwHSGFja2luZzEYMBYGA1UEAwwPem9uZXRyYW5zZmVy Lm1lMR0wGwYJKoZIhvcNAQkBFg56dG1AZGlnaS5uaW5qYTAeFw0yNTA3 MDIxMzU1MTNaFw0yNjA3MDIxMzU1MTNaMIGaMQswCQYDVQQGEwJHQjEY MBYGA1UECAwPU291dGggWW9ya3NoaXJlMRIwEAYDVQQHDAlTaGVmZmll bGQxEjAQBgNVBAoMCURpZ2luaW5qYTEQMA4GA1UECwwHSGFja2luZzEY MBYGA1UEAwwPem9uZXRyYW5zZmVyLm1lMR0wGwYJKoZIhvcNAQkBFg56 dG1AZGlnaS5uaW5qYTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoC ggEBALzYVM9WlBqOKU1lmnKJkKdIEZOhkscHQktEJORXCismSWV3Ffbs Lw7D3sfCc0h9ecZglsYvFUmEM0I0noYtuHPAlF2+FotVuoFrYuMYrEQo Zs4kuORIEx8pwHMZQUSM6KwVVLIB/FE956GfovgxGxWs33QaTKATAVCh D9KTLf6wVh/eC+0GI6mbvGvjqZFmmV/SYmmkdqEBWB7q3+SByfVrUohC A2GO30dwk6vUBtIj+J+i4SzKzLXIvFEfbCirMPQvdflgwPbjwp+cWG7o UBvfQZfZbaTp+9+V8FoBl0f8fGj/Mae1n0rSV5hnuXot8d3PAoAWQtW3 HJUv1nEboAMCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAXop6ftpV2/r7 tkXqFCsMwub7ZBd12U14nsBon+X7K5Nr6obrVAtnWO+XwD8x2UgvYIQB uRLK9LOX6VYoiWMVrItIN8KRSsin5eJe4tzewsNGrVtkVbbKULViCeBt DgmImk8rkZeWU1uNOsq0t/wd3GUZe2CM9DpKVhPFhc9Uq3pYbAsidYlp SApuuj8ka3L+VruzJVwveyKTUkWAsN1iSv7BGgEF0039WW3IEv1ZP81c AdWFy1fx+tuteM6Iz5xkx1tp0/eLtb39cnKFQnrs8itDG2j3yBc3CClY mw4NNU2nODN4COt7uzXBez6iIFSNqQjVyFyomtPn4ae0cYRHEw==
zonetransfer.me.        300     IN      HINFO   "Casio fx-700G" "Windows XP"
_acme-challenge.zonetransfer.me. 301 IN TXT     "6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN     SRV     0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN   AFSDB   1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200  IN      A       127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN    AFSDB   1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A      202.14.81.230
cmdexec.zonetransfer.me. 300    IN      TXT     "; ls"
contact.zonetransfer.me. 2592000 IN     TXT     "Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200 IN      A       143.228.181.132
deadbeef.zonetransfer.me. 7201  IN      AAAA    dead:beaf::
dr.zonetransfer.me.     300     IN      LOC     53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.    7200    IN      TXT     "AbCdEfG"
email.zonetransfer.me.  2222    IN      NAPTR   1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.  7200    IN      A       74.125.206.26
Hello.zonetransfer.me.  7200    IN      TXT     "Hi to Josh and all his class"
home.zonetransfer.me.   7200    IN      A       127.0.0.1
Info.zonetransfer.me.   7200    IN      TXT     "ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300   IN      NS      intns1.zonetransfer.me.
internal.zonetransfer.me. 300   IN      NS      intns2.zonetransfer.me.
intns1.zonetransfer.me. 300     IN      A       81.4.108.41
intns2.zonetransfer.me. 300     IN      A       167.88.42.94
office.zonetransfer.me. 7200    IN      A       4.23.39.254
ipv6actnow.org.zonetransfer.me. 7200 IN AAAA    2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.    7200    IN      A       207.46.197.32
robinwood.zonetransfer.me. 302  IN      TXT     "Robin Wood"
rp.zonetransfer.me.     321     IN      RP      robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.    3333    IN      NAPTR   2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.   300     IN      TXT     "' or 1=1 --"
sshock.zonetransfer.me. 7200    IN      TXT     "() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200   IN      CNAME   www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A 127.0.0.1
testing.zonetransfer.me. 301    IN      CNAME   www.zonetransfer.me.
vpn.zonetransfer.me.    4000    IN      A       174.36.59.154
www.zonetransfer.me.    7200    IN      A       5.196.105.14
xss.zonetransfer.me.    300     IN      TXT     "'><script>alert('Boo')</script>"
zonetransfer.me.        7200    IN      SOA     nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 140 msec
;; SERVER: 81.4.108.41#53(nsztm1.digi.ninja) (TCP)
;; WHEN: Thu Sep 04 17:24:13 CEST 2025
;; XFR size: 52 records (messages 1, bytes 3339)




```

FIERCE(1)                                                                                                 General Commands Manual                                                                                                FIERCE(1)

NAME
       fierce - DNS scanner that helps locate non-contiguous IP space and hostnames against specified domains.

SYNOPSIS
       fierce [-h] [--domain DOMAIN] [--connect] [--wide] [--traverse TRAVERSE] [--search SEARCH [SEARCH ...]] [--range RANGE] [--delay DELAY] [--subdomains SUBDOMAINS [SUBDOMAINS ...] | --subdomain-file SUBDOMAIN_FILE] [--dns-servers
       DNS_SERVERS [DNS_SERVERS ...] | --dns-file DNS_FILE] [--tcp]

DESCRIPTION
       Fierce is a semi-lightweight scanner that helps locate non-contiguous IP space and hostnames against specified domains. It's really meant as a pre-cursor to nmap, OpenVAS, nikto, etc, since all of those require that you already
       know  what  IP space you are looking for.  This does not perform exploitation and does not scan the whole internet indiscriminately. It is meant specifically to locate likely targets both inside and outside a corporate network.
       Because it uses DNS primarily you will often find mis-configured networks that leak internal address space. That's especially useful in targeted malware.  Originally written by RSnake along with others at  http://ha.ckers.org/.
       This is simply a conversion to Python 3 to simplify and modernize the codebase.

OPTIONS
         -h, --help            show this help message and exit
         --domain DOMAIN       domain name to test
         --connect             attempt HTTP connection to non-RFC 1918 hosts
         --wide                scan entire class c of discovered records
         --traverse TRAVERSE   scan IPs near discovered records, this won't enter adjacent class c's
         --search SEARCH [SEARCH ...]
                               filter on these domains when expanding lookup
         --range RANGE         scan an internal IP range, use cidr notation
         --delay DELAY         time to wait between lookups
         --subdomains SUBDOMAINS [SUBDOMAINS ...]
                               use these subdomains
         --subdomain-file SUBDOMAIN_FILE
                               use subdomains specified in this file (one per line)
         --dns-servers DNS_SERVERS [DNS_SERVERS ...]
                               use these dns servers for reverse lookups
         --dns-file DNS_FILE   use dns servers specified in this file for reverse lookups (one per line)
         --tcp                 use TCP instead of UDP

EXAMPLES
       Something basic:
              $ fierce --domain google.com --subdomains accounts admin ads

       Traverse IPs near discovered domains to search for contiguous blocks with the `--traverse` flag:
              $ fierce --domain facebook.com --subdomains admin --traverse 10

       Limit nearby IP traversal to certain domains with the `--search` flag:
              $ fierce --domain facebook.com --subdomains admin --search fb.com fb.net

       Attempt an `HTTP` connection on domains discovered with the `--connect` flag:
              $ fierce --domain stackoverflow.com --subdomains mail --connect

       Exchange speed for breadth with the `--wide` flag, which looks for nearby domains on all IPs of the [/24](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks) of a discovered domain:
              $ fierce --domain facebook.com --wide

       Zone transfers are rare these days, but they give us the keys to the DNS castle. [zonetransfer.me](https://digi.ninja/projects/zonetransferme.php) is a very useful service for testing for and learning about zone transfers:
              $ fierce --domain zonetransfer.me

       To save the results to a file for later use we can simply redirect output:
              $ fierce --domain zonetransfer.me > output.txt

       Internal networks will often have large blocks of contiguous IP space assigned. We can scan those as well:
              $ fierce --dns-servers 10.0.0.1 --range 10.0.0.0/24

---
