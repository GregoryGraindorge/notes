# ACL
![A topology illustrates an example of the locations to filter packets from host A and B to a server.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/02fig01.jpg)
In short, to filter a packet, you must enable an ACL on an interface that processes the packet, in the same direction the packet flows through that interface.

## Types of IP ACLs
https://www.learncisco.net/courses/icnd-1/acls-and-nat/type-of-acls.html

- Standard numbered ACLs (1–99)
- Extended numbered ACLs (100–199)
- Additional ACL numbers (1300–1999 standard, 2000–2699 extended)
- Named ACLs
- Improved editing with sequence numbers

![A figure depicts the comparisons between the types of IP ACL.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/02fig03.jpg)

## ACL Logic
ACLs use first-match logic. Once a packet matches one line in the ACL, the router takes the action listed in that line of the ACL and stops looking further in the ACL.

E.g:

	access-list 1 permit 10.1.1.1
	access-list 1 permit 172.16.8.0 0.0.3.255
	access-list 1 permit any

Remember, all Cisco IP ACLs end with an implicit deny any concept at the end of each ACL. That is, if a router compares a packet to the ACL, and the packet matches none of the configured statements, the router discards the packet. Want to override that default behavior? Configure a permit any at the end of the ACL.

	R2# configure terminal
	Enter configuration commands, one per line. End with CNTL/Z.
	R2(config)# access-list 1 permit 10.1.1.1
	R2(config)# access-list 1 deny 10.1.1.0 0.0.0.255
	R2(config)# access-list 1 permit 10.0.0.0 0.255.255.255
	R2(config)# interface S0/0/1
	R2(config-if)# ip access-group 1 in
	R2(config-if)# end
	R2# show running-config
	! Lines omitted for brevity

	access-list 1 permit 10.1.1.1
	access-list 1 deny 10.1.1.0 0.0.0.255
	access-list 1 permit 10.0.0.0 0.255.255.255

	R2# show ip access-lists
	Standard IP access list 1
	    10 permit 10.1.1.1 (107 matches)
	    20 deny  10.1.1.0, wildcard bits 0.0.0.255 (4 matches)
	    30 permit 10.0.0.0, wildcard bits 0.255.255.255 (10 matches)
	R2# show access-lists
	Standard IP access list 1
	    10 permit 10.1.1.1 (107 matches)
	    20 deny  10.1.1.0, wildcard bits 0.0.0.255 (4 matches)
	    30 permit 10.0.0.0, wildcard bits 0.255.255.255 (10 matches)
	R2# show ip interface s0/0/1
	Serial0/0/1 is up, line protocol is up
	  Internet address is 10.1.2.2/24
	  Broadcast address is 255.255.255.255
	  Address determined by setup command
	  MTU is 1500 bytes
	  Helper address is not set
	  Directed broadcast forwarding is disabled
	  Multicast reserved groups joined: 224.0.0.9
	  Outgoing access list is not set
	  Inbound access list is 1
	! Lines omitted for brevity


## ACL commands
https://www.cisco.com/c/en/us/support/docs/ip/access-lists/26448-ACLsamples.html

To manage `acl`, we can use 2 syntax. The old or the new one. The new one starts with the `ip` keyword.

### Global commands

	show ip access-list
	show ip access-list <access-list number or name>

### Creating extend acl

	conf t
		ip access-list extended <list-name>

### Assign an ACL

	conf t
		int gig 0/0
			ip access-group <list-name or number> in/out

### Removing ACL

	conf t
		int gig 0/0
			no access-group <acl>

### Remove an entry inside an ACL

	show ip access-list <list-name>
	conf t
		ip access-list <access-list>
			no <num of rule>

## Example 2-4 Creating Log Messages for ACL Statistics

[Click here to view code image](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch02_images.xhtml#exm02_04)

	R1# show running-config
	! lines removed for brevity
	access-list 2 remark This ACL permits server S1 traffic to host A's subnet
	access-list 2 permit 10.2.2.1 log
	!
	interface F0/0
	ip access-group 2 out

	R1#

	Feb 4 18:30:24.082: %SEC-6-IPACCESSLOGNP: list 2 permitted 0 10.2.2.1 -> 10.1.1.1, 1

	packet

	Implicit deny for everything

	Standard based on source ip address only

	eg(extend): access-list 100 deny icmp host 10.16.10.1 (src) 192.168.0.1 (dest) eq 80 (dest port)

	accesss-list 100 permit any any
	eg(named): ip access-list extended Our-ACL
	deny tcp host 10.15.1.1 any eq 21

There are five different port numbers parameters for extend ACLs:

    - eq(equal)
    - ne(not equal)
    - lt(less than)
    - gt(greater than)
    - range
