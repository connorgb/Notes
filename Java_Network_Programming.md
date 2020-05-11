# Java Network Programming
## Basics
* Java was the first language built for network programming
* Each machine on the network is a node, and nodes that are full computers are called hosts
* Nodes have network addresses
  * The order (endian), or size of the address is not guaranteed to match a primitive data type in Java
* Addresses are assigned differently on different hardware
  * Ethernet uses preassigned manufacturer codes to avoid conflicts between manufacturers
* "Internet addresses are normally assigned to a computer by the organization that is responsible for it."
* ISPs choose what an "organization" calls it's computers
  * Regional internet registries (ARIN: American Registry for Internet Numbers) are provided IP addresses by the Internet Corporation for Assigned Names and Numbers 
* Names (like 'www.notes.com' or 'My Computer') are not necessarily tied to addresses
* All modern networks are packet switched; data travels in packets
  * This makes it possible to have packets from many exchanges on one wire
* A protocol defines how computers communicate
  * HTTP defines how browsers and servers communicate
  * IEEE 802.3 defines how bits are encoded as electrical signals on a particular type of wire
### Network Layers
* Each layer represents a level of abstraction between the physical hardware and the information being transmitted
* In theory, each layer only talks to the layers directly above and below it
  * Allows you to modify code in one layer without affecting others, as long as the interfaces stay the same
* Stacks of protocols
  * The top (applications) and bottom (Wifi, Ethernet) change for hosts, but the rest less so
* Application protocols are decoupled from the physics, topology of network
* We will use the standard TCP/IP four-layer model
  * Applications run in the application layer and only talk to the transport layer
* Physical path:
  * Browser talks to transport layer on local client machine 
  * Transport layer breaks the request into TCP segments, adds sequence numbers and checksums the data
  * The internet layer fragments the segments into IP datagrams that fit the local network
  * The host-to-network layer encodes the data as analog signals for the physical medium and sends it to be recieved by the host-to-network of the system to which it's addressed
  * The recieving remote system decodes the signal back into digital, and passes the resulting IP datagrams to the server's internet layer
  * The server's internet layer checks to see if the datagrams aren't corrupt and reassembles them
  * The server's transport layer checks to see if all data arrived and requests retransmission of any corrupt or missing pieces (this request will be handled by the client's transport layer), then writes them to a stream
  * A web server running in the server's application layer reads this stream and sends a response down through the layers for delivery to the web client
* 90% of your Java code will talk to the transport layer. The other 10% is in the transport layer talking to the application layer or the internet layer
* The OSI model splits the host-to-network layer into data link and physical layers and adds presentation and session layers between the application and transport layers
  * Java's network classes work only with TCP/IP networks, and always in the application or transport layers

### The Host-to-Network Layer (Physical)
* Defines how a network interface (Ethernet card or WiFi antenna) sends IP datagrams over its physical connection to the local network and the world
* Performance Considerations
  * fiber-optic connections, or satellite connections, or 3g data plan
  * APIs to communicate across networks are the same, thanks to the internet layer

### Internet Layer (Network)
* Bits and bytes are organized into packets and handled with an addressing scheme
* IPv4 header is 20 to 60 bytes long, with a payload up to 65,515 bytes (in practice they are from a few dozen bytes to more than 8 kilobytes)
* IPv6 has a larger header and up to 4 gigabytes of data
* All bits and bytes are big endian (most significant on the left)
* Also translates between types of Host-to-Network layers (Ethernet, WiFi, DSL, etc.)

### Transport Layer
* Ensures packets are recieved properly and in order
* Two main protocols:
  * Transmission Control Protocol
    * High overhead
    * Transmits in order
  * User Datagram Protocol
    * Low overhead
    * No guarantee of order

### Application Layer
* Lower layers define how data is transferred, the application layer decides what to do after

### IP, TCP, and UDP
* IP designed to allow multiple routes between two points, to route packets around damaged routers
* IP had to be platform independent
* TCP gives IP the ability to acknowledge reciept and request retransmission
* Protocols on IP include the Internet Control Message Protocol, which uses raw IP datagrams to relay error messages
  * Java does not support ICMP, only TCP and UDP
    * Can link to native code to run lower layer protocols like ICMP, ARP, RARP

### IP Addresses and Domain Names
* 

