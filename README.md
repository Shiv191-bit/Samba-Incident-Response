# Incident Investigation & Containment: CVE-2007-2447
**Document ID:** IR-LOG-11942
**Classification:** Technical / Internal Use

---

## 1. Forensic Evidence Archive
Technical artifacts documenting the full lifecycle of the incident: Initial exploitation, log-based detection, process identification, and final SIGKILL containment.

### Primary Forensic Log
![Forensic Evidence](forensic_evidence.jpg)

### Environment Initialization
![Setup](env_init.jpg)

---






  ## 2. Network Topology & Attack Flow
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



## 3. Incident Lifecycle Detail

### Detection & Analysis
Initial telemetry was gathered by auditing the system authentication logs.
- **Log Source:** `/var/log/auth.log`
- **Finding:** Unauthorized root session established via Samba service exploit.
- **Process Audit:** Used `ps aux` to isolate malicious Netcat listeners.
- **Identified Threat PIDs:** 4910, 4911, 4913

### Containment & Eradication
To secure the host and terminate the attacker's remote access, a SIGKILL signal was issued to the identified malicious processes.

**Remediation Command:**
```bash
kill -9 4910 4911 4913
