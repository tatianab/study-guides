CS 200S - CS Seminar Series - S16
======================================================================
[Sign-in website](https://checkin.ics.uci.edu/)

----------------------------------------------------------------------
## How much time, energy, and power does an algorithm need?
Date: 4/1/2016  
Speaker: Rich Vuduc (Georgia Tech)  
[Slides](http://hpcgarage.org/uci/slides.pdf)  

###### Take aways
Code should be optimized for time *and* energy.    
Can we reconfigure power use?  

##### Introduction
We usually operate under the assumption that stores and loads are much more expensive than computations, but this won't necessarily be true for all time.  
*Memory wall* (memory isn't getting better at same rate as computation).

###### Time to execute a DAG on P processors
Work (W) : total ops  
Span (D): critical path
W / D : inherent parallelism  
We want to measure speedup over the best sequential algorithm.

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

----------------------------------------------------------------------
## Neuromorphic Learning Machines
Date: 4/8/2016  
Speaker: Emre Nefti   

###### Take aways
Building neuromorphic systems for solving practical problems requires understanding at all levels.  
Synaptic Sampling Machines are a class of neural networks that use synaptic stochasticity as a means to Monte Carlo sampling and unsupervized learining.

###### Motivation / Introduction
Brain-inspired information processing systems.  
Application: control a robot doing a "cognitive" task (something that the human brain is good at).  
Focus of the talk: Neuromorphic learning machines (a combination of machine learning, computational neuroscience and chip design).  

*Neuromorphic engineering*: use physics to model neurons? Emulation of bio-physics of neural systems circuits. Key problem: understanding the brain is more of a barrier than actually creating the systems.

*Machine learning*: many advances can be attributed to having more computing power.

*Theoretical Neuroscience*: Humans implicitly use probabilistic interference (i.e., to process sensory inputs). But how does this happen at the neuron level?  

How can we bring these three fields together?

###### Synaptic Sampling Machines
Can do: probabilistic inference, online unsupervized learning, efficient hardware design.  
*Boltzman Machine*: add stochasticity to a Hopfield network to escape local minima??. A Hopfield network is a deterministic unit.   

----------------------------------------------------------------------
## Cryptosystems Based on Group-Theoretic Problems: A Survey, New Results, Open Problems
Date: 4/15/2016  
Speaker: Delaram Kahrobaei  

###### Takeaways
Many new group-theoretic cryptosystems are based on the "conjugacy search problem".

###### Presentation
*Relation* A pair of "reduced words".  
*Free group*
*Group Presentation*

###### Word Decision Problem
Given a finitely presented group G, does there exist an algorithm to decide whether or not a word in the generators is the trivial word?  
(There are groups for which this problem is undecidable).

###### Conjugacy Problems
*Conjugacy Search Problem* (Most often used in crypto)  
Given a finitely presented group G and two conjugate words u and v, is there an algorithm to find a z such that z^{-1}uz = v?  

###### Growth rate of a Group
(Related to infinite groups).
Growth function (n) = number of words in G such that len(w) < n.  

###### A Non-Abelian Diffie Hellman Key Exchange (Ko, Lee et al.)  
Recall DH: Alice with g^a, Bob with g^b, compute g^ab.  
Based on conjugacy search problem being hard.  

*What group to use?* Finitely presented, efficiently computable normal form,
conjugacy search problem has exponential time complexity, exponential growth rate

###### Polycyclic Groups
A group is called polycyclic if there exists a polycyclic series through the group (ie a normal series of finite length with cyclic factors) such that
$G_{i+1}/G_i$ is cyclic.  
For example, the Dihedral group with 8 elements.  
Properties: finitely presented, every element has a unique normal form.  

###### Diffie-Hellman with Semi-Direct Products
Idea: use a group and operation such that we can do "Diffie Hellman with
multiplication." (Double discrete log?).
*Semi-direct product*  
*Holomorph*  

----------------------------------------------------------------------
## Rethinking Memory System Design (for Data-Intensive Computing)
Date: 4/22/2016   
Speaker: Onur Mutlu   

###### Major Trends
Need for main memory capacity, bandwidth and QoS increasing:
multi-core, data-intensive applications, consolidation.  

*Memory Capacity Gap* - less capacity / core over time.  
Memory bandwidth increase is even worse.  

Main memory energy/ power is a key system design concern. (DRAM consumes power even when not in use).

DRAM technology scaling is ending.  

###### DRAM Scaling Problem
DRAM stores charge in a capacitor. Capacitor must be large enough for reliable sensing. Hard to scale below X nm.  

*Disturbance errors* - adjacent rows in DRAM can cause errors by repeatedly applying and un-applying charge. This can be induced in many existing systems.  

Why is this a scaling problem? Smaller chips have higher error rates.

This can be a security risk! Google showed that this could be used to gain kernel privileges on a laptop. Bit-flips are predictable.  


###### Solution Approaches
One solution is to increase refresh rates. (But would need to do x7 increase).

Fix it: make memory and controllers more intelligent.  
Eliminate or minimize: replace or augment DRAM with a different tech
Embrace it: design heterogeneous memories (none perfect).

###### New Memory Architectures
Memory-centric system design.

###### Emerging Memory Technologies

###### Hybrid Memory Systems
Use different technologies with different pros / cons and use software to achieve the best of both technologies.  

Classify data as vulnerable or tolerant and map memory accordingly.  

###### Memory Interference
Interference between cores is uncontrolled - uncontrollable, unpredictable, vulnerable system.  

Goal: Predictable performance in complex systems.  

###### Strong Memory Service Guarantee
Goal: Satisfy performance/SLA requirements

###### In-Memory Computation
Push from technology, pull from systems and applications (data access is a major bottleneck).

*Approaches* - Minimally change DRAM to enable simple computation primitives or exploit the control logic in 3D-stacked memory.  

###### Changing DRAM
Now - bulk data copy.  
Future - RowClone (in memory copy).  

###### PCM (Phase Change Memory)
Naively replacing DRAM with PCM doesn't work. The hybrid approach is probably better.  

----------------------------------------------------------------------
## Lecture Title
Date: 4/29/2016  
Speaker:   
