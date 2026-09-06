# Network Security Concepts

## Defense in Depth

No single security control is enough. Layer multiple 
defenses so if one fails, another catches the attack.

Internet
↓
Perimeter Firewall
↓
DMZ (public servers)
↓
IDS/IPS
↓
Internal Firewall
↓
Network Segmentation
↓
Host-based Firewall
↓
Critical Assets

## Network Segmentation

Dividing network into separate zones to contain breaches.

Without segmentation:
Attacker breaches one device
→ Has access to entire network
→ Can reach all servers, databases, devices

With segmentation:
Attacker breaches one device
→ Contained in that network segment
→ Cannot reach other segments without crossing firewall
→ Breach limited in damage


## VPN — Virtual Private Network

Creates encrypted tunnel over public internet.

Employee at home
↓
Encrypted VPN tunnel through internet
↓
Company network

Types:
| Type | Use case |
|------|---------|
| Remote Access VPN | employee connecting from home |
| Site-to-Site VPN | connecting two office networks |
| SSL VPN | browser-based, no client needed |

## NAT — Network Address Translation

Translates private IP addresses to public IP addresses.

Private IP ranges (cannot route on internet):
10.0.0.0 - 10.255.255.255
172.16.0.0 - 172.31.255.255
192.168.0.0 - 192.168.255.255

Your home devices use private IPs
Router uses NAT to translate to one public IP
All devices share one public IP address


Security benefit:

Internal devices hidden behind NAT
External attackers cannot directly reach internal IPs
Only public IP is visible from internet

## Common Network Attacks

### Man in the Middle (MitM)

Normal: Client ←→ Server
MitM: Client ←→ Attacker ←→ Server

Attacker intercepts all communication
Can read, modify, or inject data
Achieved via: ARP spoofing, Rogue DHCP, DNS poisoning


### Denial of Service (DoS)

Attacker floods target with traffic
Target overwhelmed, cannot serve legitimate users
Types:
→ SYN Flood: incomplete TCP handshakes
→ UDP Flood: massive UDP traffic
→ ICMP Flood: ping flood
→ HTTP Flood: web request flood


### Distributed DoS (DDoS)

Attack comes from thousands of sources simultaneously
Much harder to block — cannot block single IP
Sources are often compromised devices (botnet)

### Port Scanning
```bash
# Attacker identifies open services
nmap -sS target.com        # SYN scan (stealthy)
nmap -sV target.com        # version detection
nmap -p- target.com        # all ports
nmap -A target.com         # aggressive scan

# Defender uses same tool to check own exposure
nmap -sS localhost
```

### Packet Sniffing
```bash
# Capture network traffic
sudo tcpdump -i eth0
sudo tcpdump -i eth0 port 80
sudo tcpdump -i eth0 -w capture.pcap

# Analyze captured traffic in Wireshark
```

## Network Monitoring for SOC Analysts

### Key Log Sources

| Source | What it shows |
|--------|--------------|
| Firewall logs | blocked and allowed connections |
| DNS logs | domain lookups — detect malware C2 |
| DHCP logs | device IP assignments |
| NetFlow | traffic volume and patterns |
| IDS alerts | detected attack signatures |

### Indicators of Compromise in Network Traffic

Suspicious DNS queries:
→ Random looking domain names (malware C2)
→ DNS queries to unusual countries
→ High volume of DNS failures

Suspicious connections:
→ Connections to known malicious IPs
→ Unusual ports (not 80, 443, 22)
→ Large data transfers outbound (exfiltration)
→ Connections at unusual hours

Port scanning indicators:
→ One IP connecting to many ports rapidly
→ Sequential port connections
→ Many connection failures from same source

### Reading tcpdump Output

13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? google.com

Breaking down:
13:24:32.192571 = timestamp
192.51.100.15 = source IP
52444 = source port (dynamic)
203.0.113.2 = destination IP
domain = port 53 (DNS)
A? = DNS A record query
google.com = domain being looked up


### Wireshark Filters for SOC Work

Filter by protocol

http
dns
tcp
udp

Filter by IP

ip.addr == 192.168.1.5
ip.src == 192.168.1.5
ip.dst == 192.168.1.5

Filter by port

tcp.port == 80
udp.port == 53

Filter failed connections

tcp.flags.reset == 1

Find large transfers (possible exfiltration)

frame.len > 1000


## Shared Responsibility in Cloud Networking

Cloud Provider responsible for:
→ Physical network infrastructure
→ Hypervisor security
→ Network between data centers

Customer responsible for:
→ Virtual network configuration
→ Security group rules (cloud firewall)
→ VPN configuration
→ Traffic encryption
→ Access controls

## Key Security Protocols Summary

| Protocol | Port | Purpose | Replaces |
|----------|------|---------|---------|
| HTTPS | 443 | encrypted web | HTTP port 80 |
| SSH | 22 | encrypted remote access | Telnet port 23 |
| SFTP | 22 | encrypted file transfer | FTP port 21 |
| IMAPS | 993 | encrypted email receive | IMAP port 143 |
| POP3S | 995 | encrypted email receive | POP3 port 110 |
| SMTPS | 587 | encrypted email send | SMTP port 25 |

Always use encrypted version of every protocol.
Unencrypted protocols send everything in plain text —
passwords, emails, file contents — readable by anyone 
sniffing the network.