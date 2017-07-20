---
layout: post
title: tensorflow & btrfs
published: true
---

Putting Google's Tensorflow and Linux's BTRFS to work

I recently became familiar with both of these technologies, and I think they are going to help quiet one of my fears.

### Super Short Backstory of Fear
Years of photos and video with no organization or reliable storage.

### Enter Google's Tensorflow 
Recently I came across the magic of Tensorflow, and felt a huge burden lifted. I'd already enjoyed various Google automagically made albums, but now, I saw the new sorting capabilities with Tensorflow. I must diverge into a couple of examples to explain my WOW - because I'm not so easily WOW'd. The set up. I had about 80 photos on my phone, including some crowd photos, so there were a lot of extraneous faces, in addition to important family members! 

- Wow 1: The set included about 12 photos of my 10 year old taken within the same 2 week period.  Also, in the set was one picture of this same child at 3 months old. (It was a _memory lane_ facebook post.) Tensorflow matched these! 3 mos - 10 years!!! I could barely match them ;)

- Wow 2: A picture of a grainy B&W photo of my Mom at 25 years old. The only other picture of Mom was at 85 years old. Matched! (Granted we are a family of ladies who age beautifully ;))

- Wow 3: A mistake? I thought I caught a Tensorflow mistake. My teenager and my husband seemed to be mixed up. No mistake! Upon careful inspection we found that my husbands reflection was in a background mirror in my sons photo.

The point here is that my disorganized photos are no longer an issue. In addition to this facial recognition, they are automagically sorted by occasion, location, and more. Now, my challenge is to _not_ lose them!

###Enter Linux's BTRFS

I'd been exploring the btrfs filesystem, and it caught my attention as a tool that fits the way I tend to grow my storage. I'll have a disk, fill it up with photos, video (home & commercial), then when that gets full, grab another disk, repeat. These disks tend to be salvage, because not a lot of preplanning goes into this.

btrfs is perfect for this. It allows you to dynamically grow your filesystem without the overhead of manually moving data. It also offers you failure protection with it's RAID implementation.  (not a substitute for backups!!)

For this project I've installed Tumbleweed on an old laptop to act as my NFS server for all media - I've added commercial movies to the list.

Plan is to use OpenSUSE YaST for setting up the NFS Server and btrfs, (as opposed to command line). I'll post that process when complete. My first foray into SUSE YaST. Then I will rest easy!

