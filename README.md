
# Enterprise Network Lab

A practical Cisco enterprise networking lab demonstrating enterprise LAN/WAN design, VLAN segmentation, inter-VLAN routing, OSPF, STP, EtherChannel, DHCP, SSH, ACL-based security, troubleshooting, and network verification.

---

## 🎯 Project Objective

The objective of this project is to design, configure, verify, troubleshoot, and document a small enterprise network using Cisco networking technologies.

The lab demonstrates practical enterprise networking concepts including:

- VLAN segmentation
- Inter-VLAN routing
- 802.1Q trunking
- Rapid-PVST
- LACP EtherChannel
- OSPF dynamic routing
- DHCP
- SSH management
- Extended ACL security
- Network troubleshooting
- End-to-end connectivity verification

---

## 🏗️ Network Architecture

The lab consists of:

- 2 × Layer 3 Core Switches
- 2 × Layer 2 Access Switches
- 1 × Edge Router
- Multiple VLANs
- End-user devices
- Server devices
- Redundant EtherChannel links
- OSPF routed links
- Secure SSH management

### Topology

![Enterprise Network Topology](topology/enterprise-network-topology.png)

---

## 🌐 VLAN Design

| VLAN | Name | Network | Gateway - Core SW1 | Gateway - Core SW2 |
|------|------|---------|--------------------|--------------------|
| 10 | USERS | 10.10.10.0/24 | 10.10.10.1 | 10.20.10.1 |
| 20 | SERVERS | 10.10.20.0/24 | 10.10.20.1 | 10.20.20.1 |
| 30 | MANAGEMENT | 10.10.30.0/24 | 10.10.30.1 | 10.20.30.1 |
| 40 | GUEST | 10.10.40.0/24 | 10.10.40.1 | 10.20.40.1 |
| 99 | NATIVE / MANAGEMENT | 10.10.99.0/24 | 10.10.99.1 | 10.20.99.1 |

---

## 🔢 IP Addressing

### Core SW1

| Interface | IP Address |
|-----------|------------|
| Fa0/1 | 10.10.101.2/30 |
| VLAN 10 | 10.10.10.1/24 |
| VLAN 20 | 10.10.20.1/24 |
| VLAN 30 | 10.10.30.1/24 |
| VLAN 40 | 10.10.40.1/24 |
| VLAN 99 | 10.10.99.1/24 |

### Core SW2

| Interface | IP Address |
|-----------|------------|
| Fa0/1 | 10.10.102.2/30 |
| VLAN 10 | 10.20.10.1/24 |
| VLAN 20 | 10.20.20.1/24 |
| VLAN 30 | 10.20.30.1/24 |
| VLAN 40 | 10.20.40.1/24 |
| VLAN 99 | 10.20.99.1/24 |

### Access Switch Management

| Device | Management IP |
|--------|---------------|
| ACCESS-SW1 | 10.10.99.11/24 |
| ACCESS-SW2 | 10.20.99.11/24 |

---

## 🔀 Switching Configuration

### VLANs

Configured VLANs:

- VLAN 10 - USERS
- VLAN 20 - SERVERS
- VLAN 30 - MANAGEMENT
- VLAN 40 - GUEST
- VLAN 99 - NATIVE

### 802.1Q Trunking

Trunk links use:

- IEEE 802.1Q
- Native VLAN 99
- Allowed VLANs: 10, 20, 30, 40, 99

### EtherChannel

LACP EtherChannel is configured for redundancy and increased bandwidth.

| Device | Port-Channel | Protocol | Member Ports |
|--------|--------------|----------|--------------|
| CORE-SW1 | Po1 | LACP | Fa0/2, Fa0/5 |
| CORE-SW2 | Po2 | LACP | Fa0/2, Fa0/5 |
| ACCESS-SW1 | Po1 | LACP | Fa0/1, Fa0/4 |
| ACCESS-SW2 | Po2 | LACP | Fa0/1, Fa0/4 |

---

## 🌳 Spanning Tree

Rapid-PVST / IEEE spanning tree is used for Layer 2 loop prevention.

### CORE-SW1

CORE-SW1 is the STP root bridge for:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 40

### CORE-SW2

CORE-SW2 is the STP root bridge for:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 40

Access switches use their respective Port-Channels as the root path toward the core.

---

## 🚦 Routing

### Inter-VLAN Routing

Layer 3 SVIs on the core switches provide gateway services for the VLANs.

### OSPF

OSPF provides dynamic routing between the core network and routed infrastructure.

CORE-SW1
10.10.101.2/30
        |
        | OSPF
        |
10.10.101.1/30
CORE-SW2
10.10.102.2/30
        |
        | OSPF
        |
10.10.102.1/30

OSPF neighbor verification shows:

State: FULL/BDR
📡 DHCP

DHCP is configured for client addressing.

CORE-SW1 DHCP Clients
10.10.10.21
10.10.10.22
CORE-SW2 DHCP Clients
10.20.10.21
10.20.10.22
🔐 Network Security
SSH

SSH Version 2 is enabled on the switches for secure remote management.

Configured devices include:

CORE-SW1
CORE-SW2
ACCESS-SW1
ACCESS-SW2

VTY access is restricted to SSH.

Guest Network Security

An extended ACL named:

GUEST-RESTRICT

is configured on CORE-SW1.

The ACL restricts VLAN 40 guest traffic from accessing internal networks including:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.20.10.0/24
10.20.20.0/24
10.20.30.0/24
10.20.40.0/24
10.20.99.0/24

Guest traffic that does not match the restricted destinations is permitted.

🧪 Verification

The following Cisco commands were used to verify the implementation:

show ip interface brief
show ip route
show ip ospf neighbor
show ip dhcp binding
show etherchannel summary
show interfaces trunk
show vlan brief
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
show ip ssh
show access-lists
✅ Verification Results
OSPF
Neighbor State: FULL/BDR
EtherChannel
Po1(SU) - LACP
Po2(SU) - LACP
SSH
SSH Enabled - Version 2.0
Trunking
802.1Q
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
STP

All required VLANs are forwarding through the expected root paths.

DHCP

DHCP bindings were successfully observed on both core switches.

ACL

The GUEST-RESTRICT ACL shows traffic matches, confirming that the security policy is actively processing traffic.

OSPF Process ID:
📁 Repository Structure
enterprise-network-lab/
│
├── README.md
├── LICENSE
│
├── configs/
│   └── Cisco device configurations
│
├── diagrams/
│   └── Network topology diagrams
│
├── documentation/
│   └── Network design and technical documentation
│
├── labs/
│   ├── ACCESS-SW1.txt
│   ├── ACCESS-SW2.txt
│   ├── CORE-SW1.txt
│   ├── CORE-SW2.txt
│   └── R1-EDGE.txt
│
├── topology/
│   ├── .gitkeep
│   └── enterprise-network-topology.png
│
└── verification/
    └── Testing and troubleshooting results
🛠️ Technologies Used
Cisco IOS
Cisco Packet Tracer
VLAN
802.1Q
STP / Rapid-PVST
LACP EtherChannel
Inter-VLAN Routing
OSPF
DHCP
SSH
Extended ACL
Network Troubleshooting
🎓 Skills Demonstrated

This project demonstrates practical experience with:

Enterprise LAN/WAN architecture
Cisco switching
Layer 2 troubleshooting
Layer 3 routing
VLAN segmentation
Network redundancy
Dynamic routing
Network security
Secure device management
DHCP troubleshooting
ACL implementation
Cisco verification commands
Network documentation

👨‍💻 Author
Ashutosh Vikhe
SENIOR ENGINEER - DIGITAL NETWORK & SECURITY

Focus Areas:

Enterprise Networking
Cisco Switching & Routing
Cisco ISE / NAC
Network Security
Network Automation
Cloud & DevOps




1
