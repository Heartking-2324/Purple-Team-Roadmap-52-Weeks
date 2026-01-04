# Troubleshooting: Issues and Fixes

This section documents the major issues encountered while building the
Week 01 lab, along with their root causes and resolutions.

These problems were not edge cases; they reflect real-world friction that
occurs when working with Elastic, Windows endpoints, and mixed networking
environments.

---

## 1. Virtual Networking Instability (NAT / Bridged)

### Problem
Initial attempts relied on VMware NAT and bridged networking for
connectivity between systems. Connectivity was inconsistent and frequently
failed after reboots or service restarts.

Symptoms included:
- Intermittent internet access in VMs
- VMware NAT services stopping unexpectedly
- Inconsistent routing between machines

### Root Cause
Windows 11 virtualization-based security, VPN adapters, and security
software interfered with hypervisor networking.

This resulted in fragile layer-2 and layer-3 behavior.

### Fix
- Abandoned NAT and bridged networking entirely
- Moved all inter-system communication to a VPN overlay
- Used Tailscale for stable, encrypted connectivity

### Lesson
Hypervisor networking is convenient but fragile. Overlay networks are more
predictable and closer to modern enterprise patterns.

---

## 2. Fleet Server and Endpoint Role Confusion

### Problem
Fleet Server and endpoint Elastic Agents were initially installed on the
same system without a clear separation of roles.

This caused:
- Agent enrollment confusion
- Repeated installation failures
- Difficulty determining which agent performed which role

### Root Cause
Elastic Agent supports only one installation per machine. Attempting to
reuse the same system for multiple roles without planning led to conflicts.

### Fix
- Clearly separated roles:
  - SOC host → Fleet Server
  - Windows VM → Endpoint agent
- Stopped reinstalling agents on the same system
- Treated Fleet Server as core infrastructure

### Lesson
Elastic Agent roles must be planned ahead. Control-plane and data-plane
roles should be logically separated, even in small labs.

---

## 3. Elastic Agent "Already Installed" Error

### Problem
Elastic Agent repeatedly failed with:
Error: already installed at C:\Program Files\Elastic\Agent


Even when:
- The service was not visible
- The installation directory appeared incomplete

### Root Cause
Partial installations left behind:
- Windows service entries
- Registry keys
- Missing or corrupted installation files

Elastic Agent could not recover automatically from this state.

### Fix
A full manual cleanup was required:
- Stop and delete the Windows service
- Kill all Elastic-related processes
- Remove all Elastic directories
- Clean registry entries
- Reboot before reinstalling

Only after a clean system state would enrollment succeed.

### Lesson
On Windows, Elastic Agent is sensitive to partial installs.
Blind reinstallation attempts make the problem worse.
Always reset to a clean state before retrying.

---

## 4. Fleet Server Enrollment Failures

### Problem
Fleet Server enrollment occasionally failed or appeared to hang during
initial setup.

### Root Cause
Enrollment attempts were made while:
- Elasticsearch was not fully started
- Fleet Server URLs were misconfigured
- Incorrect assumptions were made about localhost vs VPN addresses

### Fix
- Ensured Elasticsearch was fully healthy before enrollment
- Used VPN (Tailscale) IPs consistently
- Verified Fleet Server URLs in Kibana settings

### Lesson
Control-plane services depend on precise ordering.
Rushing enrollment leads to misleading errors.

---

## 5. Same Hostname on Multiple Systems

### Problem
Both the SOC and endpoint VM shared the same hostname, leading to confusion
during troubleshooting.

### Root Cause
Windows default hostnames were reused during installation.

### Fix
- Relied on Agent IDs rather than hostnames for identification
- Deferred hostname cleanup until after stable enrollment

### Lesson
Hostnames help readability but are not authoritative.
Agent IDs and policies matter more during debugging.

---

## Final Takeaways

Most issues encountered were not caused by incorrect commands, but by:
- Unclear role separation
- Repeated retries without cleanup
- Overreliance on fragile networking
- Treating Elastic Agent as stateless when it is not

Documenting these failures was essential to building a stable lab and will
directly inform later detection engineering work.

This troubleshooting process is as valuable as the final working setup.
