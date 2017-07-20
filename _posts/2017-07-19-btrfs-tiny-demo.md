---
published: false
---


# Here's a quick demo of setting up btrfs filesystems.

This is only my second time trying out some btrfs options. The first setup I used scratch disks, but they have since been commandeered for my NFS/BTRFS media server (WIP).  This time I'll fake the block devices.

```sh
$ dd if=/dev/zero of=/tmp/btrfs-v1 bs=1G count=1
$ dd if=/dev/zero of=/tmp/btrfs-v2 bs=1G count=1
$ dd if=/dev/zero of=/tmp/btrfs-v3 bs=1G count=1
```

### See the man page for ``losetup`` to set up loop devices
```sh
$ sudo losetup /dev/loop1 /tmp/btrfs-v1
$ sudo losetup /dev/loop2 /tmp/btrfs-v2
$ sudo losetup /dev/loop3 /tmp/btrfs-v3
```

### Make the filesystem using 2 devices

```sh
$ sudo mkfs.btrfs -f /dev/loop1 /dev/loop2
btrfs-progs v4.11.1
See http://btrfs.wiki.kernel.org for more information.

Performing full device TRIM /dev/loop1 (1.00GiB) ...
Performing full device TRIM /dev/loop2 (1.00GiB) ...
Label:              (null)
UUID:               814cbf18-5475-4e5a-9db5-52544051dfe7
Node size:          16384
Sector size:        4096
Filesystem size:    2.00GiB
Block group profiles:
  Data:             RAID0           204.75MiB
  Metadata:         RAID1           102.38MiB
  System:           RAID1             8.00MiB
SSD detected:       no
Incompat features:  extref, skinny-metadata
Number of devices:  2
Devices:
   ID        SIZE  PATH
    1     1.00GiB  /dev/loop1
    2     1.00GiB  /dev/loop2
```

```sh
$ sudo btrfs filesystem show /dev/loop1
Label: none  uuid: 814cbf18-5475-4e5a-9db5-52544051dfe7
	Total devices 2 FS bytes used 112.00KiB
	devid    1 size 1.00GiB used 212.75MiB path /dev/loop1
	devid    2 size 1.00GiB used 212.75MiB path /dev/loop2
```

### Mount the filesystem, only need to give one dev name
```sh
$ sudo mount /dev/loop1 /mnt
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            1.9G     0  1.9G   0% /dev
tmpfs           388M  6.2M  382M   2% /run
/dev/sda5        42G   28G   13G  70% /
tmpfs           1.9G   63M  1.9G   4% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
cgmfs           100K     0  100K   0% /run/cgmanager/fs
tmpfs           388M   56K  388M   1% /run/user/1000
/dev/loop1      2.0G   17M  1.8G   1% /mnt

$ btrfs filesystem df /mnt
Data, RAID0: total=204.75MiB, used=128.00KiB
System, RAID1: total=8.00MiB, used=16.00KiB
Metadata, RAID1: total=102.38MiB, used=112.00KiB
GlobalReserve, single: total=16.00MiB, used=0.00B
```
### Let's make another btrfs on loop3
```sh
$ sudo mkfs.btrfs -f /dev/loop3 
btrfs-progs v4.11.1
See http://btrfs.wiki.kernel.org for more information.

Performing full device TRIM /dev/loop3 (1.00GiB) ...
Label:              (null)
UUID:               ea980160-40ed-4642-9f9b-ccadfded8ef0
Node size:          16384
Sector size:        4096
Filesystem size:    1.00GiB
Block group profiles:
  Data:             single            8.00MiB
  Metadata:         DUP              51.19MiB
  System:           DUP               8.00MiB
SSD detected:       no
Incompat features:  extref, skinny-metadata
Number of devices:  1
Devices:
   ID        SIZE  PATH
    1     1.00GiB  /dev/loop3
```
### Add loop3 as a new device to /mnt
```sh
$ sudo btrfs device add -f /dev/loop3 /mnt
Performing full device TRIM /dev/loop3 (1.00GiB) ...

$ sudo btrfs filesystem show 

Label: none  uuid: 814cbf18-5475-4e5a-9db5-52544051dfe7
	Total devices 3 FS bytes used 256.00KiB
	devid    1 size 1.00GiB used 212.75MiB path /dev/loop1
	devid    2 size 1.00GiB used 212.75MiB path /dev/loop2
	devid    3 size 1.00GiB used 0.00B path /dev/loop3
```
### Rebalance after adding the new device
```sh
$ sudo btrfs filesystem balance /mnt

WARNING:

	Full balance without filters requested. This operation is very
	intense and takes potentially very long. It is recommended to
	use the balance filters to narrow down the scope of balance.
	Use 'btrfs balance start --full-balance' option to skip this
	warning. The operation will start in 10 seconds.
	Use Ctrl-C to stop it.
10 9 8 7 6 5 4 3 2 1
Starting balance without any filters.
Done, had to relocate 3 out of 3 chunks

$ sudo btrfs filesystem show 

Label: none  uuid: 814cbf18-5475-4e5a-9db5-52544051dfe7
	Total devices 3 FS bytes used 256.00KiB
	devid    1 size 1.00GiB used 144.00MiB path /dev/loop1
	devid    2 size 1.00GiB used 368.00MiB path /dev/loop2
	devid    3 size 1.00GiB used 400.00MiB path /dev/loop3
```

### Lets delete a device *nicely*
```sh
$ sudo btrfs device delete /dev/loop1 /mnt

$ sudo btrfs filesystem show 
sudo: unable to resolve host d830
Label: none  uuid: 814cbf18-5475-4e5a-9db5-52544051dfe7
	Total devices 2 FS bytes used 256.00KiB
	devid    2 size 1.00GiB used 400.00MiB path /dev/loop2
	devid    3 size 1.00GiB used 400.00MiB path /dev/loop3
```
    
### End of simplest stuff.  Next up - RAID and not so nice disk errors.
