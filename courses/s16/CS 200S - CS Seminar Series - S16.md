CS 200S - CS Seminar Series - S16
======================================================================
## How much time, energy, and power does an algorithm need?
Date: 4/1/2016  
Speaker: Rich Vuduc (Georgia Tech)  
[Slides](http://hpcgarage.org/uci/slides.pdf)  

*Memory wall* (memory isn't getting better at same rate as computation).

###### Time to execute a DAG on P processors
Work (W) : total ops  
Span (D): critical path
W / D : inherent parallelism  
We want to measure speedup over the best sequential algorithm

###### Power and Energy
Idea: given X amount of power allowed, how good is our algorithm?  
Energy required? Energy can't be "overlapped". Energy is proportional to work. Time is roughly proportional to energy.  
Power required? Power = Energy / Time. We expect a trade-off between time and power.  

###### Non-uniform costs
What if costs are *non-uniform*?
Two cases: operation costs may change, operations may have different costs.  
*External memory model:* fast memory, slow memory.  
Q = How many transfers between slow and fast memory?  
W / Q = computational intensity.
B = ratio of computations to memory operations, balance (time or energy).  
*Roofline model* : Roofline is machine capability, Arch line is?
Real systems have a "power cap".  

###### What if we could start over?
We are solving an optimization problem: fixed # of transistors, fixed power budget, fixed computation.

###### Take aways
Code optimized for time and energy  
Reconfigure power use?  

----------------------------------------------------------------------
## Lecture Title
Date: 4/8/2016  
Speaker:   
Topics:  

----------------------------------------------------------------------
## Lecture Title
Date: 4//2016  
Speaker:   
Topics:   

----------------------------------------------------------------------
## Lecture Title
Date: 4//2016   
Speaker:   
Topics:  

----------------------------------------------------------------------
## Lecture Title
Date: 4//2016  
Speaker:   
Topics:  
