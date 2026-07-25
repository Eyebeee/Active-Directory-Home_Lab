# Phase 1 — Preparing the Virtual Environment

## Objective
Set up the core virtualization environment that the rest of the Active Directory lab would be built on: install VirtualBox, create the initial virtual machines, and get Windows 11 installed without being forced into a Microsoft account.

## Environment / Setup
- **Host machine:** Lenovo Yoga 9i
- **Hypervisor:** Oracle VirtualBox 7.1.10 (later upgraded to 7.2.6)
- **VMs created in this phase:**
  - Windows 11
  - Ubuntu
  - Kali Linux

## Steps Taken
1. Installed Oracle VirtualBox 7.1.10 on the host machine.
2. Created three virtual machines: Windows 11, Ubuntu, and Kali Linux.
3. Began the Windows 11 installation and hit Microsoft's forced account sign-in screen during setup.

## Problems and Solutions

**Problem:** Windows 11 setup requires a Microsoft account and an internet connection before it will let you create a local account, which isn't ideal for an isolated lab environment.

**Solution:** Disabled the VM's network adapter *before* starting the Windows 11 installation. With no network connection detected, Windows 11 fell back to offline setup, which allowed creating a local account directly instead of forcing a Microsoft sign-in.

## Next Steps
With the three base VMs created, the next phase focused on getting them talking to each other on an isolated virtual network — see [Phase 2 — Networking](../phase-2-networking/README.md).
