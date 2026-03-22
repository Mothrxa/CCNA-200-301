# Wireless LANs
---
# Chapter 1
```
- 802.11 WLANs are always half duplex because transmissions between stations use the same frequency or channel 
  (only one station can transmit at a time).
- Full duplex mode is acheivable by using different frequencies for transmitting and reveiving. Although this 
  operation is certainly possible and practical, the 802.11 standard doesn't permit full-duplex operation.
- Distribution System is a wired ethernet that connects to an AP and transports traffic between wired and 
  wireless network.
- In an independent basic service set (IBSS/adhoc wireless network), one of the devices take the lead and begin 
  advertising a network name, any other device can then join as needed (peer-to-peer connection).
- IBSSs are meant to be organized in an impromptu, distributed fashion: therefore, they do not scale well beyond 
  eight to ten devices.
- CAPWAP (Control and Provisioning of Wireless Access Points).
```

- **Basic Service Set**
	- Basic Service Set (BSS) is a wireless service provided by an Access Point to one or many associated clients.
	- The AP and the members of the BSS must all use the same channel to communicate properly and all are bounded by the area where the AP's signal is usable (known as BSA/cell).
	- APs sends broadcast frames (beacons) to advertise the existence of the BSS, beacon frame contains the SSID  which identifies a BSS.
	- APs use BSSID based on its radio MAC address which identifies the wireless network (BSSID is machine-readable name tag that identifies the AP & SSID is human-readable name tag that identifies the wireless service).
	- Wireless devices use passive scanning to learn about BSSs within range by listening to the received beacons, they can also discover SSIDs by transmitting probe request and receive probe response from the AP.
	- To join the membership with the BSS, devices send association request and AP either grant or deny the request by sending association response, once associated a device becomes an 802.11 station to the BSS.
-  **Extended Service Set**
	- ESSID is a consistent SSID through the ESS.
	- Devices send re-association request frame to the new nearby AP when moving between different locations and receive re-association response in return, passing from an AP to another is called ***roaming***. 
- **Other Wireless Topologies**
	- *Repeater*
		- With one single transmitter and receiver, the repeater has to operate on the same channel as the AP (not effective).
		- With two transmitters and receivers, One pair is dedicated to signals in the AP’s cell, while the other pair is dedicated to signals in the repeater’s own cell.
	- *Workgroup bridge*
		- An AP that is configured to bridge between a wired device and a wireless network. The WGB acts as a wireless client.
		- Universal workgroup bridge (uWGB): A single wired device can be bridged to a wireless network.
		- Workgroup bridge (WGB): A Cisco-proprietary implementation that allows multiple wired devices to be bridged to a wireless network.
	- *Outdoor bridge*
	- *Mesh Network*
- **Wireless Bands and Channels**
	- 2.4-Ghz band range : [2.4000 - 2.4835], 5-Ghz band range : [5.150 - 5.825], 6-Ghz band range : [5.925 - 7.125].
	- The 5-Ghz band consists of non-overlapping channels.

| **WI-FI Alliance** | **IEEE Amendment** | **Bands Supported** | **Max data rate** | **Notes**                                      |
| ------------------ | ------------------ | ------------------- | ----------------- | ---------------------------------------------- |
| WI-FI 0            | 802.11             | 2.4Ghz              | 2 Mbps            | The original 802.11 standard ratified in 1997  |
| WI-FI 1            | 802.11a            | 5Ghz                | 54 Mbps           | Introduced in 1999                             |
| WI-FI 2            | 802.11b            | 2.4Ghz              | 11 Mbps           | Introduced in 1999                             |
| WI-FI 3            | 802.11g            | 2.4Ghz, 5Ghz        | 54 Mbps           | Introduced in 2003                             |
| WI-FI 4            | 802.11n            | 2.4Ghz, 5Ghz        | 600 Mbps          | HT (high throughput), Introduced in 2009       |
| WI-FI 5            | 802.11ac           | 2.4Ghz, 5Ghz        | 6.9 Gbps          | VHT (very high throughput), Introduced in 2013 |
| WI-FI 6 - 6E       | 802.11ax           | 2.4Ghz, 5Ghz, 6Ghz  | 9.6 Gbps          | High Efficiency Wireless                       |
| WI-FI 7            | 802.11be           | 2.4Ghz, 5Ghz, 6Ghz  | 46 Gbps           | EHT (Extremely High Throughput)                |

# Chapter 2
```
- A control plane is a Traffic used to control, configure, manage, and monitor the AP itself & a data plane is an 
  End-user traffic passing through the AP.
- APs operate on FlexConnect mode in remote sites to let the wireless traffic flows to and from the controller.
- An autonomous AP is a standalone device.
```

- **Split-MAC Architecture**
	- Autonomous APs function is split into two groups, that is management functions & real time processes.
	- Cisco APs handle the real-time processes such as sending and receiving 802.11 frames (beacons, probe messages) and encrypting 802.11 data, they interact with wireless clients on the *MAC Layer*.
	- Wireless LAN Controllers handle the management functions (control Cisco APs) such as authenticating users, managing security policies, and even selecting RF channels and output power.
	- APs will become nonfunctional if the connectivity to the WLC is lost.
	- AP-WLC use the Control and Provisioning of Wireless Access Points (CAPWAP) tunneling protocol to carry 802.11-related messages and client data (they don't have to be in the same subnet/vlan).  
	- The *CAPWAP* relationship consists of two separate tunnels :
		- Control messages : carry exchanges that are used to configure the AP and manage its operation. They are authenticated and encrypted, so the AP is securely controlled by only the appropriate WLC, then transported over the control tunnel.
		- Data : Used for packets traveling to and from wireless clients that are associated with the AP. Data packets are transported over the data tunnel but are not encrypted by default. When data encryption is enabled for an AP, packets are protected with Datagram Transport Layer Security (DTLS).
	- WLC functions :
		- Dynamic channel assignment.
		- Transmit power optimization.
		- Self-healing wireless coverage.
		- Flexible client roaming.
		- Dynamic client load balancing.
		- RF monitoring.
		- Security management.
		- Wireless intrusion prevention.
- **Wireless LAN Controller types**
	- *Centralized WLC deployment* (unified WLC deployment)
		- Placed into the center of the network (core layer).
		- Supports Up to 6000 APs and 64,000 client.
	- *Cloud-based WLC deployment*
		- Private cloud : The WLC can stay close to its APs to minimize the length of the data path between them. 
		- Public cloud : The WLC is placed in a distance and APs maintain a CAPWAP control tunnel to the controller.
		- Cloud-based controllers can typically support up to 6000 APs and 64,000 client.
		- Placed in the distribution layer.
	- *Distributed WLC deployment*
		- Multiple controllers are distributed within the network.
		- Used in a small campus.
		- Typical distributed WLCs can support up to 250 APs and 5000 client.
	- *Embedded Wireless Controller deployment* (EWC/ Controller-less wireless deployment)  
		- Used in a small-scale environments (branch location).
		- Placed in the access layer with an AP that is installed in the branch site.
		- The AP that hosts the WLC forms a CAPWAP tunnel with the WLC, along with any other APs at the same location.
		- An EWC can support up to 100 APs and 2000 client.
- **Cisco AP Modes** 
	- Local (default).
	- Monitor.
	- FlexConnect.
	- Sniffer.
	- Rogue detector.
	- Bridge.
	- Flex+Bridge.
	- SE-Connect.

# Chapter 3
```
- Authentication, message privacy, message integrity
- A message integrity check (MIC) is a security tool that can protect against data tampering.
```

- **Wireless Client Authentication Methods**
	- The original 802.11 standard offered only two choices to authenticate a client :
		- *Open Authentication*
			- Clients use an 802.11 authentication request before it attempts to associate with an AP.
		- *Wired Equivalent Privacy (WEP)*
			- Uses the RC4 cipher algorithm to make data private and hidden, same algorithm for encryption and decryption (same encryption key of {40 | 104} bits long).
			- Considered to be weak methods to secure a wireless LAN (deprecated).
	- *Extensible Authentication Protocol (EAP/802.1x)*
		- It can integrate with the IEEE 802.1x port-based access control standard. When 802.1x is enabled, it limits access to network media until a client authenticates.
		- Uses ***open authentication*** to associate with the AP, and then the actual client authentication process occurs at a dedicated authentication server.
		- 802.1x arrangement parties :
			- Supplicant : The client device that is requesting access.
			- Authenticator : The network device that provides access to the network (usually a WLC).
			- Authentication server (AS): The device that takes user or client credentials and permits or denies network access based on a user database and policies (usually a RADIUS server).
			- ![[EAP.png]]
			
		- *EAP-based authentication methods :*
			- Lightweight EAP (LEAP) - deprecated.
			- EAP Flexible Authentication by Secure Tunneling (EAP-FAST).
			- Protected EAP (PEAP).
			- EAP Transport Layer Security (EAP-TLS) - most secure.
- **Wireless Privacy and Integrity Methods**
	- *Temporal Key Integrity Protocol (TKIP)*
		- Adds security features using legacy hardware and the underlying WEP encryption such as MIC, Time Stamp, Sender's MAC address, TKIP sequence counter, Key mixing algorithm, Longer initialization vector (IV).
	- *Counter CBC-MAC Protocol (CCMP)* 
		- Consists of two algorithms, AES counter mode encryption and Cipher Block Chaining Message Authentication Code (CBC-MAC) used as a MIC (more secure than TKIP).
	- *Galois Counter Mode Protocol (GCMP)*
		- Consists of two algorithms, AES counter mode encryption and Galois Message Authentication Code (GMAC) used as a MIC (used in WPA3).
-  **Wi-Fi Protected Access (WPA-2-3)**
	- WPA was based on parts of 802.11i, 802.1x for authentication, TKIP and a method for dynamic encryption key management.
	- WPA2 is based around the superior AES CCMP.
	- WPA3 was introduced in 2018 as a future replacement for WPA2, it uses AES with GCMP and PMF to secure important 802.11 management frames (WLC <-> AP).
	- WPA3 includes other features beyond WPA and WPA2, such as Simultaneous Authentication of Equals (SAE), forward secrecy, and Protected Management Frames (PMF).
	- All versions support two client authentication mode : Personal mode (Pre-Shared Key (PSK)) & enterprise mode (802.1x).


| **Authentication and Encryption Feature Support** | **WPA** | **WPA2** | **WPA3** |
| ------------------------------------------------- | ------- | -------- | -------- |
| Authentication with pre-shared keys?              | Yes     | Yes      | Yes      |
| Authentication with 802.1x?                       | Yes     | Yes      | Yes      |
| Encryption and MIC with TKIP?                     | Yes     | No       | No       |
| Encryption and MIC with AES and CCMP?             | Yes     | Yes      | No       |
| Encryption and MIC with AES and GCMP?             | No      | No       | Yes      |
# Chapter 4
```
- Autonomous APs connect with the switched LAN with a trunk link & Cisco APs connect with an access link that 
  goes from the switched LAN to the WLC.
- WLCs are configured via a web browser using HTTP-S, authentication is handled by a list of local usernames or 
  an AAA server (TACACS+ or RADIUS).
- WLC has a console port for init boot functions, ethernet ports for out-of-band management and a redundancy port 
  connected to a peer controller for HA.
- Cisco controllers support a maximum of 512 WLANs, but only 16 of them can be actively configured on an AP.
- Advertising each WLAN to potential wireless clients uses up valuable airtime.
- The more WLANs created, the more beacons need to be announced (3 WLANs is best, but can go up to 5).
- Required parameters to create a WLAN: SSID string, Controller interface and VLAN number, Type of wireless 
  security needed.
- Policy profile is used to define how the controller should handle the WLAN profile
```


- **APs configuration on IOS-XE Controller**
	- Tags	
		- *Policy*
			- WLAN Profile: SSID, Band, Layer 2 & 3 Security, AAA
			- Policy Profile: VLAN, Multicast, ACL, URL Filters, QoS Ingress/Egress Policies
		- *Site*
			- AP Profile: CAPWAP Timers, AP Fallback, TCP MSS, Rogue Detection, ICap, QoS 
			- Flex Profile: Native VLAN, Local Auth, Policy ACL, VLAN, DNS Security
		- *RF*
			- RF Profile 2.4-5-6Ghz: Data Rates, MCS, RRM, Coverage Hole Detection, TPC, DCA
	-  Configuration > Wireless Setup > WLANs > Start Now

- **IOS-XE Tags and Profiles Configuration Sequence**
	- ![[Tags and Profiles.png]]
- **WLAN Configuration requirement**
	- *IOS-XE*
		- *Setup WLAN Profile*
			- ![[WLAN Creation - General.png]]
			- ![[WLAN Creation - Security_Layer2.png]] 
			- ![[WLAN Creation - Security_Layer3.png]]
			- ![[WLAN Creation - Security_AAA.png]]
			- ![[WLAN Creation - Advanced.png]]
		- *Setup Policy Profile*
			- ![[Policy profile - general.png]]
			- ![[Policy profile - access policies.png]]
			- ![[Policy profile - QOS & AVC.png]]
			- ![[Policy profile - advanced.png]]
		- *Mapping WLANs and policy profiles mapping to a policy tag*
			- ![[Policy tag.png]]
		- *Applying policy tag to APs* 
			- ![[Tag.png]]

	- AireOS
		- Create a dynamic interface; then assign an interface name and a VLAN ID.
		- Create a WLAN; then assign a WLAN profile name and SSID, along with a unique WLAN ID.
		- Configure the WLAN parameters, enable it, and allow it to broadcast the SSID.