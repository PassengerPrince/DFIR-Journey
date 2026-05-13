# ZFS Overview and DFIR

## Overview

In this article, I will go over what I learned about ZFS and why it matters from a digital forensics point of view. I focused on understanding how ZFS handles storage differently from more traditional file systems, especially with pool storage, dynamic striping, and RAIDZ.

My goal was not to cover every technical detail of ZFS, but to break down the parts that helped me better understand how data can be organized, protected, and recovered. I used a question-based approach where I started with what I already thought, asked follow-up questions, and connected the answers back to how ZFS works.

-I recently joined LetsDefend and started a Digital Forensics course where it is introducing me to a bunch on new concept to spark my curiosity. As I go through the material, I write down questions that pop up that the material in the course doesn't fully address. Then I take my curiosity with to AI to help furthur explain it

## Questions & What I Learned

### Pool Storage

**Curiosity:** I wanted to understand how ZFS pools organize storage. I asked if it was like a public swimming pool where kids and adults are swimming together, but we can still tell the difference between them. Then I wondered if it would be more accurate to say they are in the same pool, but one half is for kids and the other half is for adults. I also asked if ZFS is more flexible because it has more space to work with.

**What I learned:** ZFS uses storage pools differently than a normal file system that sits on top of one partition or one disk. Instead of each file system being locked to its own fixed area, ZFS creates a shared pool of storage that different datasets can use.

The swimming pool example helped me understand it better. A ZFS pool is like one big pool of storage, and datasets are like different groups using that same pool. They can still be separated and managed differently, but they are not stuck inside a small fixed section unless limits are added.

That is where ZFS becomes flexible. It does not mean there is unlimited space, but it does mean storage can be shared more easily. A dataset can use available space from the pool without needing to resize partitions the traditional way.

### Dynamic Striping

**Curiosity:** I asked if dynamic striping is similar to wear leveling because both involve spreading writes out. Since ZFS writes across multiple disks and wear leveling writes across multiple blocks, I wanted to know if they were basically doing the same thing in different places.

**What I learned:** Dynamic striping and wear leveling sound similar at first because both involve spreading writes out, but they are solving different problems.

Dynamic striping in ZFS is about spreading data across multiple disks in a pool so performance and space usage can be balanced. If more disks are available, ZFS can write across them instead of relying on one disk to handle everything.

Wear leveling is more of an SSD concept. It spreads writes across memory cells so one area of the SSD does not wear out faster than the others. So the simple way I understood it is this: dynamic striping is about using multiple disks efficiently, while wear leveling is about protecting the life of flash storage.

From a DFIR view, this matters because the data may not live neatly in one place. ZFS can spread data across disks, which means understanding the pool layout becomes important when trying to analyze or recover information.

### RAIDZ

**Curiosity:** I asked what RAIDZ is because I had heard of RAID 0, RAID 1, RAID 5, and RAID 1+0, but not RAIDZ. Then I asked why parity can only survive a certain number of disk failures, whether parity gets rebuilt after a failure, and if replacing a bad disk means physically swapping out the hardware.

**What I learned:** RAIDZ is ZFS’s version of RAID-style redundancy, but it is built into ZFS instead of being a separate hardware RAID layer. It protects data by spreading data and parity across multiple disks.

Parity is extra information that can help rebuild missing data if a disk fails. RAIDZ1 can survive one disk failure, RAIDZ2 can survive two, and RAIDZ3 can survive three. This helped me understand why parity does not mean unlimited protection. There is only enough parity to recover from a certain number of failed disks.

I also learned that replacing a bad disk usually means physically swapping out the failed hardware. After that, ZFS resilvers the pool, which means it rebuilds the missing data onto the replacement disk using the remaining data and parity.

The bigger picture is that RAIDZ is not just about storage space. It is about keeping data available and trustworthy even when hardware fails. From a DFIR perspective, that matters because the pool may still preserve usable evidence after a disk failure, but the investigator needs to understand the RAIDZ layout to properly interpret or recover the data.

## Use of AI

I used AI to take the content from my chat and prompted it to format it into this markdown file. My goal is to show what topics I focused on, summarize my questions to showcase my curiosity as well as what I learned, and to keep it similar to how I would write it. 

Most of what you see here is the work of AI besides this section and the hypen comment in the overview section. I chose to use AI to spend more time learning and less time wondering how Ima format it and make it sound (me get burnt out like that)

Here is the Chat link for transparency: https://chatgpt.com/share/6a05028c-c76c-83e8-be84-b739cd1156c4
