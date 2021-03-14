socat

[dest-unreach](http://www.dest-unreach.org/) / **socat**

# socat - Multipurpose relay

## Abstract

what: "netcat++" (extended design, new implementation)
OS: AIX, BSD, HP-UX, Linux, Solaris e.a. (UNIX)
lic: GPL2
inst: tar x...; ./configure; make; make install
doc: README; socat.html, socat.1; xio.help
ui: command line
exa: socat TCP6-LISTEN:8080,reuseaddr,fork PROXY:proxy:www.domain.com:80
keyw: tcp, udp, ipv6, raw ip, unix-socket, pty, pipe, listen, socks4, socks4a,
proxy-connect, ssl-client, filedescriptor, readline, stdio,
exec, system, file, open, tail -f, termios, setsockopt, chroot,
fork, perm, owner, trace, dump, dgram, ext3, resolver, datagram,
multicast, broadcast, interface, socket, sctp, generic, ioctl

## What's new?

2020-01-05: Socat [version 1.7.3.4](http://www.dest-unreach.org/socat/download/socat-1.7.3.4.tar.gz) fixes a few bugs most of which were regressions, see [CHANGES](http://www.dest-unreach.org/socat/doc/CHANGES). All people that sent feature contributions or recommended new features: Sorry, this is only a bug fix and porting release. Please be patient!

## History

2019-04-06: Socat [version 1.7.3.3](http://www.dest-unreach.org/socat/download/socat-1.7.3.3.tar.gz) fixes a couple of bugs and porting issues, see [CHANGES](http://www.dest-unreach.org/socat/doc/CHANGES).

2017-01-25: Socat [version 1.7.3.2](http://www.dest-unreach.org/socat/download/socat-1.7.3.2.tar.gz) fixes uninterruptable hang / CPU loop on host resolution problems, some compile problems, and lots of other bugs and porting issues ([socat-1.7.3.2.tar.gz](http://www.dest-unreach.org/socat/download/socat-1.7.3.2.tar.gz), [socat-1.7.3.2.tar.bz2](http://www.dest-unreach.org/socat/download/socat-1.7.3.2.tar.bz2), [socat-1.7.3.2.patch.gz](http://www.dest-unreach.org/socat/download/socat-1.7.3.2.patch.gz)).

2016-02-01: [Socat security advisory 7 and MSVR-1499: "Bad DH p parameter in OpenSSL"](http://www.dest-unreach.org/socat/contrib/socat-secadv7.html) and [Socat security advisory 8: "Stack overflow in arguments parser"](http://www.dest-unreach.org/socat/contrib/socat-secadv8.html) are published, fixes available in [socat-1.7.3.1.tar.gz](http://www.dest-unreach.org/socat/download/socat-1.7.3.1.tar.gz) and [socat-2.0.0-b9.tar.gz](http://www.dest-unreach.org/socat/download/socat-2.0.0-b9.tar.gz).

2015-04-06: [Socat version 2 beta 8](http://www.dest-unreach.org/socat/download/socat-2.0.0-b8.tar.gz) (or [bz2](http://www.dest-unreach.org/socat/download/socat-2.0.0-b8.tar.bz2)) fixes the [possible denial of service attack](http://www.dest-unreach.org/socat/contrib/socat-secadv6.txt) (CVE-2015-1379), contains all fixes introduced up to 1.7.3.0 and corrects a few version 2 specific issues.

2015-02-08: Actual corrections to version 1.7.3.0 are available in git repository [git://repo.or.cz/socat.git](http://www.dest-unreach.org/socat/git://repo.or.cz/socat.git), branch fixes.

2015-01-24: Socat version 1.7.3.0 fixes a [possible denial of service attack](http://www.dest-unreach.org/socat/contrib/socat-secadv6.txt) (CVE-2015-1379), improves SSL client security, and provides a couple of bug and porting fixes, see [CHANGES](http://www.dest-unreach.org/socat/doc/CHANGES).

Download [gz](http://www.dest-unreach.org/socat/download/socat-1.7.3.0.tar.gz) or [bz2](http://www.dest-unreach.org/socat/download/socat-1.7.3.0.tar.bz2)

2014-03-09: Socat version 1.7.2.4 contains fixes for most of the bugs and porting issues reported or found in more than two years.

Download [gz](http://www.dest-unreach.org/socat/download/socat-1.7.2.4.tar.gz), [bz2](http://www.dest-unreach.org/socat/download/socat-1.7.2.4.tar.bz2), or [patch](http://www.dest-unreach.org/socat/download/socat-1.7.2.4.patch.gz)

2014-01-28: Socat versions 1.7.2.3 and 2.0.0-b7 fix a security issue: socats PROXY-CONNECT address was vulnerable to a buffer overflow with data provided on command line (CVE-2014-0019, [advisory](http://www.dest-unreach.org/socat/contrib/socat-secadv5.txt)). Fixed versions are [1.7.2.3](http://www.dest-unreach.org/socat/download/socat-1.7.2.3.tar.gz) and [2.0.0-b7](http://www.dest-unreach.org/socat/download/socat-2.0.0-b7.tar.gz). Patches are available in the [download area](http://www.dest-unreach.org/socat/download/).

2013-05-26: Socat versions 1.7.2.2 and 2.0.0-b6 fix a security issue: Under certain circumstances an FD leak occurs and may be misused for denial of service attacks against socat running in server mode (CVE-2013-3571, [advisory](http://www.dest-unreach.org/socat/contrib/socat-secadv4.html)). Fixed versions are [1.7.2.2](http://www.dest-unreach.org/socat/download/socat-1.7.2.2.tar.gz) and [2.0.0-b6](http://www.dest-unreach.org/socat/download/socat-2.0.0-b6.tar.gz). Patches are available in the [download area](http://www.dest-unreach.org/socat/download/).

2012-05-14: A heap based buffer overflow vulnerability has been found with data that happens to be output on the READLINE address. Successful exploitation may allow an attacker to execute arbitrary code with the privileges of the socat process (CVE-2012-0219, [advisory](http://www.dest-unreach.org/socat/contrib/socat-secadv3.html)). Fixed versions are [1.7.2.1](http://www.dest-unreach.org/socat/download/socat-1.7.2.1.tar.gz) and [2.0.0-b5](http://www.dest-unreach.org/socat/download/socat-2.0.0-b5.tar.gz). Patches are available in the [download area](http://www.dest-unreach.org/socat/download/).

2011-12-05: socat version 1.7.2.0 allows tun/tap interfaces without IP address and introduces options [openssl-compress](http://www.dest-unreach.org/socat/doc/socat.html#OPTION_OPENSSL_COMPRESS) and [max-children](http://www.dest-unreach.org/socat/doc/socat.html#OPTION_MAX_CHILDREN). It fixes 18 bugs and has 11 changes for improved platform support, especially for Mac OS X Lion, DragonFly, and Android.

2011-05-29: Michael Terzo provided a [patch](http://www.dest-unreach.org/socat/contrib/socat2-nonlinux-1.html) that fixes the compile error of socat 2.0.0 up to b4 on non-Linux systems.

2010-10-03: Vitali Shukela provided a [patch](http://www.dest-unreach.org/socat/contrib/socat-redirect.html) that allows to use the original target address of an accepted connection in a socks or proxy address.

2010-08-02: A stack overflow vulnerability has been fixed that could be triggered when command line arguments were longer than 512 bytes (CVE-2010-2799, [advisory](http://www.dest-unreach.org/socat/contrib/socat-secadv2.html)). Fixed versions are [1.7.1.3](http://www.dest-unreach.org/socat/download/socat-1.7.1.3.tar.gz) and [2.0.0-b4](http://www.dest-unreach.org/socat/download/socat-2.0.0-b4.tar.gz). See [socat security advisory 2](http://www.dest-unreach.org/socat/contrib/socat-secadv2.html) for details.

2009-04-04: the third beta version (2.0.0-b3) of [socat version 2](http://www.dest-unreach.org/socat/socat-version2.html) is ready for [download](http://www.dest-unreach.org/socat/download/socat-2.0.0-b3.tar.gz). It contains all new bug fixes and features of 1.7.1.0 (plus fix:setenv, see below) and introduces the possibility to integrate external programs in address chains (see [doc/socat-addresschain.html](http://www.dest-unreach.org/socat/doc/socat-addresschain.html) and [doc/socat-exec.html](http://www.dest-unreach.org/socat/doc/socat-exec.html)).

## Get it!

You can download [socat 1.7.3.4](http://www.dest-unreach.org/socat/download/) in source form ([.gz](http://www.dest-unreach.org/socat/download/socat-1.7.3.4.tar.gz),[.bz2](http://www.dest-unreach.org/socat/download/socat-1.7.3.4.tar.bz2)). Feel free to check the [md5](http://www.dest-unreach.org/socat/download.md5sum) or [sha256](http://www.dest-unreach.org/socat/download.sha256sum)hashes.

Git [repository](http://www.dest-unreach.org/socat/git://repo.or.cz/socat.git) containing socat 1.6.0.0 and all later version 1 releases is available.

There is a page with socat [patches and contributions.](http://www.dest-unreach.org/socat/contrib/)

## Documentation

Classical documentation:

- [README](http://www.dest-unreach.org/socat/doc/README) file
- [socat man page in HTML format](http://www.dest-unreach.org/socat/doc/socat.html).
- [examples](http://www.dest-unreach.org/socat/doc/socat.html#EXAMPLES) section of the man page

Mini tutorials:

- [Securing traffic between two socat instances using SSL](http://www.dest-unreach.org/socat/doc/socat-openssltunnel.html)
- [IP multicasting with socat](http://www.dest-unreach.org/socat/doc/socat-multicast.html)
- [Building TUN based virtual networks with socat](http://www.dest-unreach.org/socat/doc/socat-tun.html)
- [socat-gender.txt](http://www.dest-unreach.org/socat/doc/socat-gender.txt) contains a simple TCP `gender changer´ example.
- [Generic sockets with socat](http://www.dest-unreach.org/socat/doc/socat-genericsocket.html)

## Contact

If you have more questions, please contact[socat@dest-unreach.org](http://www.dest-unreach.org/socat/mailto:socat@dest-unreach.org)