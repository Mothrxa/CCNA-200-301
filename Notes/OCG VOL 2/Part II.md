# IP Access Control Lists
---
# Chapter 5
```
- TCP provides retransmission (error recovery) and helps to avoid congestion (flow control).
- Forward acknowledgment is convention of acknowledging by listing the next expected byte.
- The receiver use sliding (dynamic) window to control the data flow that comes from the sender.
- Local DNS server becomes a recursive DNS server when it doesn't know the address associated with a hostname.
- HTTP/3 uses QUIC which uses UDP:443.
```

- **Transport Layer Features**

| **Function**                                    | **Protocol** | **Description**                                                                                                                                                                               |
| ----------------------------------------------- | ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Multiplexing using ports<br>(relies on sockets) | TCP/UDP      | Function that allows receiving hosts to choose the correct application for which the data is destined, based on the port number.                                                              |
| Error Recovery (reliability)                    | TCP          | Process of numbering and acknowledging data with Sequence & Acknowledgment header fields.                                                                                                     |
| Flow Control using windowing                    | TCP          | Process that uses window sizes to protect buffer space and routing devices from being overloaded with traffic.                                                                                |
| Connection establishment-termination            | TCP          | Process used to initialize port numbers and Sequence and Acknowledgment fields.                                                                                                               |
| Ordered data transfer & data segmentation       | TCP          | Continuous stream of bytes from an upper-layer process that is “segmented” for transmission and delivered to upper-layer processes at the receiving device, with the bytes in the same order. |

- **TCP/UDP Ports**

| **Application** | **Protocol** | **Port** |
| --------------- | ------------ | -------- |
| FTP data        | TCP          | 20       |
| FTP control     | TCP          | 21       |
| SSH             | TCP          | 22       |
| Telnet          | TCP          | 23       |
| SMTP            | TCP          | 25       |
| TACACS+         | TCP/UDP      | 49       |
| HTTP/S          | TCP          | 80/443   |
| POP3            | TCP          | 110      |
| DNS             | TCP/UDP      | 53       |
| DHCP server     | UDP          | 67       |
| DHCP client     | UDP          | 68       |
| TFTP            | UDP          | 69       |
| SNMP agent      | UDP          | 161      |
| SNMP manager    | UDP          | 162      |
| NTP             | UDP          | 123      |
| Syslog          | UDP          | 514      |
# Chapter 6
```
- IP ACLs can filter packets on routers, focusing on the Layer 3 and 4 headers.
- Standard Numbered ACLs match only the source IP.
- Standard Numbered ACLs range: 1-99 & 1300-1999.
- IOS refers to each line in an ACL as an access control entry (ACE) or ACL statements.
- The ACL list is searched sequentially using first-match logic.
- The default action (implicit deny), if a packet does not match any of the access-list commands is to deny 
  (discard) the packet.
- Standard ACLs should be applied as close to the destination as possible.
```

- **Finding the Right Wildcard Mask to Match a Subnet**
	- To match a subnet with an ACL, you can use the following shortcut:
		- Use the subnet number as the source value in the access-list command.
		- Use a wildcard mask found by subtracting the subnet mask from 255.255.255.255.
- **ACLs Location and Direction**
	1. Standard ACLs should be placed near the packet’s destination so that they do not unintentionally discard packets that should not be discarded.
	2. Because standard ACLs can only match a packet’s source IP address, identify the source IP addresses of packets as they go in the direction that the ACL is examining.
- **ACLs Matching Logic**
	- To match a specific address, just list the address.
	- To match any and all addresses, use the **any** keyword.
	- To match based certain octets of an address, use the wildcard masks.
	- To match a subnet, use the subnet ID as the source, and find the WC mask by subtracting the DDN subnet mask from 255.255.255.255.
### Configuration commands
- (config)#access-list **number** {permit | deny} {**source [wildcard-mask]** | host **host-address** | any} [log]
- (config)#access-list **number** remark **text**
- (config-if)#ip access-group {**acl-number** | **acl-name**} {in | out}
- (config)#clear ip access-list counters
- (router)#show ip access-lists
- (router)#show access-lists
# Chapter 7
```
- Standard Named ACL must begin with an alphabetic char, cannot use spaces and quotation marks, case sensitive.
- To delete an named ACE, either repeat the sub-command preceded with 'no' keyword or using 'no acl-id' inside 
  ACL mode.
- Issuing the 'no + acl' command on Numbered ACLs deletes the entire acl not just the ACE.
- Enter ACL mode to delete numbered ACEs.
- Extended ACLs range: 100-99 & 2000-2699.
- Extended ACLs match the source & destination address, the portocol:port.
- IP protocol number -> ICMP:1, TCP:6, UDP:17, EIGRP:88, OSPF:89 (used in protocol field of Ext ACL command).
- 'eq', 'neq', 'gt', 'lt' keywords are used after src/dst ip for conditional port selection.
- 'range' keyword is used after src/dst ip to specify a range of ports.
- Extended ACLs should be applied as close to the source as possible to limit packet far travelling.
```

### Configuration commands
- (config)#ip access-list resequence **acl-id** **starting-seq** **increment** 
- (config)#ip access-list {standard | extended} {**acl-name** | **acl-id**}
- (config-std-nacl)#[entry-number] {permit | deny} **source wildcard-mask**
- (config-ext-nacl)#[entry-number] {permit | deny} **protocol** {**src-ip** | any | host} {**dst-ip** | any | host}
- (config)#access-list **number** {permit | deny} **protocol** {**src-ip** | any | host} {**dst-ip** | any | host}

# Chapter 8
```
- By default, packets changed by a router due to the ip helper-address command bypass any outgoing ACL.
- On a router interface with both an inbound ACL and the helper function, the router performs the ACL function 
  first, before the helper function changes the IP addresses in DHCP messages.
- 'range' keyword is used after src/dst ip to filter based on a range of ports.
- 'access-class' keyword is used on vty/console line.
- IOS XE uses ACL persistence to prevent resequencing at restart, by default configured with 'ip access-list 
  persistent' global config mode. 
- 'common' keyword is used to apply two ACLs on an interface.
- 'eq port-number' can match multiple nonconsecutive ports.
```

### Configuration commands
- (config-line)#access-class {**acl-number** | **acl-name**} {in | out}
- (config-if)#ip access-group common {**acl1-number** | **acl1-name**} {**acl2-number** | **acl2-name**} {in | out}
- (config)#ip access-list persistent