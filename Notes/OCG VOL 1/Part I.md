# Intro to networking
---

# Chapter 1
## TCP/IP Networking

| **OSI Model** | **Layer** | **TCP/IP Model** |
| :------------ | :-------: | :--------------- |
| Application   |   7 ↔ 5   | Application      |
| Presentation  |     6     | ⤴️               |
| Session       |     5     | ⤴️               |
| Transport     |     4     | Transport        |
| Network       |     3     | Network          |
| Data Link     |     2     | Data Link        |
| Physical      |     1     | Physical         |


# Chapter 2
	- Optical fiber allows longer cabling distance between nodes
	- Crossover cable: If the endpoints transmit on the same pin pair
	- Straight-through cable: If the endpoints transmit on different pin pairs
	- PC/Router/WAP transmits on pins 1&2 - Switch/Hub transmits on pins 3&6
	- AUTO-MDIX: detect the cabling type and swap pins if needed
	- Multimode cabling improves the maximum distances over UTP and uses less expensive transmitters compared to Singlemode

## Cabling 

| **Speed** | **Common Name** | **Informal IEEE Standard Name** | **Formal IEEE Standard Name** |   **Cable Type**   | **Max Length** |
| :-------- | :-------------: | :------------------------------ | :---------------------------- | :----------------: | :------------- |
| 10 Mbps   |    Ethernet     | 10 BASE-T                       | 802.3                         |       Copper       | 100m           |
| 100 Mbps  |  Fast Ethernet  | 100 BASE-T                      | 802.3u                        |       Copper       | 100m           |
| 1000 Mbps |  Giga Ethernet  | 1000 BASE-LX                    | 802.3z                        | Multimode - Single | 550m - 5km     |
| 1000 Mbps |  Giga Ethernet  | 1000 BASE-T                     | 802.3ab                       |       Copper       | 100m           |
| 10 Gbps   | 10Giga Ethernet | 10G BASE-T                      | 802.3an                       |       Copper       | 100m           |
| 10 Gbps   | 10Giga Ethernet | 10G BASE-SR                     | 802.3ae                       |  Fiber Multimode   | 400m           |
| 10 Gbps   | 10Giga Ethernet | 10G BASE-LR                     | 802.3ae                       |  Fiber Singlemode  | 100m           |
| 10 Gbps   | 10Giga Ethernet | 10G BASE-ER                     | 802.3ae                       |  Fiber Singlemode  | 30km           |

- **Straight through:** Used for connecting different devices (PC → Switch)
	- Pins 1-2: Transmit
	- Pins 3-6: Receive
	- Pins 4-5, 7-8: Not used in 10/100 Mbps (used in Gigabit)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  ![[Straight through cable pins.png]]

- **Crossover:**  Used for connecting similar devices (PC → PC, Switch → Switch, Router → Router)
	- Pins 1-2 swap with pins 3-6, crossing the transmit and receive pairs so devices can communicate directly. 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  ![[Crossover cable pins.png]]


## Ethernet Frame
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![[Ethernet Frame.png]]

| **Field**                   | **Byte** | **Description**                                                                                                                               |
| :-------------------------- | :------- | :-------------------------------------------------------------------------------------------------------------------------------------------- |
| Preamble                    | 7        | Synchronization.                                                                                                                              |
| SFD (Start Frame Delimiter) | 1        | Signifies that the next byte begins the Destination MAC Address field.                                                                        |
| Destination                 | 6        | Identifies the intended recipient of this frame.                                                                                              |
| Source                      | 6        | Identifies the sender of this frame.                                                                                                          |
| Type                        | 2        | Defines the type of protocol listed inside the frame; today, most likely<br>identifies IPv4 or IPv6.                                          |
| Data                        | 46_1500  | Holds data from a higher layer, typically an L3PDU. The sender adds padding to meet the minimum length requirement for this field (46 bytes). |
| FCS (Frame Check Sequence)  | 4        | Provides a method for the receiving NIC to determine whether the frame experienced transmission errors.                                       |

## MAC Addressing
- **6 octets = 48 bits**
- **Broadcast:** `FF:FF:FF:FF:FF:FF`
- **Type (Bit 1 - Octet 1):**
  - `0` → Unicast
  - `1` → Multicast
- **If Unicast: (Bit 2 - Octet 1)**
  - `0` → Globally administered
  - `1` → Locally administered
- **Structure (6 octets total):**
  - **First 3 octets → OUI** (Organizationally Unique Identifier, vendor ID)
  - **Last 3 octets → NIC** (device interface identifier)

# Chapter 3

## Wide-Area Networks (WANs)
- WANs connect routers over long distances to enable communication between remote LANs.
- Two main WAN types covered:
  - **Leased-line WANs (serial links)** – older, point-to-point, private, reliable but costly and slower.
  - **Ethernet WANs** – modern, higher-speed WANs using Ethernet over long distances.
- Leased lines provide **Layer 1 services** only; routers must use **HDLC or PPP** at Layer 2.
- Common leased-line terms: leased circuit, serial link, point-to-point link, private line, T1.
- **HDLC vs PPP**:
  - Both encapsulate IP packets for WAN transmission.
  - PPP has more features; HDLC is simpler.
- Routers **de-encapsulate and re-encapsulate** frames at every hop; IP packets stay intact.

## Ethernet as a WAN Technology
- Ethernet WANs use Ethernet for both Layer 1 and Layer 2.
- Service provider creates a logical point-to-point Ethernet link between routers.
- Common names:
  - Ethernet WAN
  - Ethernet point-to-point link
  - Ethernet Line Service (E-Line)
  - Ethernet over MPLS (EoMPLS)
- Routers use standard **Ethernet frames**, but MAC addresses change at each hop.

## IP Routing Fundamentals
- IP (IPv4) is the primary **TCP/IP network-layer protocol**.
- IP focuses on **logical delivery**, not physical transmission.
- Routers make routing decisions based on the **destination IP address** only.
- Hosts send packets to:
  - The destination directly if on the same subnet
  - The **default gateway** if on a different subnet

## Routing Process (Forwarding Logic)
- Each router:
  1. Checks the incoming frame for errors (FCS).
  2. Removes the old data-link header/trailer.
  3. Looks up the destination IP in the routing table.
  4. Encapsulates the packet with a new data-link header.
- Data-link frames move packets **hop by hop**; IP packets move **end to end**.

## IP Addressing and Subnets
- IP addresses are grouped into **networks/subnets**.
- Devices on the same physical network must be in the same subnet.
- Devices separated by a router must be in different subnets.
- Subnetting reduces routing table size by grouping addresses.
- IP header:
  - 20 bytes
  - Contains **source and destination IP addresses**
  - Remains unchanged during routing.

## Routing Protocols
- Routing protocols allow routers to **learn routes dynamically**.
- Key functions:
  - Advertise known routes to neighbors.
  - Learn routes from neighbors.
  - Select best routes using metrics.
- Routers automatically add routes for **directly connected subnets**.
- Examples shown demonstrate how routes propagate hop-by-hop through the network.
