### Why is packet classification needed?
- packet classification is needed because, as the Internet becomes increasingly complex, networks require **quality of service guarantees and security guarantees** for their traffic
### What are three established variants of packet classification?
- **Firewalls**: Routers implement firewalls at the entry and exit points of the network to filter out unwanted traffic or to enforce other security policies
- **Resource reservation protocols**: For example, DiffServ has been used to reserve bandwidth between a source and a destination
- **Routing based on traffic type**: Routing based on the specific type of traffic helps avoid delays for time-sensitive applications
### What are the simple solutions to the packet classification problem?
- Linear Search
	- 最简单的方法是逐条检查规则，比如防火墙会从上到下查看每条规则，找到最匹配的那一条。如果规则数量很少，这种方法还行。但如果有几千条规则，查一遍就会很慢，效率低下。
- Caching
	- **缓存最近匹配过的结果**，下次遇到相同的包时可以直接用结果，速度更快。这种方法的主要问题是：即使缓存命中率很高（比如 90%），**对那 10% 未命中的包还是要慢慢查找所有规则**。所以整体性能仍然可能不理想
- Passing Labels
	- 像 MPLS（多协议标签交换）和 DiffServ（差异化服务）这样的技术，会在网络边缘先进行一次包分类，然后**贴上标签**。接下来的路由器只需要看标签，不再重新分类。这种方法适合提前规划好的路径和服务质量，能大幅降低分类开销。
### How does fast searching using set-pruning tries work?
- Fast searching using set-pruning tries is a method for classifying packets using two-dimensional rules, such as by both source and destination IP addresses
	- The **simplest way** to approach this problem is to **build a trie on the destination prefixes** in the database
	- Then, for every leaf-node at the destination trie, “hang” source tries
	- By doing this, for every destination prefix 'D' in the destination trie, the set of rules is “pruned” to those that are compatible with D
	- The algorithm first matches the destination IP address in a packet in the destination trie
	- Then, it traverses the corresponding source trie to find the longest prefix match for the source IP
	- The algorithm keeps track of the lowest-cost matching rule and concludes with the least-cost rule

- Set-Pruning Tries 用于根据**目的地址和源地址**同时分类数据包。先建一棵目的地址前缀树，每个叶节点下挂一个只包含相关规则的源地址前缀树（这叫剪枝）。查找时先匹配目的 IP，再在对应的源地址树中匹配源 IP，选出最合适的规则。缺点是源地址树可能大量重复，导致**内存消耗大**。
### What’s the main problem with the set pruning tries?
- A challenge with set-pruning tries is the memory explosion, because a source prefix can occur in multiple destination tries
### What is the difference between the pruning approach and the backtracking approach for packet classification with a trie?
- Set-pruning Tries:
	- Build a destination prefix trie; each leaf node has a source trie.
	- When searching, **first match the destination, then match the source in the corresponding source trie.**
	- **Fast lookup**, but **uses a lot of memory**, since source tries can be duplicated across many destinations.
- Backtracking Tries:
	- Also use a destination prefix trie, but only attach source tries to **exact-match prefixes**.
	- During lookup, match the destination first, then **backtrack up** the destination trie and check each source trie.
	- **Saves memory** (each rule stored once), but **slower** because of backtracking.
- > **Set-Pruning = 查得快但很占内存；Backtracking = 省内存但查得慢。**
### What’s the benefit of a grid of tries approach?
- the benefit of the grid of tries approach for packet classification is primarily centered on **reducing lookup time and improving efficiency compared to the backtracking method**
- 它的核心做法是：在源地址前缀树中，**提前计算好“跳转指针”**。当你在某个地方匹配失败时，不用回头重查上层节点，而是**直接跳到更有可能匹配的下一个源地址前缀树**。这就像在路上放了“捷径指示牌”。因此，Grid of Tries 可以省去 Backtracking 中“不断回头找父节点再重新查”的过程，提升了搜索效率，同时还保留了内存节省的优点。
### Describe the “Take the Ticket” algorithm.
- The "Take the ticket algorithm" is a simple scheduling algorithm used in an NxN crossbar switch
- Each output line maintains a distributed queue for all input lines that wish to send packets to it
- When an input line intends to send a packet to a particular output line, it requests a ticket
- The input line then waits for its ticket to be served
- Once the ticket is served, the input line connects to the output line, the crosspoint is activated, and the input line transmits the packet
- A significant problem associated with this algorithm is **head-of-line (HOL) blocking**
### What is the head-of-line problem?
- > 在多个输入口排队等同一个输出口时，**队伍最前面那个人没发完，后面的人就只能干等着，哪怕他们本来可以发数据。** 在 “Take-a-ticket” 调度中，比如 A、B、C 都想发给输出口 1，A 拿到票先发。**B 和 C 就算准备好了、其他输出口也空着，也必须等 A 发完才能轮到他们。** 这种“被排在前面的人拖住”的情况，就叫 **Head-of-Line Blocking（行首阻塞）**。
- this scheduling algorithm, an input line connects to an output line and transmits a packet once its "ticket" is served. The problem arises because while one input line is sending its packet, the entire queue for other input lines waiting to send to the same output link is blocked by the progress of the head of the queue. This is referred to as head-of-line (HOL) blocking because the entire queue is blocked by the progress of the head of the queue
### How is the head-of-line problem avoided using the knockout scheme?
- The Knockout scheme relies on breaking up packets into fixed size (cell)
- The goal is to send the packet to an output link without queuing at the input, meaning a packet arriving at an output link would only block packets sent to the same output link
- This is achieved by **having the switching fabric run 'N' times faster than the input links**
- In practice, if an output rarely receives 'N' cells and the expected number is 'k' (smaller than 'N'), then the fabric can run 'k' times as fast as an input link, instead of 'N'
- To accommodate scenarios where more cells arrive than 'k', **the system uses one or more primitive switching elements that randomly pick the chosen output**
- > **把数据包切成小块（cells）**，然后用一个**运行速度比输入快很多倍的交换结构**，让每个包尽快送到输出口，这样就不会在输入口排长队。 如果同时太多输入都想发给同一个输出，那就用一个简单的随机方式，挑出一部分送过去，其他丢弃或重发。
### How is the head-of-line problem avoided using parallel iterative matching?
- > **不只是让队列最前面的那个包尝试连接，而是让更多包有机会发出去**，这样就算最前面的包“卡住”了，后面的也不会被白白拖延。
- Requests Phase: All inputs simultaneously send requests to all outputs they intend to connect with.每个输入口**同时向所有它想要连接的输出口发出请求**，不只针对排在最前的那个请求。
- Grant Phase: Outputs that receive multiple requests randomly select one input. For example, if output link 1 receives requests from input lines A, B, and C, it might randomly choose B. Similarly, output link might randomly choose A if it receives requests from A and B每个输出口收到多个请求时，**随机挑选一个输入口**来“批准连接”。
- Accept Phase: Inputs that have received multiple grants randomly select an output to send to. For instance, if input A receives grants from output ports 2 and 3, it randomly chooses port 2每个输入口收到多个 grant 后，也会随机选一个输出口连接。
- The overall outcome is that all traffic can be sent in fewer cell times, making it clearly more efficient than the "take the ticket" algorithm
### Describe FIFO with tail drop.
- FIFO with tail drop is the simplest method of router scheduling
- > **先进先出（FIFO）排队**，如果队列满了，**新的包直接丢弃（tail drop）**
- 当数据包从输入链路进入路由器时，路由器会先查找目的地址，确定应该将该包从哪个输出端口发出。随后，数据包被放入对应输出口的队列中，该队列按照先进先出（FIFO）原则进行排队：先进入的包先被发送。如果此时队列已满，新的数据包将无法进入队尾，并被直接丢弃，这种丢包方式被称为“tail drop”。
	- Packets enter a router on input links
	- They are then looked up using the address lookup component
	- This lookup process gives the router an output link number
	- The switching system within the router then places the packet in the corresponding output port
	- This output port operates as a FIFO (first in, first out) queue
	- If the output link buffer is completely full, incoming packets to the tail of the queue are dropped
- This method results in fast scheduling decisions, but it comes with the drawback of potential loss in important data packets
### What are the reasons for making scheduling decisions more complex than FIFO?
- 虽然 **FIFO + tail drop** 方法简单、处理快，但它有个致命缺点：**无法区分重要和不重要的数据包**，导致可能丢掉关键数据，影响用户体验。因此，路由器需要更复杂的调度方法，原因有三：
- 1. **帮助处理网络拥塞**1. 现在网络流量增长很快，很多地方都会堵。如果只靠 TCP 自己控制不够，路由器如果能聪明一点，优先处理重要数据，就能提升整体吞吐量。
- 2. **保证公平性**2. 多个用户/连接一起争抢出口时，普通 FIFO 会被“大流量”占满，导致小流量（比如视频、实时通话）被卡住，甚至完全发送不了。这时候就需要**更公平的调度机制**来避免“被挤掉”。
- 3. **支持服务质量（QoS）**3. 有些连接（像视频会议、语音）对**带宽和延迟**要求很高。如果调度器能保证某个流有“最低带宽”或“最大延迟”，用户体验就会更好。FIFO 没法做到这些。
### Describe Bit-by-bit Round Robin scheduling逐比特轮询调度.
- **Bit-by-bit Round Robin（逐比特轮询）** 是一种**想象中的调度算法**，它的目标是实现**公平的带宽分配**。  ***但现实中，我们不能真的“一个比特一个比特地发”，因为数据包是整体发的。怎么办？**所以这个算法在现实中是**通过计算“假设每个包是逐比特轮询发完时的完成时间”**，来决定应该按什么顺序发整包。也就是说：虽然实际是“一整个包”一起发但我们**用“每轮发一比特”来模拟谁应先完成** 谁“理论上”最早完成，就先发谁*
- dR / dt = µ / N
- 1. **当前轮数增长速度**：dR/dt = \mu / N意思是：**活跃的流越多，轮数推进越慢**（因为带宽要分给更多人）
- **包什么时候排到头（S(i)）**：S(i) = \max(R(t), F(i-1))意思是：这个包要等前一个包发完，或等当前轮数到它，取两者中较晚的那个作为“开始轮次”。
- **包什么时候发完（F(i)）**：
    F(i) = S(i) + p(i)就是“开始轮次 + 包的长度”，得到“完成轮次”。
### Bit-by-bit Round Robin provides fairness; what’s the problem with this method?
- **你不能真的一个比特一个比特发**
- **你需要提前“算出”每个包会什么时候发完**
- **你必须从所有活跃的队列中，挑出“最早完成的包**
- **在高速网络里，这种计算太慢了**
- Bit-by-bit Round Robin ensures fairness, but it’s **impractical** in real systems. You can’t split packets, so you need to **simulate** finish times using math. That leads to **complex scheduling** using priority queues and timestamp updates, which are **too slow for high-speed routers**.
### Describe Deficit Round Robin (DRR).
-  **核心机制：**每个流（flow）有两个参数：
	- **Quantum (Qi)**：这个流每轮最多能发送多少“额度”的数据。
	- **Deficit Counter (Di)**：这个流“上轮没用完的额度”会被累计到下一轮。
- 每一轮轮询时：如果包的大小 ≤ (Qi + Di)，那就发出去，并从中扣除。 如果有多余额度但队列没包了，**Di 设为 0**；如果没发完所有包，那剩余额度存在 **Di** 里，下轮继续用。
### What is a token bucket shaping?
- This technique is used to limit the burstiness of a flow
- Token Bucket Shaping 是一个**控制网络流量速度和突发行为**的方法。它的作用有两个：
	- 1. 控制这个流的**平均发送速率**，比如：只能平均每秒发送 100kbps
	- 2. 同时也允许它**偶尔短时间突发**地发送更多，比如：允许一次性最多发 4KB
- The technique assumes a bucket per flow, which fills with tokens at a rate R per second
- If the bucket is full with B tokens, any additional tokens are dropped
- When a packet arrives, it can go through if there are enough tokens (equal to the size of the packet in bits)
- If not enough tokens are available, the packet needs to wait until enough tokens are in the bucket
- Given the maximum size of B, a burst is limited to B bits per second
- Token Bucket 本来是“每个流一个桶”，所以你得给每个流一个队列： 有的流桶是满的，可以发.有的流桶是空的，要等 那它们就不能混在一起排队.这样就导致实现复杂，特别是在很多流的系统中。### **为了解决这个问题，出现了一个变体叫：Token Bucket Policing（令牌桶监管** 它的做法是：如果桶里没有足够的 token，**直接丢包**，不等了。这样就不需要每个流一个队列，但代价是：流量不合规就直接丢。
### In traffic scheduling, what is the difference between policing and shaping?
-  **Policing 是“砍掉多余的”，Shaping 是“排队等一等”**
- Policing（流量监管）就是你设置了一个最大速率，比如 100kbps，一旦超过这个速率：
	•	多出来的数据包要么被丢弃，
	•	要么被“标记”为低优先级（比如变成 best-effort 类）
也就是说，不合格的包直接砍掉或降级处理，不管有没有空间让它等。
所以 Policer 的输出流速看起来像一条锯齿线（saw-toothed）：一会儿冲高，一会儿突然被截断，非常不平滑。
Shaping（流量整形）同样限制最大速率，但处理方式不一样：
	•	当数据速率超过设定值时，它不会直接丢包，而是把多余的包暂时存在缓冲区
	•	然后等网络“空一点”了再慢慢放出来
结果就是：虽然包可能会延迟一点点，但不会丢掉，整个流量变得更加平滑和可控。
Shaping 的输出速率曲线是平滑的“整形线”，不像 policing 那种暴力削减。
### How is a leaky bucket used for traffic policing and shaping?
- - 无论是 policing 还是 shaping，都可以用一个叫 Leaky Bucket（漏桶） 的算法实现。你可以把它想象成一个水桶：
	•	水以任意速度往桶里灌（代表突发数据）
	•	水只能以恒定速率往外滴（代表输出限速）
	•	桶满了还往里灌？那就溢出来了（非合格包 → 丢）
	所以：
	•	在 policing 里：桶满就直接丢包
	•	在 shaping 里：桶满时先等着，不丢