# Network Devices and Security Implications

## Hub

Oldest and most basic network device.

| Feature | Detail |
|---------|--------|
| Layer | Layer 1 Physical |
| Intelligence | none |
| Behavior | broadcasts everything to all ports |
| Security | terrible — everyone sees all traffic |
| Status | obsolete, rarely used today |

Hub receives packet → sends to ALL connected devices
Every device sees every other device's traffic
Attacker connected to hub = sees all network traffic

## Switch

Replaced hubs in modern networks.

| Feature | Detail |
|---------|--------|
| Layer | Layer 2 Data Link |
| Intelligence | learns MAC addresses |
| Behavior | sends traffic only to correct destination |
| Security | much better than hub |

### MAC Address Table — How Switch Learns

First packet (unknown destination):
Switch checks MAC table → not found → BROADCASTS to all ports
Destination responds → switch learns its MAC address
MAC table updated

All future packets:
Switch checks MAC table → found → sends DIRECTLY to correct port
No broadcasting — other devices cannot see traffic


### MAC Flooding Attack

Attacker sends thousands of fake MAC addresses
Switch MAC table becomes completely full
Switch cannot learn new addresses
Switch reverts to hub behavior — broadcasts everything
Attacker now captures all network traffic

Defense: Port security — limit MAC addresses per port

## Router

Connects different networks together.

| Feature | Detail |
|---------|--------|
| Layer | Layer 3 Network |
| Intelligence | reads IP addresses, makes routing decisions |
| Behavior | forwards packets between different networks |
| Key function | strips old MAC headers, creates new ones |

### What Router Does at Each Hop

When a packet arrives at a router from Network A 
and needs to be forwarded to Network B:

| Step | Layer | Action |
|------|-------|--------|
| 1 | Physical (Layer 1) | receives electrical signals, converts bits to frame |
| 2 | Data Link (Layer 2) | reads MAC header, verifies destination MAC, strips it |
| 3 | Network (Layer 3) | reads IP header, checks routing table, makes forwarding decision |
| 4 | Data Link (Layer 2) | creates NEW frame with new source and destination MAC addresses |
| 5 | Physical (Layer 1) | converts frame to bits, sends out toward Network B |

**Key point:** The router goes UP the layers to read 
the IP address, then comes back DOWN to create a new 
frame with new MAC addresses before forwarding.

**What changes and what stays the same:**

| Field | Behavior | Reason |
|-------|---------|--------|
| Source IP | never changes | permanent end-to-end identity |
| Destination IP | never changes | permanent end-to-end identity |
| Source MAC | changes at every hop | only valid within current network |
| Destination MAC | changes at every hop | only valid within current network |

### Why Router is Necessary

Switch only understands MAC addresses (Layer 2)
Cannot read IP addresses
Cannot route between different networks
Two networks connected only by switches = isolated

Router understands IP addresses (Layer 3)
Can route between networks
Responds to ARP requests as default gateway
Makes inter-network communication possible

## Firewall

Controls what traffic enters and leaves network.

| Feature | Detail |
|---------|--------|
| Layer | Layer 3 and 4 (and Layer 7 for advanced) |
| Function | filters traffic based on rules |
| Types | stateless, stateful, next-generation |

### Firewall Rule Examples

Allow TCP port 443 inbound → HTTPS traffic allowed
Block TCP port 23 inbound → no Telnet access
Block all UDP except port 53 → only DNS allowed
Allow established connections → responses to outbound traffic

### Types of Firewalls

| Type | How it works |
|------|-------------|
| Stateless | checks each packet independently against rules |
| Stateful | tracks connection state, smarter decisions |
| Next-Generation (NGFW) | inspects application layer, detects malware |
| Web Application Firewall (WAF) | specifically protects web applications |

## IDS vs IPS

| | IDS | IPS |
|--|-----|-----|
| Full name | Intrusion Detection System | Intrusion Prevention System |
| Action | detects and alerts only | detects and blocks |
| Position | behind firewall | inline with traffic |
| Risk | never blocks legitimate traffic | might block legitimate traffic |
| Analogy | security camera | security guard |

### IDS Placement

Internet → Firewall → IDS → Internal Network

Firewall filters obvious bad traffic first
IDS analyzes remaining traffic deeply
Reduces false positives significantly

## Network Security Architecture

Internet
↓
Firewall (blocks known bad traffic)
↓
DMZ (public facing servers — web, email)
↓
IDS/IPS (monitors all internal traffic)
↓
Internal Network (employee devices)
↓
Critical Assets (databases, servers)
↓
Additional Firewall (extra protection layer)

## Cybersecurity Relevance

| Device | Attack | Defense |
|--------|--------|---------|
| Switch | MAC flooding | port security |
| Router | routing table poisoning | route authentication |
| Firewall | firewall bypass | proper rule configuration |
| IDS | evasion techniques | signature updates |

## Defense in Depth

No single device provides complete security.
Layer multiple defenses so if one fails another catches it:

Firewall → IDS/IPS → Antivirus → Encryption → Access Controls

Each layer assumes the previous one might fail.