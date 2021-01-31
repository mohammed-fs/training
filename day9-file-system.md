

**9-File-System**

**Day-9**

https://github.com/becodeorg/BXL-Lovelace-6.27/tree/master/field/physical_storage

### 1-What is a sector? a cylinder? 
A hard drive contains one or more platters,
 the platters are split up into tracks and  each track is broken  down into smaller units called sectors, As sector is the basic unit of data 
storage on a hard disk.
each track has the same number of sectors, which means that the sectors
are packed much closer  together on tracks near the center of the disk.
A single track typically can have thousands of sectors.
The data size of a sector is always a power of two, and its either 512 or 4096 bytes.
![sector](https://www.linkpicture.com/q/sectors.jpg)

---

### 1-1- What is a cylinder? 
The collection of all tracks that are the same distance,
from the edge of the platter, is called a cylinder
![cylinder](https://i.ibb.co/sRM3NNz/cylinder.jpg)

---

### 2-How a hard drive work? 
The CPU and motherboard use software to tell what’s called the “Read/Write Head”
 where to move on the platter and where it then provides an electrical charge to
 a “sector” on the platter. Each sector is an isolated part of the disk containing 
thousands of subdivisions all capable of accepting a magnetic charge.

There are one or more platters in the hard drive, where information 
is stored magnetically, there's an arm mechanism that moves the 
 read-write head back and forth over the platters to record or store information,
 and there's an electronic circuit to control everything and act as a link between 
the hard drive and the rest of the computer, Each bit’s magnetic charge
 translates to a binary 1 or 0 of data.

---

### 3-Performance-wise, how does hard drive compare to solid state drives? Why?
SSD is faster than HDD in read/write. 

With HDD, It takes time for the head to move to the right position, known as seek time, There is a delay as the head waits for the right part of the platter to come around, known as rotational latency, or simply latency.

With an SSD there is no variable seek time or rotational latency, as every part of the SSD can be accessed in the same amount of time. But SSD read and write speeds are asymmetric: data reads are very rapid, but SSD write speeds are somewhat slower.

---

### 4-With FAT32 and EXT2, your drive is divided in multiple parts, try to produce a schematic showing those different parts for each file system. 

**In FAT32, drive divided into 4 parts:-**
Reserved sectors:-Includes the boot sector, the extended boot sector, the file system information sector, and a few other reserved sectors The first reserved sector is the Boot Sector, it contains some basic file system information, used for loading the operating system.

**FAT region:-** This typically contains two copies of the File Allocation Table for the sake of redundancy checking.
These are maps of the Data Region, indicating which clusters are used by files and directories.	

**Root Directory Region**
It is only used with FAT12 and FAT16

**Data Region**
This is where the actual file and directory data is stored and takes up most of the partition. 

**EXT2** uses blocks as the basic unit of storage, inodes as the mean of keeping track of files and system objects, block groups to logically split the disk into more manageable sections, directories to provide a hierarchical organization of files, block and inode bitmaps to keep track of allocated blocks and inodes, and superblocks to define the parameters of the file system and its overall state. 
is divided into small groups of sectors called “blocks”. These blocks are then grouped into larger units called block groups. 

**Block Groups**

Blocks are clustered into block groups in order to reduce fragmentation and minimise the amount of head seeking when reading a large amount of consecutive data. Information about each block group is kept in a descriptor table stored in the block(s) immediately after the superblock.

**Directories**

A directory is a filesystem object and has an inode just like a file. It is a specially formatted file containing records which associate each name with an inode number. Later revisions of the filesystem also encode the type of the object (file, directory, symlink, device, fifo, socket) to avoid the need to check the inode itself for this information

The inode allocation code should try to assign inodes which are in the same block group as the directory in which they are first created. 


**Inodes**

The inode (index node) is a fundamental concept in the ext2 filesystem. Each object in the filesystem is represented by an inode. The inode structure contains pointers to the filesystem blocks which contain the data held in the object and all of the metadata about an object except its name. The metadata about an object includes the permissions, owner, group, flags, size, number of blocks used, access time, change time, modification time, deletion time, number of links, fragments, version (for NFS) and extended attributes (EAs) and/or Access Control Lists (ACLs). 


**Allocating data**
When a new file or directory is created, ext2 must decide where to store the data. If the disk is mostly empty, then data can be stored almost anywhere. However, clustering the data with related data will minimize seek times and maximize performance.

---

### 5-Try to represent on a schema how you can find a file named test.txt on FAT32 and another one with EXT2 


---

### 6-What are softlink? hardlink? what's the difference between them? Are their limitations? Are they working on both FAT32 and EXT2? 

**Symbolic links** (also called **soft links**) and Hard Links are a resource to access files or directories from any location.

**Soft Links**

In contrast to hard links, soft links are not copies of the original file, they contain the path to the original file, because of this if the original file is removed the soft link or symbolic link will point to no file. becoming a broken link, or an orphaned link, which means if you loss the source file, if you delete or move it the symbolic link will loss access to the information, while with the hard link the information remains despite the source file removal because it is a full and exact copy of that file.

Also in contrast to hard links symbolic links don’t share the same inode with the original file, that’s why symbolic link can cross volumes and filesystems while hard links can’t. Symbolic links can be used to link directories while with hard links that’s not possible.

**Hard Links**

Hard links are not a file containing the path to the original file but mirror copies of the original file they point to. A file and it’s hard links aren’t associated by the name or path but by the inode which stores information on the file, like it’s location, creation date, permissions and other attributes. Each inode number is unique within a filesystem preventing hard links from working between different partitions or systems. Hard links can’t be used to link directories.

In contrast to soft links, hard links contain the information they link to so if the original file is removed you can still access it’s data.

**A soft link**

* can cross the file system 
* allows you to link between directories
* has different inode number and file permissions than original file
* permissions will not be updated 
has only the path of the original file, not the contents.

**A hard Link**

* can't cross the file system boundaries (i.e. A hardlink can only work on the same filesystem) 
* can't link directories 
* has the same inode number and permissions of original file
* permissions will be updated if we change the permissions of source file 
* has the actual contents of original file, so that you still can view the contents, even if the original file moved or removed.

**FAT32** do not work with Hard links or soft links.
**EXT2** can work with both Hard and soft links.

---

### 7-How does FAT32 and EXT2 represent the date of creation/modification? Why is EXT2 more precise than FAT32? 

**FAT32  file systems have three types of time stamps for each file.**
1. Time when the file was First-created
2. Time when the file was Last-modified
3. Time when the file was Last-accessed


**the ext2 filesystem inode structure has 4 timestamps:**
1. i_ctime - The time in which the inode was last allocated. (the time in which the file was created.)
2. i_mtime - The time in which the file was last modified.
3. i_atime - The time in which the file was last accessed.
4. i_dtime - The time in which the inode was deallocated. (the time in which the file was deleted.)

### Why is EXT2 more precise than FAT32?
**FAT32**
* Max. file size: 2 GiB
* Max. volume size 2: TiB


**ext2**
* Max. file size: 2 TiB
* Max. filesystem size: 32 TiB

---

### 8-Why do we use defragmentation on hard drive? What does it do?
* To increases computer performance.

It reorganizing the data stored on the hard drive so 
that related pieces of data are put back together, all lined up in a continuous fashion.
it picks up all of the pieces of data that are spread across your hard drive and puts them back together again, 
nice and neat and clean. Defragmentation increases computer performance.


Effects of Fragmentation on Computer Performance
File fragmentation directly affects the access and write speed of that disk, 
steadily corrupting computer performance to nonviable levels.
 Because all computers suffer from fragmentation, this is a critical issue to resolve.

---

### 9-Why you should never use defragmentation on a SSD?
it can cause unnecessary wear and tear which will reduce its life span.

---

### 10-Is it possible to store file over 4Go on a FAT32 partition? Why?
This is due to FAT32 limitation. Files larger than 4GB can NOT be stored on a FAT32 volume.

---

### 11-What does the file system when you delete a file? How is it possible to recover deleted files from your drive?

When a computer deletes a file or the Recycle Bin is emptied, 
it's removing the reference to the file on the hard drive. 
Once the file header, or reference, is removed, the computer can no longer see the file. 
The space the file took up is no longer reserved for that file, 
and any new file can be stored in that location.
The file is no longer readable by the computer. However, 
the file remains on the hard drive until another file or part of another file is saved to the same location.
Because the file is technically there, it may be recoverable using data recovery software 
to rebuild the file header, allowing the computer to see the file again. 
This software only works if no other file or data was saved over the top of the deleted file.

How is it possible to recover deleted files from your drive?
because the file remains on the hard drive until another file or part of another file is saved to the same location.
Because the file is technically there, it may be recoverable using data recovery software
The chances of a successful result depend heavily on the specific situation of the data loss. 
In general, the data recovery process is based on a memory scan, which is used to find certain information 
(deleted files, lost file systems) and to compose structures of the damaged file system.

---

### 12-Do an explanatory document per file system (EXT2, FAT32) explaining with schematics how the file system works.

---