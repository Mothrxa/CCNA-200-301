# Network Automation
---
# Chapter 21
```
- Data Plane refers to the process of forwarding data by the device.
- Control Plane refers to the process of controlling and changing data plane.
- Management plane refers to device management features.
- ASIC is a chip built for specific purposes, such as for message processing in a networking device.
- ASICs are used for data planes processing.
- A switch uses TCAM to store the equivalent of the MAC address table for fast table lookup.
- South Bound Interface: used by a management system to communicate with lower-level devices or network elements.
- North Bound Interface: used by a management system to communicate with higher-level applications.
- ACI uses IBN model to define the policies and intent for which endpoints should be allowed to communicate and 
  which should not. 
```

- **Logical Planes Functionality**
	- *Data Plane:*
		- De-encapsulating and re-encapsulating a packet in a data-link frame (routers, Layer 3 switches).
		- Adding or removing an 802.1Q trunking header (routers and switches).
		- Matching an Ethernet frame’s MAC address to the MAC address table (Layer 2 switches).
		- Matching an IP packet’s destination IP address to the IP routing table (routers, Layer 3 switches).
		- Encrypting the data and adding a new IP header (VPN processing).
		- Changing the source or destination IP address (NAT processing).
		- Discarding a message due to a filter (ACLs, port security).
	- *Control Plane:*
		- Routing protocols (OSPF, EIGRP, RIP,  BGP).
		- IPv4 ARP.
		- IPv6 NDP.
		- Switch MAC learning.
		- STP.
	- *Management Plane:*
		- CLI management (SSH, Telnet, Console).
		- Syslog.
		- SNMP.
		- NTP.

- **Spine-Leaf Topology**
	- Each leaf switch must connect to every spine switch.
	- Each spine switch must connect to every leaf switch.
	- Leaf switches cannot connect to each other.
	- Spine switches cannot connect to each other.
	- Endpoints connect only to the leaf switches.
- **OpenFlow and ACI Comparison**

| **Criteria**                                                                     | **OpenFlow** | **ACI**   |
| -------------------------------------------------------------------------------- | ------------ | --------- |
| Changes how the device control plane works versus traditional networking         | Yes          | Yes       |
| Creates a centralized point from which humans and automation control the network | Yes          | Yes       |
| Degree to which the architecture centralizes the control plane                   | Mostly       | Partially |
| SBIs used                                                                        | OpenFlow     | OpFlex    |
| Controllers mentioned in this chapter                                            | OpenDaylight | APIC      |
| Organization that is the primary definer/owner                                   | ONF          | Cisco     |
- **Impacts Of Automation In Network Management**
	- Northbound APIs and their underlying data models make it much easier to automate functions versus traditional networks.
	- The robust data created by controllers makes it possible to automate functions that were not easily automated without controllers.
	- The new reimagined software defined networks that use new operational models simplify operations, with automation resulting in more consistent configuration and fewer errors.
	- Centralized collection of operational data at controllers allows the application of modern data analytics to networking operational data, providing actionable insights that were likely not noticeable with the former model.
	- Time required to complete projects is reduced.
	- New operational models use external inputs, like considering time of day, day of week, and network load.
- **Traditional Networks Vs Controller-Based Networks**
	- Uses new and improved operational models that allow the configuration of the network as a system rather than per-device configuration.
	- Enables automation through northbound APIs that provide robust methods and model-driven data.
	- Configures the network devices through southbound APIs, resulting in more consistent device configuration, fewer errors, and less time spent troubleshooting the network.
	- Enables a DevOps approach to networks.
# Chapter 22
```
- The southbound of the controller contains components such as the fabric, the underlay, and the overlay.
- The underlay network provides reachability (IP connectivity) to all Cisco SD-Access devices in order to 
  support the dynamic creation of VXLAN overlay tunnels.
- Overlays use VXLAN tunnels and are used to transport traffic from one endpoint to another over the fabric.
- Fabric: Combines overlay and underlay technologies, which provide all features to deliver data across the 
  network with the desired features and attributes.
- SD-Access fabric roles: Edge node, Border node & Control node (performs LISP).
- Locator ID Separation Protocol (LISP) used for endpoint discovery and location information needed to create the 
  VXLAN tunnels.
- LISP provides the control plane of SD-Access.
	  - A list of mappings of EIDs (endpoint identifiers) to RLOCs (routing locators) is kept.
	  - EIDs identify end hosts connected to edge switches and RLOCs identify the edge switch which can be used 
	    to reach the end host.    
- Cisco TrustSec (CTS) provides policy control (QoS, security policy, etc).
- VXLAN provides the data plane of SD-Access.
- Cisco Catalyst Center can act as a Network Management Platform.
- AI (Narrow, Generative, Predictive).
- AI Ops can help lead the IT operations staff to the Root Cause Analysis (RCA) or Root Cause of Failure (RCF) by 
  determining what went wrong in the first place.
```

- **Network Management System Features**
	- *Single-pane-of-glass:* Provides one GUI from which to launch all functions and features.
	- *Discovery, inventory, and topology:* Discovers network devices, builds an inventory, and arranges them in a topology map.
	- *Entire enterprise:* Provides support for traditional enterprise LAN, WAN, and data center management functions.
	- *Methods and protocols:* Uses SNMP, SSH, and Telnet, as well as CDP and LLDP, to discover and learn information about the devices in the network.
	- *Life cycle management:* Supports different tasks to install a new device (day 0), configure it to be working in production (day 1), and perform ongoing monitoring and make changes (day n).
	- *Application visibility:* Simplifies QoS configuration deployment to each device.
	- *Converged wired and wireless:* Enables you to manage both the wired and wireless LAN from the same management platform.
	- *Software Image Management:* Manages software images on network devices and automates updates.
	- *Plug-and-Play:* Performs initial installation tasks for new network devices after you physically install the new device, connect a network cable, and power on.
- **Cisco Catalyst Center Features**
	- *Application policy:* Deploys QoS, one of the most complicated features to configure manually, with just a few simple choices from Cisco Catalyst Center.
	- *Encrypted Traffic Analytics (ETA):* Enables Cisco DNA to use algorithms to recognize security threats even in encrypted traffic.
	- *Device 360 and Client 360:* Gives a comprehensive (360-degree) view of the health of the device.
	- *Network time travel:* Shows past client performance in a timeline for comparison to current behavior.
	- *Path trace:* Discovers the actual path packets would take from source to destination based on current forwarding tables.
- **AI Ops Use Cases**
	- *Automation:* Taking routine tasks and reducing human intervention using automated processes.
	- *Monitoring and Data Analytics:* Providing constant analysis of data by using advanced tools to identify patterns and anomalies.
	- *Predictive Analysis:* Leveraging machine learning to proactively predict issues by analyzing historical data and patterns.
	- *Collaboration and Communication:* Promoting information sharing and troubleshooting techniques across IT operations as a whole.
	- *Root Cause Analysis:* Determining the underlying causes of problems by having deep visibility into the various areas of an IT environment.
# Chapter 23
```
- REST APIs follow six attributes defined by Roy Fielding: Client/server architecture, Stateless operation, 
  Clear statement of cacheable/uncacheable, Uniform interface, Layered, and Code-on-demand.
- REST stateless operation: the server does not keep state information about prior requests; the client 
  maintains all state data.
- Cacheable resources are marked with a timeframe so the client knows when to request a fresh copy.
- Python is the most popular language for network automation.
- Simple variables: one name, one value (int, float, string).
- List variable: one name assigned to a list of values.
- Dictionary variable: paired key:value items (like terms and definitions).
- CRUD actions: Create, Read, Update, Delete — map to HTTP verbs POST, GET, PATCH/PUT, DELETE.
- URI structure: Scheme (protocol) + Authority (host:port) + Path (resource) + Query (parameters).
- Authentication to Cisco Catalyst Center uses HTTP POST and returns an X-Auth-Token valid for 1 hour,
  which must be passed as a header in all subsequent API calls.
- JSON is the primary data serialization format returned by REST APIs.
- Postman is an API development environment tool used to test and build REST API calls.
```

- **Python Variable Types**
	- *Simple Variables:*
		- Unsigned integers.
		- Signed integers.
		- Floating-point numbers.
		- Text strings.
	- *Complex Variables:*
		- List: one variable name maps to an ordered list of values.
		- Dictionary: one variable name maps to a set of key:value pairs.
		- Nested: lists can contain dictionaries, dictionaries can contain lists, and so on.

- **REST API Attributes**
	- Client/server architecture.
	- Stateless operation.
	- Clear statement of cacheable/uncacheable.
	- Uniform interface.
	- Layered.
	- Code-on-demand.

- **CRUD and HTTP Verb Mapping**

| **Action**                          | **CRUD Term** | **HTTP Verb** |
| ----------------------------------- | ------------- | ------------- |
| Create new data structures          | Create        | POST          |
| Read/retrieve variables and values  | Read          | GET           |
| Update or replace variable values   | Update        | PATCH / PUT   |
| Delete variables or data structures | Delete        | DELETE        |

- **URI Structure Components**
	- *Scheme (Protocol):* Identifies the protocol, e.g. HTTPS.
	- *Authority (Host:Port):* Hostname or IP address, optionally with port number.
	- *Path (Resource):* Uniquely identifies the resource as defined by the API.
	- *Query (Parameter):* Follows the `?`, assigns values to variable names to pass data.

- **Cisco Catalyst Center API Authentication Steps**
	1. Send HTTP POST to the Authentication API URI with Basic Auth credentials.
	2. Receive an X-Auth-Token (Base64-encoded, valid 1 hour).
	3. Include the X-Auth-Token in the Headers of all subsequent API calls.
	4. Use HTTP GET with the appropriate resource URI (e.g. `/dna/intent/api/v1/network-device`) to retrieve data.
- **Rest API Authentication Methods** 
	- *Basic Authentication:* Sends username and password in every request, encoded in Base64.
	- *Bearer Authentication:* Uses a token (bearer token) as an HTTP header in each request to verify the client's identity.
	- *API Key Authentication:* Requires a unique key, typically included as an HTTP header to authenticate API requests.
	- *OAuth 2.0:* A secure framework that grants access via access tokens, commonly used for delegated access and third-party authentication.
# Chapter 24
```
- Configuration Monitoring: With configuration management tools like Ansible, a process of comparing over time a 
  device’s on-device configuration (running-config) versus the text file showing the ideal device configuration 
  listed in the tool’s centralized configuration repository. If different, the process can either change the
  device’s configuration or report the issue.
- Configuration Provisioning: With configuration management tools like Ansible, the process of configuring a 
  device to match the configuration as held in the configuration management tool.
- Configuration Template: With configuration management tools like Ansible, a file with variables, for the 
  purpose of having the tool substitute different variable values to create the configuration for a device.
- Ansible is best suited for device configurations whereas Terraform is more often used for infrastructure 
  provisioning.
- Ansible uses an agentless architecture and push model (can use pull with ansible-pull).
- Terraform uses HashiCorp Configuration Language (HCL).
```

- **Configuration Drift For Manual Configuration**
	- The on-device manual configuration process does not track change history: which lines changed, what changed on each line, what old configuration was removed, who changed the configuration, when each change was made.
	- External systems used by good systems management processes, like trouble ticketing and change management software, may record details. However, those sit outside the configuration and require analysis to figure out what changed. They also rely on humans to follow the operational processes consistently and correctly; otherwise, an engineer cannot find the entire history of changes to a configuration.
	- Referring to historical data in change management systems works poorly if a device has gone through multiple configuration changes over a period of time.
- **Management Tools Features**
	- The core function to implement configuration changes in one device after someone has edited the device’s centralized configuration file.
	- The capability to choose which subset of devices to configure: all devices, types with a given attribute (such as those of a particular role), or just one device, based on attributes and logic.
	- The capability to determine whether each change was accepted or rejected, and to use logic to react differently in each case depending on the result.
	- For each change, the capability to revert to the original configuration if even one configuration command is rejected on a device.
	- The capability to validate the change now (without actually making the change) to determine whether the change will work or not when attempted.
	- The capability to check the configuration after the process completes to confirm that the configuration management tool’s intended configuration does match the device’s configuration.
	- The capability to use logic to choose whether to save the running-config to startup-config or not.
	- The capability to represent configuration files as templates and variables so that devices with similar roles can use the same template but with different values.
	- The capability to store the logic steps in a file, scheduled to execute, so that the changes can be implemented by the automation tool without the engineer being present.
- **Templates advantages**
	- Templates increase the focus on having a standard configuration for each device role, helping to avoid snowflakes (uniquely configured devices).
	- New devices with an existing role can be deployed easily by simply copying an existing per-device variable file and changing the values.
	- Templates allow for easier troubleshooting because troubleshooting issues with one standard template should find and fix issues with all devices that use the same template.
	- Tracking the file versions for the template versus the variables files allows for easier troubleshooting as well. Issues with a device can be investigated to find changes in the device’s settings separately from the standard configuration template.
- **Ansible**
	- *Config Tree*
		- *Playbooks:* These files provide actions and logic about what Ansible should do.
		- *Inventory:* These files provide device hostnames and information about each device, like device roles, so Ansible can perform functions for subsets of the inventory.
		- *Templates:* Using Jinja2 language, the templates represent a device’s configuration but with variables.
		- *Variables:* Using YAML, a file can list variables that Ansible will substitute into templates.
	- *Role*
		- *Configuration Monitoring*: Ansible uses logic modules that detect and list configuration differences, after which the playbook defines what action to take (reconfigure or notify).
		- *Configuration Provisioning:* Configuring devices based on the changes made in the files.
- **Terraform**
	- *Components*
		- *Configuration files:* Files that describe the desired state of the infrastructure and specify resources along with their configurations.
		- *Providers:* Plugins that enable interaction with different infrastructure platforms (AWS, Azure, Google Cloud).
		- *Resources:* Components you want to create and manage, such as virtual machines, networks, or databases.
		- *Data sources:* Reference details about existing infrastructure within your configuration.
		- *Variables:* A method to reuse code and make configurations more flexible.
		- *State files:* Record the infrastructure’s current state and track configuration changes that are made.
		- *Modules:* Organize and reuse Terraform code. Modules encapsulate a set of resources and configurations that can be treated as a single unit. Modules promote code reusability and modularity.
	- *Stages*
		1. *Write:* This is where you define the resources that will be used across multiple cloud providers or services.
		2. *Plan:* An execution plan is created that describes the infrastructure that will be created, updated, or destroyed based on the current configuration and infrastructure.
		3. *Apply:* Once the plan is approved, the proposed operations will be completed in the appropriate order.

- **Ansible and Terraform Comparison**

| **Attribute**        | **Ansible**  | **Terraform**       |
| -------------------- | ------------ | ------------------- |
| File type for config | Playbook     | Configuration       |
| Protocol to Network  | SSH, NETCONF | APIs                |
| Execution Mode       | Agentless    | Client/server model |
| Push/Pull Model      | Push/Pull    | Push                |
| Modeling Language    | YAML         | HCL                 |