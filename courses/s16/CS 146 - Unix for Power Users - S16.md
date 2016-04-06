CS 146 - "Unix for Power Users" - S16
======================================================================
## Commands Cheat Sheet
```
awk
diff
file
find
ftp

grep - global regular expresson print
grep <string> <file1> .. <filen>
Find all lines in the given files that contain the given string.

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

###### File descriptors
A file descriptor is an integer that identifies a file.

----------------------------------------------------------------------
## Section 2
Date: 4/5/2016 and    
Topics: Unix Processes  

### UNIX Processes
A process is an executing program.

###### Fork
A `fork()` system call duplicates a running program. The duplicate is called the child, and the original is called the parent. The only difference is the fork return value. (The child returns 0 from the `fork()` call, the parent returns a non-zero value, which is the process ID of the child).  

###### Exec
An `exec()` system call replaces the program being run by a process by a different one (starts the new program from the beginning). This is the *only* way (`fork` then `exec`) that a new program is started in Unix.  

The `exec` system call has a parameter `char* argv` that is used to pass command line arguments to the executed commands. (`argv[0]` holds the command itself).  
There is also the argument `char* envp` (environment variables). Use the `export` command to add a local shell variable to the environment.

###### Processes and File descriptors
File descriptors are maintained across exec calls and fork calls.  

`getpid()` - get my process ID   
`getppid()` - get my parent's process ID  

###### Initializing UNIX
1. Run `/etc/init`
2. Fork and exec `/etc/getty`
3. Password stuff
4. Exec `/bin/bash`

###### Standard streams
Descriptor | Name | Purpose
--- | --- | --- |
0 | Standard Input  | Read input
1 | Standard Output | Write results
2 | Standard Error  | Report errors

###### How bash runs commands
Run a command: The shell forks then execs the typed command.

###### I/O Redirection
`<` Receive standard input from given file  
`>` Redirect standard output to given file  
This happens after the fork but before the exec.

###### Pipes
Example: `<x> | <y>` sends output of `<x>` to input of `<y>`.  
The first program still reads from standard input, and the last program writes to standard output.  
Can be (almost) arbitrarily long.  
Commands in a pipeline are run concurrently.  

### Bourne Shell

###### Redirection
`n > file`: redirect output from fd n to file (default n=1 stdout).  
`n >> file`: append output from fd n to file (default n=1 stdout).  
`n < file`: redirect input (default n=0 stdin).  

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
