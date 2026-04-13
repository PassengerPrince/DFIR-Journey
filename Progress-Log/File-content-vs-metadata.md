# File Metadata & Content (DFIR Notes)

## Overview

In this article, I go over what I learned about the importance of metadata in digital forensics. While being able to view a file’s actual content can be helpful, that is not always possible. Even when the content is available, metadata still plays an important role by providing context and supporting an investigation.

To better understand this topic, I brought my questions to AI and used it as a way to explore my curiosity. I asked follow-up questions to clear up confusion, test my understanding, and dig deeper into ideas that stood out to me. I did not include every question from that conversation because I wanted to keep this article focused and easy to follow.

I structured this article around three parts: a starting question, what I learned, and reflection. The starting question introduces a topic I was curious about and shows where my understanding began. The what I learned section explains how my understanding developed as I asked follow-up questions. The reflection section gives me space to look back on my thought process and explain how my understanding improved.


## Questions & What I Learned

### 1. Creation Time

**Starting Question:**
So let's start with creation time. Since you already defined them, let me take a different approached. When you say file first appeared, does that just mean when the file was created or could it also mean when the file first appeared such as when it was downloaded?

**What I learned:**
Creation time reflects when a file is created on a specific filesystem, not necessarily when it originally existed. For example, when a file is downloaded, the system treats that moment as its “creation,” even if the file existed elsewhere beforehand. This means creation time is tied to the system’s perspective and can be misleading if taken at face value. To get a clearer picture, it needs to be considered alongside other timestamps rather than on its own.

**Reflection:**
I suspected that the creation time didn't reffer to when the actual file was created but instead had to do more with when the file first appeared on the system. I wanted to clear that up so that when I see that, it don't lead me down the wrong direction. I wasn't sure if creation time would also tell us who created the file so I had to clear that up as well. 

---

### 2. Modification Time

**Starting Question:**
Let's move on to the modification time. This just tells us what time a file was changed? If so is it useful to help prove someone had access the file?

**What I learned:**
Modification time shows when the contents of a file were last changed. While this can indicate interaction with the file, it does not prove who accessed it or even confirm direct user involvement. Changes could come from users, programs, or automated processes. This made it clear that modification time is useful as supporting evidence, but it needs to be correlated with other artifacts to make stronger conclusions.

**Reflection:**
When a file is modified, it would make sense to see who modified it but as I learned this just tells us when it was modified and not by who. Many things such as groups, programs, scripts, etc could have modified the file so deeper digging would need to be done to find out who modified it. I would try to pair with with some type of log that would show what user has an active session during that time frame to narrow it down. It would also help to look at the file properties. Maybe there is a clue in the file path that can tell us where it came from. 

---

### 3. Access Time

**Starting Question:**
Well let's move on to access time. Like you mentioned, access time shows who open or viewed a file. We can see who opens it but is that limited to just a person? Like can it show if a program opened it?

**What I learned:**
Access time records when a file was opened or read, but it does not identify whether it was a user or a program. Both can trigger updates. On top of that, access time is not always reliable because some systems disable or limit updates to improve performance. This means it can provide helpful clues, but it should not be relied on as definitive proof of access.

**Reflection:**
I was mistaken when I said access time shows who open or viewed a file. I learned just like the other, it just tells use the timestamp instead of who opened it. Early in the chat, it also mentioned a program could access a file so I wanted to get a better understanding of what else can have access to a file other than a user. Again, this can be use along with other artifacts in colloboration. 

---

### 4. Entry Modification Time

**Starting Question:**
okay do people get tripped up on entry modified time because it can be mistaken for when a file was modded? If I had to take a guess, it may reffer to the permissions instead of the actually file contents being modded

**What I learned:**
Entry modification time (metadata change time) is often confused with content modification, but it actually tracks changes to the file’s metadata. This includes things like permission changes, renaming, or ownership updates. It does not reflect changes to the file’s contents. Understanding this distinction is important because it helps differentiate between someone altering the file itself versus changing how the file is handled or accessed.

**Reflection:**
My guess was on the right track. Instead of a change in the file content, it tracks changes to the file's metadata. Looking back at this conversation now, I should have asked if it tell us what part of the metadata has changed. That is something I need to look up later on. I learn that C time doesn't necessary raises red flags since file metadata is commonly changed but it could be part of a bigger pattern when one artifact leads to another. 

---

### 5. File Signatures & Extensions

**Starting Question:**
Let's move on to file signatures and ext. What is file signatures?

**What I learned:**
File signatures, or magic numbers, are unique byte patterns at the start of a file that identify its true type. Unlike file extensions, which can be easily changed, signatures are embedded in the file itself. Through follow-up questions, it became clear that some signatures are human-readable (like `%PDF`), while others are purely binary and require tools to interpret. This reinforced that relying on extensions alone is not enough—file content must be examined to accurately determine file type.

**Reflection:**
You can tell I had no idea what file signature was because I didn't even throw in a guess lol. Turns out it can be a super useful in idenitfying a file's true nature since the file extension can easily be changed to fool someone. There are tons of file signatures out there so it would be hard to memorize em all so it would be more efficient to rely on databases that translate them for us or if you commonly look at file headers, you may know which one is which. 

---

### 6. File Size, Path, and Permissions

**Starting Question:**
Lastly, is there anything we can learn from file size, path, perm/owner/ attribute outside of the obvious?

**What I learned:**
File metadata provides more insight than it initially seems. File size can help identify anomalies, such as files being too small (possibly incomplete) or larger than expected (potentially containing hidden data). File paths can reveal suspicious or unusual locations. Permissions and ownership can indicate unauthorized access or changes. From follow-up questions, it also became clear that recognizing these anomalies is partly based on general expectations for file types and partly on experience. Altogether, these attributes help build context and support investigative findings.

**Reflection:**
I focused more on file size here because I wonder how familiar you have to be with expected file sizes to know something is off about the size of a file. I certainly don't look at file sizes often so I can see how it can be overlooked. It did suggest some usefull tricks to identify suspicious file sizes such as comparing it to similar files on other systems, if the file opens correctly and is complete, etc. I would have to get more familiar with files to see if the file size make sense for what the file claims to be. 

---

## Use of AI

As mentioned in the overview, I had AI answer my question I had to get a better understanding of concepts. I asked it questions and also had it generate a couple scenarios for me to practice my understanding. Then I used AI to help structure this article into make it easier to read by formating it into a markdown file with headers for Github. Content from the starting question and what I learned section are based off the chat and the reflection section is what I wrote myself. My use of AI for this article is to learn and to be more efficient with my time so I don't get burnt out faster. I understand that AI can get things wrong and I hope whoever reads this can help me spot where it went wrong. For transparency, I will post the links to the chats I used when learning and formating:
--Learning: https://chatgpt.com/share/69c4997b-2dec-83e8-9ac9-1f30fe33c9d3
--Formating: https://chatgpt.com/share/69dd77fd-5c7c-83e8-a394-e87d7ae1a54e
