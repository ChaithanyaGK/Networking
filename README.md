# Networking concepts

- [IP Addresses, Subnet Masks, and Default Gateways](#ip-addresses-subnet-masks-and-default-gateways)
- [Network Address Translation (NAT) and Routing](#network-address-translation-nat-and-routing)
- [Router vs Gateway](#router-vs-gateway)
- [Load Balancer](#load-balancer)

## IP Addresses, Subnet Masks, and Default Gateways
(http://www.networkcomputing.com/network-security/ip-addresses-subnet-masks-and-default-gateways/1835691346)

One of the most basic concepts of data networking is how devices connect and communicate within an IPv4 network. To understand this, we must look at the devices' unique IP addresses as well as the associated subnet mask and default gateway. With these three pieces of information, we know how a device communicates with others locally as well as across an entire network. In this article, I'll explain each aspect of a device's IP address configuration and how they work together in order to communicate with other devices on a network.

### IP addresses

Easily the most widely understood component of the TCP/IP configuration is the IP address. Every device connected to a network must have an unique IP address to differentiate it from the others. An IP address is similar to the unique telephone number on your home phone or mobile device. The difference is that it consists of four segments called octets that are separated by a period. The numbers within each octet range between 0 and 255. Here's an example of a typical IPv4 address:

`192.168.40.39`

No other device on your network -- unless you are using NAT -- will have the same IP address. Therefore,  for a device to communicate with another, the sending device must know the location of the destination before it can begin transmitting data. Depending on the locations of the source and destination devices as they relate to the subnet mask, the process of discovering the location of the destination device address will vary.

### Subnet masks

As the name indicates, the subnet mask is used to subdivide a network into smaller, more manageable chunks. And when I say "more manageable," I'm mostly referring to the amount of broadcast traffic that can be tolerated within a network. As described above, in order for a sending device to transmit data to a receiving device, the sender needs to know where the destination is. The destination will either be on the same subnetwork as the source, or on some other subnetwork.

In our example, let's assume that the source IP address is 192.168.40.15 while our destination IP address remains 192.168.40.39. In order to determine if the devices are on the same subnet, we need the subnet mask. The mask designates where the network boundaries reside. Here, our subnet mask is 255.255.255.0. This can also be written as a /24 network mask. To understand the IP subnetwork boundaries, let's look at the IP address and subnet mask for our destination device:

**192.168.040**.039

**255.255.255**.000

As you can see, the four octets of the IP address align with the four octets of the subnet mask. The 255 octets in the subnet mask tell us that the corresponding numbers in the IP address are static and never change. Therefore, we know that the first three octets -- 192.168.40 of our IP address -- designate the network portion of our destination IP. And since the fourth octet is 0, that means this is the host octet and that individual devices can be assigned any number from 1-254. If that's the case, then our source IP address of 192.168.40.15 means these two devices belong to the same subnet.

### Broadcasts

If the devices are in the same subnet, the mechanism used to determine the location of the destination device is the broadcast. A broadcast packet is a special packet on the subnetwork that's sent to every device on the subnetwork. The broadcast address is designated as 255.255.255.255. This is known as a limited broadcast because it is limited to the subnet that it originated from.

When a broadcast is received -- which is actually a broadcast MAC address that is local to that VLAN -- all devices within that subnet respond with their IP address and the physical MAC address of the network card. These two pieces of information are then placed into an address resolution protocol (ARP) table. The table is used to keep track of the location of devices on the same subnet. And ultimately, the ARP table is used to efficiently switch data on the same subnet at the data link layer of the OSI model.

As you can imagine, the larger the subnetwork is, the more broadcast and ARP traffic will consume your network. In our example, a 255.255.255.0 network mask can hold up to 254 different devices. The next largest network is 512, and then 1024. Depending on your network capacity, anything over 1024 hosts on a single subnet is going to create too much broadcast traffic. Instead, subnets are intended to shrink broadcast domains so that the broadcasts themselves have little to no impact on the performance of the network.

### Default gateways

I've covered how devices communicate using IP when the devices are on the same subnet. But what happens if they are on different networks? This is where the default gateway comes into play. As described previously, the source device and destination device are either on the same subnet or they're on different subnets. In this example, let's change our source IP address, while keeping the destination address the same. So now our source and destination address and subnet masks are:

192.168.**99**.15 255.255.255.0

192.168.**40**.39 255.255.255.0

Because we are still using /24 subnet masks, we know these two devices are in different subnets since the third octet for each is different. And because subnets are used to break up broadcasts, using the broadcast mechanism with an ARP table will not work in this situation. This is why we need a default gateway.

The default gateway is used as the destination of all traffic that is not on the same subnet. The gateway is a layer 3 device such as a router or multi-layer switch that is used to route traffic on a hop-by-hop basis. But for the purposes of this discussion, the only thing the end device needs to know is whether the data is on the same subnet. If it's not, the source device delivers traffic to the end device through the default gateway.

The default gateway always resides in the same subnet as the end device IP. The gateway can really be any unique address within the subnet itself, but most network administrators designate the first number of the subnet as the gateway. Therefore, 192.168.99.1 would be the default gateway of our source device given the fact that we have a 255.255.255.0 subnet mask

## Network Address Translation (NAT) and Routing

**NAT is to translate private IP into public IP or Vice Versa.**

**Example:**  
x.x.x.x=Public IP which NAT's to 192.168.1.10  
Let's say you have a website www.example.com whose public DNS points to x.x.x.x, and you have an IIS website hosted on 192.168.1.10 on your internal network.  Someone on the internet would open a browser type in www.example.com, which would look up the IP x.x.x.x.  It would request the webpage and hit your firewall where it would NAT to the internal IP 192.168.1.10, and return the webpage.

**Routing is the process of moving a packet of data from source to destination.**

**Example:**  
Let's say you have two sites connected by a router with Site1 on the subnet 192.168.1.x and Site2 on the subnet 192.168.2.x.  On Site1, the default route tells it to go to the internet.  However, let's say you want to connect from Site1 to a Server on Site2; In that case, you need a route on Site1's router that says anything on the subnet 192.168.2.x should be sent to the router at Site2.  Without that route, the traffic would take the default route and try to go over the internet

## Router vs Gateway

A **Router** is a Network device that forwards packets from one network to another when a packet comes in through one port, the router reads the address information on the packet and determines the right destination, then uses routing table or routing policy to direct the packet to the next network or next hop. A router is a device that is capable of sending and receiving data packets between computer networks, also creating an overlay network.

A **Gateway**, on the other hand, joins dissimilar systems. Gateway it is defined as a network entity that allows a network to interface with another network with different protocols. Gateways acts as a network point that acts as an entrance to another network. The gateway can also allow the network to connect the computer to the internet.

**Example:**  
Suppose you have a Windows 2000 network that’s using TCP/IP as its primary protocol. Because TCP/IP is also the primary protocol of the Internet, you could use a router to connect your network to the Internet. The router would ensure that:  
- Traffic intended for the local network doesn’t bleed onto the Internet.
- Traffic residing on the Internet that’s not specifically intended for your network stays on the Internet.

A gateway, on the other hand, joins dissimilar systems. The best example of a gateway would be a device that joins a PC network with a 3270 mainframe environment or a device that allows a Windows NT network to communicate with a NetWare network. Although a gateway can be used to reduce network traffic, it’s more often used to make communication possible in dissimilar environments.

Below table will help clearly differentiate between both  Router and Gateway:

| PARAMETER | ROUTER  | GATEWAY |
| --- | --- | --- |
| Terminology	| Network device that forwards packets from one network to another.Based on internal routing tables, routers read each incoming packet and decide how to forward it. Routers work at the network layer (layer 3) of the protocol	Device that converts one protocol or format to another. | A network gateway converts packets from one protocol to another. The gateway functions as an entry/exit point to the network. |
| Primary Goal	| Route traffic from one network to other.	| Translate from one protocol to other |
| Feature support	| Routers provide additional features like DHCP server, NAT, Static Routing, and Wireless Networking/IPv6 address , Mac address	| Protocol conversion like VoIP to PSTN or Network Access Control etc. |
| Dynamic Routing	 | Supported	| Not supported |
| Hosted on	| Dedicated Appliance (Router hardware)	| Dedicated/Virtual Appliance or physical Server |
| Related terms	| Internet Router, Wireless Router	| Proxy server, Gateway Router, Voice Gateway |
| OSI layer	| Works on Layer 3 and 4	| Works upto Layer 5 |
| Working principle	| Works by installing routing information for various networks and routes traffic based on destination address	| Works by differentiating what is inside network and what is outside network |

## Load Balancer
A load balancer is a device that acts as a reverse proxy and distributes network or application traffic across a number of servers. Load balancers are used to increase capacity (concurrent users) and reliability of applications. They improve the overall performance of applications by decreasing the burden on servers associated with managing and maintaining application and network sessions, as well as by performing application-specific tasks.

![Alt text](https://f5.com/Portals/1/what%20is%20load%20balancing.png)

Load balancers are generally grouped into two categories: Layer 4 and Layer 7. Layer 4 load balancers act upon data found in network and transport layer protocols (IP, TCP, FTP, UDP). Layer 7 load balancers distribute requests based upon data found in application layer protocols such as HTTP.

Requests are received by both types of load balancers and they are distributed to a particular server based on a configured algorithm. Some industry standard algorithms are:
- Round robin
- Weighted round robin
- Least connections
- Least response time

Layer 7 load balancers(also known as Application load balancers) can further distribute requests based on application specific data such as HTTP headers, cookies, or data within the application message itself, such as the value of a specific parameter.

Load balancers ensure reliability and availability by monitoring the "health" of applications and only sending requests to servers and applications that can respond in a timely manner.
