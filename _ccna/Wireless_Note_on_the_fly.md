Wireless (Note on the fly)

**RSSI**

**BSA **basic service area -> couverture reseau
**SSID **- Service Set Identifier - Not unique - common to every wap
**BSSID **- unique and based on mac
**ESS **- extended surface set - Contains APs - cover the area of all waps

**distribution network**  - extends wire network with a collection of wap interconnected to each other

**BSS **- Basic Service Set - AP are at the heart of the BSS (Infrastructure mode) - Only one channel for every devices in the BSS

**BSA **is the area covered by AP
**DS **- Distribution System - Wired network connect to the AP
**IBSS **- Independent Basic Service Set - Ad Hoc network between two devices

**WGB **- Workgroup bridge - Use to connect a wireless device directly to the WAP (Like an external wifi card)

**Outdoor Bridge** - Big antenna to connect different location together

**Mesh network** - Different working together as multiple BSS with the same channel - The information is bridged between all  the APs

wifi analyzer

One Vlan equal to one SSID - Use trunk interface from the DS to the AP - The AP appears as an Multiple logical AP with a different BSSID (+1)

frequency measured in hertz
higher frequency are faster and shorter
A cycle is a complete loop of the signal
Hertz is number defining the number of cycle in one second

5 Ghz band is a non overlapping channels

Each AP has a management interface (it should be in a dedicated vlan)
AP management platform - Cisco Prime Infrastructure

Cloud Bases AP architecture - Cisco Meraki - Each APs connect to the cloud and gets his configuration automatically - AP are still Autonomous

Control plane - Traffic used to control the APs and for the management
Data plane -  End-user traffic

**Split-MAC Architectures**

When the functions of an autonomous AP are divided, the AP hardware is known as a lightweight access point, and performs only the real-time 802.11 operation. The lightweight AP gets its name because the code image and the local intelligence are stripped down, or lightweight, compared to the traditional autonomous AP.

The management functions are usually performed on a wireless LAN controller (WLC), which controls many lightweight APs.

Notice that the AP is left with duties in Layers 1 and 2, where frames are moved into and out of the RF domain. The AP becomes totally dependent on the WLC for every other WLAN function, such as authenticating users, managing security policies, and even selecting RF channels and output power.

WLC and APs don't need to be on the same network or Vlan because they use **CAPWAP.**

**CAPWAP **- Control and Provisioning of Wireless Access Point - Tunneling protocol for communicating between APs and WLC (Wireless Lan Controller)

**CAPWAP CONTROL** - UDP 5246 - Use to transmit management packets
**CAPWAP DATA** - UDP 5247 - Use to transmit data

**Centralized WLC Deployment **- Scheme who tends to place the WLC in the most centralized place. Usually connect the core switch. Then connect LAPs on the access switches.

**MIC **- Message integrity check - Check if the data has been tampered by integrating a secret stamp inside the frame.

Open Authentication - No security - Public location WIFI

**802.1x - EAP** - Extensible Authentication Protocol - It's framework defining a set of common functions to be used in different method authentication.

**802.1x** authentication process and role
**supplicant **- the client device that is requesting access

**authenticator **- the network device that provides access to the network (WLC)

**authenticator server** - The device that takes user or client credentials and permits or denies network access based on a user database and policies (usually a **RADIUS **server)

First the client authenticate with open authentication and then the authentication process begin with the authentication server

**LEAP **- Cisco proprietary, PEAP, EAP-TLS are all authentication methods based on EAP...

**TKIP **- Temporal Key Integrity Protocol

**TKIP **adds more security features like MIC, time stamp, sender's mac address, TKIP sequence counter

**TKIP **has been deprecated with the 802.11-2012 standard

**CCMP** - Counter CBC/MAC Protocol - Two algorithms - AES counter mode encryption and Cipher Block Chaining Message Authentication Code (**CBC-MAC**)

**CBC-MAC** is message integrity protocol like MIC

**AES **- The Advanced Encryption Standard (AES) is the current encryption algorithm adopted by U.S. National Institute of Standards and Technology (**NIST**) and the U.S. government, and widely used around the world. In other words, AES is open, publicly accessible, and represents the most secure encryption method available today.

**CCMP **works with all WPA2 compatible devices

**GCMP **- Gallois/.Counter Mode Protocol - Two algorithms - AES counter mode encryption -  Galois Message Authentication Code (**GMAC**) as a message integrity check

**GCMP **is used in **WPA3**
More secure than **CCMP**

**WPA**, **WPA2 **and **WPA3 **are certifications for wifi protections. These are standards.

**PSK **(Pre-shared keys) is supported by every standards (personal mode)

**802.1x** Authentication is also supported by every standards (enterprise mode)

**WLC **makes the difference between interfaces and ports. Interface are logical where ports are physical.

**Service port:** Used for out-of-band management, system recovery, and initial boot functions; always connects to a switch port in access mode

**Distribution system port:** Used for all normal AP and management traffic; usually connects to a switch port in 802.1Q trunk mode

**Console port:** Used for out-of-band management, system recovery, and initial boot functions; asynchronous connection to a terminal emulator (9600 baud, 8 data bits, 1 stop bit, by default)

**Redundancy port: **Used to connect to a peer controller for high availability (HA) operation

Interfaces are configure as a normal interface and then assigned to a port with vlan etc...

**Management interface:** Used for normal management traffic, such as **RADIUS **user authentication, **WLC**-to-**WLC **communication, web-based and **SSH** sessions, **SNMP**, Network Time Protocol (**NTP**), syslog, and so on. The management interface is also used to terminate **CAPWAP **tunnels between the controller and its APs.

**Redundancy management:** The management IP address of a redundant **WLC **that is part of a high availability pair of controllers. The active **WLC** uses the management interface address, while the standby WLC uses the redundancy management address.

**Virtual interface**: IP address facing wireless clients when the controller is relaying client **DHCP **requests, performing client web authentication, and supporting client mobility.

**Service port interface**: Bound to the service port and used for out-of-band management.

**Dynamic interface**: Used to connect a **VLAN **to a **WLAN**.

Cisco controllers support a maximum of 512 **WLANs**, but only 16 of them can be actively configured on an AP. As a rule of thumb, always limit the number of **WLANs** to five or fewer; a maximum of three **WLANs **is best.

A new **WLAN **needs :

**SSID** string
Controller interface and **VLAN **number
Type of wireless security needed