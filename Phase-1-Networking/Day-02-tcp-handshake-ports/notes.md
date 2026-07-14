# 🌐 Day 2: TCP/IP Deep Dive — The 3-Way Handshake & Port Numbers

## 📖 Overview
This session moved from OSI theory into the mechanics of how TCP actually
establishes a connection before any data transfer occurs, along with how
port numbers direct traffic to the correct application on a device.

## 🧠 Core Concepts

### 🤝 The TCP 3-Way Handshake
Every TCP connection begins with a fixed three-step exchange before any
application data is sent:
1. **SYN** — the client sends a synchronize packet to initiate the
   connection.
2. **SYN-ACK** — the server acknowledges the request and responds with its
   own synchronize flag.
3. **ACK** — the client confirms, completing the handshake.

Only after this sequence completes does actual data — such as a TLS
handshake or HTTP request — begin flowing.

### 🚨 SYN Flood Attacks
A SYN flood is a Denial-of-Service technique that exploits the handshake's
design: an attacker sends a high volume of SYN packets but never completes
the final ACK. The server allocates and holds resources for each half-open
connection while waiting for a response that never arrives, eventually
exhausting available connection capacity and preventing legitimate clients
from connecting.

### 🔢 Port Numbers
An IP address routes data to a device; a port number routes it to the
correct application on that device. Key ports relevant to SOC work include
22 (SSH), 53 (DNS), 80/443 (HTTP/HTTPS), 445 (SMB), and 3389 (RDP) — the
latter two being frequent targets for exploitation and brute-force attacks
due to their common exposure on internet-facing systems.

## 🛠️ Practical Exercises

### 1. 📡 Capturing the SYN and SYN-ACK Packets
Using Wireshark with the filter `tcp.flags.syn==1`, isolated the initial
handshake packets for an HTTPS connection. Identified the client's SYN
packet (ephemeral source port to destination port 443) and the server's
corresponding SYN-ACK response, confirming the reversed source/destination
relationship between request and reply.

<details>
<summary>📷 Screenshot: SYN / SYN-ACK Handshake Capture</summary>

![TCP SYN and SYN-ACK Packets](screenshots/wireshark-tcp-syn-synack-handshake.png)

</details>

### 2. ✅ Locating the Final ACK
Identified the closing ACK packet completing the handshake for the same
connection, confirming via matching source/destination ports and
sequence/acknowledgment numbers that it represented the third and final
step. Observed that the TLS Client Hello (including a visible SNI field
identifying the destination domain in plaintext) began immediately
afterward — a useful reminder that even encrypted HTTPS sessions expose the
destination hostname prior to encryption being established.

<details>
<summary>📷 Screenshot: TCP Handshake — Final ACK Completion</summary>

![TCP Handshake ACK Completion](screenshots/wireshark-tcp-handshake-ack-completion.png)

</details>

### 3. 💻 Reviewing Active Connections (`netstat -ano`)
Ran `netstat -ano` to list active TCP and UDP connections on the local
machine. Reviewed established connections, confirming remote port 443
(HTTPS) usage, and noted a connection in the `CLOSE_WAIT` state — indicating
the remote endpoint had closed the connection while the local socket had
not yet fully released it, a normal but state-specific condition worth
recognizing beyond just `ESTABLISHED`.

<details>
<summary>📷 Screenshot: netstat Active Connections</summary>

![netstat -ano Output](screenshots/netstat-established-connections.png)

</details>

## 🔍 Research: RDP Brute-Force Attacks
Port 3389 (RDP) is a frequent target for brute-force attacks when exposed
directly to the internet, as attackers can repeatedly attempt automated
username/password combinations until valid credentials are found. In logs,
this typically appears as a high volume of failed authentication attempts
from one or multiple source IPs, often followed by account lockouts or,
in a successful compromise, a login following a sustained failure pattern.

## 🛡️ SOC Analyst Relevance
- Recognizing an abnormal volume of SYN packets without corresponding
  completed handshakes is a direct indicator of a SYN flood in progress.
- Port numbers provide immediate triage context — unexpected external
  traffic on ports like 3389 or 445 is a high-priority red flag regardless
  of other packet content.
- Connection state (not just "established") matters during investigation;
  states like `CLOSE_WAIT` or an unusually high number of half-open
  connections can indicate abnormal behavior.

## 🎯 Key Takeaways
- The 3-way handshake (SYN, SYN-ACK, ACK) must complete before any
  application-layer data transfer begins.
- SYN floods work specifically because servers must reserve resources
  while waiting for handshake completion.
- TLS-encrypted traffic still exposes the destination domain via SNI before
  encryption is established.
- Port number recognition is one of the fastest available triage signals
  in log and packet analysis.
