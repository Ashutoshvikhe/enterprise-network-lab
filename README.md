
# Enterprise Network Lab

![Cisco](https://img.shields.io/badge/Cisco-IOS-blue)
![Packet Tracer](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue)
![OSPF](https://img.shields.io/badge/Routing-OSPF-orange)
![VLAN](https://img.shields.io/badge/Switching-VLAN-green)
![License](https://img.shields.io/badge/License-MIT-yellow)

A practical Cisco enterprise networking lab demonstrating enterprise LAN/WAN design, VLAN segmentation, inter-VLAN routing, STP, EtherChannel, OSPF, DHCP, SSH, ACLs, troubleshooting, and network verification.

## Project Overview

This project simulates a small enterprise network environment using Cisco Packet Tracer.

## Quick Navigation

- [Project Overview](#project-overview)
- [Project Demo](#project-demo)
- [Network Architecture](#network-architecture)
- [VLAN Design](#vlan-design)
- [Routing](#routing)
- [Switching](#switching)
- [Network Security](#network-security)
- [Verification](#verification)
- [Skills Demonstrated](#skills-demonstrated)
- [How to Use This Lab](#how-to-use-this-lab)
- [Repository Structure](#repository-structure)
- [Author](#author)

## Project Resources

| Resource | Description |
|---|---|
| [Network Topology](topology/enterprise-network-topology.png) | Enterprise network topology diagram |
| [Packet Tracer Lab](labs/enterprise-network-lab.pkt) | Complete Cisco Packet Tracer lab |
| [Device Configurations](configs/) | Cisco IOS configurations |
| [Documentation](documentation/) | Network design and technical documentation |
| [Verification](verification/) | Network verification results |
| [Troubleshooting](documentation/troubleshooting.md) | Troubleshooting procedures and commands |
| [IP Addressing Plan](documentation/ip-addressing-plan.md) | VLAN and IP addressing information |
| [Network Security](documentation/network-security.md) | ACL and network security configuration |


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
- Network troubleshooting and verification

  
## Project Demo

The following diagram shows the enterprise network topology implemented in Cisco Packet Tracer.
![Enterprise Network Topology](topology/enterprise-network-topology.png)


## Network Architecture

The lab follows a hierarchical enterprise network architecture consisting of an edge router, core Layer 3 switches, access switches, and end devices.

### Network Flow


                         ┌─────────────┐
                         │  R1-EDGE    │
                         │ Edge Router │
                         └──────┬──────┘
                                │
                             OSPF
                                │
                         ┌──────┴──────┐
                         │  R2-CORE    │
                         │ Core Router │
                         └──────┬──────┘
                                │
                    ┌───────────┴───────────┐
                    │                       │
             ┌──────▼──────┐         ┌──────▼──────┐
             │  CORE-SW1   │=========│  CORE-SW2   │
             │ Layer 3 SW  │ LACP    │ Layer 3 SW  │
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
Network Technologies
VLAN segmentation
802.1Q trunking
Inter-VLAN routing
Rapid-PVST / STP
EtherChannel using LACP
OSPF dynamic routing
DHCP
SSH version 2
Extended IPv4 ACLs



## VLAN Design

The network uses VLAN segmentation to separate users, servers, management traffic, and guest traffic.

| VLAN ID | Name | Network | Default Gateway | Purpose |
|---:|---|---|---|---|
| 10 | USERS | 10.10.10.0/24 | 10.10.10.1 | Internal user endpoints |
| 20 | SERVERS | 10.10.20.0/24 | 10.10.20.1 | Server infrastructure |
| 30 | MANAGEMENT | 10.10.30.0/24 | 10.10.30.1 | Network/device management |
| 40 | GUEST | 10.10.40.0/24 | 10.10.40.1 | Guest endpoints |
| 99 | NATIVE | 10.10.99.0/24 | 10.10.99.1 | Management and native VLAN |

### VLAN Security

Guest traffic from VLAN 40 is restricted from accessing internal networks using the `GUEST-RESTRICT` extended ACL.

The ACL blocks access from:

- USERS — `10.10.10.0/24`
- SERVERS — `10.10.20.0/24`
- MANAGEMENT — `10.10.30.0/24`
- Remote internal VLANs — `10.20.10.0/24` through `10.20.99.0/24`

Guest traffic that is not destined for these internal networks is permitted.

### Trunk Configuration

The access-to-core Port-channel trunks use:

- 802.1Q encapsulation
- Native VLAN 99
- Allowed VLANs 10, 20, 30, 40, and 99

## Routing

OSPF is used as the dynamic routing protocol between the core network and upstream router.

The core switches advertise their connected VLAN networks through OSPF.

OSPF adjacency was verified using:
show ip ospf neighbor
Switching
VLANs

VLANs 10, 20, 30, 40, and 99 are configured across the switching infrastructure.

802.1Q Trunking

802.1Q trunking is configured on the Port-channel uplinks.

Allowed VLANs:

10,20,30,40,99

Native VLAN:

99
EtherChannel

LACP EtherChannel is used to provide redundancy and increased bandwidth.

Configured Port-channels:

CORE-SW1 — Po1
CORE-SW2 — Po2
ACCESS-SW1 — Po1
ACCESS-SW2 — Po2

Verification:

show etherchannel summary
Spanning Tree

Rapid-PVST / STP is enabled to prevent Layer 2 switching loops.

The core switches act as STP root bridges for their respective VLANs.

Verification:

show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
DHCP

DHCP was configured to provide IP addresses to end devices.

DHCP bindings were verified on the core switches using:

show ip dhcp binding
Network Security

An extended ACL named GUEST-RESTRICT is configured to restrict guest network access.

The guest VLAN is prevented from accessing internal user, server, management, and remote internal networks.

ACL verification:

show access-lists
SSH Management

SSH version 2 is enabled on the network devices for secure remote management.

Verification:

show ip ssh

Expected result:

SSH Enabled - version 2.0
Verification

The following features have been verified:

Feature	Status
VLANs	✅ Verified
802.1Q Trunking	✅ Verified
EtherChannel / LACP	✅ Verified
STP	✅ Verified
Inter-VLAN Routing	✅ Verified
OSPF	✅ Verified
DHCP	✅ Verified
SSH v2	✅ Verified
ACL	✅ Verified
End-to-End Connectivity	✅ Verified
Repository Structure
enterprise-network-lab/
│
├── configs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   └── CORE-SW2.txt
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
│   └── enterprise-network-lab.pkt
│
├── topology/
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
STP / Rapid-PVST
EtherChannel
LACP
OSPF
DHCP
SSH
IPv4 ACL
Inter-VLAN Routing
Network Troubleshooting

## Skills Demonstrated

- Enterprise LAN/WAN Network Design
- Cisco Catalyst Switching
- Layer 2 Switching
- Layer 3 Switching
- VLAN Segmentation
- 802.1Q Trunking
- Rapid-PVST / STP
- EtherChannel / LACP
- Inter-VLAN Routing
- OSPF Dynamic Routing
- DHCP
- SSH Device Management
- IPv4 ACLs
- Network Security
- Network Troubleshooting
- Cisco IOS Configuration
- Network Verification

This project demonstrates practical skills in:

Enterprise LAN/WAN networking
Cisco switching
Layer 3 switching
Routing and switching troubleshooting
VLAN segmentation
Network security
Dynamic routing
Network management
Connectivity verification
Cisco IOS configuration
Lab Platform

## Lab Platform
Cisco Packet Tracer
## How to Use This Lab

1. Install Cisco Packet Tracer.
2. Download `enterprise-network-lab.pkt` from the `labs/` directory.
3. Open the `.pkt` file in Cisco Packet Tracer.
4. Review the network topology and device configurations.
5. Use the configuration files in the `configs/` directory as reference.
6. Use the `verification/` directory to review network verification results.
7. Use the `documentation/` directory for detailed design, routing, switching, security, and troubleshooting information.

## Lab Platform

Cisco Packet Tracer

Project Status
Completed

The lab configuration, connectivity testing, security controls, troubleshooting activities, and verification activities have been implemented and documented.


## Author

**Ashutosh Vikhe**

Network & Security Engineer focused on Enterprise Networking, Cisco Switching & Routing, Cisco ISE, Network Security, and Network Automation.

### Connect
- GitHub: [Ashutoshvikhe](https://github.com/Ashutoshvikhe)
- LinkedIn: https://www.linkedin.com/in/ashutosh-vikhe-1aa829201/
