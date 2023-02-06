# Introduction to Computer Networks and the Internet

---

# What Is the Internet?

---

## A Nuts-and-Bolts Description

---

- All of devices are called **hosts** or **end systems** in Internet jargon**.**
- End systems are connected together by a network of communication links and packet switches.
    - coaxial cable, copper wire, optical fiber, and radio spectrum, etc.
- Different links can transmit data at different rates, with the transmission rate of a link measured in bits/second.
- End systems send data in segments and adds header bytes to each segment.
    - The resulting packages of information, known as **packets**
- **Routers** and **link-layer switches** are two most prominent types of packet switches.
- End systems access the Internet through **Internet Service Providers (ISPs)**
- End systems, packet switches, and other pieces of the Internet run protocols that control the sending and receiving of information within the Internet.
    - The **Transmission Control Protocol (TCP)** and the **Internet Protocol (IP)** are two of the most important protocols in the Internet.
- Internet standards are developed by the **Internet Engineering Task Force (IETF)**
- The IETF standards documents are called **Requests For Comments (RFCs)**.

## A Services Description

---

- The applications are said to be distributed applications, since they involve multiple end systems that exchange data with each other.
- End systems attached to the Internet provide a **socket interface** that specifies how a program running on one end system asks the Internet infrastructure to deliver data to a specific destination program running on another end system
    - Internet has a socket interface that the program sending data must follow to have the Internet deliver the data to the program that will receive the data.

# The Network Edge

---

- host = end system. Hosts are sometimes further divided into two categories:
    - **clients** and **servers**

## Access Networks

---

> **Wired, wireless communication links**
> 
> - **Access network** — the network that **physically connects** an end system to the first router (also known as the “**edge router**”).

![Figure. shows several types of access networks with **thick, shaded lines** and the settings.](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled.png)

Figure. shows several types of access networks with **thick, shaded lines** and the settings.

### Home Access: DSL, Cable, FTTH, and 5G Fixed Wireless

- Cable-based access
- Digital Subscriber Line (DSL)
- 

# The Network Core

---

## Packet Switching

---

- To send a message from a source end system to a destination end system, the source breaks long messages into smaller chunks of data known as packets.
- Between source and destination, each packet travels through communication links and packet switches (for which there are two predominant types, **routers** and **link-layer switches**)

<aside>
💡 **Transmission rate**

if a source end system or a packet switch is sending a packet of **L** bits over a link with transmission rate **R** bits/sec, 
then the time to transmit the packet is **(L / R)** seconds.

</aside>

<aside>
💡 **Packet transmission delay**

→ Time needed to transmit L-bit packet into link

→ **L**(bits) **/** **R**(bits/s)

</aside>

### Store-and-Forward Transmission

> Store-and-forward transmission means that the packet switch must receive the entire packet before it can begin to transmit the first bit of the packet onto the outbound link.
> 

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%201.png)

- The source has transmitted some of packet 1, and the front of packet 1 has already arrived at the router. Because the router employs store-and-forwarding, at this instant of time, the router cannot transmit the bits it has received; instead it must first buffer (i.e., “store”) the packet’s bits. Only after the router has received all of the packet’s bits can it begin to transmit (i.e., “forward”) the packet onto the outbound link.
- routers need to receive, store, and process the entire packet before forwarding.
    
    ![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%202.png)
    

### Queuing Delays and Packet Loss

> The packet switch has an **output buffer** (also called an **output queue**), which stores packets that the router is about to send into that link.
> 
- If an arriving packet needs to be transmitted onto a link but finds the link busy with the transmission of another packet, the arriving packet must wait in the output buffer. Thus, in addition to the store-and-forward delays, packets suffer output buffer **queuing** **delays**.
- An arriving packet may find that the buffer is completely full with other packets waiting for transmission. In this case, **packet loss** will occur.

### Forwarding Tables and Routing Protocols

- When a source end system wants to send a packet to a destination end system, the source includes the **destination’s IP address** in the packet’s header.
- Each router has a **forwarding table** that maps destination addresses (or portions of the destination addresses) to that router’s outbound links.
    - According to the IP address to find out the next router.
    - the router examines the IP address and searches its forwarding table, using this destination address, to find the appropriate outbound link.

## Circuit Switching

---

> End-end resources allocated to, reserved for “call” between source and destination
> 

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%203.png)

- Dedicated resources: **no sharing**
    - circuit-like (guaranteed) performance
- Circuit segment idle if not used by call

<aside>
💡 A circuit in a link is implemented with either
 **frequency-division multiplexing (FDM)** 
or **time-division multiplexing (TDM)**.

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%204.png)

</aside>

## A Network of Networks

---

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%205.png)

- **Internet Exchange Point (IXP)**, which is a meeting point where multiple ISPs can peer together.
- **Content provider networks** (e.g., Google, Microsoft, Akamai) may run their own network, to bring services, content close to end users

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%206.png)

# Delay, Loss, and Throughput in Packet-Switched Networks

---

## Overview of Delay in Packet-Switched Networks

---

> The most important of these delays are 
the **nodal processing delay**, **queuing delay**, **transmission delay**, and **propagation delay.**
these delays accumulate to give a **total nodal delay**.
> 

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%207.png)

- **Nodal processing delay**
    - The time required to examine the packet’s header and determine where to direct the packet is part of the processing delay.
- **Queuing delay**
    - At the queue, the packet experiences a queuing delay as it waits to be transmitted onto the link.
- **Transmission delay**
    - Time needed to transmit L-bit packet into link
    - **L(bits) / R(bits/s)**
- **Propagation delay**
    - The time required to propagate from the beginning of the link to router B is the propagation delay.
    
    > $d$ : length of physical link
    $s$ : propagation speed (~2x108 m/sec)
    $d_{prop} = d/s$
    > 
- **Total nodal delay**

$$
d_{nodal} = d_{proc} + d_{queue} + d_{trans} + d_{prop}
$$

## Queuing Delay and Packet Loss

---

### Queuing Delay

- Let $a$ denote the average rate at which packets arrive at the queue 
($a$ is in units of packets/sec).
- Then the average rate at which bits arrive at the queue is $La$ bits/sec.
- The ratio $La/R$, called the **traffic intensity**,
    
    ![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%208.png)
    
    - $La/R$ → $0$ : avg. queueing delay small 
    - $La/R$ → $1$ : avg. queueing delay large
    - $La/R>1$ : average delay infinite!
- Typically, the arrival process to a queue is random; that is, the arrivals do not follow any pattern and the packets are spaced apart by random amounts of time

### **Packet loss**

- Arrival rate to link (temporarily) exceeds output link capacity
- With no place to store such a packet, a router will drop that packet; that is, the packet will be lost.

## Throughput in Computer Networks

---

> **Throughput**: rate **(bits / time unit)** 
at which bits are being sent from sender to receiver
> 
- If the file con- sists of F bits and the transfer takes T seconds for Host B to receive all F bits, then the **average** **throughput** of the file transfer is F/T bits/sec.
- For the simple two-link network, the throughput is min{Rc, Rs}, that is, it is the transmission rate of the **bottleneck link.**

<aside>
💡 **Bottleneck link**
link on end-end path that **constrains** end-end throughput

</aside>

# Protocol Layers and Their Service Models

---

## Layered Architecture

---

- The network hardware and software that implement the protocols in layers. (Each protocol belongs to one of the layers)
- The **services** that a layer offers to the layer - the so-called **service model** of a layer.
    - (1) performing certain actions within that layer and by (2) using the services of the layer directly below it.

### Protocol stack

- **Application Layer**
    - The application layer is where network applications and their application-layer protocols reside
    - Packet of information at the application layer called as **message**.
- **Transport Layer**
    - TCP, UDP
    - Transport-layer packet called as **segment**.
    - The Internet transport-layer protocol (TCP or UDP) in a source host passes a transport-layer segment and a destination address to the network layer
- **Network Layer**
    - The Internet’s network layer is responsible for moving network-layer packets known as **datagrams** from one host to another
- **Link Layer**
    - Link-layer packets called as **frames**.
- **Physical Layer**
    - The protocols in this layer are again link dependent and further depend on the actual transmission medium of the link (for example, twisted-pair copper wire, single-mode fiber optics).

![The Internet protocol stack](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%209.png)

The Internet protocol stack

![Untitled](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%2010.png)

## Encapsulation

---

![Hosts, routers, and link-layer switches; each contains a different set of layers, reflecting their differences in functionality](Introduction%20to%20Computer%20Networks%20and%20the%20Internet%2089b4ee1c1a434096b577c87a9b0fd8a2/Untitled%2011.png)

Hosts, routers, and link-layer switches; each contains a different set of layers, reflecting their differences in functionality

1. An application-layer message ($M$) is passed from application layer to the transport layer.
2. The application-layer message and the transport-layer header information together consti- tute the **transport-layer segment.**
3. The transport layer then passes the segment to the network layer, which adds network-layer header information ($H_n$) such as source and destination end system addresses, creating a **network-layer datagram.**
4. The datagram is then passed to the link layer, which will add its own link-layer header information and create a **link-layer frame.**