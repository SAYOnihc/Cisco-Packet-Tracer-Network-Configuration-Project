# Cisco Packet Tracer Network Configuration Project

## Why I Built This Project

My goal was to practice building a network that behaves like something a real organization could use—not just to make the devices connect, but to make the network organized, secure, reliable, and easier to troubleshoot.

In a business setting, networks support everything from employee access and department separation to communication between offices and access to outside services. A misconfigured VLAN, failed link, incorrect route, or missing security setting can interrupt work across an entire organization. This project gave me the opportunity to work through those kinds of problems in a controlled environment.

As you look through the project, you can see how I moved from basic connectivity to a more complete business-style network: separating departments with VLANs, restoring communication through routing, assigning addresses automatically with DHCP, sharing an outside connection with NAT, exchanging routes with OSPF, and adding redundancy with EtherChannel and Spanning Tree Protocol.

The screenshots show the evidence behind each step, while the explanations describe what changed, why it mattered, and how I verified that it worked.

## So, What Did I Build?

I built and configured a two-site network in Cisco Packet Tracer. The two sites communicate through a provider router, and the network includes switches, routers, PCs, VLANs, routing, DHCP, NAT, OSPF, EtherChannel, and Spanning Tree Protocol.

The goal was not just to connect a few devices. I had to make the network organized, secure, redundant, and able to move traffic between internal networks and the outside network.

## Network Topology

![Network topology](images/2.png)

Site 1 contains three switches and a router. Site 2 contains another router and a LAN. The provider router connects the two sites, but it was not directly available for configuration. That meant I had to work within the devices I could access and make my configurations fit the existing network.

## Implemented Configuration

### 1. Device Identification and Neighbor Discovery

First, I gave the routers and switches hostnames that matched their labels in the topology. Then I used Cisco Discovery Protocol to check whether neighboring devices could see one another. In other words, I was confirming that the devices were actually connected the way I expected.

![CDP neighbor verification](images/1.png)

### 2. Interface Activation

Before worrying about routing or VLANs, the physical connections had to work. I checked the topology and brought the necessary interfaces up until the links showed green in Packet Tracer.

![Operational topology](images/2.png)

### 3. IP Addressing and Management Connectivity

Next, I assigned IP addresses to the router interfaces and switch management interfaces. I verified the results with `show ip interface brief`, which quickly shows each interface, its IP address, and whether it is up or down.

![R1 interface addressing](images/3.1.png)
![S1 interface addressing](images/3.2.png)
![S2 interface addressing](images/3.3.png)
![S3 interface addressing](images/3.4.png)

### 4. Device Access Security

At this point, the devices were working, but they still needed controlled access. I configured privileged EXEC access, console access, and remote VTY access. The basic idea is simple: someone should not be able to connect to a network device and immediately change its configuration.

![S1 access configuration](images/4.1.png)
![S2 access configuration](images/4.2.png)
![S3 access configuration](images/4.3.png)
![Router access configuration](images/4.4.png)

> Passwords and other sensitive credentials should be removed or replaced before publishing this repository publicly.

### 5. VLANs and Trunking

The original network had a single main user network. I created VLAN 20 and named it `DEV` so development devices could be separated from the rest of the network.

Sooo, this is where one change caused another problem: once PC0 was placed into VLAN 20, it could no longer communicate normally with devices in VLAN 1. That was expected. VLANs are designed to separate broadcast domains, so the network needed a Layer 3 solution next.

I also configured the links between switches as trunk links. A trunk allows multiple VLANs to travel across one physical connection.

![S1 VLAN and trunk configuration](images/5.1.png)
![S2 VLAN and trunk configuration](images/5.2.png)
![S3 VLAN and trunk configuration](images/5.3.png)

### 6. Inter-VLAN Routing

So, VLAN 20 separated PC0 from VLAN 1. The fix was to configure router-on-a-stick on R1-SITE1. R1-SITE1 now acts as the Layer 3 connection between VLAN 1 and VLAN 20. I also selected a smaller subnet from `192.168.128.0/24`, sized for up to 100 hosts instead of using the entire /24 unnecessarily.

![Inter-VLAN routing configuration](images/6.png)

### 7. DHCP

After VLAN 20 could communicate through the router, devices still needed usable IP addresses. I configured DHCP on R1-SITE1 for the new subnet and excluded addresses reserved for static configuration. That means a new device can join VLAN 20 and receive the correct address, subnet mask, and gateway automatically.

![DHCP configuration](images/7.png)

### 8. NAT and PAT

The internal networks use private IP addresses, so those addresses cannot be sent directly across the public-facing connection. I configured source NAT with overload, also called PAT, on the WAN interface. Put simply, multiple internal devices can now share the router's outside address when reaching the provider network.

![NAT and PAT verification](images/8.png)

### 9. Default Routing

The router needed to know where to send traffic that was not destined for the internal `192.168.0.0/24` network. I added a static default route pointing toward the provider router. This gives R1-SITE1 a general “send everything else this way” path.

![Routing table verification](images/9.png)

### 10. OSPF

R2-SITE2 was mostly configured, but it still needed OSPF. I configured it to form a neighbor relationship with the provider router and advertise its connected networks. I also prevented OSPF hello packets from being sent toward the LAN interface.

![OSPF interface verification](images/10.png)

### 11. EtherChannel and Redundant Links

One connection between switches is a single point of failure. I added redundant links between the Site 1 switches, then grouped the links into channel groups. The result is better resilience and additional potential bandwidth.

![Redundant switch links](images/11.png)

### 12. Spanning Tree Protocol

Redundant links are useful, but they can also create switching loops. Spanning Tree Protocol solves that problem by deciding which paths should forward traffic and which paths should wait as backups. I configured S1-SITE1 as the preferred root bridge so traffic would follow the intended Layer 2 design.

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
