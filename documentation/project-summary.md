# Project Summary

## Enterprise Network Lab

This project demonstrates the design, configuration, security, routing, switching, and troubleshooting of a multi-site enterprise network using Cisco networking technologies.

## Network Components

- CORE-SW1
- CORE-SW2
- ACCESS-SW1
- ACCESS-SW2
- End-user PCs
- Server devices

## VLANs

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | USERS | End-user devices |
| 20 | SERVERS | Server devices |
| 30 | MANAGEMENT | Network management |
| 40 | GUEST | Guest devices |
| 99 | NATIVE | Native and management traffic |

## Switching Technologies

The project implements:

- VLAN segmentation
- Access ports
- 802.1Q trunking
- LACP EtherChannel
- Spanning Tree Protocol
- PortFast

## Routing Technologies

The project implements:

- Inter-VLAN routing
- Layer 3 SVI gateways
- OSPF dynamic routing
- /30 transit networks
- Routing between the two network sites

## Network Services

The project includes:

- DHCP
- SSH version 2
- Local VTY authentication
- Management VLAN

## Network Security

Security controls include:

- SSH-only remote management
- Local authentication
- Dedicated management VLAN
- Guest network isolation
- Extended ACL
- PortFast on appropriate end-host ports

## Guest Network Security

The `GUEST-RESTRICT` ACL restricts traffic from the guest network `10.10.40.0/24` to selected internal networks.

The ACL allows other IP traffic after the restricted networks are denied.

## Verification

The following technologies were verified using Cisco IOS commands:

- VLANs
- Trunking
- EtherChannel
- STP
- OSPF
- DHCP
- SSH
- ACL
- IP addressing
- Routing tables
- Interface status

## Key Verification Results

### EtherChannel

ACCESS-SW1
Po1(SU)
Fa0/1(P)
Fa0/4(P)

ACCESS-SW2
Po2(SU)
Fa0/1(P)
Fa0/4(P)

OSPF

Both core switches have established OSPF neighbor relationships with the verified state:

FULL/BDR
SSH

SSH version 2 is enabled on:

CORE-SW1
CORE-SW2
ACCESS-SW1
ACCESS-SW2
DHCP

Verified DHCP clients include:

CORE-SW1
10.10.10.21
10.10.10.22


CORE-SW2
10.20.10.21
10.20.10.22
Project Objectives

The main objectives of the project are:

Design an enterprise network topology.
Implement VLAN-based network segmentation.
Configure trunk links between switches.
Implement LACP EtherChannel.
Configure STP for Layer 2 loop prevention.
Configure inter-VLAN routing.
Implement OSPF dynamic routing.
Configure DHCP services.
Secure device management using SSH.
Implement guest network restrictions using ACLs.
Verify network connectivity and routing.
Develop a structured troubleshooting methodology.
Skills Demonstrated
Cisco IOS
Enterprise Switching
VLANs
Trunking
EtherChannel
STP
Inter-VLAN Routing
OSPF
DHCP
SSH
ACL
Network Security
Network Troubleshooting
Network Documentation
Conclusion

This lab demonstrates a complete enterprise network implementation covering switching, routing, network services, security, verification, and troubleshooting.

The project provides practical experience with designing and operating a structured Cisco enterprise network environment.
