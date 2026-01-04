# Windows Endpoint Setup

The Windows endpoint represents the primary victim system in this lab.
All attack simulations, detections, and investigations originate from
activity on this machine.

The endpoint is intentionally kept minimal, monitored, and isolated to
ensure that generated telemetry is meaningful and easy to reason about.

---

## Endpoint Role

The endpoint is responsible for:

- Executing attacker-controlled actions
- Generating system and security telemetry
- Sending logs to the SOC via Elastic Agent
- Acting as the ground truth for detection validation

No defensive or management services originate from this system.

---

## Operating System

- Platform: Windows 11
- Deployment: Virtual Machine
- Purpose:
  - Safe isolation from the host
  - Ability to reset and snapshot
  - Controlled execution of attack techniques

The endpoint VM has no direct exposure to the home network.

---

## Telemetry Sources

### Windows Event Logs

The following event channels are collected:

- Security
- System
- Application

These logs provide:
- Authentication activity
- Process creation events
- Service changes
- System-level errors

They form the baseline for most detection logic.

---

### Sysmon

Sysmon is installed to provide enhanced visibility beyond default Windows
logging.

Key telemetry includes:
- Process creation with command-line arguments
- Parent-child process relationships
- Network connections
- Image loading events

Sysmon acts as the primary signal source for understanding attacker behavior.

---

## Elastic Agent (Endpoint Mode)

Elastic Agent is installed on the endpoint in **Fleet-managed mode**.

Important characteristics:
- Managed entirely by Fleet Server
- No local configuration edits
- Policies applied centrally
- Secure enrollment using enrollment tokens

The agent’s role is strictly to collect and forward telemetry.

---

## Enrollment Process

The endpoint agent is enrolled by:

1. Generating an enrollment command from Kibana
2. Running the command on the endpoint as Administrator
3. Connecting to Fleet Server over the VPN overlay
4. Receiving the `windows-endpoint-lab` policy

Once enrolled, the agent operates as a background service.

---

## Policy Applied

The endpoint agent uses the `windows-endpoint-lab` policy, which includes:

- Windows integration
  - Security
  - System
  - Application
- Sysmon integration
- Elastic Defend (detect-only mode)

No prevention or blocking actions are enabled at this stage.

The focus is observation, not response.

---

## Verification

The endpoint setup is considered successful when:

- The agent appears as **Healthy** in Fleet
- The agent is assigned to the correct policy
- Logs appear in Kibana Discover
- Process creation events are visible

A simple validation step includes running basic commands on the endpoint
and confirming that corresponding events appear in the SOC.

---

## Common Pitfalls

Several issues were encountered during endpoint setup:

- Partial Elastic Agent installations on Windows
- Service entries existing without installation files
- Attempting to reinstall an agent without cleanup
- Confusion between Fleet Server and endpoint roles

These are documented in detail in the troubleshooting section.

---

## Why This Matters

The endpoint is where attacker actions become observable reality.

If endpoint telemetry is unreliable or incomplete, detection logic becomes
guesswork. A clean, well-monitored endpoint ensures that later detection
engineering work is grounded in accurate data.

