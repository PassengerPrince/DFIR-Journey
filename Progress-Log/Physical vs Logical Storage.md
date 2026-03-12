# Physical vs Logical Storage in Digital Forensics

## Introduction

In digital forensics, knowing how data is stored in a storage device will help determine how you will go about recovering a file. Here I will be sharing what I learned about physical storage and logical storage. First I will start with how a file is stored. Once we understand how data is stored, we will then go into how that data can be extracted to reconstruct a file. Lastly, we will go over how this all plays out in court when you present your findings.

---

## How Data is Stored on HDDs

We will go over how data is stored in both a HDD and a SSD. Data can be stored in other devices such as a USB, but that is a topic for another article.

To keep it simple, the data from files are stored in the **physical storage of a storage device**.

- In **HDDs**, sectors are grouped into clusters  
- In **SSDs**, pages (which are still recognized as sectors by the operating system) are grouped into blocks  

We will first focus on HDDs. These sectors are fixed sizes (512B sectors for example). Data from smaller files take up less space and may only need to use a few sectors, but data from larger files take up more space and therefore require more sectors.

When a file uses multiple sectors, the filesystem has to keep track of what sectors belong to a file. This is where logical storage comes in.

Logical storage groups sectors into clusters and keeps track of what clusters belong to which file using metadata. Metadata can be described as **data that describes data**. I like to think about it as the filters we use when we are shopping online. Instead of looking at all these different types of shoes, maybe I want to narrow it down to a certain brand and size.

Now, it would be ideal if these clusters were stored close to each other for convenience's sake, but often times they are spaced out throughout the disk, a process called **fragmentation**.

This happens as the disk fills up and free space becomes less available, forcing the filesystem to take whatever space is available kinda like finding a spot to park in a busy parking lot. We often aim for the closest spot, but if it is not available then we take whatever spot opens up or the open spots in the back if you are like me and have had bad experiences with people opening their car doors.

The way the filesystem allocates space can also contribute to fragmentation like how there may be a person who guides you to where you can park. When the clusters are spaced out like this, the storage device has to put in more work to find the file just like how we have to walk more if we park far away.

There is a way to group these clusters closer together through a process called **defragmentation**. Using the parking lot example again, think of it as rearranging all the cars so that all the cars that belong to a certain company are parked close to each other. Clusters of the same file are rearranged so that they can be closer together on the disk.

This creates less work for the read/write head of a HDD because it does not have to hop all around the disk. When a file is deleted, the space it took up becomes free to use.

---

## How SSD Storage Differs

Data stored in SSDs follow a similar pattern as well with a few differences. There is still a physical and logical aspect to it.

As mentioned before:

- Data is stored in **pages**
- Pages are grouped into **blocks**
- Files still take up multiple **clusters**

The filesystem continues to track which clusters belong to which file using metadata.

Instead of defragmentation, SSDs use **TRIM** and **wear-leveling** to manage the space on the drive.

- **TRIM** marks data as no longer needed so the drive can erase it later during internal cleanup processes.
- **Wear-leveling** spreads data across blocks so that one block is not overused and worn out.

Think of it as having one huge project you need to finish; instead of doing it all in one day which could fry your brain, you spread it out across different days.

---

## Slack Space

Now that we got the gist of how data is stored, let's look into how we can use this knowledge in digital forensics.

When a file is created, data from that file is stored in clusters however there still may be some leftover space within the last cluster. Data from a file may take up multiple clusters but it may not use up all the space in the cluster.

Remember each cluster is a fixed size.

For example:

- Cluster size = **4KB**
- Remaining file data = **2KB**
- Unused space = **2KB**

That remaining unused space is called **slack space**.

Slack space can contain remnants of previously stored data since it has not been overwritten yet. This data can sometimes be recovered to get an idea of what that old file may have contained.

It may not be as complete as file carving but that little piece of data may be all you need to complete the puzzle.

---

## Deleted Files and File Carving

When a file is deleted, the data for the file is actually not gone. The file's clusters are marked as available for reuse. It's like losing your car keys; you know it is somewhere but you just cannot remember where.

When a file is deleted, the space that the file took up becomes free to use. However, data from the old file may still remain in that space until it is overwritten.

This depends on things such as:

- how much free space is available  
- how the filesystem allocates space  
- overall system activity  

If the data hasn't been overwritten, there is a chance to piece that data back together to form the deleted file. This is referred to as **file carving**.

File carving tries to rebuild a file **without relying on metadata**.

It's like trying to identify a suspect of a crime. You may not know their name but you may be able to describe what the suspect was wearing, sex, race, etc.

File carving looks for patterns in raw data such as:

- **file headers**
- **file footers**
- recognizable file signatures

File carving works well if the sectors of the old file are close together and haven't been overwritten. It may take longer and be less effective if it is all spread apart with some sectors overwritten.

---

## Presenting Evidence in Court

Now let's pivot on how all of this can be used in court. The way you present your evidence depends on the type of drive. Again we will focus on HDD drives and SSDs.

When it comes to a HDD, it would be ideal if we could recover the whole file using file carving. However, you may not be able to recover the whole file, just some of it.

That made me question whether it can be used in court as evidence since it was not complete. It turns out that you do not need to recover the full file in order for it to be used in court.

Smaller files often times can be fully recovered. Files that are partially recovered may still yield useful evidence and are often supported with other artifacts as long as the method you use to collect these types of evidence are valid.

It would be like trying to prove someone was at the scene of a crime. You may not have video or photos of the person there but you can piece together things like:

- fingerprints
- footprints
- soil evidence
- other forensic artifacts

In other words, it would be nice to have evidence that clearly proves a person committed a crime but a **corroboration of multiple pieces of evidence** pointing to the same conclusion can be just as strong.

---

## Why SSD Investigations Are Different

When it comes to SSDs, the strategy often changes. It is often harder to use file carving and slack space analysis to recover a file for a few reasons.

For example:

- SSDs may have **TRIM enabled**
- **garbage collection** may erase data faster
- data may be **internally remapped** due to wear-leveling

Because of this, investigators may shift their focus away from recovering the file itself and instead focus on:

- proving the file existed  
- showing it was opened or executed  
- examining user activity  
- identifying when the file may have been deleted

## Use of AI

I used AI to satisfy my curiosity, proof read my work, and format this post. I learn best by asking questions so for this topic, I asked ChatGPT questions to better understand physical and logical storage. Once I got a good understanding, I created a rough draft and had it look for incorrect info as well as grammar mistakes. Once we had a solid draft, I had it format it into this post. To be transparent, I will include the original rough draft to show I wrote the content as well as part of my chatgpt chat to show how my curiosity works. Obviously AI can be wrong and there may still be some mistakes in this post so feel free to let me know what they are and how I can improve them.
