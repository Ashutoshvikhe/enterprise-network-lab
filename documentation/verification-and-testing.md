
# Verification and Testing

## 1. Overview

The completed enterprise network was verified using Cisco IOS show commands and connectivity testing.

## 2. VLAN Verification

VLAN configuration was verified on the access switches.

show vlan brief

Verified VLANs:

VLAN 10 — USERS
VLAN 20 — SERVERS
VLAN 30 — MANAGEMENT
VLAN 40 — GUEST
VLAN 99 — NATIVE
3. Trunk Verification

Trunk configuration was verified using:

show interfaces trunk

Verified:

Port-channel trunking is operational
802.1Q encapsulation is used
Native VLAN is 99
VLANs 10, 20, 30, 40 and 99 are allowed
All allowed VLANs are active and forwarding
4. EtherChannel Verification

EtherChannel was verified using:

show etherchannel summary
ACCESS-SW1
Po1(SU)
Fa0/1(P)
Fa0/4(P)
ACCESS-SW2
Po2(SU)
Fa0/1(P)
Fa0/4(P)

The P status confirms that the physical interfaces are successfully bundled into the EtherChannel.

5. Spanning Tree Verification

STP was verified for VLANs 10, 20, 30 and 40.

show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40

CORE-SW1 is the root bridge for the verified VLANs.

The access switches use their EtherChannel uplinks as Root Forwarding ports.

6. OSPF Verification

OSPF neighbor relationships were verified using:

show ip ospf neighbor

CORE-SW1:

Neighbor ID: 3.3.3.3
State: FULL/BDR
Address: 10.10.101.1

CORE-SW2:

Neighbor ID: 3.3.3.3
State: FULL/BDR
Address: 10.10.102.1

The FULL state confirms that OSPF adjacency is established.

7. Routing Table Verification

Routing tables were verified using:

show ip route

CORE-SW1 successfully learns the Site 2 networks through OSPF.

CORE-SW2 successfully learns the Site 1 networks through OSPF.

8. DHCP Verification

DHCP bindings were verified using:

show ip dhcp binding

CORE-SW1 verified client addresses:

10.10.10.21
10.10.10.22

CORE-SW2 verified client addresses:

10.20.10.21
10.20.10.22
9. SSH Verification

SSH was verified using:

show ip ssh

SSH version 2 is enabled on:

CORE-SW1
CORE-SW2
ACCESS-SW1
ACCESS-SW2
10. Guest ACL Verification

The guest security policy was verified using:

show access-lists

The GUEST-RESTRICT ACL contains deny statements for internal networks and a final permit statement for other IP traffic.

The ACL is applied inbound on VLAN 40.

11. Interface Verification

Interface status was verified using:

show ip interface brief

The core VLAN interfaces and routing interfaces were verified as operational.

Management interfaces were also verified:

ACCESS-SW1 VLAN 99 → 10.10.99.11
ACCESS-SW2 VLAN 99 → 10.20.99.11
12. Verification Commands Summary
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
13. Verification Summary

The following major components were successfully verified:

VLAN configuration
Access port assignment
802.1Q trunking
LACP EtherChannel
Spanning Tree Protocol
Inter-VLAN routing
OSPF routing
DHCP
SSH management
Guest network ACL
Switch management VLAN
