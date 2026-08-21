# Network Documentation

This directory contains the design and implementation documentation for the Enterprise Network Lab.

## Network Overview

The lab demonstrates an enterprise network architecture using Cisco Layer 3 core switches and Layer 2 access switches.

The network includes:

- VLAN segmentation
- Inter-VLAN routing
- 802.1Q trunking
- LACP EtherChannel
- Spanning Tree Protocol (STP)
- OSPF dynamic routing
- DHCP
- SSHv2 management
- Guest network isolation using ACLs
- Dedicated network management VLAN

## Network Devices

| Device | Role |
|---|---|
| CORE-SW1 | Layer 3 Core Switch |
| CORE-SW2 | Layer 3 Core Switch |
| ACCESS-SW1 | Layer 2 Access Switch |
| ACCESS-SW2 | Layer 2 Access Switch |

## VLAN Structure

| VLAN | Name | CORE-SW1 Network | CORE-SW2 Network |
|---:|---|---|---|
| 10 | USERS | 10.10.10.0/24 | 10.20.10.0/24 |
| 20 | SERVERS | 10.10.20.0/24 | 10.20.20.0/24 |
| 30 | MANAGEMENT | 10.10.30.0/24 | 10.20.30.0/24 |
| 40 | GUEST | 10.10.40.0/24 | 10.20.40.0/24 |
| 99 | NATIVE / MANAGEMENT | 10.10.99.0/24 | 10.20.99.0/24 |

## Routing

OSPF is configured between the core switches and the upstream Layer 3 network.

CORE-SW1 uses:

- 10.10.101.2/30

CORE-SW2 uses:

- 10.10.102.2/30

OSPF successfully establishes neighbor relationships and advertises the remote VLAN networks.

## Switching

The access switches use:

- VLAN-based segmentation
- 802.1Q trunking
- LACP EtherChannel
- Spanning Tree Protocol
- Access-port configuration
- PortFast for end-host ports

## Network Management

VLAN 99 is used for switch management.

Management addresses include:

- CORE-SW1 — 10.10.99.1
- ACCESS-SW1 — 10.10.99.11
- CORE-SW2 — 10.20.99.1
- ACCESS-SW2 — 10.20.99.11

SSH version 2 is enabled for secure remote management.

## Security

The guest network is restricted using the `GUEST-RESTRICT` extended ACL.

Guest VLAN 40 is prevented from accessing:

- VLAN 10 — USERS
- VLAN 20 — SERVERS
- VLAN 30 — MANAGEMENT
- Remote-site VLAN 10
- Remote-site VLAN 20
- Remote-site VLAN 30
- Remote-site VLAN 40
- Remote-site VLAN 99

Guest traffic that does not match the restricted internal networks is permitted.

## DHCP

DHCP is configured on the core switches to provide dynamic addressing to client networks.

The lab has verified DHCP leases on both core switches.

## Verification

Network operation is verified using Cisco IOS commands including:

- show vlan brief
- show interfaces trunk`
- show etherchannel summary`
- show spanning-tree`
- show ip interface brief`
- show ip route`
- `show ip ospf neighbor`
- `show ip dhcp binding`
- `show ip ssh`
- `show access-lists`

Detailed verification results are documented in the `verification` directory.

## Configuration Files

Device configurations are stored in the `configs` directory:

- CORE-SW1
- CORE-SW2
- ACCESS-SW1
- ACCESS-SW2
