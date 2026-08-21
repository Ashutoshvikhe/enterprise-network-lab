# Enterprise Network Lab

A multi-site enterprise network lab built using Cisco Packet Tracer, demonstrating enterprise switching, routing, network services, security, and troubleshooting.

## Project Overview

This project simulates a structured enterprise network with two core switches and two access switches.

The lab demonstrates:

- Enterprise VLAN design
- Inter-VLAN routing
- OSPF dynamic routing
- LACP EtherChannel
- Spanning Tree Protocol
- DHCP
- SSH management
- Guest network security using ACLs
- Network troubleshooting and verification

## Network Topology

The network consists of:

- CORE-SW1
- CORE-SW2
- ACCESS-SW1
- ACCESS-SW2
- User PCs
- Server devices

## VLAN Design

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | USERS | End-user devices |
| 20 | SERVERS | Server devices |
| 30 | MANAGEMENT | Management devices |
| 40 | GUEST | Guest devices |
| 99 | NATIVE | Native and management traffic |

## IP Addressing

### Site 1

| VLAN | Network | Gateway |
|---:|---|---|
| 10 | 10.10.10.0/24 | 10.10.10.1 |
| 20 | 10.10.20.0/24 | 10.10.20.1 |
| 30 | 10.10.30.0/24 | 10.10.30.1 |
| 40 | 10.10.40.0/24 | 10.10.40.1 |
| 99 | 10.10.99.0/24 | 10.10.99.1 |

### Site 2

| VLAN | Network | Gateway |
|---:|---|---|
| 10 | 10.20.10.0/24 | 10.20.10.1 |
| 20 | 10.20.20.0/24 | 10.20.20.1 |
| 30 | 10.20.30.0/24 | 10.20.30.1 |
| 40 | 10.20.40.0/24 | 10.20.40.1 |
| 99 | 10.20.99.0/24 | 10.20.99.1 |

## Switching

The switching infrastructure uses:

- VLAN segmentation
- Access ports
- 802.1Q trunking
- Native VLAN 99
- LACP EtherChannel
- Spanning Tree Protocol
- PortFast

### EtherChannel

ACCESS-SW1:

Po1
Fa0/1
Fa0/4
LACP

ACCESS-SW2:

Po2
Fa0/1
Fa0/4
LACP
Routing

Inter-VLAN routing is provided using Layer 3 SVIs.

OSPF is used for dynamic routing between the core switches.

OSPF Transit Networks
10.10.101.0/30
10.10.102.0/30

OSPF neighbor relationships were verified in the FULL/BDR state.

Network Services

The project includes:

DHCP
SSH Version 2
Local VTY authentication
Management VLAN
Network Security

Security controls include:

SSH-only remote management
Local authentication
Dedicated management VLAN
Guest network isolation
Extended ACL
PortFast on appropriate end-host ports
Guest ACL

The GUEST-RESTRICT ACL restricts guest traffic from accessing selected internal networks.

Guest network:

10.10.40.0/24
Verification

The following Cisco IOS commands were used during implementation and verification:

show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
show ip interface brief
show ip route
show ip ospf neighbor
show ip dhcp binding
show ip ssh
show access-lists
Documentation

Detailed project documentation is available in the documentation directory:

Network Requirements
IP Addressing Plan
VLAN Design
Switching
Routing
Network Services
Network Security
Verification and Testing
Troubleshooting
Project Summary
Skills Demonstrated
Cisco IOS
Enterprise Networking
Cisco Switching
VLANs
802.1Q Trunking
EtherChannel
LACP
STP
Inter-VLAN Routing
OSPF
DHCP
SSH
ACL
Network Security
Network Troubleshooting
Network Documentation
Project Status

Completed

The network configuration, security controls, routing, switching, services, verification, and documentation have been implemented and verified.

#### Author
Ashutosh Vikhe
SENIOR ENGINEER - DIGITAL NETWORK & SECURITY

Core areas:
Enterprise Networking
Cisco Switching & Routing
Network Security
Cisco ISE / NAC
Network Automation
Cloud & DevOps

