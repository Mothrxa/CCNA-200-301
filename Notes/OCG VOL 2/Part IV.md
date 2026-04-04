# IP Services
---
# Chapter 13
```
- Logs are enabled by default with 'logging console' for console port.
- 'logging monitor' enables logs to all connected users via telnet or ssh (followed- by 'terminal monitor' to enable the logs for the current user session)
- timestamps can be changed with sequence numbers and vice versa.
- Configure the device to send logs to a remote syslog server with 'logging host' followed by 'logging trap'.
- Level 6 severity is the default.
- Stratum is the distance of an ntp server from the original reference clock.
- 'clock' command sets the software clock and 'calendar' command sets the hardware clock
- Both 'cdp run' and 'cdp enable' enables cdp, the second only for the configured interface.
```
- **Syslog**
	- Example of a log: 
	   - *Dec 18 17:10:15.079: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet 0/0 changed state to down.
		- A timestamp: Dec 18 17:10:15.079
		- The facility on the router that generated the message: %LINEPROTO
		- The severity level: 5
		- A mnemonic for the message: UPDOWN
		- The description of the message: Line protocol on Interface FastEthernet 0/0, changed state to down
	- *Logs Severity Level*
	![[Log Severity.png]]
- **Network Time Protocol (NTP)**
	- NTP gives any device a way to synchronize its time-of-day clocks.
	- Devices configured with **ntp master** command acts as a server only, while they act as client/server with **ntp server**.
	- NTP servers acting in **client/server** mode must learn the time from another device to synchronize the clock.
	- NTP servers acting in **master** mode use their internal device hardware to determine the time.
	- The lower the stratum level, the more accurate the reference clock is considered to be. 
	- NTP client adds 1 to the stratum level it learns from its NTP server, so that the stratum level increases the more hops away from the original clock source.
	- NTP allows accuracy of time within ~1 ms if the ntp server is in the same LAN or ~50 ms if it's over the internet.
- **Cisco Discovery Protocol (CDP)**
	- Used to Discover basic information about neighboring routers and switches such as : 
		- *Device identifier:* Typically the hostname.
		- *Address list:* Network and data-link addresses.
		- *Port identifier:* The interface on the remote device on the other end of the link that sent the CDP advertisement.
		- *Capabilities list:* Information on what type of device it is.
		- *Platform:* The model and OS level running on the device.
	- CDP sends messages every 60 seconds by default, with a hold time of 180 seconds.
	- CDP messages are sent to MAC address 0100:0CCC:CCCC.
- **Link Layer Discovery Protocol (LLDP)**
	- LLDP defined in 802.1AB, provides the same general features as CDP and has similar configuration and practically identical show commands.
	- LLDP sends messages every 30 seconds by default, with a hold time of 120 seconds.
	- LLDP uses B as the capability code for switching (B for bridge).
	- LLDP does not identify IGMP as a capability, while CDP does.
	- LLDP lists two lines about the neighbor’s capabilities:
		- *System Capabilities:* What the device can do.
		- *Enabled Capabilities:* What the device does now with its current configuration

### Configuration commands
- *Routers/Switches Logs*
	- (config)#logging {console | monitor} [level-name | level-number] 
	- (config-line)#logging synchronous
	- (~)#terminal monitor
	- (config)#logging buffered (stores logs to ram)
	- (config)service {timestamps [datetime | uptime] | sequence-numbers}
	- (~)#show logging
- *Syslog*
	- (config)#logging host {address | hostname} 
	- (config)#logging trap [level-name | level-number] 
- *NTP*
	- *Server Config*
		- (config)#ntp master {stratum-level}
		- (config)#ntp source **interface**
		- (config)#ntp authenticate
		- (config)#ntp authentication-key **key-number** {md5 | sha256} **key-pass**
		- (config)#ntp trusted-key **key-number**
	- *Client Config* 
		- (config)#ntp server {address | hostname} [key **key-number**]
		- (config)#ntp authenticate
		- (config)#ntp authentication-key **key-number** {md5 | sha256} **key-pass**
		- (config)#ntp trusted-key **key-number**
	- (config)#clock timezone **timezone-label -hour**
	- (config)#clock summer-time **DST-label** recurring
	- (~)#clock set **hours:minutes:seconds dd mm yyyy**
	- (~)#calendar set **hours:minutes:seconds dd mm yyyy**
	- (config)#clock update-calendar (sync the hard-clock to the soft-clock)
	- (config)#clock read-calendar (sync the soft-clock with the hard-clock)
	- *Show Config*
	- (~)show ntp {status | associations}
	- (~)#show clock
	- (~)#show calendar
- *CDP*	
	- (config-if)#[no] cdp enable
	- (config)#[no] cdp run
	- (config)#cdp timer **seconds**
	- (config)#cdp holdtime **seconds**
	- (~)#show cdp [neighbors [**interface** | detail | traffic] | **interface** | traffic | entry **hostname**]
- *LLDP*
	- (config)#[no] lldp run
	- (config-if)#[no] lldp {transmit | receive}
	- (config)#lldp timer **seconds**
	- (config)#lldp holdtime **seconds**
	- (config)#lldp reinit **seconds**
	- (~)#show lldp [neighbors [**interface** | detail | traffic] | **interface** | entry **hostname**]

# Chapter 14
```
- Inside source NAT has three variants: static NAT, dynamic NAT and NAT overload (PAT).
- Cisco commands use the terms inside_local for the private addresses and inside_global for the public addresses.
- Static NAT builds NAT table staticaly, while Dynamic NAT uses matching logic to dynamically build the table.
- For Dynamic NAT and NAT/PAT configure an ACL that matches, with a permit action, the packets entering inside 
  interfaces for which the router should apply NAT.
- NAT in enabled by 'ip nat inside source' command. 
```
- **Private IPs Pool**
	- 10.0.0.0 - 10.255.255.255 (/8)
	- 172.16.0.0 - 172.31.255.255 (/12)
	- 192.168.0.0 - 192.168.255.255 (/16)
- **NAT Troubleshooting**
	- *Reversed inside and outside:* Ensure `ip nat inside` is on LAN interfaces and `ip nat outside` on WAN interfaces; translations are triggered by packets entering the inside interface.
	- *Static NAT:* In `ip nat inside source static`, the **inside local** IP must come first and the **inside global** IP second.
	- *Dynamic NAT (ACL):* The ACL must match the **inside local addresses before translation**
	- *Dynamic NAT (pool):* The pool must have enough public IPs because **each inside host uses one address**; otherwise translations fail.
	- *PAT:* Don’t forget the `overload` keyword; without it, the router performs Dynamic NAT instead of allowing many hosts to share one public IP.
	- *ACL:* An interface ACL might block packets even if NAT is correct; inbound ACLs are checked **before NAT**, outbound ACLs **after NAT**.
	- *User traffic required:* NAT translations appear only when **actual traffic from inside hosts** enters the router and triggers translation.
	- *IPv4 routing:* Routing must correctly deliver packets to and from the NAT router; Internet routers must know routes to the **public NAT addresses**.
### Configuration commands
- (config-if)#ip nat {inside | outside}
- *Static NAT*
	- (config)#ip nat inside source static **inside-local inside-global**
- *Dynamic NAT*
	- (config)#ip nat pool **name** **first-address** **last-address** {netmask **subnet-mask** | prefix-length **length**}
	- (config)#ip nat inside source list **acl-number** pool **pool-name**
- *NAT/PAT*
	- (config)#ip nat inside source list **acl-number** interface **interface** overload
-  *Show NAT Config* 
	- (~)#show ip nat {translations | statistics}
- (~)#clear ip nat translation *

# Chapter 15 
```
- Types of Traffic: Data Applications & Voice and Video Applications.
- QoS Tools: Classification and Marking, Queuing, Shaping and Policing, Congestion avoidance (QoS defines these 
  actions as per-hop behaviors (PHBs)).
- NBAR is used when an ACL isn't enough to match a packet.
- Routers use Class-Based Weighted Fair Queuing (CBWFQ) to guarantee a minimum amount of bandwidth to each class 
  in round-robin scheduling.
- Adding Low Latency Queuing (LLQ) to the scheduler provides low delay, jitter and loss which uses priority.
- Committed Information Rate (CIR) is the guaranteed minimum bandwidth an ISP promises the customer.
```

- **Network Traffic Characteristics**
	- *Bandwidth:* Capacity of the link.
	- *Delay:*  Can be described as :
		- One-Way Delay : The time between sending one packet and that same packet arriving at the destination host.
		- Round-Trip Delay : Counts the one-way delay plus the time for the receiver of the first packet to send back a packet.
	- *Jitter:* Difference Between two packets One-Way Delay.
	- *Loss:* Refers to the number of lost messages.
- **Classification & Marking**
	- Refers to the process of matching the fields in a message to make a choice to take some QoS action.
	- For any packet matched by the ACL with a permit action, consider that packet a match for QoS, so do a particular QoS action.
	- *Marking:* The process of changing one of a small set of fields in various network protocol headers, including the IP header’s DSCP field, for the purpose of later classifying a message based on that marked value.
- **Queuing**
	- Use a round-robin queuing method like CBWFQ for data classes and for non-interactive voice and video.
	- If faced with too little bandwidth compared to the typical amount of traffic, give data classes that support business-critical applications much more guaranteed bandwidth than is given to less important data classes.
	- Use a priority queue with LLQ scheduling for interactive voice and video, to achieve low delay, jitter, and loss.
	- Put voice in a separate queue from video so that the policing function applies separately to each.
	- Define enough bandwidth for each priority queue so that the built-in policer should not discard any messages from the priority queues.
	- Use call admission control (CAC) tools to avoid adding too much voice or video to the network, which would trigger the policer function.
- **Policing**
	- Policing measures the traffic rate over time for comparison to the configured policing rate.
	- Policing allows for a burst of data after a period of inactivity.
	- Policing is enabled on an interface, in either direction, but typically at ingress.
	- Policing can discard excess messages but can also re-mark the message so that it is a candidate for more aggressive discard later in its journey.
- **Shaping**
	- Shapers measure the traffic rate over time for comparison to the configured shaping rate.
	- Shapers allow for bursting after a period of inactivity.
	- Shapers are enabled on an interface for egress (outgoing packets).
	- Shapers slow down packets by queuing them and over time releasing them from the queue at the shaping rate.
	- Shapers use queuing tools to create and schedule the shaping queues, which is very important for the same reasons discussed for output queuing.
- **Congestion Avoidance**
	- Attempts to reduce overall packet loss by preemptively discarding some packets used in TCP connections through using TCP windowing mechanisms.
### Configuration commands
- (config)#class-map **class-name**
	- (config-cmap)#match protocol {amazon-ec2 | amazon-instant-video | amazon-s3 | amazon-web-services}

# Chapter 16
```
- FHRP lists three variants: Cisco HSRP, IETF VRRP and Cisco GLBP.
- HSRP uses VIP and VMAC to identify the active router.
- When a router's WAN link fails, the router lowers its priority (default 100) to let the standby router become 
  active.
- When an active router recovers from failure it becomes a standby router (no preemption), With preemption if a 
  new router appears with a higher priority, it takes over as the active router (doesn't apply in a tie).
- VRRP supports all the same functions as HSRP.
```

- **First Hop Redundancy Protocol**
	- All hosts act like they always have, with one default router setting that never has to change.
	- The default routers share a virtual IP address in the subnet, defined by the FHRP.
	- Hosts use the FHRP virtual IP address as their default router address.
	- The routers exchange FHRP protocol messages so that both agree as to which router does what work at any point in time.
	- When a router fails or has some other problem, the routers use the FHRP to choose which router takes over responsibilities from the failed router.
- **Hot Standby Router Protocol**
	- Operates with an active/standby model (active/passive), the router with the highest priority becomes active, if the priorities tie, the router with the highest IP address active.
	- HSRP uses **HSRP Hello** messages to let the other HSRP routers in the same HSRP group know that the active router continues to work.
	- HSRP defines a **Hold timer** that's three times the *HSRP Hello* message. When the standby router stops receiving HSRP Hello within the time defined by the hold time, It believes the active router has failed and becomes the active router.
	- *HSRPv1 Vs HSRPv2*

| Feature                                | Version 1      | Version 2      |
| -------------------------------------- | -------------- | -------------- |
| IPv6 support                           | No             | Yes            |
| Smallest unit for Hello timer          | Second         | Millisecond    |
| Range of group                         | [0-255]        | [0-4095]       |
| Virtual MAC used (x: hex group number) | 0000.0C07.ACxx | 0000.0C9F.Fxxx |
| IPv4 multicast address used            | 224.0.0.2      | 224.0.0.102    |

- **Virtual Router Redundancy Protocol**
	- VRRP uses 224.0.0.18 as the multicast address, VMAC pattern of 0000.5e00.01xx and a group range of 1–255.
	- VRRP uses the terms master and backup rather than active and standby.
	- Like HSRP, VRRP uses interface tracking for preemption to change the router's priority.
	- VRRP can configure the VIP with the same address as the interface's unlike HSRP.
- **Gateway Load Balancing Protocol**
	- GLBP uses 224.0.0.102 s the multicast address, VMAC pattern of 0007.b40x.xxyy (x: group number, y: avf number) and a group range of 0–1023
	- GLBP uses Active/Active Load Balancing, to achieve this, one GLBP performs the role of GLBP active virtual gateway (AVG) which handles all ARP functions for the VIP. All routers serve as a GLBP active virtual forwarder (AVF) to support load balancing (AVG: Primary active, AVF: Secondary actives).
# Chapter 17
```
- Management Information Base: MIB.
- Network Management System: NMS.
- SNMPv1 used Get & GetNext, SNMPv2 improved with GetBulk that requests multiple variables and removed community 
  strings, SNMPv2c added back the community strings, SNMPv3 improved security.
```

- **Simple Network Management Protocol**
	- The SNMP manager (NMS) uses SNMP protocols to communicate with each SNMP agent (Routers, Switches, hosts).
	- Each agent keeps a database of variables (MIB) that comprise the parameters, status, and counters for the device’s operations (supports Cisco MIB & MIB-II).
	- Agents send notification messages to the NMS using **trap** and **inform** messages.
	- *SNMP Traps*: use a *fire-and-forget* mechanism: the agent sends a UDP message to the NMS with *no acknowledgement* or *re-transmission* if it is lost.
	- *SNMP Informs*: provide *reliable notification*: the NMS must send an *SNMP Response acknowledgment*, otherwise the agent *re-transmits the Inform* after a timeout.
	- SNMPv3 replaced SNMP community RO-RW messages with *message integrity,* *Authentication,* *Encryption*
- **Trivial File Transfer Protocol**
	- Copies files and ios images from tftp server to local device, to do so using 'copy tftp: flash:', the device asks for: 
		1. What is the IP address or host name of the TFTP server?
		2. What is the name of the file?
		3. Ask the server to learn the size of the file, and then check the local router’s flash to ask whether enough space is available for this file in flash memory.
		4. Does the server actually have a file by that name?
		5. Do you want the router to erase any old files in flash?
	- TFTP supports far fewer commands than FTP (lightweight)
- **File Transfer Protocol**
	- *FTP Active mode*
		1. The FTP client allocates a currently unused dynamic port and starts listening for new connections on that port.
		2. The client identifies that port (and its IP address) to the FTP server by sending an FTP PORT command to the server over the control connection.
		3. The server, because it also operates in active mode, expects the PORT command; the server reacts and initiates the FTP data connection to the client’s address (192.168.1.102) and port (49333).
		-  ![[FTP active.png]]
	- *FTP Passive mode*
		1. Via messages sent over the control connection, the FTP client changes to use FTP passive mode, notifying the server using the FTP PASV command.
		2. The server chooses a port to listen on for the upcoming new TCP connection, in this case TCP port 49444.
		3. Again using the control connection, the FTP server notifies the FTP client of its IP address and chosen port with the FTP PORT command.
		4. The FTP client opens the TCP data connection to the IP address and port learned at the previous step.
		-  ![[FTP passive.png]]
### Configuration commands
- *SNMP*
	- (config)#snmp-server {contact **e-mail** | location **address** | community **community-name** {ro | rw}}
	- (config)#snmp-server host **nms-ip** version **version [community-name]** 
	- (config)#snmp-server enable traps **trap-types**
- *FTP/TFTP*
	- (~)#more flash0:/directory/file (displays the content of file in directory)
	- (~)#{show | dir} flash: (lists the files in the directory (cd and pwd can be used))
	- (~)#copy {tftp: | ftp:} flash:
	- (config)#ip ftp {username **username** | password **password**}
