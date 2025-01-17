---
title: "FFS"
layout: post
date: 2022-02-24 22:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- FFS
category: blog
author: xiaopeng
description: FFS
---


# FFS

1. **Disk structures**: cylinders, tracks, etc.  

2. **inodes**: metadata | direct pointers | indirect pointers  
   directories are files as well.  

    1. What did FFS do to improve performance?  
       3.1 Cylinder groups: to use spatial locality. Instead of storing inodes at one place, storing them separately in cylinder groups which is closer to their data blocks.  
       3.2 Increase blocks to 4KB, resulting in a larger maximum file size.  
       -Why not increase to a larger size?  
       -Small files lead to internal fragmentation  
       Even 4KB size leads to internal fragmentation problems, and to solve it, FFS introduces the concept of fragments. Fragments can only be used at the end of files, and managing fragments brings more complexity to the system.  
       3.3 Recovery: super blocks are used to store important data for the file system, like where is the root directory. Super blocks are replicated in every single cylinder group.  
       3.4 Read buffers in DRAM.  

3. Allocation: global allocator and local allocator.  
   **Global allocator**: Choose which cylinder group to store the new file/directory.  
   If creating a new directory, choose a relatively empty cylinder group, and if creating a new file, choose the same cylinder group as its directory.  
   **Local allocator**: Within a cylinder group, decide which blocks to use.  

