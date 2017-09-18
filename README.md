# multibooty
A utility to allow a single USB stick to boot any number of Linux based ISOs placed on it.



## Installation
There's no installation, just a single script



## Usage
Insert a USB stick into your machine that you are OK with getting wiped
Assume this is /dev/sdx

    # sudo ./multibooty --init /dev/sdx

Now that the stick is initialized, mount the /dev/sdx1 partition and then
add all your .iso files to multibooty/iso/

Run the update to update your grub boot menu.

    # sudo ./multibooty --update /dev/sdx



## How does it work?
multibooty has two modes... init and update

Init will initialize a USB stick, create a partition and install a Grub
boot loader. Init is only needed once to setup the USB stick

Update will scan the USB for an .iso files that have been added into the
multibooty/iso/ folder and then update grub to boot those ISOs. Update
can be run as many times as you wish.



## How is it different from other multi boot USB tools?
multibooty aims to boot the ISOs directly from the ISO, no need to unzip
or unpack the iso or do anything fancy with it (Debian and CentOS are
exceptions!).
It's designed to be easy for people who regularly have to build servers
with multiple distros but don't want to keep on writing USB sticks or
have a huge number of them.



## What is supported?
ISOs from the following Distros/Versions seem to work. Please report any
ammendments, either positive or negative.

Linux Distros

- Arch Linux 2015.11.01 (https://www.archlinux.org/)
- CentOS 6 (https://www.centos.org/)
- CentOS 7 (https://www.centos.org/)
- Debian 8 (https://www.debian.org/)
- Fedora 22 (https://getfedora.org/)
- Fedora 23 (https://getfedora.org/)
- Kali Linux 2.x (https://www.kali.org/)
- Knoppix 7.x (http://www.knopper.net/knoppix/index-en.html)
- Linuxmint 17.x (http://linuxmint.com/)
- Linuxmint 18.x (http://linuxmint.com/)
- OpenSUSE 13.x (https://www.opensuse.org/)
- OpenSUSE Leap 42.x (https://www.opensuse.org/)
- Red Hat Enterprise Linux 7.x (https://www.redhat.com/en)
- Tails 1.x, 2.x, 3.x (https://tails.boum.org/)

Tools
- dban 2.x (http://www.dban.org/)
- gParted 0.x (http://gparted.org/)
- Memtest86 6.x 7.x (http://www.memtest86.com/)
- Network Security Toolkit (NST 22) (www.networksecuritytoolkit.org/)
- SystemRescueCD (www.sysresccd.org/)

Special Cases
- Debian and Ubuntu require further files to work, please read the
  instructions that appear when these ISOs are found, if you do not follow
  these instructions, Debian and Ubuntu can be made to work by following the
  instructions further on in this readme file.

Not Supported/Not working yet
- Redhat/CentOS 5.x
- VMWare ESXi
Windows is not supported, if you can find a way to boot it please let me know 




## Common issues

### Debian/Ubuntu throws error "Your installation CD-ROM couldn't be mounted"
This has occured because you have not downloaded the updated initrd.gz with
iso-scan included from the distro site. Instructions on doing this can be
found by running `multiboot --init`.

As a secondary option, the following can be tried when presented with that
message, but it's not guaranteed.
 
- Select No
- Select Continue, you will be returned to a menu
- ALT+F2
- `mkdir /mnt/multibooty`
- `mount /dev/sdX1 /mnt/multibooty` (Where /dev/sdx1 is your multibooty USB
  partition)
- `mount -o loop /mnt/multibooty/multibooty/iso/ubuntu/ubuntu-15.10-server-amd64.iso /cdrom`
- ALT+F1
- Select Detect and Scan CDROM


### Failed to create partition table

When running with `--init` You receive an error..

    Creating partition table on '/dev/sdb'...
    Error: Partition(s) 2 on /dev/sdb have been written, but we have been
    unable to inform the kernel of the change, probably because it/they are
    in use.  As a result, the old partition(s) will remain in use.  You
    should reboot now before making further changes.
    FAILED
    Failed to create partition table on '/dev/sdb':  : 256 at ./multibooty
    line 402.

This is due to parted being unable to inform the kernel of the new partition table
it has just created.

To resolve, remove the USB disk, re-insert and try again. When running `--init` make
sure no partitions on the USB stick are mounted.


## Can you add support for Distro X?
Yes, I'd like to, if you can come up with a grub menu entry that will
boot the iso you're after, feel free to send it for inclusion



## I've found a problem

If you've found a problem, please report it
Please contact me with the following
- The version of multibooty you are using (multibooty -v)
- Clear instructions on how to replicate your problem
- Full output of the multibooty command you ran
- A copy of the multibooty/boot/grub/grub.cfg file from your USB stick
- The name and md5sum of any isos involved



## Site:
https://github.com/alasdairkeyes/multibooty



## Notes
Try it... go on, why not?



## License
- Released Under GPL Version 3 - See included LICENSE file



## Dependencies
- parted
- mkfs
- mount
- umount
- grub-install
- Perl
- Getopt::Long Perl Module (Should be included with Perl)
- File::Copy Perl Module (Should be included with Perl)
- File::Basename perl Module (Should be included with Perl)


These will all most likely be installed on your Linux System already



## Potential Issues
- Your ISO might not boot, see the I've found a problem section



## Future work
- Add more distros/OSses if possible
  Puppy, Mandrake, Slackware, Xandros, PClinuxos, Turbolinux, Slax, Kubuntu,
  Scientific, DSL, GRML, BSDs, DOS? Windows?
- Allow --init to work with multiple partitions
- Allow --init to install Grub/folder structure without formatting
  the USB stick



## Changelog
- 2015-11-04 :: 0.01    :: First release
- 2015-12-12 :: 0.02    :: Second release - OpenSUSE Leap 42 Support
- 2016-01-09 :: 0.03    :: Third release - Force --target=i386-pc switch with grub



## Thanks
Thanks to existing pages/projects who have helped me with their
documentation
- https://wiki.archlinux.org/index.php/Multiboot_USB_drive
- http://www.pendrivelinux.com/
- https://help.ubuntu.com/community/Grub2/ISOBoot/Examples
- https://help.ubuntu.com/community/Grub2/ISOBoot
- http://git.marmotte.net/git/glim/tree/grub2
- http://forums.justlinux.com/showthread.php?143973-A-grub-menu-booting-100-systems-of-Dos-Windows-Linux-BSD-and-Solaris

## Author
- Alasdair Keyes - https://www.akeyes.co.uk/
