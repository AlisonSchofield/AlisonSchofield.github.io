---
layout: post
title: build btrfs
published: true
---
In order to explore btrfs file system, we need a kernel with ```btrfs``` support and the utilities ```btrfs-progs```

### Build a btrfs kernel

Here I'll build a 'near' latest kernel with ```CONFIG_BTRFS_FS``` and I'll build the latest btrfs-progs from github.

I'm taking a bit of a cheat here by not grabbing kdave's btrfs branch. I'm on a borrowed network and don't want to clone a new git tree. 

Do you already have btrfs in your kernel?

```sh
$ cat /proc/filesystems | grep btrfs
```

Rebuild kernel with `CONFIG_BTRFS_FS` selected.  There are some additional BTRFS config options that we'll just pass on for this initial build.  (It's tristate, you can build it as a loadable module also.)

### Build btrfs-progs

Here I grabbed most of what was recommended and even after that, had to go grab some more as things failed. Easy part was that when something fails it tells you exactly what was missing. Your mileage will vary on this process, based on what packages you already have installed. Here's what worked for me:

```sh
$ sudo apt-get install asciidoc xmlto --no-install-recommends
$ sudo apt-get install uuid-dev libattr1-dev zlib1g-dev libacl1-dev e2fslibs-dev libblkid-dev liblzo2-dev
$ sudo apt-get install autoconf
```

Note that we are not using the distro's package manager to get btrfs-progs. We grab the latest and greatest from kdave's git repository:
```sh
$ git clone git://git.kernel.org/pub/scm/linux/kernel/git/kdave/btrfs-progs.git
```
Got it!  Now let's build it.
```sh
$ cd btrfs-progs
btrfs-progs$ ./autogen.sh
btrfs-progs$ ./configure
btrfs-progs$ make
btrfs-progs$ sudo make install
```

And, let's check that we have it...
```sh
btrfs-progs$ btrfs version
```
btrfs-progs v4.11.1

Take a peek at the btrfs man pages, and in the next post we build a file system.
