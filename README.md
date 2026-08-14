## Environment

The hands-on work in this portfolio runs on a Windows 11 virtual machine under VMware Workstation Pro, hosted on Ubuntu. Virtualizing the Windows side means it can be configured, broken, and rebuilt for log generation and Windows administration practice, and later for SIEM and Active Directory labs, without touching the host system.

**VM configuration:** 4 vCPUs, 4 GB RAM, NAT networking, UEFI firmware with TPM enabled, disk sized to comfortably hold the OS and lab tooling. RAM was originally planned at 8 GB but scaled down after the host became noticeably less responsive; full reasoning and setup steps are in [`labs/week-1-lab-setup/vmware-setup.md`](labs/week-1-lab-setup/vmware-setup.md).

![VMware Workstation Pro showing the Windows-11-Lab VM's configured devices: memory, processors, disk, and network adapter](assets/environment-vm-lab-specs.png)

Before installing any lab software, a snapshot named **Win-Lab** was taken as a clean recovery baseline. If a future experiment breaks the environment, it can be rolled back to this point instead of reinstalling Windows from scratch.

![VMware Workstation Pro "Take Snapshot" dialog creating the Win-Lab snapshot immediately after a fresh Windows 11 install](assets/environment-fresh-install-snapshot.jpg)
