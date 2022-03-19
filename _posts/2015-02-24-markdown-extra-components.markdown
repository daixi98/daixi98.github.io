---
title: "Markdown Extra Components"
layout: post
date: 2016-02-24 22:48
image: /assets/images/markdown.jpg
headerImage: false
tag:
- markdown
- components
- extra
category: blog
author: jamesfoster
hidden: false #don't count this post in blog pagination
description: Markdown summary with different options
---



# LFS

1. **Trend for hardware.**  
   Disk: seek is slow, but higher transfer speed and higher capacity.  
   Memory: larger space and cheaper -> larger file read buffer.  
   This trend (more write and less read for disks) leads to file write buffer. Programs simply write to file buffer in memory, and memory will write it to disk later.
2. **Trend for workload.**  
   More write and less read for disks due to reasons above.  
   A lot of small files (e.g., browser cache files)
3. **Problems of FFS.**  
   Still needs to seek.  
   Still needs to do synchronous writes.
4. **What does LFS do?**  
   For all the writes, just write to buffer cache. When the buffer cache comes to 1 MB, do one I/O to disk and copy the whole buffer to disk directly -> most of the time doing transfer, less time for seeking.
5. **Challenges.**  
   Garbage collection. How to find deleted blocks? -> cleaning process, significant overhead.  
   Reads. How to find inodes? -> Introduces inode map.
6. **Cost-benefit policy.**

