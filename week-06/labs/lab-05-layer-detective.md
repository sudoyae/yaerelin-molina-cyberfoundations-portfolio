# Week 6 Lab 05 — Layer Detective

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-05-layer-detective.md`

---

## Overview

**This is a SHORT lab — 20 to 30 minutes — and it needs no VM.** No Cloud Heights session, no simulator, no screenshot. This is a thinking lab: you take the evidence you have already collected in Weeks 5 and 6 and sort it into layers.

This is an **independent** lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | This worksheet only — nothing to start, nothing to connect to |
| Prerequisite | Week 5 labs and Week 6 Labs 01–04 |
| Screenshot | None required |

---

## Part A — The Seven-Row Table

Fill in every row. For the last column, name one **real thing you personally saw** in Weeks 5–6 that belongs at that layer.

| # | Layer name | One-line job | Real thing from Weeks 5–6 |
| --- | --- | --- | --- |
| 7 | Application | Provides network services directly to applications/users. | SSH when connected to the Ubuntu VM. |
| 6 | Presentation | Formats, encrypts, or translates data so applications can use it. | SSH encryption protecting the data during the SSH connection. |
| 5 | Session | Starts, maintains, and ends communication sessions between systems. | Your active SSH session with the Ubuntu VM. |
| 4 | Transport | Uses ports and TCP/UDP to deliver data between applications. | TCP port 22, used by SSH. |
| 3 | Network | Uses IP addresses and routing to move packets between networks. | VM's 10.60.6.28/26 IP address and default route through 10.60.6.1. |
| 2 | Data Link | Moves frames between devices on the same local network using interfaces/MAC addresses. | The VM's eth0 network interface with ip addr. |
| 1 | Physical | Carries raw bits through the physical or virtual networking medium. | The virtual network interface/NIC attached to your Azure VM. |

---

## Part B — Case Files

For each case, name the layer where the problem lives, and name the evidence proving the layers **below** it were already working.

### Case File 1 — The Name That Went Nowhere

A hostname lookup fails, but pinging the machine's IP address directly succeeds.

Layer:

```
Layer 7 - Application (DNS)
```

Evidence that the layers below were working:

```
1. Pinging the IP address directly succeeded, so the machine could send an ICMP packet to that address and receive a reply.

2. The failure appeared when using the hostname, so I would investigate DNS name resolution. The successful ping does not prove that every protocol works, but it shows that the IP path worked for that test.
```

### Case File 2 — Permission Denied

`ssh` to a host returns `Permission denied` after a password prompt.

Layer:

```
Layer 7 - Application (SSH authentication)
```

Evidence that the layers below were working:

```
1. I reached the SSH password prompt, which means the connection reached an SSH service and the service responded.
2. The “Permission denied” message came after I tried to authenticate. I would check the username, password or key, and account access rules before investigating the network path.
```

### Case File 3 — The Cable Story

A machine reports no link on its interface and has no address at all.

Layer:

```
Layer 1 - Physical
```

Evidence and reasoning:

```
1. The interface reports no link, so I would first check the cable, port, adapter, or virtual network connection.
2. The missing IP address could follow from the link being down, although it does not prove the cause on its own. I would verify the link before troubleshooting address assignment.
```

### Case File 4 — Ping Works, The Page Does Not

`ping` to a server succeeds, but `curl http://<that server>` returns nothing useful.

Layer:

```
Layer 7 - Application (HTTP)
```

Evidence that the layers below were working:

```
1. The successful ping shows that the server’s IP address was reachable using ICMP.
2. Ping does not test TCP port 80 or the web service. I would need the curl error to determine whether the problem is with HTTP, the TCP connection, or a rule blocking that connection.
```

### Case File 5 — Wrong Neighbourhood

A machine has an address, but its default route points somewhere that cannot forward its traffic.

Layer:

```
Layer 3 - Network 
```

Evidence and reasoning:

```
1. The machine has an IP address, so its interface has been configured with one.
2. Its default route points somewhere that cannot forward traffic. I would check ip route and test an off-subnet destination to investigate why traffic cannot leave the local subnet.
```

---

## Part C — The Silent Gateway Case

In Lab 03 the Azure default gateway did not answer your ping. However, your VM had a valid default route configured, and your local communication with the Grid Beacon — the ping replies, the HTTP banner, and `TRACE ID: CF-NET-0604` — succeeded.

A failed gateway ping is one piece of evidence — not automatically proof of a gateway or network failure. But the evidence you weigh against it has to be the right kind of evidence.

The Grid Beacon at `10.60.6.4` sits on the same local subnet as your VM (`10.60.6.0/26`). Reaching it proves **local-subnet connectivity** — that traffic never crosses the default gateway, so beacon success alone cannot prove the gateway forwarded anything. Your `ip route` output proves a **default route is configured** — your VM knows where it intends to send non-local traffic — but it does not prove the gateway forwarded that traffic. The evidence that demonstrates the **default path is functioning** is successful communication with a destination outside `10.60.6.0/26`, such as the outbound internet access through NAT that you examined in Lab 04.

### Step 1 — Rule on the Case

Is the failed gateway ping enough evidence to declare a network-layer failure? Explain your answer using the other evidence you collected. In your response, distinguish between:

- evidence that proves **local-subnet connectivity**
- evidence that proves a **default route is configured**
- evidence that supports **successful off-subnet connectivity**

```
1. The Grid Beacon at 10.60.6.4 responded to ping and returned its HTTP banner and TRACE ID: CF-NET-0604. This proves local-subnet connectivity, because the Beacon and VM are both on 10.60.6.0/26; that traffic does not cross the default gateway.
2. ip route showed a default route. This proves the VM is configured to send off-subnet traffic toward a gateway, but does not prove the gateway forwarded anything.
3. Successful communication with a destination outside 10.60.6.0/26, such as the outbound access through NAT examined in Lab 04, supports that the default path worked. The failed gateway ping only shows that the gateway did not answer that ICMP probe.
```

### Step 2 — Name the Correct Conclusion

For each of these four results, state what it actually proves: the Grid Beacon at `10.60.6.4` answering, the default route shown by `ip route`, a successful connection to a destination outside your local subnet, and the gateway's failed ping. Then state the rule you would give a junior colleague about the difference between an observation ("the gateway did not answer my ICMP probe") and a diagnosis ("the gateway is broken"):

```
1. Grid Beacon answers: My VM can communicate with that host on the local subnet.
2. Default route appears in ip route: My VM has a configured route for non-local traffic.
3. Off-subnet connection succeeds: Traffic can reach a destination beyond the local subnet, supporting that the default path is functioning.
4. Gateway ping fails: The gateway did not reply to my ICMP probe; this result alone does not show that forwarding failed.
```

---

## Part D — Two Models, One Job

The OSI model has seven layers. The practical TCP/IP model most engineers speak day to day has four or five.

### Step 1 — Map Them

Briefly show how the seven OSI layers collapse into the practical model:

```
TCP/IP Application includes OSI Layers 7, 6, and 5: Application, Presentation, and Session.
TCP/IP Transport corresponds to OSI Layer 4: Transport.
TCP/IP Internet corresponds to OSI Layer 3: Network.
TCP/IP Link/Network Access includes OSI Layers 2 and 1: Data Link and Physical.
```

### Step 2 — When Each Is Useful

Explain when the seven-layer vocabulary helps and when the practical model is the better tool:

```
1. The seven-layer OSI model helps me describe more precisely where a test succeeded and where a problem might begin.
2. For example, a successful ping supports IP connectivity but does not prove an HTTP service works.
3. The practical TCP/IP model is shorter and useful for everyday discussion of applications, transport, IP, and the local link.
```

---

## Analysis Questions

**Analysis Question 1.** Explain the Ladder Rule using layer language. What does "test the near thing first" mean when the rungs are layers? *(Minimum 3 sentences.)*

```
The Ladder Rule is a way to troubleshoot a connection one step at a time. Imagine that a website will not load: the cause could be your computer’s connection, its IP settings, the route to the server, the website’s name, or the website itself. “Test the near thing first” means starting with what is closest to your computer, such as checking whether its network interface has a link and an IP address, before testing a server somewhere else. Then I would try a device on my local subnet, a destination outside that subnet, the website’s hostname, and finally the website with curl. Each successful test shows that part of the path worked, so I can focus on the first step that fails instead of guessing where the problem is.
```

**Analysis Question 2.** Why is "which layer is this?" a faster question than "what is broken?" when you are under pressure? *(Minimum 3 sentences.)*

```
“What is broken?” could mean many different things, which makes it hard to choose a first test under pressure. Asking “which layer is this?” narrows the possibilities to areas such as the physical link, IP routing, transport, or the application. It helps me choose a command that tests one possibility instead of guessing from a symptom.
```

**Analysis Question 3.** Pick one case file from Part B and describe the very next command you would run to confirm your ruling, and what result would change your mind. *(Minimum 2 sentences.)*

```
### Analysis Question 2 — Why ask “which layer is this?”

If something will not connect, “what is broken?” is a hard question because many different things could be wrong. Asking “which layer is this?” helps me break the problem into smaller checks: does my computer have a connection, can it reach the other machine, and is the service I need responding? Under pressure, I can test those questions one at a time and use the results to decide what to check next.
```

---

## Submission Checklist

- [x] All seven rows of the OSI table completed with a real Week 5–6 anchor each (Part A)

- [x] All five case files given a layer and supporting evidence (Part B)

- [x] Silent gateway case ruled on correctly (Part C)

- [x] OSI vs. practical TCP/IP model compared (Part D)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] No screenshot required for this lab

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-05-layer-detective.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 05: Layer Detective** in the Lab Portal.
2. Fill in the worksheet fields.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-05-layer-detective.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
