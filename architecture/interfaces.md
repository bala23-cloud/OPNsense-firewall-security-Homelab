# OPNsense Interface Configuration

## Overview

This document describes the network interfaces configured for the OPNsense firewall in the lab environment.

OPNsense acts as the central firewall and router between the external network and internal lab networks.

---

## Interface Summary

| Interface | VirtualBox Adapter | Network Type | IP Address | Role | Status |
|---|---|---|---|---|---|
| WAN | Adapter 1 | NAT | `10.0.x.x` | Internet / Upstream | Implemented |
| LAN | Adapter 2 | Internal Network | `192.168.x.x/24` | Internal Network | Implemented |
| OPT1 | Adapter 3 | Host-Only | `192.168.x.x/24` | Management | Optional |
| DMZ | Future Adapter | Internal Network | TBD | DMZ / Security Testing | Planned |

---

## 1. WAN Interface

### Purpose

The WAN interface provides OPNsense with connectivity to the external/upstream network.

VirtualBox NAT is used to provide upstream connectivity.

### Configuration

```text
Interface : WAN
Hardware  : le0
VirtualBox: Adapter 1
Network   : NAT
IP        : 10.0.x.x
Gateway   : VirtualBox NAT Gateway
Traffic Role
Internal Client
      |
      v
OPNsense LAN
      |
      v
Firewall
      |
      v
OPNsense WAN
      |
      v
VirtualBox NAT
      |
      v
Internet
```

---

## 2. LAN Interface

### Purpose

The LAN interface is the primary internal network used by the lab systems.

Ubuntu currently connects to this network and is used as the management/SOC system.

### Configuration

```text
Interface : LAN
Hardware  : le1
VirtualBox: Adapter 2
Network   : OPNSENSE-LAN
IP        : 192.168.x.x/24
Role      : Internal / Management
```

### Current Client

```text
Ubuntu
IP Address : 192.168.x.x
Gateway    : 192.168.x.x
Network    : 192.168.x.x/24
```

### Traffic Role

```text
Ubuntu
192.168.x.x
     |
     v
OPNsense LAN
192.168.x.x
     |
     v
Firewall Rules
     |
     v
Routing / NAT
     |
     v
WAN
```

---

## 3. OPT1 Interface

### Purpose

OPT1 is reserved for an additional network segment.

The original lab design planned to use OPT1 as a dedicated management interface connected through VirtualBox Host-Only networking.

### Configuration

```text
Interface : OPT1
Hardware  : le2
VirtualBox: Adapter 3
Network   : Host-Only
IP        : 192.168.x.x/24
Role      : Management
```

### Planned Management Network

```text
Windows Host
192.168.x.x
      |
      |
Host-Only Network
      |
      |
OPNsense OPT1
192.168.x.x
```

Note: The OPT1/Windows Host-Only management path is not currently relied upon for normal lab operation.

---

## 4. DMZ Interface

### Purpose

A dedicated DMZ interface will be introduced later in the project.

The DMZ will provide an isolated network for security-testing systems and intentionally exposed services.

### Planned Configuration

```text
Interface : DMZ
Hardware  : Future Adapter
Network   : Dedicated Internal Network
IP        : TBD
Role      : DMZ / Security Testing
```
