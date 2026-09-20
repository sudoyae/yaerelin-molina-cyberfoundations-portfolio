# Week 6 Lab 01 — Claiming Your Room in Cloud Heights

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-01-claiming-your-room.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

Week 5 was practice. This week the machine is real. Cloud Heights is a live Ubuntu 22.04 server running in Azure, and one of its rooms has **already been reserved for you** — you do not create it, provision it, or pay for it. Your job in this lab is to walk in the front door, prove you are standing inside your own room, and understand where that room came from.

This is a **guided** lab. Every step tells you what to do and what to record. Expect 30–40 minutes.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM in Azure, reached through Azure Bastion in your browser |
| Access | Lab Portal → **My Lab Environment** → your Cloud Heights card |
| Username | `analyst` |
| Password | Provided to you separately. Never typed into this worksheet. |
| Commands used | `hostname`, `whoami`, `pwd` |
| Auto-shutdown | Your VM stops automatically after 15 minutes of inactivity. A warning with **Keep Working** appears first. |

**Before you start:** open **My Lab Environment** in the Portal. If your VM shows **Stopped**, click **Start VM** and wait until the status reads **Running** — this takes a minute or two. Only then click **Open Cloud Heights**.

---

## Part A — Walking In

### Step 1 — Start the Room

In **My Lab Environment**, check your Cloud Heights status. Start the VM if it is stopped and wait for **Running**.

The status you saw before you started, and the status you saw after:

```
Before I started the Cloud Heights VM, its status was Stopped. After I started it and waited for it to finish loading, its status changed to Running.
```

### Step 2 — Open Cloud Heights and Sign In

Click **Open Cloud Heights**. A browser-based session opens through Azure Bastion. Sign in with username `analyst` and the password you were given separately.

**Do not record the password, the link, or any part of the login screen anywhere.**

Describe what you saw once the session opened — what kind of screen greeted you:

```
Once the session opened, I was greeted by a black Linux terminal screen for the CyberVisionaries Institute CyberFoundations Lab. It displayed a welcome banner identifying the system as the Cloud Heights Technologies Student Investigation Node and listed areas such as IP addressing, network routes, DNS resolution, ICMP connectivity, TCP/IP services, SSH, and network troubleshooting. At the bottom, I saw the analyst@cf-student-09:~$ command prompt, which showed that I was connected and ready to enter Linux commands.
```

### Step 3 — Ask the Machine Its Name

Run:
```
hostname
```
Output:

```
cf-student-09
```

### Step 4 — Ask Who You Are

Run:
```
whoami
```
Output:

```
analyst

```

### Step 5 — Ask Where You Are Standing

Run:
```
pwd
```
Output:

```
/home/analyst
```

---

## Part B — What Those Three Answers Prove

### Step 1 — Read Them as Evidence

Each of those three commands answered a different question: *which machine*, *which identity*, *which location in the filesystem*. Together they are the proof that you are inside your own room and not somebody else's.

Explain, in your own words, what each output proves:

```
hostname returned cf-student-09: This proves that I am connected to the correct Cloud Heights virtual machine named cf-student-09. The hostname identifies the specific computer I am currently using.

whoami returned analyst: This proves that I am signed in under the analyst user account. It tells me which account’s permissions I have while entering commands.

pwd returned /home/analyst: This proves that my current working directory is the analyst account’s home folder. Any command that uses a relative file path will begin from this location.

Together, these results confirm that I am connected to the cf-student-09 Linux machine, signed in as analyst, and currently working inside /home/analyst.
```

### Step 2 — Capture Your Evidence

Take a screenshot of your terminal showing the three commands and their outputs.

**Required filename:** `bastion-session.png`

**Crop rules — not optional.** The screenshot must show the terminal and prompt. It must **not** show the browser address bar, the Bastion link, any login screen, or any password field. Crop before you upload.

Upload it to `assets/screenshots/week-06/` in your portfolio repository, then paste its link here:

![Cloud Heights session — hostname, whoami, pwd](https://github.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/blob/main/assets/screenshots/week-06/bastion-session.png?raw=true)

---

## Part C — Where Your Room Came From

### Step 1 — The Golden Image Idea

Every student's Cloud Heights room was built from the **same standardized image** — a known-good snapshot of a configured Ubuntu machine. Nobody hand-built 20 servers. One machine was configured correctly once, captured, and stamped out repeatedly.

Explain in your own words what a standardized (golden) image is and why an organization would build one:

```
A standardized golden image is important so that a company uses it to create many machine with the same secure configuration instead of setting up each computer manually. For example, an entry-level SOC analyst might receive an alert showing that an unfamiliar service or port has appeared on an employee’s computer. The analyst could compare the computer’s current configuration with the organization’s golden image to determine whether that service belongs in the approved setup. If it does not, the analyst could document the difference, investigate whether malware or an unauthorized change caused it, and escalate the incident. If the machine is compromised, the IT team could rebuild it from the trusted golden image instead of trying to manually remove every harmful change.
```

### Step 2 — Same Start, Different Rooms

Your room started identical to everyone else's, and from today it starts to diverge as you work in it.

Explain what stays the same across all the rooms and what becomes yours alone:

```
Every Cloud Heights room started with the same Ubuntu system, tools, accounts, and security settings because they came from the same golden image.

My room becomes unique as I run commands, create files, change settings, and generate my own activity logs. It also has its own hostname and private IP address
```

---

## Analysis Questions

**Analysis Question 1.** Why does it matter that a standardized image can be *restored*, not just deployed? Describe a realistic situation where restoring from a known-good image is the fastest safe fix. *(Minimum 3 sentences.)*

```
If someone damages their copy, you can print a clean one from the master instead of trying to erase every mistake. For example, if malware changes system files and opens unauthorized ports, restoring the computer from a known-good image may be safer and faster than trying to locate every harmful change. Important evidence should be saved first, and then the machine can be returned to its approved condition.
```

**Analysis Question 2.** Conceptually, how is a snapshot different from a separate backup? Consider what each one protects against and where each one lives. *(Minimum 3 sentences.)*

```
A snapshot is like saving your progress in a video game, while a backup is like keeping another copy of the game data on a separate drive. Furthermore, a snapshot allows it to be rolled back quickly protecting the progress. A backup however, allows your system to revive the copy even when the system is destroyed. Snapshot is temporary and a backup is more permanent. 
```

**Analysis Question 3.** Your room was reserved for you rather than created by you. What does that tell you about how cloud access is usually handed out in a real organization, and why would an employer prefer that model? *(Minimum 2 sentences.)*

```
This room was reserved for me than created by me because it prevents me from accessing unapproved systems and prevents any financial costs. For example, a SOC analyst may receive access to security logs but not permission to change the company’s financial systems.
```

---

## Submission Checklist

- [x] VM started from My Lab Environment and confirmed **Running** (Part A, Step 1)

- [x] Signed in through Bastion as `analyst` — no credentials recorded anywhere (Part A, Step 2)

- [x] `hostname`, `whoami`, and `pwd` run and outputs recorded (Part A, Steps 3–5)

- [x] Explained what each of the three outputs proves (Part B, Step 1)

- [x] `bastion-session.png` captured, address bar and login data cropped out, uploaded to `assets/screenshots/week-06/` (Part B, Step 2)

- [x] Standardized/golden image explained in your own words (Part C)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-01-claiming-your-room.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 01: Claiming Your Room in Cloud Heights** in the Lab Portal.
2. Fill in the worksheet fields — they match this file, in the same order.
3. Connect your GitHub account if you haven't already, and select your portfolio repo.
4. Click **Submit to GitHub**. The Portal commits the completed file to `week-06/labs/lab-01-claiming-your-room.md`.
5. Upload `bastion-session.png` to `assets/screenshots/week-06/` in your repo before you submit.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
