# Network Test Results

## DHCP
- PC-ADMIN received an IP address in VLAN 10: PASS
- PC-IT received an IP address in VLAN 20: PASS
- PC-GUEST received an IP address in VLAN 30: PASS

## Connectivity
- PC-ADMIN can reach its default gateway: PASS
- PC-IT can reach its default gateway: PASS
- PC-GUEST can reach its default gateway: PASS
- ADMIN can communicate with IT: PASS

## Security
- GUEST access to ADMIN is blocked: PASS
- GUEST access to IT is blocked: PASS
- GUEST access to MANAGEMENT is blocked: PASS
- Switch port security is enabled: PASS

## Hardening

### Device management
- SSH from PC-MGMT to R1, SW1 and SW2 works: PASS
- SSH from PC-ADMIN to SW1 is refused: PASS
- Telnet from PC-MGMT to SW1 is refused: PASS
- No plaintext passwords in running configs: PASS

### Layer 2
- Trunks use native VLAN 999: PASS
- DTP is disabled on trunk ports: PASS
- Unused ports are disabled and in VLAN 998: PASS 
- DHCP snooping bindings exist for all DHCP clients: PASS
- ADMIN and IT can communicate with DAI enabled: PASS
- Guest isolation still works after hardening: PASS

## Notes

### Guest DHCP blocked by ACL
**Problem:** After the `GUEST_RESTRICTIONS` ACL was applied, PC-GUEST could not
obtain an address via DHCP. A static address in VLAN 30 worked.

**Cause:** DHCP Discover is sent from source 0.0.0.0. The ACL only permitted
source 192.168.30.0/24, so the request was dropped by the implicit deny.

**Fix:** Added `permit udp any any eq 67` to `GUEST_RESTRICTIONS`.

**Verification:** PC-GUEST receives 192.168.30.21 from the GUEST pool, and guest
traffic to ADMIN is still blocked (Destination host unreachable from 192.168.30.1).

### DHCP snooping bindings
After enabling DHCP snooping, PC-IT was missing from the binding table on SW2
because it still held its address from before snooping was enabled. Renewing
the address added the binding. This was required before enabling DAI.