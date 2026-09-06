# TCP and UDP Protocols

## TCP — Transmission Control Protocol

TCP is connection-oriented — establishes connection before 
sending any data.

### Three Way Handshake — How TCP Connects

Step 1: Client → SYN → Server
"I want to connect"

Step 2: Client ← SYN/ACK ← Server
"OK I hear you, ready"

Step 3: Client → ACK → Server
"Great, let's communicate"

Connection established — data flows


### TCP Characteristics

| Feature | Detail |
|---------|--------|
| Connection | required before data transfer |
| Reliability | guarantees delivery |
| Order | data arrives in correct order |
| Error checking | retransmits lost packets |
| Speed | slower due to overhead |
| Use cases | web, email, file transfer, SSH |

## UDP — User Datagram Protocol

UDP is connectionless — sends data immediately without 
establishing connection first.

### UDP Characteristics

| Feature | Detail |
|---------|--------|
| Connection | not required |
| Reliability | no guarantee of delivery |
| Order | packets may arrive out of order |
| Error checking | minimal |
| Speed | faster, less overhead |
| Use cases | DNS, DHCP, video streaming, gaming, VoIP |

## TCP vs UDP Comparison

| | TCP | UDP |
|--|-----|-----|
| Connection | required | not required |
| Reliability | guaranteed | not guaranteed |
| Speed | slower | faster |
| Overhead | high | low |
| Use when | data must arrive complete | speed matters more |

## Simple Analogy

TCP = registered mail
→ confirmation of delivery
→ slow but guaranteed
→ used for important documents

UDP = dropping a leaflet through letterbox
→ no confirmation
→ fast but no guarantee
→ used for time-sensitive data


## Application Protocols and Their Transport

| Protocol | Uses | Reason |
|----------|------|--------|
| HTTP | TCP | webpage must load completely |
| HTTPS | TCP | secure web, must be complete |
| FTP | TCP | file must arrive whole |
| SMTP | TCP | email must arrive complete |
| SSH | TCP | secure connection required |
| DNS | Both | small queries=UDP, large responses=TCP |
| DHCP | UDP | speed needed for IP assignment |
| SNMP | UDP | small frequent monitoring updates |
| TFTP | UDP | simple fast local transfers |

## Cybersecurity Relevance

### TCP SYN Flood Attack

Attacker sends thousands of SYN packets
Never completes the handshake
Server waits for ACK that never comes
Server resources exhausted
Legitimate users cannot connect
This is a Denial of Service (DoS) attack


### Port Scanning

Attacker sends SYN to every port
Port responds with SYN/ACK = port is open
Port responds with RST = port is closed
No response = port is filtered by firewall
Nmap uses this technique


### UDP Amplification Attack

Attacker sends small UDP request
Spoofs victim's IP as source
Server sends large response to victim
Victim receives massive traffic
Used in DDoS attacks 