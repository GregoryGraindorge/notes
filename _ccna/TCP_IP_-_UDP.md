# TCP/IP - UDP

|     |     |
| --- | --- |
| Multiplexing using ports | Function that allows receiving hosts to choose the correct application for which the data is destined, based on the port number |
| Error recovery (reliability) | Process of numbering and acknowledging data with Sequence and Acknowledgment header fields |
| Flow control using windowing | Process that uses window sizes to protect buffer space and routing devices from being overloaded with traffic |
| Connection establishment and termination | Process used to initialize port numbers and Sequence and Acknowledgment fields |
| Ordered data transfer and data segmentation | Continuous stream of bytes from an upper-layer process that is “segmented” for transmission and delivered to upper-layer processes at the receiving device, with the bytes in the same order |

## TCP/IP Header**
 ![An illustration depicts the header fields of Transmission Control Protocol (TCP).](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig01.jpg)

A socket consists of three things:

- An IP address
- A transport protocol
- A port number

Well known port numbers: https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.txt

|     |     |     |
| --- | --- | --- |
| **Port Number** | **Protocol** | **Application** |
| 20  | TCP | FTP data |
| 21  | TCP | FTP control |
| 22  | TCP | SSH |
| 23  | TCP | Telnet |
| 25  | TCP | SMTP |
| 53  | UDP, TCP[1](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/ch01.xhtml#rch01tabfn1) | DNS |
| 67  | UDP | DHCP Server |
| 68  | UDP | DHCP Client |
| 69  | UDP | TFTP |
| 80  | TCP | HTTP (WWW) |
| 110 | TCP | POP3 |
| 161 | UDP | SNMP |
| 443 | TCP | SSL |
| 514 | UDP | Syslog |

![Key Topic.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/key_topic_icon.jpg)

- **Connection-oriented protocol:** A protocol that requires an exchange of messages before data transfer begins, or that has a required pre-established correlation between two endpoints.
- **Connectionless protocol:** A protocol that does not require an exchange of messages and that does not require a pre-established correlation between two endpoints.

## TCP reliability and recovery system**

![An architecture diagram depicts the occurrence of TCP acknowledgment with errors between web browser and server.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig08.jpg)

## Flow control using Windowing**
 ![An architecture diagram depicts an overview of TCP Windowing between web browser and server.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig09.jpg)
 ## UDP Header**
 ![An illustration depicts the header fields of the User Datagram Protocol (UDP).](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig10.jpg)
 ## WWW:** The World Wide Web (WWW) consists of all the Internet-connected web servers in the world, plus all Internet-connected hosts with web browsers.

## URI: **Uniform Resource Identifier (URI)

In common speech, many people use the terms *web address* or the similar related terms *Universal Resource Locator* (or Uniform Resource Locator [URL]) instead of URI, but URI is indeed the correct formal term.

![An overview of the structure of a URI along with the fields used to retrieve a web page is illustrated.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig11.jpg)

## DNS Request**
 ![An architecture summarizes the steps involved in the DNS resolution process and an overview of requesting a web page.](https://learning.oreilly.com/library/view/ccna-200-301-official/9780135262726/graphics/01fig12.jpg)
 ## HTTP - **The HTTP application layer protocol, defined in RFC 7230, defines how files can be transferred between two computers.
