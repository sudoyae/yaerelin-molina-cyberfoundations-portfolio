# Week 6 Notes — Cloud Heights: Cloud VMs, SSH, VNets & Layers

**Student Name:** Yaerelin Molina

**Date Completed:**

Summarize this week's key concepts in your own words — not copy-pasted definitions. This week moved from the simulated Grid into a real cloud environment, so focus on what you personally observed as well as what each term means.

> **Cloud Heights Security Rule:** Your Bastion shareable link and Cloud Heights password are private access credentials. Never paste either into this file, a screenshot, your GitHub repository, Circle, or a chat message.

## Key Concepts This Week

- **Cloud** — other people's computers, professionally operated and reached over a network
- **Datacenter** — the physical facility where cloud computing equipment lives
- **Region** — a geographic area where a cloud provider operates datacenters
- **Virtual machine (VM)** — a computer created in software; in Cloud Heights, your VM runs on hardware in a real datacenter
- **IaaS / PaaS / SaaS** — different levels of cloud service: rent the room, rent the workshop, or rent the finished service
- **Shared responsibility model** — the cloud provider secures the building and underlying platform; the customer is still responsible for what belongs to them
- **Provisioning** — creating and preparing a resource so it is ready to use
- **Golden image / snapshot** — a known starting point that can be used to create consistent machines
- **Snapshot vs backup** — a snapshot is a point-in-time copy used for recovery or cloning; a backup is a separate recovery copy with a different purpose
- **Azure Bastion** — the guarded front desk that gives you browser-based SSH access without giving your VM a public IP
- **Bastion shareable link** — sensitive access information that must never be committed to GitHub or exposed in screenshots
- **SSH (Secure Shell)** — remote command-line access to another machine
- **SSH client and server** — the client starts the connection; the server listens and answers
- **Port 22** — the standard numbered door used by SSH
- **Host / fingerprint verification** — the verify-before-approve habit when connecting to a host for the first time
- **Authentication** — proving that you are the account you claim to be
- **Remote session / remote shell** — the live command-line session running on another machine
- **Getting TO vs getting INTO a machine** — network reachability and authentication are different problems
- **`hostname`** — asks which machine you are on
- **`whoami`** — asks which account you are using
- **`pwd`** — asks where you are in the filesystem
- **Private IP address** — an address used inside a private network rather than directly on the public internet
- **Virtual network (VNet)** — the private cloud neighborhood where resources communicate
- **Subnet** — a smaller address range inside a VNet; a floor inside the larger building
- **NAT / outbound translation** — lets a privately addressed machine communicate outward without giving the machine its own public IP
- **Network Security Group (NSG)** — the network guard post that controls what traffic is allowed; you take control of these rules in Week 7
- **Known-good reference point** — a target whose expected behavior gives you something reliable to compare against
- **Grid Beacon** — the known-good Cloud Heights host at `10.60.6.4`
- **The silent Azure gateway** — Azure's default gateway may not answer ICMP ping even when the network is healthy
- **OSI model** — the seven-layer vocabulary used to organize network and application behavior
- **TCP/IP model** — the more compact layer model commonly used by practitioners
- **Layers** — a way to separate different jobs in a communication path so troubleshooting can be systematic
- **Encapsulation** — information travelling inside other information, like a letter inside an envelope inside a mailbag
- **The Ladder Rule in the real cloud** — work outward, prove what works, use the route and a known-good target, and never let one silent tool response choose the culprit by itself

## My Cloud Heights Command Table

You used these commands on a real Ubuntu machine this week. Instead of memorizing syntax, write down the **question each command answers** or the job it performs.

| Command | What question does it answer / what does it do? |
| --- | --- |
| `hostname` | "What machine am I using currently?" |
| `whoami` | "Which account am I using on this machine?" |
| `pwd` | "Where am I in this machine's filesystem?" |
| `ip addr` | "What network interfaces and IP addresses does this machine obtain?" |
| `ip route` | "Where will this machine send local and non-local traffic?" |
| `ping` | “Does this machine respond when I send it a test message using ICMP, a network protocol for diagnostic messages?” |
| `traceroute` | "Which hops, network devices along the way, can I see as my traffic travels toward another machine?" |
| `dig` | "What IP address does DNS, the system that looks up website names, return for this name?" |
| `curl` | "What does a website or web service send back when I request a page or other resource?" |
| `ssh` | "Can I open a secure remote command-line session so I can type commands on another machine?" |
| `exit` | "How do I close the command-line session I’m currently using?" |

## In My Own Words

### 1. Getting TO vs Getting INTO

Explain the difference between getting **TO** a machine and getting **INTO** a machine. Use something you personally observed in Cloud Heights as evidence.

```
Getting TO a machine means your connection can reach it. Getting INTO it means you have also proved you are allowed to sign in. I think of it like reaching the right hotel room: finding the door is one step, but opening it requires the right key. In Cloud Heights, Azure Bastion gave me a way to reach my VM, and the SSH sign-in let me enter its command line. After signing in, I used hostname and whoami to check which machine I had entered and which account I was using.
```

### 2. The Silent Gateway

Your Azure gateway did not answer `ping`, but your VM was still healthy. Explain how you proved the network was working and what this taught you about interpreting tool output.

```
My Azure gateway did not reply when I sent it a ping, which is a network test message. At first, that could look like the gateway was down, but a missing reply only proves that this test received no answer. My VM successfully reached the Grid Beacon at 10.60.6.4 and received its ping replies, HTTP banner, and TRACE ID: CF-NET-0604. Because the Beacon was on the same subnet, those results proved my local connection worked; they did not prove that traffic crossed the gateway. The ip route command showed that a default route was set, and successful communication with a destination outside the subnet provided the evidence that the VM could send traffic outward. This taught me to compare several test results before deciding what caused a problem.
```

### 3. Private on the Inside, Connected to the Outside

Explain how your Cloud Heights VM can reach the internet even though it has only a private IP address. Then explain how **you** reach the VM from outside its VNet.

```
My VM had a private IP address, 10.60.6.28, which works inside its cloud network but is not its own public internet address. To reach the internet, its outbound traffic goes through NAT, a process that translates private addresses so machines can communicate outward. I picture a hotel guest sending mail through the front desk: the guest can send something out without giving everyone a direct way into their room. To reach my VM from outside its VNet, or private cloud network, I used Azure Bastion to open a browser-based SSH connection. Bastion provided a guarded way in without assigning my VM its own public IP address.
```

### 4. VNet vs Subnet

Explain the difference between a VNet and a subnet using the Cloud Heights building/floor analogy. Then explain why separating systems into smaller network ranges can help security.

```
A VNet, or virtual network, is like the whole Cloud Heights building: it is the larger private space where cloud machines can communicate. A subnet is like one floor of that building, with a smaller range of addresses for the machines on that floor. My VM and the Grid Beacon were both on the 10.60.6.0/26 subnet, which is why communicating with the Beacon did not require traffic to leave that local subnet. Separating systems into subnets can make it easier to control which groups may communicate. For example, an organization could put sensitive systems in one subnet and apply network rules that limit access from other subnets.
```

### 5. The Ladder Rule Has a Map Now

The Ladder Rule never used the words OSI or TCP/IP. Explain how the layer models give you a map for the same troubleshooting process you have already been using.

```
The Ladder Rule means checking one part of a connection at a time, beginning close to my machine and working outward. The OSI and TCP/IP models give names to the different jobs involved, such as the local connection, IP addressing and routing, and the application I am trying to use. If a website does not load, I can check whether my interface is connected, whether I have an IP address, whether I can reach another machine, whether its name resolves, and whether the web service responds. Each check tells me what worked at that step. The layer models help me explain where to test next without assuming that one failed command reveals the whole cause.
```

---

## Submission Checklist

- [x] I summarized the Week 6 concepts in my own words, not copied definitions

- [x] I completed my Cloud Heights command table

- [x] I explained getting TO vs getting INTO a machine

- [x] I documented what the silent Azure gateway taught me

- [x] I explained the Cloud Heights private-network design

- [x] I connected the Ladder Rule to network layers

- [x] I checked that my Bastion shareable URL does not appear anywhere in this file

- [x] I checked that my Cloud Heights password does not appear anywhere in this file

- [x] This file is committed to my portfolio repo at `week-06/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
