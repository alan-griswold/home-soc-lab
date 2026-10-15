
A home security operations center (SOC) lab built on Proxmox VE and Wazuh SIEM, with an isolated Raspberry Pi honeypot. The goal is to practice log collection, threat detection, and incident investigation hands-on, and build a foundation for a follow-up cloud security lab in Microsoft Azure.

> **Status:** In progress. Proxmox host is installed and networked; SOC architecture and Wazuh deployment are next.

## Goals

- Build a virtualized lab environment from bare metal
- Segment the lab network so attack traffic stays isolated
- Deploy Wazuh SIEM and collect logs from Windows and Linux endpoints
- Simulate attacks against an internal honeypot and write detections for them
- Document every phase, including what broke and how it was fixed

## Architecture

**Phase 1: On-prem (this repo)**
- Proxmox VE hypervisor hosting the Wazuh server and endpoint VMs
- Raspberry Pi honeypot on an isolated network segment (internal only, not internet-facing)
- Controlled attack simulation from a separate segment, with alerts flowing into Wazuh

**Phase 2: Cloud (future repo)**
- Internet-facing honeypot in Azure feeding Microsoft Sentinel for real-world attack analysis
_Network diagram coming once the layout is finalized._

## Hardware

| Component | Role |
|---------| -------|
| HP Pavilion Gaming Desktop (GTX 1660 Ti, [RAM] GB RAM) | Proxmox VE host |
| GL.iNet Slate 7 travel router | Wired bridge to campus network, lab segmentation |
| Raspberry Pi | Internal honeypot |

---

## Progress Log

### Phase 0: Proxmox Host Setup ✅

**Bare-metal install**
- Installed Proxmox VE [version] on the desktop's second internal drive, dual-booting alongside Windows on the primary drive.

**Problems and fixes**
- **Nvidia GPU display issue:** The installer froze because of the GTX 1660 Ti. Fixed by booting the installer with the `nomodeset` kernel parameter, which disables the GPU's graphics driver during install.

### Phase 1: Network Access ✅

**Problem:** The dorm has no wired Ethernet, and campus policy doesn't allow students to broadcast their own Wi-Fi networks. The Proxmox host had no usable network connection.

**Solution:** Configured a GL.iNet Slate 7 travel router in repeater mode:
- Connects to campus Wi-Fi as a client
- Provides wired Ethernet to the Proxmox host
- Router's own access point is **disabled** to stay within campus network policy

This also sets up the router to handle VLAN segmentation for the lab in later phases.

### Next Steps

- [ ] Design the lab network layout and segments
- [ ] Deploy the Wazuh server VM
- [ ] Add Windows and Linux endpoint VMs with Wazuh agents
- [ ] Set up the Raspberry Pi honeypot on an isolated segment
- [ ] Simulate attacks and write custom detection rules

---

## Lessons Learned

- Check hypervisor release notes and known issues before installing; the newest version isn't always the most stable.
- GPU compatibility problems are common on consumer hardware; `nomodeset` is a standard first fix.
- Real environments come with constraints (like campus network rules), and the lab design has to work within them.
