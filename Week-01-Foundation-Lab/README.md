# Week 01 — Foundation & Lab Setup

This week focuses on building a stable, detection-ready Purple Team lab
that can support realistic attack simulation and investigation workflows.

The goal is not to rush into attacks, but to create an environment where
logs, telemetry, and detections can be trusted.

This lab is designed to be:
- Small and controlled
- Isolated from the home network
- Close to real-world security setups
- Suitable for long-term detection engineering practice

---

## Week 01 Objectives

- Design a clear Purple Team lab architecture
- Build a stable SOC environment using Elastic Stack
- Enroll a Windows endpoint with proper telemetry
- Avoid fragile VM networking patterns
- Document real-world setup failures and fixes

---

## Lab Components

- **SOC Host (Windows)**  
  Elasticsearch, Kibana, Fleet Server

- **Endpoint (Windows VM)**  
  Sysmon + Elastic Agent (Fleet-managed)

- **Attacker (Kali Linux)**  
  Separate physical machine

- **Networking**  
  Tailscale VPN overlay (no NAT / bridged dependency)

---

## Repository Structure (Week 01)
Week-01-Foundation-Lab/
├── 01-architecture/
├── 02-setup/
├── 03-troubleshooting/
└── 04-notes/



Each section builds on the previous one:
- Architecture explains *what and why*
- Setup explains *how*
- Troubleshooting explains *what broke and how it was fixed*
- Notes capture lessons learned

---

## ⚠️ Important Note for Readers

This is **not** a copy-paste tutorial.

The intent is to help you:
- understand design decisions
- avoid common mistakes
- reason about detection infrastructure

You are encouraged to adapt this lab to your own system constraints.

---

## 🤖 AI-Guided Setup (Recommended)

To help readers reproduce this lab safely and methodically, an AI-guided
setup prompt is provided below.

You can paste this prompt into an AI model and follow the setup step by
step, adapting it to your system.

```
You are a senior Purple Team / Detection Engineer acting as a hands-on
setup assistant.

Your role is to guide me step by step in building a Purple Team detection
lab similar to a real SOC environment.

RULES:
- Do NOT assume anything about my system
- Ask questions before giving commands
- Move one step at a time
- Wait for confirmation before proceeding
- Explain WHY a step is needed, not just HOW
- If something breaks, help debug instead of restarting blindly
- Prefer stability and observability over speed

GOAL:
Build a Purple Team lab with:
- A SOC system running Elasticsearch, Kibana, and Fleet Server
- A Windows endpoint VM enrolled via Fleet
- Sysmon + Windows Event Logs collected
- A separate attacker machine (Kali Linux)
- Centralized logging suitable for detection engineering

START BY ASKING ME THESE QUESTIONS ONLY:

1. What is your primary host operating system?
2. CPU, RAM, and disk available on the host?
3. Do you have virtualization support enabled?
4. Will your attacker machine be:
   - a VM
   - or a separate physical system?
5. Are you willing to use a VPN-based overlay network instead of NAT/bridged networking?

AFTER I ANSWER:
- Help me decide realistic VM specs
- Decide which machine hosts the SOC
- Design a network layout
- Guide Elastic Stack installation safely
- Set up Fleet Server correctly
- Enroll the Windows endpoint cleanly
- Validate that logs are flowing
- Help me confirm when the lab is “ready”

IMPORTANT:
- Treat Elastic Agent as stateful
- Never suggest reinstalling without cleanup
- Call out common Windows-specific pitfalls
- Keep security features enabled by default

Your final goal is to help me reach a stable,
observable Purple Team lab, not just a working one.
```
