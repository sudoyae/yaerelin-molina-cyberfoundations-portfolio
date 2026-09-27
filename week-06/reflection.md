# Week 6 Reflection

**Student Name:** Yaerelin Molina

**Date Completed:**

## Prompts

What clicked for you this week?

```
I finally understood how my VM can have a private IP address and still reach the internet. My Cloud Heights VM uses 10.60.6.28 inside its private network, and its default route tells it where to send traffic meant for destinations outside that network. NAT then translates the outbound traffic so the VM can communicate with the internet without having its own public IP address.
```

What's still confusing?

```
I’m still practicing how to tell exactly what each network test proves. For example, my gateway did not answer ping, but the Grid Beacon did. I now understand that reaching the Beacon proves my local subnet works, while checking a destination outside the subnet tells me more about the default route. I need more practice recognizing those differences without assuming that one failed test means the whole network is down. I can test the default path by contacting an address outside 10.60.6.0/26. 
```

How does this week's material connect to a cybersecurity career path you're interested in?

```
## How does this week’s material connect to a cybersecurity career path you’re interested in?

I’m interested in protecting essential services such as hospitals and water systems. Those organizations need their systems to communicate reliably, so understanding private IP addresses, outbound access through NAT, and network security rules matters. If a hospital system could reach other machines inside its network but not an outside service it depends on, I would need to test each part of the connection before deciding what failed. Practicing that process on my Cloud Heights VM helped me build a foundation for IT support and, eventually, cybersecurity work in critical infrastructure.
```

One thing you'd tell a friend just starting this course.

```
I would explain private IP addresses, gateways, and NAT using a hotel. A private IP address is like a room number used inside the building. The gateway is the way traffic leaves the local network, and NAT lets a machine send traffic outward without having its own public IP address. Connecting each term to a familiar example helped me understand how my cloud VM could reach the internet from a private network.
```

---

## Professional Growth Check

- [x] I documented my reflection clearly and in my own words

- [x] I used structured formatting in my submission

- [x] My commit message was meaningful and descriptive

- [x] I checked that I did not include my Bastion shareable URL or Cloud Heights password in my reflection

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
