# Vlan
## Trunk
### DTP (Dynamic trunking protocol)

DTP advertise itself every 30 seconds.

There are three modes to use in setting a switch port to trunk:

Trunk: This mode statically places the switch port as a trunk and advertises DTP packets to the other end to establish a dynamic trunk. Place a switch port in this mode with the command switchport mode trunk.

Dynamic desirable: In this mode, the switch port acts as an access port, but it listens for and advertises DTP packets to the other end to establish a dynamic trunk. If it is successful in negotiation, the port becomes a trunk port. Place a switch port in this mode with the command switchport mode dynamic desirable.

Dynamic auto (default on many switch): In this mode, the switch port acts as an access port, but it listens for DTP packets. It responds to DTP packets and, upon successful negotiation, the port becomes a trunk port. `It doesn't start the negotiation.` Place a switch port in this mode with the command switchport mode dynamic auto.

A trunk link can successfully form in almost any combination of these modes unless both ends are configured as dynamic auto.

|                   | Dynamic Auto | Dynamic desirable | Trunk                | Access               |
|                   | -            | -                 | -                    | -                    |
| Dynamic Auto      | Access       | Trunk             | Trunk                | Access               |
| Dynamic desirable | Trunk        | Trunk             | Trunk                | Access               |
| Trunk             | Trunk        | Trunk             | Trunk                | Limited Connectivity |
| Access            | Access       | Access            | Limited Connectivity | Access               |

### Commands

- Get a port mode configuration

		show interface gig 0/0 switchport

- Get trunk interfaces on the switch

		show interfaces trunk

- Show all vlan on the switch

		show vlan brief

- Set up the encapsulation mode for a trunk

		int gig 0/0
			switchport encapsulation dot1q

- Change the switchport mode

		int gig 0/0
			switchport mode dynamic-desirable

- No DTP negotiation

		int gig 0/0
			switchport nonegotiate

- Change the native vlan

		int gig 0/0 (trunk int)
			switchport trunk native vlan 100
