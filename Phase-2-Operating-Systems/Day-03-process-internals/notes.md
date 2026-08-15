# Phase 2 — Day 3: Windows Process Internals & Malicious Process Analysis

## Overview
Mapped the legitimate Windows process tree, established normal parent-child 
relationships as a detection baseline, and analysed three core techniques 
attackers use to hide malicious code inside legitimate processes. Verified 
real process paths and command-line flags on a live Windows system.

---

## The Windows Process Tree

Every process has a parent. At boot, a strict hierarchy forms — deviations 
from this hierarchy are a primary detection signal:

```
System (PID 4)
└── smss.exe                    ← Session Manager
    └── wininit.exe             ← Initialises kernel-mode services
    │   ├── services.exe        ← Service Control Manager
    │   │   ├── svchost.exe     ← Hosts grouped Windows services (multiple instances)
    │   │   └── lsass.exe       ← Live credential material (Day 2)
    │   └── lsm.exe             ← Local Session Manager
    └── winlogon.exe            ← Interactive logon UI
        └── userinit.exe → explorer.exe  ← User desktop
```

**First question on any process:** does this parent-child relationship make sense?

---

## Key Legitimate Processes & Detection Rules

| Process | Expected Parent | Instance Count | Alert If |
|---|---|---|---|
| `lsass.exe` | `wininit.exe` | Exactly 1 | 2nd instance, wrong parent, wrong path |
| `svchost.exe` | `services.exe` | Many | No `-k` flag, wrong parent, wrong path |
| `services.exe` | `wininit.exe` | Exactly 1 | Any 2nd instance |
| `explorer.exe` | `userinit.exe` (exits after) | 1 per user | Spawning `cmd.exe`/`powershell.exe` unprompted |

---

## Three Core Malicious Process Techniques

### 1. Process Masquerading
Malware names itself `lsass.exe`, `svchost.exe`, or `explorer.exe` to blend 
in with legitimate processes.

**Detection:** check the full image path and parent PID — a real `svchost.exe` 
lives in `C:\Windows\System32\` with `services.exe` as parent. A fake almost 
always fails one or both checks.

> Connected to Phase 0 Day 1: Win32/Sality used the same name-masquerading 
> concept — now extended with parent-child context as an additional detection layer.

### 2. Process Injection
Instead of running a new malicious executable, malware injects code into an 
already-running legitimate process (`explorer.exe`, `svchost.exe`, a browser).

**What the attacker gains:**
- **Camouflage** — malicious activity appears under a legitimate process name
- **Inherited trust** — injected code runs in the target's security context
- **No new executable** — nothing suspicious to find on disk or in the process list
- **Potential privilege advantage** — inherits the target process's access level

**Common injection methods:**
- **DLL injection** — loads a malicious DLL into a target process
- **Process hollowing** — spawns a legitimate process suspended, replaces its 
  memory with malicious code, resumes it
- **Thread injection** — creates a new thread inside a running process pointing 
  to malicious shellcode

### 3. Unusual Spawning Chains
Legitimate software has predictable spawn patterns. Attackers break them.

**Red-flag parent-child chains:**
- `winword.exe` → `cmd.exe` (Office spawning shell — classic macro attack)
- `explorer.exe` → `powershell.exe` → `net.exe` (lateral movement sequence)
- `svchost.exe` → anything interactive (services don't spawn user shells)
- Any browser process → `cmd.exe` or `powershell.exe`

---

## Process Hollowing vs DLL Injection

| Technique | What Happens |
|---|---|
| **DLL Injection** | A malicious DLL/module is *added* into a running legitimate process |
| **Process Hollowing** | A legitimate process is spawned suspended; its memory *replaced* with malicious content |

Key distinction: DLL injection introduces something new; hollowing *replaces* 
what was already there — making the process appear externally legitimate while 
running entirely malicious code internally.

---

## SOC Relevance

**Event ID 4688** (process creation, requires command-line logging enabled):
- `NewProcessName` — full image path (System32? or Temp/AppData/Downloads?)
- `ParentProcessName` — does this parent-child pair make sense?
- `CommandLine` — for `svchost.exe`, is `-k` present? For PowerShell, 
  is `-EncodedCommand` used? (major red flag)

**Sysmon adds what standard Event Viewer misses:**
- **Sysmon Event ID 8 — CreateRemoteThread:** records when one process creates 
  a thread inside another — a primary injection detection signal
- **Sysmon Event ID 10 — ProcessAccess:** records when one process opens 
  another — detects suspicious memory access including credential theft attempts

---

## Practical Evidence

<details>
<summary>🪟 lsass.exe — Path & Properties Verified</summary>

![lsass.exe Properties](screenshots/lsass-exe-properties-system32.png)

- **Location:** `C:\Windows\System32` ✅
- **Description:** Credential Guard & VBS Key Isolation
- Confirms Credential Guard is active — credential material is isolated inside 
  a hardware-backed VBS enclave, an additional protection layer beyond PPL
</details>

<details>
<summary>🪟 svchost.exe — Path & Properties Verified</summary>

![svchost.exe Properties](screenshots/svchost-exe-properties-system32.png)

- **Location:** `C:\Windows\System32` ✅
- **Description:** Host Process for Windows Services
- Confirmed legitimate — any instance outside System32 with this name would be 
  an immediate masquerading alert
</details>

<details>
<summary>💻 svchost.exe — Command-Line Flags via PowerShell</summary>

![svchost CommandLine](screenshots/svchost-commandline-powershell.png)

Command run:
```powershell
Get-WmiObject Win32_Process -Filter "name='svchost.exe'" | 
Select-Object ProcessId, CommandLine | Format-List
```

Real output confirms `-k` flag on multiple instances:
- PID 13376: `C:\WINDOWS\system32\svchost.exe -k LocalService -p -s NPSMSvc`
- PID 20716: `C:\WINDOWS\System32\svchost.exe -k UnistackSvcGroup`

> Note: blank `CommandLine` fields on other instances are expected — WMI cannot 
> retrieve command lines for certain protected system processes. A blank command 
> line combined with a non-System32 path would be the real red flag, not a blank 
> line alone.
</details>

---

## Key Takeaways
- The Windows process tree has a strict expected hierarchy — deviations are 
  the primary detection signal, not individual process names alone
- Process injection is harder to detect than running a new executable because 
  it hides behind a legitimate process's identity, trust level, and security context
- Process hollowing is particularly deceptive — the process *looks* legitimate 
  from the outside while running entirely malicious code internally
- Sysmon fills the telemetry gap standard Windows logging leaves — especially 
  for injection (Event ID 8) and memory access (Event ID 10)
- Credential Guard (confirmed active on this machine) adds VBS-backed isolation 
  on top of PPL — relevant to modern LSASS evasion techniques in later phases
