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
Each router keeps on sharing its complete routing table with other routers.

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

### EIGRP is a hybrid between the two protocols


## EGP
### BGP protocol