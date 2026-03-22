# Security Services
---
# Chapter 9
```
- Mitigation techniques are counter measures to prevent malicious activities.
- A TCP SYN flood attack is an example of a DoS attack.
- A password is considered as “something you know", a two-factor cred & digital cert as “something you have” and 
  Biometric cred as “something you are”.
- Cisco’s Identity Services Engine (ISE) product implements authentication and authorization for network-focused 
  activities. 
- ISEs use TACACS+ that separates each of the AAA functions and RADIUS that combines authentication and 
  authorization into a single resource.
- In the AAA client role, the switch is often called Network Access Device (NAD) or Network Access Server (NAS).
- User awareness, User training and Physical access control are Security Program Elements.
```

- **Common Security Threats**
	- *Spoofing Attacks:* An attack in which parameters such as IP and MAC addresses are spoofed with fake values to disguise the sender.
	- *Denial-of-Service Attacks:* An attack that tries to deplete a system resource so that systems and services crash or become unavailable.
	- *Reflection Attack:* An attack that uses spoofed source addresses so that a destination machine will reflect return traffic to the attack’s target, the destination machine is known as the reflector.
	- *Amplification Attack:* A reflection attack that leverages a service on the reflector to generate and reflect huge volumes of reply traffic to the victim.
	- *Man-In-The-Middle Attack:* An attack where an attacker manages to position a machine on the network such that it is able to intercept traffic passing between target hosts.
	- *Reconnaissance Attack:* An attack crafted to discover as much information about a target organization as possible; the attack can involve domain discovery, ping sweeps, port scans, and so on.
	- *Buffer Overflow Attack:* An attack meant to exploit a vulnerability in processing inbound traffic such that the target system’s buffers overflow; the target system can end up crashing or inadvertently running malicious code injected by the attacker.
	- *Malware:* such as trojan horse, virus, worm.
	- *Human Vulnerabilities:*
		- Social Engineering: Exploits human trust and social behavior.
		- Phishing: Disguises a malicious invitation as something legitimate.
		- Spear phishing: Targets one person or a small group.
		- Whaling: Targets high-profile individuals.
		- Vishing: Uses voice calls.
		- Smishing: Uses SMS.
		- Pharming: Redirects to compromised site.
		- Watering Hole Attack: Targets visitors of a specific site.
	- *Password Vulnerabilities:*
		- Password Guess: An attack where a malicious user simply makes repeated attempts to guess a user’s password.
		- Dictionary Attack: An attack where a malicious user runs software that attempts to guess a user’s password by trying words from a dictionary or word list.
		- Brute-Force Attack: An attack where a malicious user runs software that tries every possible combination of letters, numbers, and special characters to guess a user’s password. Attacks of this scale are usually run offline, where more computing resources and time are available.
- **AAA Mechanisms**
	- Authentication: Who is the user?
	- Authorization: What is the user allowed to do?
	- Accounting: What did the user do?
	- Comparisons Between TACACS+ and RADIUS

| Features                                                                       | TACACS+             | RADIUS     |
| ------------------------------------------------------------------------------ | ------------------- | ---------- |
| Most often used for                                                            | Network <br>devices | Users      |
| Transport protocol                                                             | TCP                 | UDP        |
| Authentication port<br>number                                                  | 49                  | 1812, 1845 |
| Protocol encrypts<br>the password                                              | Yes                 | Yes        |
| Protocol encrypts <br>entire packet                                            | Yes                 | No         |
| Supports function<br>to authorize each<br>user to a subset of <br>CLI commands | Yes                 | No         |
| Defined by                                                                     | Cisco               | RFC 2865   |

# Chapter 10
```
- The ultimate way to protect passwords in Cisco IOS devices is to not store passwords in IOS devices.
- If IOS is configured with both enable password and enable secret, the enable secret is the one to access with.
- 'enable secret' use hashing algorithms: 5:MD5, 8:SHA-256, 9:Scrypt.
- Traditional firewalls do the same kind of filtering that routers do with ACLs.
- A DMZ refers to a firewall security zone used to place servers that need to be available for use by users in 
  the public Internet.
- Next-generation firewalls Match message application data.
```
- **Traditional Firewalls**
	- *Packet Decision Logic*
		- Match the source and destination IP addresses.
		- Identify applications by matching their static well-known TCP and UDP ports.
		- Watch application-layer flows to know what additional TCP and UDP ports are used by a particular flow, and filter based on those ports.
		- Match the text in the URI of an HTTP request—that is, look at and compare the contents of the web address.
		- Keep state information by storing information about each packet, and make decisions about filtering future packets based on the historical state information (called stateful inspection).
	- *Issues*
		- Each IP-based application should use a well-known port.
		- Attackers know that firewalls filter well-known ports from sessions initiated from the outside zone to the inside zone.
		- Attackers use port scanning to find any port that a company’s firewall will allow through right now.
		- Attackers attempt to use a protocol of their choosing but with the nonstandard port found through port scanning as a way to attempt to connect to hosts inside the enterprise.
- **Traditional IPS**
	- An IPS applies the logic based on signatures (dos, ddos, worms, viruses) supplied mostly by the IPS vendor.
	- *Issues*
		- An IPS compares the signature database, which lists all known exploits, to all messages.
		- It generates events, often far more than the security staff can read.
		- The staff must mentally filter events to find the proverbial needle in the haystack, possible only through hard work, vast experience, and a willingness to dig.
- **Next-Generation Firewalls Features**
	- *Application Visibility and Control (AVC)*
	- *Advanced Malware Protection*
	- *URL Filtering*
	- *Next-Generation IPS*
- **Next-Generation IPS Features**
	- *Traditional IPS*
	- *Application Visibility and Control (AVC)*
	- *Contextual Awareness*
	- *Reputation-Based Filtering*
	- *Event Impact Level*
# Chapter 11
```
- Port security can be enabled on Layer 2 switches to filter based on source MAC addresses.
- 'Sticky secure MAC addresses' is used to learn MAC addresses off each port and it to the port security config.
- Port security requires to statically configure the port as a trunk or an access port.
- Switches can also use port security on voice ports and EtherChannels.
- 'secure' lists MAC addresses associated with ports that use port security & 'static' Lists MAC addresses 
   associated with ports that use port security, as well as any other statically defined MAC addresses.
- Only 'restrict' mode shows an accurate incrementing violation counter to indicate port security activity.
- 'aging static' keyword is used to enable aging of static secure MAC address.
```

- **Switch's Port Security**
	- It examines frames received on the interface to determine if a violation has occurred.
	- It defines a maximum number of unique source MAC addresses allowed for all frames coming in the interface.
	- It keeps a list and counter of all unique source MAC addresses on the interface.
	- It monitors newly learned MAC addresses, considering these to cause a violation if the newly learned MAC address would push the total number of MAC table entries for the interface past the configured maximum allowed addresses for that port.
	- It takes action to discard frames from the violating MAC addresses, plus other actions depending on the configured violation mode.
	- *Additional Features*
		- Define a maximum of three MAC addresses, defining all three specific MAC addresses.
		- Define a maximum of three MAC addresses but allow those addresses to be dynamically learned, allowing the first three MAC addresses learned.
		- Define a maximum of three MAC addresses, predefining one specific MAC address, and allowing two more to be dynamically learned.
- **Violation Modes**
	- Happens when a received frame breaks the port security rules on the interface.

| Option on the switchport port-security <br>violation Command                              | protect | restrict | shutdown |
| ----------------------------------------------------------------------------------------- | ------- | -------- | -------- |
| Discards offending traffic                                                                | Yes     | Yes      | Yes      |
| Sends log and SNMP messages                                                               | No      | No       | Yes      |
| Disables the interface by putting it <br>in an err-disabled state, discarding all traffic | No      | No       | Yes      |
### Configuration commands
- (config-if)#switchport port-security
- (config-if)#switchport port-security maximum **number**
- (config-if)#switchport port-security violation {protect | restrict | shutdown}
- (config-if)#switchport port-security mac-address {**mac-address** | sticky} (predefine allowed MAC add or dynamically learned)
- (config-if)#switchport port-security aging {time **minutes** | type {absolute | inactivity} | static}
- (config)#errdisable recovery cause psecure-violation (enable automatic recovery for interfaces in an err-disabled state)
- (config)#errdisable recovery interval **seconds**
- (Switch)#show port-security [interface **interface**]
- (Switch)#show mac address-table {secure | static}
- (Switch)#show errdisable recovery
# Chapter 12
```
- DHCP snooping requires enabling the feature on global config mode and listing which vlans to be enabled on, 
  along with adding trusted port since ports are untrusted by default.
- DHCP Snooping includes tracks the number of incoming DHCP messages. If the number exceeds that limit over a 
  one-second period, DHCP Snooping treats the event as an attack and moves the port to an err-disabled state.
- Gratuitous ARP is an ARP reply sent to an Ethernet destination broadcast address without having first received 
  an ARP request.
```

- **DHCP Snooping**
	- Examine all incoming DHCP messages.
	- If normally sent by servers, discard the message.
	- If normally sent by clients, filter as follows:
		1. For DISCOVER and REQUEST, check for MAC address consistency between the Ether frame and the DHCP message.
		2. For RELEASE or DECLINE, check the incoming interface plus IP address with the DHCP Snooping binding table.
	- For messages allowed by DHCP Snooping, observe the details in the messages, and if they result in a DHCP lease, build a new entry to the DHCP Snooping binding table.
	- *Trust Uplink ports and Untrust Downlink ports.*
	- *DHCP snooping rate limiting is disabled on all interfaces by default.*
- **Dynamic ARP Inspection**
	- Compares the ARP message’s sender IP and sender MAC address fields to the DHCP Snooping binding table.
		- If found in the table, DAI allows the ARP through.
		- If not, DAI discards the ARP.
	- Ports connected to local DHCP clients can remain in the default DAI untrusted state and all other switch ports as trusted.
	- Using ARP ACLs becomes useful for ports connected to devices that use static IP addresses rather than DHCP. DAI looks for both the DHCP Snooping binding data and ARP ACLs.
	- *All ports are untrusted by default, all ports connected to other network devices (switches, routers) should be configured as trusted, while interfaces connected to end hosts should remain untrusted*
	- *DAI rate limiting is enabled by default on untrusted ports with a rate of 15*
	- *Configuration*
		- Choose whether to rely on DHCP Snooping, ARP ACLs, or both.
		- If using DHCP Snooping, configure it and make the correct ports trusted for DHCP Snooping.
		- Choose the VLANs on which to enable DAI.
		- Make DAI trusted (rather than the default setting of untrusted) on select ports in those VLANs, typically for the same ports you trusted for DHCP Snooping.
### Configuration commands
- *DHCP Snooping*
	- (config)#ip dhcp snooping [vlan **vlan-id**]
	- (config)#no ip dhcp snooping information option (disables the option 82 on switches that do not act as a DHCP relay agent)
	- (config-if)#ip dhcp snooping trust
	- (config-if)#ip dhcp snooping rate limit **number**
	- (config)#errdisable recovery cause dhcp-rate-limit
	- (Switch)#show ip dhcp snooping [binding]
- *ARP Inspection*
	- (config)#ip arp inspection {vlan **vlan-id** | validate {ip | src-mac | dst-mac}}
	- (config)#
	- (config)#errdisable recovery cause arp-inspection
	- (config-if)#ip arp inspection trust
	- (config-if)#ip arp inspection limit rate {**number** [burst interval **seconds**] | none}
	- (Switch)#show ip arp inspection [statistics | interfaces]