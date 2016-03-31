CS 245 - Software for Embedded Systems - S16
======================================================================
## Lecture 1
Date: 3/28/2016  
Topics: Administration, Final Paper

### Administration
Website: [CS 245](http://www.ics.uci.edu/~givargis/courses/cs245/)  
Office Hours: TuTh 1:30 - 2:30 (DBH 3076)  
Grading: Two Midterms (25% each), Final Paper (50%)  

### Final Survey Paper Expectations
Structure:
* Introduction - set context, why is it important?
* Meat         - summaries of existing literature.
* Conclusion   - your opinion / analysis / insight.
(Add more to this section).

----------------------------------------------------------------------
## Lecture 2
Date: 3/30/2016  
Topics: History, Hardware Components (Processors, Storage, Communication, Peripherals)  
Reading: [Trends in Embedded Systems](Reading http://www.ics.uci.edu/~givargis/courses/cs245/papers/MartinZurawski.pdf)

### Some History
1947 - Shockley, Brattain and Bardeen invent transistor (Bell Labs)  
1961 - First commercial integrated circuit (Fairchild/Texas Instruments)  
1963 - CMOS (Complementary metal oxide semi conductor) solves problem
	   of excessive power consumption (still modern)    

Circuits now use most power at the moment of switching from 0 to 1.
(Slower clock speeds reduce power).

###### Moore's Law  
1965 - Moore's Law : number of transistors on a chip will double every
	   18 months.  
1968 - State of the art: 64 transistor chip.  

Transistors have increased exponentially, but clock speed and power consumption have leveled off around 2003. (Because keeping chips cool
becomes too difficult after a point).    

###### Definition (Embedded System)
Everything except laptops / desktops / servers that have electricity running through and perform something intelligent. Usually embedded hardware / software forms part of a larger system and are concealed

### Embedded System Hardware

###### Processors
Processor: something that computes. (Has a data-path and a controller).

*General purpose:* variety of tasks, flexible, slow / power hungry. Controller is programmable (control logic in memory --> fetch / decode overhead). Has a highly general data-path with a normal bit-width (32 or 64), complete set of arithmetic and logic units, as well as a large set of registers.  

*Single purpose:* one computation task, inflexible, fast / power efficient. The controller is hardwired, so there is no program memory or cache, and no fetch/decode overhead. The data-path is highly tuned and has a custom bit width, custom arithmetic / logic units, and a custom set of registers. (Common for embedded systems).    

###### Storage
Memory: something that stores bits (with access logic).  
Some considerations: writeability (manner / speed in which memory can be written), permanence (how long memory holds bits after they are written).  
Examples: Flash (~10 years), SRAM (static : holds data well, but loses data once power is lost), DRAM (dynamic : loses data within milliseconds, must be refreshed).

###### Communication
Bus: something that transfers bits. Can be wires air or fiber. Also includes interface logic. We need a connectivity scheme and a protocol (i.e, ports, timing diagrams of read / write cycles, error detection, direct memory access).   

*Serial Communication*: single wire for data transfer (bits sent one at a time), and one or more additional wires for control. Better for long distance communication, because of data alignment issues in parallel (need to wait for all possibly misaligned data to arrive). Lower cost. Examples: USB, Ethernet.  
(USB actually uses two wires, D+ and D- with bit and its opposite. This is more error tolerant because noise gets canceled out).

*Parallel Communication*: multiple wires for data transfer (multiple bits sent at a time), and one or more additional wires for control. Better for short distance communication (within nodes). Higher cost.

*Wireless Communication*: Infrared (IR - cheap, limited range) or radio frequency (RF - bigger range, transmitter power determines range).

###### Peripherals
(Specific computation task).

*Timers/Counters*: Timers measure time intervals, counters count pulses on a general input signal rather than a clock.


----------------------------------------------------------------------
## Lecture 3
Date: 4/4/2016  
Topics: Software

### Embedded System Software

###### Real-time systems


----------------------------------------------------------------------
## Lecture 4
Date: 4/6/2016  
Topics:

----------------------------------------------------------------------
## Lecture 5
Date: 4/8/2016  
Topics:
