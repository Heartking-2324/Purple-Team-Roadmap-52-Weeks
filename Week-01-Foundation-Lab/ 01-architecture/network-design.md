# Network Design

The network design of this lab prioritizes stability, isolation, and
realistic communication paths over convenience.

Rather than relying on virtual machine NAT or bridged networking, the lab
uses an overlay network approach that more closely resembles modern
enterprise environments.

---

## Network Model

- All systems communicate over a private VPN overlay
- No direct exposure to the home network
- No dependency on hypervisor networking for routing

### Network Participants
- SOC host (Elastic + Fleet Server)
- Windows endpoint VM
- Kali Linux attacker

All three systems are connected using Tailscale, forming a private mesh
network.

---

## Why Tailscale Was Chosen

Tailscale was selected for several reasons:

- Stable connectivity independent of NAT or bridged networking
- Encrypted communication by default
- Simple peer-to-peer connectivity across physical machines
- Minimal configuration overhead

In contrast, hypervisor-based NAT and bridged modes proved unreliable and
fragile, especially when combined with:
- Windows 11
- Hyper-V / virtualization-based security
- Security software and VPN adapters

---

## Communication Flow

- Endpoint → SOC
  - Elastic Agent sends telemetry to Fleet Server and Elasticsearch
- SOC → Endpoint
  - Management traffic (policy updates, agent management)
- Attacker → Endpoint
  - Attack traffic originating from Kali Linux
- Attacker → SOC
  - No direct access (by design)

This ensures a clear trust boundary between attacker activity and
defender infrastructure.

---

## Security Boundaries

- The endpoint has no unrestricted internet access
- Only explicit services and VPN traffic are allowed
- SOC components are reachable only through the VPN overlay
- No services are exposed to the home LAN

Blocking unnecessary connectivity improves realism and reduces noise,
while making attack paths easier to reason about.

---

## Design Trade-offs

- Using a VPN overlay introduces slight latency
- Some protocols (e.g., ICMP) may be blocked by default
- Direct layer-2 visibility is not available

These trade-offs are acceptable, as detection engineering focuses on
application- and host-level telemetry rather than raw packet capture.

---

## Summary

The network design favors predictability and realism over convenience.
By avoiding fragile virtual networking and relying on an encrypted
overlay, the lab remains stable and easier to reason about as attack
complexity increases in later weeks.

