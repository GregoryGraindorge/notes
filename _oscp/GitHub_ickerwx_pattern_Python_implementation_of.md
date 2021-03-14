GitHub - ickerwx/pattern: Python implementation of Metasploit's pattern_create/pattern_offset.

Notes:

pattern create 100
pattern offset abd1 100

##  README.md

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='40'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='632' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ickerwx/pattern#pattern---a-reimplementation-of-pattern_createpattern_offset-in-python)Pattern - a reimplementation of pattern_create/pattern_offset in Python

Metasploit comes with multiple very convenient scripts and utilities. Among the most valuable for me are pattern_create and pattern_offset. I found it increasingly annoying that both have a relatively long startup time.

So after I overheard one of my colleagues complaining about this as well, I quickly hacked together a Python version that basically does the same.

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='41'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='636' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ickerwx/pattern#usage)Usage

`$ ./pattern[[NEWLINE]]Usage: pattern (create | offset) <value> <buflen>[[NEWLINE]]`

So, to create a 2048 byte pattern you run

`$ ./pattern create 2048[[NEWLINE]]Aa0Aa1Aa2[...snip...]p7Cp8Cp9Cq0Cq1Cq[[NEWLINE]]`

and it outputs a unique pattern of said length. To find an offset in the buffer, let's say f9Cg, you run

`$ ./pattern offset f9Cg[[NEWLINE]]1738[[NEWLINE]]`

and the program returns the offset until the requested series. You can also look for memory values. The values need to be little-endian and prefixed with 0x:

`$ ./pattern offset 0x67433966[[NEWLINE]]1738[[NEWLINE]]`

To make sure the program works as close to the Metasploit version as possible, the offset mode searches through a 8192 byte pattern, just like pattern_offset. If your pattern is longer than that, append the pattern length:

`$ ./pattern offset 9Mc0[[NEWLINE]]Not found[[NEWLINE]]$ ./pattern offset 9Mc0 10000[[NEWLINE]]9419[[NEWLINE]]`

## [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='42'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='642' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/ickerwx/pattern#todo)Todo

- bad characters support for pattern creation
- partial matches