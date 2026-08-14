# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Yaerelin Molina

**Date Completed:**

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- File permissions: read/write/execute × owner/group/other, and reading `ls -l`
- Changing permissions with `chmod` (symbolic and numeric) — and THE GATEKEEPER'S RULE
- Windows ACLs, read with `Get-Acl` (the real-world `icacls` tool does the same job, but is not available in the simulator)
- Wildcards (`*`, `?`, `[ ]`) and searching inside files with `grep`/`Select-String`
- Virtual machines: host vs. guest, the hypervisor, Type 1 vs. Type 2, isolation
- The VM lifecycle: create, start, stop (deallocate), snapshot, delete — and what each costs
- Golden snapshots — how your Weeks 6–12 lab machines are made

## In My Own Words

**Decode `-rw-r-----` audience by audience: who can do what to this file?**

```
The owner of the file can read and write the file. The group can only read it and everyone else has no access to read, write, or execute. 
```

**What is a hypervisor, and what are its two jobs?**

```
A hypervisor is a program that lets one physical computer run multiple virtual machine. One of the main jobs are to give each virtual machine the resources it needs, such as CPU, RAM, and storage. The next main job is to isolates guest to prevent them from interfering with each other. 
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
A stopped VM still costs a little money due to the SSD/storage still running. The reason why storage still runs is so that the OS, files, and its applications are saved even when stopped. To reach true zero, you must delete the virtual computer itself. 
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-04/notes.md`
