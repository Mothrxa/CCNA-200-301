# IPv6
---

# Chapter 25
```
- The prefix length defines how many bits on the left side of anIPv6 address are the same value for all addresses 
  within that subnet prefix.
- Commands on Cisco devices typically do not allow spaces before or after the /.
```

- **Common Mistakes IPv6 Abbreviation**
	- Removing trailing 0s in a quartet (0s on the right side of the quartet, 2100 doesn't become 21).
	- Replacing both sets of 0000 quartets with a double colon (more than one ::, **2100:0:0:1:0:0:0:56** doesn't become **2100::1::56** but **2100:0:0:1::56**).
- **Expanding Abbreviated IPv6 Addresses**
	- In each quartet, add leading 0s as needed (on the left) until the quartet has four hex digits.
	- If a double colon (::) exists, count the quartets currently shown; the total should be less than 8. Replace the :: with multiple quartets of 0000 so that eight total quartets exist.
- **Finding the IPv6 Subnet Prefix** 
	- *If the prefix length is /P* 
		1. Copy the first P bits.
		2. Change the rest of the bits to 0.
	- *A prefix length that is a multiple of 4 means that each hex digit is either copied or changed to hex 0*
		1. Identify the number of hex digits in the subnet prefix by dividing the prefix length by 4.
		2. Copy the hex digits determined to be in the subnet prefix per the first step.
		3. Change the rest of the hex digits to 0.
# Chapter 26 
```
- Global unicast addresses serve as public addresses & unique local addresses serve as private addreses.
- Global routing prefix is a set of addresses only one company can use (similar to IPv4 Classes).
- The interface ID’s length is 64 bit & the subnet ID field is typically 64–P bits, with P being the length of 
  the global routing prefix.
- GUA starts with 2000/3 (any number that starts with 2 or 3).
```

- **Unique Local Addresses**
	- the addresses need to follow these rules :
		- Use FD as the first two hex digits (8 bits).
		- Choose a unique 40 bits global ID.
		- Append the global ID to FD to create a 48 bits prefix, used as the prefix for all addresses.
		- Use the next 16 bits as a subnet field.
		- Note that the structure leaves a convenient 64-bit interface ID field.
 ![[Unique Local Address.png]]


# Chapter 27 
```
- Routers still accept the IPv6 address command and acts as an IPv6 host but doesn't route.
- Routers can be configured to use eui-64 by splitting the mac address in half and add FFFE between the two 
  making the interface ID, then invert the 7th bit of the 1st 2 bytes of the IID (0 becomes 1 and vice-versa).
- The router uses the MAC of the lowest-numbered router interface that has a MAC for interfaces that don't have.
- IOS convert addresses to the matching prefix if an address was used with eui-64 command.
- Cisco routers support 2 ways for router's interface to dynamically learn an IPv6 address to use (DHCP - SLAAC).
- IPv6 uses LLA as a special type of unicast IPv6 address, Every interface that supports IPv6 must have an LLA.
- LLAs exist to support some overhead IPv6 protocols—protocols that need some working address to use and not 
  normal application data.
- IPv6 routing works on WAN links that use only LLAs with no GUAs or ULAs.
- IPv6 addresses that begin with FF3 are used for multicast applications (OSPFv3 uses FF02::5 & FF02::6 as the 
  all ospf routers addresses multicast).
- Packets sent to a link-local unicast address should remain on the link and not be forwarded by any router.
- The unspecified IPv6 address is ::, that is all 0s & The loopback IPv6 address is ::1, that is 127 binary 0s 
  with a single 1.
- The unspecified address is only used before the host has an LLA or other unicast address to use.
  
```

- **IPv6 Address Attributes**
	- *EUI:* The router generated the address using modified EUI-64.
	- *TEN:* Tentative. A temporary attribute that occurs while the router performs duplicate address detection (DAD).
	- *CAL:* Calendared. An address with a prescribed lifetime, including a valid and preferred lifetime. They are commonly listed when using DHCP and SLAAC.
	- *PRE:* Preferred. The preferred address to use on the interface that uses time-based (calendared) addresses. They are commonly listed when using DHCP and SLAAC.
- **Link-Local Address**
	- Provides devices that support IPv6 the capability to send and receive unicast packets on the local link before getting their IPv6 GUA/ULA.
	- Hosts generate unique LLAs with duplicate detection (DAD)
		- If unique the host keeps using the LLA.
		- If DAD finds another host using the same LLA, the new host picks a different  LLA.
	- LLAs are Unicast, its forwarding scope is the local link only, they are automatically self-generated and used as the next-hop address for IPv6 routes.
	- Routers self-assign an LLA to each enabled IPv6 interface, composed with FE80:0000:0000:0000 as the prefix (first half) and EUI-64 as the Interface ID. 
- **Most Common Well-Known IPv6 Multicast Addresses**

| **Short Name**              | **Multicast Address** | **Meaning**                                                       | **IPv4 Equivalent**    |
| --------------------------- | --------------------- | ----------------------------------------------------------------- | ---------------------- |
| *All-nodes*                 | FF02::1               | All-nodes (all interfaces that use IPv6 that are on the link)     | 224.0.0.1              |
| *All-routers*               | FF02::2               | All-routers (all IPv6 router interfaces on the link)              | 224.0.0.2              |
| *All-OSPF*<br>*All-OSPF-DR* | FF02::5<br>FF02::6    | All OSPF routers and all OSPF-designated routers,<br>respectively | 224.0.0.5<br>224.0.0.6 |
| *RIPng routers*             | FF02::9               | All RIPng routers                                                 | 224.0.0.9              |
| *EIGRPv6 routers*           | FF02::A               | All routers using EIGRP for IPv6 (EIGRPv6)                        | 224.0.0.10             |
| *DHCP relay Agent*          | FF02::1:2             | All routers acting as a DHCPv6 relay agent (DHCP servers as well) | None                   |
- **Multicast Address Scopes**

| **Scope Name **      | **Starts_With** | **Scope Defined by**     | **Meaning**                                                                                                             |
| -------------------- | --------------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| *Interface-local*    | FF01            | Derived by Device        | The packet remains within the device. Useful for internally sending packets to services running on that same host.      |
| *Link-local*         | FF02            | Derived by Device        | The host that creates the packet can send it onto the link, but no routers forward it.                                  |
| *Site-Local*         | FF05            | Configuration on Routers | The packet can be forwarded by routers, should be limited to a single physical location (not forwarded over WAN links). |
| *Organization-Local* | FF08            | Configuration on Routers | Intended to be broad, probably for an entire company or organization.<br>Must be broader than Site-Local.               |
| *Global*             | FF0E            | No Boundaries            | No Boundaries, possible to be routed over the internet.                                                                 |

- **Solicited-Node Multicast Addresses**
	- NDP improves the MAC-discovery process compared to IPv4 ARP by sending IPv6 multicast packets to communicate with a second host before the first host knows the second host’s MAC address.
	- The process uses the solicited-node multicast address associated with the unicast IPv6 address.
	- Starts with the abbreviated **FF02::1:FF** address (24 hex digits). In the last 6 hex digits, the last 6 hex digits of the unicast address are copied into the solicited-node address.
	- Solicited-node multicast address format : 
		 ![[Solicited-Node Multicast Address Format.png]]
	- Hosts/Routers calculate and use a matching solicited-node multicast address for every unicast address (GUA, ULA, LLA) on an interface.The device then registers to receive incoming multicast messages sent to that address.
- **IPv6 Anycast Addresses**
	- Used when a service is implemented on multiple routers. Hosts only need to contact one IPv6 address for the service.
	- The network automatically delivers traffic to the nearest router providing that service. “Anycast” means any one of the service instances can respond (one-to-one-of-many).
	- Multiple routers are configured with the same IPv6 address, this shared address is designated as an anycast address.
	- When a packet is sent to that address routers forward it to the closest instance based on routing metrics.
	- Comes from the unicast address range and configured with a /128 prefix (host route).
### Configuration commands
- (config-if)#ipv6 address **address/prefix-length** [anycast]
- (config-if)#ipv6 address **prefix/prefix-length** eui-64
- (config-if)#ipv6 address {dhcp | autoconfig} (either use dhcp or slaac for dynamic addressing)
- (config-if)#ipv6 address **address** link-local (creates an LLA without eui-64)
- (config-if)#ipv6 enable (enables ipv6 processing & self-assign LLA address but with no routable address)
- (config)#ipv6 unicast-routing (enable ipv6 packets routing)
- (router)#show ipv6 interface [brief | **interface-name**]
- (router)#show ipv6 {route **[type | address]** | routers | neighbor}
# Chapter 28 
```
- IPv6 supports a dynamic address assignment process called Stateless Address Autoconfiguration (SLAAC).
- DHCPv6 uses the Solicit, Advertise, Request, and Reply (SARR) messages.
- IPv6 now supports RA-based DNS to deliver the DNS server list rather than relying on stateless dhcp servers.
- The DNS server list needs to be configured on all routers so that routers supplies the DNS list to hosts via 
  the NDP RA message.
- RDNSS configuration provides a means for automatic assignment of all client IPv6 settings using router 
  configuration only, with no DHCP server at all.
```

- **Neighbor Discovery Protocol**
	- The NDP is a part of ICMPv6 defined in RFC 4861 and is the IPv6 replacement for ARP.
	- Some of its functions are : 
		- *Neighbor MAC Discovery*
			- Neighbor Solicitation (NS) similar to ARP Request (uses the solicited-node multicast address)
			- Neighbor Advertisement (NA) similar to ARP Reply (A host can also send an unsolicited NA, announcing its IPv6 and MAC addresses using the all-host local-scope multicast address (FF02::1)).
		- *Router Discovery*
			- Router Solicitation (RS) sent by hosts to ask all on-link routers to identify themselves with the all-routers local-scope multicast address (FF02::2).
			- Router Advertisement (RA) sent by routers in response to RS, includes router's LLA with other listings to the host who sent the RS. Routers also send unsolicited RA messages periodically, announcing the same details to all hosts on the link using the all-host local-scope multicast address (FF02::1).
		- *Prefix Discovery*
			- Prefix/Prefix-length are learned from a Router Advertisement message.
			- When a host receives an RA, it automatically builds two types of routes in its routing table.
				1. A route for each on-link prefix/length learned, reachable without using a router.
				2. A route for the default prefix (::/0) learned, using the router LLA as next hop, used to reach off-link destinations.
		- *Duplicate Address Detection*
			- A host sends an NDP NS message for its own IPv6 address.
			- The host expects no other host is already using that address, if it receives an NDP NA in reply the host uses another IPv6 address.
- **Stateless Address Autoconfiguration**
	- Learns the IPv6 prefix used on the link from any router, using NDP RS/RA messages.
	- Chooses its IPv6 address by making up the IID value to follow the earned IPv6 prefix (randomly or with eui-64).
	- Before using the address, it uses DAD to ensure that no other host is already using the same address.
- **Stateless DHCPv6** 
	- Needs simple configuration only, specifically the short list of DNS server addresses.
	- Needs no per-subnet configuration: no subnet list, no per- subnet address pools, no list of excluded addresses per subnet, and no per-subnet prefix lengths.
	- Has no need to track state information about DHCP leases because the server does not lease addresses to any clients.
### Configuration commands
- Show routers learned by a host & host’s NDP neighbor table
	- (windows)$netsh interface ipv6 show neighbors
	- (linux)$ip -6 neighbor [show]
	- (macOs)$ndp -{an | rn} (an to view a host’s NDP neighbor table & rn to view the routers learned by a host)
- (config)#ipv6 dhcp relay destination **dhcp-server-address**
# Chapter 29 
```
- Routers do not create connected or local IPv6 routes for link-local addresses.
- Only serial interfaces support using an ipv6 route with only an exit interface as the forwarding instructions.
- IPv6 supports using the neighbor’s GUA, ULA, or LLA as the next-hop IPv6 address.
```

- **Static route types**
	- *Directly attached:* only the exit interface is specified (can't be used on IPv6 Ethernet interfaces)
	- *Recursive:* only the next-hop is specified, (GUA or ULA only)
	- *Fully specified:* both the exit interface and the next-hop are specified, the working combinations are :
		- Next-hop GUA or ULA and outgoing interface
		- Next-hop LLA and outgoing interface
### Configuration commands
- (config)#ipv6 route **destination/prefix-length** {**next-hop** | **exit-interface** **[next-hop]**} **[ad]**

