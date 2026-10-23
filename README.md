# Enterprise Network Lab

## Overview
A small enterprise network built in Cisco Packet Tracer to demonstrate
VLAN segmentation, inter-VLAN routing, DHCP, trunking, ACLs and basic
switch port security.

## Topology
- 1 router
- 2 switches
- 3 end devices

## VLAN Design

| VLAN | Name       | Network         |
|------|------------|-----------------|
| 10   | ADMIN      | 192.168.10.0/24 |
| 20   | IT         | 192.168.20.0/24 |
| 30   | GUEST      | 192.168.30.0/24 |
| 99   | MANAGEMENT | 192.168.99.0/24 |

## Technologies
- Cisco IOS CLI
- IEEE 802.1Q trunking
- Router-on-a-stick
- DHCP
- Extended named ACLs
- Switch port security

## Verification
See `docs/test-results.md` for the test results.

## Project Files
- `configs/` - Device configurations
- `diagrams/` - Network diagrams
- `docs/` - Network design and test documentation
- `screenshots/` - Evidence of configuration and testing

## Status
Work in progress. Features and test results are documented as they
are completed and verified.