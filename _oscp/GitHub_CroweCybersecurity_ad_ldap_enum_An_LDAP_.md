GitHub - CroweCybersecurity/ad-ldap-enum: An LDAP based Active Directory user and group enumeration tool

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='43'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='664' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#ad-ldap-enum)ad-ldap-enum

An LDAP based Active Directory user and group enumeration tool

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='44'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='667' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#about)About

ad-ldap-enum is a Python script that was developed to discover users and their group memberships from Active Directory. In large Active Directory environments, tools such as NBTEnum were not performing fast enough. By executing LDAP queries against a domain controller, ad-ldap-enum is able to target specific Active Directory attributes and build out group membership quickly.

ad-ldap-enum outputs three tab delimited files 'Domain Group Membership.tsv', 'Extended Domain User Information.tsv', and 'Extended Domain Computer Information.tsv'. The first file contains users, computers, groups, and their memberships. The second file contains users and extra information about the users from Active Directory (e.g. a user's home folder or email address). The third file contains devices in the Domain Computers group and extra information about them from Active Directory (e.g. operating system type and service pack version).

ad-ldap-enum supports both authenticated and unauthenticated LDAP connections. Additionally, ad-ldap-enum can process nested groups and display a user's actual group membership.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='45'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='672' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#requirements)Requirements

The package [python-ldap](http://www.python-ldap.org/index.html) is required for the script to execute. This can be installed with the following command:

	pip install python-ldap

Additionally, this tools has been built and tested against Python v2.7.13 and python-ldap v2.4.20

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='46'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='676' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#usage)Usage

	ad-ldap-enum.py [-h] -l LDAP_SERVER -d DOMAIN [-a ALT_DOMAIN] [-e] [-n] [-u USERNAME] [-p PASSWORD] [-v]

	Active Directory LDAP Enumerator

	optional arguments:
	  -h, --help                                        show this help message and exit
	  -v, --verbose                                     Display debugging information.
	  -o FILENAME_PREPEND, --prepend FILENAME_PREPEND   Prepend a string to all output file names.

	Server Parameters:
	  -l LDAP_SERVER, --server LDAP_SERVER              IP address of the LDAP server.
	  -d DOMAIN, --domain DOMAIN                        Authentication account's FQDN. If an alternative domain is not specified this will be also used as the Base DN for searching LDAP.
	  -a ALT_DOMAIN, --alt-domain ALT_DOMAIN            Alternative FQDN to use as the Base DN for searching LDAP.
	  -e, --nested                                      Expand nested groups.

	Authentication Parameters:
	  -n, --null                                        Use a null binding to authenticate to LDAP.
	  -s, --secure                                      Connect to LDAP over SSL
	  -u USERNAME, --username USERNAME                  Authentication account's username.
	  -p PASSWORD, --password PASSWORD                  Authentication account's password.

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='47'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='678' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#example)Example

`python ad-ldap-enum.py -d contoso.com -l 10.0.0.1 -u Administrator -p P@ssw0rd`

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='48'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='681' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/CroweCybersecurity/ad-ldap-enum#assorted-links)Assorted Links

- [Membership Ranges in Active Directory](https://msdn.microsoft.com/en-us/library/Aa367017)
- [Active Directory Paging](https://technet.microsoft.com/en-us/library/Cc755809(v=WS.10).aspx#w2k3tr_adsrh_how_lhjt)