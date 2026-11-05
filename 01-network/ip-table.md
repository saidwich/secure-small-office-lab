# IP Addressing Table

## Subnets and VLANs

| VLAN | Name | Subnet | Gateway (R1) | DHCP Range | Purpose |
|---|---|---|---|---|---|
| 10 | Staff | 192.168.10.0/24 | 192.168.10.1 | 192.168.10.21 - .254 | Employee workstations |
| 20 | Servers | 192.168.20.0/24 | 192.168.20.1 | Static only | Internal servers |
| 30 | Guest | 192.168.30.0/24 | 192.168.30.1 | 192.168.30.21 - .254 | Visitor devices (isolated) |
| N/A | Simulated Internet | 203.0.113.0/30 | N/A | Static only | Link between R1 and the Internet server |

## Device Addresses

| Device | Interface | IP Address | Subnet Mask | Gateway | Assigned By |
|---|---|---|---|---|---|
| R1 | Gi0/0.10 | 192.168.10.1 | 255.255.255.0 | N/A | Static |
| R1 | Gi0/0.20 | 192.168.20.1 | 255.255.255.0 | N/A | Static |
| R1 | Gi0/0.30 | 192.168.30.1 | 255.255.255.0 | N/A | Static |
| R1 | Gi0/1 | 203.0.113.1 | 255.255.255.252 | N/A | Static |
| Server1 | NIC | 192.168.20.10 | 255.255.255.0 | 192.168.20.1 | Static |
| Internet | NIC | 203.0.113.2 | 255.255.255.252 | 203.0.113.1 | Static |
| Staff-PC1 | NIC | 192.168.10.21 | 255.255.255.0 | 192.168.10.1 | DHCP |
| Staff-PC2 | NIC | 192.168.10.22 | 255.255.255.0 | 192.168.10.1 | DHCP |
| Guest-PC1 | NIC | 192.168.30.21 | 255.255.255.0 | 192.168.30.1 | DHCP |

## Services

| Service | Host | Details |
|---|---|---|
| DNS | Server1 (192.168.20.10) | `fileserver.office.local` -> 192.168.20.10 |
| DHCP | R1 | Pools: STAFF, GUEST (first 20 addresses excluded) |

## Switch Port Assignments (SW1)

| Ports | VLAN | Connected To |
|---|---|---|
| Gi0/1 | Trunk (10, 20, 30) | R1 Gi0/0 |
| Fa0/1 - Fa0/9 | 10 (Staff) | Staff-PC1 (Fa0/1), Staff-PC2 (Fa0/2) |
| Fa0/10 - Fa0/19 | 20 (Servers) | Server1 (Fa0/10) |
| Fa0/20 - Fa0/24 | 30 (Guest) | Guest-PC1 (Fa0/20) |
