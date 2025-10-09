- using timing templates used to increase speed of scans or sometimes decrease the speed
	- Slowdown : 
		- when you're dealing with an IDS, slowing down your scan will decrease the amount of packets being sent which will in turn make the network activity look less suspicious.
		- sometimes you're dealing with a network which is using very old network hardware - in this case high number of packets being sent can cause unintended DOS or may cause the switches or the routers to crash
- timing templates range from T0-T5 
	- T0 : Paranoid used for IDS evasion
	- T1 : Sneaky
	- T2 : Polite
	- T3 : Default
	- T4 : Aggressive
	- T5 : Insane
- --scan-delay : Adjust the delay between the probes
	- Adjust the time duration between each packet being sent
- --host-timeout : give up on the target after this long
	- this option should be used very carefully as certain hosts might take longer to respond and if you set it low then there is a higher chance that you might miss some systems in host discovery

|Template|Name|Short description|
|--:|---|---|
|`-T0`|Paranoid|Extremely slow. One probe at a time. For heavy IDS/IPS evasion.|
|`-T1`|Sneaky|Very slow. Low packet rate to avoid detection.|
|`-T2`|Polite|Throttles bandwidth and CPU. Reduces impact on network.|
|`-T3`|Normal|Default. Balanced speed and accuracy.|
|`-T4`|Aggressive|Faster. Assumes stable network. More likely to trigger alerts.|
|`-T5`|Insane|Maximum speed. High packet rate. Only on very reliable LANs.|

## Usage

```bash
nmap -T<0-5> [other options] <target>
```

## Examples

```bash
# Balanced SYN scan
nmap -sS -T3 10.0.0.5

# Faster service/version detection on a stable network
nmap -sS -sV -T4 10.4.27.83

# Slow, stealthy scan to try to evade IDS
nmap -sS -T1 10.4.27.83
```

## Practical notes

- Higher `-T` increases speed and false negatives risk.
    
- Lower `-T` improves stealth and reliability on noisy networks.
    
- Combine with other evasion flags (`-f`, `--data-length`, `--ttl`, `-Pn`) carefully.
    
- `-T4` and `-T5` may overwhelm slower targets or networks.