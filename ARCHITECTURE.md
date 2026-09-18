# Active Directory Lab Architecture

## Network Diagram

```mermaid
flowchart TD
    Internet(("Internet"))

    NAT["VirtualBox NAT Network"]

    DC["Windows Server 2019<br/><b>Domain Controller</b><br/><br/>AD DS<br/>DNS<br/>DHCP<br/>RRAS / NAT"]

    HostOnly["VirtualBox Host-Only Network<br/><br/><b>Network:</b> 172.16.0.0/24<br/><b>DHCP Scope:</b> 172.16.0.100–200<br/><b>Gateway:</b> 172.16.0.1<br/><b>DNS:</b> 172.16.0.1"]

    Client["Windows 10 Client<br/><b>Domain Joined</b><br/><br/>DHCP-assigned IP<br/>DNS: 172.16.0.1"]

    Internet --> NAT
    NAT -->|"Adapter 1: External NIC<br/>DHCP-assigned IP"| DC
    DC -->|"Adapter 2: Host-Only<br/>IP: 172.16.0.1/24<br/>Gateway: None<br/>DNS: 127.0.0.1"| HostOnly
    HostOnly -->|"DHCP and DNS services"| Client

    classDef internet fill:#0969da,color:#ffffff,stroke:#0550ae,stroke-width:2px
    classDef network fill:#ddf4ff,color:#24292f,stroke:#0969da,stroke-width:2px
    classDef server fill:#fff8c5,color:#24292f,stroke:#bf8700,stroke-width:2px
    classDef client fill:#dafbe1,color:#24292f,stroke:#1a7f37,stroke-width:2px

    class Internet internet
    class NAT,HostOnly network
    class DC server
    class Client client
```

## Network Design

The domain controller uses two virtual network adapters:

- **Adapter 1 — NAT:** Provides external network connectivity.
- **Adapter 2 — Host-Only:** Connects the domain controller, Windows 10 client, and host computer through an isolated lab network.

The domain controller provides Active Directory authentication, DNS, DHCP, and routing services to the Windows 10 client.

## IP Addressing

| Component | Address or Range | Purpose |
|---|---|---|
| Host-Only network | `172.16.0.0/24` | Lab subnet |
| Domain controller | `172.16.0.1/24` | Static server address |
| DHCP scope | `172.16.0.100–200` | Client addresses |
| Default gateway | `172.16.0.1` | RRAS routing |
| DNS server | `172.16.0.1` | Domain name resolution |
