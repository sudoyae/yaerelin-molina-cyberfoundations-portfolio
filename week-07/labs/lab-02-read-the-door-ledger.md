# Week 7 Lab 02 — Read the Door Ledger

**Student Name:** Yaerelin Molina

**Date Completed:**

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-02-read-the-door-ledger.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Learn to read inbound and outbound rule ledgers in evaluation order. Translate a rule from field values into plain English, then predict which matching rule makes the decision.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights → Security Rules |
| Change level | Read-only reasoning |
| Evaluation | Lower priority number first; first match wins |
| Time | 25–30 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

The priorities **250**, **300**, and **350** used in this lab are **hypothetical examples on paper only — do not create them**. This lab is prediction-only: do not add, edit, or delete rules, and do not run **Test My Rule** unless your instructor tells you to.

Remember the live baseline also contains the protected priority **1000** `deny-tcp8080-student-subnet` fallback (Inbound Deny TCP from `10.60.6.0/26` to port 8080), which is reached only when no earlier rule matches.

Two inbound rules match TCP 8080 from the same source: priority 250 is **Deny** and priority 300 is **Allow**. Predict the verdict before reading further.

```text
Prediction: Deny. I would read the inbound rules from the lowest priority number upward. The TCP 8080 traffic from 10.60.6.4 matches the hypothetical priority 250 Deny rule first, so evaluation stops there. The hypothetical priority 300 Allow rule also describes that traffic, but it comes later and cannot change the result. The protected priority 1000 fallback is not reached either. Priority 1000 as a backup instruction is used only when no earlier rule matches.
```

## Guided Steps

### Step 1 — Separate the Ledgers

Scroll below the yellow protected-rules summary to the detailed lists headed **INBOUND — EVALUATION ORDER** and **OUTBOUND — EVALUATION ORDER**. Read the inbound list, then the outbound list. Record one sentence explaining why an inbound allow does not automatically create an outbound allow.

```text
An inbound Allow might let the Grid Beacon contact my VM on port 8080, but it does not automatically allow my VM to start a separate outbound connection to the Grid Beacon because outbound traffic is evaluated against its own rules.
```

### Step 2 — Translate a Rule

Choose one visible protected rule and translate it using this form:

> Read in the [direction] ledger at priority [number], [allow/deny] [protocol] traffic from [source]:[source port] to [destination]:[destination port].

```text
Read in the inbound ledger at priority 1000, deny TCP traffic from 10.60.6.0/26:any source port to any destination:port 8080.
```

### Step 3 — Evaluate in Order

For each hypothetical scenario below, list the rules in evaluation order, identify the first matching rule, and state the verdict. These rules are imaginary — do not create them.

1. Priority 250 Deny TCP from `10.60.6.4` to port 8080; priority 300 Allow the same traffic.
2. Priority 300 Allow TCP from `10.60.6.4` to port 8080; priority 350 Deny TCP from any source to port 8080.
3. An inbound Allow exists, but the traffic being evaluated is outbound.

```text
Scenario 1:
- Priority 250: Deny TCP from 10.60.6.4 to port 8080
- Priority 300: Allow TCP from 10.60.6.4 to port 8080
First matching rule: Priority 250 Deny
Verdict: Deny
Scenario 2:
- Priority 300: Allow TCP from 10.60.6.4 to port 8080
- Priority 350: Deny TCP from any source to port 8080
First matching rule: Priority 300 Allow
Verdict: Allow
Scenario 3:
- Inbound Allow rule: Does not match because the traffic is outbound
- First matching rule: None from the given rule
Verdict: Not allowed by the given inbound rule
```

## Stop & Check

If you find yourself reading the Allow you want and ignoring an earlier matching Deny, restart at the lowest priority number. The ledger stops at the first match.

## Test

Use the displayed rule list to predict whether Grid Beacon TCP 8080 would currently be explicitly allowed by a student rule. Do not press **Test My Rule** yet unless your instructor directs you; the service may not be listening.

## Capture Evidence

Capture the detailed **INBOUND — EVALUATION ORDER** view (and **OUTBOUND — EVALUATION ORDER** if your evidence needs it), then name the first matching hypothetical rule and the resulting verdict for each scenario in your worksheet.

![Rule view in evaluation order — week07-lab02-evaluation-order.png](https://raw.githubusercontent.com/sudoyae/yaerelin-molina-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/lab02-evaluation-order.png)

## Explain

Write a five-sentence explanation of first-match-wins that a classmate could use without memorizing Azure terminology.

```text
First-match-wins means the rules are checked one at a time in a specific order. As soon as a rule matches the traffic, that rule decides what happens. If the first matching rule says to allow it, the traffic is allowed. If it says to deny it, the traffic is denied. The other rules are ignored because the first matching rule has already made the decision.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab02-evaluation-order.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** If a Deny at priority 300 and an Allow at priority 400 both match, which wins and why? (Minimum 3 sentences.)

```text
A Deny rule at priority 300 wins because it is checked before the Allow rule at priority 400. Since the Deny rule matches the traffic first, the traffic is denied. The Allow rule is not reached.
```

**Analysis Question 2.** Why do inbound and outbound rules have to be reasoned about separately? (Minimum 3 sentences.)

```text
Inbound and outbound rules need to be considered separately because they apply to different directions of traffic. An inbound rule controls traffic coming into a system, while an outbound rule controls traffic leaving it. Therefore, an Allow rule for inbound traffic does not automatically allow the same traffic when it is going outbound.
```

**Analysis Question 3.** How can an Allow rule be correct by itself but ineffective in the full ledger? (Minimum 3 sentences.)

```text
An Allow rule can be correct by itself but still be ineffective if another matching rule is evaluated before it. If the earlier rule denies the traffic, the Allow rule will never be reached. This is why the order of the rules matters just as much as what each rule says.
```

## Submission Checklist

- [x] Three scenarios evaluated in order

- [x] One live rule translated to plain English

- [x] Inbound and outbound ledgers distinguished

- [x] `week07-lab02-evaluation-order.png` captured

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-02-read-the-door-ledger.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 02: Read the Door Ledger** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
