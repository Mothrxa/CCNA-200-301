# VLANs - STP - RSTP - EtherChannel
--- 

# Chapter 8 

`VTP tool isn't covered in the exam, the current exam's blueprint ignores and many entreprise disable VTP.`

```
- Switches have 4094 Vlan distributable, a range from 1-4094 with 0 & 4095 being reserved, all switches can use  
  a normal range is from 1 to 1005 and only some can use an extended range from 1006 to 4096.
- Untagged frames are treated as part of the native Vlan (frames without 802.1Q header).
- Access ports forward untagged frames that belong to the same Vlan.
- Trunk ports forward tagged frames to multiple Vlans.
- Default Vlan (Vlan 1) is the one that all the switche's ports belong to initially.
- Defined in 802.1Q
```

- **Trunking**
```
- Frames are identified by tags (Vlan id), each frame goes to the assigned Vlan.
- It allows switches to forward frames from multiple switches over a single physical connection.
- Switches forward only the frames that belong to the same Vlan in the same subnet.

- DTP negotiate the type of trunking and whether the switch should use trunking or not (admin mode).
- For better security disable trunk negotiations (DTP) (command: #switchport nonegotiate).
```

- **Data VLAN:** Same idea and configuration as the access VLAN on an access port but defined as the VLAN on that link for forwarding the traffic for the device connected to the phone on the desk (typically the user’s PC).
- **Voice VLAN:** The VLAN defined on the link for forwarding the phone’s traffic. Traffic in this VLAN is typically tagged with an 802.1Q header.
- **IP Telephony Ports on Switches:** 
	- Configure these ports as a static access port and assign it an access VLAN.
	- Add one more command to define the voice VLAN.
	- Look for the mention of the voice VLAN ID in `show interfaces type id switchport` command.
	- Look for both the voice and data (access) VLAN IDs in the output of `show interfaces type id trunk` command.
	- Do not expect to see the port listed in the list of operational trunks as listed by `show interfaces trunk` command.
### Configuration commands
- (config)#vlan **vlan-id** (creates a Vlan)
- (config)#interface **interface** (can use **interface range ==portType== 0/N** to configure multiple interfaces at the same time)
	- *Access*
		- (config-if)#switchport access vlan **id** (configure interface to use access for the associated vlan)  -  (creates vlan if not exist)
		- (config-if)#switchport mode access (optional, tells the switch to always be an access interface - disables DTP)

	- *Trunk*
		- (config-if)#switchport trunk {allowed {vlan **id** | all} | native vlan **id**}
		- (config-if)#switchport mode trunk
		- (config-if)#switchport nonegotiate (disables DTP) 
		- (config-if)#switchport trunk encapsulation dot1q (not necessary on switches that support only 802.1Q, use it if trunk mode is configured manually)

- (Switch)#show vlan [brief | **id**] 
- (Switch)#show interfaces **interface-name** switchport
- (Switch)#show mac address-table
- (Switch)#vtp mode transparent | vtp mode off  ---- show vtp status
- (config-if)#switchport voice **vlan-id** (activates voice Vlan in **vlan-id**)
<br>

- **Trunking Administrative Mode Options with the ==switchport== mode Command**

	- $the\ negotiation\ is\ made\ to\ choose\ whether\ the\ link\ becomes\ a\ trunk\ or\ an\ access\ port.$

| **Command**           |                                                               **Description**                                                                |
| :-------------------- | :------------------------------------------------------------------------------------------------------------------------------------------: |
| access                |                                                   Always act as an access (nontrunk) port                                                    |
| trunk                 |                                                          Always act as a trunk port                                                          |
| <br>dynamic desirable |       Initiates negotiation messages and responds<br>to negotiation messages to dynamically<br>choose whether to start using trunking        |
| <br>dynamic auto      | Passively waits to receive trunk negotiation<br>messages, at which point the switch will<br>respond and negotiate whether to use<br>trunking |

# Chapter 9

```
- STP/RSTP port state: listening, forwarding, blocking.
- Frame loops cause broadcast storms.
- Switches send each other Hello BPDU messages that contains (Root BID, sender BID, sender root cost, Timer).
- STP is enabled in switches by default.
- STP takes 50s to converge in default settings when topology changes, RSTP takes 10s. 
- PVST uses ISL encapsulation and PVST+ uses dot1q encapsulation.
- PVST+ uses 01:00:0c:cc:cc:cd as the multicast destination mac address.
- Regular STP uses 01:80:c2:00:00:00 as the multicast destination mac address.
```

- **STP**

```
- RSTP/STP elect the root switch using the same rules and tiebreakers.
- RSTP/STP switches select their root ports with the same rules.
- RSTP/STP elect designated ports on each LAN segment with the same rules and tiebreakers.
- RSTP/STP place each port in either forwarding or blocking state, although RSTP calls the blocking state the
  discarding state.
- Defined in 802.1D
```

- **STP Root Path Cost Tie**
	1. Lowest neighbor bridge ID.
	2. Lowest neighbor port priority.
	3. Lowest neighbor internal port number.

- **STP port priority**
	- First half of the port ID, example: port-id = 0x8002, '80' is the port priority in hex (128 in decimal).

- **STP Timers**

| **Timer**     | **Default value** | **Description**                                                                                                                                                                                                                                                         |
| :------------ | :---------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hello         | 2 seconds         | The time period between Hellos created by the root                                                                                                                                                                                                                      |
| Max Age       | 10 times Hello    | How long any switch should wait, after ceasing to hear Hellos, before trying to change the STP topology.                                                                                                                                                                |
| Forward Delay | 15 seconds        | Delay that affects the process that occurs when an interface changes from a blocking state to a forwarding state. A port stays in an interim **listening state**, and then an interim **learning state**, for the number of seconds defined by the forward delay timer. |

- **RSTP**
```
- Adds a mechanism by which a switch can replace its root port, without any waiting to reach a forwarding state.
- Adds a new mechanism to replace a designated port, without any waiting to reach a forwarding state.
- Uses a handshake mechanism that allows switches to actively negotiate & move directly to the forwarding state.
- Lowers MaxAge to 3 times Hello timer.
- Sends messages to the neighbouring switch to inquire whether a problem has occurred rather than wait for timer.
- Switches tell each other that the topology has changed and thus all of the receivers flush their MAC table to 
  eliminate all the potentially loop-causing entries.
- Each switch generates its own Hello.
- Learning state isn't used, disabled/blocking state becomes discarding.
- Backup port role (to be DP in failover) is only used when multiple switch ports are connected to the same hub.
- Converges quickly on point-to-point edge ports by bypassing the learning state.
- Cisco switches enable RSTP point-to-point edge ports by enabling PortFast on the port.
- Was originally defined in 802.1w and was later merged into 802.1Q for bridging and VLANs.
- BackboneFast & UpinkFast are optional features.
```

- **RSTP Port Role**

| Function                                                                 | Port Role       |
| ------------------------------------------------------------------------ | --------------- |
| Nonroot switch’s best path to the root                                   | Root port       |
| Port that will be used to replace the root port when the root port fails | Alternate port  |
| Switch port designated to forward onto a collision domain                | Designated port |
| Port that will be used to replace a designated port when it fails        | Backup port     |
| Port in a nonworking interface state (anything other than up/up)         | Disabled port   |

- **EtherChannel**
	- Uses multiple parallel links of equal speed, treated as a single interface and avoids convergence. 
- **PortFast**
	- After the interface reaches a connected state, it immediately moves the port to DP role and forwarding state without delay.
	- Speeds RSTP convergence.
	- Creates forwarding loops when connected to a switch because the port ignores BPDUs and immediately forwards traffic.
- **BPDU Guard**
	- Blocks ports that receive BPDU message to prevent it from becoming designated port.
	- Ports that use PortFast should not receive BPDUs under normal circumstances.
	-  Re-enable the err-disabled port with shutdown + no shutdown to reset (manual), or with **ErrDisable Recovery** (automatic)
	- **ErrDisable Recovery**
		- Default recovery timer is 5 minutes (300 seconds)
	- **BPDU Filter**
		- It stops a port from sending and receiving BPDUs in that interface. (disables STP on the port)
		- Protects against forwarding loops on PortFast ports and disables STP by filtering all BPDU messages.
		- when configured globally
			- If the switch receives BPDU message, it disables PortFast and reverts the port to use STP with normal STP rules.
- **Root Guard**
	- It monitors incoming BPDUs on a port and reacts if the BPDU changes the root switch.
	- Prevents the election of a new root switch on that port.
	- It listens for incoming Hellos on the port, if it is a superior hello then the switch disables by using the broken state.
	- It recovers the port automatically when the superior Hellos no longer occur.
	- Prevents designated ports from becoming root ports.
- **Loop Guard**
	- Happens on unidirectional links caused by damaged cables or faulty connectors/transceivers, common with fiber cables.
	- If the port is a root or alternate port, prevent it from becoming a designated port by moving it to the special broken state.
	- Disables an interface if it stops receiving BPDUs.
	- Should be enabled on root and non-designated ports.
	- Prevents root and non-designated ports from becoming designated ports.

 - Root Guard and Loop Guard are mutually exclusive, they can't be enabled on the same port at the same time as they serve opposite purposes.
 - If one is configured globally and the other is configured on an interface, the one on the interface takes effect.
 - if both configured on the interface the second one replaces the first one.

# Chapter 10
```
- Cisco switches default to use a default base priority of 32768.
- RSTP sends all BPDUs in the native VLAN without a VLAN tag.
```

- Layer 2 EtherChannel
```
- Configured with static config or by allowing dynamic protocols to create the channel (dynamic is recommended).
- port links are grouped with the channel-group command to be part of the channel.
- the terms EtherChannel, PortChannel, Channel-group are synonyms.
- PAgP and LACP protocols are used to negotiate whether a link becomes part of the EtherChannel or not.
- LACP was originally defined in 802.3ad but now it's defined in 802.1AX.
- LACP support up to 16 links with only 8 active - 8 standby compared to PAgP's maximum 8.
- STP/RSTP default cost == 3 with EtherChannels 2-8 active 1 Gbps links.
- New physical interface’s settings must be the same as the existing ports’ settings, otherwise the switch does 
  not add the new link to the list of approved and working interfaces in the channel.
- Settings include Speed, duplex, switchport mode, allowed VLANs/native VLANs.
- No switchport command is used in an interface mode to use layer 3 etherchannel.
```

### Configuration commands
- *STP*
	- (config)#spanning-tree pathcost method {short | long} (short for old values and long for new)
	- (config)#spanning-tree mode {pvst | rapid-pvst}
	- (config)#spanning-tree vlan **vlan-id** {root {primary | secondary} | port-priority **value**} (primary sets the priority to 24576 or 4096 less than the current root, secondary sets it to 28672 | port-priority's value is configured in increments of 32 (0-224))
	- (config-if)#spanning-tree vlan **vlan-id** cost **cost**
	- (config-if)#spanning-tree link-type {point-to-point | shared} (RSTP link type port)
	- (config)#show spanning-tree interface **interface-name** detail
	- (switch)#debug spanning-tree (enables event debugging)
- *PortFast*
	- (config)#spanning-tree portfast default (enables portfast on all access ports)
	- (config-if)#spanning-tree portfast [disable] (enabled/disabled on the interface)
	- (config-if)#spanning-tree portfast trunk
	!- switches auto-complete edge keyword to the configuration command, except for disable command
- *BPDU Guard/Filter*
	- (config)#spanning-tree portfast bpdufilter default (enables BPDU filter on all portfast ports)
	- (config)#spanning-tree portfast bpduguard default (//)
	- (config-if)#spanning-tree bpduguard {enable | disable}
	- (config-if)#spanning-tree bpdufilter {enable | disable} (not recommended)
	- **Error-disabled**
		- (switch)#show errdisable recovery 
		- (config)#errdisable recovery cause bpduguard (enable the ErrDisable recovery for ports disabled by bpduguard)
		- (config)#errdisable recovery interval **seconds**
- *Root Guard*
	- (config-if)#spanning-tree guard root
	- (config-if)#spanning-tree guard none
- *Loop Guard*
	- (config-if)#spanning-tree guard loop
	- (config)#spanning-tree loopguard default
	- (config-if)#spanning-tree guard none
<br>
- *EtherChannel*
	- (config-if)#channel-protocol {lacp | pagp} (useless command since protocol is dynamically chosen)
	- Display details
		- (switch)#show interfaces port-channel **id**  
		- (switch)#show etherchannel [summary | port-channel | load-balance]
	- Static method
		- (config-if)#channel-group **number** mode on
	- Dynamic method
		- (config)#channel-group **number** mode {desirable | auto} (enables PAgP with at least one side set to desirable)
		- (config)#channel-group **number** mode {active | passive} (enables LACP with at least one side set to active)
			!- Do not use the on parameter on one end and {auto | desirable | active | passive} on the other end, it would prevent the EtherChannel from working.
	- Load balancing 
		- (config)#port-channel load-balance {src-mac | src-dst-mac | src-ip | dst-ip | src-dst-ip | src-port | dst-port | src-dst-port}