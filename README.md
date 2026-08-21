# Enterprise Network Lab

![Cisco IOS](https://img.shields.io/badge/Cisco-IOS-blue)
![Cisco Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![OSPF](https://img.shields.io/badge/Routing-OSPF-orange)
![VLAN](https://img.shields.io/badge/Switching-VLAN-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

A practical Cisco enterprise networking lab demonstrating enterprise LAN/WAN design, VLAN segmentation, Layer 2 and Layer 3 switching, inter-VLAN routing, STP, EtherChannel, OSPF, DHCP, SSH, ACL-based security, troubleshooting, and network verification using Cisco Packet Tracer.

---

## Project Overview

This project simulates a small enterprise network environment using Cisco Packet Tracer.

The lab demonstrates practical implementation of:

- Enterprise network topology design
- VLAN segmentation
- 802.1Q trunking
- Inter-VLAN routing
- Rapid-PVST / STP
- EtherChannel using LACP
- OSPF dynamic routing
- DHCP services
- SSH device management
- Extended IPv4 ACL security
- End-to-end connectivity testing
- Network troubleshooting
- Cisco IOS verification commands

---

## Project Demo

The following diagram shows the enterprise network topology implemented in Cisco Packet Tracer.

![Enterprise Network Topology](topology/enterprise-network-topology.png)

---

## Network Architecture

The lab consists of an enterprise edge router, core Layer 3 switches, access Layer 2 switches, and end devices.

### Network Components

| Device | Role |
|---|---|
| R1-EDGE | Enterprise edge / upstream router |
| R2-CORE | Core routing device |
| CORE-SW1 | Layer 3 core switch |
| CORE-SW2 | Layer 3 core switch |
| ACCESS-SW1 | Access Layer switch |
| ACCESS-SW2 | Access Layer switch |
| User PCs | Internal user endpoints |
| Server | Enterprise server endpoint |
| Guest Endpoint | Guest network endpoint |

### Network Flow

```text
                     R1-EDGE
                        |
                       OSPF
                        |
                     R2-CORE
                        |
          +-------------+-------------+
          |                           |
      CORE-SW1                    CORE-SW2
          |                           |
    EtherChannel                EtherChannel
          |                           |
      ACCESS-SW1                ACCESS-SW2
          |                           |
    +-----+-----+             +-----+-----+
    |           |             |           |
  Users       Server        Users       Guest
```
---

## VLAN Design

VLAN segmentation is used to separate user, server, management, guest, and native VLAN traffic.

| VLAN ID | Name | Network | Gateway | Purpose |
|---:|---|---|---|---|
| 10 | USERS | 10.10.10.0/24 | 10.10.10.1 | Internal user endpoints |
| 20 | SERVERS | 10.10.20.0/24 | 10.10.20.1 | Server infrastructure |
| 30 | MANAGEMENT | 10.10.30.0/24 | 10.10.30.1 | Network and device management |
| 40 | GUEST | 10.10.40.0/24 | 10.10.40.1 | Guest endpoints |
| 99 | NATIVE | 10.10.99.0/24 | 10.10.99.1 | Native / management VLAN |

### VLAN Security

Guest traffic from VLAN 40 is restricted from accessing internal networks using the `GUEST-RESTRICT` extended ACL.

The ACL restricts guest access to:

- USERS — `10.10.10.0/24`
- SERVERS — `10.10.20.0/24`
- MANAGEMENT — `10.10.30.0/24`
- Remote internal networks — `10.20.10.0/24` through `10.20.99.0/24`
---

## Switching

VLANs 10, 20, 30, 40, and 99 are configured across the switching infrastructure.

### 802.1Q Trunking

802.1Q trunking is configured on the Port-channel uplinks.

**Allowed VLANs:**

```text
10,20,30,40,99
```
### EtherChannel

LACP EtherChannel is used to provide link redundancy and increased bandwidth between network devices.

| Device | Port-channel | Member Interfaces |
|---|---|---|
| CORE-SW1 | Po1 | Fa0/2, Fa0/5 |
| CORE-SW2 | Po2 | Fa0/2, Fa0/5 |
| ACCESS-SW1 | Po1 | Fa0/1, Fa0/4 |
| ACCESS-SW2 | Po2 | Fa0/1, Fa0/4 |

The EtherChannels operate as Layer 2 port-channels.

**Verification:**

```text
show etherchannel summary
```
### Spanning Tree Protocol (STP)

Rapid-PVST / STP is used to prevent Layer 2 switching loops.

The core switches provide the STP root functionality for the VLANs in the lab.

**Verification Commands:**

```text
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
```
### Inter-VLAN Routing

Layer 3 switching provides gateway interfaces for the VLANs and enables communication between VLANs.

The configured VLAN gateway interfaces include:

| VLAN | Gateway |
|---:|---|
| 10 | 10.10.10.1/24 |
| 20 | 10.10.20.1/24 |
| 30 | 10.10.30.1/24 |
| 40 | 10.10.40.1/24 |
| 99 | 10.10.99.1/24 |

**Verification Commands:**

```text
show ip interface brief
show ip route
```
### OSPF Dynamic Routing

OSPF is used as the dynamic routing protocol between the core network and upstream routing infrastructure.

### OSPF Network Links

| Link | Network |
|---|---|
| R2-CORE ↔ CORE-SW1 | 10.10.101.0/30 |
| R2-CORE ↔ CORE-SW2 | 10.10.102.0/30 |

The core switches advertise their connected VLAN networks through OSPF.

**OSPF Verification:**

```text
show ip ospf neighbor
```
**Routing Table Verification:**

```text
show ip route
```
### DHCP

DHCP is configured to provide IP addresses to end devices.

DHCP functionality can be verified using:

```text
show ip dhcp binding
```
### SSH Management

SSH version 2 is enabled for secure remote device management.

**SSH Verification:**

```text
show ip ssh
```
Expected result:

```text
SSH Enabled - version 2.0
```
### Network Security

#### Guest Network ACL

An extended ACL named `GUEST-RESTRICT` restricts guest network access.

The ACL prevents VLAN 40 guest traffic from accessing internal user, server, management, and remote internal networks.

**ACL Rules:**

| Source Network | Destination Network | Action |
|---|---|---|
| 10.10.40.0/24 | 10.10.10.0/24 | Deny |
| 10.10.40.0/24 | 10.10.20.0/24 | Deny |
| 10.10.40.0/24 | 10.10.30.0/24 | Deny |
| 10.10.40.0/24 | 10.20.10.0/24 | Deny |
| 10.10.40.0/24 | 10.20.20.0/24 | Deny |
| 10.10.40.0/24 | 10.20.30.0/24 | Deny |
| 10.10.40.0/24 | 10.20.40.0/24 | Deny |
| 10.10.40.0/24 | 10.20.99.0/24 | Deny |
| 10.10.40.0/24 | Any | Permit |

**ACL Verification:**

```text

show access-lists
```
---

## Verification

The following network features were verified during the lab implementation.

| Feature | Status |
|---|---|
| VLAN Configuration | ✅ Verified |
| 802.1Q Trunking | ✅ Verified |
| EtherChannel / LACP | ✅ Verified |
| STP / Rapid-PVST | ✅ Verified |
| Inter-VLAN Routing | ✅ Verified |
| OSPF Dynamic Routing | ✅ Verified |
| DHCP | ✅ Verified |
| SSH Version 2 | ✅ Verified |
| Guest ACL | ✅ Verified |
| End-to-End Connectivity | ✅ Verified |
| Network Troubleshooting | ✅ Verified |

### Common Verification Commands

```text
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
show ip interface brief
show ip route
show ip ospf neighbor
show ip dhcp binding
show access-lists
show ip ssh
```
---

## How to Use This Lab

1. Install Cisco Packet Tracer.
2. Open the `labs/` directory.
3. Download `enterprise-network-lab.pkt`.
4. Open the `.pkt` file using Cisco Packet Tracer.
5. Review the network topology.
6. Review the device configurations in the `configs/` directory.
7. Use the verification commands in this README to validate the network.
8. Review the `verification/` directory for verification results.
9. Review the `documentation/` directory for detailed technical documentation.


---

## Project Resources

| Resource | Description |
|---|---|
| Network Topology | Enterprise network topology diagram |
| Packet Tracer Lab | Complete Cisco Packet Tracer lab |
| Device Configurations | Cisco IOS device configurations |
| Documentation | Detailed technical documentation |
| Verification | Network verification results |
| Troubleshooting | Network troubleshooting procedures |
| IP Addressing Plan | VLAN and IP addressing information |
| Network Security | ACL and security documentation |

---

## Repository Structure

```text
enterprise-network-lab/
│
├── configs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   ├── CORE-SW2.txt
│   ├── R1-EDGE.txt
│   ├── R2-CORE.txt
│   └── README.md
│
├── diagrams/
│   ├── README.md
│   └── enterprise-network-topology.png
│
├── documentation/
│   ├── README.md
│   ├── ip-addressing-plan.md
│   ├── network-design.md
│   ├── network-requirements.md
│   ├── network-security.md
│   ├── network-services.md
│   ├── project-summary.md
│   ├── routing.md
│   ├── switching.md
│   ├── troubleshooting.md
│   ├── verification-and-testing.md
│   └── vlan-design.md
│
├── labs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   ├── CORE-SW2.txt
│   ├── R1-EDGE.txt
│   ├── R2-CORE.txt
│   └── enterprise-network-lab.pkt
│
├── topology/
│   └── enterprise-network-topology.png
│
├── verification/
│   ├── README.md
│   └── verification-summary.md
│
├── LICENSE
└── README.md
```

---

## Skills Demonstrated

- Enterprise LAN/WAN Network Design
- Cisco Catalyst Switching
- Layer 2 Switching
- Layer 3 Switching
- VLAN Segmentation
- 802.1Q Trunking
- STP / Rapid-PVST
- EtherChannel / LACP
- Inter-VLAN Routing
- OSPF Dynamic Routing
- DHCP Configuration
- SSH Device Management
- IPv4 ACL Configuration
- Network Security
- Network Troubleshooting
- Cisco IOS Configuration
- Network Verification

---

## Project Status

**Status: Completed**

The enterprise networking lab has been implemented and documented using Cisco Packet Tracer.

The project includes:

- Network topology design
- VLAN segmentation
- 802.1Q trunking
- EtherChannel / LACP
- STP / Rapid-PVST
- Inter-VLAN routing
- OSPF dynamic routing
- DHCP
- SSH
- ACL-based security
- End-to-end connectivity testing
- Network troubleshooting
- Network verification
- Cisco IOS configuration documentation

---

## Author

**Ashutosh Vikhe**

**Senior Engineer - Digital Network & Security**

Network & Security Engineer focused on:

- Enterprise Networking
- Cisco Switching & Routing
- Cisco ISE
- Network Security
- Network Automation

### Connect

- GitHub: [Ashutosh Vikhe](https://github.com/Ashutoshvikhe)
- LinkedIn: [Ashutosh Vikhe](https://www.linkedin.com/in/ashutosh-vikhe-1aa829201/)


    



