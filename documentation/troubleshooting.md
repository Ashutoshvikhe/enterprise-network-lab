

# Troubleshooting

## 1. Overview

This section documents common verification and troubleshooting procedures used in the enterprise network lab.

## 2. Interface Troubleshooting

Check interface status:

show ip interface brief
Verify that required interfaces show:

Status: up
Protocol: up

If an interface is administratively down:

configure terminal
interface <interface>
no shutdown
3. VLAN Troubleshooting

Verify VLANs and assigned ports:

show vlan brief

Check that the required access port belongs to the correct VLAN.

Expected VLANs:

VLAN 10 - USERS
VLAN 20 - SERVERS
VLAN 30 - MANAGEMENT
VLAN 40 - GUEST
VLAN 99 - NATIVE
4. Trunk Troubleshooting

Verify trunk status:

show interfaces trunk

Check:

Trunk status
Native VLAN
Allowed VLANs
Active VLANs
STP forwarding VLANs

Expected native VLAN:

99

Expected allowed VLANs:

10,20,30,40,99
5. EtherChannel Troubleshooting

Verify EtherChannel:

show etherchannel summary

Expected status:

Po1(SU) - ACCESS-SW1
Po2(SU) - ACCESS-SW2

Expected member interfaces:

ACCESS-SW1:
Fa0/1(P)
Fa0/4(P)


ACCESS-SW2:
Fa0/1(P)
Fa0/4(P)

The P flag indicates that the interface is successfully participating in the port-channel.

6. STP Troubleshooting

Check STP:

show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree vlan 30
show spanning-tree vlan 40

Verify:

Correct root bridge
Root ports
Forwarding state
No unexpected blocked ports

CORE-SW1 is the verified STP root for VLANs 10, 20, 30 and 40.

7. OSPF Troubleshooting

Check OSPF neighbors:

show ip ospf neighbor

Expected neighbor state:

FULL/BDR

Check routing:

show ip route

OSPF routes should appear with:

O

If OSPF routes are missing, verify:

show ip interface brief
show ip ospf neighbor
show ip route
8. DHCP Troubleshooting

Check DHCP bindings:

show ip dhcp binding

Verify that clients receive addresses from the correct network.

CORE-SW1:

10.10.10.0/24

CORE-SW2:

10.20.10.0/24
9. SSH Troubleshooting

Check SSH status:

show ip ssh

Expected:

SSH Enabled - version 2.0

Check VTY configuration:

show running-config | section line vty

Expected configuration includes:

login local
transport input ssh
10. Guest ACL Troubleshooting

Check ACL configuration and counters:

show access-lists

Verify ACL application:

show running-config | include access-group

Expected:

ip access-group GUEST-RESTRICT in

The ACL should block guest traffic to configured internal networks while permitting other IP traffic.

11. Connectivity Testing

Use ping to test connectivity between devices and networks.

Examples:

ping 10.10.10.1
ping 10.10.99.11
ping 10.20.10.1
ping 10.20.99.11

For routing verification, test connectivity to remote networks.

12. Troubleshooting Workflow

Use the following sequence when troubleshooting a connectivity problem:

Check physical/interface status.
Verify VLAN assignment.
Verify trunk configuration.
Verify EtherChannel status.
Check STP state.
Check IP addressing.
Check routing table.
Check OSPF neighbor status.
Check DHCP bindings.
Check ACL configuration and counters.
Perform ping testing.
Re-check the configuration after making changes.
13. Useful Commands
show ip interface brief
show vlan brief
show interfaces trunk
show etherchannel summary
show spanning-tree
show ip route
show ip ospf neighbor
show ip dhcp binding
show ip ssh
show access-lists
show running-config
ping <destination-ip>
14. Troubleshooting Summary

The troubleshooting methodology provides a structured approach to identifying:

Interface problems
VLAN configuration issues
Trunking problems
EtherChannel failures
STP issues
OSPF adjacency problems
Routing problems
DHCP issues
SSH configuration problems
ACL-related connectivity issues

