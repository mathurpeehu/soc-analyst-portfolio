# 📅 Day 1 — The OSI Model

## 🎯 Objective

Learn the OSI (Open Systems Interconnection) Model and understand how data travels through each of its seven layers. Gain practical experience by analyzing real network traffic using Wireshark.

---

# 📚 Concepts Learned

The OSI Model is a conceptual framework that divides network communication into seven layers. Each layer has a specific responsibility in transmitting data across a network.

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

- The OSI Model consists of seven layers, each with a specific responsibility.
- A single network request passes through multiple OSI layers before reaching its destination.
- Layer 2 uses MAC addresses, while Layer 3 uses IP addresses.
- Layer 4 is responsible for communication using TCP or UDP.
- Layer 7 contains application protocols such as HTTP, HTTPS, and DNS.
- Wireshark helps visualize real network traffic and identify protocols at different OSI layers.

---

# 🔬 Practical Implementation

## Practical 1 – DNS Packet Analysis (Layer 7)

Captured live network traffic in Wireshark while browsing a website and applied the following display filter:

```text
dns
```

This displayed DNS query and response packets used to resolve the website's domain name.

### What I Observed

- Protocol: DNS
- OSI Layer: Application (Layer 7)
- DNS queries and responses were visible during the website request.

<details>
<summary>📷 DNS Packet Analysis Screenshot</summary>

![DNS Packet Analysis](media/dns-packet-analysis.png)

</details>

---

## Practical 2 – Ethernet II Frame Analysis (Layer 2)

Expanded a captured DNS packet and inspected the **Ethernet II** header to examine Layer 2 information.

### What I Observed

- Source MAC Address
- Destination MAC Address
- Ethernet II Header

This demonstrated that Layer 2 communication relies on MAC addresses for local network delivery.

<details>
<summary>📷 Ethernet II Frame Screenshot</summary>

![Ethernet II Frame](media/ethernet-ii-frame.png)

</details>

---

# 🌐 Research

## TCP/IP Model

The TCP/IP Model is the networking model used on the Internet today. It simplifies the OSI Model into four layers.

| TCP/IP Layer | Corresponding OSI Layer(s) |
|---------------|----------------------------|
| Application | Application, Presentation, Session |
| Transport | Transport |
| Internet | Network |
| Network Access | Data Link, Physical |

### Why TCP/IP is Used

- Simpler than the OSI Model.
- Designed for real-world Internet communication.
- Used by modern operating systems and networking devices.
- Easier to implement in practical environments.

---

# 🛠 Tools Used

| Tool | Purpose |
|------|---------|
| Wireshark | Capture and analyze network traffic |

---

# 💡 Skills Gained

- Understood the purpose of each OSI layer.
- Learned the difference between MAC and IP addressing.
- Captured and analyzed DNS traffic using Wireshark.
- Examined Ethernet II frames and identified MAC addresses.
- Related real network packets to their corresponding OSI layers.

---

# 📝 Revision Notes

- OSI consists of **7 layers**.
- Layer 7 includes HTTP, HTTPS, DNS, and SSH.
- Layer 4 uses TCP and UDP.
- Layer 3 uses IP addresses.
- Layer 2 uses MAC addresses.
- Ethernet II belongs to Layer 2.
- DNS traffic can be filtered using `dns` in Wireshark.
- TCP/IP is the networking model used in real-world Internet communication.

---

# 📈 Today's Progress

- Learned the complete OSI Model.
- Understood the function of each OSI layer.
- Captured and analyzed DNS traffic using Wireshark.
- Inspected Ethernet II frames and identified MAC addresses.
- Researched the TCP/IP Model and its relationship with the OSI Model.

---

# 🎯 Learning Outcome

Today's learning strengthened my understanding of the OSI Model by connecting theory with hands-on packet analysis. Using Wireshark, I identified Layer 7 DNS traffic and examined Layer 2 Ethernet II frames, helping me understand how network communication is structured in real-world environments.
