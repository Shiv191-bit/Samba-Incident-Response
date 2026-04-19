## 1. Network Topology
The following mapping defines the logical relationship between the internal nodes during the incident.

```mermaid
graph LR
    subgraph Security_Zone [VLAN 64 - Isolated]
    A[Workstation_01<br/>192.168.64.4] -- "TCP 139/445" --> B
    B[Production_Server_02<br/>192.168.64.5] -- "TCP 4444" --> A
    end
