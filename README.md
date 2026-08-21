# Enterprise Network Lab

![Cisco](https://img.shields.io/badge/Cisco-IOS-blue)
![Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![OSPF](https://img.shields.io/badge/Routing-OSPF-orange)
![VLAN](https://img.shields.io/badge/Switching-VLAN-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

A practical Cisco enterprise networking lab demonstrating enterprise LAN/WAN design, VLAN segmentation, inter-VLAN routing, STP, EtherChannel, OSPF, DHCP, SSH, ACLs, troubleshooting, and network verification using Cisco Packet Tracer.

---

## Project Overview

This project simulates a small enterprise network environment using Cisco Packet Tracer.

The lab demonstrates:

- Enterprise network topology design
- VLAN segmentation
- 802.1Q trunking
- Inter-VLAN routing
- Rapid-PVST / STP
- EtherChannel using LACP
- OSPF dynamic routing
- DHCP services
- SSH device management
- Extended ACL security
- End-to-end connectivity testing
- Network troubleshooting
- Network verification

---

## Quick Navigation

- [Project Demo](#project-demo)
- [Network Architecture](#network-architecture)
- [VLAN Design](#vlan-design)
- [Switching](#switching)
- [Routing](#routing)
- [Network Services](#network-services)
- [Network Security](#network-security)
- [Verification](#verification)
- [Skills Demonstrated](#skills-demonstrated)
- [How to Use This Lab](#how-to-use-this-lab)
- [Repository Structure](#repository-structure)
- [Project Status](#project-status)
- [Author](#author)

---

## Project Demo

The following diagram shows the enterprise network topology implemented in Cisco Packet Tracer.

![Enterprise Network Topology](topology/enterprise-network-topology.png)

---

## Network Architecture

The lab follows a hierarchical enterprise network architecture consisting of an edge router, core Layer 3 switches, access Layer 2 switches, and end devices.

### Network Flow

                         ┌─────────────┐
                         │  R1-EDGE    │
                         │ Edge Router │
                         └──────┬──────┘
                                │
                             OSPF
                                │
                         ┌──────▼──────┐
                         │  R2-CORE    │
                         │ Core Router │
                         └──────┬──────┘
                                │
                    ┌───────────┴───────────┐
                    │                       │
             ┌──────▼──────┐         ┌──────▼──────┐
             │  CORE-SW1   │         │  CORE-SW2   │
             │ Layer 3 SW  │         │ Layer 3 SW  │
             └──────┬──────┘         └──────┬──────┘
                    │                       │
              EtherChannel             EtherChannel
                    │                       │
             ┌──────▼──────┐         ┌──────▼──────┐
             │ ACCESS-SW1  │         │ ACCESS-SW2  │
             │ Layer 2 SW  │         │ Layer 2 SW  │
             └──────┬──────┘         └──────┬──────┘
                    │                       │
             ┌──────┴──────┐         ┌──────┴──────┐
             │ User PCs    │         │ User PCs    │
             │ Server      │         │ Guest       │
             └─────────────┘         └─────────────┘


Network Components
Device	Role
R1-EDGE	Enterprise edge / upstream router
R2-CORE	Core routing device
CORE-SW1	Layer 3 core switch
CORE-SW2	Layer 3 core switch
ACCESS-SW1	Access Layer switch
ACCESS-SW2	Access Layer switch
User PCs	Internal user endpoints
Server	Enterprise server endpoint
Guest Endpoint	Guest network endpoint
VLAN Design

The network uses VLAN segmentation to separate users, servers, management traffic, and guest traffic.

VLAN ID	Name	Network	Default Gateway	Purpose
10	USERS	10.10.10.0/24	10.10.10.1	Internal user endpoints
20	SERVERS	10.10.20.0/24	10.10.20.1	Server infrastructure
30	MANAGEMENT	10.10.30.0/24	10.10.30.1	Network/device management
40	GUEST	10.10.40.0/24	10.10.40.1	Guest endpoints
99	NATIVE	10.10.99.0/24	10.10.99.1	Management and native VLAN
VLAN Security

Guest traffic from VLAN 40 is restricted from accessing internal networks using the GUEST-RESTRICT extended ACL.

The ACL blocks access from:

USERS — 10.10.10.0/24
SERVERS — 10.10.20.0/24
MANAGEMENT — 10.10.30.0/24
Remote internal networks — 10.20.10.0/24 through 10.20.99.0/24

Guest traffic that is not destined for these internal networks is permitted.

Switching
VLANs

VLANs 10, 20, 30, 40, and 99 are configured across the switching infrastructure.

802.1Q Trunking

802.1Q trunking is configured on the Port-channel uplinks.

Allowed VLANs:

10,20,30,40,99

Native VLAN:

99

Verification:

show interfaces trunk
EtherChannel

LACP EtherChannel is used to provide link redundancy and increased bandwidth between network devices.

Configured Port-channels:

Device	Port-channel	Member Interfaces
CORE-SW1	Po1	Fa0/2, Fa0/5
CORE-SW2	Po2	Fa0/2, Fa0/5
ACCESS-SW1	Po1	Fa0/1, Fa0/4
ACCESS-SW2	Po2	Fa0/1, Fa0/4

The EtherChannels operate as Layer 2 port-channels.

EtherChannel Verification
show etherchannel summary

Expected status:

Po1(SU)
Po2(SU)

Member interfaces should show:

(P)

where P indicates that the interface is successfully bundled into the Port-channel.

Spanning Tree

Rapid-PVST / STP is enabled to prevent Layer 2 switching loops.

CORE-SW1 is the STP root bridge for VLANs 10, 20, 30, and 40 in the documented topology.

Verification:

show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
Routing

OSPF is used as the dynamic routing protocol between the core network and the upstream routing infrastructure.

OSPF Design
Device	Router ID	Role
R2-CORE	3.3.3.3	OSPF routing hub
CORE-SW1	1.1.1.1	Layer 3 core
CORE-SW2	2.2.2.2	Layer 3 core
OSPF Networks

The core routing infrastructure uses /30 point-to-point networks:

Link	Network
R2-CORE ↔ CORE-SW1	10.10.101.0/30
R2-CORE ↔ CORE-SW2	10.10.102.0/30

The core switches advertise their connected VLAN networks through OSPF.

OSPF Verification
show ip ospf neighbor

The OSPF adjacency was successfully established between the routing devices.

Routing tables were verified using:

show ip route

OSPF-learned routes are identified with:

O
Inter-VLAN Routing

Layer 3 switching is used to provide gateway interfaces for the VLANs.

CORE-SW1
VLAN 10 → 10.10.10.1/24
VLAN 20 → 10.10.20.1/24
VLAN 30 → 10.10.30.1/24
VLAN 40 → 10.10.40.1/24
VLAN 99 → 10.10.99.1/24
CORE-SW2
VLAN 10 → 10.20.10.1/24
VLAN 20 → 10.20.20.1/24
VLAN 30 → 10.20.30.1/24
VLAN 40 → 10.20.40.1/24
VLAN 99 → 10.20.99.1/24

Verification:

show ip interface brief
show ip route
Network Services
DHCP

DHCP was configured to provide IP addresses to end devices.

DHCP bindings were verified on the core switches using:

show ip dhcp binding

Verified DHCP clients include endpoints in the user VLANs.

SSH Management

SSH version 2 is enabled on the network devices for secure remote management.

Verification:

show ip ssh

Expected result:

SSH Enabled - version 2.0
Network Security
Guest Network ACL

An extended ACL named GUEST-RESTRICT is configured to restrict guest network access.

The ACL prevents VLAN 40 guest traffic from accessing internal user, server, management, and remote internal networks.

ACL verification:

show access-lists

Example ACL structure:

deny ip 10.10.40.0 0.0.0.255 10.10.10.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.10.20.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.10.30.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.20.10.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.20.20.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.20.30.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.20.40.0 0.0.0.255
deny ip 10.10.40.0 0.0.0.255 10.20.99.0 0.0.0.255
permit ip 10.10.40.0 0.0.0.255 any
Verification

The following network features were verified during the lab implementation:

Feature	Status
VLANs	✅ Verified
802.1Q Trunking	✅ Verified
EtherChannel / LACP	✅ Verified
STP / Rapid-PVST	✅ Verified
Inter-VLAN Routing	✅ Verified
OSPF	✅ Verified
DHCP	✅ Verified
SSH v2	✅ Verified
Guest ACL	✅ Verified
End-to-End Connectivity	✅ Verified
Common Verification Commands
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

Detailed verification information is available in:

verification/

How to Use This Lab
Install Cisco Packet Tracer.
Download enterprise-network-lab.pkt from the labs/ directory.
Open the .pkt file in Cisco Packet Tracer.
Review the network topology and device configurations.
Use the files in configs/ as Cisco IOS configuration references.
Review the verification/ directory for verification results.
Review the documentation/ directory for detailed technical documentation.
Project Resources
Resource	Description
Network Topology	Enterprise network topology diagram
Packet Tracer Lab	Complete Cisco Packet Tracer lab
Device Configurations	Cisco IOS configurations
Documentation	Detailed technical documentation
Verification	Network verification results
Troubleshooting	Troubleshooting procedures
IP Addressing Plan	VLAN and IP addressing information
Network Security	ACL and network security documentation
Repository Structure
enterprise-network-lab/
│
├── configs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   ├── CORE-SW2.txt
│   ├── R1-EDGE.txt
│   └── R2-CORE.txt
│
├── documentation/
│   └── Technical documentation
│
├── labs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   ├── CORE-SW2.txt
│   ├── R1-EDGE.txt
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
Key Technologies
Cisco IOS
Cisco Packet Tracer
VLAN
802.1Q
STP / Rapid-PVST
EtherChannel
LACP
OSPF
DHCP
SSH
IPv4 ACL
Inter-VLAN Routing
Network Troubleshooting
Skills Demonstrated
Enterprise LAN/WAN Network Design
Cisco Catalyst Switching
Layer 2 Switching
Layer 3 Switching
VLAN Segmentation
802.1Q Trunking
Rapid-PVST / STP
EtherChannel / LACP
Inter-VLAN Routing
OSPF Dynamic Routing
DHCP
SSH Device Management
IPv4 ACLs
Network Security
Network Troubleshooting
Cisco IOS Configuration
Network Verification
Project Status

Completed

The lab configuration, connectivity testing, security controls, troubleshooting activities, and verification activities have been implemented and documented using Cisco Packet Tracer.

Author

Ashutosh Vikhe
SENIOR ENGINEER - DIGITAL NETWORK & SECURITY focused on Enterprise Networking, Cisco Switching & Routing, Cisco ISE, Network Security, and Network Automation.

Connect
GitHub: https://github.com/Ashutoshvikhe
LinkedIn: https://www.linkedin.com/in/ashutosh-vikhe-1aa829201/
