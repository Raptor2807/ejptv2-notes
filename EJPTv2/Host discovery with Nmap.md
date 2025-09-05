- the first step to pentest would be discovering all the devices on your target network, you can do it using host discovery with Nmap
- we are going to do a ping sweep or a ping scan with nmap in order to discover the hosts
- Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are
       in use, and dozens of other characteristics. While Nmap is commonly used for security audits, many systems and network administrators find it useful for routine tasks such as network inventory, managing service upgrade
       schedules, and monitoring host or service uptime.

       The output from Nmap is a list of scanned targets, with supplemental information on each depending on the options used. Key among that information is the “interesting ports table”.  That table lists the port number and
       protocol, service name, and state. The state is either open, filtered, closed, or unfiltered.  Open means that an application on the target machine is listening for connections/packets on that port.  Filtered means that a
       firewall, filter, or other network obstacle is blocking the port so that Nmap cannot tell whether it is open or closed.  Closed ports have no application listening on them, though they could open up at any time. Ports are
       classified as unfiltered when they are responsive to Nmap's probes, but Nmap cannot determine whether they are open or closed. Nmap reports the state combinations open|filtered and closed|filtered when it cannot determine which
       of the two states describe a port. The port table may also include software version details when version detection has been requested. When an IP protocol scan is requested (-sO), Nmap provides information on supported IP
       protocols rather than listening ports.

```
- ip a s
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:8c:1a:2f brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute eth0
       valid_lft 84800sec preferred_lft 84800sec
    inet6 fd17:625c:f037:2:320:deba:415:1c94/64 scope global temporary dynamic 
       valid_lft 86073sec preferred_lft 14073sec
    inet6 fd17:625c:f037:2:a00:27ff:fe8c:1a2f/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 86073sec preferred_lft 14073sec
    inet6 fe80::a00:27ff:fe8c:1a2f/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

```
- here on eth0 we can see my IP and subnet mask
	- inet 10.0.2.15/24 brd 10.0.2.255
- -sn : used to tell nmap to not to perform port scan
```
- sudo nmap -sn 10.0.2.15/24
[sudo] password for raptor: 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-09-04 21:01 CEST
Nmap scan report for localhost (10.0.2.2)
Host is up (0.00048s latency).
MAC Address: 52:55:0A:00:02:02 (Unknown)
Nmap scan report for localhost (10.0.2.3)
Host is up (0.00050s latency).
MAC Address: 52:55:0A:00:02:03 (Unknown)
Nmap scan report for localhost (10.0.2.15)
Host is up.
Nmap done: 256 IP addresses (3 hosts up) scanned in 2.04 seconds

```
----
## Netdiscover
```
                                                                                                                                                                                                                                           
┌──(raptor㉿RAPTOR)-[~]
└─$ netdiscover -i eth0 -r 10.0.2.0/24 
 Currently scanning: Finished!   |   Screen View: Unique Hosts                                                                                                                                                                            
                                                                                                                                                                                                                                          
 2 Captured ARP Req/Rep packets, from 2 hosts.   Total size: 128                                                                                                                                                                          
 _____________________________________________________________________________
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname      
 -----------------------------------------------------------------------------
 10.0.2.2        52:55:0a:00:02:02      1      64  Unknown vendor                                                                                                                                                                         
 10.0.2.3        52:55:0a:00:02:03      1      64  Unknown vendor    
```

---
### The main difference between nmap and netdiscover is that nmap uses ICMP packets to ping the hosts, while netdiscover uses ARP packets
