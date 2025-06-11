### Describe the relationships between ISPs, IXPs, and CDNs.
- Internet Service Providers (ISPs):access ISPs(tier-3),regional ISPs(tier-2), large global scale ISPs(tier-1)
- Content Distribution Networks (CDNs):CDNs are created by content providers (like Google or Netflix) or third-party providers (like Akamai or Limelight) with the goal of gaining greater control over content delivery to end-users and reducing connectivity costs
- Internet Exchange Points (IXPs): IXPs are physical infrastructures that provide a meeting point where multiple networks, including ISPs and CDNs, can interconnect and exchange traffic locally
- **Relationships** :
	-  Provider-Customer Relationship (Transit):This is a hierarchical relationship where a smaller network (customer) pays a larger network (provider) to carry its traffic to destinations in the provider's routing table
	- Peering Relationship: settlement-free agreement to directly exchange traffic between their respective customers. This allows them to **save costs** they would otherwise pay to their providers
	- Interconnection at IXPs: IXPs facilitate peering between ISPs and CDNs. Peering at IXPs offers advantages like **keeping local traffic local** (avoiding transit through other networks), **lower costs compared to transit** based on volume, and i**mproved network performance due to reduced delay**
	- CDN-ISP Interaction: CDNs strategically place their servers deep within ISP access networks (the "Enter Deep" approach) or at major IXPs (the "Bring Home" approach). 
### What is an AS?
- An Autonomous System (AS) is a group of routers, including the links among them, that operate under the same administrative authority. 
- Administrative Domain: An AS represents a single administrative domain
-  Policy and Control: Each AS implements its own set of policies, makes its own traffic engineering decisions, and determines how traffic enters and leaves its network
- Routing Protocols:
	- Interdomain Routing: The border routers of ASes use the Border Gateway Protocol (BGP) to exchange routing information with one another, specifically about IP prefixes they can reach
	-  Intradomain Routing: Within an AS, Internal Gateway Protocols (IGPs) like OSPF or RIP are used to determine paths between internal routers based on specific costs within that network
- Business Relationships:
	- Provider-Customer
	- Peering
### What kind of relationship does AS have with other parties?
- Provider-Customer Relationship (Transit)
- Peering Relationship
### What is BGP?
- Border Gateway Protocol (BGP) is the routing protocol used by the border routers of Autonomous Systems (ASes) to exchange routing information with one another.
### How does an AS determine what rules to import/export?
- Business Relationships as the Driver:Provider-Customer (or transit) and Peering
- Exporting Routes:
	- Routes learned from customers:
	- Routes learned from providers
	- Routes learned from peers
- Importing Routes:
	- Customer routes
	- Peer routes
	-  Provider routes
### What were the original design goals of BGP? What was considered later?
Goals: 
	- Scalability 
	- Express routing polices
	- allow cooperation among autonomous systems
### What are the basics of BGP?
- BGP session: A pair of routers, called BGP peers, exchange routing info over semipermanent TCP port connection called BGP session.  These sessions can be external BGP (eBGP) between routers in two different ASes, or internal BGP (iBGP) between routers within the same AS.
- After a session is established, peers exchange BGP messages, including UPDATES (announcements of new/updated routes, or withdrawals of previously announced routes) and KEEPALIVE messages to maintain the session.
- IP Prefixes and Reachability: In BGP, destinations are represented by IP Prefixes. Gateway routers running eBGP advertise the IP Prefixes they can reach according to their AS's export policy to routers in neighboring ASes
### What is the difference between iBGP and eBGP?
- external BGP (eBGP):
	- An eBGP session is established between a pair of routers that belong to two different Autonomous Systems (ASes)
	- These are typically the border routers of the ASes
	- eBGP speaking routers learn routes to external prefixes from other ASes
- internal BGP (iBGP):
	- An iBGP session is established between routers that belong to the same Autonomous System (AS)
	- The primary purpose of iBGP is to disseminate external routes (those learned via eBGP from other ASes) to other internal routers within the same AS
	- It's important to note that iBGP is not an Internal Gateway Protocol (IGP) like RIP or OSPF6. IGPs are used to establish paths between internal routers based on costs within the AS, whereas iBGP is only used to disseminate external routes within the AS
### What is the difference between iBGP and IGP-like protocols (RIP or OSPF)?
- the primary difference between iBGP and IGP-like protocols such as RIP or OSPF lies in the type of routing information they disseminate within an Autonomous System (AS) and their fundamental purpose.
- Purpose and Information Exchanged:
	- IGP-like protocols (RIP, OSPF):These protocols are used for intradomain routing. Their main purpose is to establish paths between routers within a single AS based on specific costs or metrics defined within that AS. They focus on "optimizing a path metric" within the network
	- iBGP:iBGP sessions are established between routers that belong to the same AS. its primary purpose is not to calculate paths within the AS based on internal metrics. Instead, iBGP is used to disseminate routes for external destinations
- Scope and Functionality:
	- IGP-like protocols (RIP, OSPF):Operate strictly within a single administrative domain (an AS)
	- iBGPP: Operates within a single AS, but it is a type of BGP, which is fundamentally an interdomain routing protocol used between ASes
### How does a router use the BGP decision process to choose which routes to import?
- a router uses the BGP decision process to choose which routes to import by translating its Autonomous System's (AS) routing policies, which are primarily driven by business relationships, into a series of comparisons based on route attributes
- Applying Import Policies (Filtering): Before the decision process begins, the router applies its AS's import policies. These policies are essentially rules that exclude certain received routes entirely from further consideration
- The Decision Process (Ranking): If a router receives multiple route advertisements towards the same destination prefix (and they pass the import filters), it must then rank these valid routes to select the "best" one
- Attributes Used in Ranking:
	- LocalPref
	- MED (Multi-Exit Discriminator):
	- AS-PATH
### What are the 2 main challenges with BGP? Why?
- **Scalability**: As the Internet continues to grow in size, with increasing numbers of Autonomous Systems (ASes), network prefixes in routing tables, network churn (changes in network status), and BGP traffic exchanged between routers, BGP faces challenges in managing these complications
- Misconfigurations and Faults: BGP configuration is complex, especially at the eBGP level and within an AS at the iBGP level, where route propagation happens. Configuration languages vary among routing manufacturers and may not be well-designed, adding to the complexity, as does BGP's distributed nature
### What is an IXP?
- Internet Exchange Point (IXP) is a **physical infrastructure** that provides the means for Autonomous Systems (ASes) to interconnect and directly exchange traffic with one another
### What are four reasons for IXP's increased popularity?
- IXPs are interconnection hubs handling large traffic volumes
- Important role in mitigating DDoS attacks
- “Real‑world” infrastructures with a plethora of research opportunities
- IXPs are active marketplaces and technology innovation hubs
### Which services do IXPs provide?
- Public peering
- Private peering (Private Interconnects - PIs)
- Route servers and Service level agreements
- Remote peering through resellers
- Mobile peering
- DDoS blackholing
- Free value‑added services
### How does a route server work?
- Collects and shares routing information from its peers or participants that connects with (i.e. IXP members that connect to the RS).
- Executes it’s own BGP decision process and also re‑advertise the resulting information (I.e. best route selection) to all RS’s peer routers.
