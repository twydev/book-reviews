# Computer Networking, 6th Edition
*Kurose & Rose, 2013*

### 1. Computer Networks and the Internet
___
#### Two views of the Internet:
  1. a network of interconnected computing devices / hosts / end systems, with hardware and software components.
      - [End Sys] -> {Links, Switches, Routers] -> [ISPs] 
      - Running protocols TCP, IP, etc.
  2. an infrastructure that provides services to distributed applications.
      - [Distributed Apps] -> API Calls -> [over Internet] -> run program -> [Other End Systems]

#### Network protocol
> defines the format and order of messages exchanged between communicating entities as well as the actions taken on transmission / receipt of a message or other events.

#### Network structure
  1. Network Edge - hosts such as clients and servers.
  2. Access Network - connecting end devices to edge router.
      - DSL (digital subscriber line) - modem connects to telephone infrastructure to gain internet access. Uses twisted pair copper wire.
      - Cable - modem connects to cable television infrastructure to gain internet access. Uses coaxial cable.
      - FTTH (fiber to the home) - OLT connects to optical splitter, aggregating back to ISP's ONT. Uses fiber cables.
      - Ethernet - connects end systems to Ethernet switch to router. It is a LAN, uses Ethernet cable (twisted pair copper wire).
      - WiFi - connects end systems to APs to router. It is a wireless LAN, uses 802.11 technology.
      - 3G/4G/LTE - WAN technologies. Connects end systems wirelessly to internet through cellular telephony infrastructure.
  3. Physical Medium - Twisted pair copper wire, Coaxial cable, Fiber optics, Terrestrial radio channels.
  4. Network Core
      - Packet Switching - store-and-forward transmission, queuing delay, packet loss, forwarding tables, routing protocols.
      - Circuit Switching - FDM (frequency-division multiplexing), TDM (time-division multiplexing)
      Circuit switching pre-allocates use of transmission link which most of the time is less efficient than packet switching.
  5. Network of Networks
      - End systems connect to access ISPs
      - Access ISPs connect to regional ISPs
      - Regional ISPs connect to tier-1 ISPs (global transit ISPs)
      - Content provider networks, as large scale as tier-1 ISPs, has global presence and may be able to connect directly to any tiers of ISPs.
      - PoP (Point of Presence) is a group of routers at one location, to allow customer ISPs to connect to provider ISP network.
      - Multi-home is the arrangement for a customer ISP to connect to multiple provider ISP for better reliability.
      - Peer is the arrangement between same tier ISPs to directly connect and pass traffic to each others network for free.
      - IXP (Internet Exchange Point) is a meeting point where multiple ISPs can peer together.

#### Delays in the Network
> Nodal delay = Processing delay + Queuing delay + Transmission delay (Length/Rate) + Propagation delay
> Traffic Intensity affects Queuing delay = (average rate of packet arrival * length of packet) / transmission rate
> Packet Loss = packets arriving at a full Queue will be dropped
> End-to-end delay = Number of nodes * (Processing + Transmission + Propagation delays)
> Bottleneck Link = link between an end to end connection with lowest throughput (transmission rate)

#### Protocol Layering
- a layered architecture provides modularity, ease implementation of services provided by a layer
- the service model of a layer performs action in a layer to provide service and taps on the service of layers below
- OSI 7 layer is legacy
- each lower layer encapsulate the information from a higher layer as a *payload* and appends additional information to the packet header

> Application Layer: sends *messages* to applications distributed across end systems
> Transport Layer: transports *segments* between application endpoints (connection service)
> Network Layer: move *dataagrams* between hosts (has routing and addressing service)
> Link Layer: transmit *frames* over a link between two network nodes
> Physical Layer: sends individual *bits* across physical transmission medium

#### History of Internet
1. 1961-1972: Development of packet switching (ARPAnet)
2. 1972-1980: Proprietary Networks and Internetworking
3. 1980-1990: Proliferation of Networks (TCP/IP)
4. 1990: Internet Explosion (World Wide Web)
5. The New Millennium: Broadband, high-speed wireless, Social networks, Content Provider ISP, Cloud computing

&nbsp;

### 2. Application Layer
___
#### Principles of Network Applications
Network applications run on end systems and communicate with each other over the network.
Two predominant architectural paradigms in modern network applications:
1. Client-Server Architecture - an always-on host (Server) services requests from many other hosts (Clients).
2. P2P Architecture - direct communication between pairs of intermittenly connected hosts (Peers) with minimal reliance on dedicated servers. P2P is more cost effective and self-scalable, but it may not be ISP Friendly (asymmetrical consumer bandwidth subscriptions), has security risks, and lacks incentive for peers to volunteer resources.

> To be more specific, in the context of a communication session between a pair of processes, the process that initiates the communication (that is, initially contacts the other process at the beginning of the session) is labeled as the client. The process that waits to be contacted to begin the session is the server.
- A Socket is the interface between the application layer and thje transport layer within a host (API between application and the network).
- Multiple processes may run on a single host.
- Hosts are addressed and identified in the network using *IP address*.
- Processes on the host are identified using *port number*.

#### Transport Services Available to Applications
Application push messages through the socket and the transport-layer protocol on  the other side is responsible for getting the messages to the socket of receiving process. Services that can **possibly** be provided by transport-layer protocol includes:
1. Reliable Data Transfer - guarantee that data will be delivered to destination process. May not be critical for loss-tolerant apps (e.g. multimedia).
2. Throughput - guarantee that throughput is available at some specified rate. Good for bandwidth-sensitive apps (e.g. multimedia) but less critical for elastic apps (e.g. email, file transfer).
3. Timing - guarantee that messages do not take more than some specified time/delay to reach destination. Good for real-time applications (e.g. multiplayer game).
4. Security - ensure confidentiality between communicating proceses by encrypting all data transmitted.

TCP Services - connection-oriented service that provides reliable data transfer and can be extended to provide secure encrypted connection.

UDP Services - no-frills, lightweight, connectionless, with no guarantee that messages will reach the destination and no congestion control.

Timing and Throughput guarantees are *not* provided by today's internet protocol. Applications make use of clever designs to overcome these limitations and provide satisfactory service to time-sensitive requirements.

#### Application-Layer Protocols
Defines:
- Types of messages exchanged.
- Syntax of the various message types (fields in message and how fields are delineated).
- Semantics of the fields (meaning of the information in the fields).
- Rules for determining when and how a process sends messages and responds to messages.

#### App-layer Protocol - HTTP (HyperText Transfer Protocol)
Protocol of the World Wide Web, for the requesting and delivery of websites.
- Web page = a document consisting of objects.
- Object = a file (e.g. HTML, JPEG, Java applet, video clip).
- Most web pages consists of a base HTML file and serveral referenced objects.
- URL = reference to an object, consists of a hostname + object's path name.
- Web browser = client program that requests for web content.
- Web server = server program that delivers web content.
- HTTP = defines how web clients request web pages from web servers and how servers transfer web pages to clients. Uses TCP as underlying transport protocol.
- Stateless protocol = HTTP is stateless as the server does not remember any client states.

HTTP with Non-Persistent Connections - each TCP connection is closed after the server sends a response object and will not persist for transmitting other objects. Each new connection incurr an additional RTT (round-trip time) and TCP buffers. (but subsequent objects may be transmitted over *parallel* TCP connections).

HTTP with Persistent Connections - TCP connection is open and persists after server respond. Subsequent requests and responses can be sent over the same connection. These requests can be made back-to-back without waiting for replies to pending requests (**pipelining**).

HTTP Message Format (Pls Google for more details)
- Request message methods: GET, POST, HEAD, PUT, DELETE.
- Response message status: 200 OK, 400 Bad Request, 404 Not Found, 500 Internal Server Error etc.

Cookies -  mechanism to allow servers to identify users connecting and save user state information.
1. Client: make HTTP request to Server.
2. Server: creates ID for user, store (user ID:state info) into backend DB, send HTTP response with "set-cookie: ID" header line.
3. Client: stores ID in cookie file. Interact with website.
4. Server: saves user state changes in DB, with user ID as key.
5. Client: visits the same site in future, sends HTTP request with user ID attached (retrieve from cookie file).
6. Server: retrieves user state based on ID, restore user session.
7. Client: continue browsing from previous saved state.

Web Cache - also known as Proxy Server, keeps copies of recently requested web objects from the network and serves subsequent requests on behalf of the original web server. *Content Distribution Networks (CDN)* provides web caches as a service for websites to distribute contents by geographically localising traffic. Web caches use the *Conditional GET* HTTP requests to fetch updated web objects from the original server *If-Modified-Since* a certain date.
- improves response time for a client request
- reduce load on bandwidth bottleneck
- reduce overall web traffic of the network

#### App-layer Protocol - FTP (File Transfer Protocol)
Protocol to allow user to transfer files to or from a remote host.
- Runs on two parallel TCP connection, control connection and data connection.(control information sent *out-of-band* from data transmission. Note, HTTP and SMTP works in-band).
- Client authenticates and change remote directory through control connection.
- Server maintains user state information. Sends file over data connection and closes it.
- Control connection is persistent for a user session. Data connection opens and closes for each individual file transfer. Helps to limit number of concurrent connections to reduce server load.

#### App-layer Protocol - SMTP (Simple Mail Transfer Protocol)
Protocol to allow asynchronous communication between people. Messages include attachments, hyperlinks, HTML-formatted text and embedded photos. General flow of email exchange using SMTP:
1. User Agent: end user software, uses SMTP to push messages to sender's Mail Server.
2. sender's Mail Server: stores message in message queue. Opens TCP connection to receiver's Mail Server. Performs SMTP handshake and sends message that is in queue. (if mail bounces, sender's Mail Server will keep re-attempting for a limited time).
3. receiver's Mail Server: receives message. Stores into designated user mailbox.
4. receiver: retrieves message using some form of *Mail Access Protocol*.

SMTP Characteristics - it is a push protocol. Sender's Mail Server initiates TCP connection and pushes message to receiver's Mail Server. (Unlike HTTP, which is a pull protocol where client initiates TCP connection to download information from web servers).
- it is limited to use 7-bit ASCII format to encode the body of each message.
- it places all content objects into one message (whereas HTTP encapsulates each object in its own HTTP response message).

#### App-layer Protocol - Mail Access Protocols
Used to pull messages from mailbox in a Mail Server to a receiver's User Agent or local PC.
1. Post Office Protocol Version 3 (POP3): User Agent establish TCP connection to Mail Server. Goes through the following phases:
  - Authorisation, authenticate user based on credentials entered.
  - Transaction, User Agent retrieves messages and allows user to mark out or unmark messages for deletion and obtain mail statistics.
  - Update, Mail Server deletes marked messages.
  - POP3 does not store user state and requires explicit deletion of messages, simplifying POP3 server implementation.
  - POP3 has 2 modes, download-and-delete, which removes the message from Mail Server once a copy is saved by the User Agent on the local PC.
  - download-and-keep, which does not remove the message even after reading.

2. Internet Mail Access Protocol (IMAP): extends POP3 with more features (but makes both server and client implementation more complex).
  - Each message in the IMAP server is associated with a folder (all incoming messages are associated with INBOX by default).
  - User may create folders and move messages across user-created folders.
  - User may also search for messages matching specific criteria.
  - IMAP maintains user state information (names of folders and message association).
  - IMAP permit a User Agent to only retrieve the header of a message or just one part of a multipart MIME message. Useful for low-bandwidth connection to avoid downloading long or huge messages.

3. HTTP: web based access to email service.
  - Web browser is the User Agent, communicates with remote mailbox via HTTP.
  - Messages are retrieved from mailbox to display on browser using HTTP as well.
  - Messages are also sent from browser to sender's mail server using HTTP.
  - (but communication between sender's and receiver's mail servers is still via SMTP).

#### App-layer Protocol - DNS (Domain Name System)
This protocol enables the directory service of the internet, mapping hostnames to IP addresses.
- The DNS is a distributed DB implemented in a hierarchy of DNS servers.
- The protocol allows hosts to query the distributed DB to obtain IP addresses.
- It is commonly used by other app-layer protocols. E.g. HTTP:
  1. Browser extracts hostname from URL in HTTP request, passes to DNS client.
  2. DNS client sends query containing hostname to DNS server.
  3. DNS server responds with IP address that maps to hostname.
  4. DNS client passes IP address to Browser, which initiates TCP connection to server at IP address, and starts HTTP message exchange.
- DNS provides *Host Aliasing*: a host can have a canonical hostname that maps to the IP address, and other alias names that maps to the canonical hostname.
- DNS provides *Mail Server Aliasing*: mail servers of an organisation can have the same hostname as their web or app servers, as long as it is registered as an MX record.
- DNS provides *Load Distribution*: multiple replicated web servers can be mapped to one canonical hostname. DNS DB will return a set of IP addresses to the clients but rotates the ordering of the addresses within each reply. The clients typically sends HTTP request to the first IP address in the list, hence DNS rotation helps distribute traffic.

DNS Design and Characteristics:
- Uses **UDP** to send DNS query and responses.
- Prevents single point of failure, high traffic volume, distant centralised DB and maintenance issues by using a distributed, hierarchical DB:
  - Root DNS servers: there are 13 replicated servers acting like a single server for security and reliability purpose. provides mapping of Top-Level Domain (TLD) servers.
  - TLD servers: these servers are responsible for the mapping of authoritative DNS servers for organisations using the top-level domains (e.g. com, org, net, edu, gov).
  - Authoritative DNS servers: these servers are typically owned by organisation to provide mapping of IP addresses to the hostnames of their web servers or mail servers.
  - Local DNS servers: not part of the hierarchy of DNS servers. Typically provided by ISPs or organisation's network. These servers act as a proxy to forward query to DNS server hierarchy.
    - Recursive Queries: query requests that expects the next server in the hierarchy to obtain the IP address mapping on requester's behalf, and this request is recursively passed from server to server.
    - Iterative Queries: query requester obtains DNS record of the next server in the hierarchy to direct the query, requester iteratively query each relevant server until a desired mapping is obtained.
    - a DNS client typically performs a recursive query to local DNS server, which then performs iterative query to the DNS hierarchy.
  - Local DNS servers also caches DNS records to improve DNS performance and reduce DNS traffic.

DNS Records:
A resource record (RR) is a four-tuple containing (Name, Value, Type, TTL).
- For Type A record, Name = hostname, Value = IP address.
- For Type NS record, Name = domain, Value = hostname of authoritative DNS server.
- For Type CNAME record, Name = alias hostname, Value = canonical hostname.
- For Type MX record, Name = alias hostname, Value = canonical hostname of mail server.

DNS Message Format (Pls Google for more details)
- ID, Flags, # Qns, # Answer RR, # authority RR, # additional RR.
- Questions, Answers, Authority, Additional Information.

Inserting Records into DNS DB:
1. Register a domain name at a registrar (a commercial entity that verifies the uniqueness of domain name and enters domain name into DNS DB).
2. Provide hostnames and IP addresses of Primary and Secondary Authoritative DNS servers (the Registrar will enter the NS and A records for these 2 servers into the TLD Servers).
3. Ensure Type A record and MX record of the desired hosts are entered into the Authoritative DNS servers.

#### App-layer Protocol - Peer-to-Peer Applications
P2P File Distribution - Each peer can redistribute any portion of the file it has received to any other peers, assisting the server in the distribution process. Upload capacity of a centralised file sharing server is limited by the uplink of the file server. In a P2P architecture, upload capacity is determined by uplink of file server and each peers that are redistributing the file.

#### BitTorrent
A popular P2P protocol for file sharing.
A Torrent = the collection of all peers participating in the distribution of a particular file.
A Tracker = an infrastructure node in each torrent that tracks the peers participating in the torrent. New peers register with the Tracker and provide periodic update of its participation status.

How BitTorrent Works:
- A peer joins the torrent. Obtains a list of neighbouring peers from Tracker.
- Peer establish TCP connections with neighbouring peers and obtain a list of all chunks of file held by each neighbour.
- Peer takes *rarest first* approach and request for the rarest chunks of file among all neighbours (This helps the torrent to ensure that rarest chunks are quickly redistributed).
- Peer redistributes chunks to a selective subset of neighbours (trading partners) giving priority to those that have been supplying the peer with the highest rate (reciprocating behaviour). These subset of neighbours are considered *unchoked*.
- A randomly chosen neighbour will be *optimistically unchoked* every 30 seconds, if the neighbour is able to supply data at high rate it will be able to replace an existing trading partner (This ensures that peers with compatible uploading rates will eventually find each other to partner, and even peers with no chunks will be able to obtain some data due to the randomness and be able to start trading).

#### Distributed Hash Table (DHT)
Each peer in the P2PDHT will hold a small subset of (key, value) pairs. Any peers can query the DHT with a particular key to locate the value. Any peers can also insert new key-value pairs into the DHT.

How DHT Works:
- Each key in the DB is an N-bit integer (A hash function is used to generate the keys from relevant identifying data)
- Each peer in the system is also assigned an N-bit identifier. 
- Peers with identifier that is the *closest successor* to the keys will be storing those subset of key-value pairs.
- Circular DHT: 
  - each peer keeps track of the address of its immediate predecessor peer (next smallest peer ID) and its immediate successor peer (next bigger peer ID). (Peers sequence wraps around once the max value of N-bit identifier is reached).
  - When a peer wants to query the DHT for the value of a key, it will pass the query either to the predecessor or to the successor peer.
  - Each peer that receives the query will pass it on to the next, until one of the peer is able to answer the query with the desired value. (In other words the peers form an *overlay network*, a logical, circular arrangement, on top of the physical network).
  - Circular DHT reduce the amount of overlay information (about address of other peers etc.) that needs to be maintained, but in the worst case query scenario, all peers will need to be queried, which is not efficient.
- Circular DHT with Shortcuts:
  - Peers not only tracks the immediate predecessor and successor, but also a few other peers spread out across the circular overlay network (the shortcut neighbours).
  - Shortcuts reduce the number of peers a query has to travel in the worst case scenario.
- Peer Churn:
  - To handle peers leaving the system abruptly, each peers track their two immediate predecessors and two immediate successors.
  - Peers periodically ping and verify that their neighbours are alive.
  - If a peer leaves the network, its immediate predecessor and successor will detect this and communicate with each other to recognise that there is a gap and close it. This information is propagated to the next preceding and succeeding peer.
  - Similarly when a peer wants to join the network, this request and the new peer identifier is sents into the network. The closest successor to the new peer will inform the new peer all the required peer tracking information of the neighbours. With that, the new peer will announce its entry to the network to its predecessor.
*BitTorrent uses the Kademlia DHT to create a distributed tracker to map torrent identifiers (key) to all IP addresses of peers in the torrent (values).

&nbsp;

### 3. Transport Layer
___
#### Overview and Key Services of Transport Layer
- User Datagram Protocol (UDP): provides unreliable, connectionless service with transport layer multiplexing and demultiplexing.
- Transmission Control Protocol (TCP): provides multiplexing and demultiplexing, reliable data transfer, congestion control, connection-oriented service.
- Challenges for Transport Layer protocol design comes from the fact that it runs on top of an unreliable IP Network Layer that delivers data at best effort.

#### What is Multiplexing and Demultiplexing?
- Multiple processes operating on one same host (same IP address) may need to communicate with the network.
- All processes pass segments to the same Transport layer to deliver data to the network (Multiplexing).
- All segments designated to the host comes in from the IP layer and is filtered to the correct designated processes by the Transport layer (Demultiplexing).
- Transport layer identifies the process to pass segments correctly via sockets.
- Sockets are uniquely identified by a two-tuple (dest_IP_addr, dest_port) for UDP and a four-tuple (src_IP_addr, src_port, dest_IP_addr, dest_port) for TCP.
- As TCP sockets uses both source and destination information for identification, it is able to maintain connection between 2 communicating processes. For UDP, segments from all sources designated for the same receiving process will be passed to the same designating socket.
- All segments communicated will contain information of the communicating sockets.
- Application layer of the process retrieves the data encapsulated by the segment from the socket.

#### TCP and Web Server
- Web server using HTTP commonly listens on TCP port 80 or port 443 for incoming client connections.
- In multithreaded servers, process will pass the task to a thread after a TCP connection has been established.
- The thread will inherit the socket used to communicate with the client.
- All threads will be using a unique socket even when the main server process is listening on a single port 80 or 443 (due to 4-tuple socket).
- If the same client wants to make multiple connection to the server, it must use a different source port number.

#### Connectionless Transport: UDP
Advantages of using a lightweight, connectionless UDP for applications (such as DNS):
- *Finer application-level control over what data is sent and when to send*: Good for real-time application that wants to minimise transmission delay but can afford some data loss.
- *No connection establishment*: making application much faster without connection overheads.
- *No connection state*: application uses less resource as it does not need to track buffers, congestion control parameters, sequence numbers and acknowledgements etc. The same app server can serve more active clients as compared to TCP.
- *Small packet header overhead*: faster processing and communication.

UDP is used by routing table updates (RIP), network management (SNMP) and DNS as these protocols provide services that do not require the maintenance of communication state. An application can still have reliable data transfer and all other Transport layer features while using UDP as long as those logic are implemented at the application layer.

UDP Segment Structure (Pls Google for more details)
- src_port, dest_port, length (of header plus data).
- checksum (one's complement of sum of all 16-bit words in the segment. used to detect bitwise errors).
- checksum allows UDP to provide *end-to-end principle* in system design (providing error detection without depending on reliability of lower layers of the network).
- application data.
- UDP however do not have error correction. It is up to the application to program the logic of error handling.

#### (Concepts that help to build reliable data transfer)
- checksum for error detection
- receiver feedback via ACK or NACK to indicate error
- retransmission by sender for error correction, or for data transmission loss
- sequence number for each segments to allow receiver to identify retransmissions duplicates (duplicates will still be ACK as it may be the result of ACK lost in transmission. But the duplicates will not be passed to application layer).
- sequence number for ACK segments to allow sender to identify duplicated ACKs
- sender has countdown timer to retransmit segment if no ACK is received by the time expire
- pipelining, instead of stop-and-wait protocol, allows sender to transmit a number of segments before receiving any ACKs to better utilise the network link capacity and increase data throughput.
- senders and receivers need to buffer segments in order to implement pipelining.
- pipelined error recovery takes 2 approaches, both are *Automatic Repeat reQuest (ARQ)* protocol:
  - **Go-Back-N (GBN) protocol**, a sliding-window protocol that allows sender to at any time send up to a window size of N segments without waiting for ACKs.
  - When ACK for a sent segment has reached sender, window slides forward to start from that acknowledged segment and new segments are sent. (This protocol uses cumulative acknowledgement, meaning once an ACK for a particular segment is received, the sender can expect that all preceding segments have reached the receiver correctly)
  - If a time out occur, all unacknowledged segments in the window are retransmitted (go-back-N). Timer restarts once an ACK is received but there are still unacknowledged segments in the window. Timer stops if there are no more unacknowledged segments.
  - **Selective Repeat (SR)**, unlike GBN, uses *individual* acknowledgement for each segments.
  - Sliding window of size N is still used to limit the sending and buffer the receiving of segments.
  - Segments are tracked individually.
  - Segments arriving out of order will still be ACK. Sender only retransmit particular segments that are lost or corrupted.
  - The sliding window must be less than or equal to half of the size of sequence number space to avoid collision and confustion of sequence number for two unique segments (e.g. the start and end of the sliding window which is wrapping around the sequence number space)
- Sequence numbers are not reused (for approximately 3 minutes, which is a maximum packet lifetime in the network) to ensure no segments that are still alive in the network from a previously closed connection can still propagate in the network and disrupt the current connection.

#### Connection-Oriented Transport: TCP
- connection-oriented, full-duplex service (2 way communication) that is always point-to-point (only 1 pair of sender/receiver)
- connection established via three-way handshake
- establishes **Maximum Segment Size (MSS)** based on the largest link-layer frame that can be sent by the host a.k.a. Maximum Transmission Unit (MTU) to ensure that the entire TCP segment with header encapsulated in an IP datagram can fit into a single link-layer frame.

TCP Segment Structure (Pls Google for more details)
- 16-bit src_port, dest_port, 32-bit sequence number, acknowledgement number, header length
- 1-bit flags (URG, ACK, PSH, RST, SYN, FIN)
- 16-bit receive window for control flow
- 16-bit checksum, urgent data pointer to indicate the last byte of urgent data
- Options field, to negotiate MSS
- Data field

**Sequence Numbers** are byte stream numbers for the number of bytes in the segment, not segment count.
**Acknowledgement Numnbers** are sent by receiver is always +1 from the sequence number of the latest segment received.
**Cumulative Acknowledgement** is used by TCP to only acknowledge bytes up to the first missing byte in the stream.

TCP RFCs do not impose any rules on implementation to handle **out-of-order segments**. Receiver can choose to discard segments or to keep them in buffer and wait for missing bytes to fill the gaps (more efficient).

**Round-Trip Time (RTT)** - each segment sent and corresponding ACK received is used to compute *SampleRTT*.
> EstimatedRTT = (1 - a) * EstimatedRTT + a * SampleRTT

> a = 0.125 (recommended)

This EstimatedRTT is an exponential weighted moving average (EWMA) with more weight on the latest SampleRTT that is assumed to contain the most updated network delay information.

> DevRTT = (1 - b) * DevRTT + b * |SampleRTT - EstimatedRTT|
> b = 0.25 (recommended)

This DevRTT measures the variability of RTT.

**Retransmission Timeout Interval** - should be larger than RTT but not too large such that there are unnecessary delays.
> TimeoutInterval = EstimatedRTT + 4 * DevRTT

**Doubling Timeout Interval** - Whenever a timeout event occurs, TCP retransmit segment and restart countdown timer with double the interval. Therefore retransmission increase timeout interval exponentially. However, whenever the timer is restarted due to receiving of correct ACK, the interval is set according to latest EstimatedRTT.

**Fast Retransmission** - helps TCP recovers from errors before timeout occurs.

| No. | Event | Action |
| --- | --- | --- |
| 1. | Segment with expected sequence number arrive. All previous segments already acknowledged. | Delayed ACK. Wait up to 500 msec. |
| 2. | Segment with expected sequence number arrive. Next in-order segment also arrive. | Immediately send cumulative ACK (represents both segments). |
| 3. | Segment arrive out-of-order. Gap detected. | Immediately send duplicate ACK, indicating that receiver is expecting lower end of the gap. |
| 4. | Segment arrive fills / partially fills a gap | Immediately send ACK only if segment fills lower end of gap. |

**GBN and SR hybrid** - TCP sender maintains an N sliding window to achieve pipelining, but will not go-back-N bytes on error. TCP will only send the next expected segment that is detected to be missing by the implicit mechanism. *Selective Acknowledgement* implementations of TCP allows receiver to ACK out-of-order segments.

**Flow Control**
- Flow control service prevents sender from overflowing receiver's buffer.
- Congestion control service, on the other hand, prevents congestion within the IP network.

> rwnd = RcvBuffer - (LastByteRcvd - LastByteRead)

- **Receive window (rwnd)**, maintained by receiver, indicates spare capacity in the receiver buffer that can take in new bytes from sender. This variable is sent to the sender.

> rwnd >= LastByteSent - LastByteAcked

- Sender keeps track of 2 variables, last byte sent and acked, and compares with the receive window to make sure total segments sent into the network will not overflow receiver's buffer.
- To prevent a scenario when receiver buffer is full (rwnd = 0) yet all the segments had been ACKed, which will cause the sender to lose communication with the receiver even when the buffer clears, TCP requires sender to keep sending one byte of data even if rwnd = 0.

**Connection Management**
- TCP **three-way handshake** starts with client-side sending a request segment with SYN flag and initial client_seq_no.
- Server respond with connection-granted segment with SYN ACK flags and initial server_seq_no, and put client_seq_no+1 into acknowledgement number field.
- Client completes handshake by responding with a segment with ACK flag, and acknowledgement number = server_seq_no+1.
- The final ACK segment may be piggybacked with application data in the payload.
- TCP **closes connection** by the client sending special segment with FIN flag.
- Server will ACK the special segment from client.
- Server will send its own shutdown segment with FIN flag.
- Client will ACK, and subsequently both hosts will deallocate all resources used for this connection.
- After Client receive shutdown segment from Server, it will have a waiting interval to resend ACK in case the ACK was lost. But the connection will formally close regardless of the server after a timeout (typically 30 secs to 2 mins).

**SYN Flood Attack and SYN Cookies**
- As TCP Server allocates resources for the connection variables and buffer once it receives the SYN request from a client, a flood of SYN request can cause a Denial of Service (DoS), if a botnet of clients keep sending SYN request to a Server without ever completing the handshake with a final ACK.
- To mitigate this, a SYN cookie can be used, instead of allocating resources to track the handshake state with the client.
- SYN cookie is the initial server_seq_no, derived by hashing the data (src_IP_addr,dest_IP_addr,src_port,dest_port) from the client SYN segment.
- Server will respond to the handshake with a SYN ACK and use SYN cookie.
- When Client ACK to complete the handshake, Server will check if the client is legitimate by performing the hash calculation to derive  the same SYN cookie from the segment information.
- Once validated, Server will allocate resources for this connection.

**TCP Reset Segment** - the RST flag is used in segment from receiver to inform the sender that the receiver is not running any application on that dest_port that can establish the requested connection.

**Congestion Control**
Congestion can occur for a TCP connection due to bottleneck links and finite buffers on intermediary routers.
- Congestion causes large queuing delay.
- Congestion results in retransmission of segments due to packets dropped at overflowing buffer or due to timeout.
- Congestion results in wastage of network link capacity to continually retransmit segments that had already been received but which ACK had not reach the sender.
- Congestion results in wastage of upstream network link resources used to transmit a packet all the way up to the point it was dropped due to overflowing buffer.

**Network-Assisted Congestion-Control Example: ATM ABR**
ATM refers to *Asynchronous Transfer Mode* and ABR refers to *Available Bit-Rate*.
- ATM takes a virtual-circuit (VC) oriented approach towards packet switching.
- A switch can track the transmission rate of senders and various VC state information.
- Switches can then inform the senders to take congestion control actions.
- Switches intersperse the data cells that they are transporting between senders and receivers with resource-management (RM) cells.
- RM cells can be modified by downstream switches.
- RM cells will reach the receiver and be redirected to the sender to adjust sending rate.
- RM cells can indicate presence of congestion, severity of congestion (none/mild/servere), and explicit minimum transmission rate supported by this VC of switches.
- With such feedbacks from the network, senders can take advantage of uncongested network to increase transmission rate, and also mitigate congestion.

**End-to-end Congestion-Control of TCP**
The TCP sender keeps track of a **congestion window (cwnd)** variable to constrain the sending rate.
> min{cwnd, rwnd} >= LastByteSent - LastByteAcked

- A lost segment implies congestion, hence TCP sender's rate should be decreased.
- An acknowledged segment indicates that the network is delivering the sender's segments to receiver, hence sender's rate can be increased.
- Bandwidth probing to keep adjusting sender's rate in response to arriving ACKs until a loss even occurs, then decrease the rate and re-adjust to find the optimal rate.

**TCP Congestion-Control Algorithm**
- **Slow Start Mode**: sender cwnd starts at 1 MSS (so sender's rate is approximately cwnd/RTT bytes/sec), and doubles for every successful ACK received.
- As a result, sending rate doubles every RTT and grows exponentially.
- If a loss event occur from timeout, set a new variable **slow start threshold (ssthresh)** = cwnd/2, half of the congestion window value that caused packet loss, and set the new cwnd to 1 MSS. Sender begins Slow Start process again.
- If cwnd reaches ssthresh level, TCP transitions to Congestion Avoidance mode.
- If three duplicate ACKs are detected, problem is less drastic, sender adjust the variable ssthresh = cwnd/2, then adjust cwnd = ssthresh + 3 MSS, to account for the 3 ACKs, then finally performs fast retransmission and enters Fast Recovery mode.
- **Congestion Avoidance mode**: sender cwnd increase by only 1 MSS for each successful ACK received.
- If a loss event occur from timeout, adjust the variable ssthresh = cwnd/2, and reset the new cwnd to 1 MSS.
- If three duplicate ACKs are detected, adjust the variable ssthresh = cwnd/2, then adjust cwnd = ssthresh + 3 MSS, then enter Fast Recovery state.
- **Fast Recovery mode**: value of cwnd increase by 1 MSS for every duplicate ACK received.
- Therefore while duplicated ACKs comes in, sender is still allowed to send new segments.
- When the missing segment is finally ACKed, the cwnd (which started growing at ssthresh + 3) is reset to cwnd = ssthresh, and sender returns to Congestion Avoidance mode.
- If a timeout occurs while in Fast Recovery, adjust ssthresh = cwnd/2, adjust cwnd = 1, and sender enters Slow Start state.

TCP Congestion Control is often reffered to as **Additive-Increase, Multiplicative-Decrease (AIMD)** form of control.
- The congestion window size (approximately sender's rate) forms a "saw tooth" graph over time.
- TCP's AIMD algorithm serves as a *distributed asynchronous-optimisation algorithm* that simultaneously optimise several aspects of user and network performance.
- Due to the "saw tooth" behaviour, average throughput of TCP connection is approximately = (3/4 * cwnd)/RTT

**Steady State Dynamics of TCP**
Given the congestion control behaviour of TCP, we may derived the average throughput. If an application requires high bandwidth reliable transmission at a certain minimum rate, we can derive the tolerable loss rate to achieve this using the throughput formula, or the cwnd required to achieve this.

**Fairness**
- Multiple TCP senders sending through the same bottleneck link will eventually reach a steady equilibrium state where each have an equal share of link capacity, after applying a few rounds of the AIMD algorithm.
- Intuitively, this is due to the fact that sender with a larger capacity share will decrease their congestion window by a larger amount for every cycle of AIMD, than senders who have a small capacity share and hence have small congestion window to start with.
- Eventually, senders with smaller congestion window size will "catch up" and equalise with the rest of the senders.
- But in practice, other factors can also disturb the fairness, such as connections with smaller RTT will tend to get a larger share of bottleneck link capacity, and pairs of senders and receivers may set up multiple parallel connection to get a larger capacity.
- And also, UDP is not fair by nature and can potentially crowd out TCP traffic.

**TCP Splitting Optimisation**
- A technique used to mitigate slow performance caused by TCP Slow Start by maintaining very large congestion window and better RTT to overcome congestion control.
- Commonly used by CDNs.
- Instead of having clients connect directly to web servers, they connect to CDN servers that are located nearer to the clients (lower RTT).
- CDN servers perform TCP Splitting, by maintaining a unique connection with the client, and another persistent connection with the data center that holds the web server.
- This persistent connection is able to maintain large TCP congestion window and better RTT performance due to better network infrastructure.
- Client will get to enjoy much better performance when communicating with a web server that is far away (sort of like a divide-and-conquer approach for the transport of data).

&nbsp;

### 4. Network Layer
___
#### Role of Network Layer
Role of the Network Layer is simply to move packets from a sending host to a receiving host. 3 main functions to achieve this:
1. Forwarding - Moving a packet from input link of router to the appropriate output link.
2. Routing - Routing Algorithms of network layer to determine the route / path taken by packets as they travel between sender and receiver.
3. Connection Setup - Routers along a chosen path from source to destination must connect with each other and set up states before packets can begin to flow.

**Network Service Model**
- Internet's network provides only best-effort service: no bandwidth guarantee, no loss guarantee, any ordering of packets, no timing guarantee, no congestion indicator.
- Alternative network architectures, such as ATM, provides Constant Bit Rate or Available Bit Rate indicators.

#### Virtual Circuit (VC) and Datagram Networks

**VC Networks**
- Connection service. Unlike Transport Layer, which connection is only established between hosts, VC connection is implemented on all routers along a path.
- A VC consists of a path between source and destination, a VC number for each link along the path, and forwarding table in each router.
- There are 3 phases in a VC:
  - VC Setup: Network Layer is informed of destination address by Transport Layer, determine path, determine VC number of each link, add entries in router forwarding tables. (sent via Signaling Messages).
  - Data Transfer: When a VC packet arrive, router will match the incoming interface and VC number to outgoing interface and VC number, *replace VC number in the packet* and forward it appropriately. (reduced complexity by not using network-wide unique VC number).
  - VC Teardown: Network Layer is informed of desire to terminate VC, inform both end systems, remove VC entries from router forwarding tables.

**Datagram Networks**
- Connectionless service. No set up of connection, routers do not need to maintain state information.
- Packets are stamped with destination address. Routers match destination address in forwarding table and pass packets to appropriate output interface.
- Addresses are matched to entries in the table using **longest prefix matching rule**.
- Routing Algorithms update forwarding tables (approximately one-to-five minute)
- As forwarding tables update, packets transmitted between two hosts may travel different paths.

#### Inside a Router

**Router Architecture**

| Management Control Plane (Software) | Routing Processor |
| --- | --- |
| Forwarding Data Plane (Hardware) | Input Port --> Switch Fabric --> Output Port |

**Input Processing**
- Input port makes Physical- and Link-Layer processing (line termination, decapsulation etc.)
- Input port checks packet version number, checksum, and TTL (and modify the latter 2 fields)
- Input port updates counters used for network management
- **Input port will also perform lookup** in forwarding table and push packets to appropriate Output Port through the Switch Fabric. (to avoids bottleneck at a centralised routing processor, each input port has a shadow copy of forwarding table generated by the routing processor)

**Switching**
- Switching Via Memory
  - Input Line Cards act like multiprocessors working on a shared memory with Output Ports.
  - Incoming packets are copied by input line cards to processor memory to perform lookup.
  - Input line card processor then writes packets to appropriate memory locations of output ports.
- Switching Via Bus
