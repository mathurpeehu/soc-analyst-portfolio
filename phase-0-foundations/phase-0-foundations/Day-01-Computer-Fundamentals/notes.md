# Day 1 — Computer Fundamentals (CPU, RAM, Storage, I/O)

## What I Learned
- Difference between RAM (volatile memory) and Storage (non-volatile memory)
- Difference between a program (on disk) and a process (running instance with a PID)
- Why everything reduces to binary at the hardware/transistor level

## Practical Exercise
Investigated running processes on my machine using Windows Task Manager
(Processes tab and Details tab).

- Total processes observed: ~127
- Identified 3 unfamiliar processes and researched their purpose:
  - GameInputSvc.exe — Windows service for game controller input
  - gamingservices.exe — Microsoft service for Xbox/Microsoft Store games
  - LockApp.exe — Displays the Windows lock screen

## Key Finding
`LockApp.exe` showed 0 K memory usage because its Status was "Suspended" —
Windows trims memory for processes not actively in use. This taught me that
memory usage alone doesn't tell the full story; process **status** matters too.

## Research: Malware Masquerading
Looked into Win32/Sality — a polymorphic file-infecting virus. Learned the
distinction between two evasion techniques:
- **Name masquerading** (e.g., fake svch0st.exe mimicking svchost.exe)
- **Process injection** (malicious code hidden inside a legitimate process,
  which is what Sality actually does)

## Screenshots
(will add redacted Task Manager screenshots here)
