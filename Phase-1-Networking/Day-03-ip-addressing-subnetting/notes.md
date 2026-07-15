# 🌐 Phase 1 — Day 3: IP Addressing & Subnetting

## 📖 Overview

This session covered IPv4 address structure, subnet masks, CIDR notation,
and manual subnetting calculations — a foundational skill for interpreting
network segmentation during log analysis and incident investigation.

## 🧠 Core Concepts

### 🌍 IPv4 Structure and Subnet Masks

Every IPv4 address consists of a network portion and a host portion, with
the subnet mask defining where that split occurs. In binary, a mask is a
run of 1s (network bits) followed by 0s (host bits) — e.g., `255.255.255.0`
represents 24 network bits and 8 host bits.

### 📌 CIDR Notation

CIDR notation (e.g., `/24`, `/28`) is shorthand for the number of network
bits in the mask, used throughout real-world network documentation and log
data in place of writing the full subnet mask.

### 🧮 Calculating Subnet Size

For any given CIDR value:

- 🔹 Host bits = 32 − CIDR
- 🔹 Total addresses = 2^(host bits)
- 🔹 Usable host addresses = Total − 2 (reserving the network address and
  broadcast address, which cannot be assigned to individual hosts)

### ✍️ Manual Subnetting Method

Practiced a repeatable 5-step process: determine host bits, calculate total
and usable addresses, derive the subnet mask and block size, then round the
given IP down to the nearest block boundary to find the network address,
with the broadcast address falling at the top of that block.

## 💻 Practical Exercises

### 📝 Manual Subnet Calculations

Worked through multiple subnetting problems by hand, including full
breakdowns (network address, usable host range, broadcast address) for a
`/28` subnet, and usable host counts for `/25` and `/22` subnets — building
consistency in applying the same method regardless of subnet size.

### 🖥️ Local Network Analysis (`ipconfig /all`)

Ran `ipconfig /all` to retrieve real subnet configuration, identified the
local subnet mask (`255.255.255.0`), converted it to CIDR notation (`/24`),
and calculated the usable host capacity (254 addresses) for the local
network segment.

## 🔍 Research: RFC 1918 Private IP Ranges

Identified the three official private IPv4 ranges defined by RFC 1918:

- 🟢 `10.0.0.0/8` (10.0.0.0 – 10.255.255.255)
- 🟢 `172.16.0.0/12` (172.16.0.0 – 172.31.255.255)
- 🟢 `192.168.0.0/16` (192.168.0.0 – 192.168.255.255)

Confirmed that the local network's address falls within the
`192.168.0.0/16` range, consistent with typical home/small-office network
configurations.

## 🛡️ SOC Analyst Relevance

Subnet awareness is essential for correctly interpreting internal network
logs — recognizing which subnet an IP address belongs to helps distinguish
normal traffic within a network segment from traffic crossing between
segments, which often warrants closer scrutiny. Subnetting knowledge is
also a commonly tested skill in SOC technical interviews as a baseline
check of genuine networking understanding.

## ✅ Key Takeaways

- 📌 CIDR notation is the standard shorthand used across real-world network
  documentation and logs.
- 📌 The network and broadcast addresses in any subnet are always reserved,
  regardless of subnet size.
- 📌 Manual subnetting follows a consistent, repeatable method rather than
  requiring memorization of every possible subnet size.
- 📌 Private IP ranges (RFC 1918) are non-routable on the public internet and
  form the basis of nearly all internal network addressing.
