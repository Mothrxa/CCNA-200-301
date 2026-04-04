# Ethernet LANs

---
# Chapter 4

- Basic CLI commands
### Configuration commands
- (config)#interface {type} {port-number}
- (config-if)#speed {10 | 100 | 1000 | auto}
- (config)#hostname **name**
- (config)#enable password **password**
- (config)#service password-encryption
- (config)#enable secret **password**
- (config)#exit
- (config)#end | Ctrl+Z
### EXEC commands
- (#)debug all | undebug all
- (#)reload
- (#)copy running-config startup-config
- (#)copy startup-config running-config
- (#)show running-config
- (#)write erase | erase startup-config | erase nvram
- ()#quit
- (#)show startup-config
- ()>enable
- (#)disable
- (#)configure terminal
- (#)show mac address-table

# Chapter 5
- Switches forward frames based on the destination MAC address:
    1. If the destination MAC address is a broadcast, multicast, or unknown destination unicast, the switch floods the frame.
    2. If the destination MAC address is a known unicast address :
        1. If the outgoing interface listed in the MAC address table is different from the interface in which the frame was received, the switch forwards the frame out the outgoing interface.
        2. If the outgoing interface is the same as the interface in which the frame was received, the switch filters the frame, meaning that the switch simply ignores the frame and does not forward it.
- Switches learn MAC address table entries based on the source MAC address:
    1. For each received frame, note the source MAC address and incoming interface ID.
    2. If not yet in the MAC address table, add an entry listing the MAC address and incoming interface.
- **Switch's default settings**
	- The interfaces are enabled by default, ready to start working once a cable is connected.
	- All interfaces are assigned to VLAN 1.
	- 10/100 and 10/100/1000 interfaces use autonegotiation by default.
	- The MAC learning, forwarding, flooding logic all works by default.
	- STP is enabled by default.
### Configuration commands
- (#)show mac address-table
- (#)show mac address-table dynamic [vlan **vlan-id** | address **mac-address** | interface **interface-id**] 
- (#)show mac address-table {count | aging-time}
- (#)show interfaces **id** counters
- (#)show interfaces status
- (#)clear mac address-table dynamic [vlan **vlan-number**] [interface **interface-id**] [address **mac-address**]
- (config)#mac address-table aging-time **time-in-seconds** [vlan **vlan-number**]

# Chapter 6
```
- VTY lines are configured for Telnet & SSH login.
- Connect to console with a rollover cable.
- Configure Switch's IPv4 static address (manually) or dynamic address (dhcp)
```

- **Securing User mode & Privileged mode**

```
1/Shared passwords:
- Passwords are shared from the user's perspective, not each user has a unique password.
- password "": sets the password 
- login: enables the password next time on the configured line.
- enable password: enables the password for global configuration (all lines).
- enable secret: same logic but uses md5 for encryption.
2/Local usernames&passwords:
- username "" password "": uses one password per user (use secret instead of password for encryption).
- login local: uses the local list of usernames after enabling the local usernames&passwords.
- no password: clears the old shared passwords list.
3/External authentication servers: 
- AAA servers (Authentication, Authorization, Accounting)
	- Holds the usernames*passwords.
	- Allows users to do self-service by forcing maintenance on their passwords.
 - Function: 
	- Switch/Router sends request to the AAA server asking whether the username&password are allowed, the server 
	  replies with approving/denying 
```

- **SSH Config**
	1. Configure Host Name
	2. Configure DNS domain name
	3. Generate RSA key pair
	4. Configure password
	5. Enable SSHv2 only 
	6. Configure VTY lines
### Configuration commands
- (config)#username **name** secret **pass-value**
- (config)#enable algorithm-type {md5 | sha256 | scrypt} secret **password**
- (config)#crypto key generate rsa [modulus {512 | 768 | 1024}]
- (config)#ip ssh version 2
 - (config)#line {console 0 | vty **1st-vty** **last-vty**}
	- (config-line)#exec-timeout **minutes** [**seconds**] (log user out after X minutes Y seconds of inactivity)
	- (config-line)#password **password**
	- (config-line)#login [local]
	- (config-line)#transport input {telnet | ssh | all | none}
	- (config-line)#history size **length**
	- (config-line)#logging synchronous
- >ssh {**username**@**ip-address** | -l **username ip-address**}
### EXEC commands
- (#)show running-config
- (#)show running-config | begin **line vty**
- (#)show dhcp lease
- (#)show crypto key mypubkey rsa
- (#)show ip ssh
- (#)show ssh
- (#)show interfaces vlan **number**
- (#)show ip default-gateway

# Chapter 7

```
- When autonegotiation is disabled on one end, the other end sends FLPs and receives none, it concludes that the 
  other doesnt support autonegotiation, it enables parallel detection to listen for incoming line signal
- 10/100 use half duplex, 1000/+ use full duplex
- Hubs don't send FLP messages, devices connected to it use parallel detection 
```

- **IEEE Autonegotiation**

- Autonegotiation gives the devices on each link the means to agree to use the best speed without manually 
  configuring the speed on each switch port.
- Devices send FLP (Fast Link Pulses) to declare their capabilities (speeds-duplex settings supported).
- FLP use out of band electrical signaling, it establishes the connection for transmission before link up.
- Use case:
	- Both endpoints send messages "out of band" compared to any specific data transmission using FLPs.
	- The messages declare all supported speed and duplex combinations.
	- After hearing from the link partner each device chooses the fastest speed supported by both devices and the best duplex.
### Configuration commands
- (config)#interface {type} {port-number}
- (config)#interface range {type} {port-number} - {end-port-number}
- (config-if)#shutdown | no shutdown
- (config-if)#speed {10 | 100 | 1000 | auto}
- (config-if)#duplex {auto | full | half}
- (config-if)#description **text**
- (config-if)#no duplex
- (config-if)#no speed
- (config-if)#no description
- (config)#default interface **interface-id**
- (config-if)#[no] mdix auto
### EXEC commands
- (#)show running-config
- (#)show running-config interface {type} {number}
- (#)show running-config all
- (#)show interfaces [{type} {number}] status
- (#)show interfaces [{type} {number}]
- (#)show interfaces description