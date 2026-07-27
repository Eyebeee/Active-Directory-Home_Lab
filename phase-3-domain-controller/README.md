# Phase 3 — Domain Controller

## Objective
Deploy a Windows Server 2022 virtual machine, configure it with a static IP, install the Active Directory Domain Services (AD DS) role, and promote it to the first Domain Controller of a new Active Directory forest.

## Environment / Setup
- **VM name:** DC01
- **OS:** Windows Server 2022 Standard Evaluation (Desktop Experience)
- **Allocated resources:** 4 GB RAM, 2 CPU cores, 50 GB virtual disk (later increased to 6144 MB RAM as the lab grew)
- **Network:** Host-Only Adapter (same isolated network from Phase 2)
- **Static IP:** `192.168.56.10`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** blank (no gateway needed on an isolated Host-Only network)
- **Preferred DNS:** `192.168.56.10` (points to itself, since the DC will also serve as the DNS server)
- **Domain created:** `lab.local`
- **Forest/Domain Functional Level:** Windows Server 2016

## Steps Taken
1. Downloaded the official Windows Server 2022 evaluation ISO from Microsoft.
2. Created a new VM named `DC01` and attached the ISO.
3. Installed Windows Server 2022 Standard Evaluation (Desktop Experience, not Server Core) and set the local Administrator password.
4. Before installing any roles, manually configured networking:
   - Static IP `192.168.56.10`
   - Subnet mask `255.255.255.0`
   - Default gateway left blank
   - Preferred DNS pointed to itself (`192.168.56.10`)

![Static IP and DNS configuration on DC01](Screenshots-phase3/static-ip-config.png)

5. In Server Manager, went to **Manage > Add Roles and Features** and installed **Active Directory Domain Services (AD DS)** only — no other roles.
6. After the role finished installing, clicked **Promote this server to a domain controller**.

![Post-deployment configuration prompt to promote the server](Screenshots-phase3/promote-prompt.png)

7. Selected **Add a new forest** and created the domain `lab.local`.

![Deployment Configuration screen creating the lab.local forest](Screenshots-phase3/deployment-config.png)

8. Set both the Forest and Domain Functional Level to **Windows Server 2016** (the most current functional level available, even on Server 2022 — functional levels lag behind OS releases and 2016 remains the ceiling).

![Forest and Domain Functional Level set to Windows Server 2016](Screenshots-phase3/functional-level.png)

9. Received a DNS delegation warning during the prerequisite check.
10. Prerequisite checks passed, the configuration installed, and the server rebooted automatically.
11. Confirmed the final message: *"This server was successfully configured as a domain controller."*

![DC01 successfully configured as a domain controller](Screenshots-phase3/promotion-success.png)

## Problems and Solutions

**Problem:** During promotion, the wizard displayed the warning: *"A delegation for this DNS server cannot be created..."*

![Prerequisites check showing the DNS delegation warning](Screenshots-phase3/dns-delegation-warning.png)

**Solution:** This warning is expected and harmless for a brand-new, isolated lab forest. DNS delegation normally lets a parent DNS zone forward requests for a subdomain to a child DNS server — but since `lab.local` isn't a subdomain of any real parent zone (it's the root of its own private forest with no external DNS infrastructure to delegate from), there's nothing to delegate. Microsoft's promotion wizard raises this warning by default any time it can't find a parent zone to configure delegation in, even when none is needed. It was safely ignored and did not affect the promotion.
