# Network Design

## 1. Project Overview

This project simulates a small company's enterprise network using Cisco Packet Tracer.

The objective is to design a segmented, manageable and secure network for three
departments: Administration, IT and Guest users.

## 2. Business Requirements

The company requires:

* Separate network segments for each department.
* Automatic IP address assignment for client devices.
* Communication between authorized network segments.
* Restricted access for Guest users.
* A dedicated management VLAN for network infrastructure.
* Secure, restricted administrative access to network devices.
* Documented configuration and connectivity tests.

## 3. Network Topology

* One router (R1, Cisco 2911) using router-on-a-stick.
* Two switches (SW1 and SW2, Cisco 2960) connected by an 802.1Q trunk.
* Four client computers in separate VLANs.

### Physical connections

| Device | Port   | Connected to | Port   | Mode            |
|--------|--------|--------------|--------|-----------------|
| R1     | Gi0/0  | SW1          | Fa0/1  | Trunk           |
| SW1    | Fa0/2  | SW2          | Fa0/1  | Trunk           |
| SW1    | Fa0/3  | PC-ADMIN     | Fa0    | Access VLAN 10  |
| SW1    | Fa0/4  | PC-MGMT      | Fa0    | Access VLAN 99  |
| SW2    | Fa0/2  | PC-IT        | Fa0    | Access VLAN 20  |
| SW2    | Fa0/3  | PC-GUEST     | Fa0    | Access VLAN 30  |

See `diagrams/network-topology.png`.

## 4. VLAN Design

| VLAN ID | Name       | Network         | Default Gateway |
|---------|------------|-----------------|-----------------|
| 10      | ADMIN      | 192.168.10.0/24 | 192.168.10.1    |
| 20      | IT         | 192.168.20.0/24 | 192.168.20.1    |
| 30      | GUEST      | 192.168.30.0/24 | 192.168.30.1    |
| 99      | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1    |
| 998     | PARKING    | -               | -               |
| 999     | NATIVE     | -               | -               |

## 5. IP Addressing

| Device   | Interface | IP Address      | Assignment |
|----------|-----------|-----------------|------------|
| R1       | Gi0/0.10  | 192.168.10.1/24 | Static     |
| R1       | Gi0/0.20  | 192.168.20.1/24 | Static     |
| R1       | Gi0/0.30  | 192.168.30.1/24 | Static     |
| R1       | Gi0/0.99  | 192.168.99.1/24 | Static     |
| SW1      | VLAN 99   | 192.168.99.2/24 | Static     |
| SW2      | VLAN 99   | 192.168.99.3/24 | Static     |
| PC-MGMT  | Fa0       | 192.168.99.10/24| Static     |
| PC-ADMIN | Fa0       | 192.168.10.0/24 | DHCP       |
| PC-IT    | Fa0       | 192.168.20.0/24 | DHCP       |
| PC-GUEST | Fa0       | 192.168.30.0/24 | DHCP       |

DHCP runs on R1 with one pool per user VLAN. Addresses .1-.20 in each network
are excluded for static use. The lab has no internet connection, so the
configured DNS server (8.8.8.8) is only an example.

## 6. Security Design

### Guest isolation
The extended ACL `GUEST_RESTRICTIONS` is applied inbound on Gi0/0.30. It blocks
traffic from GUEST to ADMIN, IT and MANAGEMENT, permits other guest traffic, and
permits DHCP requests (UDP 67) so guests can obtain an address.

This is a stateless ACL, not a full firewall.

### Device management
* SSH v2 only, with local user authentication. Telnet is disabled.
* VTY access is limited to 192.168.99.0/24 with the standard ACL `MGMT_ONLY`.
* `enable secret`, password encryption, console login and exec timeouts.
* Login banner on all devices.

### Layer 2 security
* Trunks use native VLAN 999 and have DTP disabled.
* Unused ports are shut down and assigned to VLAN 998.
* Port security with sticky MAC addresses on client ports.
* DHCP snooping on VLAN 10, 20 and 30. Only trunk ports toward R1 are trusted.
  Client ports are rate limited to 10 packets per second.
* Dynamic ARP Inspection on VLAN 10, 20 and 30, using the DHCP snooping
  binding table.

VLAN 99 is excluded from DHCP snooping and DAI because PC-MGMT uses a static address.

## 7. Testing

All tests, results and troubleshooting notes are documented in `docs/test-results.md`.

## 8. Project Status

Status: Implemented

The core network (v1.0) and the hardening phase (v2.0) are configured and verified.