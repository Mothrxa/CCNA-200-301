# OSPF
---
# Chapter 21

```
- If a router learns two (or more) routes via the same routing protocol to the same destination with the same 
  metric, both will be added to routing table and traffic will be load balanced (this is called Equal Cost Multi-
  Path (ECMP)).
- Administrative Distance is used to determine which routing protocol is preferred, A lower AD is preferred.
- IP routing protocols fall into one of two major categories that are IGP & EGP (internal\external gateway 
  Protocol).
- IGP is a routing protocol that was designed and intended for use inside a single autonomous system.
- EGP is routing protocol that was designed and intended for use between different autonomous systems.
- BGP (Border Gateway Protocol) is the only EGP.
- Routers exchange data with LSAs stored inside LSDB.
- LSA message contains Router ID, Router IP address, State, Cost.
- Router flood the forwarded LSA to the next neighbor only if the neighbor has not yet learned about the LSA.
- OSPF states are init, full, 2-way
- Routers sends Hello messages after X seconds as configured in the Hello interval.
- Routers marks a node down if it stops receiving Hello msgs after X seconds as configured in the dead interval.
- Each router that creates an LSA also has the responsibility to reflood the LSA every 30 minutes (the default), 
  even if no changes occur.
```

- **IGP Routing Algorithms**
	- *Distance vector* {RIP} (Bellman-Ford).
	- *Advanced distance vector {IGRP | EIGRP}* (Balanced hybrid).
	- *Link-state {OSPF | IS-IS}*.
- **Metrics**
	- RIP uses a counter of the number of routers (hops).
	- OSPF totals the cost associated with each interface in the end-to-end route with the cost based on link bandwidth.
	- EIGRP Calculates based on the route’s slowest link and the cumulative delay associated with each interface in the route.

$$Interior\ IP\ Routing\ Protocols\ Compared$$

| Feature                       | RIPv2                | EIGRP                    | OSPF                 |
| ----------------------------- | -------------------- | ------------------------ | -------------------- |
| Routing type                  | Distance Vector (DV) | Advanced Distance Vector | Link State (LS)      |
| Classless                     | Yes                  | Yes                      | Yes                  |
| Sends subnet mask in updates  | Yes                  | Yes                      | Yes                  |
| Supports VLSM                 | Yes                  | Yes                      | Yes                  |
| Algorithm used                | DV                   | Advanced DV              | LS                   |
| Supports manual summarization | Yes                  | Yes                      | Yes*                 |
| Cisco proprietary             | No                   | Yes                      | No                   |
| Routing updates               | Periodic             | Partial / Triggered      | Triggered            |
| Convergence speed             | Slow                 | Fast                     | Fast                 |
| Multicast address(es) used    | 224.0.0.9            | 224.0.0.10               | 224.0.0.5, 224.0.0.6 |

- **OSPF Neighbors**
	- Routers send OSPF Hello messages, introducing themselves to the potential neighbor.
	- Routers listen for OSPF Hello messages from new routers and react to those messages, attempting to become neighbors and exchange LSDBs.
	- OSPF Hello messages follows the IP packet header with IP protocol type 89.
	- Hello packets are sent to multicast IP address 224.0.0.5, a multicast IP address intended for all OSPF-speaking routers.
	- OSPF routers listen for packets sent to IP multicast address 224.0.0.5, in part hoping to receive Hello packets and learn about new neighbors.
	- When routers reach a 2-way state with each other, they are neighbors and ready to exchange their LSDB with each other.
	- the OSPF messages that actually send the LSAs between neighbors are called link-state update (LSU) packets.
	- **Maintaining Neighbors & LSDB**
		- Maintain neighbor state by sending Hello messages based on the Hello interval and listening for Hellos before the Dead interval expires.
		- Flood any changed LSAs to each neighbor.
		- Reflood unchanged LSAs as their lifetime expires (default 30 minutes).
	- Routers that are neither a DR nor a BDR are called DROthers by OSPF as they never reach a full state because they don't exchange LSDBs directly with each other. As a result, the **show ip ospf neighbor** command on these DROther routers lists some neighbors in a 2-way state, remaining in that state under normal operation. 

| **Neighbor state** | **Term for Neighbor** | **Term for Relationship** |
| ------------------ | --------------------- | ------------------------- |
| 2-way              | Neighbor              | Neighbor Relationship     |
| full               | Adjacent Neighbor     | Adjacency                 |

- **OSPF Area**
	- It separates **Layer 3 routing domains**.
	- It limits routing updates (LSAs).
	- Interfaces on the same subnet must use the same OSPF area.
	- Every router in the area can reach every other router in that area **without leaving the area**
	- If **all interfaces** are in the same area (not the backbone area) → the router is an **internal router**.
	- A router becomes an **ABR** when at least one interface is in **Area 0** and at least one interface is in a **non-backbone area**.
	- All non-backbone areas must have a path to reach the backbone area (area 0) by having at least one ABR connected to both the backbone area and the non-backbone area.
- **Link-State Advertisements**
	- OSPF defines the first two types of LSAs to define those exact details, as follows:
		- One router LSA for each router in the area.
		- One network LSA for each network that has a DR plus one neighbor of the DR.
		- One summary LSA for each subnet ID that exists in a different area.
	- Router LSAs Build Most of the Intra-Area Topology.
	- Network LSAs Complete the Intra-Area Topology.
 
 - **LSA Type**

| **LSA Name** | **LSA Type** | **Primary Purpose**              | **Contents of LSA**                                                |
| -------- | -------- | ---------------------------------------- | ------------------------------------------------------------------ |
| Router   | 1        | Describe a router                        | RID, interfaces, IP address/mask, current interface state (status) |
| Network  | 2        | Describe a network that has a DR and BDR | DR and BDR IP addresses, subnet ID, mask                           |
| Summary  | 3        | Describe a subnet in another area        | Subnet ID, mask, RID of ABR that advertises the LSA                |

### Configuration commands
- (router)#show ip ospf neighbor [**interface-name**]
- (router)#show ip ospf database
- (router)#show ip ospf interface [**interface-name** | brief]
- (router)#show ip ospf rib
- (router)#show ip protocols
!- type number means the interface, example: Fa0/0, FastEthernet is the type and  0/0 is the interface number
# Chapter 22
```
- OSPF router ID is configured by setting a router ID, setting a loopback IP address or by default takes the 
  highest interface IP address.
- By default routers choose an interface IP address to use as the RID.
- Routers with interfaces in multiple areas are called area border routers (ABRs)
```

### Configuration commands
- (config)#router ospf **process-id** (enters ospf configuration mode)
- (config-router)#router-id **id-value** (sets the router's ID)
- (config-router)#network **ip-address** **wildcard-mask** area **area-id**
- (config-router)#auto-cost reference-bandwidth ***megabits-per-second***
- (config-if)#ip ospf **process-id** area **area-id**
- (config-if)#ip ospf cost **interface-cost**
- (config-if)#bandwidtch ***kilobits-per-second*** (should be avoided)

# Chapter 23
 ```
- OSPF defaults to use a broadcast network type on all types of Ethernet interfaces unless changed manually.
- DR/BDR election happens only on broadcast networks.
- DROTHER routers are in Full state with DR/BDR but only 2-WAY with each other.
- Using a network type of point-to-point tells the router to not use a DR/BDR on the link.
- E-lines are Ethernet WAN links that uses point-to-point.
- Many engineers prefer to instead use an OSPF P2P network type on Ethernet WAN links that act as a P2P link.
- The “Gateway of last resort” refers to the default route currently used by the router.
 ```

- **OSPF Network type**

| **Type**           | **Dynamic Neighbor's Discovery** | **Uses DR/BDR** |
| ------------------ | -------------------------------- | --------------- |
| **broadcast**      | Yes                              | Yes             |
| **point-to-point** | Yes                              | No              |
- **DR/BDR Election**
	- If the DR fails, the BDR becomes the DR and a new BDR is elected.
	- When a better router enters the subnet, no preemption of the existing DR or BDR occurs.
	- Election happens only in DR failure.
	- The highest OSPF interface priority wins during an election with values ranging from 0 to 255 (A value of 0 prevents the router from ever becoming the DR).
	- If the priority ties, the election chooses the router with the highest OSPF RID.
- **Passive Interfaces**
	- OSPF continues to advertise about the subnet that is connected to the interface.
	- OSPF no longer sends OSPF Hellos on the interface.
	- OSPF no longer processes any received Hellos on the interface.
	- The result of enabling OSPF on an interface but then making it passive is that OSPF still advertises about the connected subnet, but OSPF also does not form neighbor relationships over the interface.
- **Defaults routes**
	- A default route is not needed when forwarding packets between subnets inside a company.
	- One router has a default route that points toward the internet.
	- All routers should dynamically learn a default route, used for all traffic going to the Internet, so that all packets destined to locations in the Internet go to the one router connected to the Internet.
	- the OSPF subcommand **default-information originate** makes the logic conditional: only advertise a default route into OSPF if you already have some other default route. 
	- Adding the always keyword to the command tells OSPF to always advertise a default route into OSPF regardless of whether a default route currently exists.
- **Metrics**
	- OSPF interface cost is configured with : 
		- Directly, using the interface subcommand **ip ospf cost x**.
		- Using the default calculation per interface, and changing the **interface bandwidth** setting, which changes the calculated value.
		- Using the default calculation per interface, and changing the OSPF **reference bandwidth** setting, which changes the calculated value.
	- Cost = Reference Bandwidth / Interface Bandwidth
- **Hello and Dead Intervals**
	- OSPF router uses Hello messages to announce the router’s presence on the link, sent with the 224.0.0.5 all-OSPF-routers multicast address.
	- The Dead timer tells the interface how long to wait. That is, how long since receiving a Hello message should a router wait before deciding that the neighbor failed.
	- Cisco IOS defaults to a 10 seconds Hello interval on all Ethernet interfaces.
	- The dead timer is four times Hello interval by default.
### Configuration commands
- (config-if)#ip ospf network {broadcast | point-to-point} (IOS defaults P2P on serial interfaces)
- (config-if)#ip ospf priority ***value***
- (config-if)#ip ospf authentication
- (config-if)#ip ospf authentication-key ***password***
- (config)#ip ospf {hello-interval | dead-interval} ***value***
- (config-router)#passive-interface {interface | default}
- (config-router)#default-information originate [always] (used to advertise a default route into the OSPF domain)
- (config-router)#distance ***value*** (modifies the OSPF AD on the local router)
- (config-router)#maximum-paths ***number*** (configures the max number of paths a router use to perform ECMP load-balancing)
# Chapter 24 
```
- OSPF routers cannot exchange LSAs with another router unless they first become neighbors.
- Routers may not become neighbors unless they have compatible values settings as listed in the Hellos exchanged.
- Routes are selected based on this logic: 
  - Routes from multiple routing protocols to the same destination -> use admin distance.
  - Multiple routes from the same routing protocol protocol -> use metrics.
  - Same destination matches multiple routing tables entries -> use most specific match (longest netmask prefix).
```

- **OSPF Neighbor Requirements**
	- After receiving a Hello message from routers, OSPF examines the information in the Hello and compares that with the local router's settings.
	- If the settings don't match, the routers don't become neighbors

| **Requirement**                                               | **Required for OSPF** | **Neighbor Missing If Incorrect** |
| ------------------------------------------------------------- | --------------------- | --------------------------------- |
| Interfaces must be in up/up state                             | Yes                   | Yes                               |
| ACLs must not filter routing protocol messages.               | Yes                   | Yes                               |
| Interfaces must be in the same subnet.                        | Yes                   | Yes                               |
| Neighbors must pass routing protocol neighbor authentication. | Yes                   | Yes                               |
| Hello and dead timers must match.                             | Yes                   | Yes                               |
| Router IDs (RID) must be unique.                              | Yes                   | Yes                               |
| Neighbors must be in the same area.                           | Yes                   | Yes                               |
| OSPF process must not be shut down.                           | Yes                   | Yes                               |
| OSPF must not be shut down on the interface.                  | Yes                   | Yes                               |
| Neighboring interfaces must use same MTU setting.             | Yes                   | No                                |
| Neighboring interfaces must use same OSPF network type.       | Yes                   | No                                |
| Neighboring interfaces cannot both use priority 0.            | Yes                   | No                                |

| **Requirement**                                  | **Best Show Command**                          |
| ------------------------------------------------ | ---------------------------------------------- |
| Hello and dead timers must match.                | **show ip ospf interface**<br>                 |
| Neighbors must be in the same area.              | **show ip ospf interface brief**               |
| RIDs must be unique.                             | **show ip ospf**                               |
| Neighbors must pass any neighbor authentication. | **show ip ospf interface<br>**                 |
| OSPF process must not be shut down.              | **show ip ospf**  / **show ip ospf interface** |

- **Issues That Prevent Neighbor Adjacencies**
	- *Area Mismatches.*
	- *Duplicate Router IDs.*
	- *Hello and Dead Timer Mismatches.*
	- *Shutting Down the OSPF Process.*
		- When a routing protocol process is shut down, IOS does the following:
			- Brings down all neighbor relationships and clears the OSPF neighbor table.
			- Clears the LSDB.
			- Clears the IP routing table of any OSPF-learned routes.
		- Shutting down OSPF does retain some important details about OSPF, in particular: 
			- IOS retains all OSPF configuration.
			- IOS still lists all OSPF-enabled interfaces in the OSPF interface list but in a DOWN state.
	- *Shutting Down OSPF on an Interface*
		- the router changes as follows:
			- The router stops sending Hellos on the interface, allowing existing OSPF neighbor relationships to time out.
			- The neighbor failure(s) triggers OSPF reconvergence, resulting in the removal of any routes that use the interface as an outgoing interface.
			- The router also stops advertising about the subnet on the link that is shut down.
		- The feature works much like OSPF passive interfaces, except that when shut down, OSPF also stops advertising about the connected subnet. 
- **Issues That Allow Neighbors but Prevent IP Routes**  
	- *A mismatched MTU (Maximum Transmission Unit) setting.*
	- *A mismatched OSPF network type.*
	- *Both neighbors using OSPF Priority 0.*

-  **Route Selection**
	- *Equal-Cost Multipath OSPF Routes*
		- The case where multiple routes have the same metric, routers places those routes in the routing table.
		- Forwarding logic is a destination-based load balancing.
	- *Multiple Routes Learned from Competing Sources*
		- The case where multiple routes for one subnet were learned by different routing protocols.
		- Routers choose the route learned by a routing protocol who has the lower administrative distance.
	- *IP Forwarding with the Longest Prefix Match*

### Configuration commands
- (router)#clear ip ospf process
- (config-if)#[no] ip ospf shutdown (shut down OSPF on an interface)
- (config-if)#{ip | ipv6} mtu **size**
- (config-router)#maximum-paths **number**