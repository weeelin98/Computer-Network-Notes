### What does the transport layer provide?
- ==End-to-End Communication== The transport layer is responsible for providing end-to-end communication between applications running on different hosts
- ==Services to the Application Layer ==It provides services to the application layer that runs above it
- Addressing Applications (Multiplexing and Demultiplexing)
### What is a packet for the transport layer called?
- segment or transport‑layer segment
### What are the two main protocols within the transport layer?
- TCP (Transmission Control Protocol) and UDP (User Datagram Protocol)
- TCP provides some strong primitives with a goal to make end to end communication more reliable and cost-effective. It's very ubiquitous.  
- UDP provides very basic functionality and relies on the application layer
### What is multiplexing, and why is it necessary?
- The sending hoist will need to gather data from different sockets, and encapsulate each data chunk with header information to create segments, and then forward the segments to the network layer. We refer to this job as ==multiplexing==.
- Reason:
	-  The network layer, specifically the IP protocol, routes packets based solely on the destination IP address
	- An IP address identifies a specific host on the network, but it doesn't specify which particular application process on that host should receive the incoming packets
	-   To ensure that data packets are delivered to the correct application process on the receiving host, an additional addressing mechanism is required to distinguish between the many processes sharing the same IP address
### Describe the two types of multiplexing/demultiplexing.
- ==connectionless and connection oriented==
- Connectionless Multiplexing and Demultiplexing (associated with UDP):
	- The identifier for a UDP socket is a two-tuple consisting of a destination IP address and a destination port number
	- When a receiving host receives a UDP segment, its transport layer identifies the correct socket by looking at the destination port field
	- Segments received with the same destination IP address and destination port number will be forwarded to the same destination process via the same destination socket
-   Connection-Oriented Multiplexing and Demultiplexing (associated with TCP):
	- The identifier for a TCP socket is a four-tuple consisting of the source IP, source port, destination IP, and destination port
	- When a receiving TCP server receives a connection request from a client, it creates a new socket that is identified by the four-tuple involving the client's source IP and port, and the server's destination IP and port
	- The server then uses this four-tuple socket identifier to demultiplex incoming data and forward it to this specific socket
	- This means that even if multiple clients connect to the same destination IP address and destination port, the receiving host will be able to demultiplex the incoming data from the different connections because they are associated with different source IP addresses or source port numbers, leading to unique four-tuples for each connection
### What are the differences between UDP and TCP?
- Service Type:
	- TCP provides a connection-oriented service
	- UDP provides a connectionless service
- Reliability:
	- TCP offers guaranteed delivery of application-layer messages
	- UDP provides a best-effort service to applications. It does not have mechanisms for reliability
- Flow Control:
	- TCP provides flow control
	- UDP does not have flow control
- Congestion Control:
	- TCP includes a congestion-control mechanism. The sender slows its transmission rate when it perceives the network to be congested
	- UDP does not have congestion control
- Overhead and Delay:
	- UDP offers less delays and better control over sending data
	- TCP has more overhead and can introduce delays due to its built-in mechanisms for reliability, flow control, congestion control, and connection management
- Addressing (Multiplexing/Demultiplexing):
	- A UDP socket is identified by a two-tuple consisting of the destination IP address and a destination port number
	- A TCP socket is identified by a four-tuple consisting of the source IP, source port, destination IP, and destination port
- Error Checking:
	- UDP includes a basic checksum for error checking. The receiver can detect if bits in the segment have been changed
	- TCP segments also include header information for error detection, but its primary method for handling errors (like corrupted packets) is through its reliability mechanisms which lead to retransmission rather than just discarding the corrupted segment
- Application Suitability:
	- UDP is often preferred for ==real-time applications sensitive to delays==, where some packet loss is acceptable, such as DNS and multimedia applications like Internet Telephony
	- ==TCP is ubiquitous and used for most applications today==
### When would an application layer protocol choose UDP over TCP?
- prefer UDP:
	- Less Delays and Better Control
	- No Congestion Control13
	- No Reliability Mechanisms
	- No Connection Management Overhead
### Explain the TCP Three-way Handshake
- establish a connection between two hosts before data transmission begin
- three steps of the TCP Three-Way Handshake:
	- Step 1: The Client Sends a SYN. This segment contains no application data
	- Step 2: The Server Responds with a SYNACK. The server then sends back a special 'connection-granted' segment, known as a SYNACK segment
	- Step 3: The Client Sends an ACK. The client then sends a final segment, an acknowledgment (ACK) segment, back to the server. In this ACK segment, the SYN bit is set to 0

### Explain the TCP connection tear down.
- The TCP Connection Teardown process is how a TCP connection between two hosts is gracefully closed after data transmission is complete
- The connection teardown typically involves a four-way exchange:
	- Step 1: The Client Sends a FIN
	- Step 2: The Server Responds with an ACK
	- Step 3: The Server Sends a FIN
	- Step 4: The Client Sends an ACK
### What is Automatic Repeat Request or ARQ?
- Reliable communication requires the sender to know which segments were received by the remote host and which were lost2
- One way to achieve this is by having the receiver send acknowledgements (ACKs) indicating that it has successfully received a specific segment
- In case the sender does not receive an acknowledgement within a given period of time (timeout), the sender can assume the packet is lost and resend it
### What is Stop and Wait ARQ?
- The sender transmits a packet
- The sender then waits for an acknowledgement from the receiver
### What is Go-back-N?
- The receiver identifies a missing packet
- The receiver sends an acknowledgement (ACK) for the most recently received in-order packet
- The sender then resends all packets starting from the most recently received in-order packet, even if some of those packets had been sent before2
- The receiver, in this scenario, discards any out-of-order received packets
### What is selective ACKing?
- the receiver acknowledges a correctly received packet even if it is not in order
- The receiver buffers the out-of-order packets until any missing packets have been received
- The sender retransmits only those packets that it suspects were received in error
-  Once the missing packets that were needed to fill gaps are received, the batch of packets that are now in order can be delivered to the application layer
### What is fast retransmit?
- It relies on duplicate acknowledgements
- When a TCP sender receives 3 duplicate ACKs for a specific packet, it interprets this as a strong indication that the packet after the acknowledged one has likely been lost in the network1
- Upon receiving this threshold of duplicate ACKs (3), the sender immediately retransmits the packet it suspects is lost, without waiting for the retransmission timeout to expire
### What is transmission control, and why do we need to control it?
- transmission control refers to mechanisms used to control the rate at which data is transmitted over a network
- Reason: **to avoid congestion in the network**
### What is flow control, and why do we need to control it?
- Flow Control is a mechanism used in the transport layer, specifically by TCP, to control the transmission rate to protect the receiver's buffer from overflowing
- Reason:to protect the receiver's buffer from overflowing
### What is congestion control?
- congestion control is a mechanism, primarily implemented by TCP (Transmission Control Protocol) in the transport layer, used to control the transmission rate Its main purpose is to protect the network from congestion
### What are the goals of congestion control?
- **Efficiency**: The algorithm should strive for high throughput and efficient utilization of the network resources
- **Fairness**: It should ensure that each user receives a fair share of the available network bandwidth. The source defines this fairness in this context as every flow under the same bottleneck link ideally getting an equal share of bandwidth
-  **Low delay** : While it's theoretically possible to maximize throughput with infinite buffers, ==this would lead to long queues and significant delays==, negatively impacting delay-sensitive applications like video conferencing. Therefore, keeping network delays small is a goal
- **Fast convergence**: A flow should quickly converge to its fair allocation of bandwidth. This is particularly important because a significant portion of network traffic consists of short flows, and slow convergence would mean these short flows might not receive their fair share before they finish
### What is network-assisted congestion control?
- In this approach, the system relies on the network layer to provide explicit feedback to the sender about congestion in the network
- potential limitation of this approach: under severe congestion, the explicit feedback itself (like ICMP packets) could be lost, making it ineffective
- This is contrasted with the end-to-end congestion control approach, where the network does not provide any explicit feedback. Instead, hosts infer congestion from network behavior, which is the approach primarily used by TCP2
### What is end-to-end congestion control?
- In this approach, the system does not rely on the network layer to provide any explicit feedback about congestion to the end-hosts
- Instead, the hosts infer congestion from network behavior and adjust their transmission rate accordingly. This approach aligns with the end-to-end principle, which suggests keeping the network core simple and placing complexity and intelligence at the end systems
### How does a host infer congestion?
- ** Packet Delay**: As the network gets congested, the queues in router buffers tend to build up2. This leads to increased packet delays2. An increase in the round trip time (RTT), which can be estimated based on acknowledgements (ACKs), can therefore be an indicator of congestion in the network2. However, packet delay in a network can be variable, which makes delay-based congestion inference quite tricky
- **packet loss** Routers start dropping packets when the network becomes congested. packet loss is primarily due to congestion, especially severe congestion
### How does a TCP sender limit the sending rate?
- a TCP sender limits its sending rate primarily through two mechanisms: ==flow control and congestion control==
- Flow Control: This mechanism controls the transmission rate to protect the receiver's buffer from overflowing
- Congestion Control: This is a very important reason for transmission control and aims to protect the network from congestion
### Explain Additive Increase/Multiplicative Decrease (AIMD) in the context of TCP
- Additive Increase/Multiplicative Decrease (AIMD) is the core combined mechanism that TCP uses for congestion control
- It represents TCP's "probe-and-adapt" strategy to adjust its congestion window (cwnd). The primary goal of this adjustment is to protect the network from congestion
- Additive Increase:The idea is to increase the cwnd gradually(SLOW!)
- Multiplicative Decrease:TCP aggressively reduces the cwnd(FAST!)
### What is a slow start in TCP?
- Slow Start is a distinct phase within TCP's congestion control mechanism used to rapidly increase the sending rate at the beginning of a connection or after detecting severe congestion
- Once the cwnd equals or exceeds this threshold, TCP exits the Slow Start phase and switches to the Additive Increase/Multiplicative Decrease (AIMD)
- Slow Start After Timeout: Slow Start is not only used for new connections. It also kicks in when a connection experiences a timeout. A timeout is considered a more severe indication of congestion compared to receiving duplicate ACKs7 After a timeout, the cwnd is typically reset to a small initial value (often 1 packet), and the Slow Start process begins again
### Is TCP fair in the case where connections have the same RTT? Explain.
- TCP is generally considered fair in the case where competing connections have the same Round Trip Time (RTT)
- This fairness in bandwidth sharing is primarily achieved through the Additive Increase/Multiplicative Decrease (AIMD) congestion control policy used by TCP
### Is TCP fair in the case where two connections have different RTTs? Explain.
- TCP is generally considered ==not fair ==in cases where competing connections have different Round Trip Times (RTTs)
- When connections have different RTTs, the connections with smaller RTT values will receive ACKs more frequently than those with larger RTT value
- The faster increase in the cwnd for connections with smaller RTTs means they will grab a larger share of the available bandwidth on a shared link compared to connections with longer RTTs
- In TCP, **throughput (发送速率)** is approximately governed by this formula: **Throughput ≈ (cwnd) / RTT** So if two connections have: the **same congestion window size (cwnd)**,but **different RTTs**, then the one with the **smaller RTT** will have **higher throughput**.

### Explain how TCP CUBIC works.
- TCP CUBIC is a modern congestion control algorithm designed primarily for high bandwidth delay product networks to improve link utilization
- Response to Congestion:
	- CUBIC uses congestion signals to adjust the sender's congestion window (cwnd)
	- When CUBIC detects congestion via triple duplicate ACKs (considered mild congestion), it reduces the cwnd. This reduction is a multiplicative decrease, where the cwnd is halved. If the cwnd was Wmax at the time of the triple duplicate ACK, it is set to Wmin = Wmax / . This multiplicative decrease is done to maintain TCP-fairness
	- A timeout is considered a more severe form of congestion, and in older TCP Reno, this would reset the cwnd to the Initial Window (often 1 packet)
- Window Growth (Increase Phase):
	- After a congestion event causes the cwnd to decrease to Wmin from Wmax, CUBIC enters an increase phase, probing for more bandwidth
	- the function is given as W(t) = C(t-K)³ + Wmax
- RTT-Fairness:
	- A key feature of CUBIC is that its window growth depends only on the time between two consecutive congestion events, not on the Round Trip Time (RTT). This contrasts with TCP Reno's ACK-based increase, where connections with smaller RTTs increase their cwnd faster due to receiving ACKs more frequently

### Explain TCP throughput calculation
- > **TCP Throughput 吞吐量 ≈ (MSS / RTT) × (1 / √p)** 吞吐量 = 单位时间内能**成功传输的数据量**
- > “一个 TCP 连接在给定网络条件下，**每秒钟**可以发送多少数据？”
-  The Sawtooth Pattern: TCP continuously decreases and increases its congestion window (cwnd) over the lifetime of a connection, resulting in a sawtooth pattern when plotting cwnd over time
- Modeling a Cycle: The calculation considers one cycle of this sawtooth pattern
- Packets Sent per Cycle: The total number of packets sent in one cycle is estimated by the area under this sawtooth pattern from W/2 to W
- Relationship with Loss Probability: The model assumes a constant packet loss probability p. This means that, on average, one packet is lost for every 1/p packets sent. Therefore, the number of packets sent in one cycle (leading to one loss event) is 1/p
- Calculating Maximum Window (W): By equating the number of packets sent in a cycle to 1/p, we get 1/p = 3/8 * W^2. Solving for W, the maximum window size reached before a loss, gives W = sqrt(8 / (3p))
-  Calculating Throughput (BW): Throughput is defined as the total amount of data transmitted per cycle divided by the time taken for one cycle
- 假设：MSS = 1460 bytes = 11,680 bits
    RTT = 100 ms = 0.1 s 
    p = 1% = 0.01
    套公式： Throughput ≈ (11,680 / 0.1) × (1 / √0.01)
    = 116,800 × (1 / 0.1)
    = **1,168,000 bps = 1.17 Mbps**