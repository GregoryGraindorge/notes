WAN linesundefined links etc...

**HDLC**

Because leased lines define only the Layer 1 transmission service, many companies and standards organizations have created data-link protocols to control and use leased lines. Today, the two most popular data-link layer protocols used for leased lines between two routers are High-Level Data Link Control (HDLC) and Point-to-Point Protocol (PPP).

All data-link protocols perform a similar role: to control the correct delivery of data over a physical link of a particular type.

ISO standard HDLC does not have a Type field, and routers need to know the type of packet inside the frame. So, Cisco routers use a Cisco-proprietary variation of HDLC that adds a Type field.

LAN1 - HDLC - LAN2 - PC1 - R1 - R2 - PC2

**Following the steps in the figure, again for a packet sent by PC1 to PC2’s IP address: **

**1. **To send the IP packet to Router R1 next, PC1 encapsulates the IP packet in an Ethernet frame that has the destination MAC address of R1.

**2. **Router R1 de-encapsulates (removes) the IP packet from the Ethernet frame, encapsulates the packet into an HDLC frame using an HDLC header and trailer, and forwards the HDLC frame to Router R2 next.

**3.** Router R2 de-encapsulates (removes) the IP packet from the HDLC frame, encapsulates the packet into an Ethernet frame that has the destination MAC address of PC2, and forwards the Ethernet frame to PC2.

**Ethernet as a WAN Technology **

For the first several decades of the existence of Ethernet, Ethernet was only appropriate for LANs.

As time passed, the IEEE improved Ethernet standards in ways that made Ethernet a reasonable WAN technology. For example, the 1000BASE-LX standard uses single-mode fiber cabling, with support for a 5-km cable length.

Today, many WAN service providers (SP) offer WAN services that take advantage of Ethernet. SPs offer a wide variety of these Ethernet WAN services, with many different names.

That works much like an Ethernet crossover cable—just over a WAN. In other words:

■ Logically, behaves like a point-to-point connection between two routers

■ Physically, behaves as if a physical fiber Ethernet link existed between the two routers

**Ethernet WAN:** A generic name to differentiate it from an Ethernet LAN.

**Ethernet Line Service (E-Line):** A term from the Metro Ethernet Forum (MEF) for the kind of point-to-point Ethernet WAN service shown throughout this book.

**Ethernet emulation:** A term emphasizing that the link is not a literal Ethernet link from end to end.

**Ethernet over MPLS (EoMPLS):** A term that refers to Multi-protocol Label Switching (MPLS), a technology that can be used to create the Ethernet service for the customer.