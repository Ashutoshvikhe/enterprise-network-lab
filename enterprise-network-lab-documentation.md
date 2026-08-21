
# Enterprise Network Lab — Technical Documentation

## 1. Project Overview

This project simulates a small enterprise network environment using Cisco Packet Tracer.

The lab demonstrates enterprise LAN/WAN design, VLAN segmentation, 802.1Q trunking, inter-VLAN routing, Rapid-PVST/STP, EtherChannel using LACP, OSPF dynamic routing, DHCP, SSH device management, extended ACL security, troubleshooting, and network verification.

---

## 2. Network Architecture

The network consists of:

- R1-EDGE
- R2-CORE
- CORE-SW1
- CORE-SW2
- ACCESS-SW1
- ACCESS-SW2
- User PCs
- Server
- Guest endpoint

### Network Flow

R1-EDGE
   |
 OSPF
   |
R2-CORE
   |
   +-------------------+
   |                   |
CORE-SW1           CORE-SW2
   |                   |
ACCESS-SW1         ACCESS-SW2
   |                   |
User PCs            User PCs
Server              Guest

3. VLAN Design
VLAN ID	Name	Network	Gateway	Purpose
10	USERS	10.10.10.0/24	10.10.10.1	Internal users
20	SERVERS	10.10.20.0/24	10.10.20.1	Server infrastructure
30	MANAGEMENT	10.10.30.0/24	10.10.30.1	Network management
40	GUEST	10.10.40.0/24	10.10.40.1	Guest endpoints
99	NATIVE	10.10.99.0/24	10.10.99.1	Native/management VLAN
4. IP Addressing
CORE-SW1
VLAN10    10.10.10.1/24
VLAN20    10.10.20.1/24
VLAN30    10.10.30.1/24
VLAN40    10.10.40.1/24
VLAN99    10.10.99.1/24
CORE-SW2
VLAN10    10.20.10.1/24
VLAN20    10.20.20.1/24
VLAN30    10.20.30.1/24
VLAN40    10.20.40.1/24
VLAN99    10.20.99.1/24
Point-to-Point Routing Links
R2-CORE ↔ CORE-SW1
10.10.101.0/30


R2-CORE ↔ CORE-SW2
10.10.102.0/30
5. Switching
VLANs

VLANs 10, 20, 30, 40, and 99 are configured across the switching infrastructure.

802.1Q Trunking

The Port-channel uplinks use 802.1Q trunking.

Allowed VLANs:

10,20,30,40,99

Native VLAN:

99

Verification command:

show interfaces trunk
6. EtherChannel

LACP EtherChannel is used to provide link redundancy and increased bandwidth.

Configured Port-channels
Device	Port-channel	Member Interfaces
CORE-SW1	Po1	Fa0/2, Fa0/5
CORE-SW2	Po2	Fa0/2, Fa0/5
ACCESS-SW1	Po1	Fa0/1, Fa0/4
ACCESS-SW2	Po2	Fa0/1, Fa0/4

The EtherChannels operate as Layer 2 port-channels.

Verification
show etherchannel summary

Expected status:

Po1(SU)
Po2(SU)

Member interfaces should show:

(P)
7. Spanning Tree

Rapid-PVST/STP is enabled to prevent Layer 2 switching loops.

CORE-SW1 acts as the STP root bridge for the documented VLANs.

Verification
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40

Expected forwarding state:

FWD
8. Inter-VLAN Routing

Layer 3 switching provides gateway interfaces for the VLANs.

CORE-SW1 provides:

10.10.10.1
10.10.20.1
10.10.30.1
10.10.40.1
10.10.99.1

CORE-SW2 provides:

10.20.10.1
10.20.20.1
10.20.30.1
10.20.40.1
10.20.99.1
Verification
show ip interface brief
show ip route
9. OSPF Routing

OSPF is used as the dynamic routing protocol between the core network and upstream routing infrastructure.

OSPF Router

R2-CORE:

Router ID: 3.3.3.3
OSPF Point-to-Point Networks
10.10.101.0/30
10.10.102.0/30

The core switches advertise their connected VLAN networks through OSPF.

Verification
show ip ospf neighbor

The OSPF neighbor relationship was verified in the FULL state.

Routing tables were verified using:

show ip route

OSPF routes are identified with:

O
10. DHCP

DHCP was configured to provide IP addresses to end devices.

Verification
show ip dhcp binding

Verified DHCP bindings included user VLAN endpoints on both core switches.

11. SSH Management

SSH version 2 is enabled on the network devices for secure remote management.

Verification
show ip ssh

Expected result:

SSH Enabled - version 2.0

VTY access is configured to use:

login local
transport input ssh
12. Network Security
Guest Network ACL

An extended ACL named:

GUEST-RESTRICT

is configured to restrict guest network access.

The guest VLAN is prevented from accessing internal user, server, management, and remote internal networks.

ACL Rules
deny 10.10.40.0/24 → 10.10.10.0/24
deny 10.10.40.0/24 → 10.10.20.0/24
deny 10.10.40.0/24 → 10.10.30.0/24
deny 10.10.40.0/24 → 10.20.10.0/24
deny 10.10.40.0/24 → 10.20.20.0/24
deny 10.10.40.0/24 → 10.20.30.0/24
deny 10.10.40.0/24 → 10.20.40.0/24
deny 10.10.40.0/24 → 10.20.99.0/24
permit 10.10.40.0/24 → any
Verification
show access-lists
13. Network Verification

The following features were verified:

Feature	Status
VLANs	Verified
802.1Q Trunking	Verified
EtherChannel / LACP	Verified
STP / Rapid-PVST	Verified
Inter-VLAN Routing	Verified
OSPF	Verified
DHCP	Verified
SSH v2	Verified
Guest ACL	Verified
End-to-End Connectivity	Verified
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
14. Troubleshooting

The lab includes troubleshooting and verification of:

VLAN membership
Trunk configuration
EtherChannel status
STP state
Interface status
IP addressing
Routing tables
OSPF neighbor relationships
DHCP bindings
SSH configuration
ACL behavior
End-to-end connectivity

Useful troubleshooting commands include:

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
15. Device Configuration Files

Complete Cisco IOS configurations are available in:

configs/

Devices:

ACCESS-SW1.txt
ACCESS-SW2.txt
CORE-SW1.txt
CORE-SW2.txt
R1-EDGE.txt
R2-CORE.txt
16. Packet Tracer Lab

The complete Cisco Packet Tracer project is available in:

labs/enterprise-network-lab.pkt

The lab can be opened directly using Cisco Packet Tracer.

17. Topology

The network topology diagram is available in:

topology/enterprise-network-topology.png
18. Technologies Used
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
19. Skills Demonstrated
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
20. Project Status

Completed

The lab configuration, connectivity testing, security controls, troubleshooting activities, and verification activities have been implemented and documented using Cisco Packet Tracer.


