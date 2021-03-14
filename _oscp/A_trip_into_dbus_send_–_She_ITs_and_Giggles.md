A trip into dbus-send – She ITs and Giggles

[Uncategorized](https://sheitsandgiggles.com/category/uncategorized/)

# A trip into dbus-send

DBus is the interprocess communication mechanism used by the plumbing layer of Linux to allow the various components to use each others services without each of them needing to implement custom code for every other component. Even for a sysadmin its a fairly esoteric subject but it does help explain how another bit of linux works. However I’ve always found exploring it confusing and I tend to forget the hard won lessons soon afterwards. This tim eI decided to post my notes to the internet in the hope that next time I feel the need to explore the world of dbus I have a place to start.

Firstly there are two buses, a session bus that is individual to the users session and used heavily by gnome and other user software, and the system bus used by the plumbing layer.

Each bus has a number of services registered on it that can be listed with the dbus-send command by querying the DBus service itself.

`dbus-send --system --print-reply --dest=org.freedesktop.DBus /org/freedesktop/DBus org.freedesktop.DBus.ListNames`

which produces an array of the services:
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]

Each service implements at least one object, which has a number of interfaces, which are groups of methods and properties. Lots of different objects can implement the same interface, and so implement the same set of methods and properties this makes it easy to treat them all in a similar way. Two interfaces pretty much everything implements are the org.freedesktop.DBus.Introspectable interface which has a single method “Introspect” that returns a list of all the interfaces, methods, and properties the object implements. The other common interface is org.freedesktop.DBus.Properties which provides common methods to get and set all the properties of the object.

A dbus send command to invoke one of these methods looks like
1
[object Object]

so a call to the org.freedesktop.systemd1 service listed in the output above needs the name of an object, but all services implement an object named similar to their service name except with / replacing dots giving /org/freedesktop/systemd1 and as discussed above all objects implement the org.freedesktop.DBus.Introspectable interface with its Introspect method, so we can call:

1
[object Object]

Which lists all the methods and properties available for systemd in an xml format, e.g.:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]

If you’ve followed along so far it should then come as no surprise that you can run the ListUnits method and get a list of all the units managed by systemd:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
[object Object]
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]

(the 2e in the path is the ascii code for a period. the question of why I have ypbind installed on my laptop is for another day, but at least its not running)

This tells us that ypbind has its own object on the bus called /org/freedesktop/systemd1/unit/ypbind_2eservice, and indeed

1
[object Object]

Lists its methods and properties because it too implements the org.freedesktop.DBus.Introspectable interface. The –dest option is still set to org.freedesktop.systemd1 because ybbind_2eservice is a child object of systemd1. But why was this not listed when we introspected the org.freedsktop.DBus object?

Objects within a service form a hierarchy, and introspecting any object will only list the direct child objects that belong to it. org.freedesktop.DBus has no child objects of its own and the ListNames method we used initially lists the top level services register on the bus, not all the objects they provide. So how then do we seek out these child objects?

When you introspect an object it not only lists the methods and properties of the object, but it also lists the names of the child objects or nodes that sit directly below it. We can see these by introspecting again:

1
2
3
4
5
[object Object]
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]
We can then introspect these in turn
1
2
3
4
5
6
[object Object]
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]

and as these are children of /org/freedesktop/systemd1/unit that gives us the /org/freedesktop/systemd1/unit/ypbind_2eservice object we saw above.

While listing objects is fun we really want to be able to send then messages over the bus, or in other words call their methods.

Pretty much every object on the bus implements the org.freedesktop.DBus.Properties.Get interface that provides methods to set and get parameters. In the introspection output above there were methods described like:

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object][object Object]

Arguments with direction “in” obviously need to be passed in and those with direction “out” are the return values. Each parameter needs to have its data type appended to its value, and they are passed on the dbus-send command line in the order specified. So to get the value of the MainPID property we need to pass the interface and the property name, both strings (s):

1
2
3
4
[object Object]

[object Object]
[object Object][object Object]

and we get back a variant (type=v), in this case the pid of the docker containerd process

1
2
3
[object Object]

[object Object]
Other methods let us start and stop the unit
1
2
3
4
5
6
7
8
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]
[object Object]
[object Object][object Object]
[object Object][object Object]
[object Object]

These parameters have no names given but a bit of trial and reading of error messages suggests that in “in” parameter is job-mode, described in the systemctl man page as controling “how to deal with already queued jobs”, with “replace” as a valid option. So we can restart the service simply by:

1
[object Object]
And we can now inspect the PID of the process again:

	# dbus-send --system --print-reply --dest=org.freedesktop.systemd1 /org/freedesktop/systemd1/unit/docker_2dcontainerd_2eservice org.freedesktop.DBus.Properties.Get string:org.freedesktop.systemd1.Service string:MainPID

	method return time=1561069031.851603 sender=:1.5 -> destination=:1.34460 serial=47532 reply_serial=2
	   variant       uint32 31169

The process number is now reporting as 31169, and when we double check:

	# ps -ef | grep 31169
	root     31169     1  0 23:14 ?        00:00:00 /usr/libexec/docker/docker-containerd-current --listen unix:///run/containerd.sock --shim /usr/libexec/docker/docker-containerd-shim-current --start-timeout 2m

And we can see the pid of the docker service has changed after it was restarted.

And there we have it, how to explore and manipulate the system via dbus.

One final bonus, systemd can also be used to control the power state of the machine, is this the most verbose reboot command possible?

1
[object Object]

### Share this:

- [Twitter](https://sheitsandgiggles.com/2019/07/16/a-trip-into-dbus-send/?share=twitter&nb=1)
- [LinkedIn](https://sheitsandgiggles.com/2019/07/16/a-trip-into-dbus-send/?share=linkedin&nb=1)

-

[Like](https://widgets.wp.com/likes/index.html?ver=20200826#)
Be the first to like this.
[July 16, 2019](https://sheitsandgiggles.com/2019/07/16/a-trip-into-dbus-send/)
![3c65b065c2d70fe9492e80da0b1126bd](../_resources/5956a88f7a218f0040936f379bde1766.png)

## Published by chrisp

[View all posts by chrisp](https://sheitsandgiggles.com/author/chrisproctercouk/)