Metro Ethernet (MetroE)

The main concept makes a Metro Ethernet service act like a big LAN switch.

MetroE provides a Layer 2 service by forwarding Layer 2 Ethernet frames. To do that, the SP often uses Ethernet switches at the edge of its network. Those switches are configured to do more than what you learn about Ethernet LAN switches for CCNA, but a LAN switch’s most fundamental job is to forward an Ethernet frame, so it makes sense for MetroE to use an Ethernet switch at the edge of the SP’s MetroE network.

![image.png](../_resources/929d67b076dd60abe2acc43dfbe8d1c3.png)

From an enterprise perspective, to use a Metro Ethernet service, each site needs to connect to the service with (at least) one Ethernet link. There is no need to connect each enterprise router to each other enterprise router directly with a physical link.

From the SP perspective, the SP needs to build a network to create the Metro Ethernet service.

To keep costs lower the SP puts a device (typically an Ethernet switch) physically near to as many customer sites as possible, in an SP facility called a point of presence (PoP).

![image.png](../_resources/4c8f037ae5017f7c907de993544e42a7.png)

The physical link between the customer and the SP is called an **access link** or, when using Ethernet specifically, an **Ethernet access link.**

Everything that happens on that link falls within the definition of the **user network interface**, or **UNI**.

![image.png](../_resources/effb3527b612c5c63af630469b217eef.png)

**MEF **(**www.mef.net**) defines the standards for Metro Ethernet, including the specifications for different kinds of MetroE services.

![image.png](../_resources/dd7f687d7d75f9a83410f459ca5870df.png)

**The Ethernet Line Service**, or E-Line, is the simplest of the Metro Ethernet services. The customer connects two sites with access links. Then the **MetroE** service allows the two customer devices to send Ethernet frames to each other.

![image.png](../_resources/ada4f5ea75757aa2c8f29f2b7b46fda5.png)

■ The routers would use physical Ethernet interfaces.
■ The routers would configure IP addresses in the same subnet as each other.
■ Their routing protocols would become neighbors and exchange routes.

By definition, an **E-Line service** (as shown in Figure 14-4) creates a point-to-point **EVC(Ethernet Virtual Connection)**, meaning that the service allows two endpoints to communicate.

![image.png](../_resources/16bd215bbd44ecfcf090ae51acbcba72.png)

**Ethernet LAN Service (Full Mesh) **

One E-LAN service allows all devices connected to that service to send Ethernet frames directly to every other device, just as if the Ethernet WAN service were one big Ethernet switch.

An E-LAN service connects the sites in a full mesh.

![image.png](../_resources/816e017b5b77f50f5129e724067fd8bb.png)

![image.png](../_resources/88585028416ee8ffa790bed2069695bb.png)
**
**
**Layer 3 design with E-Line Service**

Every E-Line uses a point-to-point topology. As a result, the two routers on the ends of an E-Line need to be in the same subnet.

![image.png](../_resources/09dc59fe207e8fc8a5708ca2c032f137.png)

**Layer 3 design with E-LAN Services**

All edges routers share the same subnet.

![image.png](../_resources/cfb5a99bd792e478f0c08958d549b0cd.png)