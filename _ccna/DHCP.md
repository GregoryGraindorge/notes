# DHCP
## DHCP Concepts
DHCP uses the following four messages between the client and server.
DHCP uses port `67` and `68` over UDP.

- Discover: Sent by the DHCP client to find a willing DHCP server
- Offer: Sent by a DHCP server to offer to lease to that client a specific IP address (and inform the client of its other parameters)
- Request: Sent by the DHCP client to ask the server to lease the IPv4 address listed in the Offer message
- Acknowledgment: Sent by the DHCP server to assign the address and to list the mask, default router, and DNS server IP addresses

DHCP messages make use of two special IPv4 addresses that allow a host that has no IP address to still be able to send and receive messages on the local subnet:

> `0.0.0.0`: An address reserved for use as a source IPv4 address for hosts that do not yet have an IP address.

> `255.255.255.255`: The local broadcast IP address. Packets sent to this destination address are broadcast on the local data link, but routers do not forward them.

## Supporting DHCP for Remote Subnets with DHCP Relay
The `ip helper-address server-ip` subcommand tells the router to do the following for the messages coming in an interface, from a DHCP client:

![A network topology shows the IP addresses used between a host and a DHCP server.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/07fig01.jpg)
- Watch for incoming DHCP messages, with destination IP address 255.255.255.255.
- Change that packet’s source IP address to the router’s incoming interface IP address.
- Change that packet’s destination IP address to the address of the DHCP server (as configured in the ip helper-address command).
- Route the packet to the DHCP server.

## Information Stored at the DHCP Server
The following list shows the types of settings the DHCP server needs to know to support DHCP clients:

- `Subnet ID and mask`: The DHCP server can use this information to know all addresses in the subnet. (The DHCP server knows to not lease the subnet ID or subnet broadcast address.)
- `Reserved (excluded) addresses`: The server needs to know which addresses in the subnet to not lease. This list allows the engineer to reserve addresses to be used as static IP addresses. For example, most router and switch IP addresses, server addresses, and addresses of most anything other than user devices use a statically assigned IP address. Most of the time, engineers use the same convention for all subnets, either reserving the lowest IP addresses in all subnets or reserving the highest IP addresses in all subnets.

- `Default router(s)`: This is the IP address of the router on that subnet.

- `DNS IP address(es)`: This is a list of DNS server IP addresses.

## Configuring a router as a DHCP server
https://www.cisco.com/en/US/docs/ios/12_4t/ip_addr/configuration/guide/htdhcpsv.html

	enable

		conf t
		
			ip dhcp excluded-address <low-address> <high-address>
			ip dhcp pool <name>
			network 10.0.0.0 255.255.255.0
			domain-name <domain-name>
			dns-server 10.0.0.1
			default-router 10.0.0.1
		
			end
	
	show ip dhcp binding

## Configuring a Router as DHCP Client
Router R1 dynamically adds a default route to its routing table with the default gateway IP address from the DHCP message—which is the ISP router’s IP address—as the next-hop address. To recognize this route as a DHCP-learned default route, look to the administrative distance value of 254.

	R1# configure terminal
	R1(config)# interface gigabitethernet0/1
	R1(config-if)# ip address dhcp
	R1(config-if)# end
	R1#
	R1# show ip route static
	! Legend omitted
	Gateway of last resort is 192.0.2.1 to network 0.0.0.0

		S 0.0.0.0/0 [254/0] via 192.0.2.1
		
### Example 7-2 Switch Dynamic IP Address Configuration with DHCP

	Emma# configure terminal
	Enter configuration commands, one per line. End with CNTL/Z.
	Emma(config)# interface vlan 1
	Emma(config-if)# ip address dhcp
	Emma(config-if)# no shutdown
	Emma(config-if)# ^Z
	Emma#
	00:38:20: %LINK-3-UPDOWN: Interface Vlan1, changed state to up

	00:38:21: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up


	Emma# show dhcp lease
	Temp IP addr: 192.168.1.101  for peer on Interface: Vlan1
	Temp sub net mask: 255.255.255.0
	  DHCP Lease server: 192.168.1.1, state: 3 Bound
	  DHCP transaction id: 1966
	  Lease: 86400 secs,  Renewal: 43200 secs,  Rebind: 75600 secs
	Temp default-gateway addr: 192.168.1.1
	  Next timer fires after: 11:59:45
	  Retry count: 0  Client-ID: cisco-0019.e86a.6fc0-Vl1
	  Hostname: Emma

	Emma# show ip default-gateway
	192.168.1.1
