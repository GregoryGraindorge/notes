Command - TTY Shell

Links
https://forum.hackthebox.eu/discussion/142/obtaining-a-fully-interactive-shell

# Spawning a TTY Shell

[Peleus](https://netsec.ws/?author=1)

Often during pen tests you may obtain a shell without having tty, yet wish to interact further with the system. Here are some commands which will allow you to spawn a tty shell. Obviously some of this will depend on the system environment and installed packages.

**Shell Spawning**

- python -c 'import pty; pty.spawn("/bin/sh")'
- echo os.system('/bin/bash')
- /bin/sh -i
- perl —e 'exec "/bin/sh";'
- perl: exec "/bin/sh";
- ruby: exec "/bin/sh"
- lua: os.execute('/bin/sh')
- (From within IRB)

exec "/bin/sh"

- (From within vi)

:!bash

- (From within vi)

:set shell=/bin/bash:shell

- (From within nmap)

!sh

Many of these will also allow you to escape jail shells. The top 3 would be my most successful in general for spawning from the command line.

* * *

With python available

python -c 'import pty;pty.spawn("/bin/bash");'

After that, do CTRL+Z to background Netcat.

Enter stty raw -echo in your terminal, which will tell your terminal to pass keyboard shortcuts etc. through.

Once that is done, run the command fg to bring Netcat back to the foreground.

Note you will not be able to see what you are typing in terminal after you change your stty setting.

You should now have tab autocomplete as well as be able to use interactive commands such as su and nano.