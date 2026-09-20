# Week 5 Reflection

**Student Name:** Yaerelin Molina

**Date Completed:**

## Prompts

What clicked for you this week?

```
What clicked for me this week was learning how to use the Ladder Rule to troubleshoot a network in an organized way. Instead of immediately assuming that the internet is down, I can begin by checking my computer’s network configuration, including its IP address, subnet mask, default gateway, and DNS server. I can then move up the ladder by testing the gateway, an outside IP address, DNS, and finally the destination’s port or service. This helped me understand that each command answers a different question and that the first failed rung can help identify where the network problem is located.
```

What's still confusing?

```
What was still confusing for me was remembering which commands to use and understanding the difference between tools that seem similar. I especially confused dig with traceroute. An easy way for me to remember the difference is that dig digs up the IP address connected to a domain name, while traceroute traces the route that packets take to reach that address. For example, dig example.com helps me check whether DNS can find the website’s IP address, while traceroute example.com shows the different routers, or hops, that my traffic passes through. I was also confused because some commands are different on macOS, Linux, and PowerShell. I need more practice choosing the correct command for my operating system and understanding what each result proves before moving to the next step of the Ladder Rule.
```

How does this week's material connect to a cybersecurity career path you're interested in?

```
This week’s material connects to my interest in industrial cybersecurity, where professionals protect operational technology and the systems that support critical infrastructure, such as power, water, transportation, and manufacturing. These environments contain connected devices such as sensors, controllers, and monitoring systems that communicate through IP addresses, ports, and network protocols. Understanding DNS, DHCP, gateways, TCP and UDP, and the Ladder Rule would help me determine whether unusual activity is caused by a normal network problem or a possible cyberattack. For example, an unfamiliar IP address, an unnecessary open port, or repeated failed SSH connections could indicate unauthorized access. In industrial environments, even a small network problem can interrupt physical operations, so knowing how to troubleshoot carefully and recognize abnormal traffic is important for protecting both computer systems and public safety.
```

One thing you'd tell a friend just starting this course.

```
I would tell a friend not to focus only on memorizing commands. First, understand what each command is testing and remember that macOS, Linux, and PowerShell may use different commands for the same purpose. Following the Ladder Rule one step at a time makes networking feel less overwhelming because it helps you understand where a problem is occurring instead of guessing.
```

---

## Professional Growth Check

- [x] I documented my reflection clearly and in my own words

- [x] I used structured formatting in my submission

- [x] My commit message was meaningful and descriptive

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
