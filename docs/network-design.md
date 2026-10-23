# Network Design

## 1. Project Overview

This project simulates a small company's enterprise network using Cisco Packet Tracer.

The objective is to design a segmented, manageable, and secure network for three departments: Administration, IT, and Guest users.

## 2. Business Requirements

The company requires:

* Separate network segments for each department.
* Automatic IP address assignment for client devices.
* Communication between authorized network segments.
* Restricted access for Guest users.
* A dedicated management VLAN for network infrastructure.
* Documented configuration and connectivity tests.

## 3. Network Topology

The planned network will include:

* One router.
* Two switches.
* Client computers in different VLANs.
* A trunk connection between the switches.
* A router-to-switch connection for inter-VLAN routing.

The topology will be documented with a network diagram after implementation.

## 4. VLAN Design

| VLAN ID | Name       | Network         | Default Gateway |
| ------- | ---------- | --------------- | --------------- |
| 10      | ADMIN      | 192.168.10.0/24 | 192.168.10.1    |
| 20      | IT         | 192.168.20.0/24 | 192.168.20.1    |
| 30      | GUEST      | 192.168.30.0/24 | 192.168.30.1    |
| 99      | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1    |

## 5. Planned Technologies

* Cisco Packet Tracer
* IPv4 addressing and subnetting
* VLAN configuration
* IEEE 802.1Q trunking
* Inter-VLAN routing
* DHCP
* Access Control Lists (ACLs)
* Network troubleshooting

## 6. Security Considerations

Guest users should have restricted access to internal networks.

Administrative access to network devices should be limited to authorized management hosts.

Security policies will be tested and documented after configuration.

## 7. Testing Plan

The following tests will be performed:

1. Verify VLAN assignments.
2. Verify trunk configuration.
3. Verify IP addressing and DHCP.
4. Test connectivity to default gateways.
5. Test routing between VLANs.
6. Verify ACL behavior.
7. Document failures, troubleshooting steps, and final results.

## 8. Project Status

Status: Planning

The network design is documented. Device configuration and validation are the next steps.
