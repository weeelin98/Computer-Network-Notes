### What are the basic components of a router 路由器?
- Main job:
	- forwarding plane functions:
		- forwarding plane function is the router's action to transfer a packet from an input link interface to the appropriate output link interface. Usually in hardware.
	- control plane functions:
		- implement routing protocols, maintaining the routing tables, computing the forwarding table. In software in the routing processor. Function could be implemented by a remote controller
- The main components of a router are the **input/output ports** （输入/输出端口）, **the switching fabric**（交换结构）, and **the routing processor**（路由处理器）
	- Input ports: physically terminate the incoming links to the router; data link processing unit decapsulates the packets;  perform the lookup function; consult the forwarding table (or Forwarding Information Base or FIB) to determine the appropriate output port through the switch fabric
	- Switching fabric: moves packets from the input ports to the output ports; makes the connections between the input and the output ports
	- Output ports:  receive and queue the packets which come from the switching fabric; send them over to the outgoing link
	- Routing processor: where the router’s control plane functions are implemented in software; These functions include implementing the routing protocols, maintaining the routing tables, computing the forwarding table; In the SDN approach, these functions could be implemented by a remote controller; Routers also implement various protocols such as SNMP, TCP, UDP, and ICMP in the routing processors
### Explain the forwarding (or switching) function of a router.
-  forwarding plane function is the router's action to transfer a packet from an input link interface to the appropriate output link interface. Usually in hardware.
### The switching fabric moves the packets from input to output ports. What are the functionalities performed by the input and output ports?
- input ports：> 负责**接收、初步处理和决定送到哪里**
	- physically terminate the incoming links to the router
	- data link processing unit decapsulates the packets
	- perform the lookup function
	- consult the forwarding table (or Forwarding Information Base or FIB) to ensure that each packet is forwarded to the appropriate output port through the switch fabric
- output ports：> 负责**排队、调度、以及把包发出去**
	- receive and queue the packets which come from the switching fabric
	-  then send them over to the outgoing link
### What is the purpose of the router’s control plane?
- **implement routing protocols**, **maintaining the routing tables**, **computing the forwarding table**. In software in the routing processor. Function could be implemented by a remote controller
### What tasks occur in a router?
- Lookup: When a packet arrives at the input link, the router looks at the destination IP address and determines the output link by looking at the forwarding table (or Forwarding Information Base or FIB)
- Switching: After lookup, the switching system takes over to transfer the packet from the input link to the output link
- Queuing: After the packet has been switched to a specific output, it will need to be queue (if the link is congested)
- Header validation and checksum: The router checks the packet’s version number, it decrements the time‑to‑live (TTL) field, and also it recalculates the header checksum
- Route processing: The routers build their forwarding tables using routing protocols such as RIP, OSPF, and BGP8. These protocols are implemented in the routing processors
- Protocol Processing: The routers implement various protocols such as SNMP, TCP, UDP, and ICMP in the routing processors to implement their functions
### List and briefly describe each type of switching. Which, if any, can send multiple packets across the fabric in parallel?
- Switching via memory：通过内存交换时，输入端口收到数据包后，会通知路由处理器，并将包复制到主内存中。路由处理器读取包内容、查找目的地，再把包复制到对应的输出端口。这种方式受 CPU 和内存速度限制，效率较低。
- Switching via bus：通过总线交换时，输入端口给包加上“目的端口”的标记，然后发送到一条共享总线上。所有输出端口都能看到这个包，但只有被指定的那个会接收。因为总线一次只能传一个包，所以速度受限。
- Switching via interconnection network:：互联网络（如交叉开关）用多个连接线把输入端口和输出端口配对。不同端口之间的包可以**同时传输**，只要路径不冲突。这种方式速度快、并发高，适合高性能路由器。
- the**Switching via interconnection network (specifically a crossbar switch is mentioned) can send multiple packets across the fabric in parallel**
- while switching via bus allows only one packet at a time. Switching via memory is described as a process involving copying packets to/from processor memory
### What are two fundamental problems involving routers, and what causes these problems?
> 路由器的难题就是两个字：**又快又聪明**——既要能撑得住越来越快的数据流，还要能“有脑子”地提供复杂服务。
- Bandwidth and Internet population scaling：
	- **网络扩展压力大**：上网设备越来越多，应用流量也越来越大（比如视频、游戏）。加上像光纤这种高速技术，要求路由器能处理**更多数据、更快速度**。
- Services at high speeds：
	- **高速下提供复杂服务很难**：路由器不仅要快，还得能提供像**抗延迟、防攻击、服务区分**等高级功能。而要在高速环境下完成这些，挑战非常大。
### What are the bottlenecks that routers face, and why do they occur?
- Longest prefix matching：- Too many hosts and networks make it hard to store exact routes, so routers use prefixes — but matching the longest prefix quickly is computationally complex.- 网络数量太多，不能每个都精确存，只能用前缀匹配；但“找最长匹配”算法很复杂，速度容易变慢。
- Service differentiation：- Routers must treat different packets differently (e.g. video vs. email), requiring complex classification beyond just destination address.- 不同业务需要不同处理（比如视频 vs 邮件），路由器必须分析更多信息（如来源或服务类型），加重处理负担。
- Switching limitations：- Routers use crossbar switching for high speed, but it suffers from issues like head-of-line blocking.- 使用交叉交换提高速度，但容易出现“前头阻塞”（一个端口卡住，后面都堵住）。
- Bottlenecks about services：- Guaranteeing quality of service and supporting new services (like security, measurements) at high speeds is hard.- 在高速下想要提供服务保证、安全功能或数据测量，非常难实现。
### Convert between different prefix notations (dot-decimal, slash, and masking).
 - Dot decimal：- This uses dotted decimal with a * to mean “remaining bits don’t matter.” e.g., 132.234* → Only first two octets matter. - 132.234*（代表前 16 位有效） **含义**：IP 前缀的前两段（132 和 234）是重要的，后面不重要。
 - Slash notation：- This is the most common format. The number after the slash tells how many bits are used for the network prefix.- 132.234.0.0/16 **含义**：斜杠后面的 **16** 表示前 16 个比特（即前两个字节）是有效的，用来做匹配，后面不重要。
 - Masking：- The subnet mask uses 255s and 0s to indicate which bits matter. e.g., 255.255.0.0 → Only first 16 bits are checked.- 132.234.0.0 + 255.255.0.0 - **含义**：掩码中 255 表示对应的 8 个比特要精确匹配，0 表示无关。所以 255.255.0.0 表示前 16 位有效，等价于 /16。
### What is CIDR, and why was it introduced?
- CIDR was introduced With the rapid exhaustion of IP addresses. It essentially assigns IP addresses using arbitrary‑length prefixes. CIDR has** helped decrease the router table size** but **cause longest-matching-prefix lookup**
### Name 4 takeaway observations around network traffic characteristics. Explain their consequences.
- Measurement studies on network traffic had shown **a large number of concurrent flows of short duration**. This already large number has only been increasing. Consequence (Inference): **This has a consequence that a caching solution would not work efficiently**
- The important element while performing any lookup operation is how fast it is done (lookup speed). Consequence (Inference): **A large part of the cost of computation for lookup is accessing memory**
- An unstable routing protocol may adversely impact the update time in the table: **add, delete or replace a prefix**. Consequence (Inference): **Inefficient routing protocols increase this value** up to additional milliseconds
- An important trade‑off is memory usage. Consequence (Inference): **We have the option to use expensive fast memory **(cache in software, SRAM in hardware) or cheaper but slower memory (e.g., DRAM, SDRAM)
### Why do we need multibit tries?
- While a unibit trie is very efficient and offers advantages such as fast lookup and easier updates, its biggest problem is the number of memory accesses that it requires to perform a lookup. It could be very inefficient in high-speed links
- So an alternative to unibit tries are the multibit tries. A multibit trie is a trie where each node has 2k children, where k is the stride. Next we will see that we can have**two flavors of multibit tries: fixed length stride tries and variable length stride tries.**
### What is prefix expansion, and why is it needed?
- Strategy called prefix expansion, where expand a given prefix to more prefixes.
- Reason:
	- It is needed to combat the issue of missing prefixes w**hen searching with a fixed stride length in multibit tries**
	- the expanded prefix is ensured to be a multiple of the chosen stride length
	- Simultaneously, all lengths that are not multiples of the chosen stride length are removed. This expansion results in more speed with **an increased cost of the database size**
### Perform a prefix lookup given a list of pointers for unibit tries, fixed-length multibit ties, and variable-length multibit tries.
- unibit Tries:
	- Begin the search for a longest prefix match by tracing the trie path, and follow the path corresponding to the bits of the destination address.
	- Continue the search until you fail (no match or an empty pointer)
	- When the search fails, the last known successful prefix traced in the path is the match and the returned value.
- Mulibit Tries: Fixed-stride:
	- The prefix search moves ahead with the preset length in n-bits (where n is the fixed stride length)
	- When the path is traced by a pointer, the last matched prefix (if any) is remembered. Every element in the trie represents two pieces of information: a pointer and a prefix value.
	- The search ends when an empty pointer is met
	- At that time, the last matched prefix is returned as the final prefix match
- Multibit Tries:  Variable Stride:
	- the core lookup mechanism would be similar to the fixed-stride approach, with the key difference being that **the number of bits examined at each node (the stride) can be different**. 
	- The optimum variable stride is selected by using dynamic programming
### Perform a prefix expansion. How many prefix lengths do old prefixes have? What about new prefixes?
- Old prefixes: The old (original) prefixes have 5 prefix lengths
- New prefixes: After prefix expansion, the new prefixes have only two lengths (3 and 6)
### What are the benefits of variable-stride versus fixed-stride multibit tries? 
- Variable Stride: 
	- Variable Stride Variable stride multibit tries offer **a more flexible approach** where a different number of bits can be examined at every step
	- By varying the strides, the goal is to make the prefix database **smaller**, and optimize for memory