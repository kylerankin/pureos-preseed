PureOS Preseed Scripts
======================

One challege with laptops that come with Linux pre-installed is how you get the OS back to a factory state, in particular if you try out a different distribution. In particular with Librem laptops, if you want full disk encryption you need to do a reinstall but it would be nice if as much of that as possible could be automated for the end-user. Since PureOS is based largely on Debian Stretch with some additional tweaks, the goal of this project is to set up two phases of preseed scripts with the end result being a rescue disk mode that will let you customize the initial install with your own user settings and perform a reinstall to get you back to a factory image.

There are two phases to the preseed, each with its own preseed file:

Phase 1: bootstrap.cfg
----------------------

This file is designed to be fully automated when loaded from within a default Debian Stretch ISO. If this repository were hosted at the root of http://www.example.com, once the Stretch DVD were booted you would move the cursor down the Install option, hit Tab to edit the boot command line, and then to the left of the --- you would add:

`auto=true url=http://www.example.com/bootstrap.cfg hostname=somehost domain=somedomain`

The bootstrap preseed will wipe out /dev/sda and use the bulk of the disk for a root partition that contains a temporary Debian install. The last 14Gb of the drive will be partitioned for a "rescuedisk" partition that will contain the rest of the preseed files as well as a copy of the install ISO used for the install. 

The preseed script will update Grub with a special PureOS Install menu option that will boot from this rescue disk and kick off the Phase 2 install. After the install finishes, the system will power down and if you want you can add additional DVD ISO files to the rescuedisk partition to reduce the Phase 2 install's reliance on a network connection.

Phase 2: preseed.cfg
--------------------

When the system boots again, the first option will be the PureOS Install option which will kick off a partially-automated wizard that installs and configures PureOS. There is also an "expert" mode that does away with any preseeding you an experienced user can set up full disk encryption, pick a different default desktop environment, or other options.

Once the install completes, the Install PureOS Grub menu option still appears, but is no longer the default. At any point the user could select that boot option and return to a vanilla PureOS install.
