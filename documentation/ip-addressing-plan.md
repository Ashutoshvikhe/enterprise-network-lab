# IP Addressing Plan

## 1. Overview

The network uses separate IP address ranges for each VLAN and site.

The first site uses the `10.10.0.0/16` address space.

The second site uses the `10.20.0.0/16` address space.

Each VLAN uses a `/24` subnet.

## 2. CORE-SW1 IP Addressing

| Interface | VLAN / Purpose | Network | IP Address |
|---|---|---|---|
| Vlan10 | USERS | 10.10.10.0/24 | 10.10.10.1 |
| Vlan20 | SERVERS | 10.10.20.0/24 | 10.10.20.1 |
| Vlan30 | MANAGEMENT | 10.10.30.0/24 | 10.10.30.1 |
| Vlan40 | GUEST | 10.10.40.0/24 | 10.10.40.1 |
| Vlan99 | NATIVE/MGMT | 10.10.99.0/24 | 10.10.99.1 |
| Fa0/1 | OSPF Transit | 10.10.101.0/30 | 10.10.101.2 |

## 3. CORE-SW2 IP Addressing

| Interface | VLAN / Purpose | Network | IP Address |
|---|---|---|---|
| Vlan10 | USERS | 10.20.10.0/24 | 10.20.10.1 |
| Vlan20 | SERVERS | 10.20.20.0/24 | 10.20.20.1 |
| Vlan30 | MANAGEMENT | 10.20.30.0/24 | 10.20.30.1 |
| Vlan40 | GUEST | 10.20.40.0/24 | 10.20.40.1 |
| Vlan99 | NATIVE/MGMT | 10.20.99.0/24 | 10.20.99.1 |
| Fa0/1 | OSPF Transit | 10.10.102.0/30 | 10.10.102.2 |

## 4. Access Switch Management

### ACCESS-SW1

| Device | Interface | IP Address | Network |
|---|---|---|---|
| ACCESS-SW1 | Vlan99 | 10.10.99.11 | 10.10.99.0/24 |

### ACCESS-SW2

| Device | Interface | IP Address | Network |
|---|---|---|---|
| ACCESS-SW2 | Vlan99 | 10.20.99.11 | 10.20.99.0/24 |

## 5. VLAN Addressing Summary

| VLAN | Name | Site 1 Network | Site 1 Gateway | Site 2 Network | Site 2 Gateway |
|---:|---|---|---|---|---|
| 10 | USERS | 10.10.10.0/24 | 10.10.10.1 | 10.20.10.0/24 | 10.20.10.1 |
| 20 | SERVERS | 10.10.20.0/24 | 10.10.20.1 | 10.20.20.0/24 | 10.20.20.1 |
| 30 | MANAGEMENT | 10.10.30.0/24 | 10.10.30.1 | 10.20.30.0/24 | 10.20.30.1 |
| 40 | GUEST | 10.10.40.0/24 | 10.10.40.1 | 10.20.40.0/24 | 10.20.40.1 |
| 99 | NATIVE/MGMT | 10.10.99.0/24 | 10.10.99.1 | 10.20.99.0/24 | 10.20.99.1 |

## 6. OSPF Transit Networks

The core switches use Layer 3 transit networks for OSPF connectivity.

| Network | Device IP | Purpose |
|---|---|---|
| 10.10.101.0/30 | CORE-SW1 — 10.10.101.2 | OSPF transit |
| 10.10.102.0/30 | CORE-SW2 — 10.10.102.2 | OSPF transit |

The verified OSPF neighbor addresses are:

- CORE-SW1 → 10.10.101.1
- CORE-SW2 → 10.10.102.1

## 7. DHCP Networks

DHCP is configured for client networks.

### CORE-SW1

DHCP network:

10.10.10.0/24
Gateway: 10.10.10.1
