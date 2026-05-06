# DFIR & File Systems (FAT,ext,APFS)

## Overview
In this article, I seek to understand more about how a filesystem affects DFIR. The most common filesytems are NTFS,FAT,ext,and APFS. For this article, Im going to focus on FAT, ext, and APFS. I'll save NTFS for another article. Not all filesystems are the same and it is worth understanding what is different about them in order to see how digital forensics is impacted. 
## Questions & What I Learned

### 1. FAT32/exFAT Timestamp Limitations

Starting Question: Let's focus on FAT32/exFAT. How are the timestamps limited? Is it because it doesn't have MAC timestamps like NTFS?

What I learned:

* FAT32 and exFAT do store timestamps, but they are less detailed and less forensically rich than NTFS timestamps.
* FAT32 commonly tracks created, modified, and accessed dates, but access time may only be recorded as a date instead of a full timestamp.
* exFAT improves some timestamp handling, but it still does not provide the same metadata depth as NTFS.
* NTFS stores multiple timestamp sets, giving investigators more points of comparison.
* The main limitation is timestamp precision, quantity, and metadata depth, not simply the absence of timestamps.

Reflection:
 
 I was thinking that FAT had similar timestamps to NTFS such as created, modified, and access so I was wondering what made it less detailed. Turns out FAT is less precise because it can exlude actual times in some of the timestamps. Furthermore, it make timeline reconstruction more difficult since the timestamps are recorded in 2 second increments. I touched up on how it can be challenged in court but as always corroboration is key so the more evidence you have to support your arguement, the better. 

---

### 2. FAT32/exFAT File Carving

Starting Question: How is FAT32/exFAT easier to carve? I notice it said it marks the entries as free but don't NTFS do that as well what does it do different from NTFS?

What I learned:

* FAT32 and exFAT can be easier to carve because their filesystem structures are simpler than NTFS.
* When files are deleted, FAT-based systems often mark directory entries as available while the file content may remain until overwritten.
* NTFS also marks records as unused, but it has more complex metadata structures like the Master File Table, attributes, journaling, resident/non-resident data, and detailed file references.
* NTFS complexity can help forensic analysis, but it can also make carving and interpretation more involved.
* FAT32/exFAT recovery is still affected by fragmentation, overwriting, and missing cluster chain information.

Reflection:

Since FAT marks entries as free like NTFS, I was wondering how it differs. They follow the same idea: when a space is marked as free, the data remains until it is overwritten. The key difference between FAT and NTFS to me is that FAT is less fragmented than NTFS so the data in FAT is more likely in one continuous chunk which makes it easier to carve out. 

---

### 3. ext3/ext4 Inodes

Starting Question: For ext3/ext4, are inodes easier to read since they are often in a table format?

What I learned:

* Inodes in ext3 and ext4 are organized in inode tables, which helps forensic tools parse the filesystem consistently.
* An inode stores metadata such as ownership, permissions, timestamps, file size, and pointers to data blocks.
* File names are usually stored in directory entries, not directly inside the inode.
* Investigators often need to connect directory entries, inode numbers, and data blocks to understand the full file record.
* Inodes are organized, but they are not automatically easy to read without understanding ext filesystem structure.

Reflection:

I was thinking back to the modules I did in TryHackMe and I could remember somewhere I seen the inodes in a table format maybe because of the use of some tool. That's why I asked that question. It was interesting to learn how metadat of a file are stored in inodes but the filename is stored somewhere else so a person would have to go and link it together. It does create more work but can be satisfying to those who enjoy they type of stuff. I just wonder if they manually do it or use some type of tool to do it. It also makes me question if linux is more simple than the other OS, why is there more work to do to get a full picture of a file? Maybe in order to make Linux more simple, it had to split those apart.  

---

### 4. File System Consistency

Starting Question: Let's talk more about filesystem consistency since Im not quite getting the picture. Is it about the filesystem performing the same process for each file?

What I learned:

* File system consistency is not about doing the exact same process for every file; it is about whether the filesystem’s records agree with each other.
* If a directory entry says a file exists, the related metadata and data block references should also make sense.
* If a file is deleted, the directory entry, allocation records, and free-space tracking should not contradict each other.
* Inconsistency can happen after crashes, interrupted writes, corruption, or improper shutdowns.
* Journaling helps maintain consistency by recording planned changes before applying them, which makes recovery easier after a failure.

Reflection:
Never knew how clutch journaling can be. I wasn't thinking about the fact of how a crash, corruption, etc can affect consistency. If I was the defense attourney and saw that everything was inconsistent, I would definetly raise the issue. Journaling is like having a itinerary for a vaction except this is far from a vacation. 

---

### 5. APFS Copy-on-Write

Starting Question: Moving on to APFS, what does copy on write mean? Is it as simple as making a copy of the data it wrote for redundancy?

What I learned:

* Copy-on-write means APFS usually does not overwrite existing data or metadata in place.
* When a change is made, APFS writes the updated version to a new location and then updates references to point to it.
* Copy-on-write is not simply making a backup copy for redundancy; it helps protect filesystem consistency and supports features like snapshots.
* Older versions may remain temporarily, which can be useful in forensic analysis.
* It can also make analysis more complex because investigators may need to understand snapshots, object references, and updated data tracking.

Reflection:
This kinda solve the issue talked about before about consistency. I say kinda because I wonder if there is anything else that can cause it to be inconsistent. Though it is nice to know that APFS writes to a new block and updates references before freeing up the old block. I also touched up on how APFS is more complex than NTFS and to simply put it, we have more to look at such as snapshots to get more historic data about a file and its changes.  

---

## Use of AI
I used AI to ask questions in order to understand how filesystems differ and how it affects digital forensics. Then, I used it to help format this markdown file. Content of this article are based on content of the chat. For transparency, I will include the chat link I used so that you can see how I went about creating this. Obviously AI can be wrong so feel free to reach out if you see anything that needs correction or clarification. 
Question: https://chatgpt.com/share/69fb7194-ad14-83e8-998c-32016156e7ac
Formating: https://chatgpt.com/share/69fb716f-37ac-83e8-816d-86a440e4319d
