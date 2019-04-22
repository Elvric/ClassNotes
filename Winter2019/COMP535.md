# COMP 535
# Lecture 1 
## General Overview
End divices are connected to intermediary devices (rooters, ethernet cables) that themselves are connected to the network. The network can be accessed through communiation links:
- copper cables
- Fiber Optic
- Wireless

# 09/01/2019
## Overview
- End device
  - Computer
- Intermediate device
  - Router
  - Switch

### Communication links
Can be wired or wireless.

Wire: can be made through cupper cables and we now use more and more fiber.

### Protocols
Set of actions that need to be undertaken in order for communication to occur successfully between devices.

## Internet structure
### Network edge

### Network core
We have all the routers (less switches) belonging to the internet service provider transfering information from one network to another.

### Access network technologies
#### Digital subscribers (DSL)
Goes over the dedicated telephone line to go to the internet (independent we are not that affective by other usage). 24 Mbps in

#### Cabe Modem
The maximum capacity of the link is shared between users so we do not have any garenties regarding the speed but the maximum speed can be hire than DSL but dependent on other users. 30 Mbps  
Data goes through the TV cable

#### Fiber
Because of the light transmission we can expect min of 10 Mbps per sec but can go up to Gigabytes and so one.

### Entreprise network
Have a second level with the ISP router that allow people to access the specific business e-mail for example.

### ISP network core
ISP is a group of routers all connected together (keep in mind the hiarchy of the ISP) where we have the national ISP with then the International level and so on.

## Information movement
### Layered approach
We have a source that has information for example (password info, loggin) and place into packages that are tagged with specific information (confidential and so on), the package is then transported through the network from your location to the destination where they are unwrapped. The path taken can vary depending on the traffic and so on.

__Advantages__:  
- Allow to deal with complex network systems
- easier maintenance and system update (change in one layer does not affect the other)
- Locate causes for a problem is easier (troubleshooting)

### OSI Model layer
Is a theoretical model that allows to standardizes how network operates. Layer serves the layer above it and is served by the layer bellow it.

1. Physical
2. Data link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

#### Application
What is the communcation applying in the example given in class house moving application.

#### Presentation
Aims to tell how do we want to represent in the information (formating aspects of the information) box name in the house moving application

#### Session
Sychronization points (where to retrieve the data) in the case of the house moving it is the time at arrival at the new house

#### Transport layer
Knowing the method and how to transport the information arround depending on their specified properties. Is it fragile, expensive and so on

#### Network layer
The infrastructure available to carry the information from the destination palce to another we need to define the route in there based on different criteria (ease of access, conjection and so on). In our case it was the choice of what road to take.

#### DataLink
The maner through which the information will be transported through the route. In our case the speed of the trucks respect direction and traffic lights.

#### Physical
Looking at the infrastructure what is the specific rules that apply at that specific moments (are there any problems?).

### Applying the model in Networkds
We have a browser open on a computer connected to a switch that is connected to the IPS? that provide a connection to the mcgill server responsible for myCourses.

#### Application
PC and servers can understand up to applicaton layer which means that it has the capacity to understand everythink happening at a lower level

In network we have:
- http/https web document access and transfer
- DNS Domain name resolution (get the IP address behind servers)
- ssh remote access
- SMTP, POP transfer of e-mail messages
- SNMP network manager


#### Presentation
Allow interpretation between communicating applications of data exchange (usally gone in practice as they are usually included in appllication)

#### Session
SCP (session control protocol). 

#### Transport
Here we care about the communication through the network
- TCP: guarenteed deliver, flow control, conjestion control, error recovery
- UDP: non of the above are garenteed

One reason to have less garenteed is to communicate faster
The firewall is at that level

#### Network
Here we care about the communcation between end devices (PC to server for example)
Router is at that level. Used to have the logical address and the path determination
- IP addressing
- Routing: OSPF (open shortest path first), EIGRP, BGP (border getway protocol used between ISPs)

#### DataLink
Physical address (MAC). moving data units between node along the transmission path.
- error detection
- flow control
- collision management

Protocols: ethernet, wifi

Equipement: access points, switches

#### Physical
How individual bits are moved from one node to another node. Define specification between a device and a physical transmission medium (cable spe, frequency)

Equipement:
- Transceivers
- Repeaters
- Hubs
- Path pane;
- Cables and connectors

#### PDU
|Layer|PDU|
|--|--|
|Application|Data|
|Presentation|Data|
|Session|Data|
|Tranpsort|Segments|
|Network|Packet|
|DataLink|Frame|
|Physical|Bit|

### Data encapsulaton and deencapsulation
Adding a header at a specific layer from the PDU above it. Putting all objects in the same box and adding a name to the box to be able to distingish it for example.

Dencapsulatio is the process of removing a header done at destination or intermediate nodes.

# 14/01/19
## Delays
### Nodal processing delay
dproc, referest to the time taken to examin the packet header, determine where to dirct teh packet, check for bit level errors usally last a few milli seconds

### Queuing delay (could even be a phd)
dqueue, waiting time in queue before transimission which depends on the number of previous packets in the queue, works as a function of intensity and nature of packets arriving to the queue. The order goes from micro to milli seconds.

### Transmission delay
ptrans = L/R the time to put the packet out onto the link/wire. L is the lenght of packet in bits and R is the transimission rate of link in bits. Goes from milli to micro seconds.

### Propagation delay
dprop = d/s the time it takes for a bit to travel the wire which depends on the type of media used (fiber, copper). s is a little less than the speed of light, sd is the distance between the nodes s ranges between 2\*10^8 to 3\*10^8 m/s.

## Packet loss
Rooters have a finite capacity so sometimes the queue can overflow in this case the rooters does not have enough space to store the packets so it just looses it (packet loss). Regions like that are called bottle necks of the network, the previous rooter sends packet faster than the rate at which the next rooter can transmit them so packets pile up there until overflow where the packets sent are just being lost.

### Trancert Command
It returns the name/ip address of routers along the way between the source and destination host as well as the round-trip delay and if any packet is lost it gets reported marked as *.

## Throughput
rate at which bits are transmited from the source to the destination.
- Instentaneous: rate at a given point in time
- Average: rate over longer period of time.

# Rooter modes
### User mode 
``` 
Router >
```
Allows to configure some basic of the rooters (small simple request) to see what configuration the rooter is in. From that mode we can reach the priviledge mode using the command:

### User priviledge mode
```
Router > enable
Router #
```
You can still not change the configuration but you can see more information about the configuration. In order to do that you must go to the general configuration mode

### General configuration mode
```
 Router # configure terminal
 Router(config)#
```
We can know change the configuration of the rooter

### Interface configuraton mode
```
Router(config)# interface g%
Router(config-if) #
```
Allows here to assign new IP addresses

# 16/01/19
## PAN (personnal area network)
Range of 1 m done through bluetooth or BAN (Body area network) used in the medical field for monitering passients through signaling probs on the network.

## Local area network (LAN)
range from 10 to 1000 m accessible through wifi, ethernet cable. It is the technology used at home/office/factory.

## Metropolitan area network (MAN)
Range of 10 km covers a city through the TV cable networks.

## Wide area network (WAN)
100 km to 1000 km, company with multiple branches, transissions lines are usually provided by the ISP infra structures.

## World
connected through a cluster of WAN and LAN.

# Physical layer deeper view
Moves individual bits from one node to the next it defines the relationship between the device and the transmission protocol such as frequency.

## Transmission media
### Twisted pairs
Info is transmitted by varying voltage or current, it is the most commun transmission media made of 2 insulated cupper wire twisted so that the waves of different wires cancel out usually 8 per cable (8 wire). This system can work over several K withouth being amplified. 

**Wiring standards** differs in the way each cables are pluged in to physical devices:
- T568A 
- T568B with RJ45 (most commun)

To connect 2 divices that are the same type switch to switch, router to router then we would use the cross over cable cable source location on the box is different from its destination arrival location on the box. If the divices are not the same swith to router, router to PC and so on then we use straight through cable, **exception comes when connecting a PC to a router this is cross over**

### Coaxial cable
Info transmited through voltage, in this case we have multiple layer covering the wire giving a high band width with low noise, used in the passed for long telephone lines

### Fiber cable
Hire bandwidth again usuing transmission with light, used for long-haul transimission in netwok backbones but are more and more used locally now

### Radio waves
Can travel for long distances but subject to interference. Properties depend highly on the frequency long waves fall sharply and short waves travel in strainglines.

### Micro waves
Travel in straight lines since they have a higher frequency, used for long distance communication but cannot penetrate buildings.

## Router consol
```
running configuration: config that the rooter is running on but if we restart the rooter
then only the start up configuration will work on the rooter

hostname blabal sets the name of the rooter .

? used to get info on possible values.

enable password blabla sets a password but allow other people to see it.
```

# 21/01/19
# Data Link Layer
Responsible for moving data from ne node to the next node through services:

## Framing service
Transmission between two end devices implies encapsulation and decapsulation procedures.  
Framing refers to the perocess of forming data link frames out of network layer packet. We generally add a header and a trailer at this stage to the packet

### Defining the packet data
If we have a packet of 2000 bytes but the frame can only take 1500 byte of data then we will have to split the packet in two and then end device will be responsible for unifying the packets.  
Another method is to have a flag the delimits where the limit of the packet in the frame is.

## Error detection and connection
These could happen because of noise or signal attenuation.

If we have an even number of 1 we set a parity check bit to 1 else to 0 so when we recieve the data we can check wether that data is correct according to the parity check bit. Better way would be to devide the data into matrices. The objective is just to show how errors can be detected.

## Flow Control
Ensure that the pace between two communicating nodes are similar to prevent a slow receiving node to be overwhelmed by a fast sending node.

## Multiple access protocol
### Types of data link channels
**Point-to-point channels**: Single sender at the end of the channel and only one receiver. Two routers connected by a long distance link, computer and Ethernet swithc

**Broadcast channel**: Multiple sending and receiveing nodes connected to the same shared broadcast channel. Hybrid fiber coaxial cable (HFC), multiple host in wireless LAN, satellite networks. The issue arises when we have to coordinate multiple access.

### Partition MAC Protocols TDMA (Time division multiple access)
- Time is divided into time slots
- Each slot is assigned to one node
- For channel with transmission rate of R bps each node gets R/N bps where N is the number of divices comunicating with it.
  
This protocol eliminates collisions and is perfectly fair but a node is limited to a fix rate even if it is the only node transmiting at a given point this is what is used in a 2G cellular network (voice communication) 3G (add a internet/data layer).

### Partition MAC Protocols FDMA (Frequency division multiple access)
- Frequency band is divided into slots with rate of R/N each
  - Apparently this is due to the fact that each sub frequency in our range carries some set of information so when we split it in parts we loose some information (transmission speed).
- Each frequency is assigned to one node.

Also used in 2G networks.

### Partition MAC Protocols CDMA (Code division multiple access)
- Each node is assigned a code to send data (like a flag)
- Different node can transmit simultaneously

Used in 3G network.

### Random access protocol PureALOHA
Node transmit at full channel data R. When collision occurs each node repeatedly retransmits its frame with a probability of p else the node waits before resending the frame, until its frame is sent without collision.

### Carrier Sence Multiple Access (CSMA)
- Listen before transmiting
- Channel sensed idl transmit
- Channel sensed busy the frame is sent later

#### Collision detection/CD used in ethernet (not used anymore)
- Carrier Sense: channel sensing to detect transimisions
- Collision detection: Detect collision and enter a back-up mode
  - Compare transmiting and receiving signall

**Binary back off mechanism**: at the m^th (m is calculated from the perspective of the frame) collision the node choose a waiting time from {0,..,2^m-1}=k then k*512 bit times, then start sending again without listening again.

512 bit times is related to early ethernet network where there was one wire and all node linked to that same cable and the max time to detect collision between the node furthest to transmiting node was 512.

#### Collision avoidance/CA used in wifi 

### Taking-Turns protocols - Polling
One master node control transimission node can transmit in a Round-Robin fashion

### Taking-Turns protocols-Token passing
A token is passed between nodes.

# 23/01/19
## Ethernet
Mostly used in LAN operating on data link and physical layer suporting a band with of 10 mbps to 100 Gbps requires switch for multiple access since 2000 (no more MAC protocol in Ethernet). Most computer include an Ehternet card.

The switch recieves all the information from all the selected divices and knows exactly from which port it should send/output the frame it recieved.

### Organization of the frame in ethernet
- Ethertype
- Data
- SRC (address)
- Preamble
- FCS (Frame check sequence)
- DST (address)

Frame: | header | payload/data | tailer |

| | preamble | destination Address | Source Address | EtherType | Data | FCS|
|--|--|--|--|--|--|--|
|Byte| 8 | 6 | 6 | 2 | 0-1500 | 4 |
Header: preamble, dest address, sources address, ethertype

#### Physical/MAC address break down
XX:XX:XX OUI/manufacturer info then Serial # XX:XX:XX

Such MAC: FF:FF:FF:FF:FF:FF means send the message to everyone in the LAN.

#### EtherType
Values equal or bellow 1500 indicate the size of payload in octet. Valuse greater or equal to 1536 indicates it is used as a type telling the reciever which running network-layer protocol to hand the frame to.

#### FCS
Error detection code

### LAN switch 
Only examine link-layer headers, a switch operation are transparent to end devices and routers. 

A switch contain a MAC address table to map MAC address to ports.

MAC address is in the table, switch forwards frame out of that interface.

MAC address not int the datable switch broadcasts the frame out of all interfaces and then learns from the response the mapping of address/interface
| MAC address | interface/port | Time |
|---|---|---|

## ARP address resolution protocol
Translate between IP address from Network Layer and link-layer address MAC on teh same LAN. On each host and router there is an ARP module that takes an IP address on the same LAN as inout and return its MAC address. 

If we have no entry in the ARP table, we broadcast and ARP message saying that it is looking for the MAC address of the device with IP address ..., the device that has this IP will then send back its MAC address.

**Host means end divices** and **Intermedary device means routers and switches**.



## Difference between MAC and IP address
LAN uses the MAC address of the device to know how to reach it. IP addresses are needed when we move outside of our LAN.



IP is used as a home address (can change overtime) and the MAC is more like the social insurance number (does not change).

# 28/01/19
All operations in LAN are transparent to switches. The MAC and IP address of the switch are not included in the ARP tables.

## Why MAC and IP address
- MAC (physical address) comes from the company that makes the product, used to move frames in local network.
- IP (logicaal address) is used to move packets accross remote networks.

LAN as a reminder is as I understood so far the set of devices that can be access directly from the rooter. A broad cast message never cross the boundary of a router (broad cast message means the mac address FF:FF:FF:FF:FF:FF is used to send it to everybody).

## Sending message accross a router
```
PC0 sends a message to PC1 that is in another lan.  
At LAN0:  
    SRC IP : PC0 IP  
    SRC MAC: PC0 MAC  
    DST IP: PC1 IP
    DST MAC: R0 MAC0

At LAN1:
    SRC IP : PC0 IP  
    SRC MAC: R0 MAC1  
    DST IP: PC1 IP
    DST MAC: PC1 MAC
```

As we can see the MAC addresses changed according to the LAN they are in (The router for example has different MAC addresses depending on the LAN that is actually the MAC address related to one of its ports or interface)

# Network Layer
Router can understand that layer (layer 3). Segments are encapsulated into packets and the reverse when receiving the packets.

It is the job of the network layer to genrate the path that needs to be taken to send the packet.

## IP protocol IPv4
Take the segment and add a header to it.
IPV4 is in 32 bits  
- version 4 bits 0100 for v4
- headerLength 4 bits
- Type of service 8 bits
  - The priority of the packets
- Datagram length 16 bits
  - Can be 65535 bytes of data
- 16 bit identifier
- flags 3 bits
- 13 bit fragmentation offset
 - time to live (TLL) 8 bits (default 64 usually)
   - Decrement by 1 each time it passes a router when at 0 the packet drops
 - Uperlayer protocol 8 bits
   - such as TCP
 - Header Checksum 16 bits
   - Detecting bits error in headers
 - SRC address 32 bits
 - DST address 32 bits
 - Options (if any)
 - Data

## IP v4 address
32 bits/4 bytes long with:
A network portion of 24 bits and a Host postion of 8 bits

### Subnet mask SM
There is also a subnet mask, its goal is to distingish the network portion from the host portion. **The shorter version of the subnet mask is /24 where 24 represents the number of ones.**

So the IP address is 192.168.18.57/24 the subnet mask is used in this case to know which area of the IP represents the Network.

# 30/01/19

## IP v4 address classes
|Class | Range | start with| Network and host part| default subnet mask|Num net|num host|
|--|--|--|--|--|--|--|
|A|1-127**|0|N.H.H.H|255.0.0.0 \8|2^7|2^24-2|
|B|128-191| 10|N.N.H.H|255.255.0.0 \16|2^14|2^16-2|
|C|192-223| 110 | N.N.N.H | 255.255.255.0 \24|2^21|2^8-2|
|D|224-239| 1110| NA | NA|
|E|240-255| 1111 |NA | NA|

Class D is used for one node to send a message to mutliple node in a network (mutli cast) but this is not used that much today.

Class E were reserved to do experimentation so we do not find them in typical networks.

### Network address and boradcast address
class A adress 5.0.0.0/8  
netowork address all 0 in host + network portion 5.0.0.0  
Broadcast address network portion + all 1 in host 5.255.255.255.

### Subnetworks system
100.4.5.1/8 In this case then we cannot have
100.0.0.0 or 100.255.255.255 as host these are reserved

100.255.255.255 is the broadcast address used to say that you want to send the frame to all host in the system, broadcast never cross the boundary of a router.

100.0.0.0 used to identify the network itself, if I want to know if a specific address is in the network itself (network IP address).

### Private vs public IP address
public: IP addresses where information can be routed accross the internet to allow other host to reach it accross the internet.

|Class|private format|
|----|---|
|A|10|
|B|172.16.0.0/16 to 172.31.0.0/16 and 172.16.0.0/12|
|C|192.168.0.0/24 to 192.168.255.0/24 and 192.168.0.0/16|

### Other IP addresses
- Loop back address 127.0.0.0/8 test that the IP stack on the computer is working verify that the machine we are in is operational.
- Link-local address ot automatic private IP addressing APIPA. DHCP.
  - 169.254.0.0/16 to 169.254.255.254

## Subnetting
Forming smaller subnets that fit into the need of the network.  
Large broadcast domains slow network and devices operations due to broadcast traffic.

So the solution is to divide the network address into multiple sub networks it uses a subnet mask to identify how many bits are reserved for the subnetwork.

Consider class C IP address 192.168.5.0/24 with only 10 users in the network we must go to a power above 2^4 so 16-2 = 14 so 4 bits is good.  
So we divide the host portion in two different parts x,y x bits on the left are the ones that identify the subnet and y that I am using to identify the hosts. So we can then have the 192.168.5.zzzz.zzzz So we are going to be able to form 16 different subnets from that specific IP address.  
Here are the possible subnets address:  
192.168.5.0 with a host range of 192.168.5.1-192.168.5.14  
192.168.5.1.0
192.168.5.1.0

In this case the subnet mask with be 255.255.255.240  
The broadcast address would be 192.168.5.15.

So the subnet mask can be identified as /28

# 04/02/19
## Exercise
We have a subnet of size 2 with IP: 172.17.0.0
We have other subnets of size 512 with IP 172.16.0.0

Notice first that this is a type B private IP address so the 172.16 is the network, now we have the possibility to divide the host portion into bits for the subnets and the host left.

#### FOR 512
So for 512 we will have to use 10 bits for the host since we must account for the network and broadcast address
172.16.xxxxxxhh.hhhhhhhh  
**1st subnet:**  
subnetIP@ = 172.16.0.0  
subnetMask: 255.255.252.0  or /22  
broadcastIP@: 172.16.3.255

**2nd subnet:**  
subnetIP@ = 172.16.4.0  
subnetMask: 255.255.252.0  or /22  
broadcastIP@: 172.16.7.255

**3rd subntet:**  
subnetIP@ = 172.16.8.0  
subnetMask: 255.255.252.0 /22  
broadcastIP@: 172.16.11.255


#### Now looking at 172.17.0.0 we need 3 bits   
**1st subnet**  
subnetIP@ = 172.17.0.0  
subnetMask = 255.255.255.252  /30  
broadcastIP@ = 172.17.0.3  

**2nd subnet**  
subnetIP@ = 172.17.0.4  
subnetMask = 255.255.255.252 /30  
broadcastIP@ = 172.17.0.7 

**Reserve the fist IP address avail for the rooter**

## Important each interface has its own IP address on routers for sure not sure about the rest


## Dynamic host Configuration protocol DHCP (Operates at the application layer)
Allows to dynamicaly assign or lease IPv4 addressfrom a pool of addresses (instead of manually going through every user). The level is normal as it has to comunicate with your devices terminal.

The way we can do that is too add a DHCP server in each one of the subnet which may be a lot if we have a lot of subnet and expensive as we are paying for 3 different servers. Today we can include just one server in the network.  Last possibility is to not even use a server and let the routers do it for us.

### One server for the entire network
DHCP server recieves a request to get an IP address and replies back the IP address required.

The server has a pool of IP addresses that the server can provide to subnets. 

The default get way is the port that connects the router to the subnet.

1. Device sends a broadcast message through the linked layer
2. Recieve an answer from the server

#### When the subnet is not directly connected to the server
1. a relay will redirect the broadcast
2. A relay is configured on local routers interface and points to the server

# 06/02/19

# Routing
Routing protocols are the protocols used when it comes to decide for the root that a given packet may trace.

It can be done as **static** routing where each router is thought manually to go through a specific path when sending a packet to the network part x. 

It can be done as **dynamic** routing that adapts to the changes that can occur in the network.

## Dynamic routing
Devided into 2 categories:
1. Interior Gate Way Protocols (IGP)
   - Used inside a autonomous system (university Network) to handle connectivity inside the network.   
2. Exterior Gate Way Protocols (BGP)
   -  Used to communicate between autonomous systems. 

## Static routing
Two kinds of routes:
1. Default Route: taken when all else fails used to reach the 
internet from a network
2. Used when summarized route to the network is known.

Static routes are used because:
- Stub network
  - If we are in a company network that connects to the ISP netowork through only one port then we can just route every outside packet through the default port.
  - The same thing apply if a network can only be reached through one router then we can also attach static route
- Lower overhead than dynamic routing (no need to extend messages between router to make decision) every thing is pre configured
- Design requierement: easier to design the network system
- Security requierments: no routes outside of the interior routes of the network can be taken.
- Useful when the network is made of only a few routers.


### Routing table
In order to know where packets must be pushed when it comes to reaching specific destination, each router holds a routing table to determine where pac\kets will be directed.

|network IP | Hops | Next-Hop IP | exit interface (of the current rounter)

Every device must have a default gate way IP set to it so it knows its router DHCP protocol assigns that automatically if used to initialize the device.

### How to configure it:
```
Router(config) # ip route de network address subnet-mask
ip add of the next hope | or MAC or exit interface

use all 0.0.0.0.0.0.0.0 for default gate way.

Router(config) # show ip route S (for static)
Router(config) # ping IP
Router(config) # show running-config
```
The port of exit only work from port to port direct connections.
Direct connection of ports are directly including into the routing table.

# 11/02/19
# Dynamic routing
Autonomous systems is a group of routers under the control of one authroity. IGP protocol is for the interior of these autonomous systems compare to BGP that is the protocol used for the outside.  
These protocoles dnyamically share info between routers and automatically update the routing table when the network structure changes, it also determines the best path to destination.

**Administrative distance**: if multiple paths to a destination are configures on a router, the path in the routing table is the one with the lowest admistrative distance (AD) (look at the table that tells us all about this distance)

**Metric**: a value used by a routing protocol to detrermine which routes are better than others can include one or more of these parameters:
- Hop count
- Bandwidth
- Delay
- Load
- Reliability
- Cost

## IGP

### Distance vector protocol (RIP: routing information protocol)
Routes are advertised as vector of distance and direction. 
Each router keeps on sharing its complete routing table with other routers. It cannot handle subnets.

### Link State protocol (OSPF, ISIS)
- State of link (interfaces) are at the core of the operations.
- Routers see the entire network structure.
- Updates are triggered by events.

Protocol:
1. Each router sends and LSP packet or (link state packet) containing the state of each directly connected link (interface) + discovered neigh: ID, Link type, bandwidth
2. LSP flood the area to all neigh routers
3. Router stores all information received and when convergeance occur no more traffic.

LSP are sent when intial router starts up, or when there is a change in topology.

#### Open Shortest Path First (OSPF)
$$metric = \frac{10^8}{Bandwidth}$$

# 20/02/19
## EIGRP (Enhance interior gate way protocol) is a hybrid between the two protocols
It has an administrative distance of 90. It only change partial updates between routers when there is a topology change unlike RIP that sends the entire routing table. Here we are going to use bandwidth and delay for metric calculation. Fast convergence as well.

### EIGRP hello packets
used to learn about the neighbours, hello packets are sent periodically (every 5 seconds). If we do not receive back hello packets from a router we can consider it down (default is 3 time the hello interval). A neighbour is identified as such if the autonomous system number and password in hello packets. 

Yes since we want to avoid security flaws thus when a neighbour sends hello packets we want to make sure that such packet indeed comes from a valid router in our autonomous system (group of routers under the control of one entity).

### EIGRP updated packets
Used to propagate routing information and only sends what changed in the topology.

- Query packet used for search networks
- Reply pacekt in response to query packet
- Acknowledgment used to acknowledge the receipt of update, querty & reply packet.

### EIGRP tables
#### Neighbor table
Contains a list of all EIGRP neighbours directly connected to it

#### Topology table
Contain all possible path to reach one specific network. 
FD is the feasible distance (cost that the router will pay to send the message). AD cost that the neighbour router will pay to reach the distination.
|Network|FD|AD|

#### Routing table
Include the best path to all possible destination

### Metric
$$ metric = (\frac{10^7}{lowestBand} + \frac{\sum delay}{10})*256$$

Every network type has its delays: serial 20.000, fast thernet 100, gigabit ethernet 10. So every router/link on the path we sum that up.

### EIGRP commands
```
router (congig) # no-eigrp 1
router (config) # router eigrp aut-sys i.e 1
router (config-router) # 
```
Then we can tell the router which network it will share information with.
```
R1 (config-router) # network 172.16.0.0 
```
Some network might as for a wild card mask afer the IP address 
/24 = 255.255.255.0 then minus that with all 255 so we get 0.0.0.255

To prevent the protocol to run on class full subnet you must run
```
no-auto summary
```

If we have a default rout configured in a router, to redistribute static command 
```
R2(config)# redistribute satic
```
**If a router has access to another autonomous system statically we can share that information with other routers in the system with the command above.**

The router identifier is the smallest IP address that the router has based on all its assigned interface.

# 25/02/2019
# IPv6
This addresses looks like a MAC address with 2^128 different possible combination looking like:  
xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx

We split it into 2 parts one is the prefix and the second part is the interface identifier.

### Abrevation of IPv6
1. Remove leading 0 for each quartlet except for 0000 then we have just 0.
2. If we have consecutive pairs :0:0: then we can change that to :: But can only be done once in an IPv6 address. Note that :0:0:0: can also become ::

## Transfer of IPv4 to IPv6 and vice versa
Tunnelling: put the IPv6 packet inside an IPv4 packet to transfer packets accross a network that only uses IPv4.

Dual Stack: Use both on the same network.

Translation: Packets are converted to the different addresses.

## IPv6 header
| Version | trafic class | flow label |  
| Paylcard | Next header | Hop limit |  
| Source IP |  
| Dest IP |
- Version 4
- Trafic class: priority 8
- Flow label:  20
- Payload lenthg 16
- Next header: layer 4 protocol 8
- Hop limit: TTL equivalent 8 

## Type of IPv6 addresses
### Unicast
Communication between individual hosts.  

#### Global unicast IPv6 addresses
Routable over the internet.
1. quartet: beggin with 2001::/16 or 3001::/16
2. quartlet: 2001:0::/32  or 3001:0::/32
3. quartlet used by ISP or 2nd tier 2011:0:0::/32  or 3001:0:0::/32

#### Link local IPv6 address
Not routable, used to communicate between devices within a subnet. (Not routable even inside the same LAN).
Start with FE80::/10 fixed for the first 10 bits and we must have 54 bits set to 0.

These addresses are usually randomely assigned by the OS but can also be configured manually.

Router use such addresses to send information with each other.

#### Unique local IPv6 addresses
Equivalent of private IPv6 used to communicate between devices locally within a subnet.

FD00::/8 Next 40 bits are unique sequence created by the company, Next 16 are the subnets Last 64 bits are for the interfaces ID

# 27/01/19
### Multi cast
Group of nodes that want to communicate between each other.  
The prefix is FF00::/8 the next 4 bits represent the life time and the next 4 bits after that the scope
- lifetime
  - 0 if permanent 1 if temporary
- Scope
  - 1 node
  - 2 link
  - 5 site
  - 8 organization
  - E global

#### Local scope
Prefix FF02:: \16  
- FF02::1 for all nodes (same as IPv4 broadcast)
- FF02::2 for all routers
- FF02::5 for all OSPF router
- FF02::9 for all RIPv2 router
- FF02::A for all EIGRP router

#### Solicited Node (used to find the MAC address)
All hosts that matches the last 4 bits of unicast or anycast address IPv6. Used to find the MAC address of a node
Take the IPv6 address that we want to find the MAC address of last 24 bits put them there then above we put 9 bits above as 1 and make all other bits 0 the prefix of the IP must be FF02 \16


### AnyCast
Used in constent delivery network, for example Neflix rely on that system they could have 3 servers one in Vancouver, Torronto and Montreal but we need the content of only 1 of them this is where anycast comes in.

Assigned to a set of interfaces, a packet sent by such addresses is sent to the closest interface with that anycast address. These addresses are alocated from the unicast address space. Assign a unicast address an anycast address makes it a unicast address (danger danger that feels like an interesting thing for hackers to pretend to be something that they are not)

#### Mscellaneous addresses
- Unkown or unspecified
  - :: address used by host without IPv6 address yet when it suspects its own IPv6 address has a problem
- Loopback IPv6
  - ::1 or 127 binary 0 with last 1
  - Samme as 127.0.0.1 IPv4
  - Used for testing IPv6 protocol stack

## EUI-64 CISCO autogenerated of interface addresses
1. Split MAC address into two parts first 24 bits (OUI) last 24 bits (Device indentifier)
2. Insert FFFE
3. Invert 7th bit

There is the IPv6 address of the device.

## Neighbor Discovery Protocol
The protocol used to discover the MAC address of a neighbor on the same subnet. It also verifies the reachability of neighbors and trak them (what is there current status).

### ICMP (Protocol that the NDP relies on)
Protocol used by ping messages

5 different ICMPv6
- NS: Neighbor solicitation protocol (used by a node that wants to get the link layer (MAC) address of another node on the same local link)
- NA: Neighbor advertisement (used to respond to a message)
- RS: Router solicitation (searching for the router in the network)
- RA: Router advertisment (reply to RS)
  - Passes prefix when transfering message to allow other devices to set up there IPv6 address.
- DAD: Duplicate address detection
  - Host sends a NA with its own unicast address or solicited node address if there is a response then that means that there is someone that uses that same address.

### Dynamic configuration of IPv6
- SLAAC (Stateless Address configuartion)
  1. Listen to RA or send RS
  2. Gather prefix from RA
  3. Hoste generate its own interface address and adds it to the prefix
  4. Send the DAD to check if it genreated a unique IPv6 address
- DHCPv6 protocol + SLAAC
  1. get IPv6 address generated by SLAAC
  2. Get information about DNS server or domain name information comes from DHCP server
- DHCPv6 server
  - Assign addresses and keeps track of them
  - Gets info from DHCP server for anything else.

NDP protocol nedgates the interface

# 11/03/19
Here we are going to focus on how do we connect the correct window per say with the correct packet information (what specific application process is being targeted) if I have my e-mails and a web page on amazon I need to know to which page does my packet segment applies to. 

This is done throug a port #. To recap we have a MAC address that is used at the link layer, IP for network layer and at the transport layer we use the port number.

Multiplexing: handle data comming from multiple proceses at the sender. 

Demultiplexing: deliver the segment to the correct process at the receiver.

## Port numbers
They can use 16 bits, 1 to 1023 ports are reserved for specific application the ones above are free.

## UDP protocol 
Known to be light wait compared to TCP. Does not offer much on top of the IP protocol (when sending something from the source point to the destination point there is no garentee that the packet will be delivered). Similarlly such protocol does not give us that, it does not maintain connection between each processes at  this level. There are no dependencies between UDP packets that are being send between two applications.

- No reliability
- No end to end connection (no connection is maintained)
- No flow control
- No congestion control
- Lightweight protocol

Adds to the header file 16 bits for length in byte of UDP segment including header, 16 bit for checksum for error detection then followed by the application payload.

Used mostly by application that want information to be sent quickly so good for video and mutlimedia, request and reply application DNS/DHCP and finally used by applicationt that handle reliability themselves SNMP, TFTP.

## Quick protocol (new google protocol)
Light wait with security advantages

# 13/03/19
## TCP protocol (Transmission control protocol)
Include flow and congestion control:
- Reliability: 
- Connection oriented: relies on an end to end connection between hosts
- Flow control: prevent a slow node from being overwhelmed
- Congestion handles congestion in the network

### TCP segments
src port 16 bits, dest prot 16 bits  
squence number 32 bits   
acknowledgment number 32 bits (use to acknowledge back the segment read in some versions of TCP)  
head Length 4, unused 6, flags 6 (tell what message is being sent), receiv window 16 (tells how many byte can the receiver receive in one shot from the sender number of seguments based not there size)   
checksum 16, urg data pointer 16 (link to the urgent flag to refer to the urgent payload part)  
options 0 to 32 bits  (usually holds the maximum segment size or length of the application data)
App data

Each communicating host has its max segment size.

#### Flags (can have mutliple flags on the same message)
- URG: urgent flag (not used often)
- **ACK**: declare the use of the acknowledgment field (the receiving host must )
- PSH: the app data should deliver asp (not used often)
- RST: connection restart
- **SYN**: declare the initial value of the sequence number field during the connection establishement phase
- **FIN**: end segment connection used when closing the connection.

### TCP connection establishement (3 way handshake protocol)
1. Choose init seq num x, send TCP SYN msg
2. Choose init seq y, send SYNACK msg, acknowledging TCP syn msg
3. TCP SYNACK msg for x received, the serer is live ACK sent back
4. ACK for y is recieved the client is live.

### TCP connection end (conventional way)
1. Client send FIN flag can no longer receive data
2. Server send ACK bit 
3. Server send FIN bit
4. Client send ACK bit

### TCP connection end
1. Client send FIN flag can no longer receive data
2. Server send FINACK bit 
3. Client send ACK bit

### Sequence # and acknowledgment #
Each sequencence # unicquely identifies each segment. It is incremented when date is sent by the number of transmitted bytes in the app data. These number help keep track of segments order for reassembling and reordering them before transmitting them to te application process at teh receiver side.

The sequence number and acknowledgment number allow to detect missing data segments. 

The initial segment number is usually randomelly choosen.

Aknowledgment number point to the sequence number that the destination point is actually expecting next.

### Retransmission
- Rely on RTT (Round trip time send from src back to src) hestimated as Timout = 2 * round trip. If timout expires before we get a response then we send again.
- Estimated RTT = alpha * Estimatedold + (1-alpha)SampleRtt alpha recomende 0.875 

The receiver nodes and source node must keep track of the last segment it last saw to remember what segment it expects next.

# 18/03/19
## TCP congestion control mechansim
The TCP header has a field called the congestion window that holds the maximum number of segments that can be tranmistted by its host it is updated based on the congestion control mechanism.

#### Def congestion
State of a node that received more data than it can handle.  
This can cause:
- Lost packet
- Long delays
- transmission capacity is wasted
- retransmission is needed

### Slow start
1. Initally the connection window cwnd = 1 MSS (maximum segment size) 
2. Get the acknowledgment back and send 2 MSS this time 
3. Get the acknowledgment back send 3 MSS this time but then we get the second acknolwedgment so send 1 MSS this time and set cwd to 4.

As we can see the MSS increases exponentially over time after each RTT (round trip time). This carries son when ssthresh : slow start threshold is reached or a loss event is reached.

After that the ssthreshold the cwnd increases linearly.

ssthresh first value is arbitrary but when a lost even occurs it is set to 1/2 of cwnd.

### Fast retrasmit
If no fast retransmit is implemented a loss evey is detected by timout. 
- cwnd is set to 1 MSS
- ssthresh is set to 1/2 of cwnd just before lost event
- cwnd then grows expontentially up to ssthresh then grows linearly.

### Fast Recovery
If there is a speciam implementation usually it is:
Instead to wait for timout if we receive 4 ACKs that are the same we consider that as and indication that a segment was lost.
This eliminates half of the TCP timouts and leads to notable increase in throughput.
In this case then the protocol is:
- ssthresh is to 1/2 cwnd just before loss event (where the packet missing was sent).
- cwnd is set to ssthreash+3
- cwnd then grows linearly

## Different TCP version (just for our knowledge)
- TCP Tahoe: Slow start, congestion avoidance, fast retransmit
- TCP Reno: all thinga above

# 20/03/19
# Application layer protocol
## DNS protocol
IP addresses are not easy to remember for humans so domain names make server addresses easier to rember. But the computer still needs the IP address to reach the server. The DNS protocl allows the dynamic transaltion of a hostname to the ip address we use the port **53** to receive DNS query.

The DNS relies on the **UDP** transport protocol.

### DNS system
There is a hiarchy bettween DNS servers.
1. Root DNS servers provide IP of TLD server
2. Top level domain or TLD server (.com,.org,.fr,.sg,.in) since these are the highest domains that we have, provide IP to reach authoritative DNS server
3. Authoritative DNS server (xwv.ca) provide IP of a hostname

#### Local DNS server
Each ISP (residential, company, university) has one when a host in such network makes DNS query the query is set to its local DNS server as it has recent cache of name to address recently access. Note that even local host also cache the IP address of the IP of domain names.

The query can be recursive following the regular hiarchy of the tree and the response back

The query can be iterated where in this case the query sent to each server comes back to the local DNS server that then itself reaches the next DNS server in the tree level.

### Operation of DNS
It uses caching to increase the speed of address translation. The DNS data is stored in the database in the form of resource record RR. 

RR is maked of 4 tubples:  
domain name, time to leave field, type, value

#### Type
- A the name is a host name and value its ip address  AAAA is for Ipv6
- NS the name is a domain name and value is the canonical name of its authoritative DNS server.
- CNAM the name is an alias for a host and value is the cononical name of the host
- MX the names is an alias for an email host and value is the canonical name for the email server.

The TTL is used to indecate when an RR can be removed from the DNS cache

# 25/03/19
# WebBrowsing
A webpage consits of a group of objests with a bast HTML-file obehst and several referenced object. Each object is addresseable by a URL.

## Hypertext Transfer Protocol
HTTP is implemented in two programs:
- Client: browser that display the page and displays it
- Server: server that sends the http protocol objects in response to the request.

Http uses TCP:
1. Client initiates TCP connection to server on port 80 (HTTP runs on top of TCP)
2. Server accepts the TCP connection
3. HTTP messages are sent between them (application layer protocol)
4. TCP connection closes

### Types of HTTP connection
#### Non persistent connection
1 TCP connection per object that we want.
1. Client initate TCP connection to HTTP server
2. Server accepts connection and notifies the client
3. clients sends HTTP request message to server
4. Server sends HTTP response mesage to client with requests objects
5. HTTP server closes the TCP connection

#### Persistent connection
1 TCP connection for all objects.

Server usually closes a TCP connection after it does not receive a message from a client for a certain amount of time.

## HTTP request messages
The first line is the method followed by the path we want to apply the command on or object, then we have \r \n to indicate the start of the header lines which inlude information about the browser then we have a \r \n that indicate the end of the header

method | URL | http version | \r \n
header
|header field name | value | \r \n
\r \n

Here are the main methods for the HTTP request messages:
1. GET (request objects from server)
2. HEAD (same as get but here we request just the header of the response message)
3. POST (used to send data to the server in the message)
4. PUT (used to uplead an object to a specific path)
5. DELETE (used to delete objects on the webserver)

## HTTP response message
status line | \r \n
header \r \n
\r \n
data of requested object.

### Status code
- 200 OK
- 301 moved permanently (object was on server but permantently move)
- 400 bad request (message not understood)
- 404 Not Found
- 505 HTTP version not supported

## Web caching
Its purpose is to save certain data in proxy servers. 

Clients when accessing servers may go through a proxy server before hand. If the proxy server has the content asked by the client then the proxy server sends the response directly else then we have a cache miss and the proxy server itself redirects the request to the correct server.

### GET with cache
How de we know that the cache is up to date.
This is called a conditional get

We have a conditional get so it sends a message to the server specifically to tell it that it cached the object at date x. If the object has not been modified since that date the server replies that to the proxy, else the server will reply to the proxy with the fresh copy.

# 27/03/19
# Remote access
## Telnet
Relies on TCP protocol using port 23. Telnet is light and easy to configure the only issue is that it does not encrypt messages between the sender and receiver. So if we send a password over the internet a person snifing the line can read the password.

```
Router(config) # enable seceet donttell
Router(config) # enable password Cisco
Router(config) # line vty 04
Router(config-line) # password cisco
Router(config-line) # login
Router(config-line) # transport input telnet
```
## SSH
Uses TCP on port 22, this is more secured than tel net but relies on more configuration.
1. Hostname ust be changed (not default) and enable password must be configured
2. Select version 2 of SSH
3. Domain name must be entered
4. Username and password must be defined
5. Ecription configured > 768
6. Uder vty entries:
   1. Login must now be local
   2. Transport is configured to ssh


# E-mail
Can be viewed as a store and forward method of sending, storing, retrieving electronic messages. Email messages are stored on server which the client must use to send and retrieve e-mailes mail servers communicated with each other to transport one e-mail from another.

All the protocol relies on TCP

## SMTP
Send from host to the mail server of the client and from server to server. 

Relies on port




## POP/IMAP
Use to transfer an e-mail from server to client  
Port: 25

### POP
Download the e-mail from the server to the client  
Port: 110

### IMAP
The client sees the messages comming from the server  
Port 143

# 01/03/19
# Sockets
A client server program is written by an application developper and includes client and server programs. So when the application is executed it results in a client and server process. 

Sockets provide the interface between the application layer and the transport layer.

A socket is a combination of IP and a Port number for example 192.168.1.5:1099 .

There a two types of sockets that we can create UDP or TCP socket. 

### Application Example:
1. client reads a line of characters from its keyboard and sends data to server.
2. Server receives the data and converts character to upper case
3. Server sends modified data to client
4. Client receives modified data and displays line on its screen.

### UDP socket
```python
from socket import*
def server():
  severName = 'hostName'
  serverSocket = socket(AF_INET,SOCK_DGRAM)

def client():
  clientSocket = socket(AF_INET, SOCK_DGRAM)
```
The port number of clients is usually done by the OS automatically what it needs to know is just the server port number.

### TCP connection server client
The server has a welcome socket as well to retrieve messages asking to connect the the server after that it will go onto the regular socket.

TCP protocol requires the connection of the client with the server which means that we ask for connection first then we send messages we do this only once.

#### Running client before the server


# File Transport Protocol
FTP requires two connections between the client and the server one for commands and replies and another connection for the actual file transfer.

The first connection is established on port 21, then we use port 20 for the second connection to send data. The client can then download or pull data from the server.

# 03/03/19
# Wirless lan
### Infrastructure Mode:
Access points with wireless hosts. Today access points are quite advanced, today wireless router acts as switch and router.

### Adhoc
We do not have an infrastructure that connects things together here we have devices that communicate with each other directly. In these kind of networks we have bluetooth and futur car communication systems.

## Protocols
The protocol type changes based on the distance that we want to cover, there are also multiple potential protocols for the same distance.

### Wireless LAN - IEEE 
| | 802.11b | 802.11a | 802.11g | 802.11n | 802.11ac
|---|---|---|---|---|---|
|freq GHz| 2.4 | 5 | 2.4 | 2.4,5| 5|
|Max data Mbps | 11 | 54 | 54 | 450 | 1300 |

#### 2.4 Ghz
Divided into more channels here 14. There are some overlaps between each channels however some channels can interfere with up to 5 other channels.

This is why when we set access points we need to make sure that we configure them with channels that do not interfere with each other.

#### 5 Ghz
We have more channels than the 2.4

### CSMA/CA
Here we cannot dedtect the collision so we use collistion avoidance.

#### 802.11 LAN associtation

Passive scanning
1. AP sends beacon frames containing its name (SSID) and MAC address.
2. Host sends associaton request to AP (Including authentication)
3. AP sends association response.

Active Scaning:
1. Host sends probe request frame in broadcast for known SSID
2. AP send probe response frame
3. Host sends association request to the selected AP
4. AP sends association request.

## Terminology
- SSID Unique identifier of a wireless network
- Password: required for the wireless client to authenticate to the AP
- Network mode: Referes to the 802.11xx standards
- Channel settings: Refer to the frequency bahds used to transmit wireless data.
- Security mode: WEP, WPA, WPA2

### CSMA / CA
Signal strength atenuates with distance thus it is hard to detect collisions.

1. First listen and see if the channel is free
2. Wait for a random period before sending
3. If the channel is still not busy send the message

#### Hidden terminal problem
Host A and B cannot here each other but there signals can still collide at the wireless router.

This is where the request to send protocl quicks in RTS and Clear to send CTS handshake allow to handle that issue. 

1. When a host A realizes that the medium is free it sends an RTS(A) message to the AP (Wireless router). 
2. The AP sends a CTS(A) message to everybody in the network
3. Then A sends the data
4. When the data is received the AP sends an ACK(A) to acknowledge that it received all the data from A and is now ready to process more data.

### Frame 802.11
2 frame  
2 duration  
6 receiver MAC  
6 sender MAC  
6 Address of router interface to which AP is attached MAC  
2 seq control   
6 a 4th address only used in ADOC mode.
0-2312 payload  
4 CRC

Will there be miss information?
Explain better the concept of hidden network will a device be able to see both wireless lan?

# 08/04/19
Final exam 40 multiple choice questions. 5 exercises that we work on.

# 10/04/19
Reach a HTTP server that we do not know the IP to.
First use DNS application layer protocol to convert "human" address to IP address thus uses UDP protoctol as it is a simple request protocol at the transport layer. The use HTTP protocol which uses TCP

