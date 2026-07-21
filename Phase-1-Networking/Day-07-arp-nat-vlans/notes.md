# 🔀 Phase 1 — Day 7: Routing & Switching Fundamentals — ARP, NAT & VLANs

## 🎯 Overview
This session bridged Layer 2 and Layer 3 concepts through ARP, examined how
NAT allows private networks to share a single public IP, and explored VLAN
segmentation as a foundational network security control — connecting
several trust-based vulnerability patterns back to earlier lessons.

## 📚 Core Concepts

### 🔗 ARP (Address Resolution Protocol)
ARP maps a known IP address to its corresponding MAC address, enabling
Ethernet frame delivery on the local network:
1. **ARP Request** — broadcast: *"Who has this IP?"*
2. **ARP Reply** — the owning device responds with its MAC address
3. The mapping is cached locally for future use

ARP operates at **Layer 2**, since its entire purpose is enabling local
frame delivery, which IP addressing (Layer 3) alone cannot do.

### ⚠️ ARP Spoofing
ARP has **no built-in authentication** — any device can send a fake ARP
reply, and others will trust and cache it without verification. This is the
same root weakness behind UDP-based IP spoofing (Day 5): protocols that
trust received information without identity verification enable attackers
to impersonate legitimate devices, commonly leading to Man-in-the-Middle
(MITM) attacks.

### 🌐 NAT (Network Address Translation)
Originally designed to conserve IPv4 addresses by allowing many private
devices to share one public IP, NAT provides an incidental security
benefit: internal devices aren't directly reachable from the internet
unless explicitly port-forwarded, since they're never directly exposed.

### 🧩 VLANs (Virtual LANs)
VLANs logically segment devices into separate broadcast domains even when
physically connected to the same switch — isolating traffic by function
(e.g., department, device type) without requiring separate physical
infrastructure.

## 🔬 Practical Exercises

### 1️⃣ Local ARP Cache Analysis
Ran `arp -a` to inspect the local ARP cache, identifying the default
gateway (`192.168.1.1`) and its corresponding MAC address, confirmed as a
**dynamic** entry (learned automatically via ARP rather than manually
configured). Also noted multicast address entries (`224.0.0.x`, mapped to
the reserved `01-00-5e` MAC prefix) — a reminder that not every ARP table
entry represents an actual physical device.

<details>
<summary>📷 Screenshot: ARP Cache — Default Gateway Mapping</summary>

![ARP Cache Table](screenshots/arp-cache-default-gateway.png)

</details>

### 2️⃣ Real-World VLAN Architecture Research
Located and analyzed a real enterprise network diagram showing
department-based VLAN segmentation (HR, Finance, Sales, Engineering, QA,
R&D, etc.) across shared physical switch infrastructure. Also identified a
separate **DMZ zone** hosting public-facing/shared services (mail, web,
database servers) isolated behind the firewall from internal departmental
VLANs — demonstrating segmentation applied at both the VLAN level and the
broader network architecture level.

<details>
<summary>📷 Screenshot: Enterprise VLAN Network Diagram</summary>

![Enterprise VLAN Diagram](screenshots/enterprise-vlan-network-diagram.png)

</details>

## 🌐 Research: ARP Spoofing Tools
**Ettercap** is a well-known tool used to perform ARP spoofing and MITM
attacks — it sends forged ARP replies to associate the attacker's MAC
address with a legitimate device's IP (commonly the gateway), allowing
interception of victim traffic. This is especially dangerous on unsecured
shared networks, such as open public Wi-Fi, where devices share the same
broadcast domain.

## 🧑‍💻 SOC Analyst Relevance
- 🚨 An IP suddenly associated with a *different* MAC address in logs is a
  genuine ARP spoofing red flag worth investigating.
- 🛡️ VLAN segmentation is a core defense-in-depth control — proper
  segmentation limits how far a compromise (e.g., on a guest network) can
  spread laterally.
- 🕵️ NAT complicates investigation — external logs often show only an
  organization's single public IP, requiring internal firewall/proxy logs
  to identify the specific responsible device.

## 💡 Key Takeaways
- ARP and IP spoofing share the same underlying weakness: protocols
  trusting unauthenticated information.
- NAT's security benefit is incidental to its original IPv4-conservation
  purpose.
- VLANs and DMZ zones both apply the same core principle — isolating
  systems by function or trust level — at different architectural scales.
- Real enterprise networks apply segmentation far more granularly than
  conceptual examples suggest (department-level VLANs, dedicated DMZ
  zones).
