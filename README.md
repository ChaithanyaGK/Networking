# Networking concepts

- [IP Addresses, Subnet Masks, and Default Gateways](#ip-addresses-subnet-masks-and-default-gateways)
- Network Adress Translator (NAT) and Routing

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

