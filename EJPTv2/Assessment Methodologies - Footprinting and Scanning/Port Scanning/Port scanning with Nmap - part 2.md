- Using Wireshark we can capture the packets while we do nmap scan
	- Response to a SYN ping
		- SYNACK : Open port
			- in response nmap sends an RST to terminate the connection
		- RST : Closed port
		- NO response : packet might be filtered by firewall and potentially dropped. 
			- hence, can't say for sure if the port is open or not
- #### Why is TCP SYN scan considered as a stealth scan by nmap?
	- bcoz most of the ports expect tcp SYN packets and SYN packets aren't uncommon on the internet, so it doesn't raise alarms
	- also, this never completes the three way handshake, so there are no logs generated on the target system
### TCP connect scan
- it is the default port scanning option used when nmap is run without the root or pseudo privileges 
- In this scan the TCP three way hand shake is done and the reason why it is not used is bcoz it is very loud on network and prone to detection by IDS
- ```
  nmap -Pn -sT 10.4.24.205
  ```
  - after the connection is established the nmap send TCP RST packet to terminate the connection.

### Scanning for UDP ports
```
nmap -sU 10.4.20.205
```
