# Protocols and Port Numbers

## What is a Port

A port is a logical channel that directs incoming traffic 
to the correct service on a device.

IP Address = apartment building address
Port Number = apartment number inside building

Data arrives at your IP
Port number decides which service receives it

## Port Number Ranges

| Range | Type | Description |
|-------|------|-------------|
| 0 - 1023 | Well-known ports | reserved for standard services |
| 1024 - 49151 | Registered ports | used by applications |
| 49152 - 65535 | Dynamic ports | temporary outgoing connections |

## Complete Protocol and Port Reference

### Communication Protocols

| Protocol | Full Name | Port | TCP/UDP | Layer | Purpose |
|----------|-----------|------|---------|-------|---------|
| HTTP | Hypertext Transfer Protocol | 80 | TCP | Application | unencrypted web |
| HTTPS | HTTP Secure | 443 | TCP | Application | encrypted web |
| FTP | File Transfer Protocol | 20/21 | TCP | Application | file transfer |
| SSH | Secure Shell | 22 | TCP | Application | secure remote access |
| Telnet | Telnet | 23 | TCP | Application | insecure remote access |
| SMTP | Simple Mail Transfer | 25 | TCP/UDP | Application | sending email |
| SMTP TLS | SMTP encrypted | 587 | TCP | Application | sending email encrypted |
| POP3 | Post Office Protocol | 110 | TCP/UDP | Application | receiving email |
| POP3 SSL | POP3 encrypted | 995 | TCP | Application | receiving email encrypted |
| IMAP | Internet Message Access | 143 | TCP | Application | email sync multiple devices |
| IMAP TLS | IMAP encrypted | 993 | TCP | Application | encrypted email sync |
| DNS | Domain Name System | 53 | Both | Application | domain to IP translation |
| DHCP | Dynamic Host Config | 67/68 | UDP | Application | automatic IP assignment |
| SNMP | Simple Network Management | 161 | UDP | Application | network device monitoring |
| TFTP | Trivial File Transfer | 69 | UDP | Application | simple file transfer |
| RDP | Remote Desktop Protocol | 3389 | TCP | Application | Windows remote desktop |

### Security Protocols

| Protocol | Port | Purpose |
|----------|------|---------|
| HTTPS | 443 | encrypted web using SSL/TLS |
| SFTP | 22 | secure file transfer using SSH |
| SSH | 22 | encrypted remote shell |

### Network Layer Protocols

| Protocol | Port | Layer | Purpose |
|----------|------|-------|---------|
| ICMP | none | Network | error reporting, ping |
| ARP | none | Data Link | IP to MAC translation |
| NAT | none | Network/Transport | private to public IP translation |

## Why Some Protocols Have Two Ports

| Protocol | Unencrypted | Encrypted | Reason |
|----------|------------|-----------|--------|
| HTTP/HTTPS | 80 | 443 | SSL/TLS encryption added |
| SMTP | 25 | 587 | TLS encryption added |
| POP3 | 110 | 995 | SSL/TLS encryption added |
| IMAP | 143 | 993 | TLS encryption added |

Always use encrypted versions — unencrypted sends 
everything in plain text including passwords.

## Most Important Ports to Memorize

| Port | Protocol | Why Important for Security |
|------|----------|--------------------------|
| 22 | SSH | most attacked port worldwide |
| 23 | Telnet | completely insecure, never use |
| 25 | SMTP | spam and email attacks |
| 53 | DNS | DNS poisoning, DNS tunneling |
| 80 | HTTP | web attacks, unencrypted traffic |
| 443 | HTTPS | secure web traffic |
| 3389 | RDP | Windows remote desktop attacks |
| 445 | SMB | EternalBlue, ransomware attacks |

## How Ports Work Together — Real Example

When you visit google.com:

Step 1: Your PC asks DNS server → port 53 (UDP)
"What is google.com's IP address?"

Step 2: DNS replies with IP address → port 53

Step 3: Your PC connects to Google → port 443 (TCP)
Three way handshake occurs

Step 4: Your PC sends random source port → port 52341
Google sends response back to your port 52341

Step 5: Browser displays the page

## Cybersecurity Relevance

### Firewall Rules Based on Ports

Block port 23 = nobody can use Telnet
Allow only 443 = only HTTPS traffic permitted
Block port 3389 = no RDP access from outside

### Port Scanning with Nmap
```bash
nmap -p 22,80,443 target.com    # scan specific ports
nmap -p- target.com              # scan all 65535 ports
nmap -sV target.com              # detect service versions
```

### Real tcpdump Log Analysis

Reading a real network capture:

13:24:32.192571 IP 192.51.100.15.52444 > 203.0.113.2.domain: 35084+ A? yummyrecipesforme.com

Breaking down:
192.51.100.15 = source IP (your computer)
52444 = source port (random dynamic port)
203.0.113.2 = destination IP (DNS server)
domain = port 53 (DNS)
A? = asking for A record (IP address)


This shows a DNS query over UDP port 53 — exactly 
what happens every time you visit a website.