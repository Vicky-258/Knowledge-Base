#computerNetworks [[03-09-2025]]
### Evolution:

- ARPANET — 1960s
- TCP/IP — 1970s
- Email — 1980s
- WWW — 1990s
- Mobile networks (broadbands) — 2000s

### Network Edge:

- Represents all the devices connected to the internet.
- Includes the servers, computer, smart devices, IoTs
### Host

Hosts can be categorized into two types
- Clients — Laptop, workstation, mobiles
- Servers—Servers, Data centers containing servers
### Access Networks

→ Home Access Networks — DSL, Cable, fiber optics
→ Institutional Access Networks — fiber optics
→ Mobile Access Networks — cellular tech (4g, 5g)

### Network core and Packet switching

Network core — Mesh of routers that are interconnected.
Routers — specialized devices, to direct packages from one to others.

- These routers operates based on packet switching
- packets — given message is broken into pieces containing src, dest and msg.
- These messages then travel through the routers and reach dest when the requested thing was done.
- This pieces each may arrive at different times, but when all of them reached the dest thy will remerge into one and then will be given to the requester.

### Forwarding

- The way how data travels from one router to another.
- This uses a table to keep track known as forwarding table.
### Routing

- It's a term used for managing the messages to travel from src to dest.
### Internet Routing Algo

Finds optimized and shortest path among the global connections.

##### Routing destination factors:

- Network Topology
- Traffic conditions
- link capabilities
#### Routing protocol

- BGP — one of the most used.
- BGP — decides routing based on data.
- They are dynamic, meaning if a link fails, they travel through others to reach dest.
#### Protocols:

- Language and grammar of internet
- They decide the form of messages
Examples:
- TCP, UDP, IP, HTTP

- TCP — Ensures data transfer correctly through the devices A and B takes safety measures such us sending the unsend packages back to the sender to let them know.
- IP — responsible for connecting devices across the internet using address (IPv4, IPv6)
- HTTP — protocol for websites and maintaining them

#### TCP/IP stack:

This contains four layers on hierarchy

- Layer 1 — Application layer — HTTP, SMTP, FTP
    - This the layer connecting the user or showing the results to the users.
- Layer 2 — Transport Layer — TCP, UDP
    - This is the layer responsible for transporting packages across src and dest.
- Layer 3—Network Layer — IPv4, IPv6
    - This layer is responsible for addressing and navigating the messages to their correct location.
- Layer 4 — Link layer — Ethernet and Wi-Fi
These layers are independent of each other in terms of robustness they won't be affected by an error on another layer
### links: [[https://youtu.be/sMHzfigUxz4?si=yieyeiAsC68UlM4C|ByteByteGo's video]]


