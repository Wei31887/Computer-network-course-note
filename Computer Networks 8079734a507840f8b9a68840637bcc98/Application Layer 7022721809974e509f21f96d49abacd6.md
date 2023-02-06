# Application Layer

---

# Principles of Network Applications

---

## Network Application Architectures

---

> The application architecture is designed by the application developer and dictates how the application is structured over the various end systems.
> 
- Application developer will likely draw on one of the two predominant architectural:
    - **Client-server architecture**
    - **Peer-to-peer (P2P) architecture**
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled.png)
    
- client-server architecture
    - A classic example is the Web application for which an always-on Web server services requests from browsers running on client hosts.
- P2P architecture

### Processes Communicating

> A process can be thought of as a program that is running within an end system.
> 
- Within same host, two processes communicate using **inter-process communication** (defined by OS)
- Processes in different hosts communicate by exchanging **messages**

<aside>
💡 Client and Server Processes

In the context of a communication session between a pair of processes, 

- The process that initiates the communication is labeled as the **client**.
- The process that waits to be contacted to begin the session is the **server**.
</aside>

- **Socket** is the interface Between the Process and the Computer Network
    - A process **sends messages into, and receives messages from**, the network through a software interface called a **socket.**
    - It is also referred to as the **Application Programming Interface (API)** between the application and the network, since the socket is the programming interface with which network applications are built.
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%201.png)
    

### Addressing Processes

- To receive messages, process must have **identifier**
- Host device has unique **32-bit IP address**
- **Identifier** includes both I**P address** and **port numbers** associated with process on host.
- Example port numbers:
    - HTTP server: 80
    - mail server: 25

### Transport Services Provided by the Internet

> When an application developer create a new network application for the Internet, one of the first decisions he has to make is whether to use **UDP or TCP.** 
Each of these protocols offers a different set of services to the invoking applications.
> 
- **TCP Services**
    1. *Connection-oriented service.*
        1. This so-called handshaking procedure alerts the client and server, allowing them to prepare for an onslaught of packets.
    2. *Reliable data transfer service.*
        1. The communicating processes can rely on TCP to deliver all data sent without error and in the proper order.
- **UDP Services**
    - UDP is a no-frills, lightweight transport protocol, providing minimal services. UDP is connectionless, so there is no handshaking before the two processes start to com- municate.
    - UDP provides **no guarantee** that the message will ever reach the receiving process.

### Application-Layer Protocols

> An application-layer protocol defines how an application’s processes, running on different end systems, pass messages to each other.
> 
- The **types of messages** exchanged
    - request messages and response messages
- The **message syntax**
    - fields in the message and how the fields are delineated
- The the meaning of the information in the fields
- Rules for determining when and how a process sends messages and responds to
messages
- **The Web’s application-layer protocol, HTTP**, defines the format and sequence of messages exchanged between browser and Web server.

## The Web and HTTP

---

> The **HyperText Transfer Protocol (HTTP)**, the Web’s application-layer protocol, is at the heart of the Web. It is defined in [RFC 1945], [RFC 7230] and [RFC 7540].
> 

![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%202.png)

- **HTTP** uses **TCP**
- **HTTP is “stateless”**
    - Server maintains no information about past client requests

### Non-Persistent

- Process of connection based on non-persistent HTTP
    1. TCP connection opened 
    2. At most one object sent over TCP connection 
    3. TCP connection closed
- **Downloading multiple objects required multiple connections**

![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%203.png)

### Persistent Connections (HTTP 1.1)

- TCP connection opened to a server
- **Multiple objects can be sent over single TCP connection between client, and that server**
- TCP connection closed

### HTTP Message Format

- Two types of HTTP messages: **request**, **response**
- HTTP Request Message
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%204.png)
    
- HTTP Response Message
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%205.png)
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%206.png)
    

### User-Server Interaction: Cookies

> **Recall:** HTTP GET / response interaction is **stateless**
Web sites and client browser use cookies to maintain some state between transactions
> 

![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%207.png)

- What cookies can be used for:
    - authorization
    - shopping carts
    - recommendations
    - user session state (Web e-mail)

### Web Caching

> **Goal**: satisfy client request without involving origin server
> 

A **Web cache** — also called a ***proxy server*** — is a network entity that satisfies HTTP requests on the behalf of an origin Web server.

- The Web cache has its own disk storage and keeps copies of recently requested objects in this storage.
- Once a browser is configured, each browser request for an object is first directed to the Web cache.
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%208.png)
    
- **The process**
    1. The browser establishes a TCP connection to the Web cache and sends an HTTP request to the Web cache.
    2. The Web cache checks to see if it has a copy of the object stored locally. 
        1. If it does, the Web cache returns the object within an HTTP response message to the client browser.
        2. If the Web cache does not have the object, the Web cache opens a TCP connec- tion to the origin server
    3. When the Web cache receives the object, it stores a copy in its local storage and sends a copy, within an HTTP response message, to the client browser (over the existing TCP connection between the client browser and the Web cache).
- example

![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%209.png)

- The traffic intensity ***on the LAN*** is 
(15 requests/sec) * (1 Mbits/request) / (100 Mbps) = 0.15
- The traffic intensity ***on the access link***  is 
(15 requests/sec) * (1 Mbits/request)/(15 Mbps) = 1
- **The traffic intensity on the access link is approach to one, resulting in the infinite queue delay**

![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%2010.png)

suppose that the cache provides a hit rate of 0.4 for this institution.

- Because the clients and the cache are connected to the same high-speed LAN, 40 percent of the requests will be satisfied almost immediately, say, within 10 milliseconds
- the traffic intensity on the access link is reduced from 1.0 to 0.6
- Given these considerations, average delay therefore is
0.4 * (0.01 seconds) + 0.6 * (2.01 seconds)

### HTTP/2

## Electronic Mail in the Internet

---

Three major components: **user agents, mail servers, and the Simple Mail Transfer Protocol (SMTP)**

- Mail servers form the core of the e-mail infrastructure
- SMTP is the principal application-layer protocol for Internet electronic mail. It uses the reliable data transfer service of TCP to transfer mail from the sender’s mail server to the recipient’s mail server.

### SMTP

- the client SMTP (running on the sending mail server host) has TCP establish a connection to **port 25** at the server SMTP (running on the receiv- ing mail server host).

## DNS—The Internet’s Directory Service

---

> Distributed database implemented in hierarchy of many name servers
> 
> - application-layer protocol: hosts, name servers communicate to resolve names (address/name translation)

### Services Provided by DNS

- **Services**
    - Host aliasing
        - A host with complicated hostname can have one or more alias name.
        - DNS can be invoked by an application to obtain the canonical hostname for a supplied alias hostname as well as the IP address of host.
    - Mail server aliasing
    - Load distribution
        - For busy sites, which are replicated over multiple servers, with each having different IP address.
        - The DNS database contains this set of IP address.

### Overview of How DNS Works

- **Distributed, Hierarchical database**
    - Top-Level Domain (TLD) servers:
        - responsible for .com, .org, .net, .edu, .aero, .jobs, .museums, and all top-level country domains, e.g.: .cn, .uk, .fr, .ca, .jp
        - Network Solutions: authoritative registry for .com, .net TLD
        - Educause: .edu TLD
    - Authoritative DNS servers:
        - organization’s own DNS server(s), providing authoritative hostname to IP mappings for organization’s named hosts
        - can be maintained by organization or service provider
    
    ![Untitled](Application%20Layer%207022721809974e509f21f96d49abacd6/Untitled%2011.png)
    
- **DNS Caching**
    
    

### DNS Records and Messages

- **DNS Records**
- DNS: distributed database storing resource records **(RR)**
    
    ```markdown
    (Name, Value, Type, TTL)
    ```
    
    - `TTL`: the time to live of resource record
    - The meaning of `Name` and `Value` depend on `Type`
    - **type=A**
        - name is host name
        - value is IP address
    - **type=CNAME**
        - `name` is alias name for some “canonical” (the real) name
        - [www.ibm.com](http://www.ibm.com) is really servereast.backup2.ibm.com
        - `value` is canonical name
    - **type=NS**
        - DNS server
        - `name` is domain (e.g., [foo.com](http://foo.com/))
        - `value` is hostname of authoritative name server for this domain
    - **type=MX**
        - `value` is name of mailserver associated with `name`