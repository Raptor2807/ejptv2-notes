- # OSI Model

| # | OSI Layer            | Function                                                                                                     | Examples                         |
|---|---------------------|-------------------------------------------------------------------------------------------------------------|---------------------------------|
| 7 | Application Layer   | Provides network services directly to end-users or applications.                                             | HTTP, FTP, IRC, SSH, DNS        |
| 6 | Presentation Layer  | Translates data between the application layer and lower layers. Handles data format translation, encryption, and compression. | SSL/TLS, JPEG, GIF, SSH, IMAP   |
| 5 | Session Layer       | Manages sessions or connections between applications. Handles synchronization, dialog control, and token management (interhost communication). | APIs, NetBIOS, RPC              |
| 4 | Transport Layer     | Ensures end-to-end communication and provides flow control.                                                 | TCP, UDP                        |
| 3 | Network Layer       | Responsible for logical addressing and routing (logical addressing).                                         | IP, ICMP, IPSec                 |
| 2 | Data Link Layer     | Manages access to the physical medium and provides error detection. Responsible for framing, addressing, and error checking of data frames (physical addressing). | Ethernet, PPP, Switches, etc.   |
| 1 | Physical Layer      | Deals with the physical connection between devices.                                                         | USB, Ethernet Cables, Coax, Fiber, Hubs, etc. |
- TCP only operates on IP, it can't work with any other protocol.
- the OSI model is just a reference model. it is not necessary that every network implements it. 

### [[Network Layer]]
### [[Transport Layer & NETSTAT command]]
