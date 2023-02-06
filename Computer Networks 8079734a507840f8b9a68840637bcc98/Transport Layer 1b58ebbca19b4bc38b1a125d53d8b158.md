# Transport Layer

---

# Introduction and Transport-Layer Services

---

> A transport-layer protocol provides for **logical communication** between application processes running on different hosts.
> 
> 
> Application processes use the logical communication provided by the transport layer to send messages to each other.
> 

## Overview

---

Internet makes two distinct transport-layer protocols available to the application layer.

1. **UDP** (User Datagram Protocol)
    - unreliable, connectionless service to the invoking application
2. **TCP** (Transmission Control Protocol)
    - reliable, connection-oriented service to the invoking application.
    - Using flow control, sequence numbers, acknowledgments, and timers
    - provides **congestion** control.
- Principle behind transport-layer services:
    - multiplexing (多工) and demultiplexing (解多工)
        
        ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled.png)
        
    - reliable data transfer
    - flow control
    - congestion control
- Transport protocols actions in end systems:
    - **sender** : breaks application messages into *segments* and passes to network-layer
    (we refer to the transport-layer packet as a *segment*.)
    - **receiver** : reassembles segments into message and deliver to application layer

## Relationship Between Transport and Network Layers

---

> **Transport-layer protocol** provides logical communication between *processes* running on different hosts.
> 

> **Network-layer protocol** provides logical- communication between *hosts*.
> 

## Multiplexing and Demultiplexing

---

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%201.png)

- **Multiplexing**
    - gathering data chunks at the source host from different sockets,
    - encapsulating each data chunk with header information to create segments
    - passing the segments to the network layer.
- **Demultiplexing**
    - delivering the data in a transport-layer segment to the correct socket. (according to the segment’s header)

<aside>
💡 Host uses **IP addresses** & **port numbers** to direct segment to appropriate socket

</aside>

![TCP/UDP segment format](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%202.png)

TCP/UDP segment format

### Connectionless demultiplexing (UDP)

<aside>
💡 **Recalls**:

- When creating socket, must specify **host-local** port #:
    
    ```markdown
    DatagramSocket mySocket1 = new DatagramSocket(12534);
    ```
    
</aside>

- When creating datagram to send into UDP socket, must specify
    - **destination IP address**
    - **destination port #**

- when receiving host receives UDP segment:
    - checks **destination port #** in segment
    - directs UDP segment to socket with that port #

<aside>
💡 In **UDP** protocol, IP/UDP datagrams with same dest. port #, but different source IP addresses and/or source port numbers will be directed to same socket at receiving host

</aside>

### Connection-Oriented Multiplexing and Demultiplexing (TCP)

- **Overview**
    - **TCP** socket identified by **4-tuple**:
        - source IP address
        - source port number
        - dest IP address
        - dest port number
    - demux: receiver uses all four values (4-tuple) to direct segment to appropriate socket
    - Server may support many simultaneous TCP sockets:
        - each socket identified by its own 4-tuple
        - each socket associated with a different connecting client

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%203.png)

## Connectionless Transport: UDP

---

In this section, we’ll take a close look at UDP, how it works, and what it does.

<aside>
💡 Characteristics of UDP

- Best effort” service, UDP segments may be:
    - lost
    - delivered **out-of-order** to app
- **Connectionless**
    - Protocol with UDP, there is **no handshaking** between sending and receiving transport-layer entities before sending a segment.
- **DNS** is an example of an application-layer protocol that typically uses **UDP**.
</aside>

### UDP Segment Structure

- The **UDP** **header** has only four fields, each consisting of two bytes. **(total 8 bytes)**

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%204.png)

### UDP Checksum

> The UDP checksum provides for **error** **detection**. detect errors (i.e., flipped bits) in transmitted segment
> 
- UDP at the sender side performs the 1s complement of the sum of all the 16-bit words in the segment
- with any overflow encountered during the sum being wrapped around.
    
    <aside>
    💡 **Wraparound**
    
    ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%205.png)
    
    </aside>
    

---

- Example
    - Suppose that we have the three 16-bit words:
    
    ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%206.png)
    
    - The sum of first two of these 16-bit words
        
        ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%207.png)
        
    - Adding the third word to the above sum gives and the overflow will be **wraparound**
    
    ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%208.png)
    
- For **sender**
    - Treat contents of UDP segment as sequence of **16-bit (2bytes)** integers.
    - Calculate the **checksum**: addition (one’s complement sum) of segment content
    - Checksum value put into UDP checksum field
- For **receiver**
    - compute checksum of received segment
    - check if computed checksum equals checksum field value:
        - not equal - error detected

# Principles of Reliable Data Transfer

---

> The **reliable data transfer protocol** provided to the upper-layer entities is that of a reliable channel through which data can be transferred. With a reliable channel, no transferred data bits are corrupted.
> 

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%209.png)

- **Issue**:
    - The layer below the reliable data transfer protocol may be unreliable

## Building a Reliable Data Transfer Protocol

---

- Incrementally develop sender, receiver sides of reliable data transfer protocol (rdt)
- Consider only unidirectional data transfer
    - but control info will flow in both directions
- Use **Finite State Machines (FSM)** to specify sender, receiver

### **Reliable Data Transfer over a Perfectly Reliable Channel: `rdt1.0`**

> In this condition which the underlying channel is completely reliable.
> 
> - no bit errors
> - no loss of packets

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2010.png)

1. Add header to the segment
2. Send it to the transport-layer
3. **Maintain the same state**

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2011.png)

1. Extract the segment 
2. Deliver to the application-layer
3. **Maintain the same state**

### **Reliable Data Transfer over a Channel with Bit Errors: `rdt2.0`**

> Underlying channel may flip bits in packet → Error may occur in the data transfer.
*But data would not loss during the transfer.*
> 
- Use **checksum** to detect the error
- How to recover from errors? → ***Ask for retransmit***
    - **Acknowledgements (ACKs)**: receiver explicitly tells sender that pkt received OK
    - **Negative Acknowledgements (NAKs)**: receiver explicitly tells sender that pkt had errors
        - sender retransmits pkt on receipt of NAK
        - such retransmission are known as **ARQ (Automatic Repeat reQuest) protocols.**

<aside>
💡 **Stop and Wait**

Sender sends one packet, then waits for receiver response

</aside>

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2012.png)

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2013.png)

### The fatal flaw in `rdt2.0`

### What happens if ACK/NAK corrupted?

- Sender doesn’t know what happened at receiver!
- Can’t just retransmit: possible duplicate

---

- **Solution**:
    - Sender adds **sequence number** to each pkt → use 0, 1 to represent the state
    - Receiver discards (doesn’t deliver up) duplicate pkt

### `rdt2.1`

- Sender
    
    ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2014.png)
    
- Receiver

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2015.png)

### `rdt2.2` : a NAK-free protocol

- Same functionality as `rdt2.1`, **but using ACKs only**

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2016.png)

### **Reliable Data Transfer over a Lossy Channel with Bit Errors: `rdt3.0`**

> Suppose now that in addition to **corrupting bits**, the underlying channel can **lose** **packets** as well (data, ACKs)
> 
- Approach
    - **Set the timer:** sender waits “reasonable” amount of time for ACK
    - **Retransmits** if no ACK received in this time

## Pipelined Reliable Data Transfer Protocols

---

### Operation performance of Stop-and-wait protocol (`rdt3.0`)

## Go-Back-N (GBN)

---

### Sender

- Sending “window” size up to N, consecutive transmitted but unACKed pkts
- **Cumulative ACK** : ACK(n) : ACKs all packets up to, including seq #n
    - Sender receiving ACK(n) → move window forward to begin at *n+1*
- **Timer for oldest in-flight packet**
- Timeout(n) : retransmit packet n and all higher seq # packets in window

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2017.png)

### Receiver

- Always send ACK for correctly-received packet so far, with highest **in-order** seq #
    - Receive the data in order
- On receipt of out-of-order packet:
    - **Discard** (don’t buffer): no receiver buffering!
    - re-ACK pkt with highest in-order seq #

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2018.png)

<aside>
💡 **Shortcoming**
A single packet error can thus cause GBN to **retransmit a large number of packets**.

</aside>

## Selective Repeat (SR)

---

> **Overview**
> 
> - SR receiver will acknowledge a correctly received packet whether or not it is in order.
>     - Need the **buffer space** for collect all packets
>     - Receiver stores out-of-order packets in a buffer when they are received.
> - Sender **maintains timer for each unACKed packet (pkt)**

### Sender

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2019.png)

- If next available sequence number is in window, then send the packet
- **Timeout(n)**
    - Resend packet n and restart timer
    - **Because each segment has its own timer**, retransmit one segment on one timeout.
- **ACK(n)** in `[sendbase, sendbase+N]`
    - Mark packet n as received
    - If n is the smallest unACKed packet, advance window base to the next smallest unACKed sequence number.

### Receiver

- Receive packet n in `[rcvbase, rcvbase+N-1]`
    - Send **ACK(n)**
    - If it is out-of-order: **buffer**
    - If it is in-order: **deliver**
        - (also deliver buffered, in-order packets),
        - advance window to next not-yet-received packet
- Receive packet n in `[rcvbase-N, rcvbase-1]`
    - ACK(n)
- otherwise:
    - ignore

### Selective repeat: a dilemma!

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2020.png)

# Connection-Oriented Transport: TCP

---

- TCP is said to be connection-oriented because before one application process can begin to send data to another, the two processes must first “handshake” with each other
- As part of TCP connection establish- ment, both sides of the connection will initialize many TCP state variables

## Overview

---

- **Point-to-point**
    - one sender, one receiver
- **Reliable, in-order *byte steam***
    - no “message boundaries"
    
    ![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2021.png)
    
- **Full duplex data**
    - Bi-directional data flow in same connection
    - **MSS**: Maximum Segment Size

- **Cumulative ACKs**
    - ie, Receiver send ACK 100 to sender, as it received all the data before the sequence number 100 → Ask for the data from sequence number 100 from sender.
- **Pipelining**
    - TCP congestion and flow control set window size
- **Connection-oriented**
    - Handshaking (exchange of control messages) initializes sender, receiver state before data exchange
- **Flow controlled**
    - sender will not overwhelm receiver

## TCP Segment Structure

---

- The **32-bit (4 bytes)** sequence number field and the **32-bit** acknowledgment number field
- The **16-bit (2 bytes)** receive window field is used for flow control.

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2022.png)

## TCP Round-Trip Time Estimation and Timeout

---

> TCP uses a timeout/retransmit mechanism to recover from lost segments and based on the estimation of round-trip time (RTT)
> 
- TCP timeout value
    - Longer than **RTT**, but RTT varies!
        - too short: premature timeout, unnecessary retransmissions
        - too long: slow reaction to segment loss
- RTT estimation
    - `SampleRTT`: measured time from segment transmission until ACK receipt
        - ignore retransmissions
    - `SampleRTT` values will fluctuate from segment to segment due to congestion in the routers and to the varying load on the end systems.

### Smooth the `SampleRTT`

- $EstimatedRTT = (1- \alpha)*EstimatedRTT + \alpha *SampleRTT$
    - exponential weighted moving average (EWMA)
    - influence of past sample decreases exponentially fast
    - typical value $\alpha$ = 0.125

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2023.png)

### Timeout Interval

- Timeout interval: `EstimatedRTT` + “safety margin”
    - Large variation in `EstimatedRTT`: want a larger safety margin
    - $TimeoutInterval = EstimatedRTT + 4*DevRTT$
- $DevRTT = (1-\beta)*DevRTT + \beta *|SampleRTT-EstimatedRTT|$

## Reliable Data Transfer of TCP

---

### TCP sender

- **Event of receiving data from application-layer**
    - Create segment with sequence number
    - **Sequence number** is byte-stream number of first data byte in segment
    - Start timer if not already running
        - Create the timer for the oldest unacknowledged segment
        - **Only have one timer** and it’s for the oldest unacknowledged segment
- **Event of timeout**
    - Retransmit segment that caused timeout
        - Only retransmit the oldest unacknowledged segment
    - Restart timer
- **Event of ACK received**
    - If ACK acknowledges previously unACKed segments
        - Update what is known to be ACKed
        - Start timer if there are still unACKed segments

### Retransmission scenarios of TCP

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2024.png)

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2025.png)

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2026.png)

### TCP Fast Retransmit

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2027.png)

<aside>
💡 Receipt of **three** duplicate ACKs indicates 3 segments received after a missing segment 
→ Losing segment is likely. 
→ So **retransmit**!

</aside>

## TCP Flow Control

---

> TCP provides a **flow-control service** to its applications to eliminate the possibility of the sender overflowing the receiver’s buffer.
> 
- Using a variable called `receive window (rwnd)` maintained by sender for providing flow control.

### Receiver

- `LastByteRead` : the number of the last byte in the data stream read from the buffer by the application process.
- `LastByteRcvd` : the number of the last byte in the data stream that has arrived from the network and has been placed in the receive buffer.

```markdown
LastByteRcvd – LastByteRead ≤ RcvBuffer
```

```markdown
rwnd = RcvBuffer – [LastByteRcvd – LastByteRead]
```

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2028.png)

- TCP receiver send free buffer space in `rwnd` field in TCP header

### Sender

```markdown
LastByteSent – LastByteAcked ≤ rwnd
```

- Sender limits amount of unACKed (“in-flight”) data to received `rwnd`

## TCP connection management

---

> Before exchanging data, TCP sender and receiver do the **“handshake”**
> 
> - Agree to **establish connection** (each host knowing the other willing to establish connection)
> - Agree on **connection parameters** (starting sequence number)

### 3-way handshake:

- **Connection**
    - **Step 1 (SYN from initiator) -** The client-side (**initiator**) request connection to the server-side (**receiver**) .
        - Initiator set the **SYN to 1**.
        - Initiator (client) randomly chooses an initial sequence number `**client_isn**`.
    - **Step 2 (SYN from receiver) -** The server-side (**receiver**) receive the connection request from client-side
        - Receiver set the **SYN to 1.**
        - Receiver (server) chooses its own initial sequence number `**server_isn**`.
        - The **ACK from receiver** is set to **`client_isn + 1`**.
    - **Step3 (ACK from initiator) -** The client-side (**initiator**) receive the server-side segment
        - The **ACK from initiator** is set to `**server_isn + 1`.**
        - And the **SYN return to 0.**
    - **Complete the connection**

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2029.png)

---

- **Closing Connection**
    - **Step 1 (FIN From Client) –** Suppose that the client application decides it wants to close the connection.
        - The client set the **FIN to 1** to the server
        - And enter the **FIN_WAIT_1** state.
            - In the **FIN_WAIT_1** state, the client waits for ACK from server.
    - **Step 2 (ACK From Server)**
        - Server sends **ACK** segment to the Sender (Client).
    - **Step 3 (Client waiting)**
        - In **FIN_WAIT_1** state, client waits for ACK from server.
        - The client enters the **FIN_WAIT_2** state when achieve the ACK.
        - In **FIN_WAIT_2** state, client waits for the segment with **FIN is set to 1**.
    - **Step 4 (FIN from Server)**
        - The server sends the **FIN set to 1** to the sender (Client)
    - **Step 5 (ACK from Client)**
        - Client send **ACK** to server and enter **TIME_WAIT** state

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2030.png)

## Principle of congestion

---

> Congestion:
- Informally: “too many sources sending too much data too fast for network to handle”
> 

### Scenario 1

- one router, infinite buffers
- input, output link capacity: R
- two flows
- no retransmissions needed

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2031.png)

### Scenario 2

- One router,
- **Finite** buffers
- Sender retransmits lost, timed-out packet
    - Application-layer input = application-layer output: $\lambda_{in} = \lambda_{out}$
    - Transport-layer input includes **retransmissions**: $\lambda_{in}^{'} = \lambda_{in}$
- 
- more work (retransmission) for given receiver throughput
    - unneeded retransmissions: link carries multiple copies of a packet
    - decreasing maximum achievable throughput

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2032.png)

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2033.png)

### Scenario 3

![Untitled](Transport%20Layer%201b58ebbca19b4bc38b1a125d53d8b158/Untitled%2034.png)

## TCP Congestion Control

---