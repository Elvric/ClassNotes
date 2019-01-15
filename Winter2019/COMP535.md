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