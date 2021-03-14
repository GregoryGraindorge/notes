dbus-send(1) - Send a message to a message bus (Man Page)...

# DBUS−SEND

[NAME](https://sarata.com/manpages/dbus-send.1.html#NAME)
[SYNOPSIS](https://sarata.com/manpages/dbus-send.1.html#SYNOPSIS)
[DESCRIPTION](https://sarata.com/manpages/dbus-send.1.html#DESCRIPTION)
[OPTIONS](https://sarata.com/manpages/dbus-send.1.html#OPTIONS)
[AUTHOR](https://sarata.com/manpages/dbus-send.1.html#AUTHOR)
[BUGS](https://sarata.com/manpages/dbus-send.1.html#BUGS)

* * *

## NAME

dbus-send − Send a message to a message bus

## SYNOPSIS

|     |     |
| --- | --- |
|     | **dbus−send**[−−system \| −−session \| −−address=*ADDRESS*] [−−dest=*NAME*] [−−print−reply [*=literal*]] [−−reply−timeout=*MSEC*] [−−type=*TYPE*] *OBJECT_PATH INTERFACE.MEMBER* [*CONTENTS*...] |

## DESCRIPTION

The**dbus−send** command is used to send a message to a D−Bus message bus. See**http://www.freedesktop.org/software/dbus/**for more information about the big picture.

There are two well−known message buses: the systemwide message bus (installed on many systems as the "messagebus" service) and the per−user−login−session message bus (started each time a user logs in). The**−−system** and **−−session**options direct **dbus−send** to send messages to the system or session buses respectively. If neither is specified, **dbus−send** sends to the session bus.

Nearly all uses of **dbus−send** must provide the**−−dest** argument which is the name of a connection on the bus to send the message to. If**−−dest** is omitted, no destination is set.

The object path and the name of the message to send must always be specified. Following arguments, if any, are the message contents (message arguments). These are given as type−specified values and may include containers (arrays, dicts, and variants) as described below.

<contents> ::= <item> | <container> [ <item> | <container>...]
<item> ::= <type>:<value>
<container> ::= <array> | <dict> | <variant>
<array> ::= array:<type>:<value>[,<value>...]
<dict> ::= dict:<type>:<type>:<key>,<value>[,<key>,<value>...]
<variant> ::= variant:<type>:<value>

<type> ::= string | int16 | uint 16 | int32 | uint32 | int64 | uint64 | double | byte | boolean | objpath

D−Bus supports more types than these, but **dbus−send**currently does not. Also, **dbus−send** does not permit empty containers or nested containers (e.g. arrays of variants).

Here is an example invocation:
dbus−send −−dest=org.freedesktop.ExampleName \
/org/freedesktop/sample/object/name \
org.freedesktop.ExampleInterface.ExampleMethod \
int32:47 string:'hello world' double:65.32 \
array:string:"1st item","next item","last item" \
dict:string:int32:"one",1,"two",2,"three",3 \
variant:int32:−8 \
objpath:/org/freedesktop/sample/object/name

Note that the interface is separated from a method or signal name by a dot, though in the actual protocol the interface and the interface member are separate fields.

## OPTIONS

The following options are supported:
**−−dest=***NAME*
Specify the name of the connection to receive the message.
**−−print−reply**

Block for a reply to the message sent, and print any reply received in a human−readable form. It also means the message type (**−−type=**) is**method_call**.

**−−print−reply=literal**

Block for a reply to the message sent, and print the body of the reply. If the reply is an object path or a string, it is printed literally, with no punctuation, escape characters etc.

**−−reply−timeout=***MSEC*

Wait for a reply for up to *MSEC* milliseconds. The default is implementation-defined, typically 25 seconds.

**−−system**
Send to the system message bus.
**−−session**
Send to the session message bus. (This is the default.)
**−−address=***ADDRESS*
Send to*ADDRESS*.
**−−type=***TYPE*
Specify**method_call** or **signal** (defaults to "**signal**").

## AUTHOR

dbus−send was written by Philip Blundell.

## BUGS

Please send bug reports to the D−Bus mailing list or bug tracker, see**http://www.freedesktop.org/software/dbus/**

* * *

## Open Source

**find** - The find program searches for files that match the specified criteria and optionally executes some function on those files. Find can compare or identify files based on name, location, type, size, creation and many more attributes. [find](https://sarata.com/linux_find.html) is a very useful and powerful command line utility.

**Open Opportunity** - Free and Open Source software provide unlimited potential for personal and community development. Use open source to build a career, establish a business or change the world. Learn why free software provides the greatest [opportunity](https://sarata.com/open_opportunity.html) on the the planet today.

**College Prep** - Get ready for college or earn college credit. EdX provides more than 40 High School and AP Exam Preparation Courses. Subjects range from mathematics to science, English and history. EdX courses help students succeed in High School and prepare for college. [pre-calc](https://www.edx.org/course/pre-university-calculus-delftx-calcsp01x)  [calculus](https://www.edx.org/course/preparing-ap-calculus-ab-exam-part-2-ricex-advcalcab-2x)

**bash** - Bash is both a command interpreter and a programming language. As a command interpreter, bash provides the user interface to the rich set of GNU utilities. The programming language features allow these utilities to be combined. Mastering a shell such as [bash](https://sarata.com/linux_bash.html) is important for anyone learning Linux.

**Free Technology Academy** - The [FTA](http://ftacademy.org/) provides a virtual campus offering course modules on Free Software and Open Standards. Educational materials in the FTA are released under free licenses. FTA is constructed on a Free Software OS and utilizes Free Software and standards to deliver its services.

**What is Linux** - Linux is a computer operating system (OS) that is free and open-source software. The software that runs the computer and the source code used to create it are both available to you at no cost. [Linux](https://sarata.com/what_is_linux.html) is used on games, watches, laptops, desktops and super computers. The capability to operate and create on these devices is available to you.

**President Obama Knows Open Source** - On January 20 2009, **President Obama's first day in office**, the [Open Government](https://sarata.com/open_government.html) initiative was issued to provide transparency and access to Government data. Learn how our Government is using open source and the opportunities this provides for you.

## More Linux Commands

[**iso-8859-2 (7)       - ISO 8859-2 character set encoded in octal, decimal, an...**](https://sarata.com/manpages/iso-8859-2.7.html)

The ISO 8859 standard includes several 8-bit extensions to the ASCII character set (also known as ISO 646-IRV). ISO 8859-2, the Latin Alphabet No. 2 is used to...

 [**glTexCoord (3gl)     - set the current texture coordinates**](https://sarata.com/manpages/glTexCoord.3gl.html)

glTexCoord specifies texture coordinates in one, two, three, or four dimensions. glTexCoord1 sets the current texture coordinates to (s, 0, 0, 1); a call to glT...

 [**XpGetImageResolution (3x) - Gets the current image resolution for a print con...**](https://sarata.com/manpages/XpGetImageResolution.3x.html)

XpGetImageResolution returns the current image resolution for the specified print context. A value of zero means the resolution automatically tracks the printer...

 [**gnutls_hex_decode (3) - API function**](https://sarata.com/manpages/gnutls_hex_decode.3.html)

This function will decode the given encoded data, using the hex encoding used by PSK password files. Note that hex_data should be null terminated. RETURNS GNUTL...

 [**zshcalsys (1)        - zsh calendar system**](https://sarata.com/manpages/zshcalsys.1.html)

The shell is supplied with a series of functions to replace and enhance the traditional Unix calendar programme, which warns the user of imminent or future even...

 [**stat (1)             - display file or file system status**](https://sarata.com/manpages/stat.1.html)

Display file or file system status. Mandatory arguments to long options are mandatory for short options too. -L, --dereference follow links -f, --file-system di...

 [**apply (n)            - Apply an anonymous function**](https://sarata.com/manpages/apply.n.html)

The command apply applies the function func to the arguments arg1 arg2 ... and returns the result. The function func is a two element list {args body} or a thre...

 [**sem_post (3)         - unlock a semaphore**](https://sarata.com/manpages/sem_post.3.html)

sem_post() increments (unlocks) the semaphore pointed to by sem. If the semaphores value consequently becomes greater than zero, then another process or thread...

 [**ab2 (1)              - Apache HTTP server benchmarking tool**](https://sarata.com/manpages/ab2.1.html)

ab2.1 - ab is a tool for benchmarking your Apache Hypertext Transfer Protocol (HTTP) server. It is designed to give you an impression of how your current Apache...

 [**sane-as6e (5)        - SANE backend for using the Artec AS6E parallel port in...**](https://sarata.com/manpages/sane-as6e.5.html)

The sane-as6e library implements a SANE (Scanner Access Now Easy) backend that provides access to Artec AS6E flatbed scanner. It requires the as6edriver program...

 [**stap-merge (1)       - systemtap per-cpu binary merger**](https://sarata.com/manpages/stap-merge.1.html)

The stap-merge executable applies when the -b option has been used while running a stap script. The -b option will generate files per-cpu, based on the timestam...

 [**wacom (4)            - Wacom input driver**](https://sarata.com/manpages/wacom.4.html)

wacom is an X input driver for Wacom devices. The wacom driver functions as a pointer input device. SUPPORTED HARDWARE This driver supports the Wacom IV and Wac...

 [**badblocks (8)        - search a device for bad blocks**](https://sarata.com/manpages/badblocks.8.html)

badblocks is used to search for bad blocks on a device (usually a disk partition). device is the special file corresponding to the device (e.g /dev/hdc1). last-...

 [**redland-config (1)   - script to get information about the installed version ...**](https://sarata.com/manpages/redland-config.1.html)

redland-config is a tool that is used to determine the compile and linker flags that should be used to compile and link programs that use the Redland RDF librar...

 [**mib2c (1)            - generate template code for extending the agent**](https://sarata.com/manpages/mib2c.1.html)

The mib2c tool is designed to take a portion of the MIB tree (as defined by a MIB file) and generate the template C code necessary to implement the relevant man...