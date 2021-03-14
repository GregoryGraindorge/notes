Mounting VHD file on Kali Linux through remote share | by Samuel Whang | Medium

# Mounting VHD file on Kali Linux through remote share

[![2*_TIOPxRp9GLbOjgN7Mqr8w.jpeg](../_resources/c3cdcd7ab36b9c00596b64ce68966239.jpg)](https://medium.com/@klockw3rk?source=post_page-----f2f9542c1f25----------------------)

[Samuel Whang](https://medium.com/@klockw3rk?source=post_page-----f2f9542c1f25----------------------)

[May 14, 2019](https://medium.com/@klockw3rk/mounting-vhd-file-on-kali-linux-through-remote-share-f2f9542c1f25?source=post_page-----f2f9542c1f25----------------------) · 3 min read

[![](data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='25' height='25' viewBox='0 0 25 25' data-evernote-id='188' class='js-evernote-checked'%3e%3cpath d='M19 6a2 2 0 0 0-2-2H8a2 2 0 0 0-2 2v14.66h.01c.01.1.05.2.12.28a.5.5 0 0 0 .7.03l5.67-4.12 5.66 4.13a.5.5 0 0 0 .71-.03.5.5 0 0 0 .12-.29H19V6zm-6.84 9.97L7 19.64V6a1 1 0 0 1 1-1h9a1 1 0 0 1 1 1v13.64l-5.16-3.67a.49.49 0 0 0-.68 0z' fill-rule='evenodd' data-evernote-id='189' class='js-evernote-checked'%3e%3c/path%3e%3c/svg%3e)](https://medium.com/m/signin?operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40klockw3rk%2Fmounting-vhd-file-on-kali-linux-through-remote-share-f2f9542c1f25&source=post_actions_header--------------------------bookmark_header-)

Recently I came across an instance where I found a .vhd image on a remote share. Virtual Hard Disk (VHD) files are typically used to backup data stored on a hard-disk partition. As such, data on a .vhd file is very interesting to penetration testers since it may contain valuable information.

There were two problems that I was faced with:

1. I was operating in Kali Linux and I didn’t have an operational Windows system to work with at the time. Generally, it is very easy to mount .vhd files in a Windows environment, but the same can’t be said within a Linux environment since the file type isn’t native to Linux.

2. The .vhd file was in a remote share, and can’t be downloaded in a reasonable time due to the size of the file.

After reading through a bunch of articles and forum posts, I came across a tool called *guestmount* that allowed me to mount .vhd in my Kali Linux environment. If you are running Kali, this tool should be available in the repository. You can get it by running the following:

*apt-get install libguestfs-tools*
After you install that, you should have *guestmount* available.

Before we begin using *guestmount* we still need to solve the issue with the .vhd file being located in a remote share and not easily downloadable. We can solve this problem by using the *mount* command, but we will need *cifs-utils*, so you may need to download that first. You can get this by running this command:

*apt-get install cifs-utils*

Now that we have all the prerequisites completed, we need to mount the remote share. In order to do this, we need to create a mount point on our Kali file system. I decided to create a directory called *“remote”* in the *“/mnt”* directory.

![1*_M8oojAknSnrmwsMXAnIzg.jpeg](../_resources/354a72a1a3a4ed59b27fd511ed3a83e8.jpg)
![1*_M8oojAknSnrmwsMXAnIzg.jpeg](../_resources/3bc531284cab8290b2ee228937b73331.jpg)
Once we navigate to this directory, we are going to mount the remote share.
![1*wJxiTyhK-ZRDTduPlY3gMg.jpeg](../_resources/ba6f3430c7ebe29f19ef506a84c55f9a.jpg)
![1*wJxiTyhK-ZRDTduPlY3gMg.jpeg](../_resources/a799b83ab3f36fb98c5af825195d547d.jpg)

When this operation completes, when you list the mount point, you should see the contents of the share. If you don’t, you have to navigate out to an arbitrary directory and navigate back to the mount point.

![1*onPqncJM6wRvr-SZpp_sSQ.jpeg](../_resources/86c605636992f0e1484c7b68c8e748ea.jpg)
![1*onPqncJM6wRvr-SZpp_sSQ.jpeg](../_resources/0061f72a676b78398778028335576013.jpg)

Now that we solved the remote share problem, we need to now mount the .vhd file within this share. In my particular scenario, I had to navigate deeper into the share until I found the .vhd file.

![1*iIkajUzrJe2da6VlttWabA.jpeg](../_resources/2490d74e2b51a1690009ea217853fa8c.jpg)
![1*iIkajUzrJe2da6VlttWabA.jpeg](../_resources/477ba8ed904a2848e9568a676e6bb89f.jpg)

Before we mount the .vhd file, we need to create another mount point where the contents of .vhd file will reside. I created “vhd” directory within my “/mnt” directory for my mount point.

![1*L-P3jHYIOu9FgKctzuJ5qA.jpeg](../_resources/dfd9033bc84cd3ccd52dc31db64ae5af.jpg)
![1*L-P3jHYIOu9FgKctzuJ5qA.jpeg](../_resources/b16db4f20337887f95c45267198950ff.jpg)
We can now mount the .vhd file using the following command:
guestmount --add /mnt/remote/path/to/vhdfile.vhd --inspector --ro /mnt/vhd -v

The mounting process may take some time. After it completes, navigate to the location where you mounted the .vhd file and the contents should be there!