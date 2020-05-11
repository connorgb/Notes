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
* IPv4 uses 4-byte addresses
* IPv6 uses 16-byte addresses
*  IPv6 addresses are customarily written in eight blocks of four hexadecimal digits separated by colons, such as FEDC:BA98:7654:3210:FEDC:BA98:7654:3210
* Leading zeros do not need to be written
* A double colon, at most one of which may appear in any address, indicates multiple zero blocks. For example, FEDC: 0000:0000:0000:00DC:0000:7076:0010 could be written more compactly as FEDC::DC:0:7076:10
* In mixed networks of IPv6 and IPv4, the last four bytes of the IPv6 address are sometimes written as an IPv4 dotted quad address. For example, FEDC:BA98:7654:3210:FEDC:BA98:7654:3210 could be written as FEDC:BA98:7654:3210:FEDC:BA98:118.84.50.16
* Java will need to proces both the numeric address and the host name -- you can do this with the `java.net.InetAddress` class
* IPs can change over time (DHCP server), so don't store the IP address when saving application state
  * Check for it whenever you need it
* All IPv4 addresses that begin with 10., 172.16. through 172.31., and 192.168. are unassigned
  * These non-routable addresses are useful for building private networks that can’t be seen on the Internet
*  IPv4 addresses beginning with 127 (most commonly 127.0.0.1) always mean the local loopback address
  * these addresses always point to the local computer, no matter which computer you’re running on
    * The hostname for this address is often localhost
* In IPv6, 0:0:0:0:0:0:0:1 (a.k.a. ::1) is the loopback address
* The address 0.0.0.0 always refers to the originating host, but may only be used as a source address, not a destination
* The IPv4 address that uses the same number for each of the four bytes (i.e., 255.255.255.255), is a broadcast address
  * Packets sent to this address are received by all nodes on the local network, though they are not routed beyond the local network

### Ports
* HTTP uses port 80
* Ports 1 to 1023 are reserved for root access on Unix, but all programs may send data to these ports
  * Complete listing of assigned ports is in /etc/services

### Internet Address Blocks
* When a company or an organization wants to set up an IP-based network connected to the Internet, their ISP assigns them a block of addresses
  * Each block has a fixed prefix -- For instance if the prefix is 216.254.85, then the local network can use addresses from 216.254.85.0 to 216.254.85.255. Because this block fixes the first 24 bits, it’s called a /24. A /23 specifies the first 23 bits, leaving 9 bits for 2^9 or 512 total local IP addresses. A /30 subnet (the smallest possible) specifies the first 30 bits of the IP addresses within the subnetwork, leaving 2 bits for 2^2 or 4 total local IP addresses
  * The lowest address in all blocks is used to identify the network itself, and the largest address is a broadcast address for the network, so you have two fewer available addresses than you might first expect

### Network Address Translation
* In NAT-based networks most nodes only have local, non-routable addresses selected from either 10.x.x.x, 172.16.x.x to 172.31.x.x, or 192.168.x.x
* The routers that connect the local networks to the ISP translate these local addresses to a much smaller set of routable addresses
  * The router watches my outgoing and incoming connections and adjusts the addresses in the IP packets. For an outgoing packet, it changes the source address to the router’s external address (216.254.85.72 on my network). For an incoming packet, it changes the destination address to one of the local addresses, such as 192.168.1.12
* Eventually, IPv6 should make most of this obsolete

### Firewalls
* The hardware and software that sits between the Internet and the local network, checking all the data that comes in or out

### Proxy Servers
*  A machine that is prevented from connecting to the external network by a firewall would make a request for a web page from the local proxy server instead of requesting the web page directly from the remote web server
* Whereas firewalls generally operate at the level of the transport or internet layer, proxy servers normally operate at the application layer
  * The notable exception are SOCKS proxy servers that operate at the transport layer, and can proxy for all TCP and UDP connections regardless of application layer protocol
* Proxy servers can also be used to implement local caching
  * When a file is requested from a web server, the proxy server first checks to see if the file is in its cache. If the file is in the cache, the proxy serves the file from the cache rather than from the Internet. If the file is not in the cache, the proxy server retrieves the file, forwards it to the requester, and stores it in the cache for the next time it is requested
* Generally established protocols like HTTP, FTP, and SMTP are allowed to pass through proxy servers, while newer protocols like BitTorrent are not
  * In Java, it’s easy and often useful to create a new protocol that is optimized for your application. However, no proxy server will ever understand these one-of-a-kind protocols. Consequently, some developers have taken to tunneling their protocols through HTTP, most notably with SOAP. However, this has a significant negative impact on security
    * Standalone Java applications can indicate the proxy server to use by setting the `socksProxyHost` and `socksProxyPort` properties (if you’re using a SOCKS proxy server)
    * You can set system properties from the command line using the -D flag, like this:
    ```
    java -DsocksProxyHost=socks.cloud9.net -DsocksProxyPort=1080 MyClass
    ```

### Client Server Model
* Servers store large amounts of data, clients run program logic and UI
* Clients initiate conversations, servers wait for clients to start conversations
* In 2013, the most popular client/server system on the Internet is the Web
* An alternative is Peer to Peer, where either party can initiate
   * Java doesn't explicitly support P2P, but can accomodate it through design 

### Internet Standards
* 2 main orgs:
  * Internet Engineering Task Force
    * Informal, democratic body with open participation
    * TCP/IP, SMTP
  * World Wide Web Consortium (W3C)
    * Dues-paying member corporations
    * HTTP, HTML, XML

### IETF RFCs
* Request For Comments are IETF standards
  * Working documents are called Internet drafts

### W3C Recommendations
* In October 1994, the World Wide Web Consortium was formed as a vendor-controlled body that might be able to avoid the pitfalls that plagued the IETF’s efforts to standardize HTML and HTTP
* 5 levels of standards: note, working draft, candidate recommendation, proposed recommendation, and recommendation

## Streams
* 







