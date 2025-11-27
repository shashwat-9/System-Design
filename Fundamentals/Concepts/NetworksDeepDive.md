# Network Deep Dive

### Breakdown the Physical Layer

#### What are Networks & what problems do they solve?

Problems:
1. How do I read this message?
 - End-delimiter -> The part that defines the end of the message
 - Start-delimiter -> The part that defines the start of the message
 - time-interval -> Time Interval, for which a signal is considered. Like a series of ones will have some time interval for each one.
 - voltage -> The electronic signal.

Msg : Start-delimiter....Information....End-delimiter
 - The sender and receiver must agree on a message contract before starting the messaging.

So, the first thing in a network is the physical layer.

The process of sending messages from sender to receiver through intermediaries is called Routing.

 - What if the messaging is a conversation, that is:
 1. How frequently can 'A' send a message to 'B'.
 2. Can 'B' send a message back to 'A', as a response.
 3. Can 'B' send a message directly to 'A'.

### Conversation Settings
 1. Frequency
 2. Directionality
 3. Context

These settings are a part of the Behavioral layer.

So, there are three layers of Network:
1. Physical Layer
2. Routing Layer
3. Behavioural Layer

If we try to map the above with OSI model, then
1. Physical Layer -> maps to the physical layer of the OSI model.
2. Routing Layer -> DataLink Layer, Network Layer, Transport Layer
3. Behaviour includes Routing layer plus Session layer.

The other 2 layers not covered in the lecture are: Application and Presentation, as they are more on the coding part.

### Connecting to the Internet: ISPs, DNS and everything in between
 - If we type `Google.com`, the computer sends the request to its NIC(Network Interface Card).
 - A Network Interface Card (NIC) is a hardware component, typically a circuit board or chip, which is installed on a computer
so it can connect to a network.
 - DNS(Domain Name Server) contains the mapping of a business name with an IP.
 - ISP stands for Internet Service Provider.
 - The way DNS gets the IP address of a business is that the business configures it on the DNS that which IP does it host the business at.
 - The way packets are sent from 'A' to 'B', is via the set of routers in between.

### Internal routing: MAC addresses and NAT

#### Content Delivery Network
 -  CNDs are a regional Server put up by the business, from where a local client can connect to get the data, leading to less cost, 
and avoiding the issues due to caching that is having stale data.
 - The CDN servers are constantly up and running, getting in sync with the original business.

#### How does a business send back the data to sender
 - There are two methods for this: 
1. the router has a table that has the request and the sender details, so when the 
response combs back, the router sends it to the exact user.
2. Another way is that the router sends the request to the business attached with the information of the sender, so that the
response comes back with the details of the sender.

#### Protocols in sending the routing information
 - Network Address Translation (NAT), is a way by which Routers assigns Virtual IPs to the connected clients.
 - When a response comes back, the client knows who sent this request and then routes back the response to that virtual IP.
 - There are security concerns too, that what if someone changes the table of (request, requester) in the router.
 - The other way is to send the virtual IP in the request. When it comes back, the router resolves the request 
IP(virtual IP, as it is assigned by the router), and sends the data back to the requester.

### HTTP, WebSockets, TCP and UDP
#### Communication between Router to Server

##### HTTP is a client Server request
 - There is a clear definition of who is the client and server.
 - Every client request will have an ID to which the server responds to
 - What if we want to have a chat application? HTTP won't be ideal there, these could be the cases:
1. We have to create two connections, each with an individual client and server.
2. There will be a requirement of keeping these connections in sync with each other.

 - These problems in the chat application can be avoided by using a different protocol, that is WebSocket, which is peer2peer protocol
 - The Websocket is also built upon the HTTP and TCP protocol.
 - Many chat applications require other details like, 1. Presence 2. Contact list 3. Instant Messaging
 - For the above features, we can use the XMPP protocol, that is, Extensible Messaging and Presence Protocol (XMPP) is an
open XML technology for real-time communication, which powers a wide range of applications including instant messaging, presence and collaborations.


### TCP - Transmission Control Protocol
 - Here the sent packages receive acks, and therefore is a Guaranteed ordered Delivery protocol.
 - HTTP runs over TCP, and therefore inherits all the benefits of the TCP.


##### An idempotent method means that the result of a successful performed request is independent of the number of times it is executed.

### UDP - User DataGram Protocol
 - It's focussed on faster delivery, and therefore no acks or guaranteed delivery is there.

### REST, GRAPHQL, gRPC

#### Thrift
 - Thrift is a protocol developed by Facebook, that can aid communication between microservices in different languages.
 - When there are multiple services running in different languages, and thus are required to read objects in different lnaguages.
 - And there comes the concept of Thrift, that helps to read the same object in different langs.
 - It's a common language that can be read and converted by any language.

There are typically two ways to query any system:
#### 1. REST
 - REST have a Cacheable response.
 - REST is a stateless protocol.
 - REST APIs are stateless because, rather than relying on the server remembering previous requests, REST applications require
each request to contain all of the information necessary for the server to understand it.

#### 2. Graph QL
 - In this protocol, we send the columns that we require, the Service queries only those columns and sends us back.
 - It saves bandwidth and also isolates concerns.

#### GRPC
 - GRPC uses HTTP2.0
 - It is used for inter-service communication.

### Problem with HTTP: Head of Line Blocking
 - Earlier, without HTTP 2.0, there was a head of line blocking issue, where say we have to send 100s of unrelated packets, all of
them were ordered in sequence, irrespective of whether it's required or not, following HTTP protocol.
 - If anyone of the packets in the middle failed, the rest of the packets in line suffered due to retrial, even if they are not supposed to be sent in order.
 - What HTTP 2.0 came up with is that it ordered the packets together that are supposed to be ordered.
For example, 1,2,3,4,5 are packets to be sent, then in HTTP, 1->2->3->4->5 is the order of sending the packets.
But in HTTP 2.0, if 3,4,5 is independent of 1,2, and thus they are sent with a different stream ids. 1->2 is sent with the id 1, 3->4->5 is sent with stream id 2,
therefore the receiver knows that if packet 2 failed, we should still be processing packet 3 as it comes with a stream id 2.
 - But HTTP2.0 is an application layer protocol, and uses TCP beneath the surface, and thus the problem still exists. 
As TCP will send the packets one by one, and if one fails then keep retring until the current doesn't get processed completely.
 - This is solved by HTTP 3.0, which uses UDP beneath the surface. It uses UDP at the transport layer, but has all the features of the HTTP,
that is guaranteed and ordered delivery etc. HTTP3.0 uses `QUIC` + `UDP` protocol.

### Protocols for Video Transmission
 - Statelessness doesn't look like a good idea.
 - And for the real-time video streaming, we can go for UDP, but the there maybe cases like videos of movies, where the reliability is important and thus TCP can be used.
 - 

[Refer this for more on this](./Resources/networks.pdf)

