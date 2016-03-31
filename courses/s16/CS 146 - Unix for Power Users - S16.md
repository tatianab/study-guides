CS 146 - "Unix for Power Users" - S16
======================================================================
## Commands Cheat Sheet
```
awk
diff
file
find
ftp
grep
make
man (manual) - man <command>
nslookup
sed
sort
tar
traceroute
uniq
vi
which
zip
wc - word count

```
----------------------------------------------------------------------
## Section 1
Date: 3/29/2016 and 3/31/2016
Topics: Administration, UNIX Organization, UNIX Files

### Administration
[Course Website](http://www.ics.uci.edu/~wayne/courses/cs146/)  
Recommended Books:
* "The UNIX Programming Environment"
(Kernighan and Pike)  
* "Linux Command Line and Shell Scripting Bible" (Blum and Bresnahan)  

### UNIX Organization
UNIX Kernel - a large C program that implements a
general interface to a computer to be used for writing programs.  
Application programs don't directly access the computer - they go
through the the OS (Kernel).  
Shell (sh) is a C program that interprets commands typed into it and
carries out the desired actions. It is an application program.  

### Files
A file is the most basic entity.
Types of files: regular, directory, character special, block special
 (like the bare bits on your harddrive), socket
 (files that connect different computers), symbolic link (shortcuts).  
All files types can be accessed via a *common interface*.  

###### Regular Files
A named sequence of bytes of variable length.

###### Directories
Folders (in a tree structure). Contain references to other files or
directories.
Can be read, but not written.  
Two ways of specifying: absolute (`/homes/blah/file.txt`) or
relative (`blah/files.txt`).  
With absolute pathnames, search starts from the *root* directory.  
Every process has a CWD (current working directory). The command `cd`
changes the CWD of the bourne shell process to the given path.  

###### Character Special Files & Block Special Files
Three kinds of interface to devices: block interface (buffered in fixed size blocks), character interface (unbuffered), line interface (line-buffered).  
All forms of I/O go through the file interface. I/O devices
(screen, keyboard, mouse, RAM) are files like `/dev/ttya`.  
Block Special Example: `$ wc -c /dev/sda` - count the number of bytes
 on my harddrive.  

###### Sockets & Pipes
Pipes are special (unnamed) files used to pass bytes between two processes.  
`Process A writes ---Pipe---> Process B reads.`  
Sockets connect two processes on different machines across a network.
Example pipe: `$ ls | wc -l`: pipe the result of ls into the input of word count with the line option.

###### File Permissions
Every user has a login name. The file `/etc/passwd` associates a UID, GID and a password with each login name. When a file is created, the UID and GID or the creator are remembered. Every named file is associated with a set of permissions (a string of bits): `rwxs rwxs rwx` (Owner-Group-Others).  
Permissions: `r` read (list contents of dir), `w` write (create and remove files in dir), `x` execute (query and cd into a dir), `s` setuid/gid (dangerous -- only use if you know what you're doing!).  

###### Inodes *
Each distinct file has an *inode* that refers to it.
An inode contains: the type of file, time of inode last modified, time file data last written, time file data last read, creator's user ID, creator's group ID, number of directory links (reference count), file size, pointer to disk blocks containing data or the major and minor device ID, permission bits, sticky bit (only makes sense for executables - flag to OS saying that this should be cached because it is used frequently).

###### Mounting
"The file system" refers to the whole system starting at the root. "A file system" refers to a subtree, i.e., a physical partitions on a disk).
A file system is contained on a disk. File systems are mounted onto existing file names.

###### Hard links and Symbolic links
Dir files contain (filename, i-number) pairs. Each entry is called a link, and each file can have more than one link. Regular links (*hard links*) are not allowed to cross file systems. All hard links have an equally strong hold on their files (so a file can't be deleted with outstanding hard links). A *symbolic link* (like a shortcut) is just a string of text (pathname) of the file being linked to, and these can cross file systems. Symbolic links can point to deleted files.

----------------------------------------------------------------------
## Section 2
Date: 3/31/2016 and    
Topics: UNIX Shell, Unix Processes  

### UNIX Shell

### UNIX Processes

----------------------------------------------------------------------
## Section 3
Date: 4//2016  
Topics:

----------------------------------------------------------------------
## Section 4
Date: 4//2016  
Topics:

----------------------------------------------------------------------
## Lecture 5
Date: 4//2016  
Topics:
