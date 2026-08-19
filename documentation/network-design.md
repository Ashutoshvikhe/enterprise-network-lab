# Enterprise Network Design

## 1. Overview

This document describes the network architecture, device roles, VLAN design, IP addressing, and routing strategy used in the Enterprise Network Lab.

## 2. Network Devices

| Device | Role |
|---|---|
| R1-EDGE | WAN/Edge Router |
| R2-CORE | Internal Router |
| CORE-SW1 | Core/Distribution Switch |
| CORE-SW2 | Core/Distribution Switch |
| ACCESS-SW1 | Access Switch |
| ACCESS-SW2 | Access Switch |

## 3. VLAN Design

| VLAN | Name | Network | Purpose |
|---:|---|---|---|
| 10 | USERS | 10.10.10.0/24 | Employee devices |
| 20 | SERVERS | 10.10.20.0/24 | Internal servers |
| 30 | MANAGEMENT | 10.10.30.0/24 | Network management |
| 40 | GUEST | 10.10.40.0/24 | Guest devices |
| 99 | NATIVE | N/A | Native/Infrastructure |

## 4. Default Gateways

| VLAN | Gateway |
|---:|---|
| 10 | 10.10.10.1 |
| 20 | 10.10.20.1 |
| 30 | 10.10.30.1 |
| 40 | 10.10.40.1 |

## 5. WAN Link

R1-EDGE and R2-CORE use a point-to-point /30 network:

- R1-EDGE: 10.10.100.1/30
- R2-CORE: 10.10.100.2/30

## 6. Routing

OSPF will be used as the dynamic routing protocol.

## 7. Switching

The lab will demonstrate:

- VLAN configuration
- 802.1Q trunking
- STP
- EtherChannel
- Inter-VLAN communication

## 8. Security

ACLs will be implemented to control communication between VLANs.

Guest users will have restricted access to internal resources.

## 9. Troubleshooting

The project will document troubleshooting scenarios involving:

- VLAN connectivity
- Trunking
- STP
- EtherChannel
- OSPF
- ACLs
- End-to-end connectivity
