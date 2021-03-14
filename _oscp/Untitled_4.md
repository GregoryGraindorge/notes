

    

Check out the new and improved `windapsearch` - rewritten in Go with some new features (including JSON support)!

https://github.com/ropnop/go-windapsearch

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='45'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='732' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#windapsearch)windapsearch

`windapsearch` is a Python script to help enumerate users, groups and computers from a Windows domain through LDAP queries. By default, Windows Domain Controllers support basic LDAP operations through port 389/tcp. With any valid domain account (regardless of privileges), it is possible to perform LDAP queries against a domain controller for any AD related information.

You can always use a tool like `ldapsearch` to perform custom LDAP queries against a Domain Controller. I found myself running different LDAP commands over and over again, and it was difficult to memorize all the custom LDAP queries. So this tool was born to help automate some of the most useful LDAP queries a pentester would want to perform in an AD environment.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='46'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='736' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#requirements)Requirements

`windapsearch` requires the `python-ldap` module. You should be able to get up and running with two commands:

	$ git clone https://github.com/ropnop/windapsearch.git
	$ pip install python-ldap #or apt-get install python-ldap
	$ ./windapsearch.py

The latest version is designed to be used with Python 3, but if you are stuck with Python 2, you can use the `windapsearch_py2.py` script.

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='47'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='740' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#usage)Usage

	$ python windapsearch.py -h
	usage: windapsearch.py [-h] [-d DOMAIN] [--dc-ip DC_IP] [-u USER]
	                       [-p PASSWORD] [--functionality] [-G] [-U] [-C]
	                       [-m GROUP_NAME] [--da] [--admin-objects] [--user-spns]
	                       [--unconstrained-users] [--unconstrained-computers]
	                       [--gpos] [-s SEARCH_TERM] [-l DN]
	                       [--custom CUSTOM_FILTER] [-r] [--attrs ATTRS] [--full]
	                       [-o output_dir]

	Script to perform Windows domain enumeration through LDAP queries to a Domain
	Controller

	optional arguments:
	  -h, --help            show this help message and exit

	Domain Options:
	  -d DOMAIN, --domain DOMAIN
	                        The FQDN of the domain (e.g. 'lab.example.com'). Only
	                        needed if DC-IP not provided
	  --dc-ip DC_IP         The IP address of a domain controller

	Bind Options:
	  Specify bind account. If not specified, anonymous bind will be attempted

	  -u USER, --user USER  The full username with domain to bind with (e.g.
	                        'ropnop@lab.example.com' or 'LABopnop'
	  -p PASSWORD, --password PASSWORD
	                        Password to use. If not specified, will be prompted
	                        for

	Enumeration Options:
	  Data to enumerate from LDAP

	  --functionality       Enumerate Domain Functionality level. Possible through
	                        anonymous bind
	  -G, --groups          Enumerate all AD Groups
	  -U, --users           Enumerate all AD Users
	  -PU, --privileged-users
	                        Enumerate All privileged AD Users. Performs recursive
	                        lookups for nested members.
	  -C, --computers       Enumerate all AD Computers
	  -m GROUP_NAME, --members GROUP_NAME
	                        Enumerate all members of a group
	  --da                  Shortcut for enumerate all members of group 'Domain
	                        Admins'. Performs recursive lookups for nested
	                        members.
	  --admin-objects       Enumerate all objects with protected ACLs (i.e.
	                        admins)
	  --user-spns           Enumerate all users objects with Service Principal
	                        Names (for kerberoasting)
	  --unconstrained-users
	                        Enumerate all user objects with unconstrained
	                        delegation
	  --unconstrained-computers
	                        Enumerate all computer objects with unconstrained
	                        delegation
	  --gpos                Enumerate Group Policy Objects
	  -s SEARCH_TERM, --search SEARCH_TERM
	                        Fuzzy search for all matching LDAP entries
	  -l DN, --lookup DN    Search through LDAP and lookup entry. Works with fuzzy
	                        search. Defaults to printing all attributes, but
	                        honors '--attrs'
	  --custom CUSTOM_FILTER
	                        Perform a search with a custom object filter. Must be
	                        valid LDAP filter syntax

	Output Options:
	  Display and output options for results

	  -r, --resolve         Resolve IP addresses for enumerated computer names.
	                        Will make DNS queries against system NS
	  --attrs ATTRS         Comma separated custom atrribute names to search for
	                        (e.g. 'badPwdCount,lastLogon')
	  --full                Dump all atrributes from LDAP.
	  -o output_dir, --output output_dir
	                        Save results to TSV files in <OUTPUT_DIR>

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='48'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='742' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#specifying-domain-and-account)Specifying Domain and Account

To begin you need to specify a Domain Controller to connect to with `--dc-ip`, or a domain with `-d`. If no Domain Controller IP address is specified, the script will attempt to do a DNS `host` lookup on the domain and take the top result.

A valid domain username and password are required for most lookups. If none are specififed the script will attempt an anonymous bind and enumerate the default namingContext, but most additional queries will fail. The username needs to include the full domain, e.g. `ropnop@lap.example.com` or `EXAMPLEopnop`

The password can be specified on the command line with `-p` or if left out it will be prompted for.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='49'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='747' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#enumerate-users)Enumerate Users

The `-U` option performs an LDAP search for all entries where `objectCategory=user`. By default, it will only display the commonName and the userPrincipalName. The `--attrs` option can be used to specify custom or additional attributes to display, or the `--full` option will display everythin for all users.

WARNING: in a large domain this can get very big, very fast
Example:

	$  ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -U
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind

	[+] Enumerating all AD users
	[+]     Found 2754 users:

	cn: Administrator

	cn: Guest

	cn: krbtgt

	cn: Andy Green
	userPrincipalName: agreen@lab.ropnop.com

	<snipped...>

To save the results to a tab-separated file, use the `-o` option and specify a directory.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='50'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='753' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#enumerate-groups-and-group-memberships)Enumerate Groups and Group Memberships

Use the `-G` option to enumerate all entries where `objectCategory=group`. This will output the DN and CN of all groups.

To query group membership, use the `-m` option with either the DN or CN of the group you wish to query. The tool supports fuzzy search matching so even a partial CN will work. If it matches more than one group, the tool will specify which group to query.

Example:

	$ ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -m IT
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind
	[+] Attempting to enumerate full DN for group: IT
	[+] Found 2 results:

	0: CN=IT Admins,OU=Groups,OU=Lab,DC=lab,DC=ropnop,DC=com
	1: CN=Ismael Titus,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com

	Which DN do you want to use? : 0
	[+]      Found 5 members:

	CN=James Doyle,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Edward Sotelo,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Cheryl Perry,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Anthony Gordon,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Desktop Support,OU=Groups,OU=Lab,DC=lab,DC=ropnop,DC=com

	[*] Bye!

#### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='51'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='758' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#domain-admins)Domain Admins

You can enumerate Domain Admins through two methods. One is to use `-m` with "Domain Admins". This will query LDAP for the "Domain Admins" entry and display all the members.

The more thorough way is to do a lookup of all users and determine if they or a group they belong to are part of "Domain Admins". This has the added benefit of discovering users who have inherited DA rights through nested group memberships. It's much slower, however. See this link for more details on the technique used: https://labs.mwrinfosecurity.com/blog/active-directory-users-in-nested-groups-reconnaissance/To do a recursive lookup for Domain Admins, you can use the "--da" option.

Example:

	root@kali:~/windapsearch# ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 --da
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind
	[+] Attempting to enumerate all Domain Admins
	[+] Using DN: CN=Domain Admins,CN=Users.CN=Domain Admins,CN=Users,DC=lab,DC=ropnop,DC=com
	[+]     Found 12 Domain Admins:

	cn: Administrator

	cn: Andy Green
	userPrincipalName: agreen@lab.ropnop.com

	cn: Natasha Strong
	userPrincipalName: nstrong@lab.ropnop.com

	cn: Linda Alton
	userPrincipalName: lalton@lab.ropnop.com

	<snipped...>

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='52'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='763' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#enumerating-computers)Enumerating Computers

LDAP queries can be used to enumerate domain joined computers. This is very useful when trying to build a list of targets without running a portscan or ping sweep.

Use the `-C` option to list all matching entries where `objectClass=Computer`. By default, the attributes displayed are 'cn', 'dNSHostName', 'operatingSystem', 'operatingSystemVersion', and 'operatingSystemServicePack'

If you specify the `-r` or `--resolve` option, the tool will perform a DNS lookup on every enumerated dNSHostName found and output the computer information, including IP address in CSV format. This can then be fed to other tools like nmap or CrackMapExec.

Example:

	$ ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -C -r
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind

	[+] Enumerating all AD computers
	[+]     Found 4 computers:

	cn, IP, dNSHostName, operatingSystem, operatingSystemVersion, operatingSystemServicePack
	WS03WIN10,172.16.13.53,ws03win10.lab.ropnop.com,Windows 10 Enterprise,10.0 (10240),
	WS02WIN7,172.16.13.50,WS02WIN7.lab.ropnop.com,Windows 7 Enterprise,6.1 (7601),Service Pack 1
	WS01WIN7,172.16.13.52,WS01WIN7.lab.ropnop.com,Windows 7 Enterprise,6.1 (7601),Service Pack 1
	DC01,172.16.13.10,dc01.lab.ropnop.com,Windows Server 2012 R2 Standard,6.3 (9600),

	[*] Bye!

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='53'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='769' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#custom-searching)Custom Searching

The tool allows for custom, fuzzy matching. You can perform a search and see results (DNs) with the `-s` option:

	$ ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -s albert
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind
	[+] Doing fuzzy search for: "albert"
	[+]     Found 6 results:

	CN=Kathi Albert,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Albert Woodell,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Albert Lyons,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Alberta Henshaw,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Alberta Taylor,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	CN=Alberto Mitchell,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com

	[*] Bye!

To query the DN and display the attributes, use the lookup option, `-l`. You can provide this with a full DN, or a search term. If the search matches more than one DN, the tool will prompt you for which to use:

	$ ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -l albert --attrs telephoneNumber
	[+] No DC IP provided. Will try to discover via DNS lookup.
	[+] Using Domain Controller at: 172.16.13.10
	[+] Getting defaultNamingContext from Root DSE
	[+]     Found: DC=lab,DC=ropnop,DC=com
	[+] Attempting bind
	[+]     ...success! Binded as:
	[+]      u:ROPNOP\ldapbind
	[+] Searching for matching DNs for term: "albert"
	[+] Found 6 results:

	0: CN=Kathi Albert,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	1: CN=Albert Woodell,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	2: CN=Albert Lyons,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	3: CN=Alberta Henshaw,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	4: CN=Alberta Taylor,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com
	5: CN=Alberto Mitchell,OU=US,OU=Users,OU=Lab,DC=lab,DC=ropnop,DC=com

	Which DN do you want to use? : 2
	telephoneNumber: 716-825-5021

	[*] Bye!

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='54'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='773' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ropnop/windapsearch#credits)Credits

Heavily influenced by this post and research done by MWR Labs:https://labs.mwrinfosecurity.com/blog/active-directory-users-in-nested-groups-reconnaissance/

and their tool to perform offline querying of LDAP:https://labs.mwrinfosecurity.com/blog/offline-querying-of-active-directory/

also, after I wrote the majority of this tool I discovered a very similar project here:https://github.com/CroweCybersecurity/ad-ldap-enumDefinitely check that tool out too! I sniped some of the code to get paging working with my tool anyway :)

For some more LDAP searching goodness, check out this Microsoft article on other AD queries you can perform (hint: use the `--custom` flag)https://social.technet.microsoft.com/wiki/contents/articles/5392.active-directory-ldap-syntax-filters.aspx