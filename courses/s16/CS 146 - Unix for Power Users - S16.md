CS 146 - "Unix for Power Users" - S16
======================================================================
## Commands Cheat Sheet
```
wc - word count

```
----------------------------------------------------------------------
## Lecture 1
Date: 3/29/2016  
Topics: Administration

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
All forms of I/O go through the file interface. I/O devices 
(screen, keyboard, mouse, RAM) are files like `/dev/ttya`.  
Block Special Example: `$ wc -c /dev/sda` - count the number of bytes
 on my harddrive.  
###### Sockets & Pipes
Pipes are special files uses to pass bytes between two processes.  
`Process A writes ---Pipe---> Process B reads.`  
Sockets connect two processes on different machines across a network.
###### Symbolic Links


----------------------------------------------------------------------
## Lecture 2
Date: 4//2016    
Topics: 

----------------------------------------------------------------------
## Lecture 3
Date: 4//2016  
Topics: 

----------------------------------------------------------------------
## Lecture 4
Date: 4//2016  
Topics: 

----------------------------------------------------------------------
## Lecture 5
Date: 4//2016  
Topics: 