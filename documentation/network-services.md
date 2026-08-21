

# Network Services

## 1. Overview

The enterprise network provides DHCP services for client devices and uses SSH for secure device management.

## 2. DHCP

DHCP is configured on the core switches to dynamically assign IP addresses to client devices.

### CORE-SW1

DHCP network:
Network: 10.10.10.0/24
Gateway: 10.10.10.1

Verified DHCP leases:

10.10.10.21
10.10.10.22
CORE-SW2

DHCP network:

Network: 10.20.10.0/24
Gateway: 10.20.10.1

Verified DHCP leases:

10.20.10.21
10.20.10.22
3. DHCP Verification

The following command was used to verify DHCP leases:

show ip dhcp binding

Example verified output on CORE-SW1:

10.10.10.21
10.10.10.22

Example verified output on CORE-SW2:

10.20.10.21
10.20.10.22
4. DHCP Gateway Configuration

Each DHCP client network uses the corresponding SVI as its default gateway.

Site 1
VLAN 10 → 10.10.10.1
Site 2
VLAN 10 → 10.20.10.1
5. SSH

SSH version 2 is enabled on all network devices for secure remote management.

Device	SSH
CORE-SW1	Enabled
CORE-SW2	Enabled
ACCESS-SW1	Enabled
ACCESS-SW2	Enabled
6. SSH VTY Configuration

The VTY lines use local authentication and allow SSH connections.

line vty 0 4
 login local
 transport input ssh
7. Management IP Addresses

Network devices are managed through VLAN 99.

Site 1
CORE-SW1:     10.10.99.1
ACCESS-SW1:   10.10.99.11
Site 2
CORE-SW2:     10.20.99.1
ACCESS-SW2:   10.20.99.11
8. Network Services Verification

The following commands can be used to verify the network services:

show ip dhcp binding
show ip ssh
show ip interface brief
show running-config | section line vty
9. Services Summary

The network services implementation provides:

Dynamic IP address assignment using DHCP
VLAN-based default gateways
Secure SSH version 2 management
Local authentication for VTY access
Dedicated management addressing through VLAN 99

