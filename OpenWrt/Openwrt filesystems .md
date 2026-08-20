## Filesystems

https://openwrt.org/docs/techref/filesystems

## Flash Layout

https://openwrt.org/docs/techref/flash.layout

## Example Log

```
root@device:/# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00080000 00020000 "bootloader"
mtd1: 02b40000 00020000 "kernel"
mtd2: 02800000 00020000 "ubi"
mtd3: 03500000 00020000 "tclinux"
mtd4: 02b40000 00020000 "kernel_slave"
mtd5: 02800000 00020000 "ubi_slave"
mtd6: 03500000 00020000 "tclinux_slave"
mtd7: 00020000 00020000 "u-boot-env"
mtd8: 00020000 00020000 "cert"
mtd9: 00300000 00020000 "configs"
mtd10: 00180000 00020000 "art"
mtd11: 00f80000 0001f000 "rootfs"
mtd12: 0101b000 0001f000 "rootfs_data"
root@device:/#
root@device:/# cat /proc/partitions
major minor  #blocks  name

   1        0       4096 ram0
   1        1       4096 ram1
   1        2       4096 ram2
   1        3       4096 ram3
   1        4       4096 ram4
   1        5       4096 ram5
   1        6       4096 ram6
   1        7       4096 ram7
   1        8       4096 ram8
   1        9       4096 ram9
   1       10       4096 ram10
   1       11       4096 ram11
   1       12       4096 ram12
   1       13       4096 ram13
   1       14       4096 ram14
   1       15       4096 ram15
  31        0        512 mtdblock0
  31        1      44288 mtdblock1
  31        2      40960 mtdblock2
  31        3      54272 mtdblock3
  31        4      44288 mtdblock4
  31        5      40960 mtdblock5
  31        6      54272 mtdblock6
  31        7        128 mtdblock7
  31        8        128 mtdblock8
  31        9       3072 mtdblock9
  31       10       1536 mtdblock10
  31       11      15872 mtdblock11
  31       12      16492 mtdblock12
254        0      15872 ubiblock0_0
root@device:/#
root@device:/# cat /proc/filesystems
nodev   sysfs
nodev   tmpfs
nodev   bdev
nodev   proc
nodev   cgroup
nodev   cgroup2
nodev   cpuset
nodev   binfmt_misc
nodev   debugfs
nodev   securityfs
nodev   sockfs
nodev   pipefs
nodev   ramfs
nodev   devpts
        ext3
        ext4
        ext2
        squashfs
        vfat
nodev   jffs2
nodev   overlay
nodev   mqueue
nodev   ubifs
        fuseblk
nodev   fuse
nodev   fusectl
root@device:/#
root@device:/# df -T
Filesystem           Type       1K-blocks      Used Available Use% Mounted on
/dev/root            squashfs       15872     15872         0 100% /rom
tmpfs                tmpfs         119392       608    118784   1% /tmp
/dev/ubi0_1          ubifs          13416        92     12604   1% /overlay
overlayfs:/overlay   overlay        13416        92     12604   1% /
tmpfs                tmpfs            512         0       512   0% /dev
root@device:/#
root@device:/# mount
/dev/root on /rom type squashfs (ro,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
cgroup2 on /sys/fs/cgroup type cgroup2 (rw,nosuid,nodev,noexec,relatime,nsdelegate)
tmpfs on /tmp type tmpfs (rw,nosuid,nodev,noatime)
/dev/ubi0_1 on /overlay type ubifs (rw,noatime,assert=read-only,ubi=0,vol=1)
overlayfs:/overlay on / type overlay (rw,noatime,lowerdir=/,upperdir=/overlay/upper,workdir=/overlay/work)
tmpfs on /dev type tmpfs (rw,nosuid,noexec,noatime,size=512k,mode=755)
devpts on /dev/pts type devpts (rw,nosuid,noexec,noatime,mode=600,ptmxmode=000)
debugfs on /sys/kernel/debug type debugfs (rw,noatime)
root@device:/#
root@device:/# ubinfo -a
UBI version:                    1
Count of UBI devices:           1
UBI control device major/minor: 10:62
Present UBI devices:            ubi0

ubi0
Volumes count:                           2
Logical eraseblock size:                 126976 bytes, 124.0 KiB
Total amount of logical eraseblocks:     320 (40632320 bytes, 38.7 MiB)
Amount of available logical eraseblocks: 35 (4444160 bytes, 4.2 MiB)
Maximum count of volumes                 128
Count of bad physical eraseblocks:       0
Count of reserved physical eraseblocks:  18
Current maximum erase counter value:     2
Minimum input/output unit size:          2048 bytes
Character device major/minor:            251:0
Present volumes:                         0, 1

Volume ID:   0 (on ubi0)
Type:        dynamic
Alignment:   1
Size:        128 LEBs (16252928 bytes, 15.5 MiB)
State:       OK
Name:        rootfs
Character device major/minor: 251:1
-----------------------------------
Volume ID:   1 (on ubi0)
Type:        dynamic
Alignment:   1
Size:        133 LEBs (16887808 bytes, 16.1 MiB)
State:       OK
Name:        rootfs_data
Character device major/minor: 251:2
root@device:/#
root@device:/# dmesg | grep -i -e mount -e ubi -e root
[    5.180618] ubi0: default fastmap pool size: 15
[    5.184992] ubi0: default fastmap WL pool size: 7
[    5.189659] ubi0: attaching mtd2
[    5.371222] ubi0: scanning is finished
[    5.382111] ubi0: attached mtd2 (name "ubi", size 40 MiB)
[    5.387361] ubi0: PEB size: 131072 bytes (128 KiB), LEB size: 126976 bytes
[    5.394199] ubi0: min./max. I/O unit sizes: 2048/2048, sub-page size 2048
[    5.400962] ubi0: VID header offset: 2048 (aligned 2048), data offset: 4096
[    5.407911] ubi0: good PEBs: 320, bad PEBs: 0, corrupted PEBs: 0
[    5.413900] ubi0: user volume: 2, internal volumes: 1, max. volumes count: 128
[    5.421102] ubi0: max/mean erase counter: 2/0, WL threshold: 4096, image sequence number: 692835193
[    5.430133] ubi0: available PEBs: 35, total reserved PEBs: 285, PEBs reserved for bad PEB handling: 18
[    5.439441] ubi0: background thread "ubi_bgt0d" started, PID 61
[    5.445896] block ubiblock0_0: created from ubi0:0(rootfs)
[    5.472902] VFS: Mounted root (squashfs filesystem) readonly on device 254:0.
[   13.331706] mount_root: loading kmods from internal overlay
[   13.517426] UBIFS (ubi0:1): Mounting in unauthenticated mode
[   13.523076] UBIFS (ubi0:1): background thread "ubifs_bgt0_1" started, PID 138
[   13.558299] UBIFS (ubi0:1): recovery needed
[   13.691165] UBIFS (ubi0:1): recovery completed
[   13.695578] UBIFS (ubi0:1): UBIFS: mounted UBI device 0, volume 1, name "rootfs_data"
[   13.703255] UBIFS (ubi0:1): LEB size: 126976 bytes (124 KiB), min./max. I/O unit sizes: 2048 bytes/2048 bytes
[   13.713150] UBIFS (ubi0:1): FS size: 15618048 bytes (14 MiB, 123 LEBs), journal size 1015809 bytes (0 MiB, 6 LEBs)
[   13.723481] UBIFS (ubi0:1): reserved for root: 737678 bytes (720 KiB)
[   13.729898] UBIFS (ubi0:1): media format: w5/r0 (latest is w5/r0), UUID D7BF8195-1311-479B-A533-599F4A6EEF37, small LPT model
[   13.742550] block: attempting to load /tmp/ubifs_cfg/upper/etc/config/fstab
[   13.753780] block: extroot: not configured
[   13.757877] UBIFS (ubi0:1): un-mount UBI device 0
[   13.762423] UBIFS (ubi0:1): background thread "ubifs_bgt0_1" stops
[   13.773236] UBIFS (ubi0:1): Mounting in unauthenticated mode
[   13.796190] UBIFS (ubi0:1): background thread "ubifs_bgt0_1" started, PID 139
[   13.864307] UBIFS (ubi0:1): UBIFS: mounted UBI device 0, volume 1, name "rootfs_data"
[   13.871963] UBIFS (ubi0:1): LEB size: 126976 bytes (124 KiB), min./max. I/O unit sizes: 2048 bytes/2048 bytes
[   13.881951] UBIFS (ubi0:1): FS size: 15618048 bytes (14 MiB, 123 LEBs), journal size 1015809 bytes (0 MiB, 6 LEBs)
[   13.892218] UBIFS (ubi0:1): reserved for root: 737678 bytes (720 KiB)
[   13.898621] UBIFS (ubi0:1): media format: w5/r0 (latest is w5/r0), UUID D7BF8195-1311-479B-A533-599F4A6EEF37, small LPT model
[   13.950739] ubi0 error: ubi_open_volume: cannot open device 0, volume 1, error -16
[   14.033469] block: attempting to load /tmp/ubifs_cfg/upper/etc/config/fstab
[   14.044187] block: extroot: not configured
[   14.049838] mount_root: switching to ubifs overlay
[   20.088120] ubi0 error: ubi_open_volume: cannot open device 0, volume 1, error -16
root@device:/#
```
