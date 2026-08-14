---
type: lab-note
status: active
tags:
  - windows
  - vm
  - environment-setup
---

# Windows 11 Lab

> **Purpose**
>
> This Windows virtual machine serves as the primary Windows laboratory for the SOC Analyst roadmap.
>
> Its primary purpose is to generate Windows logs, perform log analysis, and provide a safe environment for practicing Windows-specific cybersecurity concepts such as Active Directory, SIEM integration, system monitoring, and incident response.

---

# System Identity

| Property | Value |
|----------|-------|
| Computer Name | WIN-LAB |
| Virtual Machine Name | Windows-11-Lab |
| Operating System | Windows 11 Home |
| Version | 25H2 |

---

# User Configuration

| Property | Value |
|----------|-------|
| Username | WIN-User |
| Account Type | Local Administrator |

## Local Account Strategy

A local administrator account was intentionally chosen instead of signing in with a Microsoft account.

This virtual machine is dedicated exclusively to cybersecurity labs and experimentation. Using a local account prevents synchronization with personal Microsoft services and ensures that any future system modifications remain isolated from my primary environment.

---

# Hardware Configuration

| Component | Configuration |
|-----------|---------------|
| CPU | 4 Virtual Cores |
| Memory | 4 GB RAM |
| Storage | 64 GB Virtual Disk |
| Firmware | UEFI |
| TPM | Enabled |
| Network Mode | NAT |

---

# Installed Software

Current software installed:

- Windows 11 Home (25H2)
- VMware Tools

No additional applications have been installed.

This machine intentionally remains in a clean baseline state before beginning practical cybersecurity labs.

---

# Windows Update Status

Windows was fully updated immediately after installation.

Future updates will continue to be installed periodically to maintain system stability and compatibility.

---

# Security Configuration

Current security configuration:

- Windows Defender enabled
- Default Windows security settings enabled
- TPM enabled
- Secure Boot enabled (UEFI)

No security features have been intentionally disabled.

---

# Snapshot Strategy

| Property | Value |
|----------|-------|
| Snapshot Name | Win-Lab |
| Description | Fresh Install |

The initial snapshot was created immediately after completing the Windows installation and before installing any third-party software.

If future software installations, labs, or experiments make the operating system unstable, this snapshot can be restored to return the system to its original clean state without reinstalling Windows.

---

# Intended Use

This Windows laboratory will be used throughout the roadmap for:

- Windows Event Viewer
- Windows log generation
- Splunk
- Wazuh
- Active Directory
- Windows administration
- Incident response practice
- Blue Team investigations
- Detection engineering labs

---

# Software Policy

This virtual machine is intended exclusively for cybersecurity learning.

The following will **not** be installed or used:

- Personal Microsoft account
- Personal files
- Personal software
- Games
- Non-lab related applications

Maintaining a clean and purpose-built environment makes troubleshooting and recovery significantly easier.

---

# Recovery Plan

If the Windows laboratory becomes unstable, recovery will follow this order:

1. Restore the **Win-Lab** snapshot (Fresh Install).
2. Restore the backed-up virtual machine files stored on a separate computer.
3. Reinstall Windows using the original installation media if recovery is not possible.

---

# Maintenance Plan

To ensure the lab remains reliable throughout the roadmap:

- Keep Windows updated.
- Create snapshots before major software installations or risky experiments.
- Regularly update external VM backups.
- Use the VM only for cybersecurity learning and labs.
- Avoid mixing personal work with the lab environment.

---

# Current Status

**Status:** Ready for Cybersecurity Labs

The Windows virtual machine has been fully configured, updated, secured, and documented. It now serves as the primary Windows environment for the remainder of the SOC Analyst roadmap.

---

# Related Notes

- [SOC-L1 Workspace](soc-l1-workspace.md)
- [VMware Setup](vmware-setup.md)
