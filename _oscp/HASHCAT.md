# Cypher decrypt
## Tools
- [Cypher identifier](https://www.boxentriq.com/code-breaking/cipher-identifier)
- [Cypher decrypter](https://www.dcode.fr/vigenere-cipher)

## Examples of cypher decrypt methodology
https://f3v3r.in/htb/fortess/akerva/

# HASHCAT
https://hashes.org/search.php (to check if hash is already known)

## Generate a password list

- Generate a small list of password (pwlist.txt)
- Add more passords in the list with a for loop.

		for i in $(cat pwlist.txt); do echo $i; echo ${i}2019; echo ${i}2020; done > t
		for i in $(cat pwlist.txt); do echo $i; echo ${i}\!; done > t

		mv t pwlist.txt

- Use hashcat to make a more complex list

		hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule
		hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule (more complex)
		hashcat --force --stdout pwlist.txt -r /usr/share/hashcat/rules/best64.rule -r /usr/share/hashcat/rules/toggles1.rule | awk 'length($0) > 7' (min 7 letters)

## To identify the hash type  
https://medium.com/infosec-adventures/identifying-and-cracking-hashes-7d580b9fd7f1

***For a $2y$..... hash use the 3200 mode.***

	~$ cd hash-identifier
	~/hash-identifier$ ls

	hash-id.py README.md screenshots

	~/hash-identifier$ python3 hash-id.py

## To get password's hashes examples (Check Ippsec forest video)

	./hashcat --example-hashes | grep -i krb
	./hashcat --example-hashes | less

--> /krb5tgs23 (to search for the mode --> 18200)

	hashcat --example-hashes | grep HASH | less | awk '{print $2}' | awk 'length($0) == 24'
	hashcat --example-hashes | grep -B 2 -E '^HASH: .{96}$'

## To crack a hash

	./hashcat -m 18200(got it from hashcat examples below) hashesDIR/hash /opt/wordlist/rockyou.txt -r /usr/share/hashcat/rules/InsidePro-PasswordsPro.rule

- if ntlm we can use --user and hashcat is gonna make the awk filter for us so we can put every hashes in a file without taking care of extracting the user part only. (ntlm from secretdump = 1000)

- if user: hash format, we can use the argument `--username` to specify that we included the username in the hash.

- If mode 18200 -> **Insert 23$ after $krb5asrep$ so that the first line will be $krb5asrep$23$User.....**

### Crack NTLM hash

	hashcat -m 1000 --force eriu3485idrueprwiuo /usr/share/wordlist/rockyou.txt

### Check previously cracked hashes

	./hashcat -m 3200 hashes --show --username
