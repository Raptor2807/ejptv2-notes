- The Transport Layer is the fourth layer of the OSI (Open Systems Interconnection) model.
- Responsible for facilitating communication between two devices across a network.
- Ensures **reliable, end-to-end communication** by handling:
  - Error detection
  - Flow control
  - Segmentation of data into smaller units
- Provides **ordered and reliable delivery** of data.

---

## Transport Layer Protocols

- **TCP (Transmission Control Protocol):**
  - Connection-oriented protocol
  - Reliable and ordered data delivery

- **UDP (User Datagram Protocol):**
  - Connectionless protocol
  - Faster, but no guarantees for order or reliability

---

# TCP (Transmission Control Protocol)

- Connection-oriented, reliable protocol at Transport Layer (Layer 4).
- Provides dependable and ordered delivery between two devices.
- Ensures application data is received accurately and in the correct order.

### Key Characteristics
- **Connection-Oriented:**
  - Establishes a connection before exchanging data (virtual circuit).
- **Reliability:**
  - Uses acknowledgments (ACK) and retransmissions for guaranteed delivery.
- **Ordered Data Transfer:**
  - Reorders segments if they arrive out of sequence.

---

# TCP Three-Way Handshake

Process for establishing a reliable connection between client and server before data exchange:

1. **SYN:**  
   - Client sends a TCP segment with SYN flag set.
   - Includes Initial Sequence Number (ISN).

2. **SYN-ACK:**  
   - Server responds with SYN + ACK flags set.
   - ACK number = client’s ISN + 1. (ISN = Initial SEQ no.)
   - Server generates its own ISN.

3. **ACK:**  
   - Client sends ACK with ACK number = server’s ISN + 1.
   - Connection is now established.
   - Data transmission begins.

---

# TCP Header Fields (Key)

- **Source Port (16 bits):** Identifies sending port.
- **Destination Port (16 bits):** Identifies receiving port.
- Other fields include:
  - Sequence Number
  - Acknowledgment Number
  - Header Length
  - Flags (SYN, ACK, FIN, RST, PSH, URG)
  - Window Size
  - Checksum
  - Urgent Pointer

---

# TCP Control Flags

- **SYN:** Initiate a connection
- **ACK:** Acknowledge received data
- **FIN:** Terminate connection
- **RST:** Reset connection
- **PSH:** Push function (deliver data immediately)
- **URG:** Urgent pointer field significant

### Connection Establishment
- SYN set, ACK clear, FIN clear (Client Request)
- SYN + ACK set (Server Response)

### Connection Termination
- ACK set, FIN set (Close request)

---

# TCP Port Ranges

- **Port Numbers:** 16-bit unsigned integers (0–65535)

### Ranges
- **Well-Known Ports (0–1023):**
  - Reserved for standard services (HTTP, FTP, SSH, SMTP, etc.)
- **Registered Ports (1024–49151):**
  - Assigned by IANA to specific applications (e.g., 3306 MySQL, 3389 RDP)
- **Dynamic/Private Ports (49152–65535):**
  - Used by client applications for ephemeral communication

---

# UDP (User Datagram Protocol)

- Lightweight, connectionless transport protocol.
- Focuses on speed and simplicity rather than reliability.
- No connection setup — each datagram is independent.

### Characteristics
- **Connectionless:** No session state, no handshakes.
- **Unreliable:** No ACKs, retransmissions, or ordering guarantees.
- **Use Cases:** Real-time apps (VoIP, streaming, gaming).

---

# TCP vs UDP (Comparison)

| Feature         | UDP                     | TCP                                            |
| --------------- | ----------------------- | ---------------------------------------------- |
| **Connection**  | Connectionless          | 3-Way Handshake (Connection-oriented)          |
| **Reliability** | No delivery guarantee   | Reliable, ordered delivery with retransmission |
| **Header Size** | Small (low overhead)    | Larger (more control information)              |
| **Use Cases**   | VoIP, streaming, gaming | HTTP, Email, File Transfer                     |
| **Examples**    | DNS, DHCP, SNMP, VoIP   | HTTP, FTP, Telnet, SMTP, HTTPS                 |


---
Here’s a clear Markdown note for `netstat -antp` in Kali Linux:

````markdown
# netstat -antp Command (Kali Linux)

## Purpose
Used to display **active TCP connections** and **listening ports** on the system, along with the process IDs (PIDs) that own them.

## Command Breakdown
```bash
netstat -antp
````

- **`-a`** → Show **all** sockets (listening + established connections)
    
- **`-n`** → Show addresses and ports as **numbers** (no DNS/service name resolution)
    
- **`-t`** → Show **TCP** connections only
    
- **`-p`** → Show **PID/Program name** for each socket
    

## Sample Output

```
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 192.168.1.10:22        192.168.1.100:54321    ESTABLISHED 1234/sshd
tcp        0      0 0.0.0.0:80             0.0.0.0:*              LISTEN      2345/apache2
```

### Columns Explained

- **Proto** → Protocol in use (`tcp`)
    
- **Recv-Q / Send-Q** → Queued data waiting to be read/sent
    
- **Local Address** → IP and port on the local machine
    
- **Foreign Address** → Remote IP and port connected to the local socket
    
- **State** → Status of the connection
    
    - `LISTEN` → Service is waiting for incoming connections
        
    - `ESTABLISHED` → Active connection with remote host
        
    - `TIME_WAIT`, `CLOSE_WAIT`, etc. → Connection teardown states
        
- **PID/Program name** → Process using the socket (helps identify service)
    

## When to Use

- Enumerate all **open TCP ports** on your machine
    
- Identify **active network connections**
    
- Determine **which process** owns a connection (useful for debugging or security analysis)
    

## Notes for Pentesters

- Helpful for finding services running on unexpected ports.
    
- Good first step during local enumeration to see what services may be exploitable.
    
- Combine with `lsof -i` for cross-verification.
    

