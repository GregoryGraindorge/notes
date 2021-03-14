# Find Files That Have Been Modified Recently in Linux - Baeldung on Linux

> Learn how to find files that have been changed recently in Linux.

1\. Introduction[](https://www.baeldung.com/linux/recently-changed-files#introduction)
--------------------------------------------------------------------------------------

There are various occasions when we want to search for files that have been changed recently.

For example, as a system admin, we’re responsible to maintain and configure computer systems. Sometimes, because we’re dealing with a lot of configuration files, we probably want to know what are the files recently modified.

In this tutorial, **we’re going to find the files that have been changed recently in Linux using bash commands.**

2\. The _find_ Command[](https://www.baeldung.com/linux/recently-changed-files#find-command)
--------------------------------------------------------------------------------------------

First, we’ll explore the [_find_ utility](https://linux.die.net/man/1/find) which is the most common way to achieve the intended purpose. This command is used to **find files and directories recursively and to execute further operations on them.** 

### 2.1. _\-mtime_ and _\-mmin_[](https://www.baeldung.com/linux/recently-changed-files#1--mtime-and--mmin)

_\-mtime_ is handy, for example, if we want to find all the files from the current directory that have changed in the last 24 hours:

    find . -mtime -1

Note that the _._ is used to refer to the current directory. **_\-mtime n_ is an expression that finds the files and directories that have been modified exactly _n_ days ago**.

In addition, the expression can be used in two other ways:

*   _\-mtime +n_ = finds the files and directories modified more than _n_ days ago
*   _\-mtime -n_ = finds the files and directories modified less than _n_ days ago

In the same way, **we can use the _\-mmin n_ expression to rely on minutes instead of days**:

    find /home/sports -mmin +120

So, this command recursively finds all the files and directories from the _/home/sports_ directory modified at least 120 minutes ago.

Next, if we want to limit the searching only to files, excluding directories, we need to add the _\-type f_ expression:

    find /home/sports -type f -mmin +120

Furthermore, we can even compose expressions. So, let’s find the files that have been changed less than 120 minutes ago and more than 60 minutes ago:

    find . -type f -mmin -120 -mmin +60

### 2.2. _\-newermt_[](https://www.baeldung.com/linux/recently-changed-files#2--newermt)

**There are times when we want to find the files that were modified based on a particular date.** In order to fulfill this requirement, we have to explore another parameter, which has the following syntax:

    -newermt 'yyyy-mm-dd'

By using this expression, we can get the files that have been changed earlier than the specified date.

So, let’s build a command to better understand the new parameter:

    find . -type f -newermt 2019-07-24

Moreover, **we could get the files modified on a specific date by using a composed expression.**

So, we’re going to get the files modified on ‘2019-07-24’:

    find . -type f -newermt 2019-07-24 ! -newermt 2019-07-25

Finally, there’s **another version of the _-newermt_ parameter similar to _\-mmin_ and _\-mtime_**.

The first command finds the files modified in the last 24 hours. The rest of them are similar:

    find . -type f -newermt "-24 hours" 
    find . -type f -newermt "-10 minutes" 
    find . -type f -newermt "1 day ago" 
    find . -type f -newermt "yesterday"

3\. The _ls_ Command[](https://www.baeldung.com/linux/recently-changed-files#ls-command)
----------------------------------------------------------------------------------------

We know that the [_ls_ command](https://linux.die.net/man/1/ls) lists information about the files in a specific directory. One of its usages is to show the long format of the files and to sort the output by modification time:

    ls -lt

Which would result in something like:

    -rw-r--r-- 1 root root 4233 Jul 27 18:44 b.txt 
    -rw-rw-r-- 1 root root 2946 Jul 27 18:12 linux-commands.txt 
    -rw-r--r-- 1 root root 5233 Jul 20 17:02 a.txt

We may not be able to list the files recently modified exactly as the _find_ command does. But, **we can filter the above output based on a specific date or time by applying the _grep_ command on the result of the _ls_ command**:

    ls -lt | grep 'Jul 27'

    -rw-r--r-- 1 root root 4233 Jul 27 18:44 b.txt 
    -rw-rw-r-- 1 root root 2946 Jul 27 18:12 linux-commands.txt

    ls -lt | grep '17:'

    -rw-r--r-- 1 root root 5233 Jul 20 17:02 a.txt

Note that the _find_ command is recursive by default. In order to enable the recursive capability on the _ls_ command, we also need to add the _R_(uppercase) parameter:

    ls -ltR

4\. Conclusion[](https://www.baeldung.com/linux/recently-changed-files#conclusion)
----------------------------------------------------------------------------------

In this quick tutorial, we’ve described a few ways that help us find the files that have been changed recently on a Linux operating system.

First, we’ve explored the _find_ command and created several examples with different parameters like _\-mtime_, _\-mmin_ and _\-newermt_.

Then, we’ve shown how we can achieve similar results using a combination of two better known Linux utilities like the _ls_ and _grep_ commands.


[Source](https://www.baeldung.com/linux/recently-changed-files)
