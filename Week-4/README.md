# Week-4 Network Configuration

## Overview
This document contains all IP addresses and interface configurations for the Week-4 network topology.

---

## Network Topology

```
PCE - SW1
SW1 - R1V6 
R1V6 - R2V6
R2V6 - ISP
ISP - R1
R1 - R2
R1 - MLS1
R1 - MLS2
MLS1 - MLS2 (Layer 3)
MLS1 - ALS1
MLS1 - ALS2
MLS2 - ALS1
MLS2 - ALS2
ALS1 - PC1 (VLAN 10)
ALS2 - PC2 (VLAN 20)
```

---

## IPv4 Configuration

### Router Loopback Addresses
| Device | Loopback0 | Purpose |
|--------|-----------|---------|
| ISP | 10.255.255.1/32 | OSPF Router ID |
| R1 | 10.255.255.2/32 | OSPF Router ID |
| R2 | 10.255.255.3/32 | OSPF Router ID |
| MLS1 | 10.255.255.4/32 | OSPF Router ID |
| MLS2 | 10.255.255.5/32 | OSPF Router ID |

### Point-to-Point Links (10.0.0.0/24 with /30 subnets)
| Link | Device 1 | Interface | IP | Device 2 | Interface | IP |
|------|----------|-----------|-----|----------|-----------|-----|
| ISP-R1 | ISP | Serial0/1/0 | 10.0.0.1/30 | R1 | Serial0/1/0 | 10.0.0.2/30 |
| R1-R2 | R1 | Serial0/1/1 | 10.0.0.5/30 | R2 | Serial0/1/0 | 10.0.0.6/30 |
| R1-MLS1 | R1 | GigabitEthernet0/0/0 | 10.0.0.9/30 | MLS1 | GigabitEthernet0/1 | 10.0.0.10/30 |
| R1-MLS2 | R1 | GigabitEthernet0/0/1 | 10.0.0.13/30 | MLS2 | GigabitEthernet0/1 | 10.0.0.14/30 |
| MLS1-MLS2 | MLS1 | GigabitEthernet0/2 | 10.0.0.17/30 | MLS2 | GigabitEthernet0/2 | 10.0.0.18/30 |

### VLAN Configuration (10.0.{VLAN}.0/24)

#### VLAN 10 (Gateway on R1)
| Device | VLAN | Interface | IP Address | Purpose |
|--------|------|-----------|------------|---------|
| R1 | 10 | VLAN10 | 10.0.10.1/24 | Gateway |
| MLS1 | 10 | VLAN10 | 10.0.10.2/24 | VRRP Primary (Priority 110) |
| MLS2 | 10 | VLAN10 | 10.0.10.3/24 | VRRP Secondary (Priority 100) |
| VRRP VIP | 10 | - | 10.0.10.1/24 | Virtual IP |
| PC1 | 10 | - | DHCP | Client |

#### VLAN 20 (Gateway on R1)
| Device | VLAN | Interface | IP Address | Purpose |
|--------|------|-----------|------------|---------|
| R1 | 20 | VLAN20 | 10.0.20.1/24 | Gateway |
| MLS1 | 20 | VLAN20 | 10.0.20.3/24 | VRRP Secondary (Priority 100) |
| MLS2 | 20 | VLAN20 | 10.0.20.2/24 | VRRP Primary (Priority 110) |
| VRRP VIP | 20 | - | 10.0.20.1/24 | Virtual IP |
| PC2 | 20 | - | DHCP | Client |

### R2 VLAN Configuration (for summarization)

#### VLAN 32
| Device | Interface | IP Address |
|--------|-----------|------------|
| R2 | VLAN32 | 192.168.32.1/24 |
| R2 | Loopback1 | 192.168.32.0/32 |

#### VLAN 33
| Device | Interface | IP Address |
|--------|-----------|------------|
| R2 | VLAN33 | 192.168.33.1/24 |
| R2 | Loopback2 | 192.168.33.0/32 |

#### VLAN 34
| Device | Interface | IP Address |
|--------|-----------|------------|
| R2 | VLAN34 | 192.168.34.1/24 |
| R2 | Loopback3 | 192.168.34.0/32 |

#### VLAN 35
| Device | Interface | IP Address |
|--------|-----------|------------|
| R2 | VLAN35 | 192.168.35.1/24 |
| R2 | Loopback4 | 192.168.35.0/32 |

**Summary Route (advertised as ABR):** 192.168.32.0/22

---

## IPv6 Configuration

### Router Loopback Addresses
| Device | Loopback0 | Purpose |
|--------|-----------|---------|
| ISP | 2001:db8::1/128 | OSPFv3 Router ID |
| R1V6 | 2001:db8::2/128 | OSPFv3 Router ID |
| R2V6 | 2001:db8::3/128 | OSPFv3 Router ID |
| SW1 | 2001:db8::100/128 | Management |

### IPv6 Point-to-Point Links

| Link | Device 1 | Interface | IPv6 Address | Device 2 | Interface | IPv6 Address |
|------|----------|-----------|--------------|----------|-----------|--------------|
| ISP-R1 | ISP | Serial0/1/0 | 2001:db8:1::1/127 | R1 | Serial0/1/0 | 2001:db8:1::1/127 |
| ISP-R2V6 | ISP | Serial0/1/1 | 2001:db8:1::2/127 | R2V6 | Serial0/1/1 | 2001:db8:1::2/127 |
| R1V6-R2V6 | R1V6 | Serial0/1/0 | 2001:db8:2::1/127 | R2V6 | Serial0/1/0 | 2001:db8:2::2/127 |
| R1V6-SW1 | R1V6 | GigabitEthernet0/0/0 | 2001:db8:100::1/127 | SW1 | GigabitEthernet0/1 | 2001:db8:100::2/127 |
| SW1-PCE | SW1 | GigabitEthernet0/2 | 2001:db8:101::1/127 | PCE | - | 2001:db8:101::2/127 |

---

## Device Details

### ISP Router
**Hostname:** ISP  
**Router ID:** 10.255.255.1

#### Interfaces:
- **Loopback0:** 10.255.255.1/32, 2001:db8::1/128
- **Serial0/1/0:** ISP-to-R1(Serial0/1/0)
  - IPv4: 10.0.0.1/30
  - IPv6: 2001:db8:1::1/127
- **Serial0/1/1:** ISP-to-R2V6(Serial0/1/1)
  - IPv6: 2001:db8:1::2/127

#### Routing:
- **OSPF AS 1:** Area 0
- **OSPFv3 AS 1:** Area 0
- **Default Route:** Originating (redistributed in OSPF)

---

### R1 Router
**Hostname:** R1  
**Router ID:** 10.255.255.2

#### Interfaces:
- **Loopback0:** 10.255.255.2/32
- **Serial0/1/0:** R1-to-ISP(Serial0/1/0)
  - IPv4: 10.0.0.2/30
  - IPv6: 2001:db8:1::1/127
- **Serial0/1/1:** R1-to-R2(Serial0/1/0)
  - IPv4: 10.0.0.5/30
- **GigabitEthernet0/0/0:** R1-to-MLS1(GigabitEthernet0/1)
  - IPv4: 10.0.0.9/30
- **GigabitEthernet0/0/1:** R1-to-MLS2(GigabitEthernet0/1)
  - IPv4: 10.0.0.13/30
- **VLAN10:** 10.0.10.1/24 (Gateway)
- **VLAN20:** 10.0.20.1/24 (Gateway)

#### Routing:
- **OSPF AS 1:** Area 0
- **Default Route:** Redistributed from ISP

---

### R2 Router
**Hostname:** R2  
**Router ID:** 10.255.255.3

#### Interfaces:
- **Loopback0:** 10.255.255.3/32
- **Loopback1-4:** 192.168.32.0/32, 192.168.33.0/32, 192.168.34.0/32, 192.168.35.0/32 (Summary routes)
- **Serial0/1/0:** R2-to-R1(Serial0/1/1)
  - IPv4: 10.0.0.6/30
- **VLAN32:** 192.168.32.1/24
- **VLAN33:** 192.168.33.1/24
- **VLAN34:** 192.168.34.1/24
- **VLAN35:** 192.168.35.1/24

#### Routing:
- **OSPF AS 1:** Area 0
- **Summary Route:** 192.168.32.0/22 (ABR announcement)

---

### R1V6 Router
**Hostname:** R1V6  
**Router ID:** 10.255.255.3 (for OSPFv3)

#### Interfaces:
- **Loopback0:** 2001:db8::2/128
- **GigabitEthernet0/0/0:** R1V6-to-SW1(GigabitEthernet0/1)
  - IPv6: 2001:db8:100::1/127
- **Serial0/1/0:** R1V6-to-R2V6(Serial0/1/0)
  - IPv6: 2001:db8:2::1/127

#### Routing:
- **OSPFv3 AS 1:** Area 0

---

### R2V6 Router
**Hostname:** R2V6  
**Router ID:** 10.255.255.4 (for OSPFv3)

#### Interfaces:
- **Loopback0:** 2001:db8::3/128
- **Serial0/1/0:** R2V6-to-R1V6(Serial0/1/0)
  - IPv6: 2001:db8:2::2/127
- **Serial0/1/1:** R2V6-to-ISP(Serial0/1/1)
  - IPv6: 2001:db8:1::2/127

#### Routing:
- **OSPFv3 AS 1:** Area 0

---

### MLS1 (Multilayer Switch)
**Hostname:** MLS1  
**Router ID:** 10.255.255.4

#### Interfaces:
- **Loopback0:** 10.255.255.4/32
- **GigabitEthernet0/1:** MLS1-to-R1(GigabitEthernet0/0/0)
  - IPv4: 10.0.0.10/30
- **GigabitEthernet0/2:** MLS1-to-MLS2(GigabitEthernet0/2) [Layer 3]
  - IPv4: 10.0.0.17/30
- **GigabitEthernet0/3:** MLS1-to-ALS1(GigabitEthernet0/1)
- **GigabitEthernet0/4:** MLS1-to-ALS2(GigabitEthernet0/1)
- **VLAN10:** 10.0.10.2/24 (VRRP Primary - Priority 110)
- **VLAN20:** 10.0.20.3/24 (VRRP Secondary - Priority 100)

#### Spanning Tree:
- **Root:** VLAN 10 (Priority 8192)
- **Secondary:** VLAN 20

#### Routing:
- **OSPF AS 1:** Area 0

---

### MLS2 (Multilayer Switch)
**Hostname:** MLS2  
**Router ID:** 10.255.255.5

#### Interfaces:
- **Loopback0:** 10.255.255.5/32
- **GigabitEthernet0/1:** MLS2-to-R1(GigabitEthernet0/0/1)
  - IPv4: 10.0.0.14/30
- **GigabitEthernet0/2:** MLS2-to-MLS1(GigabitEthernet0/2) [Layer 3]
  - IPv4: 10.0.0.18/30
- **GigabitEthernet0/3:** MLS2-to-ALS1(GigabitEthernet0/2)
- **GigabitEthernet0/4:** MLS2-to-ALS2(GigabitEthernet0/2)
- **VLAN10:** 10.0.10.3/24 (VRRP Secondary - Priority 100)
- **VLAN20:** 10.0.20.2/24 (VRRP Primary - Priority 110)

#### Spanning Tree:
- **Root:** VLAN 20 (Priority 8192)
- **Secondary:** VLAN 10

#### Routing:
- **OSPF AS 1:** Area 0

---

### ALS1 (Access Layer Switch)
**Hostname:** ALS1

#### Interfaces:
- **GigabitEthernet0/1:** ALS1-to-MLS1(GigabitEthernet0/3)
- **GigabitEthernet0/2:** ALS1-to-MLS2(GigabitEthernet0/3)
- **GigabitEthernet0/3:** ALS1-to-PC1(Interface0) - VLAN 10

#### VLANs:
- **VLAN 10:** Active

---

### ALS2 (Access Layer Switch)
**Hostname:** ALS2

#### Interfaces:
- **GigabitEthernet0/1:** ALS2-to-MLS1(GigabitEthernet0/4)
- **GigabitEthernet0/2:** ALS2-to-MLS2(GigabitEthernet0/4)
- **GigabitEthernet0/3:** ALS2-to-PC2(Interface0) - VLAN 20

#### VLANs:
- **VLAN 20:** Active

---

### SW1 (Switch for IPv6)
**Hostname:** SW1

#### Interfaces:
- **Loopback0:** 2001:db8::100/128
- **GigabitEthernet0/1:** SW1-to-R1V6(GigabitEthernet0/0/0)
  - IPv6: 2001:db8:100::2/127
- **GigabitEthernet0/2:** SW1-to-PCE(Interface0)
  - IPv6: 2001:db8:101::1/127

---

## Summary

### Total IPv4 Subnets
- **Router-to-Router Links:** 5 × /30 subnets (10.0.0.0/30 range)
- **VLAN Networks:** 2 × /24 subnets (VLAN 10 & 20)
- **R2 VLANs:** 4 × /24 subnets (VLAN 32-35)
- **Loopback Addresses:** 5 × /32

### Total IPv6 Subnets
- **Point-to-Point Links:** 5 × /127 subnets
- **Loopback Addresses:** 4 × /128

### Routing Protocols
- **OSPF:** IPv4 Area 0 with default route redistribution and VLAN summarization
- **OSPFv3:** IPv6 Area 0
- **VRRP:** Active/Standby redundancy on VLANs 10 & 20
- **NAT-PT:** IPv4 to IPv6 translation at ISP

---

## Access Information

All devices use:
- **Username:** user
- **Password:** cisco
- **Enable Password:** class
- **Domain:** hhs.nl
- **SSH:** Enabled (RSA 2048-bit keys)

---

Last Updated: 4 January 2026
