Nmap Tips and Tricks. Nmap (Network MAPper) is a network port… | by Peter Kacherginsky | Medium

# Nmap Tips and Tricks

[![2*-BRbhnyX7V_oWbr0mWob2w.png](../_resources/4b1be472b1976612253d2ee265b28137.png)](https://medium.com/@iphelix?source=post_page-----5b4a3d2151b3--------------------------------)

[Peter Kacherginsky](https://medium.com/@iphelix?source=post_page-----5b4a3d2151b3--------------------------------)

[Dec 15, 2008](https://medium.com/@iphelix/nmap-scanning-tips-and-tricks-5b4a3d2151b3?source=post_page-----5b4a3d2151b3--------------------------------) · 24 min read

[![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='25' height='25' viewBox='0 0 25 25' data-evernote-id='216' class='js-evernote-checked'%3e%3cpath d='M19 6a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v14.66h.01c.01.1.05.2.12.28a.5.5 0 0 0 .7.03l5.67-4.12 5.66 4.13a.5.5 0 0 0 .71-.03.5.5 0 0 0 .12-.29H19V6zm-6.84 9.97L7 19.64V6a1 1 0 0 1 1-1h9a1 1 0 0 1 1 1v13.64l-5.16-3.67a.49.49 0 0 0-.68 0z' fill-rule='evenodd' data-evernote-id='217' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://medium.com/m/signin?actionUrl=%2F_%2Fbookmark%2Fp%2F5b4a3d2151b3&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40iphelix%2Fnmap-scanning-tips-and-tricks-5b4a3d2151b3&source=post_actions_header--------------------------bookmark_header-----------)

**Nmap** (Network MAPper) is a network port scanner with service version and operating system detection engines. The tool was originally developed by Fyodor and published in Phrack Issue 51 in 1997. The tool is command line although a number of GUIs exist. Nmap runs on a variety of platforms including Linux, *BSD, Windows, and others.

# Port Scanning

Nmap uses several port scanning approaches. Table below summarizes “canned” scan types and corresponding command line flags:

- **-sT:** TCP Connect() Scan
- **-sS:** SYN Scan
- **-sA:** ACK Scan
- **-sW:** Window
- **-sN:** Null Scan
- **-sF:** FIN Scan
- **-sX:** XMas Scan
- **-sU:** UDP Scan
- **-sM:** Maimon Scan
- **-sO:** IP Protocol Scan
- **-sI:** host:port Idle Scan
- **-b:** FTP Bounce Scan

Using the above table, we can quickly generate a simple SYN scan on a Windows box:

nmap -sS 192.168.1.100Interesting ports on 192.168.1.100:
Not shown: 1692 closed ports
PORT STATE SERVICE
135/tcp open msrpc
139/tcp open netbios-ssn
445/tcp open microsoft-ds
1025/tcp open NFS-or-IIS
5000/tcp open UPnP

MAC Address: 00:11:22:33:44:55Nmap finished: 1 IP address (1 host up) scanned in 1.347 seconds

It is often useful to know the reason for nmap’s decision on port’s state. Use option `--reason` to get detailed explanation:

nmap -sS 72.14.207.99 -p22,80 --reason
...
Interesting ports on eh-in-f99.google.com (72.14.207.99):
PORT STATE SERVICE REASON
22/tcp filtered ssh no-response

80/tcp open http syn-ackNmap done: 1 IP address (1 host up) scanned in 2.028 seconds

With the command line above, only default set of ports will be scanned. To scan all ports on the machine use *-p* flag:

nmap -sS 192.168.1.100 -p1-65535
To scan a large number of machines, you may use ranges and wildcards:
nmap -sA 192.168.*.1-10,250-254

The above will scan everything beginning with ***192.168*** and ending with either ***1–10*** or ***250–254***. The less flexible CIDR notation may also be used. Below is an example on how to perform a UDP scan on a Class C subnet:

nmap -sU 192.168.0.0/24

The majority of scans are trivial to execute, they only require users to set a proper flag, target, and a range of ports. Some scan types, such as Idle Scan and FTP Bounce Scan require additional hosts with certain characteristics to act as intermediaries.

In addition to predefined set of scanning methodologies, NMap allows its users to generate custom TCP packets with different flags set (URG, ACK, PSH, RST, SYN, and FIN). Results collected from a custom scan will be interpreted as if it was a SYN Scan ( SYN/ACK indicating an open port and RST indicating a closed port) by default, but you can specify different approach to interpreting results by specifying any other scan type. For example, if we want to create a variation on FIN Scan and instead scan using a combination of URG and PSH flags set in packets we send the following nmap command will do the trick:

nmap 192.168.1.101 -p666 -sF --scanflags URGPSH

In the example above we are attempting to probe port 666 on the target host 192.168.1.101 with the following packets being sent:

0.144033 192.168.1.100 -> 192.168.1.101 TCP 56242 > 666 [PSH, URG] Seq=0 Urg=0 Len=0

0.144144 192.168.1.101 -> 192.168.1.100 TCP 666 > 56242 [RST, ACK] Seq=0 Ack=0 Win=0 Len=0

We have received RST from the target host which will be interpreted as closed port as if it was a FIN Scan in the first place. Below is nmap’s output for this custom scan:

Interesting ports on 192.168.1.101:
PORT STATE SERVICE
666/tcp closed doom

# Host Discovery

Nmap’s default host discovery facility is **Ping Scan**. It communicates with target hosts by sending ICMP echo request and an ACK packet to port 80 when raw socket access is available. The latter is akin to TCP ACK Scan where RST response will be generated if the target host exists and its ports are unfiltered. This is a useful feature since more and more hosts ignore ICMP Echo requests. The following nmap command will perform this type of scan on hosts 4.2.2.1 to 4.2.2.3:

nmap -sP 4.2.2.1-3 -n

Below is the packet trace of the above request. Note that we have disabled DNS lookup for brevity:

# nmap is sending both ping request and ACK packet to port 80

# to test existance of hosts0.000000 192.168.1.100 -> 4.2.2.1 ICMP Echo (ping) request

0.000369 192.168.1.100 -> 4.2.2.1 TCP 59997 > www [ACK] Seq=0 Ack=0 Win=2048 Len=0

0.000877 192.168.1.100 -> 4.2.2.2 ICMP Echo (ping) request

0.000969 192.168.1.100 -> 4.2.2.2 TCP 59997 > www [ACK] Seq=0 Ack=0 Win=2048 Len=0

0.001069 192.168.1.100 -> 4.2.2.3 ICMP Echo (ping) request

0.052535 192.168.1.100 -> 4.2.2.3 TCP 59997 > www [ACK] Seq=0 Ack=0 Win=3072 Len=0# not only hosts are nice enough to respond with ping reply, but

# they further confirm their existance with RST responses0.055146 4.2.2.1 -> 192.168.1.100 ICMP Echo (ping) reply

0.059524 4.2.2.1 -> 192.168.1.100 TCP www > 59997 [RST] Seq=0 Len=0 0.061627 4.2.2.2 -> 192.168.1.100 TCP www > 59997 [RST] Seq=0 Len=0 0.061910 4.2.2.2 -> 192.168.1.100 ICMP Echo (ping) reply

0.062106 4.2.2.3 -> 192.168.1.100 ICMP Echo (ping) reply
0.100264 4.2.2.3 -> 192.168.1.100 TCP www > 59997 [RST] Seq=0 Len=0
And here is nmap’s output for the above scan:
Host 4.2.2.1 appears to be up.
Host 4.2.2.2 appears to be up.
Host 4.2.2.3 appears to be up.

For scanning environments where access to raw sockets is not available, nmap utilizes an approach similar to TCP Connect Scan by trying to connect to port 80 on the target host. Naturally any response (SYN/ACK or RST) will confirm hosts existance. Here is a packet trace for scan above made using unprivileged account:

# nmap attempts to connect to port 80 on target hosts0.000000 192.168.1.100 -> 4.2.2.1 TCP 51223 > www [SYN] Seq=0 Len=0 MSS=1460 TSV=22750678 TSER=0 WS=2

0.000980 192.168.1.100 -> 4.2.2.2 TCP 49964 > www [SYN] Seq=0 Len=0 MSS=1460 TSV=22750678 TSER=0 WS=2

0.001323 192.168.1.100 -> 4.2.2.3 TCP 37757 > www [SYN] Seq=0 Len=0 MSS=1460 TSV=22750678 TSER=0 WS=2 # target hosts reply that port is not available thus revealing their existence

0.014831 4.2.2.1 -> 192.168.1.100 TCP www > 51223 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0

0.018126 4.2.2.2 -> 192.168.1.100 TCP www > 49964 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0

0.023210 4.2.2.3 -> 192.168.1.100 TCP www > 37757 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0

While Ping Scan offers a preset collection of tests, you may fine tune the discovery process with ping types below:

## ICMP Ping

Nmap allowed us to use several unique ICMP tyles for pings: -PE, -PP, and -PM. The above three arguments correspond to standard ECHO reply, Timestamp reply, and Address Mask reply. Below are commands and corresponding commands for the above ping types:

For echo ping the following nmap command can be used:
nmap -sP -PE 4.2.2.1
For Timestamp ping we can use this command instead:
nmap -sP -PP 4.2.2.1
At last Address Mask ping will require the following command:
nmap -sP -PM 4.2.2.1

## TCP SYN Ping

TCP SYN Ping allows us to use SYN packets sent to specific port(s) when attempting to determine machine’s existence.

nmap -sP -PS53 4.2.2.1

## TCP ACK Ping

Below is the nmap command that will perform ACK ping on ***4.2.2.1–3*** range just like in SYN ping example.

nmap -sP -PA53 4.2.2.1-3

## UDP Ping

UDP Ping sends an empty UDP packet to an uncommon port hoping that it will produce ICMP reply revealing that the target system is live. UDP ping uses ***31338*** as a default port to test on the target system. Below is a sample case of UDP scan of a range from ***4.2.2.1*** to ***4.2.2.3:***

nmap -sP -PU 4.2.2.1

## ARP Ping

You can force any of scan to use ARP ping by adding `--send-eth `to the command. On the other hand you can prevent the use of raw ethernet frames by adding `--send-ip `to disable ARP ping. Here is an example of trying to ping host ***192.168.1.1***:

nmap -sP -PR 192.168.1.1

## IP Protocol Ping

As a natural extension of IP Protocol Scan, Nmap implement IP Protocol Ping to illicit some response from the host:

nmap -sP -PO 192.168.1.1

## Reverse DNS

We can conduct mass rDNS queries using nmap’s **List Scan**. It does not make any direct queries to target hosts, but it lists all ip addresses in the specified range and performs rDNS queries. You can also specify your own list of dns servers. For example a command to list scan ip range from ***64.233.187.99*** to ***64.233.187.101*** using ***4.2.2.1*** as a DNS server:

nmap -sL 64.233.187.99-101 --dns-servers 4.2.2.1,4.2.2.2

Starting Nmap 4.20 ( http://insecure.org ) at 2007-10-04 19:32 PDT Host jc-in-f99.google.com (64.233.187.99) not scanned

Host jc-in-f100.google.com (64.233.187.100) not scanned
Host jc-in-f101.google.com (64.233.187.101) not scanned
Nmap finished: 3 IP addresses (0 hosts up) scanned in 4.080 seconds

# Service/Version Detection

When Nmap discovers an open port on the target system, it prints out a service name commonly associated with that port number. Such descriptions are obtained from a fixed database with direct mappings of port numbers and their associated description. Unfortunately, if a particular service will be bound on a non-standard port, information supplied by Nmap would be faulty. To establish which service is running on an open port to a greater degree of certainty, Nmap can optionally further interrogate the system to extract not only the service name but also version number, protocol version, and other useful information. Nmap includes a number of generic probes: 10 UDP Probes and 30 TCP Probes. In the majority of time a simple TCP NULL Probe is sufficient to determine all information we need about the port; however if clarification or more information is needed nmap will launch more probe requests.

## TCP NULL Probe

This probe is launched by default when attempting to run service/version detection mechanism on an open TCP port. In this case a TCP connection is opened, but nothing is sent. Instead nmap waits for 6 seconds (used to be 5) and listens for any response such as welcome banners. Responses are matched with hundreds of signatures stored in Nmap’s service probes database stored in *nmap-service-probe* file. For example, let’s attempt to detect the exact service and version running on port 22 (ssh) of the target system 192.168.1.101:

nmap -sV -p22 192.168.1.101 --version-all
This request generates the following packet trace:

# Nmap first SYN Scans the target port and determines it is open0.125807 192.168.1.100 -> 192.168.1.101 TCP 63744 > ssh [SYN] Seq=0 Len=0 MSS=1460

0.125908 192.168.1.101 -> 192.168.1.100 TCP ssh > 63744 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460

0.125931 192.168.1.100 -> 192.168.1.101 TCP 63744 > ssh [RST] Seq=1 Len=0# Next a connection is established and we passively wait for

# the server response0.244139 192.168.1.100 -> 192.168.1.101 TCP 35239 > ssh [SYN] Seq=0 Len=0 MSS=1460 TSV=1221975 TSER=0 WS=2

0.244281 192.168.1.101 -> 192.168.1.100 TCP ssh > 35239 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=7886057 TSER=1221975 0.244306 192.168.1.100 -> 192.168.1.101 TCP 35239 > ssh [ACK] Seq=1 Ack=1 Win=5840 Len=0 TSV=1221975 TSER=7886057# Server responds with a welcome banner which includes exact service # name and version number which completes TCP Null Probe routine0.259871 192.168.1.101 -> 192.168.1.100 SSH Server Protocol: SSH-2.0-OpenSSH_4.5p1 FreeBSD-20061110

0.259899 192.168.1.100 -> 192.168.1.101 TCP 35239 > ssh [ACK] Seq=1 Ack=40 Win=5840 Len=0 TSV=1221979 TSER=7886072

0.399051 192.168.1.100 -> 192.168.1.101 TCP 35239 > ssh [FIN, ACK] Seq=1 Ack=40 Win=5840 Len=0 TSV=1222014 TSER=7886072

0.399176 192.168.1.101 -> 192.168.1.100 TCP ssh > 35239 [ACK] Seq=40 Ack=2 Win=66608 Len=0 TSV=7886212 TSER=1222014

0.399771 192.168.1.101 -> 192.168.1.100 TCP ssh > 35239 [FIN, ACK] Seq=40 Ack=2 Win=66608 Len=0 TSV=7886212 TSER=1222014

0.399788 192.168.1.100 -> 192.168.1.101 TCP 35239 > ssh [ACK] Seq=2 Ack=41 Win=5840 Len=0 TSV=1222014 TSER=7886212

Upon connecting to the target system on port 22, nmap received the following welcome banner:

SSH-2.0-OpenSSH_4.5p1 FreeBSD-20061110

It was immediately matched with the following regular expression entry in Nmap’s service probe database which extracted not only service name, type, and protocol number but also details on host operating system.

match ssh m|^SSH-([\d.]+)-OpenSSH_([\w.]+) FreeBSD-([\d]+)\n| p/OpenSSH/ v/$2/ i/FreeBSD $3; protocol $1/ o/FreeBSD/

As a result nmap produced the following output:
Interesting ports on 192.168.1.101:
PORT STATE SERVICE VERSION
22/tcp open ssh OpenSSH 4.5p1 (FreeBSD 20061110; protocol 2.0)
MAC Address: 00:01:02:03:04:05
Service Info: OS: FreeBSD

## Other Probes

If the target port is UDP, TCP Null Probe did not return sufficient results, or additional information can be collected, Nmap launches one of its probes. Detailed listing of each probes can be found on Nmap Service Probe page.

Which specific probe to launch is based on whatever information was gathered in TCP Null Probe or the service most likely to run on a given port. For example, port 80 is likely to run a web server thus TCP GetRequest probe is likely to get launched. At the same time if both NULL Probe fails and the second probe doesn’t return any results, nmap proceeds to launch every single probe sequentially. Here is an example TCP GetRequest probe definition:

Probe TCP GetRequest q|GET / HTTP/1.0\n\n| rarity 1 ports 1,70,79,80-85,88,113,139,143,280,497,505,514,515,540,554,620,631,783,888,898,900,901,993,995,1026,1080,1214, 1220,1234,1311,1314,1503,1830,1900,2001,2002,2030,2064,2160,2306,2525,2715,2869,3000,3002,3052,3128,3280,3372,3531, 3689,4000,4660,5000,5427,5060,5222,5269,5432,5800-5803,5900,6103,6346,6544,6600,6699,6969,7007,7070,7776,8000-8010, 8080-8085,8118,8181,8443,8880-8888,9001,9030,9050,9080,9090,9999,10000,10005,11371,13013,13666,13722,14534,15000, 18264,40193,50000,55555,4711

sslports 443

From the definition above we can see that for ports 1,70,79,80–85… nmap is going to send out `GET / HTTP/1.0\n\n` request. This should produce signatures that will match common web servers on the net. Also note the rarity attribute is set to “1” which means that this type of probe is extremely popular and will be executed at highest priority.

Here is an example where we are going to get service/version information on port 80 of google.com:

nmap -sV -p80 64.233.187.99 --version-all
This will produce the following packet trace:

# Nmap is SYN Scanning port 80 on google.com and confirms it is open0.000000 192.168.1.100 -> 64.233.187.99 TCP 54087 > www [SYN] Seq=0 Len=0 MSS=1460

0.135261 64.233.187.99 -> 192.168.1.100 TCP www > 54087 [SYN, ACK] Seq=0 Ack=1 Win=8190 Len=0 MSS=1460

0.135291 192.168.1.100 -> 64.233.187.99 TCP 54087 > www [RST] Seq=1 Len=0# Now we start NULL Probe of nmap's service/version detection engine0.264543 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [SYN] Seq=0 Len=0 MSS=1460 TSV=2192037 TSER=0 WS=2

0.370085 64.233.187.99 -> 192.168.1.100 TCP www > 39240 [SYN, ACK] Seq=0 Ack=1 Win=8190 Len=0 MSS=1460

0.370120 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [ACK] Seq=1 Ack=1 Win=5840 Len=0# After waiting for 6 seconds, nmap gives up and based on

# target port 80 sends TCP GetRequest probe instead.6.370004 192.168.1.100 -> 64.233.187.99 HTTP GET / HTTP/1.0

6.477470 64.233.187.99 -> 192.168.1.100 TCP www > 39240 [ACK] Seq=1 Ack=19 Win=5720 Len=0# Google Web server responds with service name and version headers6.483348 64.233.187.99 -> 192.168.1.100 HTTP HTTP/1.0 200 OK (text/html)

6.483367 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [ACK] Seq=19 Ack=1431 Win=8580 Len=0

6.483623 64.233.187.99 -> 192.168.1.100 HTTP Continuation or non-HTTP traffic

6.483634 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [ACK] Seq=19 Ack=2861 Win=11440 Len=0

6.483868 64.233.187.99 -> 192.168.1.100 HTTP Continuation or non-HTTP traffic

6.483881 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [ACK] Seq=19 Ack=4184 Win=14300 Len=0

6.483870 64.233.187.99 -> 192.168.1.100 TCP www > 39240 [FIN, ACK] Seq=4184 Ack=19 Win=5720 Len=0

6.489446 192.168.1.100 -> 64.233.187.99 TCP 39240 > www [RST, ACK] Seq=19 Ack=4185 Win=14300 Len=0

As we can see from the above packet trace the initial NULL Probe didn’t bring any results so Nmap launched GetRequest probe emulating a standard web request. Google returned its standard front page including HTTP Headers revealing server name and version:

HTTP/1.0 200 OK
Cache-Control: private
Content-Type: text/html; charset=ISO-8859-1

Set-Cookie: PREF=ID=XXXX:TM=XXXX:LM=XXXX:S=XXXX; expires=Sun, 17-Jan-2038 19:14:07 GMT; path=/; domain=.google.com

Server: GWS/2.1
Date: Fri, XX XXX XXXX XX:XX:XX GMT
Connection: Close
This response was matches with the following regex entry in nmap’s database:

match http m|^HTTP/1\.[01] \d\d\d .*\nServer: GWS/([\d.]+)\n|s p/Google httpd/ v/$1/ i/GWS/ o/Linux/

As a result, nmap produced the following output:

Interesting ports on 64.233.187.99: PORT STATE SERVICE VERSION 80/tcp open http Google httpd 2.1 (GWS) Service Info: OS: Linux

## Cheats and Fallbacks

To increase reliability, Nmap uses a system of cheats and fallbacks. In case where a target system is slow to respond to nmap’s probes, the actual response may occur when nmap already gave up on Null Probe and is launching some other probe instead. If response returned by the system does not match to anything corresponding to the second probe, nmap cheats by still attempting to match the response to anything in the NULL probe thus increasing chances of correct detection.

In addition to Null Probe cheats, nmap allows definition of fallback Probes that can be launched if any given probe fails.

# OS Detection

Nmaps’s OS detection mechanism relies on different implementations of IP stack across different operating systems. By sending a number of probes to the target host and comparing responses to internal os signature database, Nmap is capable of detecting operating system name and version, uptime, network distance, IPID Sequence Generation, and other information.

Nmap uses two versions of signature databases. The older signature database is less precise and contains less tests, but has a lot more signatures compared to the more recent signature database which introduces better precision. Older database is stored in *nmap-os-fingerprints* while newer signatures are stored in *nmap-os-db* file. Below is a sample signature from *nmap-os-db*:

# XP Professional 5.1, Service Pack 2, Build 2600. Fingerprint Microsoft Windows XP SP2 Class Microsoft | Windows | XP | general purpose SEQ(SP=F4-FE%GCD=<7%ISR=10B-115%TI=I%II=I%SS=S%TS=0) OPS(O1=M5B4NW0NNT00NNS%O2=M5B4NW0NNT00NNS%O3=M5B4NW0NNT00%O4=M5B4NW0NNT00NNS%O5=M5B4NW0NNT00NNS%O6=M5B4NNT00NNS) WIN(W1=4470%W2=41A0%W3=4100%W4=40E8%W5=40E8%W6=402E) ECN(R=Y%DF=Y%T=7F%TG=7F%W=4470%O=M5B4NW0NNS%CC=N%Q=) T1(R=Y%DF=Y%T=7F%TG=7F%S=O%A=S+%F=AS%RD=0%Q=) T2(R=Y%DF=N%T=7F%TG=7F%W=0%S=Z%A=S%F=AR%O=%RD=0%Q=) T3(R=Y%DF=Y%T=7F%TG=7F%W=402E%S=O%A=S+%F=AS%O=M5B4NW0NNT00NNS%RD=0%Q=) T4(R=Y%DF=N%T=7F%TG=7F%W=0%S=A%A=O%F=R%O=%RD=0%Q=) T5(R=Y%DF=N%T=7F%TG=7F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=) T6(R=Y%DF=N%T=7F%TG=7F%W=0%S=A%A=O%F=R%O=%RD=0%Q=) T7(R=Y%DF=N%T=7F%TG=7F%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=) U1(DF=N%T=7F%TG=7F%TOS=0%IPL=B0%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUL=G%RUD=G) IE(DFI=S%T=7F%TG=7F%TOSI=Z%CD=Z%SI=S%DLI=S)

And here is a much shorter version from *nmap-os-fingerprints*:

# Microsoft Windows XP Professional (English) w/ SP2 (Build 2600.xpsp_sp2_rtm.040803-2158 : Service Pack 2) # Widows XP Professional (English UK) SP2 - latest patches as of 20 Dec 2004 - build 2600.xpsp_sp2_rtm.040803-2158 # Microsoft Windows XP Home Edition (French) SP2 build 2600.xpsp_sp2_rtm.040803-2158 # Microsoft Windows XP Profesional (English) SP2 Ver 5.1 build 2600.xpsp_sp2_rtm.040803-2158 : Service Pack 2 Fingerprint Microsoft Widows XP SP2 Class Microsoft | Windows | NT/2K/XP | general purpose TSeq(Class=TR%gcd=<6%IPID=I%TS=U) T1(DF=Y%W=805C|88A4|FC94|FFFF%ACK=S++%Flags=AS%Ops=MNW) T2(Resp=Y%DF=N%W=0%ACK=S%Flags=AR%Ops=) T3(Resp=Y%DF=Y%W=805C|88A4|FC94|FFFF%ACK=S++%Flags=AS%Ops=MNW) T4(DF=N%W=0%ACK=O%Flags=R%Ops=) T5(DF=N%W=0%ACK=S++%Flags=AR%Ops=) T6(DF=N%W=0%ACK=O%Flags=R%Ops=) T7(DF=N%W=0%ACK=S++%Flags=AR%Ops=) PU(DF=N%TOS=0%IPLEN=B0%RIPTL=148%RID=E%RIPCK=E%UCK=E%ULEN=134%DAT=E)

The first keyword *Fingerprint* defines system’s name for which we will specify characteristic ip stack signature. Next like keyword *Class* defines vendor, OS name, OS family, and type of device. Following these two lines are the actual fingerprints collected from probes sent by Nmap. Specific signature attributes can be left blank or defined as either a static value or as a set of possible values. When defining different values common operators like OR (|), Range (-), Greater than (>), and Less than (<).

Details of individual parameters are covered in section *Probes* while descriptions of tests for each for the paramters are covered in section *Tests* below.

## Probes

To start nmap’s os detection engine use a command as follows:
nmap -O 192.168.1.101 -p22 -n -PN

Six packets are sent 110ms apart to an open port on the target host. Below is the packet trace of traffic generated:

# Packet #1: window scale (10), NOP, MSS (1460), timestamp (TSval: 0xFFFFFF; TSecr: 0), SACK permitted. The window field is 10.221494 192.168.1.100 -> 192.168.1.101 TCP 45079 > ssh [SYN] Seq=0 Len=0 WS=10 MSS=1460 TSV=4294967295 TSER=0

0.221611 192.168.1.101 -> 192.168.1.100 TCP ssh > 45079 [SYN, ACK] Seq=504255574 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=533947 TSER=4294967295

0.221629 192.168.1.100 -> 192.168.1.101 TCP 45079 > ssh [RST] Seq=1 Len=0# Packet #2: MSS (1400), window scale (0), SACK permitted, timestamp (TSval: 0xFFFFFF; TSecr: 0). The window field is 63.0.325501 192.168.1.100 -> 192.168.1.101 TCP 45080 > ssh [SYN] Seq=0 Len=0 MSS=1400 WS=0 TSV=4294967295 TSER=0

0.325642 192.168.1.101 -> 192.168.1.100 TCP ssh > 45080 [SYN, ACK] Seq=1875345108 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=534051 TSER=4294967295

0.325661 192.168.1.100 -> 192.168.1.101 TCP 45080 > ssh [RST] Seq=1 Len=0# Packet #3: Timestamp (TSval: 0xFFFFFF; TSecr: 0), NOP, NOP, window scale (5), NOP, MSS (640). The window field is 4.0.429498 192.168.1.100 -> 192.168.1.101 TCP 45081 > ssh [SYN] Seq=0 Len=0 TSV=4294967295 TSER=0 WS=5 MSS=640

0.429602 192.168.1.101 -> 192.168.1.100 TCP ssh > 45081 [SYN, ACK] Seq=3217721325 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=534155 TSER=4294967295

0.429619 192.168.1.100 -> 192.168.1.101 TCP 45081 > ssh [RST] Seq=1 Len=0# Packet #4: SACK permitted, Timestamp (TSval: 0xFFFFFF; TSecr: 0), window scale (10). The window field is 4.0.533498 192.168.1.100 -> 192.168.1.101 TCP 45082 > ssh [SYN] Seq=0 Len=0 TSV=4294967295 TSER=0 WS=10

0.533614 192.168.1.101 -> 192.168.1.100 TCP ssh > 45082 [SYN, ACK] Seq=1209695118 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=534259 TSER=4294967295

0.533633 192.168.1.100 -> 192.168.1.101 TCP 45082 > ssh [RST] Seq=1 Len=0# Packet #5: MSS (536), SACK permitted, Timestamp (TSval: 0xFFFFFF; TSecr: 0), window scale (10). The window field is 16.0.637494 192.168.1.100 -> 192.168.1.101 TCP 45083 > ssh [SYN] Seq=0 Len=0 MSS=536 TSV=4294967295 TSER=0 WS=10

0.637616 192.168.1.101 -> 192.168.1.100 TCP ssh > 45083 [SYN, ACK] Seq=3691572495 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=534363 TSER=4294967295

0.637636 192.168.1.100 -> 192.168.1.101 TCP 45083 > ssh [RST] Seq=1 Len=0# Packet #6: MSS (265), SACK permitted, Timestamp (TSval: 0xFFFFFF; TSecr: 0). The window field is 512.0.741499 192.168.1.100 -> 192.168.1.101 TCP 45084 > ssh [SYN] Seq=0 Len=0 MSS=265 TSV=4294967295 TSER=0

0.741602 192.168.1.101 -> 192.168.1.100 TCP ssh > 45084 [SYN, ACK] Seq=3601587824 Ack=1 Win=65535 Len=0 MSS=1460 TSV=534467 TSER=4294967295

0.741621 192.168.1.100 -> 192.168.1.101 TCP 45084 > ssh [RST] Seq=1 Len=0

The following signature parameters will be gathered by this probe and corresponding tests will be performed:

- SEQ — sequence analysis (GCD, SP, ISR, IPID, TS)
- OPS — TCP options received (01–06)
- WIN — window sizes received (W1-W6)
- T1 — collection of gathered test values (R, DF, T, TG, S, A, F, RD, Q)

Two ICMP echo packets are sent:

# Packet #1: IP DF set, TOS code 9, Seq=295, random IP ID and ICMP

# id 120 byte payload of random characters0.775051 192.168.1.100 -> 192.168.1.101 ICMP Echo (ping) request 0.775183 192.168.1.101 -> 192.168.1.100 ICMP Echo (ping) reply# Packet #2: IP DF set, TOS code 0, Seq=296, IP ID+1, ICMP id+1

# 150 byte payload of random characters0.809496 192.168.1.100 -> 192.168.1.101 ICMP Echo (ping) request 0.809629 192.168.1.101 -> 192.168.1.100 ICMP Echo (ping) reply

The following signature parameter will be gathered by this probe and corresponding tests will be performed:

- IE — (R, DFI, T, TG, TOSI, CD, SI, DLI)

A single UDP packet is sent to a closed port with ‘C’ character repeated 300 times in the data field.

0.845489 192.168.1.100 -> 192.168.1.101 UDP Source port: 44998 Destination port: 41293

0.845631 192.168.1.101 -> 192.168.1.100 ICMP Destination unreachable (Port unreachable)

The following signature parameter will be gathered by this probe and corresponding tests will be performed:

- U1 — (R, DF, T, TG, TOS, IPL, UN, RIPL, RID, RIPCK, RUCK, RUL, RUD)

A single SYN packet is sent with ECN CWR and ECE congestion control flags set:

0.873497 192.168.1.100 -> 192.168.1.101 TCP 45091 > ssh [SYN, ECN, CWR] Seq=0 Len=0 WS=10 MSS=1460

0.873601 192.168.1.101 -> 192.168.1.100 TCP ssh > 45091 [SYN, ACK] Seq=0 Ack=1 Win=65535 Len=0 MSS=1460 WS=1

0.873621 192.168.1.100 -> 192.168.1.101 TCP 45091 > ssh [RST] Seq=1 Len=0

The following signature parameter will be gathered by this probe and corresponding tests will be performed:

- ECN — (R, DF, T, TG, W, O, CC, Q)

Six TCP packets are sent with varying flags set:

# T2 - TCP NULL Win=1280.909497 192.168.1.100 -> 192.168.1.101 TCP 45093 > ssh [] Seq=0 Len=0 WS=10 MSS=265 TSV=4294967295 TSER=0# T3 - SYN FIN URG PSH Win=2560.937498 192.168.1.100 -> 192.168.1.101 TCP 45094 > ssh [FIN, SYN, PSH, URG] Seq=0 Urg=0 Len=0 WS=10 MSS=265 TSV=4294967295 TSER=0 0.937597 192.168.1.101 -> 192.168.1.100 TCP ssh > 45094 [SYN, ACK] Seq=2009364148 Ack=1 Win=65535 Len=0 MSS=1460 WS=1 TSV=534663 TSER=4294967295

0.937615 192.168.1.100 -> 192.168.1.101 TCP 45094 > ssh [RST] Seq=1 Len=0# T4 - ACK Win=10240.965490 192.168.1.100 -> 192.168.1.101 TCP 45095 > ssh [ACK] Seq=0 Ack=0 Win=1024 Len=0 WS=10 MSS=265 TSV=4294967295 TSER=0

0.965577 192.168.1.101 -> 192.168.1.100 TCP ssh > 45095 [RST] Seq=0 Len=0# T5 - SYN Win=313370.993496 192.168.1.100 -> 192.168.1.101 TCP 45096 > 40394 [SYN] Seq=0 Len=0 WS=10 MSS=265 TSV=4294967295 TSER=0

0.993581 192.168.1.101 -> 192.168.1.100 TCP 40394 > 45096 [RST, ACK] Seq=3545403769 Ack=1 Win=0 Len=0# T6 - ACK Win=327681.021489 192.168.1.100 -> 192.168.1.101 TCP 45097 > 40394 [ACK] Seq=0 Ack=0 Win=32768 Len=0 WS=10 MSS=265 TSV=4294967295 TSER=0 1.021615 192.168.1.101 -> 192.168.1.100 TCP 40394 > 45097 [RST] Seq=0 Len=0# T7 - FIN PSH URG Win=655351.049481 192.168.1.100 -> 192.168.1.101 TCP 45098 > 40394 [FIN, PSH, URG] Seq=0 Urg=0 Len=0 WS=15 MSS=265 TSV=4294967295 TSER=0

1.049563 192.168.1.101 -> 192.168.1.100 TCP 40394 > 45098 [RST, ACK] Seq=3545403769 Ack=0 Win=0 Len=0

The following signature parameter will be gathered by this probe and corresponding tests will be performed:

- T2-T7 — (R, DF, T, TG, W, O, CC, Q)

See Nmap OS Detection page page for a complete listing and explanation of all tests.

# Firewall/IDS Evasion and Spoofing

## Fragmented Packets

For networks which do no queue IP fragments, nmap can fragment packets it sends in order to evade firewalls. Here is a sample command line to use fragmented packets:

nmap -f 192.168.1.1 -p443 -PN -n
Traffic generated from the above command would be:

0.052163 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=0)

0.052168 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=8)

0.052173 192.168.1.100 -> 192.168.1.1 TCP 35955 > https [SYN] Seq=0 Len=0 MSS=1460

0.052811 192.168.1.1 -> 192.168.1.100 TCP https > 35955 [SYN, ACK] Seq=0 Ack=1 Win=5840 Len=0 MSS=1460

0.052833 192.168.1.100 -> 192.168.1.1 TCP 35955 > https [RST] Seq=1 Len=0
You can specify your own fragment size:
nmap --mtu 16 192.168.1.1 -p443 -PN -n --data-length 80
This will produce the following traffic:

0.082687 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=0)

0.082916 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=16)

0.083007 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=32)

0.083092 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=48)

0.083177 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=64)

0.083263 192.168.1.100 -> 192.168.1.1 IP Fragmented IP protocol (proto=TCP 0x06, off=80)

0.083364 192.168.1.100 -> 192.168.1.1 SSL Continuation Data

0.083873 192.168.1.1 -> 192.168.1.100 TCP https > 60414 [SYN, ACK] Seq=0 Ack=1 Win=5840 Len=0 MSS=1460

0.083892 192.168.1.100 -> 192.168.1.1 TCP 60414 > https [RST] Seq=1 Len=0

Fragmented packets are not effective against packet filters and queuing firewalls.

## Decoys

Nmap is capable of sending multiple port scan requests from spoofed hosts in parallel with the real scanning host. When spoofing hosts it is recommended to make sure they are up in order to avoid SYN flooding the target. Also it is recommended to avoid the use of nameserver so as not to reveal scanner’s real ip address. Here is a command that uses 3 decoys to peform the scan:

nmap -D 64.233.167.99,207.46.197.32,ME -sS 192.168.1.1 -p443 -n -PN
This will produce the following trace:

# decoys0.063884 64.233.167.99 -> 192.168.1.1 TCP 36588 > https [SYN] Seq=0 Len=0 MSS=1460

0.064575 207.46.197.32 -> 192.168.1.1 TCP 36588 > https [SYN] Seq=0 Len=0 MSS=1460# real syn scan0.064698 192.168.1.100 -> 192.168.1.1 TCP 36588 > https [SYN] Seq=0 Len=0 MSS=1460

0.065288 192.168.1.1 -> 192.168.1.100 TCP https > 36588 [SYN, ACK] Seq=0 Ack=1 Win=5840 Len=0 MSS=1460

0.065317 192.168.1.100 -> 192.168.1.1 TCP 36588 > https [RST] Seq=1 Len=0

## Spoof source address

Source ip address can be completely spoofed, lets ask google to scan microsoft
nmap -sS -p80 207.46.197.32 -S 64.233.167.99 -e eth0 -PN -n

This will simply generate the SYN request, but we will never hear the response since it will be routed to google.com:

0.000000 64.233.167.99 -> 207.46.197.32 TCP 54933 > www [SYN] Seq=0 Len=0 MSS=1460

Of course spoofing source address becomes even more useful when it is possible to intercept traffic coming and going to the spoofed host. For example, in this scan I will use Ettercap to arp poison 192.168.1.101 so that all traffic is redirected to my host 192.168.1.100. Although the initial SYN will be forged on behalf of 192.168.1.101, I will still be able to learn the response from the target host by intercepting all traffic:

# First arp poison 192.168.1.101ettercap -T -M arp:remote /192.168.1.1/ /192.168.1.101/# Spoof source address and scan the target hostnmap -sS -p80 207.46.197.32 -S 192.168.1.101 -e eth0 -PN -n

This will generate the following traffic:

# Spoof the request0.000000 192.168.1.101 -> 207.46.197.32 TCP 38966 > www [SYN] Seq=0 Len=0 MSS=1460# Intercept the response0.053994 207.46.197.32 -> 192.168.1.101 TCP www > 38966 [SYN, ACK] Seq=0 Ack=1 Win=16384 Len=0 MSS=1460# Forward it to spoofed host0.054085 207.46.197.32 -> 192.168.1.101 TCP www > 38966 [SYN, ACK] Seq=0 Ack=1 Win=16384 Len=0 MSS=1460# Nmap sends RST back to target host0.054200 192.168.1.101 -> 207.46.197.32 TCP 38966 > www [RST] Seq=1 Len=0# Spoofed host sends RST for an unexpected SYN/ACK0.054236 192.168.1.101 -> 207.46.197.32 TCP 38966 > www [RST] Seq=1 Len=0

## Spoof source port number

Spoofing scanner originating port number can be used to trick firewalls that only allow traffic originating from specific ports such as 53(dns):

nmap -sS -p80 207.46.197.32 -g53 -PN -n
This scan will produce the following trace:

# Sending SYN scan originating from port 53

0.000000 192.168.1.100 -> 207.46.197.32 TCP 53 > www [SYN] Seq=0 Len=0 MSS=1460

0.191068 207.46.197.32 -> 192.168.1.100 TCP www > 53 [SYN, ACK] Seq=0 Ack=1 Win=16384 Len=0 MSS=1460

0.191108 192.168.1.100 -> 207.46.197.32 TCP 53 > www [RST] Seq=1 Len=0

## Append random data to sent packets

In order to make nmap scans slightly less apparent it is possible to attach random data to all requests:

nmap -sS -p80 207.46.197.32 --data-length 100 -PN -n
Packet trace for the above scan:

# SYN packet with 100 byte random data appended to it0.000000 192.168.1.100 -> 207.46.197.32 HTTP Continuation or non-HTTP traffic0000 00 16 b6 28 55 08 00 0c 6e 6b cc 73 08 00 45 00 ...(U... nk.s..E.

0010 00 90 f5 59 00 00 31 06 3d b3 c0 a8 01 64 cf 2e ...Y..1. =....d..
0020 c5 20 e5 64 00 50 fd 24 7f 62 00 00 00 00 60 02 . .d.P.$ .b....`.
0030 08 00 c8 b4 00 00 02 04 05 b4 f1 fc 9b 76 3e b8 ........ .....v>.
0040 56 dd 65 39 8b 7c 4d c4 13 2c c9 c6 95 a2 e8 01 V.e9.|M. .,......
0050 97 50 d6 7b bb 26 2e 0e 4a e1 2a f5 e0 40 76 b0 .P.{.&.. J.*..@v.
0060 7e c2 92 ad 3b 4f c7 a3 2d e5 3e d5 e0 30 0e c7 ~...;O.. -.>..0..
0070 27 75 c8 f1 71 e0 70 e2 bc b5 c2 a5 b1 c7 5a 50 'u..q.p. ......ZP
0080 c8 87 3d e5 9e 16 28 05 ea b0 6a 0c b8 12 9f 1d ..=...(. ..j.....

0090 10 2c a5 5a 87 e9 67 3c 37 91 bf 22 4c 08 .,.Z..g< 7.."L.0.037300 207.46.197.32 -> 192.168.1.100 TCP www > 62395 [SYN, ACK] Seq=0 Ack=1 Win=16384 Len=0 MSS=1460

0.037330 192.168.1.100 -> 207.46.197.32 TCP 62395 > www [RST] Seq=1 Len=0

## IP options

Users may also set different ip options to further obfuscate the scan, determine a path packets take to the target or even attempt to bypass firewalls by selecting different route.

The following IP options are supported by Nmap in a form of shortcuts:

- R — record route
- T — record internet timestamps
- U — record timestamps and ip addresses
- L … — loose source routing
- S … — strict source routing

Below is a command line to execute Ping Scan with Timestamp IP option enabled:
nmap -sP — ip-options “T” 4.2.2.1

The above shortcuts can be avoided completely by specifying custom IP options in a hexadecimal form:

nmap -sP — ip-options “\x44\x24\x05\x00\x00*32” 4.2.2.1

## Set IP TTL

Nmap can set IP’s TTL field:
nmap -sP 4.2.2.1 — ttl 30

## Spoof MAC address

When performing a scan on a local ethernet network ( — send-eth enabled), Nmap can optionally spoof scanning interface MAC address:

nmap -sP 10.0.0.1 — spoof-mac 00:11:22:33:44:55

## Send bogus TCP/UDP checksums

As a measure to detect firewalls, IDSs, or other devices that do not perform checksum analysis, nmap can send malformed TCP/UDP checksums to illicit response from such devices with an additional *— badsum* option.

# Traceroute

Latest releases of Nmap (4.20Alpha4) integrate a TCP based traceroute mechanism which contrasts with the classic approach of ICMP method commonly blocked on the internet. Below is a sample command to start traceroute to google.com on port 80:

nmap -sS -p80 -n 72.14.207.99 --traceroute
This produces the following packet trace:

# First nmap performs standard host discovery with icmp echo request0.000000 192.168.1.71 -> 4.2.2.1 ICMP Echo (ping) request TTL=47 0.000072 192.168.1.71 -> 4.2.2.1 TCP 61120 > www [ACK] Seq=0 Ack=0 Win=1024 Len=0 TTL=48

0.022076 4.2.2.1 -> 192.168.1.71 ICMP Echo (ping) reply TTL=247# Now we perform a standard SYN scan on port 80 and get RST back (port closed)0.428388 192.168.1.71 -> 4.2.2.1 TCP 61099 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=57

0.448015 4.2.2.1 -> 192.168.1.71 TCP www > 61099 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0 TTL=57# Traceroute part of the scan sends initial prove with TTL set to

# 255, this is used to estimate number of hops to the target0.731381 192.168.1.71 -> 4.2.2.1 TCP 35047 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=255# Target host responded with TTL of 57. A mechanism similar to OS

# Detection estimates that host's MAX_TTL is 64 so we can estimate

# the number of hops by substracting received TTL (57) from MAX_TTL

# (64) which gives us # 7 hops from the target.0.749600 4.2.2.1 -> 192.168.1.71 TCP www > 35047 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0 TTL=57# Now we send more probes with an estimate TTL + 1 as a starting

# point and increase it until we get a response0.749734 192.168.1.71 -> 4.2.2.1 TCP 35048 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=8

0.749786 192.168.1.71 -> 4.2.2.1 TCP 35049 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=9# hop 80.767319 4.2.2.1 -> 192.168.1.71 TCP www > 35048 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0 TTL=57# Now that we got the upper bound we can decrement TTL to find all

# hosts in between0.767350 192.168.1.71 -> 4.2.2.1 TCP 35050 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=7

0.767387 192.168.1.71 -> 4.2.2.1 TCP 35051 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=6# hop 9 which is discarded since we already got the upper bound0.775198 4.2.2.1 -> 192.168.1.71 TCP www > 35049 [RST, ACK] Seq=0 Ack=1 Win=0 Len=0 TTL=57# Continue sending probes with decreasing TTLs0.775228 192.168.1.71 -> 4.2.2.1 TCP 35052 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=5# hop 70.785094 4.68.101.164 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=247

0.785189 192.168.1.71 -> 4.2.2.1 TCP 35053 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=4# hop 60.787522 4.68.110.197 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=247

0.787565 192.168.1.71 -> 4.2.2.1 TCP 35054 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=3# hop 50.793147 151.164.191.250 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=248

0.793184 192.168.1.71 -> 4.2.2.1 TCP 35055 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=2# hop 40.802876 70.239.122.33 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=252

0.803028 192.168.1.71 -> 4.2.2.1 TCP 35056 > www [SYN] Seq=0 Len=0 MSS=1460 TTL=1# hop 30.805313 65.43.19.227 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=253# hop 10.807392 192.168.1.254 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=255# hop 20.809030 68.254.175.254 -> 192.168.1.71 ICMP Time-to-live exceeded (Time to live exceeded in transit) TTL=63

With the above parameters nmap produces the following output:
Interesting ports on 4.2.2.1:
PORT STATE SERVICE
80/tcp closed httpTRACEROUTE (using port 80/tcp)
HOP RTT ADDRESS
1 1.95 192.168.1.254
2 15.86 68.254.175.254
3 15.78 65.43.19.227
4 15.75 70.239.122.33
5 21.59 151.164.191.250
6 185.06 4.68.110.197
7 19.25 4.68.101.132
8 17.72 4.2.2.1Nmap finished: 1 IP address (1 host up) scanned in 1.696 seconds

# Misc Features

With the power of nmap’s firewall/ids evasion flags it is possible to launch several popular attacks:

This LAND Attack will crash vulnerable systems:
nmap -sS 192.168.1.123 -S 192.168.1.123 -p139 -g139 -e eth0
Nmap can be executed in interactive mode:
nmap --interactive
From there a user can perform the following actions:

- `n` — execute an nmap scan with provided arguments
- `f [--spoof] [--nmap-path]`— launches nmap in the background, uses spoofed fakeargs (vi textfile) to hide what you are doing in the process list. You may also execute nmap from a defined nmap-path.
- `x` — exit

# Local Privilege Escalation

If nmap is started in interactive mode with root privileges, arbitrary commands may be executed by when using the following syntax:

nmap> ! id uid=0(root) gid=0(root) groups=0(root)

# External Links

- [Official NMap page](http://insecure.org/nmap/)
- [Phrack 51 — The Art of Port Scanning](http://www.phrack.org/archives/51/P51-11)
- [Service and Application Version Detection](http://insecure.org/nmap/vscan/)
- [Remote OS Detection via TCP/IP Fingerprinting v2](http://insecure.org/nmap/osdetect/)
- [Remote OS Detection via TCP/IP Fingerprinting v1](http://insecure.org/nmap/nmap-fingerprinting-old.html)
- [Send packets with specified ip options](http://seclists.org/nmap-dev/2006/q3/0052.html)
- [Nmap traceroute](http://seclists.org/nmap-dev/2006/q3/0285.html)
- [Nmap Scripting Engine](http://insecure.org/nmap/nse/index.html)