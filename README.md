# Cisco Packet Tracer Network Configuration Project

## Overview

This project documents the design and configuration of a multi-site network in Cisco Packet Tracer. The topology includes two sites connected through a provider router, with VLAN segmentation, inter-VLAN routing, DHCP, NAT/PAT, OSPF, EtherChannel, and Spanning Tree Protocol.

The project demonstrates practical Cisco IOS configuration, IPv4 subnetting, Layer 2 switching, Layer 3 routing, network security, and connectivity validation.

## Network Topology

![Network topology](images/2.png)

The topology contains two simulated sites connected through a provider network. Site 1 includes three switches and a router, while Site 2 includes a router and LAN devices.

## Implemented Configuration

### 1. Device Identification and Neighbor Discovery

Configured hostnames on the routers and switches and verified neighboring devices using Cisco Discovery Protocol.

![CDP neighbor verification](images/1.png)

### 2. Interface Activation

Verified that the required physical and data-link interfaces were operational and displayed green status in Packet Tracer.

![Operational topology](images/2.png)

### 3. IP Addressing and Management Connectivity

Configured router interfaces and switch management interfaces using the assigned IPv4 networks. Interface status and addressing were verified with `show ip interface brief`.

![R1 interface addressing](images/3.1.png)
![S1 interface addressing](images/3.2.png)
![S2 interface addressing](images/3.3.png)
![S3 interface addressing](images/3.4.png)

### 4. Device Access Security

Configured privileged EXEC access, console access, and remote VTY access on the network devices. Password protection and VTY line settings were verified with `show running-config | section line`.

![S1 access configuration](images/4.1.png)
![S2 access configuration](images/4.2.png)
![S3 access configuration](images/4.3.png)
![Router access configuration](images/4.4.png)

> Passwords and other sensitive credentials should be removed or replaced before publishing this repository publicly.

### 5. VLANs and Trunking

Created VLAN 20 for the development network, assigned the appropriate access port, propagated the VLAN across the switching environment, and configured switch uplinks as trunks.

![S1 VLAN and trunk configuration](images/5.1.png)
![S2 VLAN and trunk configuration](images/5.2.png)
![S3 VLAN and trunk configuration](images/5.3.png)

### 6. Inter-VLAN Routing

Configured router-on-a-stick on R1-SITE1 so that devices in VLAN 1 and VLAN 20 could communicate. VLAN 20 uses a subnet sized for up to 100 hosts rather than the entire assigned /24 network.

![Inter-VLAN routing configuration](images/6.png)

### 7. DHCP

Configured R1-SITE1 to provide DHCP service for the new VLAN 20 subnet while excluding statically assigned addresses.

![DHCP configuration](images/7.png)

### 8. NAT and PAT

Configured source NAT with overload so internal VLAN traffic could share the WAN interface address when communicating with the provider network.

![NAT and PAT verification](images/8.png)

### 9. Default Routing

Configured a static default route on R1-SITE1 pointing toward the provider router for traffic destined outside the internal network.

![Routing table verification](images/9.png)

### 10. OSPF

Configured OSPF on R2-SITE2, formed an adjacency with the provider router, advertised connected networks, and prevented OSPF hello packets from being sent toward the LAN interface.

![OSPF interface verification](images/10.png)

### 11. EtherChannel and Redundant Links

Added redundant links between the Site 1 switches and configured channel groups to provide link redundancy and increased bandwidth.

![Redundant switch links](images/11.png)

### 12. Spanning Tree Protocol

Configured S1-SITE1 as the preferred root bridge so Layer 2 traffic would follow the intended path even if additional default-configured switches were introduced.

![Spanning Tree verification](images/12.png)

## Skills Demonstrated

- Cisco IOS command-line configuration
- IPv4 addressing and subnetting
- VLAN creation and access-port assignment
- 802.1Q trunking
- Router-on-a-stick inter-VLAN routing
- DHCP configuration
- NAT and PAT
- Static routing and OSPF
- EtherChannel
- Spanning Tree Protocol
- Network troubleshooting and verification

## Evidence and Project Files

The `images/` directory contains configuration and verification screenshots organized by project section. The Packet Tracer `.pkt` file can be added to the repository so the topology can be opened and inspected directly.

## Verification Commands

Examples of commands used to validate the configuration include:

```text
show cdp neighbors
show ip interface brief
show vlan brief
show interfaces trunk
show ip route
show ip ospf interface
show ip nat statistics
show spanning-tree
```

## Notes for Publication

Before making the repository public, remove real credentials, personal identifiers, student IDs, and any unrelated course materials. Use placeholder passwords in configuration files intended for public viewing.
