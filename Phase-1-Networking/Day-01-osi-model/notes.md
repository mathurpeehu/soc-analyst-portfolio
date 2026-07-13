# 📅 Day 1 — The OSI Model

## 🎯 Objective

Learn the OSI (Open Systems Interconnection) Model and understand how data travels through each of its seven layers. Gain practical experience by capturing and analyzing real network traffic using Wireshark.

---

# 📚 Concepts Learned

The OSI Model is a conceptual framework that divides network communication into seven layers. Each layer has a specific responsibility and works together to ensure reliable data transmission.

## Layer 7 – Application

Provides network services directly to end-user applications.

**Examples:** HTTP, HTTPS, DNS, SSH

---

## Layer 6 – Presentation

Handles data formatting, encryption, and compression before transmission.

**Examples:** TLS, SSL, JPEG

---

## Layer 5 – Session

Establishes, manages, and terminates communication sessions between devices.

**Example:** NetBIOS

---

## Layer 4 – Transport

Ensures reliable or fast delivery of data using transport protocols.

**Protocols:** TCP, UDP

---

## Layer 3 – Network

Responsible for logical addressing and routing packets between different networks.

**Examples:** IPv4, IPv6, ICMP

---

## Layer 2 – Data Link

Uses MAC addresses to deliver data within the local network.

**Examples:** Ethernet II, MAC Address

---

## Layer 1 – Physical

Transmits raw bits over the physical medium.

**Examples:** Ethernet Cable, Fiber Optic Cable, Wi-Fi Signals

---

# 🧠 Key Takeaways

- The OSI Model consists of seven layers, each with a specific role.
- A single network request passes through multiple OSI layers before reaching its destination.
- Layer 2 uses MAC addresses, while Layer 3 uses IP addresses.
- Layer 4 is responsible for communication using TCP or UDP.
- Layer 7 contains application protocols such as HTTP, HTTPS, and DNS.
- Wireshark helps visualize how these layers work in real network traffic.

---

# 🔬 Practical Implementation

## 1. Installed Wireshark

Installed Wireshark on Windows to analyze live network traffic.

**What I Learned**

- Wireshark captures packets travelling through the network.
- It is one of the most widely used packet analysis tools in cybersecurity.

---

## 2. Captured Live Network Traffic

Started a packet capture on my Wi-Fi adapter and opened a website in the browser.

**What I Learned**

- Every network action generates packets.
- Wireshark captures packets in real time.

---

## 3. Filtered DNS Traffic

Applied the following display filter:

```text
dns
```

Observed DNS queries and responses generated while loading the website.

**What I Learned**

- DNS converts domain names into IP addresses.
- Multiple DNS requests may appear for a single website.

---

## 4. Inspected an Ethernet II Frame

Expanded the packet details and examined the Ethernet II header.

Observed:

- Source MAC Address
- Destination MAC Address

**What I Learned**

- Ethernet II belongs to Layer 2.
- MAC addresses identify devices within the local network.

---

## 5. Observed QUIC Traffic

After the DNS exchange, Wireshark displayed QUIC packets.

**What I Learned**

- QUIC is a modern transport protocol built on UDP.
- Many websites, including Google services, use QUIC to improve speed and reduce latency.

---

# 🌐 Research

## TCP/IP Model

The TCP/IP Model is the networking model used on the Internet today.

| TCP/IP Layer | Corresponding OSI Layer(s) |
|--------------|----------------------------|
| Application | Application, Presentation, Session |
| Transport | Transport |
| Internet | Network |
| Network Access | Data Link, Physical |

### Why TCP/IP is Used

- Simpler than the OSI Model.
- Designed around real Internet communication.
- Used by modern operating systems and networking devices.
- Easier to implement in real-world environments.

---

# 🛠 Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Capture and analyze network packets |

---

# 💡 Skills Gained

- Understood the seven layers of the OSI Model.
- Learned the difference between MAC and IP addressing.
- Captured live network traffic using Wireshark.
- Applied display filters to isolate DNS traffic.
- Examined Ethernet II frames and MAC addresses.
- Observed modern QUIC traffic during packet analysis.

---

# 📷 Practical Screenshots

### Wireshark Installation

![Wireshark Installation](media/wireshark-installation.png)

---

### Packet Capture Overview

![Packet Capture Overview](media/packet-capture-overview.png)

---

### DNS Packet Analysis

![DNS Packet Analysis](media/dns-packet-analysis.png)

---

### Ethernet II Frame

![Ethernet II Frame](media/ethernet-ii-frame.png)

---

### MAC Address Inspection

![MAC Address Inspection](media/mac-address-inspection.png)

---

### QUIC Traffic

![QUIC Traffic](media/quic-traffic.png)

---

# 📝 Revision Notes

- OSI has **7 layers**.
- Layer 7 handles user-facing protocols like HTTP and DNS.
- Layer 4 uses TCP and UDP.
- Layer 3 uses IP addresses.
- Layer 2 uses MAC addresses.
- Ethernet II is a Layer 2 protocol.
- DNS traffic can be filtered using `dns` in Wireshark.
- TCP/IP is the networking model used in real-world Internet communication.

---

# 📈 Today's Progress

- Learned the complete OSI Model.
- Understood the role of each OSI layer.
- Installed and configured Wireshark.
- Captured live network traffic.
- Filtered and analyzed DNS packets.
- Examined Ethernet II frames and MAC addresses.
- Researched the TCP/IP Model and its relationship with the OSI Model.

---

# 🎯 Learning Outcome

Today's learning helped me understand how network communication is organized using the OSI Model. By combining theory with hands-on packet analysis in Wireshark, I gained practical experience identifying Layer 2 and Layer 7 traffic, understanding MAC addressing, and relating network activity to real-world cybersecurity investigations.
