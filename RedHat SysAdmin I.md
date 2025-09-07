
## Quick access

[CH03_Managing Files From the Command Line](#ch03_managing-files-from-the-command-line)



### CH02_Accessing the Command Line
- A command line is a **text-based interface** which can be used to input instructions to a computer system.
- The **default shell** for users in Red Hat Enterprise Linux is the **CNU Bourne-Again Shell** (**bash**).
- When a shell is used interactively, it displays a string when it is waiting for a command from the user. This is called the **shell prompt**.
#### some common commands
- **date**: print the current date & time
- **passwd**: Changes a user's own password.
- **file**: Scans the beginning of a file's contents and displays what type it is.
- **cat, less, head, tail**
- **history**
	- `!<command_no>` : run the command of this indexed number 
	- `!!` :  run the last command 
	- `ctrl+r` : search in the history about specific command

#### some important terminal shortcuts
- **Tab**: auto completion
- `\` :  continue a long command on another line
- **ctrl+A**:  Jump to the beginning of the command line.
- **ctrl+E**: Jump to the end of the command line.
- **ctrl+U**: Clear from the cursor to the beginning of the command line.
- **ctrl+K**: Clear from the cursor to the end of the command line.
- **Ctrl+LeftArrow**: Jump to the beginning of the previous word on the command line.
- **Ctrl+RightArrow**: Jump to the end of the next word on the command line.
- **ctrl+R**: Search the history list of commands for a pattern.

<br>
<br>
<br>

### CH03_Managing Files From the Command Line

#### Understand file system hierarchy.

![Linux-file-system-hierarchy-Linux-file-structure-optimized](https://github.com/user-attachments/assets/b9a941a0-b5ac-4719-b010-6ab56720593e)

<img width="1198" height="473" alt="Pasted image 20250814183504" src="https://github.com/user-attachments/assets/78f18827-1524-438d-a3eb-920f394b2354" />


##### **/usr**:
Installed software, shared libraries, include files, and read-only program data.
Important subdirectories include:
- /usr/bin: User commands.
- /usr/sbin: System administration commands.
- /usr/local: Locally customized software.
##### **/etc:**
Configuration files specific to this system.
##### **/var:**
Variable data specific to this system that should persist between boots. 
Files that dynamically change. 
such as databases, cache directories, log files, printer-spooled documents and website content may be found under `/var`.
##### **/run:**
Runtime data tor processes started since the last boot. 
This includes process ID files and lock files. among other things, The contents of this directory are
recreated on reboot. 
This directory consolidates /var/run and /var/lock from earlier versions of Red Hat Enterprise Linux.
##### **/home:**
Home directories are where regular users store their personal data and
configuration files.
##### **/root:**
Home directory for the administrative superuser, root.
##### **/tmp:**
A world-writable space for temporary files. 
Files which have not been accessed, changed, or modified for 10 days are deleted from this directory automatically. 
Another tempbrary directory exists, `/var/tmp`, in which files that have not been accessed, changed, or modified in more than 30 days are deleted automatically.
##### **/boot:**
Files needed in order to start the boot process.
##### **/dev:**
Contains special device files that are used by the system to access hardware.




#### Specify what of files are available in Linux
##### types of files in Linux

| Symbol | Meaning      |
| ------ | ------------ |
| -      | Regular file |
| d      | Directory    |
| l      | Link         |
| c      | Special file |
| s      | Socket       |
| p      | Named Pipe   |
| b      | Block Device |


<img width="1230" height="514" alt="Pasted image 20250815021703" src="https://github.com/user-attachments/assets/0dc7e203-3056-4883-803c-ed944ea5099b" />


##### **File Naming**
**Should**
- Be Descriptive
- Only alphanumeric characters: UPERCASE, lowercase, number, @, _ 

**Shouldn't**
- Include Embedded Links
- Contain Shell Metacharacters: `* ? >< / ; & ! [ ] | \ ' " ( ) { }`

**Notes**
- Files Are case sensitive.
- Filenames starting with a "." are hidden.
- Maximum number of character for a filename is 255.


#### Access directories using absolute and relative path.

**tools to Navigate Paths:**
- #### pwd 
- #### cd
```Bash 
cd -           # back to the last path
cd ..          # back to the parent folder of the current one   
cd ~           # go to /home directory  OR use "cd  " 
cd <USERNAME>  # go to this username folder.
cd ~/USERNAME  # go to /home/user directory
```

#### List the contents of directories
##### ls command
The **Is** command has multiple options for displaying attributes on files, The most common and useful are:
```Bash
-l     # long listing format 
-a     # all files, including hidden files
-R     # recursive, to include the contents of all subdirectories
-lh    # human readable
-t     # access time 
-r     # reverse order
```


##### dir command
the same as **ls** command but not colored, but there is an option to make the output colored: 
`dir --color`

#### Create, copy, move, and remove files and directories

##### create files & directories (touch & mkdir)
```Bash
touch file1 FILE1  # Case Sensitive
touch /root/Desktop/file1
touch {file1, file2, file3, file4} 
```

> **NOTE!:** if the file exists, it will reset the timestamp of the file. 


- **mkdir**
```Bash
mkdir DIRECTORY   # Create a directory
mkdir dir1/dir2/dir3    # throw an ERROR 
mkdir -p dir1/dir2/dir3 # that's fine
```


##### Copy files & directories
```Bash
cp file1 file2    # make file2 as a cpoy of file1
cp filel /root/Desktop/
cp filel file2 file3 /root/Desktop/  # the last argument must be a directory
cp -r /etc /dirl     # copy non empty directory (the folder itself and its content) 
cp -r /etc/* /dirl   # copy the content of /etc directory into dir1
```

##### Move files & directories
```bash
mv file1 new_file1     # rename the file
mv file1 file2 file3 /root/Documents/  # the last argument must be a directory
mv dir1 dir2 dir3 dir4    # the last argument must be a directory
```

##### Remove files & Directories
```Bash
rm file      # Remove a file
rm -r dir1   # remove non empty directory
# if you are the root, each rm command you will be asked to remove..so use -f
rm -f dir1   # remove this empty dir. without asking
rm -rf dir2  # remove this non empty dir. without asking
rm -rf *     # remove all contents of the current directory without asking 
rm -rf /     # remove all contents of the system without asking, this command is a very critical command, the system will be gone even without asking
rmdir directory   # Remove an empty directory
```



#### Create hard links and soft links.

A **link** in Linux is a **pointer to a file or Directory**. 
Like pointers in any programming languages, links in Linux are pointers pointing to a file or a directory. 
**Creating links is a kind of shortcuts to access a file**. Links allow more than one file name to refer to same file, elsewhere.

<img width="782" height="272" alt="Pasted image 20250816053733" src="https://github.com/user-attachments/assets/9077e77d-ecf1-4d6c-b25a-abbcc97631d8" />

##### Soft Links
- very small size compared to the original file  
- if the original file is deleted, the Soft Link will be useless 
- the Soft Link has a different **Inode** than original file   
##### Hard Link 
- Has the same size as the original file
- if the original file is deleted, the Hard Link won't be affected 
- Has the same **Inode** as the original file 


##### What is Inode (Index Node)
Linux allocate an index node (inode) for every file and directory in filesystem. 
An **inode (index node)** is a **data structure on a filesystem** that stores metadata about a file or directory.  It is **not the file itself** and **not just a number**.

Inode is Unique Identifier for the file in the filesystem.
**Inodes** **do not store actual data**.

each file or directory has an **inode entry** in the **Inode table**. 
each **Inode entry** has: 
- **Metadata** 
- **Data pointer** (pointers to data blocks (where the file’s content is actually stored)

the following **Metadata** exists in an inode:
- #### File type
- #### Permissions
- #### Hard links count
- #### Owner ID
- #### Group ID
- #### Size of file
- #### Time stamp (access time)
- #### Time stamp (modification time )
- #### Soft/Hard Links
- #### Access Control List (ACLs)
- #### Pointers to data blocks (the actual disk locations where file content is stored)

<img width="999" height="344" alt="redhat5" src="https://github.com/user-attachments/assets/2cf41ea1-f03d-4252-98d5-326c22f727c2" />


##### File System Structure (especially for ext-family like ext2/ext3/ext4)
- The filesystem splits the disk into **block groups** to reduce disk head movement (for HDDs) and improve performance.
- Each block group is like a “mini filesystem” with its own **bitmaps, inode table, and data blocks**.

```mathematica
Disk / Partition
 ├── Boot sector
 ├── Superblock
 ├── Block Group 0
 │    ├─ Inode Bitmap
 │    ├─ Block Bitmap
 │    ├─ Inode Table  <── here, you find the Inode Entries
 │    └─ Data Blocks
 ├── Block Group 1
 │    ├─ Inode Bitmap
 │    ├─ Block Bitmap
 │    ├─ Inode Table
 │    └─ Data Blocks
 ├── Block Group 2
 │    ├─ Inode Bitmap
 │    ├─ Block Bitmap
 │    ├─ Inode Table
 │    └─ Data Blocks
 └── ...
```

###### Boot Sector
**contains initial boot code or reserved space for boot loaders.**
- It is The very first sector(s) of a storage device or partition.
- In a **disk with an OS**, it may contain:
    - **Boot loader code** (small program that starts the OS loading process).
    - Partition table (if it’s the MBR).
- In a **filesystem context** (like ext4), the `boot sector` of the partition is usually just reserved space; some filesystems don’t actually use it except for boot loaders.

###### Superblock
**master record of the filesystem — tells the OS how to interpret and navigate the whole disk structure.**
- It is a critical data structure at the start of each filesystem.
- Contains **global metadata about the entire filesystem**, such as:
    - Filesystem type (ext4, xfs, etc.)
    - Size of the filesystem (total number of blocks/inodes)
    - Size of each block    
    - Number of free/used blocks and inodes
    - Location of the inode table and block bitmaps        
    - Mount status (clean or dirty shutdown)

###### Inode Bitmap
**tracks allocation status of inodes.**
- A bitmap = a sequence of bits (0s and 1s).
- Each bit represents whether a given **inode** is free or in use.
    - `0` → free inode (available for a new file).
    - `1` → allocated inode (belongs to some file/directory).



###### Block Bitmap
**Tracks allocation status of data blocks.**
- Same idea of Inode Bitmap, but for **data blocks**.
- Each bit represents whether a **data block** is free or used.
    - `0` → free block.
    - `1` → block is occupied by some file’s content.


###### Inode Table
 **Holds metadata + block pointers for every file/directory.**
- This is the actual table (array) where all **inode entries** live.
- Each inode entry contains:
    - File type, size, ownership, timestamps, permissions.
    - Pointers to data blocks (direct/indirect).


###### Data Blocks
**Store the actual content (file data or directory entries).**
- The real storage space of the disk for file content.
- For directories, the data blocks store the **list of filenames → inode numbers** mapping.










##### Create Hard/Soft Links
- `ln` command: create a **new hard link** (another name) that points to an existing file.
- `In -s` command: create a **new hard link** (another name) that points to an existing file or directory.
<img width="1260" height="538" alt="radhat" src="https://github.com/user-attachments/assets/eebe1942-b67f-42ae-b32a-7dd27a52719d" />


<img width="1258" height="480" alt="redhat2" src="https://github.com/user-attachments/assets/23d478b9-bc6a-4cf0-9280-ce5114c49435" />


<img width="1225" height="867" alt="redhat4" src="https://github.com/user-attachments/assets/040a5dd9-c413-4ed4-b90d-e29aed69f052" />





Notes
- **soft links can link both a file or directory**, 
  **hard links can only link to a file, not directory.**

<img width="1236" height="451" alt="redhat3" src="https://github.com/user-attachments/assets/b2679bae-11a3-42fc-a065-f7c031dfae63" />

  
- Soft Links can be created across filesystems, Hard links can't be created outside the filesystem. 
<img width="1225" height="867" alt="redhat4" src="https://github.com/user-attachments/assets/484d168a-d32a-4267-b17d-e27966db7e34" />

	Notes
	- df command  ==> Disk Free, It shows disk space usage of mounted filesystems.
	- common options:
		- df -h     # human-readable (KB, MB, GB).
		- df -T     # also show filesystem type (ext4, xfs, tmpfs).    
		- df -i     # show inode usage instead of disk space (useful for inode exhaustion).

#### Efficiently run commands affecting many files by using pattern matching features of the Bash shell.

##### Regular Expression (Regex)

| **Pattern**                   | **Matches**                                                                                                    |
| ----------------------------- | -------------------------------------------------------------------------------------------------------------- |
| *****                         | Any string ot zero or more characters.                                                                         |
| **?**                         | Any single character.                                                                                          |
| **\[abc]**                    | Any one character in the enclosed class (between the square brackets).                                         |
| **\[!abc...]**                | Any one character not in the enclosed class.                                                                   |
| **\[^abc...]<br>ex: \[^a-c]** | Any one character not in the enclosed class.                                                                   |
| **\[[:alpha:]]**              | Any alphabetic character.                                                                                      |
| **\[[:lower:]]**              | Any lowercase character.                                                                                       |
| **\[[:upper:]]**              | Any uppercase character.                                                                                       |
| **\[[:alnum:]]**              | Any alphabetic character or digit.                                                                             |
| **\[[:punct:]]**              | Any printable character not a space or alphanumeric.                                                           |
| **\[[:digit:]]**              | Any Single digit from O to 9.                                                                                  |
| **\[[:space:]]**              | Any single white space character. This may include tabs. newlines.<br>carriage returns. form feeds. or spaces. |


##### grep command

```Bash
-l   # if the pattern is matched, disply the filename 
-i   # case insensitive
-w   # search with the exact word 
-n   # number the lines
-A3  # Display 3 lines after the regular expression match. 
-B3  # Display 3 lines before the regular expression match.
-r   # recursive
-v   # reverse
-e   # Used for multiple search atterns
```


##### cut & tr command
**cut command**: used for **cutting out the sections** **from each line of files** and writing the result to standard output.

**tr command**: is a utility for **translating or deleting characters**. It supports a range of transformations including uppercase to lowercase.

```Bash
echo hello world | tr a-z AZ
echo hello world | tr [a-z] [A-Z]
echo hello world | tr [:lower:] [:upper:]
echo "Welcome To Linux" | tr '\t'
echo "Welcome To Linux" | tr -d 'W'
echo "my ID is 73535" | tr -d [:digit:]
```




