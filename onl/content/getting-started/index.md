---
date: 2016-07-19T00:11:02+01:00
title: Open Network Linux - Getting started
weight: 10
---
Getting Started
------------------------------------------------
To install and run ONL you need is an ONL Compatible switch (see
http://opennetlinux.org/hcl) and the ONL installer binary.  Every
ONL compatible switch ships with the ONIE installer environment installed
which gives you a multitude of ways of getting ONL installed on your switch.

We document the easiest ways here (manual install via console and NFS)
but the http://onie.org website contains a variety of installation
methods including via USB, over the network, and even via ssh.

The resulting installation has a default account ("root") with a default
password ("onl").  The network interface is disabled by default so that
you can change the root password before the system comes up.


ONL Manual Install
------------------------------------------------
1) Attach a serial terminal to the switch
2) Boot switch and hit return to go to ONIE''s interactive mode
    2a) You must wait until after uboot has finished loading; if you
        accidentally interupt uboot first, just run `boot` to continue
        booting into ONIE
3) Download the ONL installer from http://opennetlinux.org and run it by hand

Expected Serial Console Output (from an QuantaMesh LB9, other switches ouput wil
l vary):

        U-Boot 2010.12 (Oct 08 2013 - 17:11:37)

        CPU:   8541, Version: 1.1, (0x80720011)
        Core:  Unknown, Version: 2.0, (0x80200020)
        Clock Configuration:
               CPU0:825  MHz,
               CCB:330  MHz,
               DDR:165  MHz (330 MT/s data rate), LBC:41.250 MHz
        CPM:   330 MHz
        L1:    D-cache 32 kB enabled
               I-cache 32 kB enabled
        I2C:   ready
        DRAM:  Detected UDIMM TS128MSD64V3A
        Detected UDIMM(s)
        DDR: 1 GiB (DDR1, 64-bit, CL=2.5, ECC off)
        FLASH: 64 MiB
        L2:    256 KB enabled

        LB9 U-Boot
          Product Name          : LB9
          Model Name            : QUANTA LB9
          Serial Number         : QTFCA63280001
          Part Number           : 1LB9BZZ0STQ
          Label Revision Number : 1
          Hardware Version      : 1.0
          Platform Version      : 0xb901
          Release Date          : 2013/7/5
          MAC Address           : 08:9e:01:ce:bd:2d
        Set ethaddr MAC address = 08:9e:01:ce:bd:2d
        In:    serial
        Out:   serial
        Err:   serial
        Net:   TSEC0: PHY is Broadcom BCM5461S (2060c1)
        TSEC0
        IDE:   Bus 0: OK
          Device 0: Model: 4GB CompactFlash Card Firm: Ver6.04J Ser#: CDE207331D0100001484
                    Type: Hard Disk
                    Capacity: 3811.9 MB = 3.7 GB (7806960 x 512)
        Hit any key to stop autoboot:  0
        ## Error: "nos_bootcmd" not defined
        Loading Open Network Install Environment ...
        Platform: powerpc-quanta_lb9-r0
        Version : 1.5.2-20131008154633
        WARNING: adjusting available memory to 30000000
        ## Booting kernel from Legacy Image at 04000000 ...
           Image Name:   quanta_lb9-r0
           Image Type:   PowerPC Linux Multi-File Image (gzip compressed)
           Data Size:    3479390 Bytes = 3.3 MiB
           Load Address: 00000000
           Entry Point:  00000000
           Contents:
              Image 0: 2762740 Bytes = 2.6 MiB
              Image 1: 707380 Bytes = 690.8 KiB
              Image 2: 9254 Bytes = 9 KiB
           Verifying Checksum ... OK
        ## Loading init Ramdisk from multi component Legacy Image at 04000000 ...
        ## Flattened Device Tree from multi component Image at 04000000
           Booting using the fdt at 0x434f378
           Uncompressing Multi-File Image ... OK
           Loading Ramdisk to 2ff53000, end 2ffffb34 ... OK
           Loading Device Tree to 03ffa000, end 03fff425 ... OK
        Cannot reserve gpages without hugetlb enabled
        setup_arch: bootmem
        quanta_lb9_setup_arch()
        arch: exit

        ONIE: Using DHCPv4 addr: eth0: 10.7.1.10 / 255.254.0.0
        discover: installer mode detected.  Running installer.

        Please press Enter to activate this console. ONIE: Using DHCPv4 addr: eth0: 10.7.1.10 / 255.254.0.0
        ONIE: Starting ONIE Service Discovery

        To check the install status inspect /var/log/onie.log.
        Try this:  tail -f /var/log/onie.log

Now press RETURN here to jump into ONIE''s manual installer mode.  You should se e:

        ** Installer Mode Enabled **

        ONIE:/ #

Then simply download the latest ONL installer for the appropriate architecture (powerpc or amd64) from the website and run it.

        ONIE:/ # install_url http://opennetlinux.org/binaries/latest-$ARCH.installer

        Connecting to opennetlinux.org (107.170.237.53:80)
        Open Network Installer running under ONIE.
        Installer Version: Open Network Linux e148b7a (powerpc.all,2014.05.21.18.57,e148b7a90131c07eb8d49f74316baf8f2aae92c6)
        Detected platform: powerpc-quanta-lb9-r0
        Installing in standalone mode.
        Unpacking Open Network Linux installer files...
        onl.powerpc-as4600-54t.loader
        onl.powerpc-as5600-52x.loader
        ...


Note:

1) If there is different OS(other than ONL) running on the switch.
Then halt the booting process at U-boot mode, Then check for the ONIE
details in the environment(=> printenv). Open the ONIE in rescue mode,
while ONIE has many different installation modes,
we recommend the rescue mode for doing a manual (read: via console)
because it disables the automatic ONIE server discovery.
Then run (=> run onie_rescue) command to take you to the ONIE environment.

2) For development purpose, to load freshly build ONL installer from directly ON
IE.
Run a http server from the build machine (example:python -m SimpleHTTPServer 800
0) and access it as,


    example: ONIE:/ # install_url http://buildmachineIPAddress:/path/to/directory/onl-09b7bba-powerpc-all.2016.02.05.05.17.installer # update for specific file/date/build

Also, you can use install via scp with two steps,

       example: ONIE:/ # scp [username]@buildmachineIPAddress:/path/to/directory/onl-09b7bba-powerpc-all.2016.02.05.05.17.installer  ONL.installer # update for specific file/date/build
                ONIE:/ # sh ONL.installer

ONL rc.boot and persistant storage
------------------------------------------------

Given that the default installation of ONL does not persist files across
reboots (this is intentional -- flash disks should not be written to
as often as spinning disks), it is sometimes useful to have a normally
writable, larger disk available for the switch. For this ONL has the concept
of the /mnt/onl/data directory and the ability to use rc.boot scripts.

To persist and setup programs:

1) Run the ONL installer normally (e.g., via the manual mode per above) so that the ONL loader is installed.

2) Edit /mnt/onl/data/rc.boot and insert the necessary commands

     # cat /mnt/onl/data/rc.boot
     #!/bin/sh
     dpkg -i /mnt/onl/data/package.deb

3) reboot and confirm that commands are executing correctly:
