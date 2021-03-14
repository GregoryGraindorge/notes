Security - DHCP Snooping

#### **DHCP Snooping Concepts**

**
**
DHCP Snooping on a switch acts like a firewall or an ACL in many ways.

While DHCP itself provides a Layer 3 service, DHCP Snooping operates on LAN switches and is commonly used on Layer 2 LAN switches and enabled on Layer 2 ports.

![An illustration of the DHCP snooping concept is presented.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/08fig01.jpg)

- DHCP messages received on an untrusted port, for messages normally sent by a server, will always be discarded.
- DHCP messages received on an untrusted port, as normally sent by a DHCP client, may be filtered if they appear to be part of an attack.
- DHCP messages received on a trusted port will be forwarded; trusted ports do not filter (discard) any DHCP messages.

![An illustration of the rules for DHCP snooping.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/08fig04.jpg)

![Key Topic.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/key_topic_icon.jpg)

1. Examine all incoming DHCP messages.
2. If normally sent by servers, discard the message.
3. If normally sent by clients, filter as follows:

    1. **For DISCOVER and REQUEST messages, check for MAC address consistency between the Ethernet frame and the DHCP message.**

    2. For RELEASE or DECLINE messages, check the incoming interface plus IP address versus the DHCP Snooping binding table.

4. For messages not filtered that result in a DHCP lease, build a new entry to the DHCP Snooping binding table.

DHCP uses CHADDR (Client Hardware Address) in the header of every dhcp messages.

Attackers try to flood the DHCP server with DHCP messages and different CHADDR int the header. DHCP snooping can defeat that technique by comparing Layer 2 mac address and the CHADDR.

#### **DHCP Snooping Configuration**

- enable DHCP Snooping
- list the VLANs on which to use DHCP Snooping
- configure a few ports as trusted ports (default is untrusted)

***Example 8-1**** DHCP Snooping Configuration to Match [Figure 8-8](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08.xhtml#ch08fig8)*

[Click here to view code image](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08_images.xhtml#exm08_01)

ip dhcp snooping
ip dhcp snooping vlan 11
no ip dhcp snooping information option
!
interface GigabitEthernet1/0/2
ip dhcp snooping trust

***Example 8-2**** SW2 DHCP Snooping Status*

[Click here to view code image](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08_images.xhtml#exm08_02)

SW2# show ip dhcp snooping
Switch DHCP snooping is enabled
Switch DHCP gleaning is disabled
DHCP snooping is configured on following VLANs:
11
DHCP snooping is operational on following VLANs:
11
Smartlog is configured on following VLANs:
none
Smartlog is operational on following VLANs:
none
DHCP snooping is configured on the following L3 Interfaces:

Insertion of option 82 is disabled
  circuit-id default format: vlan-mod-port
  remote-id: bcc4.938b.a180 (MAC)
Option 82 on untrusted port is not allowed
Verification of hwaddr field is enabled
Verification of giaddr field is enabled
DHCP snooping trust/rate is configured on the following Interfaces:

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
GigabitEthernet1/0/2      yes        yes            unlimited
  Custom circuit-ids:

**Insertion of option 82 is disabled**. That line confirms the addition of the **no ip dhcp information option** command to the configuration

DHCP relay agents add new fields to DHCP requests—defined as option 82 DHCP header fields (in RFC 3046).

To make DHCP Snooping work on a switch that is not also a DHCP relay agent, disable the option 82 feature using the **no ip dhcp snooping information option** global command.

##### **Limiting DHCP Message Rates**

Devise new attacks, including attacking DHCP Snooping itself.

Attackers can devise attacks to generate large volumes of DHCP messages in an attempt to overload the DHCP Snooping feature and the switch CPU itself. That might cause DHCP Snooping to fail to examine every message.

DHCP Snooping includes an optional feature that tracks the number of incoming DHCP messages. DHCP Snooping treats the event as an attack and moves the port to an err-disabled state.

***Example 8-3**** Configuring DHCP Snooping Message Rate Limits*

[Click here to view code image](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08_images.xhtml#exm08_03)

errdisable recovery cause dhcp-rate-limit
errdisable recovery interval 30
!
interface GigabitEthernet1/0/2
ip dhcp snooping limit rate 10
!
interface GigabitEthernet1/0/3
ip dhcp snooping limit rate 2

A repeat of the **show ip dhcp snooping** command now shows the rate limits near the end of the output, as noted in [Example 8-4](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08.xhtml#ch08exa4).

***Example 8-4**** Confirming DHCP Snooping Rate Limits*

[Click here to view code image](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch08_images.xhtml#exm08_04)

SW2# show ip dhcp snooping
! Lines omitted for brevity

Interface                  Trusted    Allow option    Rate limit (pps)
-----------------------    -------    ------------    ----------------
GigabitEthernet1/0/2      yes        yes            10
  Custom circuit-ids:
GigabitEthernet1/0/3      no        no              2
  Custom circuit-ids:

![Config checklist.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/config_checklist.jpg)

**Step 1.** Configure this pair of commands (both required):

**A.** Use the **ip dhcp snooping** global command to enable DHCP Snooping on the switch.

**B.** Use the **ip dhcp snooping vlan** *vlan-list* global command to identify the VLANs on which to use DHCP Snooping.

**Step 2.** **(Optional):** Use the **no ip dhcp snooping information option** global command on Layer 2 switches to disable the insertion of DHCP Option 82 data into DHCP messages, specifically on switches that do not act as a DHCP relay agent.

**Step 3.** Configure the **ip dhcp snooping trust** interface subcommand to override the default setting of not trusted.

**Step 4.** **(Optional):** Configure DHCP Snooping rate limits and err-disabled recovery:

**Step A. (Optional):** Configure the **ip dhcp snooping limit rate** *number* interface subcommand to set a limit of DHCP messages per second.

**Step B. (Optional):** Configure the **no ip dhcp snooping limit rate** *number* interface subcommand to remove an existing limit and reset the interface to use the default of no rate limit.

**Step C. (Optional):** Configure the **errdisable recovery cause dhcp-rate-limit** global command to enable the feature of automatic recovery from err-disabled mode, assuming the switch placed the port in err-disabled state because of exceeding DHCP Snooping rate limits.

**Step D. (Optional):** Configure the **errdisable recovery interval** *seconds* global commands to set the time to wait before recovering from an interface err-disabled state (regardless of the cause of the err-disabled state).