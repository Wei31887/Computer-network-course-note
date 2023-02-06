# The Link Layer and Local Area Networks

# Introduction

---

> **link layer** has responsibility of transferring datagram from one node to **physically adjacent** node over a **link**
> 

<aside>
💡 **Terminology**

- **Nodes**: hosts and routers
- **Links**: communication channels that connect adjacent nodes along communication path
    - wired
    - wireless
    - LANs
- **Frame** (encapsulates datagram): **layer-2 (link-layer)** packet
</aside>

- **Transportation analogy**
    - trip from Princeton to Lausanne
        - limo: Princeton to JFK
        - plane: JFK to Geneva
        - train: Geneva to Lausanne
    - tourist = **datagram**
    - transport segment = **communication link**
    - transportation mode = **link-layer protocol**
    - travel agent = **routing algorithm**

## The Services Provided by the Link Layer

---

> The basic service of any link layer is to move a datagram from one node to an adjacent node over a single communication link
> 

Possible services that can be offered by a link-layer protocol include as follows

### Services

---

- **Framing**
    - **encapsulate datagram into frame**, adding header, trailer
- **Link access**
    - MAC addresses in frame headers identify source, destination (different from IP address!)
- **Reliable delivery between adjacent nodes**
- **Error detection**
    - Errors caused by signal attenuation, noise.
    - receiver detects errors, signals retransmission, or drops frame
- **Error correction**
    - Receiver identifies and **corrects bit error(s) without retransmission**
- **Half-duplex and full-duplex**
    - with half duplex, nodes at both ends of link can transmit, but not at same time

## Where Is the Link Layer Implemented?

---

- Link layer implemented in **network interface card (NIC)** or on a chip
    - Ethernet, WiFi card or chip
    - implements link, physical layer
- Attaches into host’s system buses
- Combination of hardware, software, firmware

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled.png)

# Error-Detection and -Correction

---

- **The bit-level error detection and correction**
    - Detecting and correcting the corruption of bits in a link-layer frame sent from one node to another physically connected neighboring node

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%201.png)

- Error detection not 100% reliable!
    - protocol may miss some errors, but rarely
    - larger EDC field yields better detection and correction

<aside>
💡 **Notation**

- **EDC**: error detection and correction bits (e.g., redundancy)
- **D**: data protected by error checking, may include header fields
</aside>

## Parity checking

---

### Single bit parity

---

- Detect single bit errors

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%202.png)

<aside>
💡 **Even parity**

Set parity bit so there is an **even** number of 1’s 
→ total bits (data+parity bits) is even of 1’s 

</aside>

### Two-dimensional bit parity

---

- Detect and **correct** single bit errors

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%203.png)

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%204.png)

## Cyclic Redundancy Check (CRC)

---

> An error-detection technique used widely in today’s computer networks is based on 
**cyclic redundancy check (CRC)** codes.
> 
- **D**: data bits (given, think of these as a binary number)
- **G**: bit pattern (**generator**), of r+1 bits (given)
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%205.png)
    
    ### Example
    
    ---
    
    > Data bits $D = 101110$
    > 
    > 
    > Generator ($r+1$ bits) $G  = 1001, r = 3$
    > 
    
    ---
    
    - CRC generation
        1. First, add **3 bits of 0’s ($r=3$)** after the data 
        $D → D = 101110\underline{000}$
        2. $D$ divided by $G =1001$
        and get the reminder  $R = \underline{011}$
        3. Actually sent bit pattern 
        $= <D+R> = 101110\underline{011}$
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%206.png)
    
    ---
    
    - CRC verification
        1. Divide the CRC code <D,R> by G
            1. If zero remainder → non error
            2. If non-zero remainder → error detected!
    
    ![截圖 2023-01-06 14.05.40.png](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/%25E6%2588%25AA%25E5%259C%2596_2023-01-06_14.05.40.png)
    

# Multiple Access Links and Protocols

---

## Introduction

---

### Multiple access links

---

- two types of “links”
1. point-to-point
    - point-to-point link between Ethernet switch, host
    - PPP for dial-up access
2. **Broadcast (shared wire or medium)**
    - old-fashioned Ethernet
    - upstream HFC in cable-based access network
    - 802.11 wireless LAN, 4G/4G. satellite

### Multiple access protocol

---

> **Multiple access protocols** are which nodes regulate their transmission into the **shared broadcast channel**
> 
- Two or more simultaneous transmissions by nodes
    - **collision** if node receives two or more signals at the same time
- Multiple access protocol as belonging to one of three categories
    1. **Channel partitioning protocols**
        - Divide channel into smaller “pieces” (time slots, frequency, code)
        - Allocate piece to node for exclusive use
    2. **Random access protocols**
        - channel not divided, allow collisions
        - “recover” from collisions
    3. **Taking-turns protocols**
        - nodes take turns, but nodes with more to send can take longer turns

## Channel Partitioning Protocols

---

### TDMA:  Time Division Multiple Access

---

- TDM divides time into **time frames** and further divides each time frame into **N time slots.**
- Access to channel in “rounds”
- Each station gets **fixed length slot** (length = packet transmission time) in each round
- Unused slots go idle
- Example: 6-station LAN, 1,3,4 have packets to send, slots 2,5,6 idle

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%207.png)

### FDMA:  frequency division multiple access

---

- Channel spectrum divided into frequency bands
- Each station assigned fixed frequency band
- Unused transmission time in frequency bands go idle
- Example: 6-station LAN, 1,3,4 have packet to send, frequency bands 2,5,6 idle

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%208.png)

## Random access protocols

---

- Random access MAC protocol specifies
    - how to **detect** **collisions**
    - how to **recover from collisions** (e.g., via delayed retransmissions)
- Example of random access MAC protocols:
    - ALOHA, slotted ALOHA
    - CSMA, CSMA/CD
    - CSMA/CA (wireless)
    
    ### Slotted ALOHA
    
    ---
    
    - **Assumptions**
        - All frames consist of exactly `L` bits
        - **Time is divided into slots of size L/R seconds (time to transmit 1 frame)**
        - nodes start to transmit only slot beginning
        - nodes are synchronized
        - if 2 or more nodes transmit in slot, all nodes detect collision
    - **Operation**
        - when node obtains fresh frame, transmits in next slot
            - **if no collision**: node can send new frame in next slot
            - **if collision**: node retransmits frame in each subsequent slot **with probability p until success**
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%209.png)
    
    - **Pros**
        - single active node can continuously transmit at full rate of channel
        - highly decentralized: only slots in nodes need to be in sync
        - simple
    
    - **Cons**
        - collisions, wasting slots
        - idle slots
        - nodes may be able to detect collision in less than time to transmit packet
        - **clock synchronization**
    - Efficiency of Slotted ALOHA
        - max efficiency = 1/e = 0.37
        - **at best: channel used for useful transmissions 37% of time!**
    
    ### Pure ALOHA
    
    ---
    
    - **unslotted Aloha**: simpler, no synchronization
        - when frame first arrives: transmit immediately
    - collision probability increases with no synchronization:
        - frame sent at t0 collides with other frames sent in [t0-1,t0+1]
    - **pure Aloha efficiency: 18% !**
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2010.png)
    
    ### CSMA
    
    ---
    
    - **Carrier Sense Multiple Access**
    - **simple CSMA**: **listen before transmit**
        - if channel sensed idle → transmit entire frame
        - if channel sensed busy → defer transmission
    - **CSMA/CD**: CSMA with **collision detection**
        - collisions detected within short time
        - colliding transmissions aborted, reducing channel wastage
        - **collision detection easy in wired, difficult with wireless**
            
            → The distance between a and b may be too far to detect the collision!
            
    - **Collision**
    
    ![CSMA](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2011.png)
    
    CSMA
    
    ![CSMA/CD](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2012.png)
    
    CSMA/CD
    
    - **Algorithm**
        1. NIC receives datagram from network layer, creates frame
        2. If NIC senses channel:
            - if idle → start frame transmission.
            - if busy → wait until channel idle, then transmit
        3. If NIC transmits entire frame without collision, NIC is done with frame !
        4. If NIC **detects another transmission** while sending: abort, send jam signal
        5. After aborting, NIC enters **binary (exponential) backoff**:
            - after **mth collision,** NIC chooses **$K$ at random** from **$\{0,1,2,...,2^m-1\}$**
            - NIC waits $K·512\;(\text{bit times})$, returns to Step 2
            - more collisions → longer back off interval
    - **CSMA/CD efficiency**
        - $T_{prop}$ = max prop delay between 2 nodes in LAN  ttrans = time to transmit max-size frame
        - efficiency = $\frac{1}{1 +5t_{prop} /t_{trans}}$
        - efficiency goes to 1
            - as $t_{prop}$ goes to 0
            - as  $t_{trans}$ goes to infinity
        - **Better performance than ALOHA**: and simple, cheap, decentralized!
    
    ### “Taking turns” MAC protocols
    
    ---
    
    - A remote controller
    - **polling**
        - master node “invites” other nodes to transmit in turn
    - token passing:
        - control token passed from one node to next sequentially.
        - token message

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2013.png)

# Local Area Networks (LANs)

---

## Link-Layer Addressing and ARP

---

### MAC address

---

- MAC (or LAN or physical or Ethernet) address
    - **Function**
        - used “locally” to get frame from one interface to another physically-connected interface (same subnet, in IP-addressing sense)
    - **48-bit MAC address** (for most LANs) burned in NIC ROM, also sometimes software settable
        - `1A-2F-BB-76-09-AD`
- Each interface on LAN
    - has unique 48-bit MAC address
    - has a locally unique 32-bit IP address
- **Portability** of MAC flat address
    - can move interface from one LAN to another
    - Recall:
        - IP address not portable: depends on IP subnet to which node is attached

### ARP

---

> Use ARP to determine interface’s MAC address with known IP address.
> 

- Address Resolution Protocol (ARP)
- **ARP table** → each IP node (host, router) on LAN has table
    - IP/MAC address mappings for some LAN nodes:
    `< IP address; MAC address; TTL>`
    - TTL (Time To Live): time after which address mapping will be forgotten (typically 20 min)

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2014.png)

- **ARP in action**
    
    <aside>
    💡 ARP is “plug-and-play”
    
    - nodes create their ARP tables without intervention from net administrator
    </aside>
    
    - Example: A wants to send datagram to B
        - B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
    1. A **broadcasts ARP query**, **containing B's IP address**
        - destination MAC address = FF-FF-FF-FF-FF-FF
        - **all nodes on LAN receive ARP query**
            
            → but only B replies
            
    2. B replies to A with ARP response, giving its MAC address
    3. A receives B’s reply, **adds B entry into its local (A’s) ARP table**

### Sending a Datagram off the Subnet

---

- walkthrough: sending a datagram from A to B via R
    - focus on addressing – at IP (datagram) and MAC layer (frame) levels
    - assume that:
        - A knows B’s IP address
        - A knows IP address of first hop router, R (how?)
        - A knows R’s MAC address (how?)
        
        ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2015.png)
        
1. A creates IP datagram with IP source A, destination B
2. A creates link-layer frame containing A-to-B IP datagram
    - R's MAC address is frame’s destination
        
        ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2016.png)
        
3. R determines outgoing interface, passes datagram with IP source A, destination B to link layer
4. R creates link-layer frame containing A-to-B IP datagram. Frame destination
address: B's MAC address

## Ethernet

---

- **Bus**: popular through **mid 90s**
    - all nodes in same collision domain (can collide with each other)
- **Switched**: prevails today
    - active link-layer 2 switch in center
    - each “spoke” runs a (separate) Ethernet protocol (nodes do not collide with each other)
    
    ### Ethernet frame structure
    
    ---
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2017.png)
    
    - **preamble**
        - used to **synchronize** receiver, sender clock rates
        - 7 bytes of `10101010` followed by one byte of `10101011`
    - **Unreliable, connectionless**
        - **Connectionless**: no handshaking between sending and receiving NICs
        - **unreliable**: receiving NIC doesn’t send ACKs or NAKs to sending NIC
            - data in dropped frames recovered only if initial sender uses higher layer rdt (e.g., TCP), otherwise dropped data lost
        - Ethernet’s MAC protocol: **unslotted CSMA/CD with binary backoff**

## Link-Layer Switches

---

- Switch is a **link-layer device**: takes an active role
- Switches **buffer** packets
- → **Switching**: A-to-A’ and B-to-B’ can transmit simultaneously, **without collisions**
    
    ### Filtering and Forwarding
    
    ---
    
    > **Q**: how does switch know A’ reachable via interface 4, B’ reachable via interface 5? 
    **A**: each switch has a switch table
    > 
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2018.png)
    
    ### Self-Learning
    
    ---
    
    - Self-learning
    - Switch **learns** which hosts can be reached through which interfaces
    - **Example**
        1. The switch table is initially empty.
        2. For each incoming frame received on an interface, the switch stores in its table
        3. The switch deletes an address in the table if no frames are received with that address as the source address after some period of time (the aging time)
    
    ### **Switches vs. routers**
    
    ---
    
    - **both are store-and-forward**
        - **Routers**: network-layer devices (examine network-layer headers)
        - **Switches**: link-layer devices (examine link-layer headers)
    - **both have forwarding tables**
        - **Routers**: compute tables using routing algorithms, IP addresses
        - **Switches**: learn forwarding table using flooding, learning, MAC addresses

## Virtual Local Area Networks (VLANs)

---

### Motivation

---

- Single **broadcast** domain
    - Scaling: all layer-2 broadcast traffic (ARP, DHCP, unknown MAC) **must cross entire LAN**
    - efficiency, security, privacy, efficiency issues
- Administrative issues
    - CS user moves office to EE - physically attached to EE switch, but wants to remain logically attached to CS switch
    
    ![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2019.png)
    

### Port-based VLANs

---

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2020.png)

- **Traffic isolation**: frames to/from ports 1-8 can only reach ports 1-8
    - can also define VLAN based on MAC addresses of endpoints, rather than switch port
- **Dynamic membership**: ports can be dynamically assigned among VLANs
- **Forwarding between VLANS** : done via routing (just as with separate switches)
- **Trunk port**: carries frames between VLANS defined over multiple physical switches

![Untitled](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2021.png)

### 802.1Q VLAN frame format

---

![Original Ethernet frame (top); 802.1Q-tagged Ethernet VLAN frame (below)](The%20Link%20Layer%20and%20Local%20Area%20Networks%2037f9ae8d1990463bb343e4a5850136a3/Untitled%2022.png)

Original Ethernet frame (top); 802.1Q-tagged Ethernet VLAN frame (below)