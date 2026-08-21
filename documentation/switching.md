
# Switching

## 1. Overview

The switching design uses VLAN segmentation, 802.1Q trunking, LACP EtherChannel, and Spanning Tree Protocol (STP).

## 2. VLAN Configuration

The following VLANs are configured:

| VLAN ID | Name | Purpose |
|---:|---|---|
| 10 | USERS | End-user devices |
| 20 | SERVERS | Server devices |
| 30 | MANAGEMENT | Management devices |
| 40 | GUEST | Guest devices |
| 99 | NATIVE | Native and management traffic |

## 3. Access Ports

### ACCESS-SW1

| Interface | Mode | VLAN |
|---|---|---:|
| Fa0/2 | Access | 10 |
| Fa0/3 | Access | 10 |
| Fa0/5 | Access | 40 |

PortFast is enabled on the guest access port Fa0/5.

### ACCESS-SW2

| Interface | Mode | VLAN |
|---|---|---:|
| Fa0/2 | Access | 10 |
| Fa0/3 | Access | 10 |
| Fa0/5 | Access | 20 |
| Fa0/6 | Access | 20 |
| Fa0/7 | Access | 30 |
| Fa0/8 | Access | 30 |
| Fa0/9 | Access | 40 |
| Fa0/10 | Access | 40 |

## 4. Trunking

802.1Q trunking is configured on the EtherChannel uplinks.

### ACCESS-SW1

Port-channel: Po1
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
Status: Trunking

ACCESS-SW2
Port-channel: Po2
Native VLAN: 99
Allowed VLANs: 10,20,30,40,99
Status: Trunking
5. EtherChannel

LACP is used to bundle two physical interfaces into a single logical link.

ACCESS-SW1
Port-channel: Po1
Protocol: LACP
Members: Fa0/1, Fa0/4
Status: SU
ACCESS-SW2
Port-channel: Po2
Protocol: LACP
Members: Fa0/1, Fa0/4
Status: SU

The P flag confirms that the physical interfaces are successfully participating in the EtherChannel.

6. Spanning Tree Protocol

STP is enabled to prevent Layer 2 switching loops.

CORE-SW1 is the STP root for VLANs 10, 20, 30, and 40.

CORE-SW1
VLAN 10 → Root
VLAN 20 → Root
VLAN 30 → Root
VLAN 40 → Root
ACCESS-SW1

The EtherChannel uplink Po1 operates as the Root Forwarding port.

ACCESS-SW2

The EtherChannel uplink Po2 operates as the Root Forwarding port.

7. STP Verification

Verified STP states include:

Po1 → Root FWD on ACCESS-SW1
Po2 → Root FWD on ACCESS-SW2

The core switches have their respective EtherChannels in the forwarding state.

8. Switching Verification Commands
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
show interfaces switchport
9. Switching Summary

The switching implementation provides:

VLAN segmentation
Access port configuration
802.1Q trunking
Native VLAN 99
LACP EtherChannel
Link redundancy
STP loop prevention
PortFast on appropriate end-host ports
Controlled VLAN propagation across trunk links


