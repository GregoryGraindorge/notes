GitHub - Gallopsled/pwntools: CTF framework and exploit development library

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='63'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='976' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/Gallopsled/pwntools#pwntools---ctf-toolkit)pwntools - CTF toolkit

[![logo.png](../_resources/2532f6edb5383bf7fa5c8c997b6e39ed.png)](https://github.com/Gallopsled/pwntools/blob/stable/docs/source/logo.png?raw=true)

[![68747470733a2f2f696d672e736869656c64732e696f2f707970692f762f70776e746f6f6c733f7374796c653d666c6174](../_resources/41b058c6ae16e4e3f86e3638196f4d08.png)](https://pypi.python.org/pypi/pwntools/)[![68747470733a2f2f72656164746865646f63732e6f72672f70726f6a656374732f70776e746f6f6c732f62616467652f3f76657273696f6e3d737461626c65](../_resources/970a0949bffc9039c2ed68a7f8e83a0f.png)](https://docs.pwntools.com/)[![68747470733a2f2f696d672e736869656c64732e696f2f7472617669732f47616c6c6f70736c65642f70776e746f6f6c732f6465763f6c6f676f3d547261766973](../_resources/7c181bb807da9f163015b989a97ab974.png)](https://travis-ci.org/Gallopsled/pwntools)[![68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f776f726b666c6f772f7374617475732f47616c6c6f70736c65642f70776e746f6f6c732f436f6e74696e756f7573253230496e746567726174696f6e2f6465763f6c6f676f3d476974487562](../_resources/f291b870d042d5294973a008c6f3440e.png)](https://camo.githubusercontent.com/8db014ce20c73b8d9116e51839261f156b88fdd5/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f776f726b666c6f772f7374617475732f47616c6c6f70736c65642f70776e746f6f6c732f436f6e74696e756f7573253230496e746567726174696f6e2f6465763f6c6f676f3d476974487562)[![68747470733a2f2f696d672e736869656c64732e696f2f636f766572616c6c732f6769746875622f47616c6c6f70736c65642f70776e746f6f6c732f6465763f6c6f676f3d636f766572616c6c73](../_resources/f46d1cda5ca7c9adcb8b3d15e2e3ae65.png)](https://coveralls.io/github/Gallopsled/pwntools?branch=dev)[![68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6c6963656e73652d4d49542d626c75652e7376673f7374796c653d666c6174](../_resources/eac0cc8e9758690eb2517a30e37c1903.png)](http://choosealicense.com/licenses/mit/)[![68747470733a2f2f696d672e736869656c64732e696f2f747769747465722f666f6c6c6f772f50776e746f6f6c73](../_resources/2cefe6dff0015d756c1f62f06e4ce861.png)](https://twitter.com/pwntools)

Pwntools is a CTF framework and exploit development library. Written in Python, it is designed for rapid prototyping and development, and intended to make exploit writing as simple as possible.

from  pwn  import  *context(arch  =  'i386', os  =  'linux')r  =  remote('exploitme.example.com', 31337)# EXPLOIT CODE GOES HEREr.send(asm(shellcraft.sh()))r.interactive()

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='64'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='1005' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/Gallopsled/pwntools#documentation)Documentation

Our documentation is available at [docs.pwntools.com](https://docs.pwntools.com/)

A series of tutorials is also [available online](https://github.com/Gallopsled/pwntools-tutorial#readme)

To get you started, we've provided some example solutions for past CTF challenges in our [write-ups repository](https://github.com/Gallopsled/pwntools-write-ups).

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='65'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='1009' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/Gallopsled/pwntools#installation)Installation

Pwntools is best supported on 64-bit Ubuntu LTE releases (14.04, 16.04, 18.04, and 20.04). Most functionality should work on any Posix-like distribution (Debian, Arch, FreeBSD, OSX, etc.). Python >= 2.7 is required (Python 3 suggested as best).

Most of the functionality of pwntools is self-contained and Python-only. You should be able to get running quickly with

apt-get update

apt-get install python3 python3-pip python3-dev git libssl-dev libffi-dev build-essential

python3 -m pip install --upgrade pip
python3 -m pip install --upgrade pwntools

However, some of the features (assembling/disassembling foreign architectures) require non-Python dependencies. For more information, see the [complete installation instructions here](https://docs.pwntools.com/en/stable/install.html).

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='66'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='1014' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/Gallopsled/pwntools#contribution)Contribution

See [CONTRIBUTING.md](https://github.com/Gallopsled/pwntools/blob/dev/CONTRIBUTING.md)

# [![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' class='octicon octicon-link js-evernote-checked' viewBox='0 0 16 16' version='1.1' width='16' height='16' aria-hidden='true' data-evernote-id='67'%3e%3cpath fill-rule='evenodd' d='M7.775 3.275a.75.75 0 001.06 1.06l1.25-1.25a2 2 0 112.83 2.83l-2.5 2.5a2 2 0 01-2.83 0 .75.75 0 00-1.06 1.06 3.5 3.5 0 004.95 0l2.5-2.5a3.5 3.5 0 00-4.95-4.95l-1.25 1.25zm-4.69 9.64a2 2 0 010-2.83l2.5-2.5a2 2 0 012.83 0 .75.75 0 001.06-1.06 3.5 3.5 0 00-4.95 0l-2.5 2.5a3.5 3.5 0 004.95 4.95l1.25-1.25a.75.75 0 00-1.06-1.06l-1.25 1.25a2 2 0 01-2.83 0z' data-evernote-id='1016' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://github.com/Gallopsled/pwntools#contact)Contact

If you have any questions not worthy of a [bug report](https://github.com/Gallopsled/pwntools/issues), feel free to ping us at `#pwntools` on Freenode and ask away. Click [here](https://kiwiirc.com/client/irc.freenode.net/pwntools) to connect. There is also a [mailing list](https://groups.google.com/forum/#!forum/pwntools-users) for higher latency discussion.