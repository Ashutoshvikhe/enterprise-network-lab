# Network Security

## 1. Overview

Basic network security controls are implemented to secure management access and restrict guest network access.

## 2. SSH Management

SSH version 2 is enabled on all network devices for secure remote management.

| Device | SSH Status | Version |
|---|---|---|
| CORE-SW1 | Enabled | 2.0 |
| CORE-SW2 | Enabled | 2.0 |
| ACCESS-SW1 | Enabled | 2.0 |
| ACCESS-SW2 | Enabled | 2.0 |

## 3. VTY Line Security

The VTY lines are configured for local authentication and SSH-only access.

cisco
line vty 0 4
 login local
 transport input ssh
 
The additional VTY lines are also configured to use local authentication and SSH.
4. Management VLAN

VLAN 99 is used for network device management.

Site 1
CORE-SW1     10.10.99.1
ACCESS-SW1   10.10.99.11
Site 2
CORE-SW2     10.20.99.1
ACCESS-SW2   10.20.99.11
5. Guest Network Security

VLAN 40 is dedicated to guest devices.

The GUEST-RESTRICT extended ACL is configured on CORE-SW1 to restrict guest traffic from accessing internal networks.

The ACL blocks traffic from:

10.10.40.0/24

to the following networks:

10.10.10.0/24
10.10.20.0/24
10.10.30.0/24
10.20.10.0/24
10.20.20.0/24
10.20.30.0/24
10.20.40.0/24
10.20.99.0/24

Other IP traffic from the guest network is permitted.

6. Guest ACL Configuration
ip access-list extended GUEST-RESTRICT
 deny ip 10.10.40.0 0.0.0.255 10.10.10.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.10.20.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.10.30.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.20.10.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.20.20.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.20.30.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.20.40.0 0.0.0.255
 deny ip 10.10.40.0 0.0.0.255 10.20.99.0 0.0.0.255
 permit ip 10.10.40.0 0.0.0.255 any

The ACL is applied inbound on VLAN 40.

interface Vlan40
 ip access-group GUEST-RESTRICT in
7. PortFast

PortFast is configured on appropriate end-host access ports.

It is used only on ports connected to end devices and not on trunk or switch-to-switch links.

8. Security Verification

The following commands were used to verify the security configuration:

show ip ssh
show running-config | section line vty
show access-lists
show running-config | include access-group
show ip interface brief
9. Security Summary

The network security implementation provides:

SSH version 2 secure management
Local VTY authentication
SSH-only remote access
Dedicated management VLAN
Guest network isolation
Extended ACL-based traffic filtering
PortFast on appropriate host-facing ports
Security configuration verification
