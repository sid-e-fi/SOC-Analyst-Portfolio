---
type: lab-note
status: active
tags:
  - workspace
  - environment-setup
  - organization
---

# SOC-L1 Workspace

> **Purpose**
>
> The **SOC-L1** workspace serves as the central repository for every file, document, virtual machine, project, report, and resource created throughout the SOC Analyst roadmap.
>
> Rather than scattering files across multiple locations, the entire learning environment is organized into a single structured workspace. This ensures consistency, simplifies navigation, and makes the roadmap easier to maintain over the long term.

Workspace Location:

```text
/run/media/siddharth/Data/SOC-L1
```

---

# Workspace Structure

```text
SOC-L1/
│
├── Backups
├── Downloads
├── ISOs
├── Notes
├── Projects
├── Reports
├── Resources
├── Screenshots
└── VMs
```

---

# Folder Purpose

## ISOs

Stores installation media for operating systems and other bootable images used throughout the roadmap.

Examples:

- Windows 11
- Ubuntu
- Kali Linux
- Windows Server

---

## VMs

Contains all virtual machine files.

Examples:

- Windows-11-Lab
- Kali Linux
- Active Directory Lab
- Windows Server

---

## Notes

Contains all documentation written during the roadmap.

This includes:

- Daily logs
- Learning notes
- Lab documentation
- Investigation notes
- Cheat sheets

---

## Projects

Stores source code and project files created during the roadmap.

Examples:

- Splunk dashboards
- Detection engineering projects
- Automation scripts
- Python tools

---

## Reports

Contains reports produced by either myself or cybersecurity tools.

Examples:

- Incident reports
- Investigation reports
- Scan reports
- Log analysis summaries

---

## Resources

A curated collection of useful learning material.

Examples:

- PDFs
- Documentation
- Cheat sheets
- Reference guides
- Helpful articles

---

## Downloads

Temporary storage for files downloaded while following the roadmap.

Files should be moved into their appropriate folders once they become part of the workspace.

---

## Backups

Contains backups of important files created during the roadmap.

This folder serves as an additional recovery layer beyond virtual machine snapshots.

---

## Screenshots

Stores screenshots of important milestones and work that is easier to understand visually.

Examples:

- VMware setup
- Windows installation
- Splunk dashboards
- Wazuh alerts
- Investigation timelines

---

# Organization Rules

The following rules will be followed throughout the roadmap:

- No random files inside the workspace.
- Every file belongs in the correct folder.
- Duplicate files should be removed whenever possible.
- Use consistent naming conventions.
- Keep the workspace clean and easy to navigate.

---

# Naming Convention

A consistent naming convention will be used throughout the roadmap.

Examples:

- Week-01-Day-01
- Splunk-Lab
- Windows-11-Lab
- Brute-Force-Investigation

The goal is to make every file immediately understandable without needing to open it.

---

# Backup Strategy

The workspace follows multiple layers of protection.

Layer 1

VMware snapshots for quick recovery.

Layer 2

Copies of important virtual machines stored on another computer.

Layer 3

Backup of the complete SOC-L1 workspace to cloud storage.

This strategy minimizes the risk of losing months of work due to hardware or operating system failures.

---

# Future Expansion

The workspace is designed to grow throughout the roadmap.

Future additions include:

- Splunk
- Wazuh
- Active Directory
- PCAP captures
- Malware samples
- Detection rules
- Python scripts
- Automation tools
- Incident investigations

The folder structure is intentionally flexible so new technologies can be integrated without major reorganization.

---

# Why Not Store Everything on the Desktop?

The Desktop is intended for temporary convenience, not long-term project management.

Using a dedicated workspace provides several advantages:

- Keeps the operating system clean and uncluttered.
- Makes every file easy to locate.
- Improves professionalism and consistency.
- Simplifies backups.
- Makes collaboration and repository organization easier.
- Reduces the chance of accidentally deleting important files.

Additionally, the SOC-L1 workspace is stored on a dedicated data partition rather than the operating system partition. This means that if the operating system becomes corrupted or requires reinstallation, the project data remains unaffected.

For a long-term cybersecurity roadmap spanning several months, maintaining a structured workspace is significantly more reliable than storing files on the Desktop.

---

# Current Status

**Status:** Operational

The workspace has been created and organized successfully.

It will serve as the central location for all labs, documentation, projects, reports, and learning material throughout the SOC Analyst roadmap.

---

# Related Notes

- [VMware Setup](vmware-setup.md)
- [Windows 11 Lab](windows-11-lab.md)
