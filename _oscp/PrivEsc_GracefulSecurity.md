PrivEsc | GracefulSecurity

# Tag: PrivEsc

[Build Security](https://gracefulsecurity.com/category/build-security/) / [Infrastructure](https://gracefulsecurity.com/category/infrastructure/)

## [Linux PrivEsc: Abusing SUID](https://gracefulsecurity.com/linux-privesc-abusing-suid/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[May 25, 2016](https://gracefulsecurity.com/linux-privesc-abusing-suid/)

Recently during a CTF I found a few users were unfamiliar with abusing setuid on executable on Linux systems for the purposes of privilege escalation. If an executable file on Linux has the “suid” bit set when a user executes a file it will execute with the owners permission level and not the executors permission level. Meaning if you find a file with this bit set, which is owned by a user with a higher privilege level than yourself you may be able to steal their permissions set.

[Read More](https://gracefulsecurity.com/linux-privesc-abusing-suid/)

[Build Security](https://gracefulsecurity.com/category/build-security/)

## [PrivEsc: DLL Hijacking](https://gracefulsecurity.com/privesc-dll-hijacking/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[January 3, 2016](https://gracefulsecurity.com/privesc-dll-hijacking/)

I posted earlier about Privilege Escalation through Unquoted Service Paths and how it’s now rare to be able to exploit this in the real world due to the protected nature of the C:\Program Files and C:\Windows directories. It’s still possible to exploit this vulnerability, but only when the service executable is installed outside of these protect directories which in my experience is rare. Writing that post though got me thinking about another method of privilege escalation which I think is a little more common to see – DLL Hijacking.

[Read More](https://gracefulsecurity.com/privesc-dll-hijacking/)

[Infrastructure](https://gracefulsecurity.com/category/infrastructure/)

## [PrivEsc: Insecure Service Permissions](https://gracefulsecurity.com/privesc-insecure-service-permissions/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[January 3, 2016](https://gracefulsecurity.com/privesc-insecure-service-permissions/)

/I’ve written a few articles recently about methods of escalating privileges on Windows machines, such as through DLL Hijacking and Unquoted Service Paths, so here I’m continuing the series with Privilege Escalation through Insecure Service configurations. This one’s  pretty simple issue really, generally speaking it’s simply a matter of altering the service so that it runs the executable and parameters you want it to, instead the default configuration allowing you to supply a command and privilege level for the execution. So you can simply run the add user command as local system and create your own local administrator account!

[Read More](https://gracefulsecurity.com/privesc-insecure-service-permissions/)

[Build Security](https://gracefulsecurity.com/category/build-security/)

## [PrivEsc: Unquoted Service Path](https://gracefulsecurity.com/privesc-unquoted-service-path/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[January 2, 2016](https://gracefulsecurity.com/privesc-unquoted-service-path/)

A couple of days ago I posted an article about the first steps an attacker would likely take to perform a desktop breakout attack. Where that post left off was at the point of looking for privilege escalation from domain user to local administrator.

[Read More](https://gracefulsecurity.com/privesc-unquoted-service-path/)

[Infrastructure](https://gracefulsecurity.com/category/infrastructure/)

## [PrivEsc: Stealing Windows Access Tokens – Incognito](https://gracefulsecurity.com/privesc-stealing-windows-access-tokens-incognito/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[October 18, 2015](https://gracefulsecurity.com/privesc-stealing-windows-access-tokens-incognito/)

If an attacker is able to get SYSTEM level access to a workstation, for example by compromising a local administrator account, and a Domain Administrator account is logged in to that machine then it may be possible for the attacker to simply read the administrator’s access token in memory and steal it to allow them to impersonate that account. There’s a tool available to do this, it’s called Incognito.

[Read More](https://gracefulsecurity.com/privesc-stealing-windows-access-tokens-incognito/)

[Infrastructure](https://gracefulsecurity.com/category/infrastructure/)

## [PrivEsc: Privilege Escalation in Windows Domains](https://gracefulsecurity.com/privesc-privilege-escalation-in-windows-domains/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[September 27, 2015](https://gracefulsecurity.com/privesc-privilege-escalation-in-windows-domains/)

During Penetration Testing engagements one of my favourite issues to exploit is a Domain User with Local Administrator permissions. It’s a pretty common issue to see and when speaking to IT Departments about the issue it seems that the risk is often under-estimated. So a user has been given administrative permission over one workstation – what’s the worst that can happen?

[Read More](https://gracefulsecurity.com/privesc-privilege-escalation-in-windows-domains/)

[Infrastructure](https://gracefulsecurity.com/category/infrastructure/)

## [PrivEsc: Dumping Passwords in Plaintext – Mimikatz](https://gracefulsecurity.com/privesc-dumping-passwords-in-plaintext-mimikatz/)

 **by [HollyGraceful](https://gracefulsecurity.com/author/user/)**[September 27, 2015](https://gracefulsecurity.com/privesc-dumping-passwords-in-plaintext-mimikatz/)

A tool exists for dumping plaintext passwords out of memory on Windows, it requires Local Administrator level privileges but it’s a great tool for privilege escalation from Local Admin to Domain Admin. There are Windows EXEs available but it’s also been rolled into Meterpreter! It can also inject a hash into memory to effectively perform a local pass-the-hash attack! If you want to run it on a remote machine remember to check out this post on running remote commands on Windows machines.

[Read More](https://gracefulsecurity.com/privesc-dumping-passwords-in-plaintext-mimikatz/)