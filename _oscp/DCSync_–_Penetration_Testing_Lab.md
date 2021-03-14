DCSync – Penetration Testing Lab

It is very common during penetration tests where domain administrator access has been achieved to extract the password hashes of all the domain users for offline cracking and analysis. These hashes are stored in a database file in the domain controller (NTDS.DIT) with some additional information like group memberships and users.

The NTDS.DIT file is constantly in use by the operating system and therefore cannot be copied directly to another location for extraction of information. This file can be found in the following Windows location:

1
[object Object]

There are various techniques that can be used to extract this file or the information that is stored inside it however the majority of them are using one of these methods:

1. Domain Controller Replication Services
2. Native Windows Binaries
3. WMI

## Mimikatz

Mimikatz has a feature (dcsync) which utilises the Directory Replication Service (DRS) to retrieve the password hashes from the NTDS.DIT file. This technique eliminates the need to authenticate directly with the domain controller as it can be executed from any system that is part of the domain from the context of domain administrator. Therefore it is the standard technique for red teams as it is less noisy.

1
[object Object][object Object]  [object Object][object Object]  [object Object]

![mimikatz-dump-domain-hashes-via-dcsync-clear-version.png](../_resources/54bdd94f7dd4b4cb1140bdfa0593084d.png)

Mimikatz – Dump Domain Hashes via DCSync

By specifying the domain username with the **/user** parameter Mimikatz can dump all the account information of this particular user including his password hash.

1
[object Object][object Object]  [object Object]
![mimikatz-dump-user-hash-via-dcsync.png](../_resources/35c1edd723e1b41ea79bb885f114f4c2.png)
Mimikatz – Dump User Hash via DCSync

Alternatively executing Mimikatz directly in the domain controller password hashes can be dumped via the lsass.exe process.

1
2
[object Object]
[object Object]

![mimikatz-dump-domain-hashes-via-lsass.png](../_resources/c04112324cc597a217167f8e06e4109c.png)

Mimikatz – Dump Domain Hashes via lsass
The password hashes of the domain users will retrieved.

![mimikatz-dump-domain-hashes-via-lsadump.png](../_resources/4403c7b39e81015caf52453d5b50d65f.png)

Mimikatz – Dump domain hashes via lsadump

## Empire

PowerShell Empire has two modules which can retrieve domain hashes via the DCSync attack. Both modules needs to be executed from the perspective of domain administrator and they are using Microsoft replication services. These modules rely on the **Invoke-Mimikatz** PowerShell script in order to execute Mimikatz commands related to DCSync. The following module will extract the domain hashes to a format similar to the output of Metasploit **hashdump** command.

1
[object Object]
![empire-dcsync-hashdump-module-clean.png](../_resources/6586ab29fa2a8faf14371cf3815e337d.png)
Empire – DCSync Hashdump Module

The **DCSync** module requires a user to be specified in order to extract all the account information.

![empire-dcsync-module.png](../_resources/4eb02f719ebf9644cb5c98088c1b586f.png)
Empire – DCSync Module
The following information will obtained:
![empire-dcsync-account-information.png](../_resources/935e84ae7f7594f674238a5b3f7fe8f0.png)
Empire – DCSync Account Information

## Nishang

[Nishang](https://github.com/samratashok/nishang) is a PowerShell framework which enables red teamers and penetration testers to perform offensive operations against systems. The [Copy-VSS](https://github.com/samratashok/nishang/blob/master/Gather/Copy-VSS.ps1) script can be used to automatically extract the required files: NTDS.DIT, SAM and SYSTEM. The files will be extracted into the current working directory or into any other folder that will specified.

1
2
3
[object Object][object Object]
[object Object]
[object Object]
![nishang-extract-ntds-powershell.png](../_resources/585ccf64964cc61e99039fdd0c6c92d8.png)
Nishang – Extract NTDS PowerShell

Alternatively the script can be executed from an existing Meterpreter session by loading the PowerShell extension.

1
2
3
[object Object]
[object Object][object Object]
[object Object]
![nishang-extract-ntds-meterpreter.png](../_resources/dcdf583cdaf2285ad46bcdca8e732ce5.png)

It is also possible to establish a direct PowerShell session with the command **powershell_shell** in order to extract the files once the script has been imported to the existing Meterpreter session.

1
2
[object Object]
[object Object]

![nishang-extract-ntds-meterpreter-powershell.png](../_resources/b5670c26abf91762d51c49c7d33fd9c8.png)

Nishang – Extract NTDS Meterpreter PowerShell

## PowerSploit

PowerSploit contains a PowerShell script which utilizes the volume shadow copy service to create a new volume that could be used for extraction of files.

1
2
3
[object Object][object Object]
[object Object]
[object Object]
![powersploit-volumeshadowcopytools.png](../_resources/3f5fddf903dec80f130c69e203110c8c.png)
PowerSploit – VolumeShadowCopyTools

Alternatively it can be executed from an existing Meterpreter session by loading the PowerShell extension.

1
2
3
[object Object]
[object Object]
[object Object]
![powersploit-volume-shadow-copy.png](../_resources/70a3db98123779663c74a91bd8007fcf.png)
PowerSploit – Volume Shadow Copy

Files can then copied from the new volume to a destination path with the command **copy**.

## Invoke-DCSync

The [Invoke–DCSync](https://gist.github.com/monoxgas/9d238accd969550136db) is a PowerShell script that was developed by [Nick Landers](https://twitter.com/monoxgas) and leverages PowerView, Invoke-ReflectivePEInjection and a DLL wrapper of PowerKatz to retrieve hashes with the Mimikatz method of DCSync. Executing directly the function will generate the following output:

1
[object Object]
![invoke-dcsync-powershell.png](../_resources/396d6a1de856c0a09e143f7bc6fec295.png)
Invoke-DCSync – PowerShell

The results will be formatted into four tables: Domain, User, RID and Hash. However executing the **Invoke-DCSync** with the parameter **-PWDumpFormat** will retrieve the hashes in the format: **user:id:lm:ntlm:::**

1
[object Object]

![invoke-dcsync-powershell-pwdump-format.png](../_resources/fddbe0403b5a8f78def22ff8fbf3a6d9.png)

Invoke-DCSync – PowerShell PWDump Format

The same output can be achieved by running the script from an existing Meterpreter session.

![invoke-dcsync-metasploit.png](../_resources/bee6cb076bbfbb0d8fd87255d142aeb3.png)
Invoke-DCSync Metasploit
With the PWDumpFormat:

![invoke-dcsync-metasploit-pwdump-format.png](../_resources/ed070422e48c6e17d7df8e1fdbc2ba9d.png)

Invoke-DCSync – Metasploit PWDump Format

## ntdsutil

The **ntdsutil** is a command line tool that is part of the domain controller ecosystem and its purpose is to enable administrators to access and manage the windows Active Directory database. However it can be abused by penetration testers and red teams to take a snapshot of the existing ntds.dit file which can be copied into a new location for offline analysis and extraction of password hashes.

1
2
3
4
5
6
[object Object]
[object Object]
[object Object]
[object Object]
[object Object]
[object Object]
![ntdsutil.png](../_resources/86e44e3a8f530657e343e942f76c2fef.png)
ntdsutil

Two new folders will be generated: Active Directory and Registry. The NTDS.DIT file will be saved in the Active Directory and the SAM and SYSTEM files will be saved into the Registry folder.

![ntdsutil-ntds.png](../_resources/e536c9737366bd95e4a9144e555a313d.png)
ntdsutil – ntds

## DiskShadow

**DiskShadow** is a Microsoft signed binary which is used to assist administrators with operations related to the Volume Shadow Copy Service (VSS). Originally [bohops](https://twitter.com/bohops) wrote about this binary in his [blog](https://bohops.com/2018/03/26/diskshadow-the-return-of-vss-evasion-persistence-and-active-directory-database-extraction/). This binary has two modes **interactive** and **script** and therefore a script file can be used that will contain all the necessary commands to automate the process of NTDS.DIT extraction. The script file can contain the following lines in order to create a new volume shadow copy, mount a new drive, execute the copy command and delete the volume shadow copy.

1
2
3
4
5
6
7
[object Object]
[object Object]
[object Object]
[object Object]
[object Object][object Object]  [object Object]
[object Object]
[object Object]

It should be noted that the **DiskShadow** binary needs to executed from the **C:\Windows\System32** path. If it is called from another path the script will not executed correctly.

1
[object Object]
![diskshadow.png](../_resources/346c5467321e2962bc43b3985b79bdbc.png)
DiskShadow

Running the following command directly from the interpreter will list all the available volume shadow copies of the system.

1
2
[object Object]
[object Object]
![diskshadow-retrieve-shadow-copies.png](../_resources/441b2054f6986d386b5c9630c60ecd6a.png)
diskshadow – Retrieve Shadow Copies

The SYSTEM registry hive should be copied as well since it contains the key to decrypt the contents of the NTDS file.

1
[object Object]
![diskshadow-copy-system-from-registry.png](../_resources/8ff60ebe49d6f0420ea2b60ebcfa7605.png)
diskshadow – Copy system from Registry

## WMI

[Sean Metcalf](https://twitter.com/PyroTek3) demonstrated in his [blog](https://adsecurity.org/?p=2398) that it is possible to remotely extract the NTDS.DIT and SYSTEM files via WMI. This technique is using the **vssadmin** binary to create the volume shadow copy.

1
[object Object][object Object][object Object][object Object]
![wmi-create-volume-shadow-copy.png](../_resources/e717fee181f5912164bd0eceed20b2bb.png)
WMI – Create Volume Shadow Copy

Then it executes the copy command remotely in order to extract the NTDS.DIT file from the volume shadow copy into another directory on the target system.

1
[object Object][object Object][object Object][object Object]
![wmi-copy-ntds-file.png](../_resources/385e54456eeea14007609144a0e822a7.png)
WMI – Copy NTDS File
The same applies and for the SYSTEM file.
1
[object Object][object Object][object Object][object Object]
![wmi-copy-system-file.png](../_resources/b916e0ace7f7c1f75e908a2f30d336f0.png)
WMI – Copy System File

The extracted files can then transferred from the domain controller into another Windows system for dumping the domain password hashes.

1
2
[object Object][object Object][object Object][object Object][object Object]
[object Object][object Object][object Object][object Object][object Object]
![wmi-transfer-files-via-copy.png](../_resources/be6859e6f597401a830988fba1840295.png)
Transfer Files via Copy

Instead of credentials if a Golden ticket has been generated it can be used for authentication with the domain controller via Kerberos.

## vssadmin

The volume shadow copy is a Windows command line utility which enables administrators to take backups of computers, volumes and files even if they are in use by the operating system. Volume Shadow Copy is running as a service and requires the filesystem to be formatted as NTFS which all the modern operating systems are by default. From a Windows command prompt executing the following will create a snapshot of the **C:** drive in order files that are not normally accessible by the user to be copied into another location (local folder, network folder or removable media).

1
[object Object]
![vssadmin-create-volume-shadow-copy.png](../_resources/a00c5670a944ad32f653b6dd48dab4eb.png)
vssadmin – Create Volume Shadow Copy

Since all the files in the C: drive have been copied into another location (HarddiskVolumeShadowCopy1) they are not directly used by the operating system and therefore can be accessed and copied into another location. The command **copy** and will copy the **NTDS.DIT** and **SYSTEM** files to a new created folder on the local drive named ShadowCopy.

1
2
[object Object][object Object][object Object]
[object Object][object Object][object Object][object Object][object Object]
![copy-files-from-volume-shadow-copy.png](../_resources/32e808abdeda2424322857ca5c463b2e.png)
Copy Files from Volume Shadow Copy

These files needs to be copied from the domain controller into another host for further processing.

![shadowcopy-files.png](../_resources/c7587139a1c77005ea4c9749d1a14085.png)
ShadowCopy – Files

##  vssown

Similar to the **vssadmin** utility [Tim Tomes](https://twitter.com/lanmaster53) developed [vssown](https://github.com/lanmaster53/ptscripts/blob/master/windows/vssown.vbs) which is a visual basic script that can create and delete volume shadow copies, run arbitrary executables from an unmounted shadow copy and initiate and stop the volume shadow copy service.

1
2
3
4
[object Object]
[object Object]
[object Object]
[object Object]
![vssown-volume-shadow-copy.png](../_resources/ec7e65571e2afd67b71287842daff5da.png)
vssown – Volume Shadow Copy
The required files can be copied with the command **copy**.
1
2
3
[object Object][object Object][object Object]
[object Object][object Object][object Object][object Object][object Object]
[object Object][object Object][object Object][object Object][object Object]

![vssown-copy-ntds-system-and-sam-files.png](../_resources/7a302651a552ff5a7dd3b8b8d8a570f6.png)

vssown – Copy NTDS, SYSTEM and SAM Files

## Metasploit

Metasploit framework has a module which authenticates directly with the domain controller via the server message block (SMB) service, creates a volume shadow copy of the system drive and download copies of the NTDS.DIT and SYSTEM hive into the Metasploit directories. These files can be used with other tools like **impacket** that can perform extraction of active directory password hashes.

1
[object Object]
![metasploit-ntds-module.png](../_resources/ac73fae3e1dcfc21a66dd9a72fa617a8.png)
Metasploit – NTDS Module

There is also a post exploitation module which can be linked into an existing Meterpreter session in order to retrieve domain hashes via the ntdsutil method.

1
[object Object]
![metasploit-domain-hashdump.png](../_resources/0d309808075deb4fbca8328d4637eadb.png)

Alternatively if there is an existing Meterpreter session to the domain controller the command **hashdump** can be used. However this method is not considered safe as it might crash the domain controller.

1
[object Object]
![metasploit-hashdump-on-dc.png](../_resources/8ed56b056ce356ee8c3558624a87305b.png)
Metasploit – Hashdump on DC

## fgdump

The [fgdump](http://www.foofus.net/fizzgig/fgdump/fgdump-2.1.0-exeonly.zip) is an old executable file which can extract LanMan and NTLM password hashes. It can be executed locally or remotely if local administrator credentials have been acquired. During execution fgdump will attempt to disable the antivirus that might run on the system and if it is successful will write all the data in two files. If there is an antivirus or an endpoint solution fgdump should not be used as a method of dumping password hashes to avoid detection since it is being flagged by most antivirus companies including Microsoft’s Windows Defender.

1
[object Object]
![fgdump-domain-controller.png](../_resources/df2d8f805ed76f6c7111b6fcc4c43ed2.png)
fgdump – Domain Controller

The password hashes can be retrieved by examining the contents of the .pwdump file.

1
[object Object][object Object][object Object][object Object][object Object]
![fgdump-pwdump-file.png](../_resources/aa5dd6f0d9a242fa3f1e27c9803fcd40.png)
fgdump – pwdump File

## NTDS Extraction

[Impacket](https://github.com/CoreSecurity/impacket) is a collection of python scripts that can be used to perform various tasks including extraction of contents of the NTDS file. The **impacket-secretsdump** module requires the SYSTEM and the NTDS database file.

1
[object Object]
![impacket-extract-ntds-contents.png](../_resources/9a91e1031881572ae4650b8b5ded2a5c.png)
impacket – Extract NTDS Contents

Furthermore **impacket** can dump the domain password hashes remotely from the NTDS.DIT file by using the computer account and its hash for authentication.

1

[object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object][object Object]  [object Object][object Object][object Object][object Object]

![impacket-extract-ntds-contents-remotely.png](../_resources/7ddf881720450c579849338dadc478fa.png)

impacket – Extract NTDS Contents Remotely

As an alternative solution to impacket, [NTDSDumpEx](https://github.com/zcgonvh/NTDSDumpEx) binary can extract the domain password hashes from a Windows host.

1
[object Object]
![ntdsdumpex.png](../_resources/fe3e99df6b2929fdf8324dbb5199b817.png)
NTDSDumpEx

There is also a shell script [adXtract](https://github.com/LordNem/adXtract) that can export the username and password hashes into a format that can be used by common password crackers such as John the Ripper and Hashcat.

1
[object Object]
![adxtract.png](../_resources/d07c90d1b465f5db9b26ceba4ee44e5c.png)
adXtract

The script will write all the information into various files under the project name and when the decryption of the database file NTDS is finished will export the list of users and password hashes into the console. The script will provide extensive information regarding the domain users as it can be demonstrated below.

![adxtract-list-of-users.png](../_resources/8f38c1ea5a58d068d28b7b1263cee97d.png)
adXtract – List of Users
The password hashes will be presented into the following format.
![adxtract-password-hashes.png](../_resources/90c5a8f799580ed681a57929f7bb147c.png)