# The Network Layer: Control Plane

# Overview

---

> What is **control plane?**
> 
> - The control plane is the part of a network that controls how data packets are forwarded
>     
>     → creating a **routing table**
>     
- Understand principles behind network **control plane**
    - traditional routing algorithms
    - SDN controllers
    - network management, configuration
- Instantiation, implementation in the Internet
    - OSPF, BGP → the protocol to ****create the routing table****
    - OpenFlow, ODL and ONOS controllers
    - Internet Control Message Protocol: ICMP
    - SNMP

# Introduction

---

## Review

---

### Network-layer functions

---

1. **Forwarding** : move packets from router’s input to appropriate router output
    
    → Data plane
    
2. **Routing** : determine route taken by packets from source to destination
    
    → Control plane
    
- **Two approaches to structure network control plane**
    - per-router control (traditional)
    - logically centralized control (software defined networking)

---

### Per-router control plane

- Individual routing algorithm components in each and every router interact in the control plane

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled.png)

### SDN control plane

- **Remote controller computes**, installs forwarding tables in routers

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%201.png)

# Routing Protocol

---

> **Routing protocol goal** 
Determine “good” paths, from sending hosts to receiving host, through network of routers
> 

In this session, the two algorithms of common routing protocol used are introduced as follows

1. **The Link-State (LS) Routing Algorithm**
2. **The Distance-Vector (DV) Routing Algorithm**

## Graph abstraction

---

- A **graph** is used to formulate routing problems

<aside>
💡 **Definition and Notation**

1. Graph: $G = (V,E)$ (vertice, edge) 
2. Set of routers: $V$ = $\{ u, v, w, x, y, z \}$
3. Set of links:
$E$ = $\{ (u,v), (u,x), (v,x), (v,w), \\(x,w), (x,y), (w,y), (w,z), (y,z) \}$
4. Cost/distance of direct link connecting a and b: $c_{a,b}$
    - e.g., $c_{w,z}$ = 5, $c_{u,z}$ = ∞
</aside>

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%202.png)

### Routing algorithm classification

---

1. **Centralized / global routing algorithm**
    - Computes the least-cost path using **complete, global knowledge** about the network
    - **→ Link-state (LS) algorithms**
2. **Decentralized routing algorithm**
    - Calculation of the least-cost path is carried out in an **iterative, distributed manner by the routers**
    - → **Distance-vector (DV) algorithm**

## The Link-State (LS) Routing Algorithm

---

- **Centralized**:
    - network topology, link costs known to all nodes
    - accomplished via “**link state broadcast**” 
    → In each update of each router, the update information is sent to all other routers.
    - all nodes have same information
- **Iterative**
    - after k iterations, know least cost path to k destinations
    

The link-state routing algorithm present here is known as ***Dijkstra’s algorithm.***
The algorithm computes the least-cost path from one node to all other nodes in the network.

<aside>
💡 **Notation**

1. $c_{x,y}$: direct link cost from node x to y; 
            = ∞ if not direct neighbors
2. $D(v)$ : **current estimate of cost** of least-cost-path from **source to destination v**
3. $p(v)$   : **predecessor node** along the **current estimate path** from source to v
4. $N'$       : set of nodes whose least-cost-path definitively known
</aside>

The implementation of algorithm can be described as following,

```markdown
**Loop**
find a node in N' which have the minimum D(a)
	add a to N'
update D(b) for all b adjacent to a and not in N' :
	D(b) = min(D(b), D(a) + ca,b)
**until** N' = N
```

### Example

---

1. **In the initialization step**
look up all node which is neighbor to the source $u$, and give the corresponding cost; for those who are not $u$’s neighbor, give them infinite cost. 
2. **In first iteration**
look among those nodes not yet added to the set $N'$ and find that node with the least cost as of the end of the previous iteration.
    1. e.g, 
    For the cost from source u to w, which in initialization step is 5, compare the $D(w)=5$ and $D(v) + c_{v,w}=4$ and choose the small one (4) and iterative the result
3. **And so on ...**

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%202.png)

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%203.png)

### Dijkstra’s algorithm: oscillations possible

---

- When link costs depend on **traffic volume**, **route oscillations** possible.

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%204.png)

![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%205.png)

## The Distance-Vector (DV) Routing Algorithm

---

> The router only got the information **from it's neighbors** 
→ it doesn't need to get the global information of networking
> 
- It is **distributed** in that each node receives some information from one or more of its **directly** **attached** **neighbors**
- It is **iterative** in that this process continues on until no more information is exchanged between neighbors.
- Then the least costs are related by the celebrated **Bellman-Ford equation**,
    
    <aside>
    💡 **Bellman-Ford equation**
    
    - $d_x(y) = min_v\{c(x, v) + d_v(y)\}$
    </aside>
    
    ### Key idea of algorithm
    
    ---
    
    - **From time-to-time, each node sends its own distance vector estimate to neighbors**
    - when x receives new DV estimate from any neighbor, it updates its own DV using B-F equation:
    $d_x(y) = min_v\{c(x, v) + d_v(y)\}$
    - under natural conditions, the estimate $D_x(y)$ converge to the actual least cost $d_x(y)$
    
    ```markdown
    **Loop**
    1. Wait for (change in local link cost or msg from neighbor)
    	2. Recompute DV estimates using DV received from neighbor
    	3. **If** DV to any destination has changed, 
    		 **then** notify neighbors
    **Forever**
    ```
    
    ### Example
    
    ---
    
    ![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%206.png)
    
    ![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%207.png)
    
    ### link cost changes
    
    ---
    
    > “Good news travels fast.”
    > 
    > 
    > “Bad news travels slow.”
    > 
    - cost from large to small
        - link cost changes:
            - node detects local link cost change
            - updates routing info, recalculates local DV 
            - if DV changes, notify neighbors
    - cost from small to large – **count-to-infinity problem**

# Intra-ISP routing: OSPF

---

> In practice, it is too simplistic for all routers executing the same routing algorithm
> 
- For two important reasons:

1. **Scale**
    - billions of destinations
    - can’t store all destinations in routing tables!
    - routing table exchange would swamp links!

1. **Administrative autonomy**
    - Internet: a network of networks 
    - each network admin may want to control routing in its own network

### Internet approach to scalable routing

---

- Aggregate routers into regions known as **“autonomous systems” (AS)**
    - e.g. Chunghwa Telecom (中華電信) as an ISP manages the routers within its domain.
    
    1. **Intra-AS**
        - **a.k.a. “intra-domain”**
        - routing among **within** **same AS**
            - all routers in AS must run same intra-domain protocol
            - routers in different AS can run different intra-domain routing protocols
            - **gateway router**: at “edge” of its own AS, has link(s) to router(s) in other AS’es
    
    1. **Inter-AS** 
        - **a.k.a. “inter-domain”**
        - routing **among** **AS’es**
            - gateways perform inter-domain routing (as well as intra-domain routing)
            
    
    ---
    
- The routing algorithm running within an autonomous system is called an
**intra-autonomous system routing protocol.**

### **OSPF: Open Shortest Path First**

---

- Based on **link-state routing**
- IS-IS protocol (ISO standard, not RFC standard) essentially same as OSPF
- **Security**
    - all OSPF messages authenticated (to prevent malicious intrusion)

# Internet inter-AS routing: BGP

---

> To route the packets **through multiple ASs**, an **inter-autonomous system routing protocol** for ASs is needed.
> 
- In the Internet, **all ASs run the same inter-AS routing protocol**,
    
    → Called the **Border Gateway Protocol (BGP)**
    

## BGP

---

- Allows subnet to advertise its existence, and the destinations it can reach, to rest of Internet
- BGP provides
    - **eBGP (external)**: **obtain** subnet reachability information **from neighboring ASes**
    - **iBGP (internal)**: **propagate** reachability information to **all AS-internal routers**.
    - determine “good” routes to other networks based on reachability information and **policy**
    
    ### eBGP, iBGP connections
    
    ---
    
    ![Untitled](The%20Network%20Layer%20Control%20Plane%20bb1d311f51a64e7384f68ba2f4c9b2e6/Untitled%208.png)
    
    ### BGP basics
    
    ---
    
    - **BGP session**
        - two BGP routers (“peers”) exchange BGP messages over **semi-permanent TCP connection**
        - advertising paths to different destination network prefixes (BGP is a “path vector” protocol)
    - **Policy-based routing**
        - gateway receiving route advertisement uses import policy to
            - accept
            - decline path.
        - AS policy also determines whether to advertise path to other other neighboring ASes

## Why different Intra-AS and Inter-AS routing ?

---

- **Policy**
    - **inter-AS**: admin wants control over how its traffic routed, who routes through its network
    - **intra-AS**: single admin → no policy decisions needed
- **Performance**
    - **inter-AS**: policy dominates over performance
    - **intra-AS**: can focus on performance

# Software defined networking (SDN)

---

→ 使routing path 更為彈性

# ICMP: internet control message protocol

---