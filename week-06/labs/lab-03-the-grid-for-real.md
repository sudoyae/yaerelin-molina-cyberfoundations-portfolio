# Week 6 Lab 03 — The Grid, For Real

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-03-the-grid-for-real.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

In Week 5 you ran `ip addr`, `ip route`, `ping`, and `traceroute` in a simulator that always behaved. Today you run the same toolkit against real cloud infrastructure that does **not** always behave the way the textbook implies — and you learn to tell "broken" apart from "normal."

This is an **independent** lab. It tells you what to accomplish; you choose the commands. Expect about 40 minutes.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Commands used | `ip addr`, `ip route`, `ping`, `traceroute`, `curl` |
| Known-good target | **Grid Beacon — `10.60.6.4`** |
| Prerequisite | Week 6 Labs 01–02 |

---

## Part A — Where You Actually Are

### Step 1 — Read Your Own Address

Run the command that lists your interfaces and addresses.

Command and output:

```
ip addr
```

Your private IPv4 address and prefix length:

```
inet 10.60.6.28/26 
```

### Step 2 — Read Your Route

Run the command that shows the routing table.

Command and output:

```
ip route
```

Your default gateway:

```
default via 10.60.6.1
```

### Step 3 — Compare to Week 5

Compare this live Ubuntu output to what the CLI Simulator produced in Week 5. What looks the same, what looks different, and what surprised you:

```
The Week 5 CLI Simulator was mainly intended to help me recognize foundational network information, such as the IP address, prefix length, interface, and default gateway. The live Ubuntu output showed those same foundations but included more real-world details, such as the loopback interface, broadcast address, MAC address, IPv6 address, MTU, and interface status. This reminded me of using `ifconfig` in my macOS Terminal because it also produces a longer and more detailed output. What surprised me was that a real operating system keeps track of much more network information than the simulator needed to show.

```

---

## Part B — The Gateway That Does Not Answer

### Step 1 — Ping the Gateway

Ping the default gateway address you recorded. Let it run a few seconds, then stop it.

Command and output:

```
Command: 10.60.6.1
Output: packets transmitted, 0 received, 100% packet loss
```

### Step 2 — Interpret It Correctly

You almost certainly got **no replies**. In Azure, the platform gateway commonly does not answer ICMP. This is **expected platform behaviour** and by itself proves nothing about whether your machine or network is broken.

Explain why "the gateway did not answer ping" is weak evidence:

```
“The gateway did not answer ping” is weak evidence because the gateway may be working but configured to ignore ping requests. Under the Ladder Rule, a failed gateway ping is only a clue, not enough proof that the gateway is unavailable. I would continue to test an outside IP address. If the outside IP responds, that proves the gateway is still forwarding traffic even though it did not answer the ping directly. If the outside IP responds, the gateway is forwarding traffic, I would then continue to test the DNS. 
```

---

## Part C — The Known-Good Target

The **Grid Beacon** at `10.60.6.4` is a machine that is known to be up and known to answer. When your first probe fails, you test against something known-good before you conclude anything.

### Step 1 — Ping the Beacon

```
ping 10.60.6.4
```
Output:

```
--- 10.60.6.4 ping statistics ---
26 packets transmitted, 26 received, 0% packet loss, time 25033ms
rtt min/avg/max/mdev = 1.019/1.401/3.078/0.536 ms
```

### Step 2 — Trace the Path

```
traceroute 10.60.6.4
```
Output:

```
 1  grid-beacon.internal.cloudapp.net (10.60.6.4)  1.265 ms  1.223 ms *
```

### Step 3 — Ask the Application

```
curl http://10.60.6.4
```
Output:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GRID BEACON | CVI CyberFoundations</title>
    <style>
        body {
            background: #071426;
            color: #d9f7ef;
            font-family: monospace;
            max-width: 850px;
            margin: 80px auto;
            padding: 30px;
        }
        .beacon {
            border: 1px solid #31d6a6;
            padding: 35px;
        }
        h1 { color: #31d6a6; }
        .label { color: #8ca8ff; }
        .status { color: #31d6a6; }
        .classified {
            margin-top: 30px;
            border-top: 1px solid #31445e;
            padding-top: 20px;
        }
    </style>
</head>
<body>
<div class="beacon">

    <h1>GRID BEACON</h1>

    <p><span class="label">NODE:</span> grid-beacon</p>
    <p><span class="label">NETWORK:</span> CVI Training Grid</p>
    <p><span class="label">STATUS:</span>
       <span class="status">ONLINE</span></p>
</div>
</div>
</body>
</html>
```

> ### ⚠️ Grid Beacon not responding?
> The Grid Beacon is shared course infrastructure and should normally be available. First, confirm your Cloud Heights VM shows **Running** and that you completed the preceding network checks. Then retry the command once after a minute or two.
>
> If the Grid Beacon still does not respond, **stop this part of the lab and contact your instructor.** Record that the shared service was unavailable; do not treat the result as evidence that your VM or your work is incorrect.
>
> Do not change networking, NSGs, firewall rules, routes, DNS, or any Azure settings to try to reach the beacon.
>
> *Instructor note: a confirmed Grid Beacon outage is an environment issue, not a student error. Affected students may complete this portion of Lab 03 after the service is restored, with no penalty.*

### Step 4 — Record the Application Evidence

The beacon returns a banner and a trace ID. Record exactly what you received:

```
Banner: BEACON | CVI CyberFoundations
Trace ID: CF-NET-0604
```

Explain the difference between what the `ping` proved and what the `curl` proved:

```
The ping command proved that the destination was reachable across the network and could return ICMP packets. It did not prove that the web application was working. The curl command sent an HTTP request and received the GRID BEACON page, proving that the web service was running and responding with application content.
```

### Step 5 — Capture Your Evidence

Two screenshots, both cropped to the terminal only:

**Required filename:** `vm-toolkit-live.png` — your `ip addr` and `ip route` output

![Live VM toolkit — ip addr and ip route](https://raw.githubusercontent.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/vm-toolkit-live.png)

**Required filename:** `beacon-reply.png` — your beacon ping/traceroute/curl evidence

![Grid Beacon reply](https://raw.githubusercontent.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/beacon-reply.png)

---

## Part D — Rewrite the Ladder Rule

Week 5 taught the Ladder Rule: test the near thing before the far thing. Real infrastructure adds a wrinkle — a silent rung is not automatically a broken rung.

Rewrite the Ladder Rule in your own words so that it survives real cloud infrastructure. Your version must include both **route/path evidence** and **a known-good target**:

```
Step 1: Check my machine’s network configuration. I would use ip addr to confirm that my interface is active and has an IP address and prefix length. I would then use ip route to confirm that my machine has a default gateway and knows where to send traffic outside its local network. If this information is missing or incorrect, I would investigate the interface or network configuration before testing anything farther away.

Step 2: Test the default gateway. I would ping the gateway to see whether it answers ICMP requests. If it responds, I know my machine can communicate with the gateway. If it does not respond, I would not immediately call it broken because cloud gateways and firewalls may ignore ping while continuing to forward other traffic.

Step 3: Test a known-good target by IP address. I would ping a target that is expected to be online, such as the Grid beacon. Using its IP address allows me to test network connectivity without depending on DNS. If the beacon responds even though the gateway did not, that proves my traffic is still leaving the local network and the gateway is forwarding it.

Step 4: Examine the route or path. I would use traceroute to view the hops traffic takes toward the destination. This provides path evidence and may help identify where responses stop or delays begin. However, asterisks do not always mean the route is broken because some routers forward traffic without answering traceroute.

Step 5: Test DNS. After proving that an IP address is reachable, I would use dig to check whether a domain name can be translated into the correct IP address. If the known-good IP works but the name does not resolve, DNS becomes the likely problem.

Step 6: Test the destination’s port and application. I would use a tool such as curl or ssh to test the actual service. A successful ping does not prove that a web server or SSH service is running. I would only decide after comparing network configuration, route or path evidence, a known-good target, DNS, and the application response.
```

---

## Analysis Questions

**Analysis Question 1.** Your ping to the gateway failed and your ping to the beacon succeeded. What does that pair of results, taken together, prove about your machine's networking? *(Minimum 3 sentences.)*

```
The failed gateway ping by itself did not prove that the gateway was unavailable because it may have been configured to ignore ICMP requests. The successful ping to the beacon proved that my machine had a working IP configuration and a usable route to another system. The results show that the gateway was still forwarding my traffic even though it did not respond directly to ping. This is why the Ladder Rule uses multiple tests instead of treating one silent response as a complete answer.
```

**Analysis Question 2.** Why is `traceroute` useful even when `ping` already answered? What extra thing does it show you? *(Minimum 2 sentences.)*

```
ping shows whether the destination responds, but traceroute displays the path taken toward that destination. It can show the routers, or hops, along the route and help identify where delays or missing responses begin. When asterisks show up during a traceroute command, it does not automatically mean the route failed because some routers forward traffic without replying to traceroute.
```

**Analysis Question 3.** A service is unreachable and ping to it succeeds. Where would you look next, and why is "the network is fine" an incomplete answer? *(Minimum 3 sentences.)*

```
If ping succeeds but the service is unreachable, I would next test the application directly. I could use curl to check whether a website responds or ssh to test an SSH connection. A successful ping only proves that the host responded to ICMP traffic. The application could still be stopped, its port could be closed, or a firewall could allow ping while blocking the service.
```

**Analysis Question 4.** Something already controls what is allowed to reach your machine in Cloud Heights. If you could decide those rules, what would you want to allow, what would you want to block, and who in an organization should get to make that decision? *(Minimum 3 sentences.)*

```
would allow only the traffic required for the machine to perform its job. For example, I would allow SSH only from approved administrative systems and allow web traffic only if the machine hosts a website. I would block unnecessary ports, services, and untrusted sources to reduce opportunities for unauthorized access. A cybersecurity team and network/system administrators should make these decisions together to ensure the team follow company procedures and policies. 
```

---

## Submission Checklist

- [x] `ip addr` output recorded and own private IP/prefix identified (Part A)

- [x] `ip route` output recorded and default gateway identified (Part A)

- [x] Live output compared to the Week 5 simulator (Part A, Step 3)

- [x] Gateway pinged and the silent result interpreted correctly (Part B)

- [x] Beacon `ping`, `traceroute`, and `curl` all run and recorded (Part C)

- [x] Beacon banner and TRACE ID recorded (Part C, Step 4)

- [x] `vm-toolkit-live.png` and `beacon-reply.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part C, Step 5)

- [x] Ladder Rule rewritten with route evidence + known-good target (Part D)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-03-the-grid-for-real.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 03: The Grid, For Real** in the Lab Portal.
2. Fill in the worksheet fields and upload both screenshots to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-03-the-grid-for-real.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
