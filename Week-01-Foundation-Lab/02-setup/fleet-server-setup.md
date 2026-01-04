# Fleet Server Setup

Fleet Server acts as the control plane for Elastic Agents in this lab.
It is responsible for agent enrollment, policy distribution, and health
monitoring.

This section documents how Fleet Server was deployed, where it was placed,
and the issues encountered during setup.

---

## Role of Fleet Server in the Lab

Fleet Server is used to:

- Enroll Elastic Agents securely
- Distribute agent policies
- Monitor agent health and connectivity
- Act as a single management endpoint for all agents

Without Fleet Server, agent configuration becomes fragmented and difficult
to reason about.

---

## Placement Decision

Fleet Server is hosted on the SOC machine alongside Elasticsearch and Kibana.

### Reasons for this choice

- Simplifies agent enrollment
- Avoids circular dependencies between endpoints
- Keeps the control plane separate from attack activity
- Reduces the chance of agent corruption during testing

Initial attempts to colocate Fleet Server on the endpoint VM caused
confusion between endpoint and control roles and led to repeated agent
installation failures.

Centralizing Fleet Server resolved these issues.

---

## Network Configuration

Fleet Server listens on:  https://<SOC_TAILSCALE_IP>:8220
```  
Key characteristics:

- Communication occurs only over the Tailscale VPN
- No exposure to the home network
- TLS enabled by default
- Certificate fingerprint validation enforced

Using a VPN overlay ensured stable connectivity and avoided reliance on
hypervisor networking modes.

---

## Enrollment and Security

Fleet Server was enrolled using:

- A service token generated from Kibana
- TLS certificate fingerprint verification
- Default security settings

No security features were disabled during setup.

This ensured that:
- Agents could not enroll without authorization
- Management traffic remained encrypted
- The lab stayed close to real-world defaults

---

## Fleet Configuration in Kibana

After Fleet Server enrollment, the following were verified in Kibana:

- Fleet Server host URL correctly set
- Elasticsearch output configured over HTTPS
- Default agent binary source retained
- Fleet Server agent status marked as healthy

At this stage, no endpoint agents were enrolled.

---

## Common Issues Encountered

### Confusion Between Fleet Server and Endpoint Agents

A key early mistake was attempting to install multiple Elastic Agents on
the same machine using different roles.

Elastic Agent supports only one installation per machine. Attempting to
reinstall an agent with a different role leads to inconsistent states and
service corruption.

This reinforced the importance of clearly separating:
- Fleet Server (control plane)
- Endpoint agents (data plane)

---

### Partial Agent Installations on Windows

Repeated installation attempts on Windows resulted in situations where:
- The Windows service existed
- The installation directory was missing

In these cases, Elastic Agent could not recover automatically and required
manual cleanup.

These scenarios are documented in the troubleshooting section.

---

## Verification Criteria

Fleet Server setup was considered complete when:

- Fleet Server appeared as **Healthy** in Kibana
- Fleet Server policy was applied correctly
- Agents could reach Fleet Server over the VPN
- No endpoint agents were enrolled yet

Only after these checks was endpoint onboarding attempted.

---

## Why This Matters

Fleet Server stability directly affects every downstream activity:
- Endpoint visibility
- Policy enforcement
- Detection accuracy

Treating Fleet Server as core infrastructure rather than a background
component reduces friction and makes later detection work significantly
easier.
```
