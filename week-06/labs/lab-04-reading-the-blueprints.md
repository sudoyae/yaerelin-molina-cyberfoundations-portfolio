# Week 6 Lab 04 — Reading the Blueprints

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-04-reading-the-blueprints.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

**This is a SHORT lab — 15 to 20 minutes.** It is deliberately small. You already have the commands; this lab is about matching a drawing to reality.

The **Cloud Heights Network Blueprint** is displayed at the top of this lab page in the portal. Everything you write about the network's architecture comes from that blueprint or from your own machine — never from a guess.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Source of truth | The Cloud Heights Network Blueprint shown at the top of this lab page |
| Commands used | `ip addr`, `ip route` |
| Known value | Student subnet: **`10.60.6.0/26`** |

---

## Part A — Read the Drawing

### Step 1 — Record the Architecture Values

From the blueprint at the top of this page, record each value **exactly as drawn**. If a value is not shown on the blueprint, write "not shown on blueprint" — do not guess.

| Item | Value from the blueprint |
| --- | --- |
| VNet name | vnet-cf-labs |
| VNet address space | 10.60.6.0/24 |
| Student subnet range | 10.60.6.0/26 |

---

## Part B — Verify Against Your Own Machine

### Step 1 — Confirm Your Address Lives in the Subnet

Run `ip addr` and find your private IPv4 address.

Command and output:

```
Command: ip addr
Output: 1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNK
NOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq st
ate UP group default qlen 1000
    link/ether 7c:1e:52:40:d0:e5 brd ff:ff:ff:ff:ff:ff
    inet 10.60.6.28/26 metric 100 brd 10.60.6.63 scope global e
th0
       valid_lft forever preferred_lft forever
    inet6 fe80::7e1e:52ff:fe40:d0e5/64 scope link 
       valid_lft forever preferred_lft forever
3: enP35350s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500
 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 7c:1e:52:40:d0:e5 brd ff:ff:ff:ff:ff:ff
    altname enP35350p0s2
```

Your private IP:

```
10.60.6.28/26
```

Explain how you know your address falls inside `10.60.6.0/26` — what range does that prefix actually cover:

```
The subnet 10.60.6.0/26 covers the complete range from 10.60.6.0 through 10.60.6.63. The first address, 10.60.6.0, identifies the subnet itself, while the last address, 10.60.6.63, is the broadcast address. My private IPv4 address is 10.60.6.28, so it belongs to this subnet because 28 falls between 0 and 63.

The purpose of the /26 prefix is to determine where the subnet begins and ends. It helps my computer identify which IP addresses are part of its local network and which are outside it. If a destination is outside the subnet, my computer sends the traffic through the default gateway.
```

### Step 2 — Confirm Route Behaviour

Run `ip route`.

Command and output:

```
Command: ip route
output: default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.28 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.28 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.28 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.28 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.28 metric 100 
```

What the default route tells you about traffic that is not destined for your own subnet:

```
The default route tells me that traffic not destined for my local 10.60.6.0/26 subnet is sent to the default gateway at `10.60.6.1`. My computer sends this traffic through the eth0 interface so the gateway can forward it toward another network or the internet.
```

### Step 3 — Capture Your Evidence

**Required filename:** `blueprint-verified.png`

This must be **your own `ip addr` and `ip route` output** — not a re-screenshot of the blueprint. Crop out the address bar and any login information.

![Blueprint verified — my address inside the student subnet](https://raw.githubusercontent.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/blueprint-verified.png)

---

## Part C — How Traffic Actually Moves

### Step 1 — No Public IP

Your VM has a private address and **no public IP**. Explain what that means for who can reach it directly from the internet:

```
My VM has the private address 10.60.6.28, which can be used inside the Cloud Heights private network but cannot be reached directly from the public internet. Someone on the internet cannot simply enter that private address and connect to my VM because the VM does not have its own public IP address. It is similar to an employee working in a private office that does not have an outside entrance; visitors must enter through an approved main entrance.
```

### Step 2 — Outbound vs. Inbound

Outbound internet traffic from your VM leaves through address **translation (NAT)**. Inbound access for you arrives through **Azure Bastion**, not through a public address on the VM.

Explain both directions in your own words:

```
Outbound traffic starts from the VM and travels to another system. For example, when a VM uses curl to open a website, the request leaves the VM. NAT translates the VM’s private IP address into a public address so the request can travel across the internet.

Inbound traffic starts outside the VM and attempts to enter it. Because my VM has no public IP address, people cannot connect to it directly from the internet. The approved inbound connection goes through Azure Bastion, which acts as a protected entry point before connecting to the VM’s private address.
```

### Step 3 — The Guard Post You Do Not Touch Yet

Each student machine sits behind its own **network security group** — a per-student guard post that decides what traffic is allowed in.

**In Week 6 you do not configure it.** Week 7 is when you take control of those rules.

Write one sentence naming what the guard post does and one sentence stating what you are *not* doing with it this week:

```
The network security group acts as a guard post that uses rules involving direction, IP addresses, ports, and protocols to allow or block traffic. In Week 6, I am only observing its protection and am not adding, removing, or changing its rules.
```

---

## Analysis Questions

**Analysis Question 1.** Why would an organization put every student machine in one small subnet instead of giving each machine a public address? *(Minimum 3 sentences.)*

```
An organization would place the student machines in one small private subnet so they can communicate within a controlled network without exposing every VM directly to the internet. Giving each machine a public IP address would create a larger attack surface, meaning attackers would have more internet-facing machines, ports, and services that they could scan or attempt to access. A private subnet prevents exposure and allows access to be managed through network security groups, which allow or block traffic based on rules involving IP addresses, ports, protocols, and direction. Azure Bastion provides another security control by giving approved users a protected path to the private VMs without placing a public IP address on each machine.
```

**Analysis Question 2.** Segmentation means separating a network into parts that cannot freely reach each other. Give one concrete benefit of segmentation during a security incident. *(Minimum 3 sentences.)*

```
Segmentation can prevent an attacker from moving freely from one part of a network to another during a security incident. For example, many companies face phishing attacks in which an employee opens a malicious attachment and infects their workstation with ransomware. If the employee network is separated from systems containing financial records, customer information, or backups, segmentation can help prevent the ransomware from spreading to those critical systems. This limits the damage and gives cybersecurity professionals time to isolate the infected device, investigate the alert, and protect the rest of the organization.
```

**Analysis Question 3.** A diagram and a live machine disagree about an address range. Which do you trust, what do you do next, and why? *(Minimum 2 sentences.)*

```
he diagram represents how the network is supposed to be configured, while the live machine shows its actual configuration at that moment. If they disagree, I would use commands such as ip addr and ip route to document what the machine is currently using, but I would not assume that either source is automatically correct. I would compare the results with the official cloud configuration and notify the administrator because the diagram could be outdated or the machine could be misconfigured. For a cybersecurity professional, this difference matters because an unexpected change could be configuration drift, a human error, or evidence of unauthorized activity.
```

---

## Submission Checklist

- [x] VNet name, address space, and subnet range recorded from the blueprint (Part A)

- [x] `ip addr` run and own private IP confirmed inside `10.60.6.0/26` (Part B, Step 1)

- [x] `ip route` run and default route behaviour explained (Part B, Step 2)

- [x] `blueprint-verified.png` captured from your own terminal, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 3)

- [x] Private address / NAT / Bastion explained (Part C, Steps 1–2)

- [x] Per-student guard post identified — and explicitly not configured this week (Part C, Step 3)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-04-reading-the-blueprints.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 04: Reading the Blueprints** in the Lab Portal.
2. Fill in the worksheet fields and upload `blueprint-verified.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-04-reading-the-blueprints.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
