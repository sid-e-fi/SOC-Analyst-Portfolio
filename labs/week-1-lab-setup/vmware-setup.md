---
type: lab-note
status: active
tags:
  - virtualization
  - vmware
  - environment-setup
---

# VMware Setup

> **Purpose**
>
> VMware Workstation Pro was chosen to create an isolated Windows laboratory while keeping Ubuntu as the primary operating system.
>
> Using virtualization allows multiple operating systems to run simultaneously on a single physical machine without requiring separate hardware. This provides a safe environment for experimentation, software testing, and future cybersecurity labs.

---

# Host System

| Property | Value |
|----------|-------|
| Host Operating System | Ubuntu 26.04 LTS |
| Host Name | siddharth-Linux |
| Hypervisor | VMware Workstation Pro |
| VMware Version | VMware Workstation Pro 26H2 |

---

# Why VMware Workstation Pro?

I have previously used both VMware Workstation and Oracle VirtualBox.

Although both are capable virtualization platforms, I experienced compatibility issues and unresolved errors while using VirtualBox. VMware has consistently provided a smoother and more reliable experience for me, including better guest operating system compatibility.

Based on that experience, VMware Workstation Pro was selected as the hypervisor for this roadmap.

---

# Why Use Virtual Machines?

Running Windows inside a virtual machine allows me to:

- Run Ubuntu and Windows simultaneously on one computer.
- Keep my primary operating system separate from my cybersecurity lab.
- Experiment freely without risking my main installation.
- Generate Windows logs for future investigations.
- Build and test enterprise security tools such as Active Directory, SIEMs, and monitoring software.
- Restore the environment quickly if it becomes unstable.

---

# Virtual Machine Configuration

| Setting | Value |
|----------|-------|
| VM Name | Windows-11-Lab |
| Guest OS | Windows 11 x64 |
| CPU | 4 Cores |
| Memory | 4 GB RAM |
| Storage | 64 GB Virtual Disk |
| Firmware | UEFI |
| TPM | Enabled |
| Network Mode | NAT |

---

# Memory Allocation

The original roadmap recommended allocating 8 GB of RAM to the virtual machine.

During testing, assigning 8 GB caused the Ubuntu host to become noticeably less responsive.

To maintain a better balance between host and guest performance, the VM memory was reduced to 4 GB.

This configuration provides acceptable Windows performance while keeping the host operating system responsive.

---

# Networking

The virtual machine uses **NAT (Network Address Translation)** networking.

This allows the guest operating system to access the internet through the host while remaining isolated from the local network.

Each virtual machine receives its own private virtual IP address inside VMware's virtual network. To devices on the physical network, all VM traffic appears to originate from the host computer.

NAT was chosen because it is simple, secure, and suitable for the majority of beginner cybersecurity labs.

---

# VMware Tools

VMware Tools was installed immediately after Windows installation.

Benefits include:

- Improved display resolution
- Better graphics performance
- Automatic screen resizing
- Improved mouse and keyboard integration
- Clipboard sharing between host and guest
- Better overall virtual machine performance

Installing VMware Tools is considered essential before beginning practical labs.

---

# Snapshot Strategy

| Property | Value |
|----------|-------|
| Snapshot Name | Win-Lab |

Before installing any cybersecurity software, a snapshot named **Win-Lab** was created.

This snapshot represents the baseline configuration of the Windows laboratory.

If future software installations, experiments, or configuration changes cause instability, the VM can be restored to this clean state within minutes.

This eliminates the need to reinstall Windows after every major issue.

---

# Storage Location

The virtual machine is stored inside:

```text
SOC-L1/
└── VMs/
```

A secondary backup of the VM will also be maintained on another computer to protect against storage failure.

---

# Planned Uses

This Windows laboratory will be used throughout the roadmap for:

- Windows administration
- Event Viewer investigations
- Windows log generation
- Active Directory labs
- Sysmon configuration
- SIEM integration
- Threat detection exercises
- Incident response practice
- Blue Team investigations

---

# Recovery Strategy

If the virtual machine becomes unstable during future labs, the recovery process will be:

1. Restore the **Win-Lab** snapshot.
2. If snapshots become unusable or corrupted, restore the VM from the external backup.
3. If both recovery methods fail, reinstall Windows using the original installation ISO.

This ensures that the lab can always be rebuilt with minimal downtime.

---

# Related Notes

- [SOC-L1 Workspace](soc-l1-workspace.md)
- [Windows 11 Lab](windows-11-lab.md)
