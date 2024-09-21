# prepare disk

root@utms01:/home/utms# qemu-img create -f qcow2 /data/lfs12.2.qcow2 64G

root@utms01:/home/utms# /sbin/modprobe nbd

root@utms01:/home/utms# qemu-nbd --connect=/dev/nbd0 /data/lfs12.2.qcow2

root@utms01:/home/utms# /usr/sbin/fdisk /dev/nbd0

Command (m for help): g

Created a new GPT disklabel (GUID: 074FB352-16BA-3642-8CA8-D6D347FADF5E).

Command (m for help): n

Partition number (1-128, default 1): 

First sector (2048-134217694, default 2048): 

Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-134217694, default 134215679): 1024k

Created a new partition 1 of type 'Linux filesystem' and of size 511 MiB.

Command (m for help): n

Partition number (2-128, default 2): 

First sector (1048577-134217694, default 1050624): 

Last sector, +/-sectors or +/-size{K,M,G,T,P} (1050624-134217694, default 134215679): 1024M

Value out of range.

Last sector, +/-sectors or +/-size{K,M,G,T,P} (1050624-134217694, default 134215679): +1024M

Created a new partition 2 of type 'Linux filesystem' and of size 1 GiB.

Command (m for help): n

Partition number (3-128, default 3): 

First sector (1048577-134217694, default 3147776): 

Last sector, +/-sectors or +/-size{K,M,G,T,P} (3147776-134217694, default 134215679): +8192M

Created a new partition 3 of type 'Linux filesystem' and of size 8 GiB.

Command (m for help): n

Partition number (4-128, default 4): 

First sector (1048577-134217694, default 19924992): 

Last sector, +/-sectors or +/-size{K,M,G,T,P} (19924992-134217694, default 134215679):

Created a new partition 4 of type 'Linux filesystem' and of size 54.5 GiB.

<br/>
Command (m for help): t

Partition number (1-4, default 4): 1

Partition type or alias (type L to list all): 4

Command (m for help): t

Partition number (1-4, default 4): 3

Partition type or alias (type L to list all): swap

Changed type of partition 'Linux filesystem' to 'Linux swap'.

<br/>
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
 
<br/>
root@utms01:/home/utms# /sbin/mkfs.ext4 /dev/nbd0p4

mke2fs 1.47.0 (5-Feb-2023)

Discarding device blocks: done    

Creating filesystem with 14286336 4k blocks and 3571712 inodes

Filesystem UUID: 8a1832f5-496e-4539-bb0d-da72e882955b

Superblock backups stored on blocks: 

    32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
    
    4096000, 7962624, 11239424
    
Allocating group tables: done                            

Writing inode tables: done

Creating journal (65536 blocks): done

Writing superblocks and filesystem accounting information: done  
 
<br/>
root@utms01:~# mount /dev/nbd0p4 /mnt/lfs/


# install build tool packages
root@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs# apt install sudo

root@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs# apt install ament-cmake-xmllint/stable

utms@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs$ sudo apt install xsltproc

utms@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs$ sudo apt install docbook-xml

utms@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs$ sudo apt install docbook-xsl

utms@utms01:/mnt/lfs/alfs/lfs12.2/jhalfs$ sudo mkdir /mnt/lfs/build_dir
 
<br/>
# alfs build
root@utms01:/mnt/lfs/alfs/lfs12.2# git clone https://git.linuxfromscratch.org/jhalfs.git jhalfs

Cloning into 'jhalfs'...

remote: Enumerating objects: 11251, done.

remote: Counting objects: 100% (11251/11251), done.

remote: Compressing objects: 100% (4003/4003), done.

remote: Total 11251 (delta 7802), reused 10407 (delta 7201), pack-reused 0

Receiving objects: 100% (11251/11251), 1.99 MiB | 896.00 KiB/s, done.

Resolving deltas: 100% (7802/7802), done.

<br/>
root@utms01:/mnt/lfs/alfs/lfs12.2# git clone --progress --branch 12.2 -v "https://git.linuxfromscratch.org/lfs.git"

root@utms01:/mnt/lfs/alfs/lfs12.2# mv lfs lfs-book

utms@utms01:/mnt/lfs/alfs/lfs12.2$ git clone https://git.linuxfromscratch.org/blfs.git -b 12.2 blfs-book
 
<br/>

# build config
   Use BOOK (Linux From Scratch systemd)  --->
    
   Book version (Working Copy)  --->
   
(/mnt/lfs/alfs/lfs12.2/lfs-book) Loc of working copy (mandatory)

   Multilib (Standard LFS on i686 or amd64)  --->
   
   Build method (chroot)  --->
   
[*] Add blfs-tool support

   blfs-tool dependencies  --->
   
   BLFS book version (BLFS working copy)  --->
   
(/mnt/lfs/alfs/lfs12.2/blfs-book) Location of the local BLFS working copy (mandatory)

(/blfs_root) Root of the tools directory (see help) (NEW)

(blfs-xml) BLFS sources directory (internal parameter) (NEW)

(lfs-xml) LFS sources directory (internal parameter) (NEW)

[ ] Add custom tools support

(/var/lib/jhalfs/BLFS) Installed packages database directory (NEW)

<br/>
(/mnt/lfs/build_dir) Build Directory

[*] Retrieve source files

($SRC_ARCHIVE) Package Archive Directory

[*]     Retry on 'connection refused' failure

(20)    Number of retry attempts on download failures

(30)    Download timeout (in seconds)

[*] Run the makefile

[ ] Rebuild files

<br/>
Parallelism settings  --->

[ ] Run testsuites

[ ] Package management (NEW)

[*] Create a log of installed files for each package

[*] Strip Installed Binaries/Libraries

[ ] DO NOT use/display progress_bar (NEW)


[ ] Use a custom fstab file

[ ] Build the kernel

[ ] Install non-wide-character ncurses

(asia/shanghai) TimeZone

(en) Language

[ ] Install the full set of locales

   Groff page size (A4)  --->
   
(lfs12.2) Hostname (see help)

   Network configuration  --->
   
   Console configuration  --->
   
<br/>
(eth0) netword card name (NEW)

(192.168.42.13) Static IP address

(192.168.42.254) Gateway

(24) Subnet prefix (NEW)

(192.168.42.255) Broadcast address

(local) Domain name (see help) (NEW)

(192.168.0.127) Primary Name server

(8.8.8.8) Secondary Name server (NEW)
 

# build system
 
 Building target 501-binutils-pass1
 
[++++++++++++++++++++++++++++++++++++++++++++-make: *** [Makefile:263: 501-binutils-pass1] Error 2

make: Leaving directory '/mnt/lfs/build_dir/jhalfs'

make[1]: *** [Makefile:102: mk_LUSER] Error 2

make[1]: Leaving directory '/mnt/lfs/build_dir/jhalfs'

ERROR:  Error 2 at common/common-functions line 38!

<jhalfs> exit

make: *** [Makefile:11: all] Error 2
 
<br/>

modify tools direcory to utms or build system use root user

 
# build kernel
enter into chroot，build kernel

sudo chroot "/mnt/lfs" /usr/bin/env -i HOME=/root TERM="$TERM" PS1='(lfs chroot) \u:\w\$ ' PATH=/usr/bin:/usr/sbin      MAKEFLAGS="-j$(nproc)" TESTSUITEFLAGS="-j$(nproc)" /bin/bash --login

(lfs chroot) root:/sources# tar -xf linux-6.10.5.tar.xz

(lfs chroot) root:/sources# cd linux-6.10.5

(lfs chroot) root:/sources/linux-6.10.5# make mrproper

(lfs chroot) root:/sources/linux-6.10.5# make menuconfig

(lfs chroot) root:/sources/linux-6.10.5# make

(lfs chroot) root:/sources/linux-6.10.5# make modules_install
 
<br/>
# boot partion
utms@utms01:/mnt/lfs$ sudo /sbin/mkfs.ext2 /dev/nbd0p1

Creating filesystem with 523264 1k blocks and 130560 inodes

Filesystem UUID: 72b28462-c1bd-46ca-b4e8-a50abd07b5b1

Superblock backups stored on blocks:

        8193, 24577, 40961, 57345, 73729, 204801, 221185, 401409

Allocating group tables: done

Writing inode tables: done

Writing superblocks and filesystem accounting information: done
 
<br/>
utms@utms01:/data$ sudo mkfs.ext4 /dev/nbd0p2

utms@utms01:/mnt/lfs$ sudo mount /dev/nbd0p2 /mnt/lfs/boot/
 
<br/>
(lfs chroot) root:/sources/linux-6.10.5# cp -iv arch/x86_64/boot/bzImage /boot/vmlinuz-6.10.5-lfs-12.2-systemd

(lfs chroot) root:/sources/linux-6.10.5# cp -iv System.map /boot/System.map-6.10.5
 
<br/>
(lfs chroot) root:/sources/linux-6.10.5# install -v -m755 -d /etc/modprobe.d

> /etc/modprobe.d/usb.conf << "EOF"
> 
#Begin /etc/modprobe.d/usb.conf

install install: creating directory '/etc/modprobe.d'

ohci_hcd /sbin/(lfs chroot) root:/sources/linux-6.10.5# cat > /etc/modprobe.d/usb.conf << "EOF"

> #Begin /etc/modprobe.d/usb.conf

in/modpr>

> install ohci_hcd /sbin/modprobe ehci_hcd ; /sbin/modprobe -i ohci_hcd ; true

> install uhci_hcd /sbin/modprobe ehci_hcd ; /sbin/modprobe -i uhci_hcd ; true
>
> #End /etc/modprobe.d/usb.conf
> EOF
 

# install grub
utms@utms01:/mnt/lfs$ sudo mount -v --bind /dev $LFS/dev

[sudo] password for utms:

mount: /dev bound on /mnt/lfs/dev.

<br/> 

utms@utms01:~$ sudo qemu-nbd --disconnect /dev/nbd0

(lfs chroot) root:/# grub-install /dev/nbd0

Installing for i386-pc platform.

grub-install: error: cannot find a GRUB drive for /dev/nbd0p2.  Check your device.map.

grub-install: info: cannot open `/boot/grub/device.map': No such file or directory.
 
<br/>
mount virtual file system

for d in dev sys proc; do sudo mount --bind /$d /mnt/lfs/$d; done
 
cat > /boot/grub/grub.cfg << "EOF"

#Begin /boot/grub/grub.cfg

set default=0

set timeout=5
<br/>
insmod part_gpt

insmod ext2

set root=(hd0,2)

<br/>
menuentry "GNU/Linux, Linux 6.10.5-lfs-12.2-systemd" {

        linux   /boot/vmlinuz-6.10.5-lfs-12.2-systemd root=/dev/sda2 ro
        
}

EOF
 
<br/>
# create password for root
passwd

# create swap
-bash-5.2# mkswap /dev/sda3

# reboot and login to the new system
