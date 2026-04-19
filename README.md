# Incident Investigation & Containment: CVE-2007-2447
**Document ID:** IR-LOG-11942
**Classification:** Technical / Internal Use

## 1. Network Topology & Attack Flow
The following diagram illustrates the logical relationship and the sequence of the compromise.

```mermaid
graph TD
    subgraph Security_Zone [VLAN 64 - Isolated Lab]
        direction TB
        
        %% Attacker Node
        Attacker["💻 <b>Workstation_01</b><br/>(Kali Linux)<br/>192.168.64.4"]
        
        %% Target Node
        Target["🖥️ <b>Production_Server_02</b><br/>(Metasploitable 2)<br/>192.168.64.5"]
        
        %% Traffic Flow
        Attacker -- "1. SMB Exploit (TCP 139/445)" --> Target
        Target -. "2. Reverse Shell (TCP 4444)" .-> Attacker
    end

    %% Styling
    style Attacker fill:#1a1a1a,stroke:#333,stroke-width:2px,color:#fff
    style Target fill:#1a1a1a,stroke:#333,stroke-width:2px,color:#fff
    style Security_Zone fill:#0d1117,stroke:#58a6ff,stroke-dasharray: 5 5
