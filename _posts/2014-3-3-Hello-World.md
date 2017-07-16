---
layout: post
title: btrfs: build it
---

Here I'll build a 'near' latest kernel with CONFIG_BTRFS_FS and I'll build the latest btrfs-progs from github.

I'm taking a bit of a cheat here by not grabbing kdave's btrfs branch. I'm on a bit of a vacation and don't want to clone a new git tree. 

Do you already have btrfs already in your kernel?

$ cat /proc/filesystems | grep btrfs

Rebuild kernel with CONFIG_BTRFS_FS selected.  
(passing on selecting the additional options for now)

While that kernel is building, you can go ahead and start getting the
btrfs-progs from the github account. We're going to do that to get the
latest rather than using a package manager.

Getting and Building btrfs-progs

Here I grabbed most of what was recommended and even after that, had to
go grab some more as things failed. Easy part was that when something fails
it tells you exactly what was missing. Your mileage may vary on this process,
but here's a peek at what worked for me.


Next you can update your site name, avatar and other options using the _config.yml file in the root of your repository (shown below).

![_config.yml]({{ site.baseurl }}/images/config.png)

The easiest way to make your first post is to edit this one. Go into /_posts/ and update the Hello World markdown file. For more instructions head over to the [Jekyll Now repository](https://github.com/barryclark/jekyll-now) on GitHub.
