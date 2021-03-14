# Traditional telephony
## Core components
### PSTN
https://en.wikipedia.org/wiki/Public_switched_telephone_network
The public switched telephone network (PSTN) is the aggregate of the world's circuit-switched telephone networks that are operated by national, regional, or local telephony operators, providing infrastructure and services for public telecommunication. The PSTN consists of telephone lines, fiber optic cables, microwave transmission links, cellular networks, communications satellites, and undersea telephone cables, all interconnected by switching centers, thus allowing most telephones to communicate with each other. Originally a network of fixed-line analog telephone systems, the PSTN is now almost entirely digital in its core network and includes mobile[1] and other networks, as well as fixed telephones.

### PBX (Private branch extend)
PBX stands for Private Branch Exchange, which is a private telephone network used within a company or organization. The users of the PBX phone system can communicate internally (within their company) and externally (with the outside world), using different communication channels like Voice over IP, ISDN or analog. A PBX also allows you to have more phones than physical phone lines (PTSN) and allows free calls between users. Additionally, it provides features like transfer calls, voicemail, call recording, interactive voice menus (IVRs) and call queues.

![image](/home/r0ck/Documents/notes/_resources/pbx.jpeg)

Time and technology, however, have changed the consumer telephony landscape, with the flag-bearer being the Open-Standards-based IP PBX. The point of the “IP” in this new era is that the phone calls are delivered using the Internet Protocol as the underlying transport technology.

### Analog phone
![/home/r0ck/Documents/notes/_resources/telephone.jpg](/home/r0ck/Documents/notes/_resources/telephone.jpg)
An analog phone is one which makes use of analog technology. Analog technology is simply the process by which the technology takes an audio or video signal and translates it into electronic pulses (the human voice being transmitted over the phone, for instance). Also known as Plain Old Telephone Service (POTS), these lines are analog support standard phones, fax machines and modems.

### Digital phone
http://www.differencebetween.net/technology/difference-between-analog-and-digital-phones/
A digital phone is one that uses technology that breaks an audio or video signal (such as your voice or a television) into binary code – essentially, a code comprised of 1s and 0s. Once translated into binary code, the signal gets transferred to the other end of the device into another device (whether be it a phone, modem or television). This receiving device takes the binary code and reassembles it into the original signal (meaning, once received, the other device translates the binary code into the signal from which the code came – a reciprocating action) and sends it back to the other end.

### KEY DIFFERENCES between analog and digital signal

- An analog signal is a continuous signal whereas Digital signals are time separated signals.
- Analog signal is denoted by sine waves while It is denoted by square waves
- Analog signal uses a continuous range of values that help you to represent information on the other hand digital signal uses discrete 0 and 1 to represent information.
- The analog signal bandwidth is low while the bandwidth of the digital signal is high.
- Analog instruments give considerable observational errors whereas Digital instruments never cause any kind of observational errors.
- Analog hardware never offers flexible implementation, but Digital hardware offers flexibility in implementation.
- Analog signals are suited for audio and video transmission while Digital signals are suited for Computing and digital electronics.

## Converting analog signal to digital signal
https://flylib.com/books/en/3.437.1.24/1/

The process of converting the signal from analog to digital is called `sampling`.

- First we take an analog signal.

	![home r0ck Documents notes _resources png](/home/r0ck/Documents/notes/_resources/51c48c2ece395fd35a000000.png)

- Then we put some "dots" on the analog curve.

	![home r0ck Documents notes _resources sampling jpeg](/home/r0ck/Documents/notes/_resources/sampling.jpeg)

- The recommend amount of sampling is `2 x 4000 hz` because `4000 hz` is the highest (average) frequency heard by human during a conversation. So the average sampling is in theory `8000 hz` per second.

- Now we need to convert these dots into a digital format. There are two ways to do that, `PAM` and `PCM`. Today we `Pulse Code Modulation` to digitally represent sampled analog signals. For each dots on the curve, we are gonna use 8 bits to represent it.

![home r0ck Documents notes _resources Screenshot](/home/r0ck/Documents/notes/_resources/Screenshot) from 2021-03-12 13-04-44.png

	- 1 bit for the `polarity`
	- 3 bits for the `segment`
	- 4 bits for the `step`

- So we take `8000` samples per second and we multiply it with `8` bits. We got `64000` bits per second or `64` Kbits per second.

### Compression codecs
https://fitsmallbusiness.com/g729-vs-g711-voip-codecs/

There are different codecs but the most popular are `G711` and `G729`.

![home r0ck Documents notes _resources codecs jpeg](/home/r0ck/Documents/notes/_resources/codecs.jpeg)

The main difference is in the compression. `G711` doesn't compress the data.

# VOIP Components
## Cisco Unified Communication Manager
A `CUCM` is a `PBX` replacement. It is based on a server and most of the time it runs inside a `VM`.
![home r0ck Documents notes _resources cucm bmp](/home/r0ck/Documents/notes/_resources/cucm.bmp)

### CUCM-PUB
When we work with clusters, we need to replicate data between all the communication managers. In cluster, the publisher maintains the database and push it to the subscribers.
He is the only one to have a read/write copy of the database.

### CUCM-SUB
The subscribers only receives the database from the publisher.
### CUCME

## CUC (Cisco Unity Connection)
- Cisco Unity Connection is a voice messaging solution.
- It runs on Linux.
- 20000 mailboxes per server or per cluster.
	- If we add a server to a cluster, it doesn't add more mailboxes available.
- Compatible with PBX (for migration).
- A couple of protocols can be use to communicate with the `cisco unified communication manager`. One of these is `SCCP` and is very well known but now the industry standard is `SIP`.

### CUC Routing Vocabulary
- `ANI (Automatic number identification)`: Brings informations about the caller id.
- `DNIS (Dialed number information service)`: Brings the number that somebody dialed in.
- `RDNIS (Redirected Dialed number information service)`: When a call is redirected by a phone, the `rdnis` gives informations about who redirected the call.

## IM&P (Cisco Instant Messaging and Presence Server)
Presence information can represented as the willingness to take a call. For example, we can press a button on the phone to make us unavailable or we can be reported as `idle` because we are not at our desk. The `IM&P` is in charge of handling these informations and communicate them to the communication manager. It uses `SIP` to communicate with the communication manager because `SIP` is able to transport these informations.
`IM&P` is able to handle informations reported by `XMPP`. For example, a `Cisco jabber client` can send availability informations based on our calendar. The device uses `XMPP` to carry these informations to the `IM&P`.

## Cisco IP Phone
### 7800 Series
- Entry level phone
- Lower-cost ip phone
- Audio only calls

### 8800 Series
- Audio/Video calls
- Can mirror a display to a table
- Video in 720p
- Support wi-fi


## Telepresence
Collection of products dedicated to improve the experience of users during meetings etc...
Cisco has a different series with different products in it. (MX, SX, DX etc...)

## DSP (Digital Signaling processors)
It's a module for Cisco router that have to process digital signals from the PSTN.

![home r0ck Documents notes _resources dsp png](/home/r0ck/Documents/notes/_resources/dsp.png)

## CUBE (Cisco Unified Border Element)
https://www.cisco.com/c/en/us/products/unified-communications/unified-border-element/index.html
![home r0ck Documents notes _resources cube png](/home/r0ck/Documents/notes/_resources/cube.png)

The `cube` is a network border element that act as a gateway. It's a bridge between different type of Voice protocol. CUBE is used by enterprise and small and medium-sized organizations to interconnectSIPPSTN access with SIP and H.323 enterprise unified communications networks.

## DNS-INT
## EXWAY-C / EXWAY-E
When `Jabber` wants to communicate with a `communication manager` inside a local network, it needs to bypass the firewall.
To do that, we use `exway`.

- The first appliance seats inside the LAN. (ExpressWay C)
- The second applicance seats inside the `DMZ` the LAN. (ExpressWay E)
- Expressway C create a tunnel with ExpressWay E and they keep the tunnel open. 
- It works because expressway C seats inside the network, so, it can create a connection.
- The jabber device tries to resolve the dns name of the phone using a custon dns server address. 
  It connects to Expressway E at first, and then, it goes through the tunnel and get to the `communication manager`.

![home r0ck Documents notes _resources exway png](/home/r0ck/Documents/notes/_resources/exway.png)

# VOIP Softwares
## Cisco Jabber Client
https://www.cisco.com/c/en/us/products/unified-communications/jabber-windows/index.html

- `Cisco Jabber Client` is able to communicate with the `IM&P` using `xmpp`. When the `IM&P` receives the `xmpp` packet, it converts it to a valid `SIP` format and send it to the destination phone.
- `CJC` uses `imap` to retreives voice call from the `Unity connection server`.
- `CJC` communicate to the `communication manager` using `CTI/QBE` to send `SIP` messages to our phone. This way we can control the phone remotely. Go off-hook, on-hook etc...

# VOIP Protocols
## Signaling protocols
### SIP (Session Initiation Protocol)
- SIP is the only signaling protocol that has the ability to transport `presence` information.
### H.363
### MGCP
### SCCP

## RTP (real time transfer protocol)
RTP carries the actual data (voice/video).
When 2 phones are connected to each other, the don't rely on the `communication manager` any longer and they talk to each other directly using the protocol `rtp`.
