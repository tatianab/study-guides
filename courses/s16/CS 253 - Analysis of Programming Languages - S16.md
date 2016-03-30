CS 253 - Analysis of Programming Languages - S16
======================================================================
## Lecture 1
Date: 3/29/2016  
Topics: Introduction

### Introduction
Why analyze programs? Detect errors, improve performance, detect
security violations, improve maintainability / understandability.

Part of the reason why C "won" is that it is fast - it doesn't do
anything that it doesn't have to (i.e., initialize vars for you,
do dynamic checks for null pointers or types).  

**Static Analysis** - understand / analyze a program without running a
program. Often quite conservative, because we need to allow for all
possible options. (Focus of this course).
Dynamic Analysis - understand a program by running it (with dynamic
checks). This adds time and space overhead to the running of a program.  

Five major areas:
* Data Flow Analysis
* Abstract Interpretation
* Constraint-based Analysis
* Type and Effect System
* Scalable Interprocedural Analysis

### Static Analysis
Key idea: safe approximation. We must consider the space of possibilities.  
Example:
```
read(x);               // x is "killed".
if (x > 0) { y = 1; }  
else { y = 2; S(); }   // S does not modify y.
z = y;			       // this is a "copy".
```
We can conclude that `z in {1,2}`.

### While language
We will use this language throughout the course.
* Numbers, variables, operators (boolean, relational, arithmetic)
* If statements
* While statements

Quotes:

"It takes a year to learn C++, half a year to learn Java, and a year 
of learning Python to ruin you for life."

"You can program with recursion guilt-free!"

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