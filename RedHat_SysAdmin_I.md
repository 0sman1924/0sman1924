
## Quick access

[CH03_Managing Files From the Command Line](#ch03_managing-files-from-the-command-line)

<br>
<br>
<br>

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


<br>


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

<br>

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
<br>

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

<br>

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

<br>


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

<br>

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


<br>
<br>
<br>


### CH04_Getting Help in Red Hat Enterprise Linux

**One source of documentation** that is generally available **on the local system** are system manual pages or man pages.
These pages are **shipped as part of the software packages** for which they provide documentation, and **can be accessed** from the command line by using the **man** command.

<br>

#### Common Sections of the Linux Manual
---
> **NOTE!**: the 1, 5, 8 are the most frequented section for system administrators 

| SECTION | CONTENT                                                             |
| ------- | ------------------------------------------------------------------- |
| **1**   | User commands (both executable and shell programs)                  |
| 2       | System calls (kernel routines invoked from user space)              |
| 3       | Library functions (provided by program libraries)                   |
| 4       | Special files (such as device files)                                |
| **5**   | File formats (for many configuration files and structures)          |
| 6       | Games (historical section for amusing programs)                     |
| 7       | Conventions, standards, and miscellaneous (protocols, file systems) |
| **8**   | System administration and privileged commands (maintenance tasks)   |
| 9       | Linux kernel API (internal kernel calls)                            |

<br>

#### manual page sections
---

| HEADING         | DESCRIPTION                                                            |
| --------------- | ---------------------------------------------------------------------- |
| **NAME**        | Subject name. Usually a command or file name. Very brief description.  |
| **SYNOPSIS**    | Summary of the command syntax.                                         |
| **DESCRIPTION** | In-depth description to provide a basic understanding of the topic.    |
| **OPTIONS**     | Explanation of the command execution options.                          |
| **EXAMPLES**    | Examples Of how to use the command, function, or file.                 |
| **FILES**       | A list of files and directories related to the man page.               |
| **SEE ALSO**    | Related information. normally other man page topics.                   |
| **BUGS**        | Known bugs in the software.                                            |
| **AUTHOR**      | Information about who has contributed to the development of the topic. |

<br>

#### Notes
---
- to search for a pattern in Manual Page: `man -K "PATTERN"`
- **which** & **whereis** command are useful to get the man page locations of any binary 
- **--help** option: get less info than manual page. ex: `ls --help`

<br>

#### Another documentations to get help:
---
- RadHat online docs ==>  https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/
- [The Linux Documentation Project](https://tldp.org/)
- Documentations in `/usr/share/doc`, recommended to be opened in a browser. 


<br>
<br>
<br>


### CH05_Creating, Viewing, and Editing Text Files

Save command output or errors to a file with shell redirection, and process command output
through multiple command-line programs with pipes.

Create and edit text files using the Vim editor.

use shell variables to help run commands. and edit Bash startup scripts to set shell and environment variables to modify the behavior of the shell and programs run from the shell.

<br>

#### Input Output Redirection

<img width="738" height="487" alt="Pasted image 20250816141254" src="https://github.com/user-attachments/assets/7ae7f4d4-f665-4154-b99a-dba5b6f2f9f6" />

<img width="1079" height="433" alt="Pasted image 20250816141403" src="https://github.com/user-attachments/assets/6e8c882a-903f-42fb-a49c-9b43f001c4ca" />


##### some examples for redirection

```Bash
date > date.txt
cal >> date.txt
ls sdfghjdfgh 2>errors.txt 
find / -name passwd 1>output.txt 2>/dev/null 
who >> users.txt
who >> users.txt | cat users.txt | wc -l >> users_numbers.txt | cat users_numbers.txt
```

<br>

#### Constructing Pipelines 

<img width="960" height="239" alt="Pasted image 20250816145422" src="https://github.com/user-attachments/assets/2c7f4e9a-3526-4342-b86e-e976cb9f1d0b" />

<img width="868" height="427" alt="Pasted image 20250816150037" src="https://github.com/user-attachments/assets/fb4798a6-3a3b-4400-8bec-138ca96e0a01" />


##### the Difference between Redirection & Pipeline 

- **Redirection** ==> save the output into a file
- **Pipelines**     ==> the output is as input to the next process 

##### some examples for pipelines and redirection

```Bash
ls -lR / 2>/dev/null | wc -l  # get No. of files in the system
ls -lR /etc/ | wc -l    # get No. of all files under /etc 
ls -l /etc/ | wc -l           
			# get No. of files & folders under /etc (not all files in /etc) 
ls -l /usr/bin/ | wc -l       # get No. of binaries in your system
ls -l /usr/bin/ | less        # navigate the outputas pages 
ls -ltr /etc | tail -n 10 > output.txt     
			# get the latest modified 10 files or folders in /etc
```

<br>

#### VIM text editor

<img width="1085" height="584" alt="Pasted image 20250816153511" src="https://github.com/user-attachments/assets/accc618c-380f-4040-9d4f-70017870e188" />


##### Vim operating modes

<img width="666" height="425" alt="Pasted image 20250816153407" src="https://github.com/user-attachments/assets/d59e9bdd-6b93-4781-a445-6c1118e8a2a9" />



##### Command Mode 
```Bash
yy     # copy a line
3yy    # Copy three lines.                            
dd     # Cut a line                                   
p      # Paste after the current cursor.              
P      # Paste before the current cursor.(Upper case) 
/      # To search forward                            
?      # To search backward                           
n      # Find the next match
N      # Find the previous match   
ZZ     # save & exit
```


##### Insert Mode 
```Bash
i     # insert (switches vim to insert mode)
a     # append (moves the cursor after the current character and enters insert mode)

I     # moves the cursor to the beginning of the line and enters insert mode 
A     # moves the cursor to the end of the line and enters insert mod
o     # inserts a new line below the current line and enters insert mode on the new line
O     # inserts a new line above the current line and enters insert mode on the new line
```


##### Extended Mode

###### Saving Files
```Bash
:wq    # Save and quit the current file.
:x     # Save the current file if there are unsaved changes, then quit
:w     # Save the current file and remain in editor.
:q     # Quit the current file (only if there are no unsaved changes).
:q!    # Quit the current file, ignoring any unsaved changes.
:10    # Jump to line number 10.
```
###### Search & Replace
```Bash 
# this syntax for replacing every occurence of a string in the entire text
:%s/old/new/g   
```

###### Execute a command in vim
```Bash
:.!date   # execute date command at the current cursor position
:3!date   # execute date command at the third line
```

###### Vim options
```Bash
:set number    # enables line numbers
:set nonumber  # disables line numbers
```



##### Visual Mode
The visually identified text can be deleted, copied, changed, or modified with any other Vim
editing command.

###### There are 3 keystrokes:
- Character mode: **v**  ---> **VISUAL** will appear at the **bottom** of the screen.
- Line mode: **Shift+V** ---> **VISUAL LINE** will appear at the **bottom** of the screen.
- Block mode: **Ctrl+V** ---> **VISUAL BLOCK** will appear at the **bottom** of the screen.

###### Visual mode options 
```Bash 
d            # delete the text 
y            # copy the text
u            # undo and start again
. or ctrl+r  # redo  
p            # paste the text 
c            # change the highlighted text. 
# The highlighted text will disappear,and you will be in Insert mode where you can add new text
```


##### Vim Cheat sheet

- [Vim Cheat Sheet](https://vim.rtorr.com/)
- [Vim cheatsheet](https://devhints.io/vim)
- [VIM Cheatsheet - Interactive Guide to VIM Commands](https://www.vim.ninja/cheatsheet)
- [A Great Vim Cheat Sheet](https://vimsheet.com/)

<img width="713" height="437" alt="Pasted image 20250817044854" src="https://github.com/user-attachments/assets/fd31bbd7-0772-48bf-baea-77f1eefd56e9" />



<br>

#### Variables in Linux
- ##### User Defined Variables
- ##### Shell Variables

##### User Defined Variables 

$VariableName = VALUE$

```Bash
[user@host~]$ x = 15 
[user@host~]$ echo $x 

[user@host~]$ y = 10
[user@host~]$ echo "x + y = $[x + y]"
25

[user@host~]$ first_name="mahmoud" 
[user@host~]$ last_name="abdelnaby"
[user@host~]$ echo "Hello ${first_name} ${last_name}"
Hello mahmoud abdelnaby

```

**Note**:
- if you set a variable in a shell, then you run a child shell,,,, this will be the result:  

<img width="1113" height="240" alt="redhat00" src="https://github.com/user-attachments/assets/4a48cac4-9413-4e8d-b5c0-fe84dcaf6d2a" />


- so, sure to export the variable

<img width="1138" height="368" alt="redhat10" src="https://github.com/user-attachments/assets/57fc1352-1c85-416d-bd86-edb3766035ca" />


##### Shell Variables
run **set** command to show all shell variables: `set | less`

<img width="1128" height="580" alt="Pasted image 20250817074506" src="https://github.com/user-attachments/assets/65d56169-f2be-49aa-bde8-c35483082fe0" />


for example, we edit **PS1** variable. **PS1** variable has the shell format, all editing reset when start a new shell. 

<img width="1130" height="395" alt="redhat8" src="https://github.com/user-attachments/assets/d6f13738-3c2e-4a51-a13a-387f0bdf3772" />


###### **PATH** variable 
Contains a list of colon separated directories that contain programs:

if we have a script execute some commands like this:
```Vim
date

cal

whoami
hostname
pwd
```

<img width="1266" height="745" alt="Pasted image 20250817083041" src="https://github.com/user-attachments/assets/88da68e4-3bae-4b94-8bfc-e8ab94160c66" />


<img width="1249" height="694" alt="Pasted image 20250817083534" src="https://github.com/user-attachments/assets/71bf01b1-1161-4ce5-82d3-9363231d5b11" />


#####  remove or unexport a variable

- **unset** command
<img width="687" height="332" alt="Pasted image 20250817084215" src="https://github.com/user-attachments/assets/1d52f69e-d961-461d-8f91-24d730c84615" />


- **unexport** command: `export -n <VAR_NAME>`

##### Setting Variables Automatically
- You can edit the Bash startup scripts.
- These exact scripts that run depend on how the shell was started, whether it is an interactive login shell, an interactive non-login shell, or a shell script.
- Assuming the default **/etc/profile**, **/etc/bashrc**, and **~/.bash_profile** files, 
  if you want to make a change to your user account that affects all your interactive shell prompts at startup, edit your **~/.bashrc** file.
- For example, you could set that account's default editor to nano by editing the file to read:
```Bash 
# .bashrc
# Source global definitions

if [ -f /etc/bashrc ]; then
  . /etc/bashrc
fi

# User specific environment
PATH=$HOME/.local/bin:$HOME/bin:$PATH "
export PATH
```


<br>
<br>
<br>


### CH06_Managing Local Users and Groups (DONE)

- There are three main types of user account: the **superuser**, **system users**, and **regular users**.
- A user **must have a primary group** and may be a member of **one or more supplementary groups**. 
- The three critical files containing user and group information are 
  **/etc/passwd**, **/etc/group**, and **/etc/shadow**.
- The **su** and sudo commands can be used to run commands as the superuser.
- The **useradd**, **usermod**, and **userdel** commands can be used to manage users. 
- The **groupadd**, **groupmod**, and **groupdel** commands can be used to manage groups.
- The **chage** command can be used to configure and view password expiration settings for users.

<br>

#### User Identifier UID
**/etc/login.defs**
- use **id** to show user ID

to show all users on the system, cat this file **/etc/passwd** 
```Bash
# the Formula of /etc/passwd lines 
username:Encrypted_password:UID:GID:real_name:home_directory:available_shell

# examples:
root:x:0:0:root:/root:/bin/bash

#   root ---> username
#   x ---> a symbol for encrypted password kept in /etc/shaodw file 
#   0 ---> UID
#   0 ---> GID
#   root ---> real username
#   /root ---> home directory for this user  
#   /bin/bash ---> The default shell program for this user, which runs on login
```

>**Note!**
>A system user might use **/sbin/nologin** if interactive logins are not allowed for that user.


<br>

#### Group ID GID
- Groups can be used to grant access to files to a set of users instead of just a single user.
- Internally, the system distinguishes groups by the unique identification number assigned to them, the group ID or GID.
- The mapping of group names to GIDs is defined in databases of group account information.
- By default, systems use the **/etc/group** file to store information about local groups.

<img width="905" height="266" alt="Pasted image 20250818210133" src="https://github.com/user-attachments/assets/bb39c51a-a610-44cd-a35d-201e0c8b58c6" />


##### Primary Groups & Supplementary Groups
- When you create a new regular user, a new group with the same name as that user is created. That group is used **as the primary group for the new user**, and that user is the only member of this User Private Group.
- Users may also have supplementary groups. Membership in supplementary groups is determined by the **/etc/group** file. Users are granted access to files based on whether any of their groups have access.
- It doesn't matter if the group or groups that have access are primary or supplementary for the user.

For example, if the user user01 has:
- a primary group user01 
- supplementary groups **wheel** and **webadmin**, 
then that user can read files readable by any of those three groups.

to switch between users, use **su** command: `su username`  **OR**  `su - username`

- `su username` : Switches to **that user**. 
  But it **keeps the current environment** (your PATH, variables, shell, etc.).
  You’re just switching the effective UID, not loading a full login shell.

- `su - username` :    Switches to **that user** **AND** starts a _login shell_ (**logging in as that user**)
  Loads the user’s environment (`~/.bash_profile`, `~/.bashrc`, etc.),
  Sets `HOME` to that user’s home directory,
  Updates PATH to that user’s default,

```Bash
# some examples:  

su root     # OR (su  ) without username, it dierctly switchs to root user without root's environment(root's PATH, variables, shell, etc.)

su - root   # switches to that user AND starts a login shell as root with all root's setup and environment
 
```


<img width="776" height="177" alt="Pasted image 20250821163929" src="https://github.com/user-attachments/assets/86f2c96c-ba94-4051-9a82-bc9281a38362" />


<img width="786" height="179" alt="redhat_1" src="https://github.com/user-attachments/assets/a9d73450-761e-41dd-97a6-20d846554bff" />


<br>

#### SUDO permissions 

In some cases, the root user's account **may not have a valid password** at all for security reasons.
So, **users cannot log in** to the system **as root directly with a password**, and **su** cannot be used to get an interactive shell. 
One tool that can be used to get root access in this case is **sudo**.

Unlike su, **sudo** normally requires users to **enter their own password for authentication**, not the password of the user account they are trying to access. 
That is, users who use sudo to run commands as root do not need to know the root password. Instead, they use their own passwords to authenticate access.


> NOTE!!
  If a user tries to run a command as another user, and the sudo configuration does not permit it, the
  command will be blocked, the attempt will be logged, and by default an email will be sent to the root user.



For example, 
- when sudo is configured to allow a user to run the command **useradd** as root, this user could run the following command to create a new user account:
<img width="1205" height="420" alt="Pasted image 20250821173127" src="https://github.com/user-attachments/assets/ad8c31cf-3be4-4c20-988a-e9255fb46915" />



<br>

#### Grant Super user access 

this section illustrates how we set a user to run commands as root using sudo. 
- The **main configuration** file for **sudo** is **/etc/sudoers**. 
- To avoid problems if multiple administrators try to edit it at the same time, it should only be edited with the special **visudo** command.

- the following figure from the **/etc/sudoers** file enables sudo access for members of group wheel.
<img width="1114" height="528" alt="red2 1" src="https://github.com/user-attachments/assets/2685cf0d-b58d-472d-a0f8-53a65fc074d0" />



```Mathematica 
root    ALL=(ALL)     ALL 
  │      │    │        │   
  │      │    │        └─────> specify command list (ALL = any commands)  
  │      │    └─────> specify Run-as list  (ALL = run as any user)        
  │      └─────> specify host that the user can run the commands on
  └─────> username, if we set for a group naming wheelas an ex. --> %wheel 
```

##### Examples
###### Restrict to specific host
```Bash 
ramy    server1=(root)   /usr/bin/systemctl restart nginx

# On host myserver1 only, ramy can restart nginx as root:
sudo systemctl restart nginx
```


###### Restrict run-as user 
```Bash
khaled ALL=(postgres)  /usr/bin/psql

# khaled can only run /usr/bin/psql as the postgres user.
sudo -u postgres psql 
```

###### Multiple commands, root only
```Bash 
ahmed  ALL=(root)  /usr/bin/systemctl restart httpd, /usr/bin/systemctl status httpd

# ahmed can restart Apache and check its status, but nothing else.
sudo systemctl restart httpd
sudo systemctl status httpd
```


###### Different hosts with different permissions
```Bash 
david   webserver=(root)       /usr/bin/systemctl restart nginx
david   dbserver=(postgres)    /usr/bin/psql

# On webserver, david can restart nginx as root.  
# On dbserver, david can run psql as postgres.
systemctl restart nginx  # on webserver host
sudo -u postgres psql    # on dbserver host
```


###### No password required (NOPASSWD)
```Bash
omar  ALL=(root) NOPASSWD: /usr/bin/systemctl restart sshd 

# omar can restart sshd without being asked for a password.
sudo systemctl restart sshd
```


###### Group with limited command on a specific host
```Bash
%webadmins  webserver=(root)  /usr/bin/systemctl restart nginx

# any user in group webadmins can restart nginx as root, but only on webserver.
sudo systemctl restart nginx  # for any user in webadmins group
```



###### Group running as another user (not root)
```Bash
%dba  ALL=(postgres)  /usr/bin/psql
#  Any user in group dba can run psql as the postgres user.
sudo -u postgres psql    # for any user in dba group  
```


###### Multiple allowed commands
```Bash
%devops  ALL=(root)  /usr/bin/systemctl restart httpd, /usr/bin/systemctl status httpd
# Anyone in devops can restart Apache and check its status.
```


###### Different rules for the same group on different hosts
```Bash
%ops  webserver=(root)  /usr/bin/systemctl restart nginx
%ops  dbserver=(postgres)  /usr/bin/psql
# Members of ops group can restart nginx on the webserver,  and connect to PostgreSQL as postgres on the dbserver.
```


###### NOPASSWD with a group
```Bash
%netadmins  ALL=(root) NOPASSWD: /usr/bin/systemctl restart sshd
# Members of netadmins group can restart sshd without entering a password.
```

<br>

#### CRUD operations for users

##### create a user (useradd)
```Bash
# Create a new user osman
sudo useradd osman

# Create a user with home directory and default shell
sudo useradd -m -s /bin/bash ali
```

In creating the account for osman, the useradd command performs several actions: 
- Reads the **/etc/login.defs** and **/etc/default/useradd** files to **get default values** to use when creating accounts. 
- **Checks command-line parameters** to find out which default values to override. 
- **Creates** a new user **entry** in the **/etc/passwd** and **/etc/shadow** files based on the default values and command-line parameters. 
- **Creates any new group entries** in the /etc/group file. 
  (Fedora creates a group using the new user's name.) 
- **Creates a home directory** based on the user's name in the /home directory. 
- **Copies any files** located within the **/etc/skel** directory **to the new home directory**. 
  This usually includes login and application startup scripts.




> **Note!** 
> **-p passwd**: Enter a password for the account you are adding. This must be an encrypted password.
> Instead of adding an encrypted password here, you can simply use the passwd user command later to add a password for user. (**To generate** an **encrypted MD5** password, type **openssl passwd**.)

##### Read (list/check user info) (id) 
```Bash
# Check user details
id osman

# Show login info from /etc/passwd
grep osman /etc/passwd
```

##### Update/Modify a user (usermod)
```Bash
# Change username
sudo usermod -l NEWNAME osman

# Change user’s home directory
sudo usermod -d NEW_HOME_DIR -m ali

# Add user to a group
sudo usermod -aG wheel ali
```

##### Delete a user (userdel)
```Bash
# Delete user but keep home directory
sudo userdel osman

# Delete user and remove home directory
sudo userdel -r ali
```

**NOTES:**
- Keep in mind that **simply removing** the user account **does not change anything about the files**
  that user **leaves** around the system (except those that are deleted when you use -r). 
  However, ownership of files left behind appear as **belonging to the previous owner’s user ID** number when you run ls -l on the files.
- Because **files that are not assigned to any username** are **considered** to be **a security risk**, 
  it is a good idea to find those files and assign them to a real user account.

to find all files in the filesystem that are not associated with any user (the files are listed by UID):
```Bash 
find / -nouser -ls 
```

<br>

#### CRUD operations for groups
##### Create a group (groupadd) 
```Bash
# Create a new group devops
sudo groupadd devops
```

##### Read (list/check group info) (getent) 
```Bash
# Show group details
getent group devops

# Show all groups
getent group
```

##### Update a group (groupmod)
```Bash
# Change group name
sudo groupmod -n engineers devops

# Change group ID
sudo groupmod -g 505 engineers

# Add user ali to group engineers
sudo usermod -aG engineers ali
```

##### Delete a group
```Bash
# Remove a group
sudo groupdel engineers
```


<br>

#### managing user passwords
```Bash
# this is a line scheme in /etc/shadow file
username:password:lastchg:min:max:warn:inactive:expire:reserved

# example: 
osman:$6$abcdefg123456$Q8HzW9a...3w9X9/:19700:7:90:14:30:20000:
```

- **osman** → Username
- \$6\$abcdefg123456\$Q8HzW9a...3w9X9/ → Encrypted password (SHA-512 in this case)
- **19700** → Password last changed on day 19700 since 1970-01-01 (~2023-12-29)
- **7** → User must wait at least 7 days before changing password again
- **90** → Password expires after 90 days
- **14** → User warned 14 days before password expires
- **30** → Password still works for 30 days after expiration before account is locked
- **20000** → Account expires on day 20000 (~2024-10-11)
- **(empty)** → Reserved for future use

##### configure password aging
The following diagram relates the relevant password aging parameters, which can be
adjusted using the **chage** command to implement a password aging policy

<img width="751" height="284" alt="Pasted image 20250820005156" src="https://github.com/user-attachments/assets/837ce2cf-e7a0-41a4-8737-7359b775e0bb" />

<br>

#### Restricting Access to User Accounts
There are situations where a user account should exist on the system but should **not be allowed to log in interactively**. RHEL provides two common ways to restrict access:
- ##### Locking an Account
- ##### Using the _nologin_ Shell

##### Locking an Account

```Bash 
usermod -L USERNAME
# This prevents the user from logging in with their password, but the account itself still exists (useful for temporary disabling).

usermod -U USERNAME  # to unlock this user
```

##### Using the _nologin_ Shell

The **`/sbin/nologin`** shell acts as a replacement shell for accounts not intended to log in interactively.
- Best practice for accounts that serve system roles but don’t need interactive login (e.g., mail server accounts).
- When the user attempts to log in, the **connection is closed immediately**.

For example:

<img width="1010" height="545" alt="Pasted image 20250821205303" src="https://github.com/user-attachments/assets/c682d0be-f76b-4ef6-ae8c-0f300e47c592" />


<br>
<br>
<br>


### CH07_ Controlling Access to Files (DONE)

Set Linux file-system permissions on files and to interpret the security effects of different permission settings.

<br>

#### Linux File-system Permission
- File permissions control access to files. The Linux file permissions system is simple but flexible, which makes it easy to understand and apply, yet still able to handle most normal permission cases easily.
- Files have three categories of user to which permissions apply. The file is owned by a user, normally the one who created the file. The file is also owned by a single group, usually the primary group of the user who created the file, but this can be changed.
- Different permissions can be set for the owning user, the owning group, and for all other users on the system that are not the user or a member of the owning group. 
- Three categories of permissions apply: read, write, and execute.

##### Permissions on Files & Directories

| PERMISSION  | EFFECTS ON **FILES**                 | EFFECTS ON **DIRECTORIES**                                                                                                                                                |
| :---------: | ------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  r (read)   | Contents of the file can be read.    | Contents of the directory (the file<br>names) can be listed.                                                                                                              |
|  w (write)  | Contents of the file can be changed. | Any file in the directory can be created<br>or deleted.                                                                                                                   |
| x (execute) | Files can be executed as commands.   | Contents of the directory can be accessed. (You can change into the directory, read information about its files, and access its files if the files permissions allow it.) |


##### View file & Directory permissions and ownership

use this command: `ls -ld` 

<img width="857" height="208" alt="Pasted image 20250820130634" src="https://github.com/user-attachments/assets/cf06d144-3da4-495f-980c-268a405b1025" />

- Each file has an owner
- Each file is assigned to a group
- Permissions can only be changed by the owner and root

<br>

#### change permissions of files & Directories

##### Symbolic Method
---
**Examples**

```Bash
chmod g+w filel
chmod o+w filel
chmod u-w filel
chmod u+w,g+wx,0+r filel
chmod go-rw filel
chmod u=rw,g=r,o=r filel
chmod a+x filel   # OR chmod ugo=x file1 
chmod a=rw filel  # OR chmod ugo=rw file1
chmod u= filel    # remove all permissions from owner
chmod a=- filel    # OR chmod a= file1 (remove all permissions from all)
chmod +rw filel
chmod =rw filel   # OR chmod a=rw file1
chmod -R g+rwx dirl
chmod -R o+rwx dirl
chmod -R o=- dirl
```


##### Numeric Method
---

| DECIMAL | OCTAL | REPRESENTATION |
| :-----: | :---: | :------------: |
|  **0**  |  000  |    **---**     |
|  **1**  |  001  |    **--x**     |
|  **2**  |  010  |    **-w-**     |
|  **3**  |  011  |    **-wx**     |
|  **4**  |  100  |    **r--**     |
|  **5**  |  101  |    **r-x**     |
|  **6**  |  110  |    **rw-**     |
|  **7**  |  111  |    **rwx**     |

```mathematica 
## Example
chmod 745 file1   ## rwx r-- r-x
      ││└─────> others permissions = 5 = 101 = r-x
      │└──────> group permissions  = 4 = 100 = r--
      └───────> owner permissions  = 7 = 111 = rwx   
```


##### Requires Permissions for most common commands

| **Commands** | **Source Directory** | **Source File** | **Target Directory** |
| :----------: | :------------------: | :-------------: | :------------------: |
|      cd      |          x           |       N/A       |         N/A          |
|      ls      |        x , r         |       N/A       |         N/A          |
| mkdir, rmdir |        x , w         |       N/A       |         N/A          |
|  cat, less   |          x           |        r        |         N/A          |
|      cp      |          x           |        r        |        x , w         |
|    cp -r     |        x , r         |        r        |        x , w         |
|      mv      |        x , w         |    none !!!     |        x , w         |
|      vi      |        x , r         |      r , w      |         N/A          |
|      rm      |        x , w         |    none !!!     |         N/A          |

<br>


#### change file & directory ownership

- **Only root** can change the ownership of a file.
- **Root or the files owner** can change group ownership.
- we use **chown**, **chgrp** to change the ownership 
##### some Examples 

```Bash
# chown OWNER_NAME:GROUP_NAME file

chown mahmoud filel         # change file owner
chown mahmoud dirl          # change directory owner 
chown mahmoud:devops_team file1
chown :devops_team file1    # change group ownership for a file
chown -R mahmoud dirl       # change owner of a directory and its contents 
chown -R mahmoud:devops_team dirl # change user&group ownership for a directory and its content
```

<br>


#### Set Special Permissions

Special permissions provide additional access-related features over and above what the basic permission types allow. 
##### why set special permissions??
we pick an example to understand...
- when you change your password using **passwd** command, 
  the new password is updated into **/etc/shadow** file.
- **/etc/shadow** file is a critical file, which is under root control ONLY. 
  which meaning that, you haven't write permissions to update your password in this file
- So, **how** you **change** your password and **update the new password** into **/etc/shadow** file **Although** you **haven't write permissions** to update your password ?!

**passwd** command has set permission, which meaning the user running this command has the root permission to write into **/etc/shadow** file. 

##### effects of set special permissions for files & directories 

| SPECIAL PERMISSION | EFFECT ON FILES                                                                   | EFFECT ON DIRECTORIES                                                                                                                          |
| :----------------: | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
|   **u+s (suid)**   | File **executes as the user that owns the file**, not the user that run the file. | No effect.                                                                                                                                     |
|   **g+s (sgid)**   | File **executes as the group that owns the file**.                                | Files newly created in the directory have their group owner set to match the group owner of the directory.                                     |
|  **o+t (sticky)**  | No effect.                                                                        | Users with write access to the directory can only remove files that they own; they cannot remove or force saves to files owned by other users. |

##### setting special permissions
- **Symbolically**: 
	- setuid = u+s
	- setgid g+s
	- sticky = 0+t
- **Numerically** (fourth preceding digit): 
	- setuid = 4
	- setgid = 2
	- sticky = 1

```Bash 
chmod g+s directory    # add setgid for a directory
chmod 2770 directory   # add setgid for a directory with rwx permissions for the user and group with no access for others
```

<br>


#### Default Permissions
When you create a new file or directory, it is assigned initial permissions. 
There are two things that affect these initial permissions.
- The first is whether you are creating a regular file or a directory. 
- The second is the current umask.

##### umask command  

- **umask** (User File-Creation Mode Mask) defines the **default permissions** for newly created files and directories.
- It **removes** (masks) permissions from the system’s default permissions

###### Default Behavior
- By default, Linux assigns:
    - **Files** → `666` (read & write for everyone, no execute)
    - **Directories** → `777` (read, write, execute for everyone)
- Then the **umask value is subtracted (bitwise)** from these defaults.
- **Effective Permission = Default Permission - umask**
	 - Default file = `666`, umask = `022` → **644** (`rw-r--r--`).
	- Default dir = `777`, umask = `022` → **755** (`rwxr-xr-x`).

###### Notes
- The system's default umask values for Bash shell users are defined in the **/etc/profile** and **/etc/bashrc** files for services in **/etc/login.defs**
- **any change** for a user umask is a **temporary change**, all is reset to defaults after exit shell.
  to **change umask permanently**, you can override the system defaults in the **.bash_profile** and **.bashrc** files in their home directories.  
	- `echo "umask 007" >> ~/.bachrc  `, then logout and login again


###### common examples

| umask                           | File Default (666 - umask) | Dir Default (777 - umask) |
| :------------------------------ | :------------------------: | :-----------------------: |
| `0000`                          |     666 → `rw-rw-rw-`      |     777 → `rwxrwxrwx`     |
| `0022` (common)                 |     644 → `rw-r--r--`      |     755 → `rwxr-xr-x`     |
| `0002` (for collaborative dirs) |     664 → `rw-rw-r--`      |     775 → `rwxrwxr-x`     |
| `0077` (restrictive)            |     600 → `rw-------`      |     700 → `rwx------`     |

<br>
<br>
<br>

### CH08_Monitoring and Managing Linux Processes (DONE)

A process is a running instance of a launched, executable program.
Every new process is assigned a unique process ID (PID) for tracking and security.
The PID and the parent's process ID (PPID) are elements of the new process environment.
Any process may create a child process.

<br>

#### Listing Processes 
The **ps** command is used for **listing current processes**.
 It can provide detailed process information, including:
- **User identification (UID)**, which determines process privileges.
- Unique process identification (**PID**)
- **CPU and real time** already expended
- **How much memory** the process has allocated in various locations
- The location of process **stdout**, known as the **controlling terminal**
- The **current process state**

##### Viewing Processes with `ps`

Common Options for `ps`
- **ps aux**  → Displays **all processes**, including those without a controlling terminal.
- **ps lax**   → Provides **more technical detail** (extra columns).
- **ps -ef**   → Displays **all processes** (UNIX-style syntax, commonly used in scripts)
- **ps axo**  → lets you **customize output columns**.
  ex: `ps axo user,pid,ni,command`



Notes
- **ps** displays the process list **once**. so, If you need a **real-time updating view**, use: **top** command 
- you can view processes in **tree format** to see parent-child relationships using **pstree** command 
- **PID 1** → `systemd` (the very first process started by the kernel). 
  on older systems like **RHEL 6**, this was `initd`.

##### to display processes in tree format
```Bash
pstree       # process status tree
ps fax       # process status tree   
pstree -p    # Display PID of each process
pstree -p USERNAME     # Display PID of each process of this user 
pgrep -u USERNAME -l   # Show processes of this user
```


<br>

#### manage foreground & background processes

##### what is Control Jobs?
- Job control is a **shell feature** that allows a single shell instance to run and manage multiple commands.
- **Key rules of job control**:
    - Only **one job** at a time can read input and receive keyboard signals from a terminal,
       this is the **foreground job**.
    - Other jobs attached to the same terminal but not in the foreground are **background jobs**.
    - Background jobs **cannot read input or receive keyboard interrupts**, but they may write output to the terminal.
    - If a background job tries to read from the terminal, it is **automatically suspended**.
    - Each terminal session can have:
        - **1 foreground job**
        - **0 or more background jobs**

- **Foreground** jobs = **interact directly** with the terminal (can **take input**).
- **Background** jobs = **cannot take input**, but can run or write output.
	- to make a command in background directly, type **&** at the end of command
	  ex: `sleep 1000 &` 

##### Monitoring Jobs
```Bash
jobs   # to list jobs managed by the current shell  
ps j   # alternative view with process information
```

##### Moving Jobs Between Foreground and Background
```Bash
# to bring a background job → foreground
fg %JOB_ID  

# to Send a foreground job → background
ctrl+z       # suspend the running job
bg %JOB_ID   # resume it in the background 
```

<br>

#### Killing a Processes
**kill** command **sends a signal to a process by PID number**. 
Despite its name, the kill command can be used for sending any signal, not just those for terminating programs.
to list the names and numbers of all available signals: use the `kill -l` command 

```Bash
kill -l      # List all signals
  
pidof PROCESS_NAME    # get PID of a process     

kill PID   # Default is SIGTERM 15


pkill PROCESS_NAME

killall PROCESS_NAME   # terminate all processes of this process name

pkill -U user   # kill processes of specific user
```

##### NOTES!! (most common signals)
- `kill -15`  to terminate the process cleanly
- `kill -9`  aggressive termination (used in emergency termination but not recommended !)
- `kill -1`  (**SIGHUB**) to reload an re-read the configurations (configuration reload without termination)   
  OR use this command `systemctl SERVICE_NAME`


##### Logging Users Out Administratively 
You may need to log other users off for any of a variety of reasons. To name a few of the many possibilities:
- the user committed a **security violation**.
- the user may have **overused resources**. 
- the user may **have an unresponsive system**. 
- the user has **improper access to materials**.

In these cases, you may need to administratively terminate their session using signals.
Firstly,  **identify the PID numbers to be killed** using **pgrep**.
```Bash 
pgrep -l -u USERNAME
pkill -SIGKILL -u USERNAME
```
###### Note
- **pgrep** operates much like **pkill**, including using the same options, except that **pgrep** lists processes rather than killing them.
- **pkill**
	**kill processes by name or attribute** instead of by PID.
	It is part of the **procps** package (same family as `ps`, `top`, `pgrep`).
	- saves you from looking up PIDs manually with `ps` or `pgrep`.
	- Faster than `kill` because you don’t need the PID.
	- Useful for stopping multiple processes of the same program at once.
	- Often used in **system administration scripts** to manage daemons/services.

<br>

#### real-time processes monitoring
**top** program is a **dynamic view of the system's processes**.

```Bash
top
top -n 2  # Specifies the maximum number of iterations, or frames, top should produce before ending.
top -d 2  # Number of second between each update (the delay between screen updates)
```

> **Note!**
> **lsof** -->  Lists all opened files and processes accessing them in the provided directory.
> it's useful to check which processes are preventing successful unmounting.

##### common keystrokes in top (type the option during monitoring)

| TYPE THIS OPTION | DESCRIPTION                                                               |
| :--------------: | ------------------------------------------------------------------------- |
|      **1**       | to **show all cpu cores**                                                 |
|      **s**       | to **change the default refresh** rate which is 3 seconds                 |
|      **h**       | for **help**                                                              |
|      **k**       | to **kill a process**                                                     |
|      **r**       | to **renice a process**                                                   |
|      **M**       | to change the display to **sort by** the amount of **memory**             |
|      **P**       | to change the display to **sort by** the **CPU utilization**              |
|      **n**       | to change the **number of processes shown**                               |
|      **w**       | to **save current display configuration**                                 |
|      **q**       | to **quit**                                                               |
|      ? or H      | Help for interactive keystrokes.                                          |
|     L, T, M      | Toggles for load. threads. and memory header lines.                       |
|        B         | Enables use of bold in display, in the header. and for Running processes. |
|     Shift+H      | Toggle threads: show process summary or individual threads.               |
|    U, Shift+U    | Filter for any user name (effective, real).                               |

<br>

#### Linux process scheduling and multitasking
- Linux (and other operating systems) can run more processes is by employing a technique called **time-slicing**.
- The part of the Linux kernel that performs switching btw. processes is called the **process scheduler**. 
- There are exactly **40** different **levels of niceness** a process can have (**-20 to 19**). 
- By default, **processes** will **inherit their nice level from their parent**, **which is usuall 0**.
- **Higher** nice **levels** indicate **less priority**, while **lower** nice **levels** indicate a **higher priority**.
	- 20 ===> is the highest priority
	- 19 ===> is the lowest priority
- **Only root** is allowed to **set all nice level** on existing processes. 
- **Un privileged users** are only allowed to **set positive nice levels**, and they are only allowed to raise the nice level on their existing process but cannot lower them.
  ex: a regular user can set 5 to 7. But can't set 7 to 5 or any higher priority (6,5,4,3,2,....,-20)

- we can set priority for a process using **nice & renice** commands  
##### nice command 
- used to **start a process with a given priority**.
- Default is **0**.

example: 
```Bash
# nice COMMAND

nice vim text & # default 10,  
nice -n -20 vim text &  # -n to set priority  
```


##### renice command
- used to **change the priority of an already running process**.
- you can change priority from **top**, using **r** option.   

example:
```Bash 
# renice PRIORITY PID   

renice 19 1220   # set priority 19 for process with ID 1220
```


<br>
<br>
<br>


### CH09_Controlling Services and Daemons

<br>

#### Intro to systemd
- In **Red Hat Enterprise Linux**, the **first process that starts (PID 1)** is `systemd`.
- The `systemd` daemon is responsible for:
	- Managing system startup
    - Starting and supervising services
    - Activating system resources, daemons, and processes both at boot time and while the system is running

<br>

#### systemctl 
##### Listing Service Units
use the `systemctl` command to **explore the current state of the system**.
- `--all` → list all service units, including inactive ones
- `--state=` → filter results by **LOAD**, **ACTIVE**, or **SUB** fields

```Bash
# List active service units (default behavior)
systemctl list-units --type=service  

# List all active and inactive services
systemctl list-units --type=service --all  

# List only failed services
systemctl --failed --type=service
```


##### view service status

```Bash 
systemctl status name.type    # check  the status of a specific unit

# example: Check SSH service status
systemctl status sshd.service   
##### SCREEEEN HERE
```

> **NOTE!!**: if the unit type is not specified, systemctl assumes service.



##### Verify the Status of a service 

The **systemctl** command provides methods for **verifying the specific status of a service**. 

```Bash
systemctl is-active sshd.service     # active or inactive 
systemctl is-enabled sshd.service    # enable or disable  
systemctl is-failed sshd.service     # active or failed
systemctl --failed --type=service    # to list all the failed units
```



##### list unit dependency
```Bash
# command displays a hierarchy mapping of dependencies to start the service unit.
The systemctl list-dependencies UNIT 

# For example
systemctl list-dependencies sshd.service
```

<br>

#### Service Unit Information

|    FIELD     | DESCRIPTION                                                                             |
| :----------: | --------------------------------------------------------------------------------------- |
|  **Loaded**  | Whether the service unit is **loaded into<br>memory.**                                  |
|  **Active**  | Whether the service unit **is running** and if so,<br>**how long** it has been running. |
| **Main PID** | The main process ID of the service. <br>including the command name.                     |
|  **Status**  | **Additional information** about the service.                                           |

<br>

#### Service state in the output of systemctl 

|     KEYWORD      | DESCRIPTION                                                             |
| :--------------: | ----------------------------------------------------------------------- |
|    **loaded**    | Unit configuration file has been processed.                             |
| **active (running)** | Running with one or more continuing processes.                          |
| **active (exited)**  | Successfully completed a one-time configuration.                        |
| **active (waiting)** | Running but waiting for an event.                                       |
|     **inactive**     | Not running.                                                            |
|     **enabled**      | Is started at boot time.                                                |
|     **disabled**     | Is not set to be started at time.                                       |
|      **static**      | Cannot be enabled. but may be started by an enabled unit automatically. |

<br>

#### Control system services

| COMMAND                          | TASK                                                                           |
| -------------------------------- | ------------------------------------------------------------------------------ |
| **systemctl status UNIT<br>**        | View detailed information about a unit state.                                  |
| **systemctl stop UNIT**              | Stop a service on a running system.                                            |
| **systemctl start UNIT**             | Start a service on a running system.                                           |
| **systemctl restart UNIT**           | Restart a service on a running system.                                         |
| **systemctl reload UNIT**            | Reload the configuration file of a running<br>service.                         |
| **systemctl mask UNIT**              | Completely disable a service from being<br>started. both manually and at boot. |
| **systemctl unmask UNIT**            | Make a masked service available.                                               |
| **systemctl enable UNIT**            | Configure a service to start at boot time.                                     |
| **systemctl disable UNIT**           | Disable a service from starting at boot time.                                  |
| **systemctl list-dependencies UNIT** | List units required and wanted by the specified unit.                          |





<br>
<br>
<br>


### CH10_Configuring and Securing SSH (DONE)

<br>

#### Access the Remote Command Line with SSH

**OpenSSH** is an open-source implementation of the **Secure Shell (SSH)** protocol, which is widely used in **Red Hat Enterprise Linux (RHEL)** systems.  
SSH provides **secure, encrypted communication** over insecure networks such as the internet.

##### How SSH Works (Key Points)
- SSH encrypts all data exchanged between systems, including passwords.
- The **first time** a user connects to a remote host via SSH, the **server’s public key** is stored in the file:  `~/.ssh/known_hosts`
- This helps protect against **man-in-the-middle (MITM)** attacks in future connections by verifying the server’s identity.


<img width="1155" height="330" alt="Pasted image 20250823201429" src="https://github.com/user-attachments/assets/f5ca3497-7cf8-4b9b-8e4a-2158983c0823" />

##### Secure shell examples

The following command initiates a **remote login session** using SSH:
```Bash
[user01@host ~]$ ssh user02@remotehost 
user02@remotehost's password: shadowman 
...output omitted... 
[user02@remotehost ~]$
```
- Connects as user `user02` to host `remotehost`.
- Prompts for the **remote user’s password**.
- Upon success, opens an **interactive shell** on the remote machine.



This command runs a **single command remotely** without opening a shell:
```Bash
[student@serverA ~]$ ssh student@serverB hostname
student@serverB's password: student
serverb.lab.example.com
```
- Connects to `serverB` as user `student`.
- Executes the `hostname` command.
- Displays the remote system’s hostname and exits.

<br>

#### Configure SSH Key-based Authentication

##### SSH Authentication Methods
1. **Password Authentication**
    - The most basic method.
    - User enters a password each time they connect.
    - Easy to use but less secure (vulnerable to brute-force attacks).
2. **SSH Key-Based Authentication**
    - Uses a **private–public key pair**.
    - No need to enter a password for every login.
    - More secure and recommended for production.
3. **Host-Based Authentication**
    - The client host is trusted by the server.
    - Requires configuration of `/etc/ssh/ssh_known_hosts` and server trust.
    - Mostly used in controlled environments (e.g., clusters).
4. **Keyboard-Interactive Authentication**
    - Server prompts the user with one or more questions (like a password, OTP, or token).
    - Often used for **multi-factor authentication (MFA)**.
5. **GSSAPI Authentication** (Kerberos)
    - Uses Kerberos tickets for authentication.
    - Common in enterprise environments with centralized authentication (Active Directory, FreeIPA).

The two most common methods you’ll actually use in **RHEL administration** are:
- **Password Authentication**
- **SSH Key-Based Authentication**


##### SSH Key-Based Authentication
- Uses a **private–public key pair**.
- The **private key** stays securely on your machine.
- The **public key** is shared with the remote system.
- Authentication works when the server verifies that the client’s private key matches the stored public key.

##### How to generate SSH Keys
Use the **ssh-keygen** command to generate a key pair:
- By default, keys are stored in **/.ssh** folder in the home directory :
    - Private key → `~/.ssh/id_rsa`
    - Public key → `~/.ssh/id_rsa.pub`
- **Permissions**:
    - Private key (`id_rsa`) → `600` (only readable by the user)
    - Public key (`id_rsa.pub`) → `644` (readable by others if needed)
Correct permissions are essential, otherwise SSH may refuse to use the keys.


##### Sharing the Public Key
To enable key-based authentication, the **public key** must be copied to the **remote host**.  
The easiest way is with the `ssh-copy-id` command:

```Bash
[user@host ~]$ ssh-copy-id -i ~/.ssh/id_rsa.pub user@remotehost
```

- Copies the public key into the remote user’s `~/.ssh/authorized_keys` file.
- If the path to the public key is omitted, it defaults to `~/.ssh/id_rsa.pub`.

After this setup, you can log in without typing a password: 
```Bash
[user@host ~]$ ssh user@remotehost
```

<br>

#### Customizing open SSH service configuration

##### OpenSSH Service Configuration
- The OpenSSH service is provided by the **`sshd` daemon**.
- Its main configuration file is:  `/etc/ssh/sshd_config`

##### Prohibit Root Login via SSH
- **Best practice**: Disable direct root login from remote systems to reduce security risks.
- Controlled by the **`PermitRootLogin`** parameter in `/etc/ssh/sshd_config` :
    - `PermitRootLogin yes` → Allows root login (not recommended).
    - `PermitRootLogin no` → Denies root login.
- After making changes, reload the SSH service: `systemctl reload sshd`


##### Prohibit Password-Based Authentication
- Controlled by the **`PasswordAuthentication`** parameter in `/etc/ssh/sshd_config`:
    - `PasswordAuthentication yes` → Password logins allowed (default).
    - `PasswordAuthentication no` → Password logins disabled.
        - In this case, users must log in with **SSH key-based authentication**.
- After changing the file, reload the SSH service: `systemctl reload sshd`


> **NOTE!!**: Always test a new SSH session before logging out of your current one when making changes, to avoid accidentally locking yourself out.





<br>
<br>
<br>


### CH11_Analyzing and Storing Logs (DONE)


#### System Log Architecture

- Processes and the operating system kernel record a log of events.  
- Logs are used for auditing and troubleshooting.  
- Many systems store logs in text files under **/var/log**, which can be inspected using tools like **less** and **tail**.  
- In **RHEL**, log handling is done by:
  - **systemd-journald**: Manages and provides access to log files.  
  - **rsyslog**: Sorts and writes syslog messages to persistent files under **/var/log**.  


##### common log files

|       LOG FILE        | TYPE OF MESSAGES STORED                                                                                                                                                                            |
| :-------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **/var/log/messages** | Most syslog messages are logged here. Exceptions include<br>messages related to authentication and email processing,<br>scheduled job execution, and those which are purely debugging-<br>related. |
|  **/var/log/secure**  | Syslog messages related to **security and authentication events**.                                                                                                                                 |
| **/var/log/maillog**  | Syslog messages related to the **mail server**.                                                                                                                                                    |
|   **/var/log/cron**   | Syslog messages related to **scheduled job execution**.                                                                                                                                            |
| **/var/log/boot.log** | **Non-syslog console messages** related to **system startup**.                                                                                                                                     |


<br>

#### Review Syslog Files

#####  Overview of Syslog Priorities

| CODE | PRIORITY | SEVERITY                         |
| ---- | -------- | -------------------------------- |
| 0    | emerg    | System is unusable               |
| 1    | alert    | Action must be taken immediately |
| 2    | crit     | Critical condition               |
| 3    | err      | Non-critical error condition     |
| 4    | warning  | Warning condition                |
| 5    | notice   | Normal but significant event     |
| 6    | info     | Informational event              |
| 7    | debug    | Debugging-level message          |

##### rsyslog Configuration
- Uses **facility + priority** to decide how to handle logs.  
- Configured in **/etc/rsyslog.conf**.  

Rules section in **/etc/rsyslog.conf**:
<img width="919" height="579" alt="Pasted image 20250906080222" src="https://github.com/user-attachments/assets/a41c7e20-e277-478c-9fd1-276ae3e4e4a8" />


##### log file rotation
- **logrotate** tool prevents log files from growing too large.  
- Configured in **/etc/logrotate.conf**.  

<img width="1172" height="495" alt="Pasted image 20250826200935" src="https://github.com/user-attachments/assets/0a8b86cd-d2ad-496c-b0c4-975058d3fcd8" />


<img width="892" height="581" alt="Pasted image 20250906080438" src="https://github.com/user-attachments/assets/e4f50cb3-d014-4a0c-9fd6-276fb42f73ce" />



##### Monitoring logs
Monitoring one or more log files for events is helpful to reproduce problems and issues,
`tail -f /path/to/file` command outputs the **last 10 lines** of the file specified and continues to output new lines in the file as they get written.

##### send Syslog messages manually 
The **logger** command can **send messages to the rsyslog service**. By default, it sends the message to the user facility with the notice priority (**user.notice**) unless specified otherwise with the **-p** option. To send a message to the **rsyslog** service that gets recorded in the **/var/log/boot.log** log file and **messages** file
 
for example: `logger -p local7.notice "Hello,I am a Log"`
<img width="1330" height="473" alt="Pasted image 20250906082016" src="https://github.com/user-attachments/assets/630183c6-7f22-4a04-9b5d-2fe1d6cdea57" />


<br>

#### Review System Journal Entries
The **journalctl** command **highlights important log messages**: 
messages at **notice** or **warning** priority are in **bold text** while messages at the **error** priority or **higher** are in **red text**.
By default, `journalctl -n` shows the **last 10 log entries**. You can adjust this with an optional argument that specifies how many log entries to display. 
For the last five log entries, run the following journalctl command: `journalctl -n 5`

```Bash
journal -n
journal -f     # Similar to the tail -f command
journal -p PROIRITY_NAME    #  list journal entries at the err priority or higher
journal -b
journal --since DURATION 
journal _PID=PROCESS_ID
journal _UID=USER_ID
journal _SYSTEMD_UNIT=UNIT_NAME
```


##### How to make journal log file permanent?
```Bash
mkdir /var/log/journal 
chown -R root:systemd-journal /var/log/journal  
chmod g+s /var/log/journal 
vi /etc/systemd/journal.conf
# make storage parameter uncomment and evaluate (persistent) 
systemctl restart systemd-journal
```

<br>

#### Change the Timezone in Linux

##### Maintaining Time Zone

The **timedatectl** command shows an overview of the current time-related system settings, including current time, time zone, and NTP synchronization settings of the system.

```Bash
# Show system time settings:  
timedatectl
    
# List available time zones:    
timedatectl list-timezones
    
# superuser can Change system time zone:   
timedatectl set-timezone <WANTED_ZONE>
```

##### Maintaining Accurate Time

Use the **tzselect** command to determine the appropriate time zone.

```Bash
# Determine appropriate timezone:
tzselect
    
# Change system time:
timedatectl set-time 'YYYY-MM-DD HH:MM:SS'
    
# Example:
timedatectl set-time 09:00:00
```

<br>
<br>
<br>


### CH12_Managing Networking

#### Gathering Interface Information

Common tools:
- ##### `ifconfig` (deprecated, part of **net-tools**)
- ##### `ip` (modern replacement, part of **iproute2**)

```Bash
ip link show                  # Show all interfaces 
ip addr show DEVICE_NAME      # Display IP addresses 
ip -s link show DEVICE_NAME   # Display interface statistics
```

> **Note!** Use `ip` instead of `ifconfig` → `ifconfig` may not be installed in RHEL 8/9.


<br>
#### Testing Connectivity Between Hosts

```Bash
ping     # check reachability
ip route # show routing table
netstat -nr  # old tool for routing info (deprecated)
traceroute / tracepath # show path to remote host
```

> **Note!** `netstat` belongs to **net-tools** (deprecated). Use `ss` and `ip route` instead.

<br>

#### Troubleshooting Ports & Services

- TCP services use **sockets** = (IP + Protocol + Port).
- Standard ports listed in `/etc/services`.
- ##### use `ss` command (replacement for `netstat`) ---> `ss -tulnp`.
   `netstat` is deprecated in RHEL 8/9.

| OPTION | DESCRIPTION                                             |
| :----: | ------------------------------------------------------- |
| **-n** | Show numbers instead of names for interfaces and ports. |
| **-t** | Show **TCP sockets**.                                   |
| **-u** | Show **UDP sockets**.                                   |
| **-l** | Show **only listening** sockets.                        |
| **-a** | Show **all** (listening and established) sockets.       |
| **-P** | Show **the processes** using the sockets.               |

<br>

#### configure network from CLI (nmcli command)

##### Configuration files

- **RHEL 8**: `/etc/sysconfig/network-scripts/ifcfg-NAME`
- **RHEL 9**: `/etc/NetworkManager/system-connections/*.nmconnection` 
  (Network-scripts removed)


The old network-scripts package has been removed (it was deprecated in the RHEL 8) which means you'll not find anything in the **/etc/sysconfig/network-scripts** directory

<img width="1078" height="450" alt="Pasted image 20250830182000" src="https://github.com/user-attachments/assets/5d8ab3b2-7e4b-4b76-bb05-2b4ca9e6037b" />





##### View network information
```Bash
nmcli dev status      # Status of all devices
nmcli con show        # Show all connections
nmcli con show --active   # Show only active connections
```


##### Add a network connection
**nmcli con add** command is used to add new network connections.

###### The next example 
- **creates** an **static-ens224 connection** for the **ens224 device** 
- with a **static IPv4** 192.168.0.5/24 
- using the **IPv4 address and network prefix 192.168.0.5/24** 
- and **default gateway address** 192.168.0.254 
- but still **auto connects at startup** and **saves its configuration into the same file**.

```Bash
nmcli connection add con-name static-ens224 type ethernet ifname ens224 ipv4.addresses 192.168.1.55/24 gw4 192.168.1.1 connection.autoconnect yes ipv4.method manual
```



<br>

#### Modify Network Configuration files

**there are 3 methods:**
- #### `nmcli` → CLI tool (scriptable, fast)
- #### `nmtui` → TUI (text user interface, easier for beginners)
- ####  Edit config files → manual edits in `/etc/...` then reload (`nmcli con reload`)

##### nmcli

- The **nmcli con mod NAME** command is used to **change the settings for a connection**.
- for example: to **set**:
	- **IPv4 address** to 192.0.2.2/24 
	- **default gateway** to 192.0.2.254 
	for the **connection static-ens3**:
```Bash
nmcli con mod static-ens3 ipv4.addresses "192.0.2.2/24 192.0.2.254"
```

##### edit the configuration file 

By default, changes made with **nmcli con mod NAME** are automatically saved to:
- **/etc/sysconfig/network-scripts/ifcfg-NAME** (RHEL8)
OR
- **/etc/NetworkManager/system-connections/NAME.nmconnection** (RHEL9)

The files can also be **manually edited with a text editor**. After doing so,  **reload** the file so that
NetworkManager reads the configuration changes.
```Bash 
nmcli con reload
```

<br>

#### delete a network connection
The **nmcli con del NAME** command **deletes the connection named** from the system,
disconnecting it from the device and removing the file: 
- /etc/sysconfig/network-scripts/ifcfg-NAME in case of RHEL8 and removing the file
- /etc/NetworkManager/system- connections/NAME.nmconnection in case of RHEL9.

```Bash 
# for example
nmcli con del ens160   
```

<br>

#### who can modify network settings??
- The root user can make any necessary network configuration changes with nmcli.
- Regular users that log in using ssh **do not have access to change network permissions** without becoming root.
- to check your current permissions
```Bash 
nmcli gen permissions
```


##### regular user permissions
<img width="1068" height="550" alt="Pasted image 20250831011606" src="https://github.com/user-attachments/assets/af9e1a27-d279-4010-b3ac-631b8ee365a4" />


##### root permissions
<img width="1179" height="549" alt="Pasted image 20250831011732" src="https://github.com/user-attachments/assets/0dbe09f7-0721-4197-bd1e-32aa55d03611" />



<br>

#### Configure hostnames & name resolution

##### Configure hostnames
- A **static hostname** may be specified in the **/etc/hostname** file. 
- The **hostnamectl** command is used to **modify /etc/hostname** and may be used to view the status of the system's fully qualified hostname.


```Bash
# for example: 
hostnamectl set-hostname host@example.com
hostnamectl status
```


##### Configure name resolution
The **DNS resolver** is used to **convert host names to IP addresses or the reverse**. 
It determines where to look based on the configuration of the **/etc/nsswitch.conf** file. 

By default, the contents of the **/etc/hosts** file are checked first

If an entry is **not found in the /etc/hosts** file, by default the stub resolver tries to **look up the**
**hostname** by using a DNS nameserver. The **/etc/resolv.conf** file controls how this query is performed.

###### Configure name resolution using nmcli 

NetworkManager updates the **/etc/resolv.conf** file using DNS settings in the connection configuration files. Use the **nmcli** to modify the connections.


```bash
# example of set dns resolver in configuration file using nmcli 
nmcli con mod ens160 ipv4.dns 8.8.8.8
nmcli con down ens160
nmcli con up ens160
```

<br>

#### summary of common nmcli commands 


| COMMAND                         | PURPOSE                                                                                   |
| :------------------------------ | ----------------------------------------------------------------------------------------- |
| **nmcli dev status**            | Show the **NetworkManager status** of all network<br>interfaces.                          |
| **nmcli con show**              | List **all connections**.                                                                 |
| **nmcli con show NAME**         | List the **current settings** for the connection name.                                    |
| **nmcli con add con-name NAME** | **Add a new connection** named NAME.                                                      |
| **nmcli con mod NAME**          | **Modify** the connection **name**.                                                       |
| **nmcli con reload NAME**       | **Reload the configuration files** <br>(useful after they have been edited by hand).      |
| **nmcli con up NAME**           | **Activate** the connection **name**.                                                     |
| **nmcli dev dis DEV_NAME**      | **Deactivate and disconnect** the **current connection** on<br>the network interface dev. |
| **nmcli con del NAME**          | **Delete** the **connection name** and its **configuration file**.                        |


<br>
<br>
<br>

