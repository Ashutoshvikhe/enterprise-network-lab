
# Network Verification Summary

## 1. VLAN Verification

The following VLANs were configured and verified across the enterprise network:

| VLAN ID | Name | Network |
|---|---|---|
| 10 | USERS | 10.10.10.0/24 |
| 20 | SERVERS | 10.10.20.0/24 |
| 30 | MANAGEMENT | 10.10.30.0/24 |
| 40 | GUEST | 10.10.40.0/24 |
| 99 | NATIVE | Management / Native VLAN |

VLAN membership was verified using:

show vlan brief
2. Trunk Verification

Trunk connectivity was verified on both access switches.

ACCESS-SW1
Port        Mode         Encapsulation  Status        Native vlan
Po1         on           802.1q         trunking      99

Allowed VLANs:

10,20,30,40,99
ACCESS-SW2
Port        Mode         Encapsulation  Status        Native vlan
Po2         on           802.1q         trunking      99

Allowed VLANs:

10,20,30,40,99

Verification command:

show interfaces trunk
3. EtherChannel Verification

LACP EtherChannel was successfully verified.

CORE-SW1
Group  Port-channel  Protocol
1      Po1(SU)       LACP

Members:

Fa0/2(P)
Fa0/5(P)
CORE-SW2
Group  Port-channel  Protocol
2      Po2(SU)       LACP

Members:

Fa0/2(P)
Fa0/5(P)
ACCESS-SW1
Group  Port-channel  Protocol
1      Po1(SU)       LACP

Members:

Fa0/1(P)
Fa0/4(P)
ACCESS-SW2
Group  Port-channel  Protocol
2      Po2(SU)       LACP

Members:

Fa0/1(P)
Fa0/4(P)

Verification command:

show etherchannel summary
4. Spanning Tree Verification

Spanning Tree Protocol was verified for VLANs 10, 20, 30 and 40.

CORE-SW1

CORE-SW1 is the STP root bridge for VLANs 10, 20, 30 and 40.

Example:

VLAN0010


Root ID    Priority    24586
           Address     00E0.F9D5.1608


           This bridge is the root
CORE-SW2

CORE-SW2 is the STP root bridge for VLANs 10, 20, 30 and 40 in its local VLAN domain.

Example:

VLAN0010


Root ID    Priority    28682
           Address     0002.1718.23BE


           This bridge is the root
ACCESS-SW1

The uplink Port-channel1 is forwarding toward the STP root.

Po1    Root FWD
ACCESS-SW2

The uplink Port-channel2 is forwarding toward the STP root.

Po2    Root FWD

Verification command:

show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40
5. Inter-VLAN Routing Verification

SVIs were verified on both Layer 3 core switches.

CORE-SW1
Vlan10    10.10.10.1
Vlan20    10.10.20.1
Vlan30    10.10.30.1
Vlan40    10.10.40.1
Vlan99    10.10.99.1
CORE-SW2
Vlan10    10.20.10.1
Vlan20    10.20.20.1
Vlan30    10.20.30.1
Vlan40    10.20.40.1
Vlan99    10.20.99.1

Verification command:

show ip interface brief

All configured SVIs were verified as:

up/up
6. OSPF Verification

OSPF adjacency was successfully established between the core switches and the upstream router.

CORE-SW1
Neighbor ID     State
3.3.3.3         FULL/BDR

Neighbor:

10.10.101.1
CORE-SW2
Neighbor ID     State
3.3.3.3         FULL/BDR

Neighbor:

10.10.102.1

Verification command:

show ip ospf neighbor
7. Routing Table Verification

CORE-SW1 successfully learned the remote 10.20.0.0/16 networks through OSPF.

Examples:

O 10.20.10.0/24
O 10.20.20.0/24
O 10.20.30.0/24
O 10.20.40.0/24
O 10.20.99.0/24

CORE-SW2 successfully learned the remote 10.10.0.0/16 networks through OSPF.

Examples:

O 10.10.10.0/24
O 10.10.20.0/24
O 10.10.30.0/24
O 10.10.40.0/24
O 10.10.99.0/24

Verification command:

show ip route
8. DHCP Verification

DHCP bindings were successfully verified.

CORE-SW1
10.10.10.21
10.10.10.22
CORE-SW2
10.20.10.21
10.20.10.22

Verification command:

show ip dhcp binding
9. SSH Verification

SSH version 2 was successfully enabled on:

CORE-SW1
CORE-SW2
ACCESS-SW1
ACCESS-SW2

Verification command:

show ip ssh

Expected result:

SSH Enabled - version 2.0

Remote SSH connectivity was also tested from end devices.

10. ACL Verification

The guest network is restricted from accessing internal networks.

ACL:

GUEST-RESTRICT

The ACL denies traffic from:

10.10.40.0/24

to internal networks including:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.20.10.0/24
10.20.20.0/24
10.20.30.0/24
10.20.40.0/24
10.20.99.0/24

Other traffic is permitted by:

permit ip 10.10.40.0 0.0.0.255 any

ACL hit counters confirmed that the deny rules were matching traffic.

Verification command:

show access-lists
11. End-to-End Connectivity

Connectivity testing was performed using ICMP ping.

Successful tests included:

PC-USER01 → 10.10.10.1
PC-USER01 → 10.10.10.22
PC-SERVER01 → 10.20.10.1
PC-SERVER01 → 10.20.10.22
PC-SERVER01 → 10.10.10.21

Observed result:

Packets Sent = 4
Packets Received = 4
Lost = 0 (0% loss)
12. Overall Verification Status
Feature	Status
VLANs	✅ Verified
802.1Q Trunking	✅ Verified
EtherChannel / LACP	✅ Verified
STP	✅ Verified
Inter-VLAN Routing	✅ Verified
OSPF	✅ Verified
DHCP	✅ Verified
SSH v2	✅ Verified
ACL	✅ Verified
End-to-End Connectivity	✅ Verified
Project Status

The core enterprise network implementation and verification have been completed.

Remaining documentation will focus on troubleshooting scenarios, configuration references, and final project presentation.
