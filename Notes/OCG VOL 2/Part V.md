# Network Architecture
---
# Chapter 18
```
- Access switch refers to switches used to connect to endpoint devices.
- Distribution switch refers to switches that don't connect to endpoints but rather link each access switch.
- Up-link refers to the switch-to-switch links (usually trunks) between access and distribution switches.
- UTP cable category: CAT 3, CAT 5, CAT 5E, CAT 6, CAT 6A, and CAT 8.
- Multigig Ethernet The common name for the 2.5G and 5G Ethernet standards, which represented an option for UTP 
  Ethernet at speeds of multiple gigabits between the standard speeds of 1 Gbs and 10 Gbs.
- Optical Miltimode: the category of multimode Fiber cables (OM1, OM2, OM3 and OM4).
- PoE performs power detection then power classification.
- When planning a LAN design that includes PoE consider: Powered devices, Power requirements, Switch ports, 
  Switch power supplies, PoE standards versus actual.
```

- **ANSI/TIA Cable Category**

| **Standard <br>(common)** | **Standard<br>(original)** | **Year of<br>Standard** | **ANSI/TIA<br>Category** | **Speed** |
| :------------------------ | :------------------------- | ----------------------- | ------------------------ | --------- |
| 10 BASE-T                 | 802.3                      | 1990                    | CAT 3                    | 10 Mbs    |
| 100 BASE-T                | 802.3u                     | 1995                    | CAT 5                    | 100 Mbs   |
| 1000 BASE-T               | 802.3ab                    | 1999                    | CAT 5E                   | 1 Gbs     |
| 10G BASE-T                | 802.3an                    | 2006                    | CAT 6                    | 10 Gbs    |
| 40G Base-T                | 802.3ba                    | 2010                    | CAT 8                    | 40 Gbs    |
| 2.5G Base-T               | 802.3bz                    | 2016                    | CAT 5E                   | 2.5 Gbs   |
| 5G Base-T                 | 802.3bz                    | 2016                    | CAT 5E                   | 5 Gbs     |
- **ISO Cable Category**

| **ISO<br>Category** | **Core/Cladding<br>Diameter** | **1000BASE-SX <br>Max Distance** | **10GBASE-SR<br>Max Distance** |
| ------------------- | ----------------------------- | -------------------------------- | ------------------------------ |
| OM1                 | 62.5/125                      | 220m                             | 33m                            |
| OM2                 | 50/125                        | 550m                             | 82m                            |
| OM3                 | 50/125                        | N/A                              | 300m                           |
| OM4                 | 50/125                        | N/A                              | 400m                           |

- **PoE Standards**

| **Name** | **Standard** | **Watts at PSE** | **Power Class** | **Powered Wire Pairs** |
| -------- | ------------ | ---------------- | --------------- | ---------------------- |
| PoE      | 802.3af      | 15               | 0               | 2                      |
| PoE+     | 802.3at      | 30               | 4               | 2                      |
| UPoE     | 802.3bt      | 60               | 6               | 4                      |
| UPoE+    | 802.3bt      | 90               | 8               | 4                      |
# Chapter 19
```
- A Point of Presence (PoP) is an Ethernet switch installed by SP that's physically near as many customer sites 
  as possible in an SP facility, used to create the Metro Ethernet Service.
- Ethernet Virtual Connection (EVC) is a service that allows two endpoints to communicate.
- MPLS VPN is a WAN service that uses MPLS technology, with many customers connecting to the same MPLS network, 
  but with the VPN features keeping each customer’s traffic separate from others.
- The customer edge (CE) refers to to a router at the customer site.
- The provider edge (PE) devices sit at the edge of the SP’s network.
- Internet Access use DSL, Cable Internet (coaxial), Wireless Wan (4g/5g), Fiber (Ethernet).
```

- **IEEE Ethernet Standards For Metro Ethernet Access**

| **Name**      | **Speed** | **Distance** |
| ------------- | --------- | ------------ |
| 100BASE-LX10  | 100 Mbs   | 10 Km        |
| 1000BASE-LX   | 1 Gbs     | 5 Km         |
| 1000BASE-LX10 | 1 Gbs     | 10 Km        |
| 1000BASE-ZX   | 1 Gbs     | 100 Km       |
| 10GBASE-LR    | 10 Gbs    | 10 Km        |
| 10GBASE-ER    | 10 Gbs    | 40 Km        |
- **Metro Ethernet Topologies**
	- *E-Line (VPWS):* Two customer premise equipment (CPE) devices can exchange Ethernet frames, similar in concept to a leased line (point-to-point topology).
	- *E-LAN (VPLS/EoMPLS):* This service acts like a LAN so that all devices can send frames to all other devices (full-mesh).
	- *E-Tree (hub&spoke):* A central site can communicate with a defined set of remote sites, but the remote sites cannot communicate directly (partial-mesh).
- **Multiprotocol Label Switching**
	- MPLS creates a WAN service that routes IP packets between customer sites.
	- MPLS VPN: 
		- Builds routing protocol neighbor relationships with customer routers.
		- Learn customer subnets/routes with those routing protocols.
		- advertise a customer’s routes with a routing protocol so that all routers connected to the MPLS VPN can learn all routes as advertised through the MPLS VPN network.
		- Makes decisions about MPLS VPN forwarding, including what MPLS labels to add and remove, based on the customer’s IP address space and customer IP routes.
- **Internet VPN**
	- Provides essential security features, such as:
		- *Confidentiality:* Preventing anyone in the middle of the Internet (MITM) from being able to read the data.
		- *Authentication:* Verifying that the sender of the VPN packet is a legitimate device and not a device used by an attacker.
		- *Data integrity:* Verifying that nothing changed the packet as it transited the Internet.
		- *Anti-replay:* Preventing a man in the middle from copying packets sent by a legitimate user, so they can later send those packets and appear to be that legitimate user.
	- Devices use Generic Routing Encapsulation (GRE) to create a tunnel with IPsec.
	- VPN Types :
		- *Site-to-Site:* provides VPN services for the devices at two sites with a single VPN tunnel using IPsec.
		- *Remote Access (Client VPN):* 
			- A software used by a client to access the enterprise's network securely using TLS. 
			- Cisco has replaced the Cisco AnyConnect Secure Mobility Client software with the Cisco Secure Client.

| **Attribute**                                              | **Site-to-Site** | **Remote Access** |
| ---------------------------------------------------------- | ---------------- | ----------------- |
| Does the end-user <br>device need VPN <br>client software? | No               | Yes               |
| Devices supported <br>by one VPN                           | Many             | One               |
| Typical use                                                | Permanent        | On-demand         |
| Does the VPN use <br>IPsec tunnel mode                     | Yes              | No                |
| Does the VPN use <br>IPsec transport mode                  | No               | Yes               |
# Chapter 20
```
- Hypervisor: Software that runs on server hardware to create the foundations of a virtualized server environment 
  primarily by allocating server hardware components to the VMs running on the server.
- Cloud Computing's Characteristics: On-demand self-service, Broad network access, Resource pooling, Rapid-
  elasticity, Measured service.
- Cloud's Deployement Models: Private Cloud, Public Cloud, Hybrid Cloud, Community Cloud.
- Virtual Routing & Forwarding (VRF): makes one router act as multiple routers by assigning interfaces and 
  neighbors to specific VRFs, with related routes landing in the associated VRF-unique routing table.
- By using a separate IP routing table per VRF, the router can support overlapping subnets by placing them in 
  different VRFs.
- Public Cloud <-> Internet: Pros (Agility, Migration, Remote work) Cons (Security, Capacity, QoS, No WAN SLA).
- Public Cloud <-> Private WAN (Ethernet WAN | MPLS VPN): Pros(Security, QoS) Cons (Capacity, Migration, Cost).
- Public Cloud <-> Internet VPN: Pros (Security, Migration) Cons (QoS).
- Intercloud Echange: refers to any company that creates a private network as a service. on one side, it connects 
  to multiple cloud providers. On the other side, the intercloud connects to cloud consumers.
- Using intercloud companies get the same benefits as when connecting with a private WAN connection to a public 
  cloud but with the additional pro of easier migration to a new cloud provider. The main con is that using an 
  intercloud exchange introduces another company.
- Cloud Management's benefits: Centralized management, Accessibility, Scale, Automation, Security, Lower cost of 
  ownership.
- Cisco Meraki: a cloud-based management solution for onsite devices. 
- Meraki Dashboard Advantages: Zero touch provisioning (ZTP), Topology, Path visualization, Single-click into 
  devices, Color-coded states of operation.
```

- **Comparison of Public Cloud WAN Options**

|                                   | Internet | Internet VPN | MPLS VPN | Ethernet WAN | Intercloud |
| --------------------------------- | -------- | ------------ | -------- | ------------ | ---------- |
| Makes data private                | No       | Yes          | Yes      | Yes          | Yes        |
| Supports QoS                      | No       | No           | Yes      | Yes          | Yes        |
| Requires capacity planning        | Yes      | Yes          | Yes      | Yes          | Yes        |
| Eases migration to a new provider | Yes      | Yes          | No       | No           | Yes        |
| Speeds initial installation       | Yes      | Yes          | No       | No           | No         |
- **Cloud Management and Traditional Management Comparison**

| **Use Case**                      | **Cloud Management**                                                                                                                     | **Traditional Management**                                                                                               |
| --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Centralized  device<br>management | Typically, a single point of management for all devices                                                                                  | Unless using a third-party NMS or tool, devices are typically managed individually                                       |
| Scale and speed of deployment     | Zero touch provisioning                                                                                                                  | Manual or automated provisioning through scripting or third-party tools                                                  |
| Security policy and enforcement   | Security polices can be applied to multiple devices <br>within a single central dashboard                                                | Unless using third-party tools or automation, typically security policies have to be applied on a device-by-device basis |
| Troubleshooting                   | Centralized management makes troubleshooting much more effective due  to the ability to see the <br>whole network in a single  dashboard | Typically is more difficult and time-consuming from having to  troubleshoot on a device-by-device basis                  |
| Monitoring and <br>reporting      | Cloud management enables  you to monitor your network  centrally to see and report on issues across all devices                          | Traditional networks monitor or report on devices using a third-party tool, which may require you to correlate the data  |
