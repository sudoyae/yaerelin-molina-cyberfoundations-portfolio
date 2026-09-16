# Week 5 Lab 03 — Reading the Grid's Mail (Packet Inspector)

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 5  
**Submission Path:** `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## Overview

Lesson 4 showed you the three panes of a packet analyzer — the packet list, the detail pane, and the filter bar — and told you that professionals live in this tool. This lab is where you stop watching and start driving. You'll open a recorded minute of traffic on The Grid and read it packet by packet: a name lookup (Part B), a handshake and the message that followed it (Part C), and one conversation that refuses to be read at all.

**The capture is the same for every student.** It's a recording, not a live network — fifteen packets, captured once, replayed identically for everyone. That means your numbers should match your classmates' exactly. If yours don't, you've filtered something out; clear the filter and look again.

**Nothing here can break anything.** You are reading a recording. There is no traffic to disturb and nothing to configure.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations **Packet Inspector** — browser-based, inside the Lab Portal. Nothing to install. You will never install Wireshark for this course |
| Where to find it | Lab Portal main navigation, alongside the **CLI Simulator** |
| Capture | "The Grid — Workstation Capture." Canned and identical for every student |
| Prerequisite | Week 5, Lesson 3 (ports, the handshake) and Lesson 4 (the three panes) completed. Lab 01 recommended first — Part B builds directly on it |
| Filters available | `dns` · `icmp` · `tcp` · `http` · `ip.addr == x.x.x.x` · `tcp.port == N` |
| Time | Plan for 30–40 minutes, including this worksheet |

**Before you start:** log into the Lab Portal, open **Week 5 → Packet Inspector**, and load the capture named **"The Grid — Workstation Capture."** You should see a list of packets across the top, an empty detail pane below it, and a filter bar above everything. Keep this worksheet open in a second browser tab so you can record answers as you go.

**Get your screenshot tool ready now.** **Windows:** `Win + Shift + S` · **Mac:** `Cmd + Shift + 4`, then drag to capture. You'll need it twice in this lab, and both screenshots are required.

**A note on filters:** a filter never deletes packets. It hides the ones that don't match so you can concentrate. Clearing the filter box brings everything back. This matters in Part A — count *before* you filter anything.

---

## Part A — Orientation

### Step 1 — Count the Whole Capture

With the filter bar **empty**, look at the packet list and count how many packets the capture contains. Every packet has a number in the first column, so the last row tells you the answer without counting by hand.

The total number of packets in the capture:

```
The total number of packets in the capture is 15. 
```

### Step 2 — Inventory the Protocols

Scan the **Protocol** column from top to bottom and write down every distinct protocol name you see. There are five. You met all five in Lessons 2, 3 and 4 — this is the first time you're seeing them as labelled traffic rather than as ideas.

The protocols that appear in this capture:

```
The protocols that appear in this capture is:
1. DNS
2. ICMP
3. TCP
4. TLS
5. HTTP
```

### Step 3 — Read the Columns

Four columns do most of the work in a packet analyzer. In your own words — not the Lesson 4 wording — write one plain sentence for each: what does it tell you, and what question does it answer?

What the Source column tells you:

```
The Source colomn demonstrates the IP address of the device that sent the packet. 
```

What the Destination column tells you:

```
The Destination column demonstrates the IP address if the device 
```

What the Protocol column tells you:

```
The Protocol column demonstrates the network protocol used by the packet, such as DNS, ICMP, TCP, TLS, or HTTP.
```

What the Info column tells you:

```
The Info column demonstrates a summary of what the packet is doing or what information it contains. For example, the Info column shows GET /shift-notice.html HTTP/1.1, which indicates that the device is requesting a web page. 
```

### Step 4 — Spot Your Own Machine

One address appears more often than any other, and it's the one sending most of the requests. That's the workstation the capture was taken from — the same address you recorded in Lab 01.

The workstation's IP address, and how you worked out it was the workstation:

```
The workstation's IP address is 10.20.5.42 because the Ethernet 2 indicated IPv4 was associated with ivy-workstation
```

---

## Part B — Follow a DNS Lookup

In Lab 01 you ran `dig foundry-archive.grid.local` and read a tidy answer: a name went in, an IP address came out. You saw the *result*. Now you're going to see the actual conversation that produced it — the question travelling across the wire and the answer coming back.

### Step 1 — Filter for DNS

Type `dns` into the filter bar and apply it. The packet list shrinks to the name-lookup traffic only.

The packet numbers that remain after the `dns` filter:

```
The filter demonstrates two packets:
1. 10.20.5.42 to 10.20.5.10
2. 10.20.5.10 to 10.20.5.42
```

### Step 2 — Read the Question

Click the first of your two DNS packets and read its Info column and detail pane. This is the query — your workstation asking the Directory Board a question.

The packet number of the query, and the name being asked about:

```
Packet #1 query A: foundry-archive.grid.local
The DNS query is asking for the IPv4 address associated with foundry-archive.grid.local
```

The source and destination addresses of the query — who asked, and who was asked:

```
Source address: 10.20.5.42
Destination address: 10.20.5.10

The workstation (10.20.5.42) asked the DNS server (10.20.5.10) for the IP address of foundry-archive.grid.local
```

### Step 3 — Read the Answer

Now click the second DNS packet. This is the response coming back the other way.

The packet number of the response, and the IP address it returned:

```
Packet #2 
IP address returned: 10.20.5.20
```

### Step 4 — Find the Door

DNS doesn't just travel to an address — it travels to a specific **port** on that address, the way Lesson 3 described a numbered door on a building. Open the detail pane on either DNS packet and find the port number the lookup used.

The port number DNS used here:

```
Under the User Datagram Protocol (UDP), the source port as port 53.
```

### Step 5 — Tie It Back to Lab 01

This is the same lookup you ran with `dig` in Lab 01 — same name, same answer, same DNS server. The difference is the altitude you're viewing it from. `dig` handed you the conclusion; the Packet Inspector shows you the two messages that produced it.

In two or three sentences: what did the packet view show you that `dig` did not?

```
The packet view showed me how the DNS request traveled from my workstation to the DNS server. Unlike dig, I could see the source and destination IP addresses and the port used for the DNS request. 
```

### Step 6 — Capture Screenshot 1 (REQUIRED)

With the `dns` filter still applied and one of the two DNS packets selected, take a screenshot showing the filter bar, the filtered packet list, and the detail pane. Name it exactly **`packet-dns-query.png`**. Upload instructions are in the GitHub Commit section.

---

## Part C — Doors and the Handshake

Lesson 3 told the TCP handshake as a knock at a door: **SYN** ("knock"), **SYN-ACK** ("who's there — come in"), **ACK** ("thanks"). You're about to watch one happen.

### Step 1 — Filter for Port 443

Clear the `dns` filter and enter `tcp.port == 443` instead. Port 443 is HTTPS — one of the well-known doors from Lesson 3.

The packet numbers that remain after the `tcp.port == 443` filter:

```
Packet #7
Packet #8
Packet #9
Packet #10
```

### Step 2 — Identify the Three-Step Handshake

Three of those packets are the handshake itself. Find each one by reading the flags in the Info column, and record its number, its direction (which address sent it to which), and which step of the handshake it is.

The SYN — packet number and direction:

```
Packet 7: from 10.20.5.42 to 10.20.5.20
```

The SYN-ACK — packet number and direction:

```
Packet 8: from 10.20.5.20 to 10.20.5.42
```

The ACK — packet number and direction:

```
Packet 9: from 10.20.5.42 to 10.20.5.20
```

### Step 3 — Notice the Two Port Numbers

Look at the Info column on the SYN packet. Two port numbers appear: a high, odd-looking one on your workstation's side, and 443 on the server's side. Only one of them is a "well-known door" — the other is a temporary one your machine picked for this conversation.

The two port numbers, and which belongs to the server:

```
The two port numbers are 51542 and 443 [SYN]. Port 442 is the server's well-known door for HTTPS traffic. 
```

### Step 4 — Switch to the Plain Conversation

Clear the filter and enter `http` instead. This is a different conversation to a different machine on The Grid — the notice board — on port 80.

The packet numbers that remain after the `http` filter:

```
Packet 14 and Packet 15
```

### Step 5 — Open the Request and Read It

Click **packet 14** and expand its detail pane all the way. Unlike everything you've read so far, this packet's contents are ordinary text — you can simply read them, the way you'd read a note left on a desk.

The request line at the top of packet 14 (the method, the page, and the version):

```
GET /shift-notice.html HTTP/1.1

Method: GET
Page: /shift-notice.html
Version: HTTP/1.1

This indicates that using HTTP version 1.1, it send a copy of the webpage's data to browser. 

```

The Host line — which machine the request was addressed to:

```
Host: grid-notice.grid.local 

The request was addressed to the machine named grid-notice.grid.local. This hostname identifies the server receiving the request.  
```

Every other readable line in packet 14's detail pane:

```
Host: grid-notice.grid.local indicates the browser or software that sent the request. 
User-Agent: GridBrowser/2.4 indicates the browser or software that sent the request.                     
X-Staff-Code: FOUNDRY-2026-STOREROOM indicates a custom staff identification or access code included in the request. 
```

### Step 6 — Notice What You Just Read

One of those lines is not like the others. It carries a value that looks like an internal code rather than a technical setting.

The name of that header and the exact value it carries:

```
X-Staff-Code: FOUNDRY-2026-STOREROOM
Header name: X-Staff-Code
Exact value: FOUNDRY-2026-STOREROOM
```

In one or two sentences: if you were sitting on this network with a packet analyzer open, what would you now know that you were never meant to know?

```
If I were monitoring this network, I would be able to see a private employee code, FOUNDRY-2026-STOREROOM, and identify the webpage where someone used it. Because the website used HTTP instead of HTTPS, the information was sent without encryption. 
```

### Step 7 — Capture Screenshot 2 (REQUIRED)

With the `http` filter applied and **packet 14** selected and expanded so the readable lines are visible, take a screenshot. Name it exactly **`packet-http-plaintext.png`**.

### Step 8 — Now Try to Read the Other One

Clear the filter and click **packet 10** — the TLS Client Hello from the port 443 conversation you examined in Steps 1–3. Expand its detail pane and try to read it the same way you just read packet 14.

What packet 10's detail pane shows you, described in your own words:

```
Packet 10's detail pane demonstrates that ivy-workstation sent 505 bytes of data from IP address 10.20.5.42 and temporary port 51514 to the foundry-archive server at 10.20.5.20 through HTTPS port 443. Although I can see the devices and ports used, the TLS encrypts the message itself. 
```

Compare it to packet 14 — what can you still tell about packet 10 from the packet list, even though you can't read its contents?

```
Packet 14 uses regular HTTP through port 80, so I can read the entire request, including the requested page, host, and browser. Packet 10 uses HTTPS through port 443, so I can still determine connection information, but I cannot read the protected message inside. 
```

**Don't chase this yet.** You've just found something real, and the explanation is a later part of this course. For now, sit with the observation: two conversations, one legible and one not. Analysis Question 4 asks what you make of it.

---

## Analysis Questions

**Analysis Question 1.** In Part A you counted the packets before applying any filter. Explain why a filter is a *view* rather than a deletion, and describe a situation where forgetting that could lead an analyst to a wrong conclusion. *(Minimum 3 sentences.)*

```
A filter is a view, meaning it temporarily displays only the packets that match what I searched for, but it does not delete the other packets from the capture. When I clear the filter, all the hidden packets appear again. If an analyst forgets that a filter is still active, they might wrongly report that certain network activity ever happened.
```

**Analysis Question 2.** You have now seen the same DNS lookup twice — once as `dig` output in Lab 01, once as two packets here. Describe what each view is good for, and name one investigation where you would specifically want the packet view. *(Minimum 3 sentences.)*

```
The dig output gives a simple summary of a DNS lookup, which is the process of translating a website name into an IP address that a computer can use. 

The packet view shows the lookup as it actually moved across the network, including the request, response, packet numbers, IP addresses, and ports. 

I would use the packet view when investigating whether a computer tried to reach a suspicious website because it could demonstration which computer made the request, which DNS server received it, and what address was returned. 

I would also use the packet view to troubleshoot a connection problem. It could help me determine whether my computer sent the request, whether the correct server received it, and whether a server responded or the communication failed somewhere along the way. 
```

**Analysis Question 3.** The handshake took three packets and about three milliseconds before a single byte of real content moved. Using the knock-at-the-door analogy from Lesson 3, explain what those three packets accomplish and why a connection would be less reliable without them. *(Minimum 3 sentences.)*

```
Using the knock-at-the-door analogy, the three packets are like knocking on a door, getting an answer, and confirming that both people are ready to communicate. The SYN is the first knock asking to connect, the SYN-ACK is the server answering the knock, and the ACK confirms that the workstation received the response. Without these three steps, a device might start sending information without knowing whether the other device is actually receiving it. There needs to be a confirmation so that the devices can continue to communicate with each other. 
```

**Analysis Question 4.** You read packet 14 word for word, and packet 10 not at all — same capture, same network, same workstation. What do you think explains the difference between them? And why does it matter that the one you *could* read contained a staff code? You are not expected to know the mechanism yet; give us your best reasoning from what you observed. *(Minimum 4 sentences.)*

```
Packet 14 could be read word for word because its information appears to have been sent in plain text, while packet 10 demonstrated TLS, meaning the actual application data was encrypted. This suggests that one connection protected its information while the other allowed me to see what was being transmitted. This matters because the staff code was visible in te packet. If I could read it just by looking at the network traffic, someone else capturing that traffic could potentially read it too, which creates a security risk. 
```

---

## Submission Checklist

- [x] Total packet count recorded with the filter bar empty (Part A, Step 1)

- [x] All five protocols listed (Part A, Step 2)

- [x] Source, Destination, Protocol and Info columns each explained in your own words (Part A, Step 3)

- [x] Workstation address identified with reasoning (Part A, Step 4)

- [x] `dns` filter applied; query and response packets identified by number (Part B, Steps 1–3)

- [x] Hostname queried, IP address returned, and port number recorded (Part B, Steps 2–4)

- [x] Lab 01 comparison written (Part B, Step 5)

- [x] **REQUIRED:** `packet-dns-query.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part B, Step 6)

- [x] `tcp.port == 443` filter applied; SYN, SYN-ACK and ACK identified by number *and* direction (Part C, Steps 1–2)

- [x] `http` filter applied; the remaining packet numbers recorded (Part C, Step 4)

- [x] Both port numbers recorded, server's port identified (Part C, Step 3)

- [x] Packet 14's readable lines recorded, including the staff-code header and its exact value (Part C, Steps 5–6)

- [x] **REQUIRED:** `packet-http-plaintext.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part C, Step 7)

- [x] Packet 10 opened and its contrast with packet 14 described (Part C, Step 8)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## GitHub Commit Subsection

This lab's written answers are submitted through the **CyberFoundations Lab Portal**, the same way as Week 4.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 5 → Lab 03: Reading the Grid's Mail**.
3. Fill in the worksheet fields — they match the steps and questions in this file.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-05/labs/lab-03-reading-the-grids-mail.md` for you.

**📸 REQUIRED — both screenshots.** This lab has two, and both are graded:

| Screenshot | From | Filename |
|---|---|---|
| Filtered DNS view with a query packet selected | Part B, Step 6 | `packet-dns-query.png` |
| Packet 14 expanded, readable lines visible | Part C, Step 7 | `packet-http-plaintext.png` |

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-05/` (create the folder if this is your first Week 5 screenshot).
2. Click **Add file → Upload files**, drag both images in, named exactly as above (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click each uploaded image's filename to open it and confirm the packet detail is readable at full size.
5. Record both filenames below so your grader knows to look for them.

The filename of your DNS screenshot:

```
https://github.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/blob/main/assets/screenshots/week-05/packet-dns-query%202.png?raw=true
```

The filename of your packet 14 screenshot:

```
https://github.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/blob/main/assets/screenshots/week-05/packet-http-plaintext.png?raw=true
```

Both screenshots live in `assets/screenshots/week-05/` in your repository. They do not need to be linked inside this worksheet.

**Commit message tip:** name the work, not the file type — *"Add Week 5 Lab 03 packet analysis evidence"* reads far better to an employer browsing your repo than "update."

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
