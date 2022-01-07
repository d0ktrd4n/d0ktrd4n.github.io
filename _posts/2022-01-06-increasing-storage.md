---
layout: post
title: Increasing storage area by reducing OS reserved space
categories: [informational]
tags: [linux]
fullview: false
comments: false
---

On Linux, by default, an area of **5%** of your storage is reserved for emergencies (if you HD gets full, the OS needs space for it not to crash quickly).

5% space probably would make sense in older times, when our storage area was a lot smaller. If you have a good sized HD/SSD (500GB, 1T, or more), this may actually be a tremendous overkill, and waste of space. For **500GB** it means that you have **25GB** that you cannot use, if you have a 1T disk, it's about 50GB of unusable space... well... except if the OS needs for "emergencies".

You can find how many blocks are reserved and the size of each block in bytes (often 4096 bytes/4kB) by running  the following command:
```bash
tune2fs -l /dev/sdXX | grep -E "Reserved block count|Block size"
```

In the command above,  `/dev/sdXX` is your partition. To find the reserved space in bytes, you simply multiply number of reserved blocks by the size of each block. 

This is not the worse yet. The same thing also applies to any storage device. For example, if you have a 2T external HD formatted as ext4 (or most linux-based file systems), 5% of its space is reserved, which means you lose 100GB of storage area. If you're using the device for backup, that's simply 100GB of wasted space. 

For full disclosure, what I am discussing here may apply to a device that is formatted with FAT/NTFS. I still need to check that out. 

The good news is that you can reduce the reserved space using the following command: 
```bash
tune2fs -r YYYYYY /dev/sdXX
``` 
Where `YYYYYY` is the number of blocks (divide the space you intend to reserve by the number of bytes in a block) and `sdxx` is the partition you want to modify. 

Some recommend you to leave about 1GB of reserved space. 

For my actual HD, I'd keep it at 5% or slightly over the amount of RAM in your my computer (whichever is smaller). My rationale for this is that, if OS needs to save memory state, it'd have enough space for it. But you can take your chances.

For an external storage area, however, I am actually struggling with the idea of leaving anything more than  0.5 GB. Frankly, I even find that 500MB may be too much. If it's a device used for backup purposes (which most external devices are), why would I need to reserve space for the OS? 

But the decision is yours. It's your computer, your device! 
