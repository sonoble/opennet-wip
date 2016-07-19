---
date: 2016-07-19T00:11:02+01:00
title: Getting Started With The Wedge
weight: 10
---
# Facebook Wedge Support

- ONL Supports the [[Wedge 40](http://www.edge-core.com/productsInfo.php?cls=1&cls2=7&cls3=57&id=110)] running the Facebook [[FBOSS](https://github.com/facebook/fboss)] fowarding agent and [[Snaproute](http://www.snaproute.com/)].
- ONL Supports the Wedge 100 running [[Snaproute](http://www.snaproute.com/)].

## Installation Instructions

1. If your Wedge does not have ONIE, you will need to install it to install ONL, it can be download from here:
    1. Wedge 40 - [ONIE for Wedge 40](http://opennetlinux.org/binaries/accton-wedge/onie-recovery-wedge.iso)
    2. Wedge 100 - [ONIE for Wedge 100](http://opennetlinux.org/binaries/accton-wedge/onie-recovery-x86-64-facebook-wedge100-r0.iso)

2. Burn the ONIE .iso to a USB
3. Assuming that your USB is /dev/sdb, under linux you would do: dd if=onie-recovery-x86_64-accton_wedge_x16-r0.iso of=/dev/sdb bs=10M
4. Follow the directions in [Wedge Installation Guide](https://github.com/opencomputeproject/OpenNetworkLinux/blob/master/docs/GettingStartedWedge.md)

## Video Tutorials

1. Installing ONIE   
[Installing ONIE on the Wedge](http://vimeo.com/146086436). 
2. Installing ONL   
[Installing ONL on the Wedge](http://vimeo.com/146086486). 
3. Running FBOSS   
[Running FBOSS on the Wedge](http://vimeo.com/146086434).
