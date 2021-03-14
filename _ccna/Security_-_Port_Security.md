# Security - Port Security
## PORT SECURITY CONCEPTS AND CONFIGURATION
The engineer can use [port security](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/glossary.xhtml#glos_346) to restrict an interface so that only the expected devices can use it.

Port security identifies devices based on the source MAC address
![A network topology shows the source MAC addresses in frames sent by the PCs to the switches.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/06fig01.jpg)
- It examines frames received on the interface to determine if a violation has occurred.
- It defines a maximum number of unique source MAC addresses allowed for all frames coming in the interface.
- It keeps a list and counter of all unique source MAC addresses on the interface.
- It monitors newly learned MAC addresses, considering those MAC addresses to cause a violation if the newly learned MAC address would push the total number of MAC table entries for the interface past the configured maximum allowed MAC addresses for that port.
- It takes action to discard frames from the violating MAC addresses, plus other actions depending on the configured violation mode.

Those rules define the basics, but port security allows other options as well, including options like these:

- Define a maximum of three MAC addresses, defining all three specific MAC addresses.
- Define a maximum of three MAC addresses but allow those addresses to be dynamically learned, allowing the first three MAC addresses learned.
- Define a maximum of three MAC addresses, predefining one specific MAC address, and allowing two more to be dynamically learned.

- Use the switchport mode access or the switchport mode trunk interface subcommands, respectively, to make the switch interface either a static access or trunk interface.
- Use the switchport port-security interface subcommand to enable port security on the interface.
- (Optional) Use the switchport port-security maximum number interface subcommand to override the default maximum number of allowed MAC addresses associated with the interface (1).
- (Optional) Use the switchport port-security mac-address mac-address interface subcommand to predefine any allowed source MAC addresses for this interface. Use the command multiple times to define more than one MAC address.
- (Optional) Use the switchport port-security mac-address sticky interface subcommand to tell the switch to “sticky learn” dynamically learned MAC addresses.

![A network setup shows the trunk ports and the access ports.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/06fig02.jpg)

## Verifying Port Security

The `show port-security interface` command provides the most insight to how port security operates.

	SW1# show port-security interface fastEthernet 0/1
	Port Security              : Enabled
	Port Status                : Secure-shutdown
	Violation Mode            : Shutdown
	Aging Time                : 0 mins
	Aging Type                : Absolute
	SecureStatic Address Aging : Disabled
	Maximum MAC Addresses      : 1
	Total MAC Addresses        : 1
	Configured MAC Addresses  : 1
	Sticky MAC Addresses      : 0
	Last Source Address:Vlan  : 0013.197b.5004:1
	Security Violation Count  : 1

	SW1# show port-security interface fastEthernet 0/2
	Port Security              : Enabled
	Port Status                : Secure-up
	Violation Mode            : Shutdown
	Aging Time                : 0 mins
	Aging Type                : Absolute
	SecureStatic Address Aging : Disabled
	Maximum MAC Addresses      : 1
	Total MAC Addresses        : 1
	Configured MAC Addresses  : 1
	Sticky MAC Addresses      : 1
	Last Source Address:Vlan  : 0200.2222.2222:1
	Security Violation Count  : 0

The interface fa0/1 is in a secure-shutdown state, which means that the interface has been disabled because of port security.
You need to use one of these options to see the MAC table entries associated with ports using port security:

Lists MAC addresses associated with ports that use port security

	show mac address-table secure 

Lists MAC addresses associated with ports that use port security, as well as any other statically defined MAC addresses

	show mac address-table static 

Actions When Port Security Violation Occurs

|                                                                                       |         |          |                    |
| ---                                                                                   | ---     | ---      | ---                |
| Option on the switchport port-security violation Command                              | Protect | Restrict | Shutdown (Default) |
| Discards offending traffic                                                            | Yes     | Yes      | Yes                |
| Sends log and SNMP messages                                                           | No      | Yes      | Yes                |
| Disables the interface by putting it in an err-disabled state, discarding all traffic | No      | No       | Yes                |

## Port Security Shutdown Mode (Default)
It acts as if port security has shut down the port; however, it does not literally configure the port with the shutdown interface subcommand. Instead, port security uses the `err-disabled` feature.

- The switch interface state (per show interfaces and show interfaces status) changes to an err-disabled state.
- The switch interface port security state (per show port-security) changes to a secure-down state.
- The switch stops sending and receiving frames on the interface.

To recover from an err-disabled state, the interface must be shut down with the shutdown command and then enabled with the no shutdown command.

* * *

A global command to enable automatic recovery for interfaces in an err-disabled state caused by port security

	errdisable recovery cause psecure-violation 

A global command to set the time to wait before recovering the interface

	errdisable recovery interval seconds 

* * *

You can check the port security configuration on any interface with the show port-security or  show port-security interface type number.



	SW1# show port-security
	Secure Port  MaxSecureAddr  CurrentAddr  SecurityViolation  Security Action
	                (Count)      (Count)          (Count)
	---------------------------------------------------------------------------
	    Fa0/13              1            1                  1        Shutdown
	---------------------------------------------------------------------------
	Total Addresses in System (excluding one mac per port) : 0
	Max Addresses limit in System (excluding one mac per port) : 8192

	! The next lines show the log message generated when the violation occurred.

	Jul 31 18:00:22.810: %PORT_SECURITY-2-PSECURE_VIOLATION: Security violation occurred,

	caused by MAC address d48c.b57d.8200 on port FastEthernet0/13

	! The next command shows the err-disabled state, implying a security violation.
	SW1# show interfaces Fa0/13 status

	Port    Name                  Status        Vlan  Duplex  Speed  Type
	Fa0/13                        err-disabled  1      auto    auto  10/100BaseTX
	!

	! The next command's output has shading for several of the most important facts.

	SW1# show port-security interface Fa0/13
	Port Security              : Enabled
	Port Status                : Secure-shutdown
	Violation Mode              : Shutdown
	Aging Time                  : 0 mins
	Aging Type                  : Absolute
	SecureStatic Address Aging  : Disabled
	Maximum MAC Addresses      : 1
	Total MAC Addresses        : 1
	Configured MAC Addresses    : 1
	Sticky MAC Addresses        : 0
	Last Source Address:Vlan    : 0200.3333.3333:2
	Security Violation Count    : 1

The violations counter notes the number of times the interface has been moved to the err-disabled (secure-shutdown) state.

## Port Security Protect and Restrict Modes
These modes still discard offending traffic, but the interface remains in a connected (up/up) state and in a port security state of secure-up. As a result, the port continues to forward good traffic but discards offending traffic.
With protect mode, the only action the switch takes for a frame that violates the port security rules is to discard the frame. The switch does not change the port to an err-disabled state, does not generate messages, and does not even increment the violations counter.

Restrict mode provides a compromise between the other two modes. The port status would have also remained in a secure-up state; however, IOS would show some indication of port security activity, such as an accurate incrementing violation counter, as well as syslog messages.

	SW1# show port-security interface fa0/13
	Port Security              : Enabled
	Port Status                : Secure-up
	Violation Mode              : Restrict
	Aging Time                  : 0 mins
	Aging Type                  : Absolute
	SecureStatic Address Aging  : Disabled
	Maximum MAC Addresses      : 1
	Total MAC Addresses        : 1
	Configured MAC Addresses    : 1
	Sticky MAC Addresses        : 0
	Last Source Address:Vlan    : 0200.3333.3333:1
	Security Violation Count    : 97
	!
	! The following log message also points to a port security issue.
	!

	01:46:58: %PORT_SECURITY-2-PSECURE_VIOLATION: Security violation occurred, caused by

	MAC address 0200.3333.3333 on port FastEthernet0/13.
