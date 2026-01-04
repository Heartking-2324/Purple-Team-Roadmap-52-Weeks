# SOC Setup (Elastic Stack)

The SOC host acts as the central point for detection, investigation, and
agent management in this lab.

Rather than distributing components across multiple machines, the SOC
stack is intentionally consolidated on a single Windows host to keep the
environment simple, observable, and resource-efficient.

---

## SOC Role and Responsibilities

The SOC host is responsible for:

- Ingesting telemetry from monitored endpoints
- Managing Elastic Agents via Fleet Server
- Providing visibility and investigation through Kibana
- Serving as the control plane for the lab

No attack activity originates from this system.

---

## Components Deployed

### Elasticsearch
- Purpose: Central log storage and search engine
- Security:
  - TLS enabled by default
  - Authentication enforced
- Configuration:
  - Single-node deployment
  - Heap size reduced for lab usage

Elasticsearch is treated as the system of record for all telemetry.

---

### Kibana
- Purpose:
  - Log exploration and analysis
  - Fleet management
  - Detection visibility
- Access:
  - Local access only
  - No exposure to external networks

Kibana serves as the primary interface for investigation and learning.

---

### Fleet Server
- Purpose:
  - Centralized Elastic Agent management
  - Policy distribution
  - Agent enrollment and health monitoring
- Placement:
  - Hosted on the SOC machine

Placing Fleet Server on the SOC simplifies agent enrollment and avoids
circular dependencies between endpoints and control infrastructure.

---

## Installation Approach

Elastic Stack components were installed using the official distribution
packages provided by Elastic.

Key decisions during installation:

- Default security settings were preserved
- No attempt was made to disable TLS or authentication
- Memory usage was tuned post-installation for stability

The goal was to stay as close as possible to a secure-by-default
configuration.

---

## Resource Considerations

Because this lab runs alongside virtual machines, resource usage was
carefully managed.

Adjustments included:
- Reducing Elasticsearch JVM heap size
- Avoiding unnecessary integrations
- Running a single-node cluster

This ensured the SOC remained responsive even during attack simulations.

---

## Verification

The SOC setup was considered complete once:

- Elasticsearch responded over HTTPS
- Kibana loaded successfully and authenticated
- Fleet Server showed a healthy status in Kibana
- No agents were enrolled yet

Endpoint onboarding was intentionally deferred to later steps.

---

## Why This Matters

A stable and predictable SOC environment is critical for detection
engineering. If the logging or control plane is unreliable, it becomes
impossible to reason about what is actually happening during attacks.

This setup prioritizes clarity and stability over scale.

