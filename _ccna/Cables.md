Cables

**10BASE-T -> 10 Mbps -> Two wire pairs**
**100BASE-T -> 100 Mbps -> Two wire pairs**

As a rule, Ethernet NIC transmitters use the pair connected to pins 1 and 2; the NIC receivers use a pair of wires at pin positions 3 and 6. LAN switches, knowing those facts about what Ethernet NICs do, do the opposite: Their receivers use the wire pair at pins 1 and 2, and their transmitters use the wire pair at pins 3 and 6.

**1000BASE-T -> 1000 Mbps -> Four wire pairs**

First, 1000BASE-T requires four wire pairs. Second, it uses more advanced electronics that allow both ends to transmit and receive simultaneously on each wire pair.

**Fiber**

The standards for 10 Gigabit Ethernet over Fiber allow for distances up to 400m.

10GBASE-S MM 400m
10GBASE-LX4 MM 300m
10GBASE-LR SM 10km
10GBASE-E SM 30km

Multimode -> the cable allows for multiple angles (modes) of light waves entering the core.

Singlemode -> uses a smaller-diameter core, around one-fifth the diameter of common multimode cables. To transmit light into a much smaller core, a laser-based transmitter sends light at a single angle.

Single-mode allows distances into the tens of kilometers, but with slightly more expensive SFP/SFP+ hardware.

To transmit between two devices, you need two cables, one for each direction.