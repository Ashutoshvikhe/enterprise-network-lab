
# Network Requirements

## 1. Project Objective

The objective of this project is to design and implement a small enterprise network using Cisco switching and routing technologies.

The lab is designed to demonstrate:

- Enterprise VLAN segmentation
- Inter-VLAN routing
- Layer 2 switching
- Layer 3 routing
- OSPF dynamic routing
- 802.1Q trunking
- LACP EtherChannel
- Spanning Tree Protocol
- DHCP
- SSHv2
- Network management
- Guest network isolation using ACLs

---

## 2. Network Architecture

The network consists of:

- 2 Layer 3 core switches
- 2 Layer 2 access switches
- Multiple VLANs
- End-user devices
- Inter-core Layer 3 connectivity
- EtherChannel uplinks between core and access switches

The core switches provide Layer 3 gateway services for their local VLANs and exchange routes using OSPF.

---

## 3. VLAN Requirements

The network uses the following VLANs:

| VLAN | Name | Purpose |
|---:|---|---|
| 10 | USERS | End-user devices |
| 20 | SERVERS | Server devices |
| 30 | MANAGEMENT | Management-related devices |
| 40 | GUEST | Guest/end-user isolated network |
| 99 | NATIVE | Native and network management VLAN |

---

## 4. IP Addressing Requirements

The network uses separate /24 networks for each VLAN.

### CORE-SW1

| VLAN | Network | Default Gateway |
|---:|---|---|
| 10 | 10.10.10.0/24 | 10.10.10.1 |
| 20 | 10.10.20.0/24 | 10.10.20.1 |
| 30 | 10.10.30.0/24 | 10.10.30.1 |
| 40 | 10.10.40.0/24 | 10.10.40.1 |
| 99 | 10.10.99.0/24 | 10.10.99.1 |

### CORE-SW2

| VLAN | Network | Default Gateway |
|---:|---|---|
| 10 | 10.20.10.0/24 | 10.20.10.1 |
| 20 | 10.20.20.0/24 | 10.20.20.1 |
| 30 | 10.20.30.0/24 | 10.20.30.1 |
| 40 | 10.20.40.0/24 | 10.20.40.1 |
| 99 | 10.20.99.0/24 | 10.20.99.1 |

---

## 5. Routing Requirements

OSPF is used as the dynamic routing protocol.

The core switches must:

- Establish an OSPF neighbor relationship
- Advertise their local VLAN networks
- Learn remote VLAN networks dynamically
- Maintain reachable routes after network changes

The core-to-core routing infrastructure uses /30 point-to-point networks.

---

## 6. Switching Requirements

The access layer must provide:

- Access VLAN assignment
- 802.1Q trunking
- LACP EtherChannel
- STP
- PortFast on end-host ports

The EtherChannel uplinks must operate as a single logical trunk.

---

## 7. DHCP Requirements

DHCP must provide IP addresses dynamically to client devices.

The DHCP configuration must provide:

- Network address
- Default gateway
- Address pool
- Excluded addresses

DHCP operation is verified using DHCP bindings on the core switches.

---

## 8. Network Security Requirements

The network must provide basic segmentation and access control.

The Guest VLAN must not be able to access internal user, server, management, or remote-site networks.

The `GUEST-RESTRICT` ACL is used to enforce this restriction.

SSH version 2 is used for secure device management.

---

## 9. Management Requirements

Network devices must have dedicated management addressing.

VLAN 99 is used for network device management.

The following management addresses are configured:

- CORE-SW1 — 10.10.99.1
- ACCESS-SW1 — 10.10.99.11
- CORE-SW2 — 10.20.99.1
- ACCESS-SW2 — 10.20.99.11

SSHv2 is enabled on the network devices to provide secure remote management.

---

## 10. Availability Requirements

EtherChannel is used on the core-to-access uplinks to provide:

- Link redundancy
- Increased logical bandwidth
- Improved uplink resilience

STP is used to prevent Layer 2 loops.

---

## 11. Verification Requirements

The completed network must be verified using Cisco IOS commands.

Required verification includes:

- VLAN status
- Trunk status
- EtherChannel status
- STP status
- Interface status
- Routing table
- OSPF neighbors
- DHCP bindings
- SSH status
- ACL counters
- End-to-end connectivity

---

## 12. Project Outcome

The completed lab should demonstrate a functioning enterprise network with:

- Segmented VLANs
- Inter-VLAN routing
- Dynamic OSPF routing
- Redundant EtherChannel uplinks
- STP protection
- DHCP services
- Secure SSH management
- Guest network isolation
- Verified end-to-end connectivity
