

# VLAN Design

## 1. Overview

VLANs are used to logically segment the enterprise network into separate broadcast domains.

The lab uses five VLANs:

- VLAN 10 — USERS
- VLAN 20 — SERVERS
- VLAN 30 — MANAGEMENT
- VLAN 40 — GUEST
- VLAN 99 — NATIVE

The VLAN structure is implemented across the core and access switching infrastructure.

---

## 2. VLAN Table

| VLAN ID | VLAN Name | Purpose |
|---:|---|---|
| 10 | USERS | End-user devices |
| 20 | SERVERS | Server devices |
| 30 | MANAGEMENT | Management devices and management traffic |
| 40 | GUEST | Guest devices |
| 99 | NATIVE | Native VLAN and network management |

---

## 3. VLAN 10 — USERS

### Purpose

VLAN 10 is used for normal end-user devices.

### Site 1
text
Network: 10.10.10.0/24
Gateway: 10.10.10.1

Site 2
Network: 10.20.10.0/24
Gateway: 10.20.10.1
Access Ports

ACCESS-SW1:

Fa0/2
Fa0/3

ACCESS-SW2:

Fa0/2
Fa0/3
4. VLAN 20 — SERVERS
Site 1
Network: 10.10.20.0/24
Gateway: 10.10.20.1
Site 2
Network: 10.20.20.0/24
Gateway: 10.20.20.1

ACCESS-SW2:

Fa0/5
Fa0/6
5. VLAN 30 — MANAGEMENT
Site 1
Network: 10.10.30.0/24
Gateway: 10.10.30.1
Site 2
Network: 10.20.30.0/24
Gateway: 10.20.30.1

ACCESS-SW2:

Fa0/7
Fa0/8
6. VLAN 40 — GUEST
Site 1
Network: 10.10.40.0/24
Gateway: 10.10.40.1
Site 2
Network: 10.20.40.0/24
Gateway: 10.20.40.1

ACCESS-SW1:

Fa0/5

ACCESS-SW2:

Fa0/9
Fa0/10
7. VLAN 99 — NATIVE / MANAGEMENT

VLAN 99 is configured as the native VLAN on trunk links and is also used for network device management.

Site 1
Network: 10.10.99.0/24
CORE-SW1: 10.10.99.1
ACCESS-SW1: 10.10.99.11
Site 2
Network: 10.20.99.0/24
CORE-SW2: 10.20.99.1
ACCESS-SW2: 10.20.99.11
8. Trunk Configuration
ACCESS-SW1
Port-channel: Po1
Protocol: LACP
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
ACCESS-SW2
Port-channel: Po2
Protocol: LACP
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
9. EtherChannel Members
ACCESS-SW1
Po1
Fa0/1
Fa0/4
ACCESS-SW2
Po2
Fa0/1
Fa0/4

Both EtherChannels are operating using LACP.

10. Spanning Tree

STP is enabled to prevent Layer 2 switching loops.

CORE-SW1 is the STP root for VLANs 10, 20, 30 and 40 in the verified configuration.

The access switches use their EtherChannel uplinks as root forwarding ports.

PortFast is configured on appropriate end-host access ports.

11. Guest VLAN Security

Guest VLAN 40 is restricted using the GUEST-RESTRICT extended ACL.

The ACL blocks guest traffic from accessing:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.20.10.0/24
10.20.20.0/24
10.20.30.0/24
10.20.40.0/24
10.20.99.0/24

Other guest traffic is permitted.

The ACL is applied inbound on VLAN 40.

12. VLAN Verification Commands
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
show ip interface brief
13. VLAN Design Summary

The VLAN design provides:

Logical network segmentation
Separate broadcast domains
Dedicated user and server networks
Dedicated guest network
Dedicated management infrastructure
Controlled trunk VLAN propagation
LACP EtherChannel redundancy
STP loop prevention
Guest network isolation
