---
layout: post
title: "Secureboot on Fedora"
date: 2015-12-19T01:43:52-08:00
categories: [fedora,secureboot]
---

The following describes a setup for Secureboot when using the fedora_shim, and grub2
boot loader.  This combination requires that each kernel be signed with a certificate.
There are alternatives like the Linux Foundation PreLoader which doesn't require
kernels to be signed, instead it just needs to be given a hash for each authorized
boot kernel.  There are other setups where grub2 wouldn't be needed either, such as
when using the [rEFInd boot manager](http://www.rodsbooks.com/refind/).

To sign kernels that can be booted under UEFI Secureboot you first need a certificate.  
This certificate is registered with the UEFI, and is also used by the pesign utility
to bless each kernel image.

First create this certificate:

    # efikeygen \
        --self-sign \
        --nickname="Loops Local Certificate" \
        --common-name="CN=Loops of Eztux certificate for local signing" \
        --url="http://eztux.com/about"

The certificate is placed in the /etc/pki/pesign datastore.  This means that
the above (and following) commands need to be run as root.  You can then
extract this certificate into a separate file used when registering it with UEFI.

    # cd ~
    # certutil -d /etc/pki/pesign/ -L -n "Loops Local Certificate" -r > ./loops.cer

Make sure to back this "loops.cer" certificate since it will be used for all
locally authorized kernels (including those from other distros if you want to
boot them).

Now register this certificate with UEFI:

    # mokutil --import ./loops.cer

You will have to assign a password which will be used next time you reboot the computer
to authorize this certificate being imported into UEFI.  Once this has been done,
any kernel that you boot with the fedora_shim will work in secure mode, as long as it
has been signed with this certificate:

    # pesign -c 'Loops Local Certificate' -i bzImage -o vmlinuz.signed --sign

Note that the external loops.cer file is no longer needed and should be removed from
the filesystem.  The copy held in the pki store will be used by the pesign utility.

Move the vmlinuz.signed output file into /boot to be accessed by grub on startup.

