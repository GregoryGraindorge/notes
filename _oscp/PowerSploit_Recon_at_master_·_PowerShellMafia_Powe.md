PowerSploit/Recon at master · PowerShellMafia/PowerSploit · GitHub

To install this module, drop the entire Recon folder into one of your module directories. The default PowerShell module paths are listed in the $Env:PSModulePath environment variable.

The default per-user module path is: "$Env:HomeDrive$Env:HOMEPATH\Documents\WindowsPowerShell\Modules" The default computer-level module path is: "$Env:windir\System32\WindowsPowerShell\v1.0\Modules"

To use the module, type `Import-Module Recon`
To see the commands imported, type `Get-Command -Module Recon`
For help on each individual command, Get-Help is your friend.

Note: The tools contained within this module were all designed such that they can be run individually. Including them in a module simply lends itself to increased portability.

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='37'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='635' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#powerview)PowerView

PowerView is a PowerShell tool to gain network situational awareness on Windows domains. It contains a set of pure-PowerShell replacements for various windows "net *" commands, which utilize PowerShell AD hooks and underlying Win32 API functions to perform useful Windows domain functionality.

It also implements various useful metafunctions, including some custom-written user-hunting functions which will identify where on the network specific users are logged into. It can also check which machines on the domain the current user has local administrator access on. Several functions for the enumeration and abuse of domain trusts also exist. See function descriptions for appropriate usage and available options. For detailed output of underlying functionality, pass the -Verbose or -Debug flags.

For functions that enumerate multiple machines, pass the -Verbose flag to get a progress status as each host is enumerated. Most of the "meta" functions accept an array of hosts from the pipeline.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='38'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='640' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#misc-functions)Misc Functions:

	Export-PowerViewCSV             -   thread-safe CSV append
	Set-MacAttribute                -   Sets MAC attributes for a file based on another file or input (from Powersploit)
	Copy-ClonedFile                 -   copies a local file to a remote location, matching MAC properties
	Get-IPAddress                   -   resolves a hostname to an IP
	Test-Server                     -   tests connectivity to a specified server
	Convert-NameToSid               -   converts a given user/group name to a security identifier (SID)
	Convert-SidToName               -   converts a security identifier (SID) to a group/user name
	Convert-NT4toCanonical          -   converts a user/group NT4 name (i.e. dev/john) to canonical format
	Get-Proxy                       -   enumerates local proxy settings
	Get-PathAcl                     -   get the ACLs for a local/remote file path with optional group recursion
	Get-UserProperty                -   returns all properties specified for users, or a set of user:prop names
	Get-ComputerProperty            -   returns all properties specified for computers, or a set of computer:prop names
	Find-InterestingFile            -   search a local or remote path for files with specific terms in the name
	Invoke-CheckLocalAdminAccess    -   check if the current user context has local administrator access to a specified host
	Get-DomainSearcher              -   builds a proper ADSI searcher object for a given domain
	Get-ObjectAcl                   -   returns the ACLs associated with a specific active directory object
	Add-ObjectAcl                   -   adds an ACL to a specified active directory object
	Get-LastLoggedOn                -   return the last logged on user for a target host
	Get-CachedRDPConnection 		-	queries all saved RDP connection entries on a target host
	Invoke-ACLScanner               -   enumerate -1000+ modifable ACLs on a specified domain
	Get-GUIDMap                     -   returns a hash table of current GUIDs -> display names
	Get-DomainSID                   -   return the SID for the specified domain
	Invoke-ThreadedFunction         -   helper that wraps threaded invocation for other functions

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='39'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='642' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#net--functions)net * Functions:

	Get-NetDomain                   -   gets the name of the current user's domain
	Get-NetForest                   -   gets the forest associated with the current user's domain
	Get-NetForestDomain             -   gets all domains for the current forest
	Get-NetDomainController         -   gets the domain controllers for the current computer's domain
	Get-NetUser                     -   returns all user objects, or the user specified (wildcard specifiable)
	Add-NetUser                     -   adds a local or domain user
	Get-NetComputer                 -   gets a list of all current servers in the domain
	Get-NetPrinter                  -   gets an array of all current computers objects in a domain
	Get-NetOU                       -   gets data for domain organization units
	Get-NetSite                     -   gets current sites in a domain
	Get-NetSubnet                   -   gets registered subnets for a domain
	Get-NetGroup                    -   gets a list of all current groups in a domain
	Get-NetGroupMember              -   gets a list of all current users in a specified domain group
	Get-NetLocalGroup               -   gets the members of a localgroup on a remote host or hosts
	Add-NetGroupUser                -   adds a local or domain user to a local or domain group
	Get-NetFileServer               -   get a list of file servers used by current domain users
	Get-DFSshare                    -   gets a list of all distribute file system shares on a domain
	Get-NetShare                    -   gets share information for a specified server
	Get-NetLoggedon                 -   gets users actively logged onto a specified server
	Get-NetSession                  -   gets active sessions on a specified server
	Get-NetRDPSession               -   gets active RDP sessions for a specified server (like qwinsta)
	Get-NetProcess                  -   gets the remote processes and owners on a remote server
	Get-UserEvent                   -   returns logon or TGT events from the event log for a specified host
	Get-ADObject                    -   takes a domain SID and returns the user, group, or computer
	                                    object associated with it
	Set-ADObject                    -   takes a SID, name, or SamAccountName to query for a specified
	                                    domain object, and then sets a specified 'PropertyName' to a
	                                    specified 'PropertyValue'

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='40'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='644' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#gpo-functions)GPO functions

	Get-GptTmpl                     -   parses a GptTmpl.inf to a custom object
	Get-NetGPO                      -   gets all current GPOs for a given domain
	Get-NetGPOGroup                 -   gets all GPOs in a domain that set "Restricted Groups"
	                                    on on target machines
	Find-GPOLocation                -   takes a user/group and makes machines they have effective
	                                    rights over through GPO enumeration and correlation
	Find-GPOComputerAdmin           -   takes a computer and determines who has admin rights over it
	                                    through GPO enumeration
	Get-DomainPolicy                -   returns the default domain or DC policy

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='41'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='646' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#user-hunting-functions)User-Hunting Functions:

	Invoke-UserHunter               -   finds machines on the local domain where specified users are logged into, and can optionally check if the current user has local admin access to found machines
	Invoke-StealthUserHunter        -   finds all file servers utilizes in user HomeDirectories, and checks the sessions one each file server, hunting for particular users
	Invoke-ProcessHunter            -   hunts for processes with a specific name or owned by a specific user on domain machines
	Invoke-UserEventHunter          -   hunts for user logon events in domain controller event logs

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='42'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='648' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#domain-trust-functions)Domain Trust Functions:

	Get-NetDomainTrust              -   gets all trusts for the current user's domain
	Get-NetForestTrust              -   gets all trusts for the forest associated with the current user's domain
	Find-ForeignUser                -   enumerates users who are in groups outside of their principal domain
	Find-ForeignGroup               -   enumerates all the members of a domain's groups and finds users that are outside of the queried domain
	Invoke-MapDomainTrust           -   try to build a relational mapping of all domain trusts

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='43'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='650' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/PowerShellMafia/PowerSploit/tree/master/Recon#metafunctions)MetaFunctions:

	Invoke-ShareFinder              -   finds (non-standard) shares on hosts in the local domain
	Invoke-FileFinder               -   finds potentially sensitive files on hosts in the local domain
	Find-LocalAdminAccess           -   finds machines on the domain that the current user has local admin access to
	Find-ManagedSecurityGroups      -   searches for active directory security groups which are managed and identify users who have write access to

	                                - those groups (i.e. the ability to add or remove members)

	Find-UserField                  -   searches a user field for a particular term
	Find-ComputerField              -   searches a computer field for a particular term
	Get-ExploitableSystem           -   finds systems likely vulnerable to common exploits
	Invoke-EnumerateLocalAdmin      -   enumerates members of the local Administrators groups across all machines in the domain