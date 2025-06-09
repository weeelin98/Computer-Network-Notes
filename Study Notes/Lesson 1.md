# Study Questions:

### What are the advantages and disadvantages of a layered architecture?
- Advantages of a Layered Architecture
	- ==scalability, modularity, and the flexibility== to add or delete components which makes it easier overall for cost-effective implementations1.
- Disadvantages of a Layered Architecture
	- Some layers functionality depends on the information from other layers, which can ==violate the goal of layer separation==
	- One layer may ==duplicate lower layer functionalities==
	- There is some additional overhead that is caused by the abstraction between layers

### What are the differences and similarities between the OSI model and the five-layered Internet model?
- Similarities:
	- Both of them follow a layered model
	- In both frameworks, every layer implements some functionality
	- Every layer works ==based on the service provided by the layer below it==, and also ==provides some service to the layer above it== 
	- Separating the functionalities into layers offers multiple advantages in both models
- Differences:
	- The International Organization for Standardization (ISO) proposed the seven-layered OSI model.
	- The Internet architecture model has five layers
	- In the Internet architecture model, the application, presentation, and session layers from the OSI model are combined into a single layer, called the application layer


### What are sockets?
- The ==interface between the application layer and the transport layer== are the sockets.
- Sockets are used by the transport layer to solve the problem of multiple applications running on the same host and sharing the same IP address
- The transport layer uses sockets and additional identifiers called ==ports==
-  Each application on a host binds itself to a unique port number by opening sockets and listening for data from a remote application
### Describe each layer of the OSI model.
- Application Layer: This is the top layer1. It includes multiple protocols. The specific services offered depend on the application that is implemented

- Presentation Layer: This layer plays an intermediate role of formatting the information that it receives from the layer below and delivering it to the application layer. ==Service==: Convert data between different representations. ==Protocol==:Define data formats; Apply transformation rules 

- Session Layer: The session layer is responsible for the mechanism that manages the different transport streams that belong to the same session between end-user application processes. ==Service==: Access Management; Synchronization. ==Protocol==: Token management; Insert checkpoints

- Transport Layer: This layer is responsible for the end-to-end communication between end hosts. The two main transport protocols mentioned are TCP and UDP.  At the transport layer, the packet of information is referred to as a ==segment==. The transport layer uses ==sockets and ports== to handle multiplexing and demultiplexing, enabling multiple applications on the ==same host to share the same IP address==.  ==Service==:Multiplexing/Demultiplexing; Congestion control;  Reliable, in order delivery. ==Protocol==: Port numbers; Reliability/ Error correction; Flow-control information. ==Example==: UDP, TCP

-  Network Layer: In this layer, the packet of information is referred to as a ==datagram==. A source Internet host sends the segment along with the destination address, ==from transport layer to the network layer==. **The network layer is responsible for moving datagrams from one Internet host to another.**  Protocols include the IP Protocol, which defines ==the fields in the datagram and how hosts/routers use them,== so the datagram that a source internet host sends reach their destination. The routing protocols that determine the routes that the datagram can take between sources and destinations. ==Service==: Deliver packets across the network; handle fragmentation/reassembly; Buffer management. ==Interface==: Send one packet to a specific destination. ==Protocol==: Define globally unique addresses; Maintain routing tables. ==Example==: Internet Protocol(IP); IPv6

- Data Link Layer:  In this layer, packets of information are referred to as frames. The data link layer is responsible to move the frames from one node (host or router) to the next node. It receives a datagram from the network layer at a node and delivers it to the next node, where it passes the datagram up to the network layer.==Service:== Data framing; /media access control(MAC); Per-hop reliability and flow control; ==Interface:== Send one packet between two hosts connected to the same media. ==Protocol==: Physical addressing(e.g. MAC addressing)====Examples== :Ethernet, PPP, and WiFi DOCSIS

-  Physical layer: This is the bottom layer and is responsible for facilitating the interaction with the actual hardware. Its job is to transfer bits within a frame between two nodes that are connected through a physical link. The protocols in this layer depend on the specific link and transmission medium. ==Service==: Move information between two systems connected by a physical link. ==Interface==: Specifies how to send one bit.  ==Protocol==: Encoding scheme for one bit; Voltage levels; Timing of signals. ==Examples==: coaxial cables; fiber optics; radio frequency transmitters.

### Provide examples of popular protocols at each layer of the five-layered Internet model.
  
- Application Layer: This is the top layer and includes multiple protocols that provide specific services depending on the implemented application4. In the Internet architecture model, this layer combines the functionality of the application, presentation, and session layers from the OSI model1. Popular protocols found at this layer include: 
	- HTTP (Hypertext Transfer Protocol), used for the web
	- SMTP (Simple Mail Transfer Protocol), used for e-mail
	- FTP (File Transfer Protocol), used for transferring files between two end hosts45.
	- DNS (Domain Name System), used for translating domain names to IP addresses4....

- Transport Layer: This layer is responsible for end-to-end communication between end hosts910. It receives messages from the application layer, appends header information, and passes the resulting segment to the network layer. The two main transport protocols are:
	- TCP (Transmission Control Protocol), which offers a connection-oriented service, guaranteed delivery of application-layer messages, flow control, and congestion control9.
	- UDP (User Datagram Protocol), which provides a connectionless best-effort service without reliability, flow, or congestion control913.

- Network Layer: This layer is responsible for moving datagrams from one Internet host to another9. It receives segments from the transport layer with a destination address and delivers the datagram to the transport layer at the destination host914. The main protocols in this layer are:
	- IP Protocol (Internet Protocol), often referred to as "the glue" that binds the Internet together14. It defines the fields in the datagram and how hosts and routers use them14. IP provides a best-effort delivery service model12.
	- Routing protocols, which determine the routes that datagrams take between sources and destinations14. Examples of Interior Gateway Protocols (IGPs) that operate within an Autonomous System (AS) include OSPF (Open Shortest Path First), IS-IS, RIP (Routing Information Protocol), and E-IGRP15. The Border Gateway Protocol (BGP) is used for interdomain routing, exchanging information between ASes.
- Data Link Layer: This layer is responsible for moving frames from one node (host or router) to the next node across a physical link16. Example protocols in this layer include:
	- Ethernet
	- PPP (Point-to-Point Protocol)
	- WiFi (Wireless Fidelity, based on 802.11 standards)

- Physical Layer: This is the bottom layer and facilitates the interaction with the actual hardware, responsible for transferring bits within a frame between two nodes connected through a physical link19. The protocols at this layer depend on the specific link and transmission medium19. The source mentions that Ethernet, a data link layer protocol, has different physical layer protocols for various media like twisted-pair copper wire, coaxial cable, and single-mode fiber optics20.

### What is encapsulation, and how is it used in a layered model?
- Encapsulation is a fundamental process in a layered network architecture, such as the Internet model
- Here is how encapsulation works in the layered model：
	- An application layer message (M) is sent to the transport layer
	- The transport layer receives the message and appends its transport layer header information (Ht)
	- transport-layer segment, and it encapsulates the application layer message
	- The segment is then forwarded to the network layer
	- The network layer receives the segment and adds its own network header information (Hn)
	- The datagram is then passed to the data link layer
	- The data link layer receives the datagram and appends its link layer header information (Hl). This unit is called a frame, and it encapsulates the datagram
	- Finally, the frame is transmitted as bits across the physical medium by the physical layer
	- *At the receiving end, the reverse process, known as de-encapsulation, occurs*

### What is the end-to-end (e2e) principle?
- The e2e principle suggests that specific application-level functions usually cannot, and preferably should not, be built into the lower levels of the system at the core of the network
- In simple terms, the principle is summarized as: the network core should be simple and minimal, while the end systems should carry the intelligence
- The function in question can completely and correctly be implemented only with the knowledge and help of the application standing at the endpoints of the communications system
- providing that function as a feature of the communication system itself is not possible
- any attempt to build features in the network core to support specific applications must be avoided or only viewed as a tradeoff

### What are the examples of a violation of e2e principle?
 - Firewalls, which are intermediate devices at the network periphery that monitor and can drop traffic between end hosts7. 
- Network Address Translation (NAT) boxes7. NAT boxes violate the principle because hosts behind them are not globally addressable or routable, preventing other hosts on the public Internet from initiating connections without the NAT box's intervention
==A data link layer protocol like 802.11 (WiFi) implementing basic error correction ==is ==not a violation==. This is because violations typically refer to functionalities that cannot be implemented entirely at the end hosts
### What is the EvoArch model?
- the EvoArch model provides a quantitative framework to understand how network protocols succeed or fail based on their dependencies on lower layers and the value derived by higher layers, explaining why the Internet's core has become so stable and why innovation tends to flourish at the edges.
- Its primary purpose is to help understand the dynamics of how layered protocol stacks develop and why certain protocols become dominant and difficult to replace, particularly explaining the emergence and stability of the Internet's "hourglass" shape

### Explain a round in the EvoArch model.
Each round of the EvoArch model involves performing a specific sequence of steps
-  Introduce New Nodes
- Examine and Process Layers (Top to Bottom)
- Connect New Nodes
- Update Node Values
- Remove Nodes
- Check Termination Condition
### What are the ramifications of the hourglass shape of the internet?
-  Modification of Technologies for Internet Compatibility
- Difficulty and Slow Pace of Core Protocol Transition

### Repeaters, hubs, bridges, and routers operate on which layers?
- Repeaters and Hubs operate on the physical layer (L1)
- Bridges (and Layer 2 Switches) operate on the data link layer (L2)
- Routers (and Layer 3 Switches) operate on Layer 3, which is the network layer
### What is a bridge, and how does it “learn”?
- bridge: A bridge is a network device with multiple inputs and outputs1. Its primary function is to transfer frames from an input to one or multiple outputs1. Bridges operate on the data link layer (L2)
- how does it learn:When a bridge receives any frame, it uses this as a "learning opportunity" to discover which hosts are reachable through which ports over a frame arrives and the source host
### What is a distributed algorithm?
- a distributed algorithm is one where calculations and processes are not carried out in a single, centralized location. Instead, it involves ==individual nodes== performing ==their own calculations== based on information exchanged with their direct neighbors, and then potentially sharing their updated results back

### Explain the Spanning Tree Algorithm.
- the Spanning Tree Algorithm (STA) is a distributed algorithm used by bridges (and Layer 2 switches) to address the problem of loops in a network topology
- Bridges operate on the data link layer (L2) and forward frames based on MAC addresses
- The goal of the Spanning Tree Algorithm is to eliminate these loops by having the bridges select which links (ports) to use for forwarding
- how the algorithm works
	- Node Identification: Every node (bridge) in the graph has a unique ID
	- Root Selection: The bridges collectively select one bridge to be the root of the topology
	- Rounds and Message Exchange: The algorithm runs in "rounds"
	- Comparing Configurations:
		- The perceived root has a smaller ID
		- If the perceived roots have equal IDs, the message indicates a smaller distance (fewer hops) from the root
		- If both root IDs and distances are the same, the tie is broken by selecting the configuration from the sending node with the smallest ID
	- Updating and Forwarding: Nodes update their view of the network based on the best configuration message received and may resend their updated results
	- Link/Port Selection: A node effectively decides not to use a particular link (port) for forwarding when it receives a configuration message over that link that indicates the sending neighbor is closer to the root, or has the same distance from the root but a smaller ID

### What is the purpose of the Spanning Tree Algorithm?
- The Spanning Tree Algorithm helps to prevent boardcast storms
