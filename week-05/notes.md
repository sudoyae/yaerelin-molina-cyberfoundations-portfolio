# Week 5 Notes — The Grid: Addresses, Names, Ports, and Diagnostics

**Student Name:** Yaerelin Molina

**Date Completed:**

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- IP addresses — the dotted-quad number every device on a network needs (`10.20.5.42` on The Grid)
- The subnet mask — the answer to "which addresses are my neighbours?" (`/24` = `255.255.255.0`)
- The default gateway — the door out of your neighbourhood (`10.20.5.1` on The Grid)
- Private vs public addresses — `10.x`, `172.16–31.x`, and `192.168.x` are *inside* addresses
- DNS — the Grid's Directory Board: a name goes in, an IP address comes out
- NXDOMAIN vs a host that resolves but is down — two different failures with two different causes
- DHCP — the Address Office: leases, why addresses change, why a laptop "just works" on a new network
- Ports — the numbered doors on a building: 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 3389 RDP, 25 SMTP
- TCP vs UDP — a confirmed conversation vs a shout across the room
- The TCP handshake — SYN → SYN-ACK → ACK (packets 7, 8 and 9 in Lab 03)
- The diagnostic toolkit — `ping` (is it alive?), `traceroute` (where does it stop?), `dig` (what number is behind that name?)
- **THE LADDER RULE** — check yourself → check your gateway → check the target by NAME → check the target by IP → trace the path. *Work outward, one rung at a time, and let the evidence pick the culprit.*

## My Command Table

You learned the same five jobs twice this week — once in bash, once in PowerShell. Fill the pairs in from memory if you can, and check them afterwards. This table is worth keeping.

The bash command and its PowerShell equivalent for each job — show my own address, show my default gateway, test reachability, trace the path, look up a name:

```
Bash:
1. ifconfig
2. route -n get default
3. ping hostname
4. traceroute hostname
5. nslookuphostname

PowerShell:
1. ipconfig
2. Test-Connection
3. tracert
4. Resolve-DnsName
```

## In My Own Words

Your machine has three numbers: an address, a subnet mask, and a default gateway. Explain what each one is for, the way you'd explain it to someone who has never heard those words.

```
I think of an IP address like my seat number at a concert because it identifies my specific location. The subnet mask is like the seating section because it helps determine which other seats are considered part of my local area, or local network. The default gateway is like the exit from my section because if I need to go somewhere outside my local area, I have to go through that exit. In networking, the default gateway is usually a router that forwards my traffic to other networks.
```

What does DNS actually do? Include the difference between a name that comes back "Name or service not known" (NXDOMAIN) and a name that resolves perfectly well to a host that never answers.

```
Domain Name System, works like the contacts list on my phone. I remember a person’s name instead of their phone number, and my phone connects that name to the correct number. Similarly, DNS translates a domain name into an IP address that a computer can use. If I get “Name or service not known” or NXDOMAIN, DNS could not find that name, similar to trying to call someone whose name or number does not exist in my contacts. I also think of how my phone might label an unknown caller as “Scam Likely,” because the number exists and the call can reach me, but I do not recognize or trust who is behind it. Similarly, if a domain name resolves perfectly to an IP address but the host never answers, DNS still worked correctly; the problem is with reaching or getting a response from the destination, not with finding its address.
```

An IP address gets your traffic to the right building. What does a port number add to that, and why would a defender care how many doors are open?

```
Domain Name System, works like the contacts list on my phone. I remember a person’s name instead of their phone number, and my phone connects that name to the correct number. Similarly, DNS translates a domain name into an IP address that a computer can use. If I get “Name or service not known” or NXDOMAIN, DNS could not find that name, similar to trying to call someone whose name or number does not exist in my contacts. I also think of how my phone might label an unknown caller as “Scam Likely,” because the number exists and the call can reach me, but I do not recognize or trust who is behind it. Similarly, if a domain name resolves perfectly to an IP address but the host never answers, DNS still worked correctly; the problem is with reaching or getting a response from the destination, not with finding its address.
```

Write out THE LADDER RULE — all five rungs, in order — and say why running them in that order matters more than running them fast.

```
1. Link
2. Address
3. Gateway
4. DNS
5. Destination

Running the ladder rule in order is important to prevent false diagnostics and waste of even more time for the business. For example, if there is no physical link, it's impossible to have an address. For example, trying to troubleshoot a DNS problem when your Ethernet cable is unplugged is similar to fixing the engine of a car with no wheels. 
```

What is DHCP, and why does your laptop get an address automatically on a network it has never joined before, while a server like `grid-dns` keeps the same address permanently?

```
Dynamic Host Configuration Protocol, is basically what helps my laptop get connected to a network automatically. I think of it like walking into a restaurant and being assigned a table. I do not have to choose or set up the table myself because the restaurant gives me an available one. In the same way, when my laptop joins a new network, DHCP can automatically give it an IP address and other information it needs to communicate, such as the subnet mask, default gateway, and server.

A server like grid-dns is different because other devices need to know where to find it. It is more like the restaurant's kitchen, which needs to stay in the same place so everyone knows where orders should go. For grid-dns, keeping a predictable address is important because other computers need to know where to send their DNS requests. If its address constantly changed, clients could have trouble finding the DNS server.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I completed the bash-to-PowerShell command table

- [x] I answered all five "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-05/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
