# 📡 Day 9: Wireless Networking Fundamentals — WPA2/WPA3 & Attacks

## 🎯 Overview
This session covered wireless-specific security concepts — Wi-Fi security
protocol evolution, the WPA2 4-way handshake vulnerability, WPA3's design
improvements, and common wireless attacks, connecting several trust-based
weakness patterns from earlier sessions into the wireless/radio-frequency
context.

## 📚 Core Concepts

### 🔐 Wi-Fi Security Protocol Evolution
| Protocol | Status | Key Weakness |
|---|---|---|
| WEP | Broken | Static, easily crackable key |
| WPA | Deprecated | Known vulnerabilities |
| WPA2 | Widely used | AES-based, but handshake enables offline attacks |
| WPA3 | Current standard | SAE resists offline dictionary attacks |

### 🤝 WPA2 4-Way Handshake & Its Weakness
WPA2 derives a session encryption key from the network password via a
4-way handshake. If captured, this handshake can be taken **offline** and
tested against unlimited password guesses without any further interaction
with the network — the exact weakness WPA3 was designed to eliminate.

### 🛡️ WPA3's SAE (Simultaneous Authentication of Equals)
Unlike WPA2, SAE requires a **new, real interaction with the access point
for every password guess** — turning what was an unlimited offline attack
into a slow, network-visible, rate-limitable online attack.

### ⚠️ Common Wireless Attacks
- **Evil Twin** — a fake access point impersonates a legitimate SSID,
  tricking victims into connecting and exposing their traffic to
  interception.
- **Deauthentication Attack** — forged, unauthenticated 802.11 management
  frames force victims to disconnect, without the attacker ever needing the
  network password.
- **WPS PIN Brute-Force** — exploits weak WPS PIN implementations to bypass
  the actual Wi-Fi password.
- **Rogue Access Points** — unauthorized APs (accidental or malicious)
  bypassing an organization's intended security controls.

### 🔗 Shared Root Cause with ARP Spoofing (Day 7)
Evil Twin attacks and ARP spoofing share the same underlying weakness:
victims trust an identity claim (an SSID broadcast, or an ARP reply)
because the underlying protocol provides no strong authenticity
verification — the same "implicit trust without verification" pattern
identified across multiple protocols this week.

## 🔬 Practical Exercises

### 1️⃣ Local Wi-Fi Security Configuration
Ran `netsh wlan show interfaces` to inspect the current wireless
connection, confirming **WPA2-Personal** authentication with **CCMP
(AES)** encryption, operating on the 5 GHz band via 802.11ax (Wi-Fi 6),
with strong signal quality (88%, RSSI -48).

<details>
<summary>📷 Screenshot: netsh wlan show interfaces</summary>

![WLAN Interface Details](./netsh-wlan-show-interfaces.png)

</details>

### 2️⃣ Nearby Network Reconnaissance
Ran `netsh wlan show networks mode=bssid` to enumerate 11 visible nearby
networks, reviewing SSID, BSSID, signal strength, authentication method,
encryption type, and channel for each — practical reconnaissance data
useful for assessing wireless coverage and spotting anomalies such as
duplicate SSIDs (a potential Evil Twin indicator).

<details>
<summary>📷 Screenshot: netsh wlan show networks mode=bssid</summary>

![Nearby Wireless Networks](./netsh-wlan-show-networks-bssid.png)

</details>

## 🌐 Research: Aircrack-ng
**Aircrack-ng** is a well-known open-source wireless security testing
suite capable of capturing WPA/WPA2 handshakes, analyzing wireless
traffic, and testing password strength against captured handshakes.
Widely used for legitimate, authorized security audits and training, but
also historically misused in unauthorized real-world attacks — a dual-use
tool reflecting the broader reality of security tooling.

## 🧑‍💻 SOC Analyst Relevance
- 📶 Wireless compromises often occur below the visibility of standard
  network logs, since the attack happens at the radio-frequency layer
  before traffic reaches monitored infrastructure.
- 🚨 Wireless Intrusion Detection Systems (WIDS) exist specifically to
  detect rogue APs, deauth flood patterns, and Evil Twin SSIDs.
- 🧠 "The network looked fine in our logs" does not rule out a
  wireless-layer compromise — an important reasoning gap to avoid.

## 💡 Key Takeaways
- WPA3's core improvement is forcing password guesses online and
  detectable, not just "stronger encryption."
- Evil Twin, ARP spoofing, and IP spoofing all share one root cause:
  protocols trusting identity claims without verification.
- Deauthentication attacks require no network credentials at all — only
  the ability to forge unauthenticated management frames.
- Wireless reconnaissance data (BSSID, signal, security type) is a
  practical tool for spotting anomalies like duplicate SSIDs.
