CS 253 - Analysis of Programming Languages - S16
======================================================================
## Lecture 1
Date: 3/29/2016  
Topics: Introduction

### Introduction
Why analyze programs? Detect (low-level) errors, improve performance, detect
security violations, improve maintainability / understandability, validate software correctness.  ** Will be a question on the quiz!

Part of the reason why C "won" is that it is fast - it doesn't do
anything that it doesn't have to (i.e., initialize vars for you,
do dynamic checks for null pointers or types).  

Dynamic Analysis - understand a program by running it (with dynamic
checks). This adds time and space overhead to the running of a program.

Five major areas:
* Data Flow Analysis
* Abstract Interpretation
* Constraint-based Analysis
* Type and Effect System
* Scalable Interprocedural Analysis

###### Static Analysis
Understand / analyze a program without running a
program. Often quite conservative, because we need to allow for all
possible options. (Focus of this course).  

Key idea: *safe approximation*. We must consider the entire set of possibilities. (Must be conservative!).    
Example:
```
read(x);               // x is "killed".
if (x > 0) { y = 1; }  
else { y = 2; S(); }   // S does not modify y.
z = y;                 // this is a "copy".
```
We can conclude that `z in {1,2}`.

###### Tiny-ada
We will use this language throughout the course.
* Numbers, variables, operators (boolean, relational, arithmetic)
* If statements
* While statements

###### Operational Semantics
* Execution of program is described directly (in terms of the state of the computation)
* Or by translation

###### Halting Problem
Optimization is undecidable, so we are always using heuristics.  
There are many things about programming languages that we can't understand (like pointers). *Alias problem*: two different expressions can refer to same memory cell.

###### Some Math
*Lattice Theory* - partially ordered sets.  
*Total order* - all objects are related (e.g. > on integers).  
*Partial order* - some objects are related, others aren't.  
*Monotonic functions* - nonincreasing or nondecreasing.  

Quotes:
* "It takes a year to learn C++, half a year to learn Java, and a year of learning Python to ruin you for life."
* "You can program with recursion guilt-free!"

----------------------------------------------------------------------
## Lecture 2
Date: 3/31/2016    
Topics: Homework 1 (Flex and Bison)

### Homework notes
For else and else-if, only change `expr.cpp`.  

A *lexeme* is a string of characters considered atomic. A *token* is an abstraction to represent a set of lexemes. (We map strings to integer tokens because string comparison is slow).

Files:
```
tiny-ada.l - lexemes to tokens (flex)
tiny-ada.y - shows grammar (bison).
tiny-ada.h - tokens created by bison
exprs.h    - Abstract syntax
```

Syntax for describing grammar in yacc:
```
$1, $2 are like the parameters and $$ is the return value.
Curly braces surround action (in C++) to perform.
| is or.
yy is a prefix that indicates flex or bison methods or data.
```

A *union* is an overlay of one or more objects. (A single memory section that can be interpreted as multiple types).

----------------------------------------------------------------------
## Lecture 3
Date: 4/5/2016  
Topics: Data Flow Analysis

### Data Flow Analysis

###### Requirements of DFA
Must preserve the meaning of a program.  
Side effects (any modification to a non-local variable) and aliases may prevent some optimizations.  
Must provide a measurable improvement in code. (will be a constant like 2 to 10, not a big-O change).  
Improvement must be worth the implementation effort.

###### Optimization Types
*Common subexpression elimination*  
```
T1 = 4 * i;
T2 = 4 * i;
A[T1] = B[T2]
```

*Constant Folding and Constant Propagation*  
Replace references to constants with the actual constant. Statically compute known arithmetic like constant + constant.  
```
T1 = 0;
T2 = T1 + 1;
T3 = T2 + 1;
put T3;       
```

*Dead Store Elimination*  
Get rid of unused variables.
```
T1 = 0;
T2 = 1;
T3 = 2;
put T3;
```

*Induction Variable Identification and Elimination*  
```
for (int i = 0; i < 10; ++i) {
  a[i] = 0;
}
```
Add 4 to the pointer instead of continually computing address.   

*Alias Classification and Pointer Analysis*  
```
int i = 0;
int *p = &i;
*p = 10;
put(i);
```

###### Scope of Data Flow Analysis
*Local data flow* - analysis limited to a single basic block.  
*Global data flow* - analysis of entire function.  
*Interprocedural Data Flow* - entire program. (many compilers do not do this -- too many libraries).  

###### Basic Blocks and Flow Graphs
A *definition* of a a variable is an asssignment to
*Use*  
*Live* (a point is in between two instructions)
*Basic block*
*Control flow graph*

----------------------------------------------------------------------
## Lecture 4
Date: 4//2016  
Topics:

----------------------------------------------------------------------
## Lecture 5
Date: 4//2016  
Topics:
