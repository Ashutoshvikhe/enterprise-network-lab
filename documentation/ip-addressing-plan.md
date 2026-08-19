# IP Addressing Plan

## 1. Overview

This document defines the IP addressing scheme used in the Enterprise Network Lab.

The addressing plan uses private IPv4 address space and separates users, servers, management, and guest devices into different VLANs.

## 2. VLAN Addressing

| VLAN ID | VLAN Name | Network | Subnet Mask | Default Gateway |
|---:|---|---|---|---|
| 10 | USERS | 10.10.10.0/24 | 255.255.255.0 | 10.10.10.1 |
| 20 | SERVERS | 10.10.20.0/24 | 255.255.255.0 | 10.10.20.1 |
| 30 | MANAGEMENT | 10.10.30.0/24 | 255.255.255.0 | 10.10.30.1 |
| 40 | GUEST | 10.10.40.0/24 | 255.255.255.0 | 10.10.40.1 |
| 99 | NATIVE | N/A | N/A | N/A |

## 3. Router Addressing

| Device | Interface | IP Address | Subnet Mask | Purpose |
|---|---|---|---|---|
| R1-EDGE | G0/0 | 10.10.100.1 | 255.255.255.252 | WAN link |
| R2-CORE | G0/0 | 10.10.100.2 | 255.255.255.252 | WAN link |

## 4. End Device Addressing

### VLAN 10 - Users

| Device | IP Address | Gateway |
|---|---|---|
| PC-USER01 | 10.10.10.10/24 | 10.10.10.1 |
| PC-USER02 | 10.10.10.11/24 | 10.10.10.1 |

### VLAN 20 - Servers

| Device | IP Address | Gateway |
|---|---|---|
| PC-SERVER01 | 10.10.20.10/24 | 10.10.20.1 |

### VLAN 40 - Guest

| Device | IP Address | Gateway |
|---|---|---|
| PC-GUEST01 | 10.10.40.10/24 | 10.10.40.1 |

## 5. Management Addressing

Management addresses will be assigned from VLAN 30.

| Device | Management IP |
|---|---|
| CORE-SW1 | 10.10.30.11 |
| CORE-SW2 | 10.10.30.12 |
| ACCESS-SW1 | 10.10.30.21 |
| ACCESS-SW2 | 10.10.30.22 |

## 6. WAN Network

The point-to-point WAN network uses:

Network: `10.10.100.0/30`

| Device | IP Address |
|---|---|
| R1-EDGE | 10.10.100.1 |
| R2-CORE | 10.10.100.2 |

## 7. Address Allocation Strategy

The following allocation strategy will be used:

- `.1` - Default gateway
- `.2-.9` - Infrastructure devices
- `.10-.49` - Static endpoints
- `.50-.199` - DHCP client range
- `.200-.254` - Reserved addresses

## 8. Design Considerations

The addressing plan provides:

- Logical network segmentation
- Simple troubleshooting
- Predictable addressing
- Dedicated management network
- Separate guest network
- Room for future expansion

## 9. Future Expansion

Additional VLANs and subnets can be added using the same structured addressing approach.

Examples:

- Voice VLAN
- Wireless VLAN
- Security/IoT VLAN
- Additional server VLAN
