# Lab Overview

This lab is designed to support a full Purple Team workflow, where
attack simulation, detection, and investigation are treated as a single
continuous process.

The focus is not on running exploits, but on observing how real systems
behave when they are attacked, what telemetry is generated, and what a
defender can realistically detect.

This lab is intentionally small, isolated, and controlled, while still
reflecting patterns used in real environments.

---

## Core Goals

- Simulate attacks from an attacker’s perspective
- Observe system and security telemetry generated during attacks
- Centralize logs for analysis and detection engineering
- Document failures, misconfigurations, and design trade-offs

---

## Lab Components

### SOC (Security Operations Center)
- Platform: Windows host system
- Role:
  - Elasticsearch (log storage and search)
  - Kibana (analysis and visualization)
  - Fleet Server (central agent management)
- Purpose:
  - Acts as the detection and investigation environment
  - Central point for all telemetry and analysis

---

### Endpoint (Victim System)
- Platform: Windows virtual machine
- Role:
  - Primary target for attack simulations
  - Generates security and system telemetry
- Telemetry Sources:
  - Windows Event Logs (Security, System, Application)
  - Sysmon (process, network, and execution visibility)
  - Elastic Agent (log shipping and endpoint telemetry)

---

### Attacker
- Platform: Kali Linux (separate physical laptop)
- Role:
  - Simulates attacker behavior
  - Performs controlled attack techniques
- Separation:
  - Physically separated from SOC and endpoint
  - Mimics real-world remote attacker conditions

---

## Architectural Principles

- Clear separation between attacker, defender, and victim roles
- Centralized logging with minimal dependencies
- Isolation from the home network
- Reproducible and observable attack paths

This architecture is intentionally simple but structured in a way that
supports deeper detection engineering work in later stages of the roadmap.

