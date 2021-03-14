# Bug bounty methodology

## To do
- [ ] [The Web Application Hacker's Handbook](https://www.bol.com/nl/f/the-web-application-hacker-s-handbook/9200000005373096/)
- [ ] Learn more about `Google Dork`
- [ ] Create an account on `[vultr](https://www.vultr.com/features/upload-iso/)` and look at how to deploy an instance with `arch` linux.
- [ ] Check what's better between `aquatone` and `eyeWitness`. Aquatone doesn't support `ssl` ???


## References
- https://thehackerish.com/my-bug-bounty-methodology-and-how-i-approach-a-target/
- https://www.shellinthecity.com/bug-bounty-hunter-methodology/
- https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html
- https://0xpatrik.com/subdomain-enumeration-2019/
- https://pentester.land/conference-notes/2018/08/02/levelup-2018-the-bug-hunters-methodology-v3.html

- [Interesting article about VPS cloud providers](https://pentester.land/tips-n-tricks/2018/08/16/cloud-vps-providers-for-bug-hunters.html)
## Choose a bug bounty program

### Program launch date
- When the bug bounty program was launched to have an idea of how old the program is.

### Program responsiveness
- Response posture. 
	- If the program takes a lot of time to resolve security issues, it means that there is a higher chance of getting duplicates. Usually, all other response metrics, such as time to first response, time to triage and time to bounty are lower than the resolution time, so the shorter it is, the better.  
	- You can also see the percentage of the reports which have met those response metrics. If it is above 90%, I’d probably accept the invitation if the rest of the metrics is ok.

### The scope of the bug bounty program
- Prefer bigger scopes. 
	- Wildcard domains are better than single web application because it reduces competition.

### Bug bounty rewards
- Prefer higher paying bug bounty programs. 
	- Avoid programs with no rewards not only because of money, but also because the reputation you get is significantly lower.

### The business of the company
- Check if the company’s business matches your values. If it doesn’t, I simply reject the invitation.

## First approach
### Subdomains
- [Here](https://0xpatrik.com/subdomain-enumeration-2019/) is another methodology. More accurate than the one below but also more complex)
- Light subdomain enumeration to gauge the public presence of the bug bounty program and quickly find something to work on.
- Always avoid brute forcing at this stage. Save it for later with a custom wordlist tailored just for this domain.

1. Start subdomain enumeration with Tomnomnom’s [](https://github.com/tomnomnom/assetfinder) [assetfinder](https://github.com/tomnomnom/assetfinder) tool.

		assetfinder --subs-only domain.name
	
	**Other tools that can be use for that step:**
	- [Amass](https://github.com/OWASP/Amass)
	- [Subfinder](https://github.com/projectdiscovery/subfinder)
	- [Massdns](https://github.com/blechschmidt/massdns)
	- [Findomain](https://github.com/Edu4rdSHL/findomain)
		- Awesome little tool that can sometimes find domains amass cant - Also very quick as its built with rust :)

	See [M0chan](https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html) cheat sheet to get more enumeration tools.

2. **Generate a wordlist for `DNS Brute Forcing`:**
	- [Commonspeak](https://github.com/pentester-io/commonspeak)
	- [Scans.io](https://scans.io/)
	- [John Haddix Wordlist](https://gist.github.com/jhaddix/86a06c5dc309d08580a018c66354a056)
3. (Optional) Brute Forcing

	- [Massdns](https://github.com/blechschmidt/massdns) is the go tool for brute forcing:
		- Can run a million line dictionary in 30 sec
		- Because it’s written in C and breaks up your wordlist into small pieaces & assigns each piece to a different DNS resolver in Parallel

### IP
- https://bgp.he.net : the most common one that people use to find an organization's autonomous system number (the IP range that they have registered)
- https://www.arin.net and https://www.ripe.net : to find an organization's registered IPs and domains
- https://www.domainiq.com/reverse_whois : give you anything related to a domain name via reverse whois lookup.
- https://www.shodan.io/ : this site references the result of massive port scans performed on the Internet. It's a gold mine of information for finding organization's internet systems.

### Top Level Domains (If wide scope)
> There is some Bug Bounty programs who have wide scope. It means that if you can identify that some infrastructures belong to that organization, they want you to try to hack it.

- [Wikipedia](https://www.wikipedia.org/)
- [Crunchbase](https://www.crunchbase.com/)
- Linked Discovery
	- The principle of this method is to basically visiting your target site itself, and see where it links out to.
- [DomLink](https://github.com/vysecurity/DomLink)
	- The next method is called DomLink, the idea with this method is to recursively looking at reverse whois programmatically (python script), based on who registered a domain and then creating a link between those domains.
- Wappalyzer
- Use [Google Dork](https://boxpiper.com/posts/google-dork-list) to look for `traderMark`: `TESLA @ 2015", "TESLA @ 2019`

## Web applications enumeration
1. Filter only web applications using Tomnomnom’s [httprobe](https://github.com/tomnomnom/httprobe).
2. Focus on ports 80 and 443.


	cat domains | httprobe


3. If the program is new, use [Shodan](https://www.shodan.io/) or perform a port scan using [masscan](https://github.com/robertdavidgraham/masscan) to see if any web applications are running on non-standard open ports. These are ports greater than 1024.

	
	masscan -p1-65535 -iL $TARGET\_LIST --max-rate 100000 -oG $TARGET\_OUTPUT


>*The -oG parameter allow to save the output in a nmap format, so we can re-scan it with nmap version scanning*

The problem with MasScan is that it won't take DNS name, it will only take IPs, so here is a small shell script to basically run a dig on a domain, it will also strip out a HTTP or HTTPS prefix if you are pulling it out of another tool, and then running MasScan against it :

```
#!/bin/bash
strip=$(echo $1 | sed 's/https\?:\/\///')
echo ""
echo "##################################"
host $strip
echo "##################################"
echo ""
masscan -p1-65535 $(dig +short $strip \
| grep -oE "\b([0-9](1,3)\.){3}[0-9]{1,3}\b" \
| head -1) \
--max-rate 1000 \
| & tee $strip_scan
```

- From [M0chan](https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html):
>Some people will tell you to use massscan due to the speed but I find it misses a lot of ports so VPS+ nMap + Screen is the most reliable.  

- The next step would be to use `BruteSpray`.
>BruteSpray is a tool that will take our previous scan output (nmap format with version scanning output), and analyse it, and when BruteSpray will encounters one of those remote administration protocols (like FTP or SSH) it will use a small wordlist to try to brute force those hosts (it's using Medusa), with common passwords, default passwords or anonymous login.

4. Run [aquatone](https://github.com/michenriksen/aquatone) to screenshot the list of live web applications.
	- Quickly spot any visual deviation from the common user interface.
	- Get a bird’s eye view of the different web application categories and technologies.
	- Other possible tools for that step:
		- [EyeWitness](https://github.com/FortyNorthSecurity/EyeWitness) - It works with `http` and `https`.

## Choosing a web application
- Choose the application which deviates from the herd. 

> If all web applications implement a centralized Single Sign-on authentication mechanism, I would look for any directly accessible asset. If I spot a user interface of common software such as monitoring tools, or known Content Management Systems, I would target them first. Another example is when the application discloses the name and the version of the software being used. In this case, I look online for any available exploits. If I am lucky, I might get easy issues to report.
>
> For the other custom-made web applications, I will generally choose the one whose user interface deviates from the common company’s theme. If I don’t find one, I might repeat my previous steps with deeper enumeration. For instance, I would take the subdomains I found earlier and combine them with the name of the company to generate a custom wordlist. Then, I’d use tools like [OWASP amass](https://github.com/OWASP/Amass) and brute force the subdomains using the wordlist I constructed.

## How to approach a web application

- Reduce the time between your first interaction with the program and this phase. 
- Enumerating as much as possible to draw the largest attack surface possible.

### Mapping the application features

> I do my best to focus on understanding the business features and making note of the interesting ones. For instance, I always look for file uploads, data export, rich text editors, etc.

- Open up the web browser and use the application as a normal user. 
	- Signup feature
	- Create a user and login 
	- Visit every tab 
	- Click on every link
	- Fill up every form 
	- Create an order using a fake credit card 

- Meanwhile, capture all the traffic with `ZAP`.

### Understanding the main application architecture and defense mechanisms
- How authentication is made?
- Does the application use a third-party for that?
- Is there any OAuth flow?
- Is there any CSRF protection?
- If yes, how is it implemented?
- Are there any resources referenced using numerical identifiers?
- If yes, is there any protection against [IDOR vulnerabilities](https://thehackerish.com/idor-explained-owasp-top-10-vulnerabilities/)?
- Does the application use any API?
- How does the application fetch data?
- Does it use a front-end Framework?
- What JavaScript files contain calls to the API?
- Does it use a back-end Framework?
- If yes, what is it and which version is being used?

### Platform Identification & CVE searching
- [retire.js](https://retirejs.github.io/retire.js/): Outdated libraries (cmd-line, Burp on online form)
- [builtwith](https://builtwith.com/): Stack information profiling
- [wappalyzer](https://www.wappalyzer.com/): Similar to Builtwith (cmd-line, browser extension or online form)
- [burp-vulners-scanner](https://github.com/vulnersCom/burp-vulners-scanner): Burp plugin, detects versions with CVEs

### JavaScript enumeration
> Whenever I have the opportunity to read some code, I make sure to do so.
- Collect and analyze `Javascript` codes.
- Hidden endpoints, Cross-site scripting and broken access control vulnerabilities can be found this way.
- Use tools like [LinkFinder](https://github.com/GerbenJavado/LinkFinder) to collect URLs and cross-reference with the endpoints collected from the mapping exercise.
- It could be done the other way around. Look for API endpoints in JavaScript files using the naming convention of the endpoints. This allows to save all the API endpoints into a file. It becomes handy when implementing some automation to detect when the developers add new endpoints to the application.
- Use `Zaproxy`'s `Ajax Spider` to spider the website with headless browser.

### Focusing on one feature at a time
> It all depends on your experience, but a solid start would be the OWASP Top 10. Covered in much detail in a [hands-on training](https://thehackerish.com/owasp-top-10-the-ultimate-guide/).

- Try to focus on one feature at a time.
- The goal is to learn the flow in detail
- If the request seems to be fetching data from a database, try SQL injection.
- If the user input gets returned, try Cross-Site Scripting.
- Check the [M0chan](https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html) or [Pentester Land](https://pentester.land/conference-notes/2018/08/02/levelup-2018-the-bug-hunters-methodology-v3.html) cheat sheets to get more clues on what to look for.


### WAF
- It’s common for bug hunters to get banned by WAF or CDN vendors security products
	- Predominant WAFs: Cloudflare & Akamai
	- Dedicated WAFS
- Solutions
	- Encoding (Meh)
	- Finding origin

![https://pentester.land/assets/img/conference-notes/the-bug-hunters-methodology-v3/akamai.jpg](https://pentester.land/assets/img/conference-notes/the-bug-hunters-methodology-v3/akamai.jpg)

- Finding dev
- If a WAF blocks you on domain.com, try bypassing it by going to:
	- dev.domain.com
	- stage.domain.com
	- ww1/ww2/ww3…domain.com
	- www.domain.uk/jp/… (regionalized domains)
