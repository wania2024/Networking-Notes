# OSI Model and TCP/IP Model

## Why Two Models Exist

| Model | Layers | Purpose |
|-------|--------|---------|
| OSI Model | 7 layers | theoretical reference model |
| TCP/IP Model | 4 layers | practical real world implementation |

Both describe the same communication process — just different 
numbering systems. This causes confusion when courses switch 
between them.

## OSI Model — 7 Layers

| Layer Number | Layer Name | Data Unit | Function |
|-------------|-----------|-----------|----------|
| Layer 7 | Application | Data | user-facing protocols HTTP, DNS, SMTP |
| Layer 6 | Presentation | Data | encryption, compression, formatting |
| Layer 5 | Session | Data | managing sessions between devices |
| Layer 4 | Transport | Segment | TCP/UDP, port numbers, reliability |
| Layer 3 | Network | Packet | IP addressing, routing between networks |
| Layer 2 | Data Link | Frame | MAC addressing, switching within network |
| Layer 1 | Physical | Bits | electrical signals, cables, wireless |

## TCP/IP Model — 4 Layers

| TCP/IP Layer | Equivalent OSI Layers | Examples |
|-------------|----------------------|---------|
| Application | 7, 6, 5 | HTTP, HTTPS, DNS, SMTP, SSH |
| Transport | 4 | TCP, UDP |
| Internet | 3 | IP, ICMP, ARP |
| Network Access | 2, 1 | Ethernet, WiFi, MAC addresses |


## Encapsulation — How Data Travels Down Layers

Each layer adds its own header to the data before passing 
it to the layer below:

Application Layer: [DATA]
Transport Layer: [TCP HEADER | DATA] = Segment
Network Layer: [IP HEADER | TCP HEADER | DATA] = Packet
Data Link Layer: [MAC HEADER | IP HEADER | TCP HEADER | DATA | MAC TRAILER] = Frame
Physical Layer: converts everything to BITS and transmits


## Decapsulation — How Data Travels Up Layers

On the receiving side each layer strips its own header:

| Layer | What it strips | What remains |
|-------|---------------|-------------|
| Physical | converts bits to frame | complete frame |
| Data Link | strips MAC header and trailer | packet |
| Network | strips IP header | segment |
| Transport | strips TCP header | data |
| Application | reads pure data | message |

## Key Rule
Each layer only reads and removes its OWN header.
Layer 2 never touches IP header.
Layer 3 never touches MAC header.
Each layer minds its own business.

## What Changes at Each Hop

When data travels through a router:

| Field | Behavior |
|-------|---------|
| Source IP | never changes — same end to end |
| Destination IP | never changes — same end to end |
| Source MAC | changes at every router hop |
| Destination MAC | changes at every router hop |

IP addresses = permanent end to end identity
MAC addresses = temporary hop to hop identity

## Cybersecurity Relevance

| Layer | Common Attacks |
|-------|---------------|
| Layer 7 Application | SQL injection, XSS, phishing |
| Layer 4 Transport | SYN flood, port scanning |
| Layer 3 Network | IP spoofing, routing attacks |
| Layer 2 Data Link | MAC flooding, ARP spoofing |
| Layer 1 Physical | cable tapping, hardware tampering |

