# Network Test Results

All tests were run in Cisco Packet Tracer after configuration was complete.

| # | Source | Destination | Test | Expected | Result | Screenshot |
|---|---|---|---|---|---|---|
| T1 | Staff-PC1 | Staff-PC2 | ping | Pass | Pass | screenshots/t1-staff-to-staff.png |
| T2 | Staff-PC1 | Server1 | ping | Pass | Pass | screenshots/t2-staff-to-server.png |
| T3 | Staff-PC1 | fileserver.office.local | ping (DNS) | Pass | Pass | screenshots/t3-dns-resolution.png |
| T4 | Staff-PC1 | Internet | ping | Pass | Pass | screenshots/t4-staff-to-internet.png |
| T5 | Staff-PC1 | Internet | tracert | Via 192.168.10.1 | Pass | screenshots/t5-staff-tracert.png |
| T6 | Guest-PC1 | Internet | ping | Pass | Pass | screenshots/t6-guest-to-internet.png |
| T7 | Guest-PC1 | Staff-PC1 | ping | Blocked | Blocked | screenshots/t7-guest-to-staff-blocked.png |
| T8 | Guest-PC1 | Server1 | ping | Blocked | Blocked | screenshots/t8-guest-to-server-blocked.png |
| T9 | Guest-PC1 | fileserver.office.local | ping | DNS resolves, ping blocked | As expected | screenshots/t9-guest-dns-allowed-ping-blocked.png |
| T10 | Guest-PC1 | Staff-PC1 | tracert | Stops at gateway | As expected | screenshots/t10-guest-tracert-blocked.png |

## Summary
- Inter-VLAN routing, DHCP, and DNS work as designed.
- The Guest ACL blocks Guest access to Staff and Servers while allowing DHCP, DNS, and internet.
- ACL match counters on R1 confirm the deny rules are actively blocking traffic.
