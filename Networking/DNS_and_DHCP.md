# DNS and DHCP

## DNS — Domain Name System

DNS translates human-readable domain names into 
IP addresses that computers use.

You type: google.com
DNS translates to: 142.250.185.46
Browser connects to: 142.250.185.46


Without DNS you would need to memorize IP addresses 
for every website — impossible at scale.

## How DNS Works — Step by Step

Step 1: You type google.com in browser

Step 2: Your PC checks local DNS cache
"Have I looked this up recently?"
If yes → use cached IP → skip remaining steps

Step 3: Your PC asks local DNS resolver (your router)
"What is google.com's IP?"

Step 4: Router asks Root DNS server
"Who handles .com domains?"

Step 5: Root server replies
"Ask the .com nameserver at this IP"

Step 6: Router asks .com nameserver
"Who handles google.com?"

Step 7: .com nameserver replies
"Ask Google's nameserver at this IP"

Step 8: Router asks Google's nameserver
"What is google.com's IP?"

Step 9: Google's nameserver replies
"google.com = 142.250.185.46"

Step 10: Your browser connects to 142.250.185.46

## DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| A | maps domain to IPv4 address | google.com → 142.250.185.46 |
| AAAA | maps domain to IPv6 address | google.com → 2607:f8b0::... |
| MX | mail server for domain | gmail.com mail server |
| CNAME | alias pointing to another domain | www → google.com |
| TXT | text information about domain | SPF records for email |
| NS | nameserver for domain | which server handles DNS |
| PTR | reverse lookup IP to domain | 142.250.185.46 → google.com |

## DNS Port and Protocol

Port: 53
Protocol: UDP for normal queries (fast, small packets)
TCP for large responses (zone transfers, DNSSEC)


## DNS Security Attacks

### DNS Poisoning / Cache Poisoning

Attacker injects false DNS records into cache
Victim asks "what is bank.com's IP?"
Poisoned DNS replies with attacker's IP
Victim connects to fake bank website
Credentials stolen


### DNS Tunneling

Attacker encodes data inside DNS queries
DNS traffic rarely blocked by firewalls
Data exfiltrated through DNS
Bypasses security controls completely

### DNS Amplification DDoS

Attacker sends small DNS query
Spoofs victim's IP as source
DNS server sends large response to victim
Victim overwhelmed with traffic

### Subdomain Enumeration
```bash
# Used in reconnaissance phase
sublist3r -d google.com
# Finds all subdomains
# Identifies forgotten/vulnerable subdomains
# First step of many attacks
```

## DHCP — Dynamic Host Configuration Protocol

DHCP automatically assigns IP addresses and network 
configuration to devices joining a network.

Without DHCP:

Every device needs manual IP configuration
Network administrator sets each device individually
Extremely time consuming at scale


With DHCP:

Device joins network
DHCP automatically assigns:
→ IP address
→ Subnet mask
→ Default gateway
→ DNS server addresses

## How DHCP Works — DORA Process

D — Discover:
New device broadcasts: "Is there a DHCP server here?"
Sent to 255.255.255.255 (broadcast address)

O — Offer:
DHCP server responds: "I can give you this IP: 192.168.1.50"

R — Request:
Device responds: "Yes please, I'll take 192.168.1.50"

A — Acknowledge:
DHCP server: "Confirmed, 192.168.1.50 is yours for 24 hours"

## DHCP Port Numbers

| Role | Port | Protocol |
|------|------|---------|
| DHCP Server | 67 | UDP |
| DHCP Client | 68 | UDP |

UDP is used because:

Device just joined network and needs IP immediately
No time for TCP three way handshake
Speed critical — UDP perfect here


## DHCP Security Attacks

### Rogue DHCP Server

Attacker sets up fake DHCP server on network
Device joins network, sends Discover broadcast
Both real and fake DHCP server respond
If fake server responds faster:
→ Assigns attacker-controlled DNS server
→ Assigns attacker as default gateway
→ All victim traffic now routes through attacker
→ Man in the middle attack achieved

### DHCP Starvation

Attacker requests all available IP addresses
Legitimate devices cannot get IP addresses
Cannot join network
Denial of service against DHCP


## ARP — Address Resolution Protocol

ARP translates IP addresses to MAC addresses within 
the same network.

Device knows destination IP: 192.168.1.5
Does not know destination MAC address
Sends ARP broadcast: "Who has 192.168.1.5?"
Device with that IP responds: "I do, my MAC is aa:bb:cc:dd:ee:ff"
Sender caches this in ARP table
Communication can now proceed


### ARP Spoofing Attack

Attacker sends fake ARP replies
"I am the router — my MAC is attacker's MAC"
Victim updates ARP cache with fake entry
All traffic meant for router goes to attacker instead
Attacker captures and forwards traffic
Man in the middle achieved

Detection:
```bash
# Check ARP table for suspicious entries
arp -a

# Two different IPs with same MAC = suspicious
# Could indicate ARP spoofing attack
```