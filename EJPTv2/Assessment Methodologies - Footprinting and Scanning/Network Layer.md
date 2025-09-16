- responsible for logical addressing, routing, and forwarding data packets between devices across different networks
- primary goal is to determine the optimal path for data to travel form one point to other
### Network Layer Protocols
Several key protocols operate at the network layer (Layer 3) of the OSI model. Here are some prominent network layer protocols:
Internet Protocol (IP):
+ IPv4 (Internet Protocol version 4): The most widely used version of IP, employing 32-bit addresses and providing the foundation for communication on the Internet.
+ IPv6 (Internet Protocol version 6): Developed to address the limitations of IPv4, it uses 128-bit addresses and offers an exponentially larger address space.
Internet Control Message Protocol (ICMP):
+ Used for error reporting and diagnostics. ICMP messages include ping (echo request and echo reply), traceroute, and various error messages.
---

### Internet Protocol (IP) Functionality
Logical Addressing:
+ IP addresses serve as logical addresses assigned to network interfaces. These addresses uniquely identify each device on a network.
+ IP addresses are hierarchical and structured based on network classes, subnets, and CIDR (Classless Inter-Domain Routing) notation.
Packet Structure:
+ IP organizes data into packets for transmission across networks. Each packet consists of a header and payload.
+ The header contains essential information, including the source and destination IP addresses, version number, time-to-live (TTL), and protocol type.
---
### Internet Protocol (IP) Functionality
Fragmentation and Reassembly:
+ IP allows for the fragmentation of large packets into smaller fragments when traversing networks with varying Maximum Transmission Unit (MTU) sizes.
+ The receiving host reassembles these fragments to reconstruct the original packet.
IP Addressing Types:
+ IP addresses can be classified into three types: unicast (one-to-one communication), broadcast (one-to-all communication within a subnet), and multicast (one-to-many communication to a selected group of devices).
Subnetting:
+ Subnetting is a technique that divides a large IP network into smaller, more manageable sub-networks. It enhances network efficiency and security.
+ *DMZ = demilitarized zone* is a good example for subnetting.
	+ typically used for hosting webservers
---
### Internet Protocol (IP) Functionality
Internet Control Message Protocol (ICMP):
+ ICMP is closely associated with IP and is used for error reporting and diagnostics. Common ICMP messages include echo request and echo reply, which are used in the ping utility.
Dynamic Host Configuration Protocol (DHCP):
+ DHCP is often used in conjunction with IP to dynamically assign IP addresses to devices on a network, simplifying the process of network configuration.
---
### IPv4
# IPv4 Header Format

| **Field**              | **Purpose** |
|-----------------------|-------------|
| **Version (4 bits)**  | Indicates the version of the IP protocol being used. For IPv4, the value is 4. |
| **Header Length (4 bits)** | Specifies the length of the IPv4 header in 32-bit words. Minimum value is 5 (20-byte header), maximum is 15 (60-byte header). |
| **Type of Service (8 bits)** | Originally for Quality of Service. Contains DSCP (Differentiated Services Code Point) and ECN (Explicit Congestion Notification) fields for packet priority and congestion control. |
| **Total Length (16 bits)** | Total size of IP packet (header + payload). Maximum size is 65,535 bytes. |
| **Identification (16 bits)** | Used to reassemble fragmented packets. All fragments share the same ID. |
| **Flags (3 bits)** | Control fragmentation: <br>• Bit 0 (Reserved) = always 0 <br>• Bit 1 (DF) = Don't Fragment <br>• Bit 2 (MF) = More Fragments follow |
| **Time-to-Live (TTL, 8 bits)** | Max number of hops before packet is discarded. Decremented at each hop. |
| **Protocol (8 bits)** | Identifies the next-layer protocol (e.g., TCP=6, UDP=17, ICMP=1). |
| **Header Checksum (16 bits)** | Validates integrity of the IPv4 header. Routers recompute this at each hop. |
| **Source IP Address (32 bits)** | IPv4 address of the sender. |
| **Destination IP Address (32 bits)** | IPv4 address of the receiver. |
| **Options (Variable)** | Optional field, used for testing, debugging, or security (rarely used today). |
| **Padding** | Ensures the header ends on a 32-bit boundary. |

---

**Note:**  
- First 4 bits = IP version (4 for IPv4).  
- 32 bits starting at bit position 96 = Source IP address.  
- Next 32 bits = Destination IP address.
# IPv4 Header Format

## Bit-Level Layout (32-bit rows)
0 1 2 3  
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1  
+---+---+---+---+-----------+-----------------------------------+  
|Ver| IHL | DSCP | ECN | Total Length |  
+---+---+---+---+-----------+-----------------------------------+  
| Identification |Flags| Fragment Offset |  
+-------------------------------+-----+-------------------------+  
| Time To Live | Protocol | Header Checksum |  
+---------------------------------------------------------------+  
| Source IP Address |  
+---------------------------------------------------------------+  
| Destination IP Address |  
+---------------------------------------------------------------+  
| Options (if any) ... |  
+---------------------------------------------------------------+  
| Padding (if needed) |  
+---------------------------------------------------------------+

---
### # Reserved & Special-Use IPv4 Address Ranges (RFC 5735 / RFC 6890)

| **Address Range**       | **CIDR**        | **Purpose / Use** |
|------------------------|----------------|------------------|
| 0.0.0.0 – 0.255.255.255 | 0.0.0.0/8     | "This" network. Used as source address before a device gets an IP (e.g., DHCP discovery). |
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8  | Private network range. Not routable on the Internet. |
| 100.64.0.0 – 100.127.255.255 | 100.64.0.0/10 | Carrier-Grade NAT (CGNAT) address space (used by ISPs). |
| 127.0.0.0 – 127.255.255.255 | 127.0.0.0/8 | Loopback addresses (localhost). Example: 127.0.0.1 for internal testing. |
| 169.254.0.0 – 169.254.255.255 | 169.254.0.0/16 | Link-Local addresses (APIPA). Auto-assigned when DHCP fails. |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 | Private network range (Class B private addresses). |
| 192.0.0.0 – 192.0.0.7 | 192.0.0.0/29 | IETF Protocol Assignments. |
| 192.0.2.0 – 192.0.2.255 | 192.0.2.0/24 | TEST-NET-1. Reserved for documentation and examples. |
| 192.88.99.0 – 192.88.99.255 | 192.88.99.0/24 | IPv6 to IPv4 relay (deprecated). |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 | Private network range (Class C private addresses). |
| 198.18.0.0 – 198.19.255.255 | 198.18.0.0/15 | Network benchmark tests (performance testing). |
| 198.51.100.0 – 198.51.100.255 | 198.51.100.0/24 | TEST-NET-2. Reserved for documentation and examples. |
| 203.0.113.0 – 203.0.113.255 | 203.0.113.0/24 | TEST-NET-3. Reserved for documentation and examples. |
| 224.0.0.0 – 239.255.255.255 | 224.0.0.0/4 | IPv4 Multicast range. |
| 240.0.0.0 – 255.255.255.254 | 240.0.0.0/4 | Reserved for future use. |
| 255.255.255.255 | /32 | Limited Broadcast address (broadcast to all hosts on local network). |

---

**Key Points**
- **Private IP Ranges:** `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` — used for LANs.
- **Loopback:** `127.0.0.1` for local testing.
- **APIPA:** `169.254.0.0/16` auto-assigned when DHCP fails.
- **Multicast:** `224.0.0.0/4` for group communication (e.g., streaming, routing protocols).
- **Documentation Ranges:** `192.0.2.0/24`, `198.51.100.0/24`, `203.0.113.0/24` — safe for educational use.
---
