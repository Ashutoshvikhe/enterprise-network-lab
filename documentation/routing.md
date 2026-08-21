# Routing

## 1. Overview

OSPF is used as the dynamic routing protocol between the core switches.

The network uses Layer 3 VLAN interfaces for inter-VLAN routing and /30 networks for OSPF transit links.

## 2. CORE-SW1 Routing

### VLAN Interfaces

| Interface | IP Address | Network |
|---|---|---|
| Vlan10 | 10.10.10.1/24 | 10.10.10.0/24 |
| Vlan20 | 10.10.20.1/24 | 10.10.20.0/24 |
| Vlan30 | 10.10.30.1/24 | 10.10.30.0/24 |
| Vlan40 | 10.10.40.1/24 | 10.10.40.0/24 |
| Vlan99 | 10.10.99.1/24 | 10.10.99.0/24 |

### OSPF Transit Interface

Interface: FastEthernet0/1
IP Address: 10.10.101.2/30
3. CORE-SW2 Routing
VLAN Interfaces
Interface	IP Address	Network
Vlan10	10.20.10.1/24	10.20.10.0/24
Vlan20	10.20.20.1/24	10.20.20.0/24
Vlan30	10.20.30.1/24	10.20.30.0/24
Vlan40	10.20.40.1/24	10.20.40.0/24
Vlan99	10.20.99.1/24	10.20.99.0/24
OSPF Transit Interface
Interface: FastEthernet0/1
IP Address: 10.10.102.2/30
4. OSPF Neighbor Verification

CORE-SW1 has an OSPF neighbor:

Neighbor ID: 3.3.3.3
Address: 10.10.101.1
Interface: FastEthernet0/1
State: FULL/BDR

CORE-SW2 has an OSPF neighbor:

Neighbor ID: 3.3.3.3
Address: 10.10.102.1
Interface: FastEthernet0/1
State: FULL/BDR

The FULL state confirms that the OSPF adjacency is established.

5. CORE-SW1 OSPF Routes

CORE-SW1 learns the following networks through OSPF:

10.10.102.0/30
10.20.10.0/24
10.20.20.0/24
10.20.30.0/24
10.20.40.0/24
10.20.99.0/24

Example:

O 10.20.10.0/24 [110/3] via 10.10.101.1
6. CORE-SW2 OSPF Routes

CORE-SW2 learns the following networks through OSPF:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.10.40.0/24
10.10.99.0/24
10.10.101.0/30

Example:

O 10.10.10.0/24 [110/3] via 10.10.102.1
7. Inter-VLAN Routing

Inter-VLAN routing is provided by the Layer 3 SVI interfaces on the core switches.

Each VLAN has a dedicated gateway:

VLAN 10 → .1
VLAN 20 → .1
VLAN 30 → .1
VLAN 40 → .1
VLAN 99 → .1
8. Routing Verification Commands
show ip interface brief
show ip route
show ip ospf neighbor
show ip route 10.20.10.0
show ip route 10.10.40.0

9. Routing Summary

The routing implementation provides:

Inter-VLAN routing
Dynamic routing using OSPF
OSPF neighbor adjacency between core devices
Route exchange between network sites
/30 transit networks
Layer 3 SVI gateways
Verified routing tables
