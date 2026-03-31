# Network Documentation
## Cisco Packet Tracer - Enterprise Network

---

## Table of Contents

1. [Project Overview](#1-project-overview)
2. [Devices Used](#2-devices-used)
3. [VLAN & IP Addressing](#3-vlans--ip-addressing)
4. [P2P Link Addressing](#4-point-to-point-link-addressing)
5. [Management & Loopback Addressing](#5-management--loopback-addressing)
6. [EtherChannel](#6-etherchannel)
7. [Trunk Links](#7-trunk-links)
8. [HSRP](#8-hot-standby-router-protocol-hsrp)
9. [Rapid PVST+](#9-rapid-spanning-tree-protocol)
10. [OSPF](#10-open-shortest-path-first-ospf)
11. [DHCP](#11-dynamic-host-configuration-protocol-dhcp)
12. [DNS](#12-domain-name-system-dns)
13. [NTP](#13-network-time-protocol-ntp)
14. [NAT](#14-network-address-translation-nat)
15. [Telephony Service](#15-telephony-service)
16. [ACL](#16-access-control-lists-acls)
17. [Layer 2 Security](#17-layer-2-security)
18. [SSH](#18-secure-shell-ssh)
19. [Syslog](#19-syslog)
20. [Wireless](#20-wireless)
21. [Credentials](#21-credentials)

---

## 1. Project Overview

This project simulates a fully functional enterprise network for a four-floor office building using Cisco Packet Tracer. The design follows the 3-Tier Hierarchical Network Model, consisting of Access, Distribution, and Core layers, with an Edge router connecting to a simulated ISP.

| Item | Detail |
|------|--------|
| **Architecture** | 3-Tier Hierarchical |
| **Floors** | 4 floors |
| **Departments** | 11 departments + 1 Server Room |
| **Routing Protocol** | OSPF |
| **Redundancy** | HSRP, RSTP, EtherChannel |
| **Network Services** | DHCP, DNS, NTP, SYSLOG, SSH, NAT |
| **Network Security** | ACLs, Port Security, DHCP Snooping, DAI |
| **IP Addressing** | 10.1.x.0/24 per VLAN, 10.0.0.x/30 for P2P links |
| **WAN** | 203.0.113.0/30 |

---

## 2. Devices Used

| Device | Model | 
|--------|-------|
| Edge Router | 2911 |
| ISP Router | 2911 |
| Telephony Router | 2811 |
| Core Switches | IE-9320 |
| Distribution Switches | 3650-24PS |
| Access Switches | 2950T-24 |
| Access Points | AP-PT-AC |

---

## 3. VLANs & IP Addressing

| Floor | VLAN | Name | Network | Subnet Mask | Gateway |
|-------|------|------|---------|-------------|---------------------|
| 1 | 10 | LOGISTICS | 10.1.10.0/24 | 255.255.255.0 | 10.1.10.1 |
| 1 | 20 | SALES | 10.1.20.0/24 | 255.255.255.0 | 10.1.20.1 |
| 1 | 30 | CUSTOMER_SUPPORT | 10.1.30.0/24 | 255.255.255.0 | 10.1.30.1 |
| 2 | 40 | HR | 10.1.40.0/24 | 255.255.255.0 | 10.1.40.1 |
| 2 | 50 | ACCOUNTING | 10.1.50.0/24 | 255.255.255.0 | 10.1.50.1 |
| 2 | 60 | LEGAL | 10.1.60.0/24 | 255.255.255.0 | 10.1.60.1 |
| 3 | 70 | RESEARCH | 10.1.70.0/24 | 255.255.255.0 | 10.1.70.1 |
| 3 | 80 | ENGINEERING | 10.1.80.0/24 | 255.255.255.0 | 10.1.80.1 |
| 3 | 90 | PROJECT_MGMT | 10.1.90.0/24 | 255.255.255.0 | 10.1.90.1 |
| 4 | 100 | IT | 10.1.100.0/24 | 255.255.255.0 | 10.1.100.1 |
| 4 | 110 | EXECUTIVE | 10.1.110.0/24 | 255.255.255.0 | 10.1.110.1 |
| 4 | 120 | SERVER | 10.1.120.0/24 | 255.255.255.0 | 10.1.120.1 |
| 1 & 2 | 200 | VOICE | 10.1.200.0/25 | 255.255.255.128 | 10.1.200.1 |
| 3 & 4 | 200 | VOICE | 10.1.200.128/25 | 255.255.255.128 | 10.1.200.129 |
| 1 & 2 | 500 | MANAGEMENT | 10.1.0.0/26 | 255.255.255.192 | 10.1.0.1 |
| 3 & 4 | 500 | MANAGEMENT | 10.1.0.64/26 | 255.255.255.192 | 10.1.0.65 |
| All | 1000 | MISC | N/A | N/A | N/A |

---

## 4. Point-to-Point Link Addressing

All P2P links use /30 subnets from the `10.0.0.0/24` range.

| Link | Interface (Device A) | IP (Device A) | Interface (Device B) | IP (Device B) | Subnet |
|------|----------------------|---------------|----------------------|---------------|--------|
| ISP1 ↔ R1 | G0/0/0 | 203.0.113.1 | G0/0/0 (DHCP) | 203.0.113.2 | 203.0.113.0/30 |
| R1 ↔ CSW1 | G0/1/0 | 10.0.0.2 | G1/0/3 | 10.0.0.1 | 10.0.0.0/30 |
| R1 ↔ CSW2 | G0/2/0 | 10.0.0.6 | G1/0/3 | 10.0.0.5 | 10.0.0.4/30 |
| CSW1 ↔ CSW2 | Po1 | 10.0.0.9 | Po1 | 10.0.0.10 | 10.0.0.8/30 |
| CSW1 ↔ DSW1 | G1/0/4 | 10.0.0.14 | G1/1/3 | 10.0.0.13 | 10.0.0.12/30 |
| CSW2 ↔ DSW1 | G1/0/4 | 10.0.0.18 | G1/1/4 | 10.0.0.17 | 10.0.0.16/30 |
| CSW1 ↔ DSW2 | G1/0/5 | 10.0.0.22 | G1/1/3 | 10.0.0.21 | 10.0.0.20/30 |
| CSW2 ↔ DSW2 | G1/0/5 | 10.0.0.26 | G1/1/4 | 10.0.0.25 | 10.0.0.24/30 |
| CSW1 ↔ DSW3 | G1/0/6 | 10.0.0.30 | G1/1/3 | 10.0.0.29 | 10.0.0.28/30 |
| CSW2 ↔ DSW3 | G1/0/6 | 10.0.0.34 | G1/1/4 | 10.0.0.33 | 10.0.0.32/30 |
| CSW1 ↔ DSW4 | G1/0/7 | 10.0.0.38 | G1/1/3 | 10.0.0.37 | 10.0.0.36/30 |
| CSW2 ↔ DSW4 | G1/0/7 | 10.0.0.42 | G1/1/4 | 10.0.0.41 | 10.0.0.40/30 |

---

## 5. Management & Loopback Addressing

| Device | IP Address | Subnet Mask | VLAN / Type |
|--------|------------|-------------|-------------|
| R1 | 10.1.0.135 | 255.255.255.255 | Loopback 0 |
| CSW1 | 10.1.0.133 | 255.255.255.255 | Loopback 0 |
| CSW2 | 10.1.0.134 | 255.255.255.255 | Loopback 0 |
| DSW1 | 10.1.0.129 | 255.255.255.255 | Loopback 0 |
| DSW2 | 10.1.0.130 | 255.255.255.255 | Loopback 0 |
| DSW3 | 10.1.0.131 | 255.255.255.255 | Loopback 0 |
| DSW4 | 10.1.0.132 | 255.255.255.255 | Loopback 0 |
| ASW1 | 10.1.0.6 | 255.255.255.192 | VLAN 500 |
| ASW2 | 10.1.0.7 | 255.255.255.192 | VLAN 500 |
| ASW3 | 10.1.0.8 | 255.255.255.192 | VLAN 500 |
| ASW4 | 10.1.0.9 | 255.255.255.192 | VLAN 500 |
| ASW5 | 10.1.0.10 | 255.255.255.192 | VLAN 500 |
| ASW6 | 10.1.0.11 | 255.255.255.192 | VLAN 500 |
| ASW7 | 10.1.0.71 | 255.255.255.192 | VLAN 500 |
| ASW8 | 10.1.0.72 | 255.255.255.192 | VLAN 500 |
| ASW9 | 10.1.0.73 | 255.255.255.192 | VLAN 500 |
| ASW10 | 10.1.0.74 | 255.255.255.192 | VLAN 500 |
| ASW11 | 10.1.0.75 | 255.255.255.192 | VLAN 500 |
| ASW12 | 10.1.0.76 | 255.255.255.192 | VLAN 500 |

---

## 6. EtherChannel

| Port-Channel | Devices | Member Interfaces | Protocol | Layer | Role |
|--------------|---------|-------------------|----------|-------|------|
| Po1 | DSW1 ↔ DSW2 | G1/1/1, G1/1/2 | LACP | Layer 2| Trunk |
| Po1 | DSW3 ↔ DSW4 | G1/1/1, G1/1/2 | LACP | Layer 2| Trunk |
| Po1 | CSW1 ↔ CSW2 | G1/0/1, G1/0/2 | LACP | Layer 3 | Routing |

---

## 7. Trunk Links

### ASW1 to ASW6 (Floors 1 & 2)

| Access Switch | Uplink to DSW | Native VLAN | Allowed VLANs |
|---------------|---------------|-------------|---------------|
| ASW1 | DSW1 (G1/0/1), DSW2 (G1/0/1) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |
| ASW2 | DSW1 (G1/0/2), DSW2 (G1/0/2) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |
| ASW3 | DSW1 (G1/0/3), DSW2 (G1/0/3) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |
| ASW4 | DSW1 (G1/0/4), DSW2 (G1/0/4) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |
| ASW5 | DSW1 (G1/0/5), DSW2 (G1/0/5) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |
| ASW6 | DSW1 (G1/0/6), DSW2 (G1/0/6) | 1000 | 10, 20, 30, 40, 50, 60, 200, 500 |

### ASW7 to ASW12 (Floors 3 & 4)

| Access Switch | Uplink to DSW | Native VLAN | Allowed VLANs |
|---------------|---------------|-------------|---------------|
| ASW7 | DSW3 (G1/0/1), DSW4 (G1/0/1) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |
| ASW8 | DSW3 (G1/0/2), DSW4 (G1/0/2) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |
| ASW9 | DSW3 (G1/0/3), DSW4 (G1/0/3) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |
| ASW10 | DSW3 (G1/0/4), DSW4 (G1/0/4) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |
| ASW11 | DSW3 (G1/0/5), DSW4 (G1/0/5) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |
| ASW12 | DSW3 (G1/0/6), DSW4 (G1/0/6) | 1000 | 70, 80, 90, 100, 110, 120, 200, 500 |

---

## 8. Hot Standby Router Protocol (HSRP)

HSRP Version 2 is configured on all distribution switches. Priority 110 with preempt is set on the active router. The standby router uses the default priority of 100.

| VLAN | Virtual IP | Active Router | Active IP | Standby Router | Standby IP |
|------|----------------------|---------------|-----------|----------------|------------|
| 10 | 10.1.10.1 | DSW1 | 10.1.10.2 | DSW2 | 10.1.10.3 |
| 20 | 10.1.20.1 | DSW1 | 10.1.20.2 | DSW2 | 10.1.20.3 |
| 30 | 10.1.30.1 | DSW1 | 10.1.30.2 | DSW2 | 10.1.30.3 |
| 40 | 10.1.40.1 | DSW2 | 10.1.40.3 | DSW1 | 10.1.40.2 |
| 50 | 10.1.50.1 | DSW2 | 10.1.50.3 | DSW1 | 10.1.50.2 | 
| 60 | 10.1.60.1 | DSW2 | 10.1.60.3 | DSW1 | 10.1.60.2 |
| 70 | 10.1.70.1 | DSW3 | 10.1.70.2 | DSW4 | 10.1.70.3 |
| 80 | 10.1.80.1 | DSW3 | 10.1.80.2 | DSW4 | 10.1.80.3 |
| 90 | 10.1.90.1 | DSW3 | 10.1.90.2 | DSW4 | 10.1.90.3 |
| 100 | 10.1.100.1 | DSW4 | 10.1.100.3 | DSW3 | 10.1.100.2 |
| 110 | 10.1.110.1 | DSW4 | 10.1.110.3 | DSW3 | 10.1.110.2 |
| 120 | 10.1.120.1 | DSW4 | 10.1.120.3 | DSW3 | 10.1.120.2 |
| 200 (Floors 1 & 2) | 10.1.200.1 | DSW2 | 10.1.200.3 | DSW1 | 10.1.200.2 |
| 200 (Floors 3 & 4) | 10.1.200.129 | DSW4 | 10.1.200.131 | DSW3 | 10.1.200.130 |
| 500 (Floors 1 & 2) | 10.1.0.1 | DSW1 | 10.1.0.2 | DSW2 | 10.1.0.3 |
| 500 (Floors 3 & 4) | 10.1.0.65 | DSW3 | 10.1.0.66 | DSW4 | 10.1.0.67 |

---

## 9. Rapid Spanning Tree Protocol

Mode: **Rapid PVST+** on all access and distribution switches.
PortFast + BPDU Guard enabled on access ports facing end devices.

| VLAN | Root Bridge | Root Priority | Secondary Bridge | Secondary Priority |
|------|-------------|---------------|------------------|--------------------|
| 10 | DSW1 | 0 | DSW2 | 4096 |
| 20 | DSW1 | 0 | DSW2 | 4096 |
| 30 | DSW1 | 0 | DSW2 | 4096 |
| 40 | DSW2 | 0 | DSW1 | 4096 |
| 50 | DSW2 | 0 | DSW1 | 4096 |
| 60 | DSW2 | 0 | DSW1 | 4096 |
| 70 | DSW3 | 0 | DSW4 | 4096 |
| 80 | DSW3 | 0 | DSW4 | 4096 |
| 90 | DSW3 | 0 | DSW4 | 4096 |
| 100 | DSW4 | 0 | DSW3 | 4096 |
| 110 | DSW4 | 0 | DSW3 | 4096 |
| 120 | DSW4 | 0 | DSW3 | 4096 |
| 200 (Floors 1 & 2) | DSW2 | 0 | DSW1 | 4096 |
| 200 (Floors 3 & 4) | DSW4 | 0 | DSW3 | 4096 |
| 500 (Floors 1 & 2) | DSW1 | 0 | DSW2 | 4096 |
| 500 (Floors 3 & 4) | DSW3 | 0 | DSW4 | 4096 |

> **Note:** RSTP root/secondary bridge assignments are aligned with HSRP active/standby roles per VLAN to ensure traffic symmetry and avoid suboptimal forwarding paths.

---

## 10. Open Shortest Path First (OSPF)

| Parameter | Value |
|-----------|-------|
| Process ID | 1 |
| Area | 0 (Backbone) |
| Type | Single Area OSPF |
| Network Type | Point-to-Point (on all P2P links) |

### Router IDs

| Device | Router ID |
|--------|-----------|
| R1 | 10.1.0.135 |
| CSW1 | 10.1.0.133 |
| CSW2 | 10.1.0.134 |
| DSW1 | 10.1.0.129 |
| DSW2 | 10.1.0.130 |
| DSW3 | 10.1.0.131 |
| DSW4 | 10.1.0.132 |

### Passive Interfaces

All VLAN SVIs and Loopback interfaces are set as passive to prevent unnecessary OSPF hellos toward end devices.

### Default Route

R1 is configured a static default route (`0.0.0.0/0`) pointing to ISP1 (`203.0.113.1`) and redistributes it into OSPF using `default-information originate`.

---

## 11. Dynamic Host Configuration Protocol (DHCP)

### Data VLANs (served by DHCP Server at 10.1.120.20)

DHCP relay (`ip helper-address 10.1.120.20`) is configured on all data VLAN SVIs of DSW1–DSW4.

| Pool Name | VLAN | Default Gateway | DNS Server | Start IP | Subnet Mask | Max Users |
|-----------|------|-----------------|------------|----------|-------------|-----------|
| LOGISTICS10 | 10 | 10.1.10.1 | 10.1.120.30 | 10.1.10.11 | 255.255.255.0 | 244 |
| SALES20 | 20 | 10.1.20.1 | 10.1.120.30 | 10.1.20.11 | 255.255.255.0 | 244 |
| CS30 | 30 | 10.1.30.1 | 10.1.120.30 | 10.1.30.11 | 255.255.255.0 | 244 |
| HR40 | 40 | 10.1.40.1 | 10.1.120.30 | 10.1.40.11 | 255.255.255.0 | 244 |
| ACCOUNTING50 | 50 | 10.1.50.1 | 10.1.120.30 | 10.1.50.11 | 255.255.255.0 | 244 |
| LEGAL60 | 60 | 10.1.60.1 | 10.1.120.30 | 10.1.60.11 | 255.255.255.0 | 244 |
| RND70 | 70 | 10.1.70.1 | 10.1.120.30 | 10.1.70.11 | 255.255.255.0 | 244 |
| ENGINEERING80 | 80 | 10.1.80.1 | 10.1.120.30 | 10.1.80.11 | 255.255.255.0 | 244 |
| PROJECT90 | 90 | 10.1.90.1 | 10.1.120.30 | 10.1.90.11 | 255.255.255.0 | 244 |
| IT100 | 100 | 10.1.100.1 | 10.1.120.30 | 10.1.100.11 | 255.255.255.0 | 244 |
| EXECUTIVE110 | 110 | 10.1.110.1 | 10.1.120.30 | 10.1.110.11 | 255.255.255.0 | 244 |

### Voice VLAN (served by Telephony Server at 10.1.200.133)

DHCP relay (`ip helper-address 10.1.200.133`) is configured on all Voice VLAN SVIs of DSW1 and DSW2.

| Pool Name | VLAN | Subnet | Default Gateway | TFTP Server | DNS Server |
|-----------|------|--------|-----------------|--------------------------|------------|
| VOICE-FL12 | 200 (Floors 1 & 2) | 10.1.200.0/25 | 10.1.200.1 | 10.1.200.133 | 10.1.120.30 |
| VOICE-FL34 | 200 (Floors 3 & 4) | 10.1.200.128/25 | 10.1.200.129 | 10.1.200.133 | 10.1.120.30 |

**Excluded addresses:**
- `10.1.200.1 – 10.1.200.10` (reserved for network infrastructure)
- `10.1.200.129 – 10.1.200.138` (reserved for network infrastructure)

---

## 12. Domain Name System (DNS)

**DNS Server IP:** 10.1.120.30

### A Records

| Name | IP Address | Notes |
|------|------------|-------|
| google.com | 172.253.62.100 | Simulated (matches ISP1 loopback) |
| youtube.com | 152.250.31.92 | Simulated (matches ISP1 loopback) |
| gmail.com | 66.235.200.145 | Simulated (matches ISP1 loopback) |
| enterprise.com | 10.1.120.40 | Internal (points to Syslog/Web server) |

### CNAME Records

| Name | Canonical Hostname |
|------|--------------------|
| www.google.com | google.com |
| www.youtube.com | youtube.com |
| www.gmail.com | gmail.com |

---

## 13. Network Time Protocol (NTP)

| Device | Role | NTP Source | Auth Key |
|--------|------|------------|----------|
| ISP1 | NTP Master (Stratum 1) | Internal clock | None |
| R1 | NTP Server (Stratum 5) / Client | 216.239.25.4 (ISP1) | Enterprise@NTP |
| All others | NTP Client | 10.1.0.135 (R1) | Enterprise@NTP |

---

## 14. Network Address Translation (NAT)

**Type:** Dynamic PAT  
**Configured on:** R1  
**NAT Outside Interface:** G0/0/0 (toward ISP1)  
**NAT Inside Interfaces:** G0/1/0 (to CSW1), G0/2/0 (to CSW2)  

The following internal networks are permitted for NAT:

| Permitted Network | Description |
|-------------------|-------------|
| 10.0.0.0/24 | P2P Infrastructure Links |
| 10.1.0.0/24 | Management VLAN |
| 10.1.10.0/24 – 10.1.120.0/24 | All Data VLANs |
| 10.1.200.0/24 | Voice VLAN |

---

## 15. Telephony Service

**CME Router:** Telephony Server (Cisco 2811)  
**IP Source Address:** 10.1.200.133, Port 2000  
**Max ePhones:** 11 | **Max DNs:** 11  

### Interface Addressing

| Interface | Connected To | IP Address | Subnet |
|-----------|-------------|------------|--------|
| F0/0 | ASW12 (VLAN 200) | 10.1.200.133 | /25 |
| F0/1 | ASW12 (VLAN 120) | 10.1.120.10 | /24 |

### ePhone Directory Numbers

| ePhone | Extension | Department | Switch |
|--------|-----------|------------|--------|
| 1 | 1001 | Engineering & Design | ASW8 |
| 2 | 1002 | Executive Management | ASW11 |
| 3 | 1003 | IT Operations | ASW10 |
| 4 | 1004 | Project Management | ASW9 |
| 5 | 1005 | Research & Development | ASW7 |
| 6 | 1006 | Sales & Marketing | ASW2 |
| 7 | 1007 | Human Resources | ASW4 |
| 8 | 1008 | Customer Support | ASW3 |
| 9 | 1009 | Legal & Compliance | ASW6 |
| 10 | 1010 | Logistics & Operations | ASW1 |
| 11 | 1011 | Finance & Accounting | ASW5 |

---

## 16. Access Control Lists (ACLs)

### ACL Summary

| ACL Name | Type | Applied On | Direction | Purpose |
|----------|------|------------|-----------|---------|
| PROTECT_SERVERS | Extended | DSW3, DSW4 (VLAN 120 SVI) | Outbound | Restricts access to server room, only IT and necessary services allowed |
| VOICE_ISOLATION | Extended | DSW1 to DSW4 (VLAN 200 SVI) | Inbound | Isolates voice traffic, phones can only reach CME and each other |
| RESTRICT_ACCOUNTING | Extended | DSW1, DSW2 (VLAN 50 SVI) | Inbound | Accounting can only reach servers (VLAN 120) and IT (VLAN 100) |
| STANDARD_ACCESS | Extended | DSW1 to DSW4 (Department VLANs) | Inbound | Blocks access to infrastructure, accounting, management, and voice VLANs |
| VTY_SSH | Standard | All devices (VTY lines) | Inbound | Restricts SSH access to IT VLAN (10.1.100.0/24) only |
| PAT-ACL | Standard | R1 — NAT | Outbound | Defines which internal networks are translated for internet access |

---

### PROTECT_SERVERS (Applied: VLAN 120 — Outbound on DSW3 & DSW4)

| # | Action | Protocol | Source | Destination | Port | Notes |
|---|--------|----------|--------|-------------|------|-------|
| 1 | Permit | IP | 10.1.100.0/24 | 10.1.120.0/24 | Any | IT full access to server room |
| 2 | Permit | UDP | 10.0.0.0/8 | 10.1.120.20 | 67 | DHCP requests to DHCP server |
| 3 | Permit | UDP | 10.0.0.0/8 | 10.1.120.20 | 68 | DHCP replies from DHCP server |
| 4 | Permit | UDP | 10.0.0.0/8 | 10.1.120.30 | 53 | DNS queries (UDP) |
| 5 | Permit | TCP | 10.0.0.0/8 | 10.1.120.30 | 53 | DNS queries (TCP) |
| 6 | Permit | TCP | 10.0.0.0/8 | 10.1.120.40 | 80 | HTTP to Syslog/Web server |
| 7 | Permit | TCP | 10.0.0.0/8 | 10.1.120.40 | 443 | HTTPS to Syslog/Web server |
| 8 | Permit | UDP | 10.0.0.0/8 | 10.1.120.40 | 514 | Syslog messages |
| 9 | Deny | IP | 10.0.0.0/8 | 10.1.120.0/24 | Any | Block all other access to server room |
| 10 | Permit | IP | Any | Any | Any | Allow everything else |

---

### VOICE_ISOLATION (Applied: VLAN 200 — Inbound on DSW1, DSW2, DSW3, DSW4)

| # | Action | Protocol | Source | Destination | Port | Notes |
|---|--------|----------|--------|-------------|------|-------|
| 1 | Permit | UDP | Any | 10.1.200.133 | 67 | DHCP request to CME |
| 2 | Permit | UDP | Any | 10.1.200.133 | 68 | DHCP reply from CME |
| 3 | Permit | UDP | 10.1.200.0/24 | 10.1.200.133 | 69 (TFTP) | Phone firmware/config download |
| 4 | Permit | TCP | 10.1.200.0/24 | 10.1.200.133 | 2000 (SCCP) | Phone registration to CME |
| 5 | Permit | IP | 10.1.200.0/25 | 10.1.200.128/25 | Any | Inter-floor voice communication |
| 6 | Permit | IP | 10.1.200.128/25 | 10.1.200.0/25 | Any | Inter-floor voice communication |
| 7 | Deny | IP | 10.1.200.0/24 | 10.1.0.0/16 | Any | Block voice from accessing data network |
| 8 | Permit | IP | Any | Any | Any | Allow everything else |

---

### RESTRICT_ACCOUNTING (Applied: VLAN 50 — Inbound on DSW1 & DSW2)

| # | Action | Protocol | Source | Destination | Notes |
|---|--------|----------|--------|-------------|-------|
| 1 | Permit | IP | 10.1.50.0/24 | 10.1.120.0/24 | Allow access to servers |
| 2 | Permit | IP | 10.1.50.0/24 | 10.1.100.0/24 | Allow access to IT |
| 3–14 | Deny | IP | 10.1.50.0/24 | All Data VLANs | Block lateral movement |
| 15 | Permit | IP | Any | Any | Allow other traffic |

---

### STANDARD_ACCESS (Applied: Department VLANs — Inbound on DSW1–DSW4)

Applied to VLANs: **10, 20, 30, 40, 60** (on DSW1 & DSW2) and **70, 80, 90, 110** (on DSW3 & DSW4)

| # | Action | Protocol | Source | Destination | Notes |
|---|--------|----------|--------|-------------|-------|
| 1 | Deny | IP | Any | 10.0.0.0/24 | Block access to P2P infrastructure |
| 2 | Deny | IP | Any | 10.1.50.0/24 | Block access to Accounting VLAN |
| 3 | Deny | IP | Any | 10.1.0.0/24 | Block access to Management VLAN |
| 4 | Deny | IP | Any | 10.1.200.0/24 | Block access to Voice VLAN |
| 5 | Permit | IP | Any | Any | Allow all other traffic |

---

## 17. Layer 2 Security

### Port Security

| Switch | Interface | Max MACs | Mode | Violation Action |
|--------|-----------|----------|------|-----------------|
| ASW1 to ASW11 | F0/1 (PC + Phone) | 3 | Sticky | Restrict |
| ASW1 to ASW11 | F0/23 (Printer) | 1 | Sticky | Restrict |
| ASW12 | F0/1–F0/5 (Servers) | 1 | Sticky | Restrict |

> **Note:** F0/24 (Access Point ports) on ASW1 to ASW11 do not have port security applied.

### DHCP Snooping

| Switch Group | Trusted Ports | Rate-Limited Ports | Snooped VLANs |
|-------------|---------------|-------------------|---------------|
| ASW1 to ASW6 | G0/1, G0/2 (uplinks) | F0/1, F0/23 at 15 pps; F0/24 at 100 pps | 10, 20, 30, 40, 50, 60, 200 |
| ASW7 to ASW11 | G0/1, G0/2 (uplinks) | F0/1, F0/23 at 15 pps; F0/24 at 100 pps | 70, 80, 90, 100, 110, 120, 200 |
| ASW12 | G0/1, G0/2 (uplinks) | F0/1–F0/5 at 15 pps | 70, 80, 90, 100, 110, 120, 200 |

> **Note:** `no ip dhcp snooping information option` is configured on all access switches to prevent Option 82 issues with the centralized DHCP server.

### Dynamic ARP Inspection (DAI)

| Switch Group | Trusted Ports | Inspected VLANs | Validation |
|-------------|---------------|-----------------|------------|
| ASW1 to ASW6 | G0/1, G0/2 (uplinks) | 10, 20, 30, 40, 50, 60, 200 | src-mac, dst-mac, ip |
| ASW7 to ASW12 | G0/1, G0/2 (uplinks) | 70, 80, 90, 100, 110, 120, 200 | src-mac, dst-mac, ip |

---

## 18. Secure Shell (SSH)

| Parameter | Value |
|-----------|-------|
| SSH Version | 2 |
| RSA Key Size | 4096 bits |
| Domain Name | enterprise.com |
| Permitted Source | 10.1.100.0/24 (IT VLAN only) |
| Authentication | Local |
| Applied To | VTY 0–15 on all network devices |

CDP is disabled on all access ports facing end devices on ASW1 to ASW12 to prevent topology information leakage.

---

## 19. Syslog

| Parameter | Value |
|-----------|-------|
| Syslog Server IP | 10.1.120.40 |
| Logging Level | Debugging |
| Timestamps | `datetime msec` |
| Buffer Size| 8192 bytes |
| `logging userinfo` | Enabled on Distribution, Core, and Router |

---

## 20. Wireless

One access point per department, configured with 5 GHz channels and WPA2-PSK. Each AP is on its respective department VLAN.

| Access Point | SSID | VLAN | 5 GHz Channel | Security | Passphrase |
|-------------|------|------|---------------|----------|------------|
| AP-LOG | LOG-WIFI | 10 | 36 | WPA2-PSK | Enterprise@LOG |
| AP-MKT | MKT-WIFI | 20 | 40 | WPA2-PSK | Enterprise@MKT |
| AP-CS | CS-WIFI | 30 | 44 | WPA2-PSK | Enterprise@CS |
| AP-HR | HR-WIFI | 40 | 48 | WPA2-PSK | Enterprise@HR |
| AP-ACC | ACC-WIFI | 50 | 52 | WPA2-PSK | Enterprise@ACC |
| AP-LGL | LGL-WIFI | 60 | 56 | WPA2-PSK | Enterprise@LGL |
| AP-RND | RND-WIFI | 70 | 60 | WPA2-PSK | Enterprise@RND |
| AP-ENG | ENG-WIFI | 80 | 64 | WPA2-PSK | Enterprise@ENG |
| AP-PROJ | PROJ-WIFI | 90 | 100 | WPA2-PSK | Enterprise@PROJ |
| AP-IT | IT-WIFI | 100 | 104 | WPA2-PSK | Enterprise@IT |
| AP-EXEC | EXEC-WIFI | 110 | 108 | WPA2-PSK | Enterprise@EXEC |

> No access point is deployed in the Server Room (VLAN 120).
> Non-overlapping channels are used across all APs to minimize interference.

---

## 21. Credentials

| Credential | Value |
|------------|-------|
| Username | netadmin |
| Password | Root@1234 |
| Enable Secret | Admin@1234 |
| VTP Domain | ENTERPRISE |
| VTP Password | Enterprise@VTP |
| NTP Auth Key | enterprise@NTP |
