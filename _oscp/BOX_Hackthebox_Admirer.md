BOX - Hackthebox - Admirer

# Admirer Walkthrough | HackTheBox

[![2*ktWSj3qeVaD8L8WnnsHrOQ.jpeg](../_resources/df7208617bcdfaacfddadf4c6f9f4ad5.jpg)](https://medium.com/@xy83rx?source=post_page-----3b4dc128f62c----------------------)

[David Kingsly](https://medium.com/@xy83rx?source=post_page-----3b4dc128f62c----------------------)

[Jun 12](https://medium.com/@xy83rx/admirer-walkthrough-3b4dc128f62c?source=post_page-----3b4dc128f62c----------------------) · 7 min read

[![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='25' height='25' viewBox='0 0 25 25' data-evernote-id='191' class='js-evernote-checked'%3e%3cpath d='M19 6a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v14.66h.01c.01.1.05.2.12.28a.5.5 0 0 0 .7.03l5.67-4.12 5.66 4.13a.5.5 0 0 0 .71-.03.5.5 0 0 0 .12-.29H19V6zm-6.84 9.97L7 19.64V6a1 1 0 0 1 1-1h9a1 1 0 0 1 1 1v13.64l-5.16-3.67a.49.49 0 0 0-.68 0z' fill-rule='evenodd' data-evernote-id='192' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://medium.com/m/signin?operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40xy83rx%2Fadmirer-walkthrough-3b4dc128f62c&source=post_actions_header--------------------------bookmark_header-)

![1*15HvvOxTb_4UljV1VXwjEg.png](../_resources/516c7256837a17ab81d4f1237212eb65.png)
![1*15HvvOxTb_4UljV1VXwjEg.png](../_resources/300df1dc4246ef970fb8a77830b13f9e.png)

This Is David Kingsly And This Is My Walkthrough For The Admirer Machine From HackTheBox. It’s A Linux Machine With Easy Difficulty Level.

Like All The Machines, I Used **nmap** To Scan For Open Ports And Services.
**nmap -sC -sV 10.10.10.187**
![1*Ws3cb3XJwooq6LptqYmBOQ.png](../_resources/99d816d2b5c523258334608a901a1de8.png)
![1*Ws3cb3XJwooq6LptqYmBOQ.png](../_resources/e61282d5efad1a8da75eea72f2ac040d.png)

I Found **FTP Server** Running On Port **21**, **SSH** Running On Port **22** And **Apache HTTP Server** Running On Port **80**.

I Checked The Web On Port 80. The Index Page Of The Site Looks Like A Gallery.
![1*CZwlwG6j1DjMx4hIF-sCjQ.png](../_resources/82819fac207d4e2a6ed66b8be09ea704.png)
![1*CZwlwG6j1DjMx4hIF-sCjQ.png](../_resources/8ad2f279a3a545430f7540d1e8206644.png)

I Also Checked The Source Of The Index Page For Hints. I Found That It Was A Static HTML Template From HTML5 UP.

![1*UuJ5GEFwYPbIkUK3YFnesA.png](../_resources/c390dc5764bb84b758530b2a687736a8.png)
![1*UuJ5GEFwYPbIkUK3YFnesA.png](../_resources/ab3e04b20c8ae21b98f7380689862666.png)

The namp Scan Showed There’s A **robots.txt** File On The Web Server, This File Is Usually For The Crawler Bots. I Checked The **robots.txt** File And Found A Directory Called **admin-dir** And It’s Not Allowed To Read By Anyone Even The Crawler Bots.

![1*Jx5eeQus6aVALF0dpaxlAw.png](../_resources/df6905a4a985e5860d83155718a8e279.png)
![1*Jx5eeQus6aVALF0dpaxlAw.png](../_resources/090b394cb8f6e5c6cbd202f93a30e1d2.png)

After That I Went To The **admin-dir** Directory, It Returned A Error 403 Forbidden That I Don’t Have Enough Permission To Access That Directory.

![1*L1fKUEogRTiPMBCwWjFTzg.png](../_resources/93438bcda0f8d773e08f6b324502133a.png)
![1*L1fKUEogRTiPMBCwWjFTzg.png](../_resources/0fe4f515b47bf2df2fa9551f3969cd38.png)
Then I Used **gobuster** To Brute-Force The Directories In That Server.

**gobuster dir -w /usr/share/seclists/Discovery/Web-Content/big.txt -u http://10.10.10.187/admin-dir/**

![1*vWjuJG1esZjt9sZEpFEjpg.png](../_resources/012953c2e33649b09b3f1588e5abe345.png)
![1*vWjuJG1esZjt9sZEpFEjpg.png](../_resources/78c5f41c3723c1da1630a9636ce5af9c.png)

It Doesn’t Find Any Directories. So, I Used **wfuzz** To Scan For** .txt** Files Inside The **admin-dir **Directory.

**> wfuzz -u http://10.10.10.187/admin-dir/FUZZ.txt -w /usr/share/seclists/Discovery/Web-Content/big.txt --sc 200**

![1*mL1d_LFLbDyAj473zOms7w.png](../_resources/a6941ec2448e1e3f8ed751ecfd316243.png)
![1*mL1d_LFLbDyAj473zOms7w.png](../_resources/fb9a7692ecb9b33caa8c6ffa2fd4405a.png)

There I Found Two Files, **contacts.txt** And **credentials.txt**. The **contacts.txt** File Contains Only The Email Addresses Of The Members And The **credentials.txt **File Contains The Webmail, FTP And WordPress Credentials.

![1*md0k0YWtm4pBsV_fuktvkg.png](../_resources/5efbe9d0aa6c512c6434a5c586173e68.png)
![1*md0k0YWtm4pBsV_fuktvkg.png](../_resources/21c0c9c96353eae68755adb03556dd9c.png)
With The Help Of The Credentials I Logged Into FTP Server As **ftpuser**.
![1*9o6YJmlBihJvVqqfKUT3pA.png](../_resources/8a2f4af8ae04bf24c98d0a094c762edf.png)
![1*9o6YJmlBihJvVqqfKUT3pA.png](../_resources/dd230132ad96065ab02c6901c6c123dd.png)

In The FTP Server I Found Two Files, **dump.sql** And **html.tar.gz**. Then I’ve Downloaded The Both Files To My Machine.

![1*9rwdAEjgi-RCmhseCJ3sDA.png](../_resources/13b7d8100253bf79e7d0b7dd4486f3c1.png)
![1*9rwdAEjgi-RCmhseCJ3sDA.png](../_resources/e4c85b6c402207822b3ceee3bc2af63b.png)

The SQL Doesn’t Have Anything Interesting, It Only Has The Information About The MariaDB And The Pictures On The Homepage. After That, I Have Extracted The **html.tar.gz** Archive.

It Contains Files Same As The In The Web Server And Additionally It Contains **utility-scripts** And **w4ld0s_s3cr3t_d1r** Directories. The **w4ld0s_s3cr3t_d1r **Directory Contains The Files Which Were Found In The **admin-dir** Directory.

![1*MzAI6Bl7aeOefdfb93rPqg.png](../_resources/693a477ce1de6076a12b6533d31c179c.png)
![1*MzAI6Bl7aeOefdfb93rPqg.png](../_resources/74387e25fceebb4e8706bf8e16a96e94.png)

In The utility-scripts Directory I Found 4 PHP Files. **admin_tasks.php**, **db_admin.php**, **info.php **And **phptest.php**.

![1*1RBMQ698EnHDCUW9q_hFGw.png](../_resources/500b5c19cae45a2634978e307d83dd57.png)
![1*1RBMQ698EnHDCUW9q_hFGw.png](../_resources/82375909557106eb0d90adb5fafaa01f.png)

The** info.php **And **phptest.php** Files Were Just There For Testing The PHP Server. The **admin_tasks.php** Contains The Source For Administrative Tasks Page And **db_admin.php** Contains The Credentials For The SQL Database.

![1*ZpsYHVinSiGTP21vpn16BQ.png](../_resources/1bb3d18a61c47421a9d492e7b003df05.png)
![1*ZpsYHVinSiGTP21vpn16BQ.png](../_resources/733c3341c8837279e14cf2fe1380f040.png)

The **utility-scripts** Directory Is Available On The Web Server. I Tried To Access It And I Got Error 403 Forbidden. Then I Tried To Access The File **admin_tasks.php**, It Was Successful.

![1*Ec3doI-xrEf_iOCWtor7bg.png](../_resources/d7d9b9983043d54e69166b5793999787.png)
![1*Ec3doI-xrEf_iOCWtor7bg.png](../_resources/e84dbcfc3420634aa64ced376f1e1d81.png)

It Doesn’t Have Anything To Do With, Then I Tried To Access **db_admin.php** But The File Is Not Available On That Sever. Maybe The Admin Have Used Any Alternative. So, I Have Used **wfuzz **To Scan For **PHP** Files Inside utility-scripts Directory.

**wfuzz -u http://10.10.10.187/utility-scripts/FUZZ.php -w /usr/share/seclists/Discovery/Web-Content/big.txt --sc 200**

![1*hgWH-45sF55ru0shyzfJ_A.png](../_resources/42efee0eda915f1829c528844f633891.png)
![1*hgWH-45sF55ru0shyzfJ_A.png](../_resources/91e009e7b3542bf1767f592f7873235d.png)

There I Found **adminer.php** And I Went To **adminer.php** And It Asked For Credentials. None Of The Credentials Were Worked.

![1*alnlZfawgz_sV7eiRwHQEg.png](../_resources/a9ae5a3d526a2e7ff3c4e152d3fee16d.png)
![1*alnlZfawgz_sV7eiRwHQEg.png](../_resources/a74887361fc68546d9685f5f0d94d955.png)

Then I Searched For **Adminer Version 4.6.2** Vulnerabilities. Then I Found One, What I Have To Do Is To Setup A SQL Server On My Machine And Create A Database, Create A Table With A Single Column And Login To My Database On Adminer And From There I Could Dump Any Local File On That Server.

[ ## Serious Vulnerability Discovered In Adminer Database Administration Tool   #### www.foregenix.com](https://www.foregenix.com/blog/serious-vulnerability-discovered-in-adminer-tool)

So I Have Started The SQL Sever And Created A Database **admirer** And Table **test** With Single Column.

**CREATE DATABASE admirer;
CREATE USER ‘demo’@’%’ IDENTIFIED BY ‘demo_admirer’;
GRANT ALL PRIVILEGES ON * . * TO ‘demo’@’%’;
FLUSH PRIVILEGES;
USE admirer;
create table test(data VARCHAR(255));**
![1*Z86wQjvr5OEbnDzJbqywKA.png](../_resources/5d839019f18f6b0df0368ef839423cb8.png)
![1*Z86wQjvr5OEbnDzJbqywKA.png](../_resources/692eae2fb23eb0e5251ae103c13ee62c.png)

After That I Have Finished My SQL Server Setup And Tried To Login Database Via Adminer.

![1*zYHov2eNExNiq8Oy2zf1qQ.png](../_resources/8966eb8348e8d4fa83450c635d17ad37.png)
![1*zYHov2eNExNiq8Oy2zf1qQ.png](../_resources/178d6b1ba21f29ff936a073e16f17ce7.png)
And It Was Successful.
![1*Tmf9B9ZOubxJnhTWBLJEkQ.png](../_resources/0aa46718a03464ee2fd542fd10b7a8c8.png)
![1*Tmf9B9ZOubxJnhTWBLJEkQ.png](../_resources/ade8e4095c6074c2ef5e9682b93e2571.png)

Then I Went To SQL Command Section. As The **admin_tasks.php** Is Running As **www-data** And It’s An Apache Server. So, I Decided To Dump The **index.php** File.

**load data local infile ‘../index.php’
into table test
fields terminated by “/n”**
![1*POSzWzOTwaRs8vssWhnUMw.png](../_resources/c23ff449ab5ccc06e3cc6d85aa73a2a2.png)
![1*POSzWzOTwaRs8vssWhnUMw.png](../_resources/376880d9443ae18e9540dc39b35dae97.png)

After That I Went To See The Dumped **index.php** File. There I Got A Username And Password.

![1*mqzhdau73ZzrARS3Rn4few.png](../_resources/9243babe18ca52b1fdae3fb81b653298.png)
![1*mqzhdau73ZzrARS3Rn4few.png](../_resources/4e499dc4ac6d01fb6370c4dffa32334d.png)

Then I Tried To Login Via SSH With The User **waldo **And Password **&<h5b~yK3F#{PaPB&dA}{H>**

**ssh waldo@10.10.10.187**
![1*GTOWrHycwKSjPnOn3uaYfA.png](../_resources/f08eb399d774c22e1dd60783c6defede.png)
![1*GTOWrHycwKSjPnOn3uaYfA.png](../_resources/f73b75ccfe6d27ad5fc28f394b96a2e0.png)

Now I Logged In As User **waldo**. Then I Went To **/home/waldo **Directory And Grabbed The **user.txt **(User Flag). Now I Got The User Flag.

![1*vPRZ1tuh69oBbEXUPQV4Gw.png](../_resources/6a361787f3c13c2b62584d50a54de3c5.png)
![1*vPRZ1tuh69oBbEXUPQV4Gw.png](../_resources/f3270873daa36f3b35ba6fd2f1ddac73.png)
**User: 4b61bb97a2c89fc75be6ae9cf1ed411f**

As I Got User, Next I Have To Obtain Root Access To Read The Root Flag. I Used **sudo -l **Command To See Which Commands Can User **waldo **Can Run Without Root Access.

![1*nJbiV2qtpQOqCMHXNifWVQ.png](../_resources/488779ca4472eea3c5030b4c1eba4c3c.png)
![1*nJbiV2qtpQOqCMHXNifWVQ.png](../_resources/442567d03707b233c1464d148fbf2fb9.png)

Looks Like The User **waldo **Can Run Only The **admin_tasks.sh **Script As Root. While Looking At The **admin_tasks.sh **File I Saw A Function Namely **backup_web()**, It Calls A Python Script In The Same Directory.

![1*SwvMNcnx2JhOYKK0sey5fQ.png](../_resources/c1bcb30040b8eddae08f8e1a7a9da843.png)
![1*SwvMNcnx2JhOYKK0sey5fQ.png](../_resources/7d06387f003005b5c41fa15e53aa10a0.png)

The **backup.py **Python Script Makes A Backup Of The **html **Directory And Converts Them Into An Archive. So, We Got Nothing To Do With That Python Script. Since, **admin_tasks.sh **Can Be Executed As Root The Called Python Script Also Be Executed As Root.

If We Modify The Bash Or Python Script We Can Achieve Root Access. But Both Files Are Not Writable By The User **waldo**.

![1*bcZYx-BtjqaVVP0YGGQzEw.png](../_resources/d79a257657e524f846dcafcb8f9cc0ca.png)
![1*bcZYx-BtjqaVVP0YGGQzEw.png](../_resources/53d425bbf0e97e76f00495c1f1550b28.png)

After Searching Some Time On The Inter I Found This, Privilege Escalation Via Python Library Hijacking.

[ ## Privilege Escalation Via Python Library Hijacking   #### rastating.github.io](https://rastating.github.io/privilege-escalation-via-python-library-hijacking/)

If I Change The Python’s Import Directory, And Created My Own **shutil **Python Script I Can Manage To Obtain Root Access. Because It’s My Own Python Script And I Make The Script To Execute Anything I Want.

[ ## Command Line And Environment - Python 3.8.3 Documentation   #### docs.python.org](https://docs.python.org/3/using/cmdline.html#environment-variables)

While Searching For Modules To Import ( Such As Shitil Above), Python Utilizes The Idea Of PATH Environment Variables, Similar To How Unix Searchs For Binaries. One Of These Variables Is Called **PYTHONPATH **And **PYTHONPATH** Directories Will Always Be Searched First.

I Made My Own **shutil.py **And Used **os **Module To Call OS Funcations And Execute Any Command That I Want.

![1*83Oh9gGxgUNPyn6z3mOTJw.png](../_resources/1037bab47d4088418123287e050655b4.png)
![1*83Oh9gGxgUNPyn6z3mOTJw.png](../_resources/9253c69eb56eb979a6a17d8f86432369.png)
Now I Ran The Bash Script With The Help Of **PYTHONPATH **Variable.
**sudo PYTHONPATH=/tmp/parsnips /opt/scripts/admin_tasks.sh**
![1*GY4N-DEriV807_VdoVA7-g.png](../_resources/34a6dbe027189cb3cd4944d1f8a41478.png)
![1*GY4N-DEriV807_VdoVA7-g.png](../_resources/233488284243df981edbb5991007b4d2.png)

Then I Came Back To My **Netcat **Listener, I Got Shell As Root. As I Got Root Shell, I Went To The **root **Directory And Grabbed The **root.txt **(Root Flag). Now I Got The Root Flag.

![1*dANYNCMXlk00bmrEZDU7zA.png](../_resources/9c6fe987407449cfeebaec186bb8275f.png)
![1*dANYNCMXlk00bmrEZDU7zA.png](../_resources/4eac6673df009d4df3792b6f20218a1e.png)
**Root: 6df1bcbd7d0eb6b94ed8759cf4b621d7**
That’s All, Thanks For Reading.