What is the difference between forwarding and routing?
- ==Forwarding== (also known as switching) refers to the a==ction of transferring== a packet from an incoming link interface to the appropriate outgoing link interface within a single router. This process is local to the router3 and happens at very short timescales, typically a few nanoseconds, and is usually implemented in hardware
- ==Routing== refers to how routers work together using routing protocols ==to determine== the good paths (or good routes) over which the packets travel from the source to the destination node across the network. It's an end-to-end process
What is the main idea behind a link-state routing algorithm?
- the main idea behind a link-state routing algorithm is that all nodes (routers) in the network have complete knowledge of the network topology and the costs of all links
- Core idea: 
	- Global Knowledge
	- Information Sharing
	- Independent Computation
	- Algorithm Used
What is an example of a link-state routing algorithm?
- OSPF (Open Shortest Path First)
- It uses the Dijkstra least-cost path algorithm to find the best path between routers
Walk through an example of the link-state routing algorithm.
- The main idea behind the link-state routing algorithm is that ==all nodes (routers) in the network know the complete network topology and the costs of all links==
- **Selecting the next node has the least current cost D(w)**
- **Updating neighbor costs**
What is the computational complexity of the link-state routing algorithm?
- > The **computational complexity** of the link-state routing algorithm, mainly determined by **Dijkstra’s algorithm**, is typically **O(N²)**, or **O(N log N + E)**
- Array -> **O(N²)**,   Stack -> **O(N log N + E)**
What is the main idea behind the distance vector routing algorithm?
- the main idea behind the distance vector routing algorithm is a ==distributed, iterative, and asynchronous== process where each node (router) calculates and maintains its own view of the network, specifically the costs to reach every other node
Walk through an example of the distance vector algorithm.
- The algorithm is based on the Bellman-Ford equation: 
	- Dx(y)= minv{c(x,v)+ Dv(y)}
	- Initialization
	- Iterations (Exchange and Update)
	- Convergence 收敛
When does the count-to-infinity problem occur in the distance vector algorithm?
- when a link cost increases by a large amount
How does poison reverse solve the count-to-infinity problem?
- If a node (say, node Z) determines that its best path to reach a destination (say, destination X) is through a specific neighbor (say, neighbor Y), node Z will advertise its distance to X back to that neighbor Y as infinity,
What is the Routing Information Protocol (RIP)?
- the Routing Information Protocol (RIP) is a routing protocol used for intradomain routing, meaning it operates within a single administrative domain or Autonomous System (AS)
	-  Based on Distance Vector
	-  Metric
	- Periodic Updates
	- RIP Advertisements
	- Routing Table Structure
	- Route Aggregation
	- Neighbor Reachability
	- Transport Protocol
What is the Open Shortest Path First (OSPF) protocol?
- It was introduced as an advancement of the RIP Protocol
- Based on Link-State Algorithm: OSPF utilizes a link-state routing algorithm to determine the best path between routers
- running Dijkstra's algorithm locally
- OSPF routers exchange Link State Advertisements (LSAs),LSAs communicate the router's local routing topology (its links and their costs) to all other routers in the same OSPF area, not just its neighbors,This process is called flooding
- Link State Database: LSAs are used by routers to build and maintain a database, called the link state database, which contains all the link states within the area. This database helps form a consistent network topology view across all routers in the domain
- Whenever there is a change in a link's state (e.g., cost changes or failures), the connected router broadcasts LSAs to all other routers in the AS. Routers also broadcast LSAs periodically, even if the state hasn't changed, with a default refresh period of 30 minutes
- Processing Messages: When a router receives an LS update packet containing LSAs from a neighbor, its OSPF process (running in the route processor) checks if each LSA is new or a duplicate using sequence numbers.  New LSAs update the link-state database, trigger the scheduling of a Shortest Path First (SPF) calculation (Dijkstra), and are flooded out to other interfaces. The SPF calculation is a CPU-intensive task and is scheduled over time to manage router resources
- Forwarding Table (FIB): The results of the SPF calculation are used to update the router's Forwarding Information Base (FIB), which is then used for forwarding data packets
- **Key Features & Advancements**: 
	- Authentication:
	- Multiple Same-Cost Paths
	- Hierarchy: An OSPF autonomous system can be configured hierarchically into areas. Each area runs its own OSPF algorithm. Area border routers are responsible for routing traffic outside their area, and traffic between different areas must pass through these border routers and a designated backbone area
	- Policy: OSPF is primarily focused on "optimizing a path metric" within the network
How does a router process advertisements?
- Distance Vector Routing (e.g., RIP)
	- each router maintains its own distance vector, which contains the calculated least cost to reach every other destination node in the network7. This is part of the router's routing table
	- When receiving updates, **Bellman-Ford equation**: Dx(y)= minv{c(x,v)+ Dv(y)}
	- If router's Dv changes, sends its updated DV to neighbors
- Link-State Routing (e.g., OSPF)
	- When a router receives an LS update packet containing LSAs, its OSPF process processes them
	- For each LSA in the packet, the router checks if it is a new LSA or a duplicate by referring to its link-state database and checking sequence numbers
	- If an LSA is new, the router **updates its link-state database**
	- Updating the link-state database triggers the scheduling of a **Shortest Path First (SPF) calculation (which is Dijkstra's algorithm)**
	- The router runs Dijkstra's algorithm locally, considering itself the root, to compute the shortest path (least-cost path) to all other subnets in the AS. This is a CPU-intensive task and is scheduled over time
	- The results of the SPF calculation are used to update the router's** Forwarding Information Base (FIB)**
- Path Vector Routing (e.g., BGP)
	- BGP routers exchange routing information over BGP sessions, which are semi-permanent TCP connections between BGP peers
	- Routing information is exchanged using BGP messages, primarily UPDATE messages
	- BGP routes are advertised for IP Prefixes (destination subnets) and include various path attributes, such as the AS-PATH (list of ASes the route traverses) and NEXT-HOP (IP address of the next router)
	- When a router receives incoming BGP messages/advertisements, it first applies import policies to filter the routes
	- Accepted routes then enter the BGP Decision Process
	- The selected best routes are installed into the router's **Forwarding Information Base (FIB) or forwarding table**
	- Finally, the router applies export policies to decide which neighbors it will advertise its selected routes to.
What is hot potato routing?
- Hot Potato Routing is a technique or practice used in large networks that rely on both intradomain and interdomain routing protocols to route traffic
- When a packet's final destination is **outside the network** (requiring interdomain routing), the traffic needs to travel towards one of the network's exit points, known as egress points
- Hot Potato Routing involves choosing the closest egress point based on the intradomain path cost
- **Advantages**:
	- It simplifies computations for the routers as they already have the IGP path costs readily available
	- It helps ensure that the internal path remains consistent
	- It effectively reduces the network's resource consumption by directing traffic out of the network as soon as possible
