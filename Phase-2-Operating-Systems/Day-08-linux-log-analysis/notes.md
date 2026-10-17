# 🐧Day 8: Linux Log Analysis & Monitoring

## 🎯 Overview
Mapped the Linux logging architecture — multiple distributed sources vs
Windows' centralised event log — identified the core log files and what
each contains, built a Linux IR investigation workflow, and verified real
log evidence on a live Ubuntu VM. Connected every persistence mechanism
from Day 6 to its corresponding detection log.

---

## 🏗️ Linux Logging Architecture

Unlike Windows, Linux has no single centralised log service. Multiple
sources write to different locations:

| Source | What it logs |
|---|---|
| **syslog/rsyslog** | General system and service events |
| **journald** | systemd service logs, unified journal |
| **PAM** | Authentication events → auth.log |
| **auditd** | Kernel-level syscall/file access (separate from syslog) |
| **Application logs** | Web servers, databases, etc. write independently |

> 💡 **SOC implication:** you must know which source to check for which
> event type — there is no single place that has everything.

---

## 📚 Core Log Files Reference

### 🔐 `/var/log/auth.log` (Debian/Ubuntu) — `/var/log/secure` (RHEL)
Primary authentication log:
- SSH login attempts (success and failure) with source IP
- `sudo` invocations — who ran what command as root
- PAM authentication events — local logons, `su` usage
- New user creation via `adduser`/`useradd`
- Failed password attempts

### 🗒️ `/var/log/syslog` (Debian/Ubuntu) — `/var/log/messages` (RHEL)
General system events:
- Cron job execution (every time a scheduled task fires)
- Service starts and stops
- Kernel and network messages
- systemd service state changes

### 🛡️ `/var/log/audit/audit.log`
auditd output — entirely separate from syslog:
- File reads/writes to watched files
- Syscall-level activity
- Process execution and network connections
- Whatever audit rules specify (e.g. shadow_read key from Day 5)

### 👤 `/var/log/wtmp` and `/var/log/btmp` — binary files
- `wtmp` → all successful logins, logouts, reboots → read with `last`
- `btmp` → all failed login attempts → read with `lastb`

### 📝 `/root/.bash_history` and `~/.bash_history`
- Not a formal log — can be cleared or disabled by attacker
- **Absence is itself a forensic indicator** — raises the question
  of what activity cannot be reconstructed from command history
- Cleared history is a lead, not proof — correlate with auth.log,
  syslog, audit records, and file timestamps
- An attacker who cleared history may have: created privileged accounts,
  read /etc/shadow, installed persistence, downloaded malicious scripts,
  or deleted logs

---

## 🔑 Reading SSH Events in auth.log

```bash
# Successful key-based auth
sshd: Accepted publickey for alice from 192.168.1.5 port 52341

# Successful password auth
sshd: Accepted password for alice from 192.168.1.5 port 52342

# Failed attempt
sshd: Failed password for root from 10.0.0.99 port 44123

# Invalid username
sshd: Invalid user admin from 10.0.0.99 port 44124
```

**🚨 SOC alert patterns:**
- Multiple `Failed password` from same IP in short time = brute force
- `Invalid user` cycling through common names = credential stuffing
- `Accepted publickey` from unexpected IP = potential backdoor key
- `Accepted password` for root when PermitRootLogin should be off =
  misconfiguration or compromise

---

## ⚡ Reading sudo Events in auth.log
