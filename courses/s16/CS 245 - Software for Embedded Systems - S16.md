CS 245 - Software for Embedded Systems - S16
======================================================================
## Readings

 [Trends in Embedded Systems](http://www.ics.uci.edu/~givargis/courses/cs245/papers/MartinZurawski.pdf)

[Notes on State Machines](http://www.ics.uci.edu/~givargis/courses/cs245/papers/DavidRWrightNotesOnStateMachines.pdf)

 [Ptolemy Project](http://www.ics.uci.edu/~givargis/courses/cs245/papers/PtolemyProject.pdf)

----------------------------------------------------------------------
## Introduction
Date: 3/28/2016  
Topics: Administration, Final Paper, Some History

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

What is the problem, why is it important? What has been done (state of the art), what needs to be done? What are your opinions?

Groups of two are OK, but paper must be beefier.  

Read between 4 and 8 papers. (Could take 3 or 4 readings to understand). Look at papers that brought a topic to light - informative about the problem and why it is important.

Avoid just grabbing papers that cite others. Give a sense of multiple viewpoints.  

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

----------------------------------------------------------------------
## Embedded System Review
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture1.pdf)  
Date: 3/30/2016 and 4/4/2016    
Topics: Hardware Components (Processors, Storage, Communication, Peripherals), Software Components  

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

###### Timers / Counters
Timers measure time intervals, counters count pulses on a general input signal rather than a clock. A timer/counter is n-bits of storage with some way of incrementing. Two signals: clock and reset. When the source of the clock is tied to an oscillator, then it is a timer. If it is tied to some other source, it is a counter.  

What happens when timer overflows? Usually we wrap around and send some overflow signal (so the timer can be seen as a divider). It is also cascadable.

What if we were able to *write* to the timer? We could set some initial value for the timer, and then know something exact about time when we see the interrupt signal.  
```
Suppose we have an 8-bit timer and an oscillator that makes 256 signals per second.
void func() { // This is called every time an interrupt happens.
	WRITE(0);   // Reset the second.
	ENABLE(1);  // Or if the overhead is 3, start at 3...
	updateTime();
}
main() {
	WRITE(0);
	ENABLE(1);
}
```

*Tradeoff between range and resolution*: Range is the difference between the smallest and largest times we can pick, resolution is which values we can pick. For same counter size, larger range --> smaller resolution (tradeoff).  Example: 8-bit counter. Range: 1/265 sec to 1 sec. Resolution: x/265.

*Watchdog timer*: timer must reset itself every X units, else timer generates a signal. Common use: detect failure, self-reset.  
```
main() {
	check(); // Check to see if a watchdog was called when system failed.
	----
	WTR(); // Call Watchdog timer reset
	----
	WTR();
	----
}
```  
Problem: relies on programmer's ability to know how long each program statement
takes to run.

###### Pulse width Modulator
Generates pulses with specific high/low times. Duty cycle: % time high (Square wave is 50% duty cycle). Common use: control average voltage to electric device. (Simpler than DC-DC converter or digital-analog converter).
```
for (;;) {
	DOUT = 1;
	wait(T1); // How long to stay on high signal
	DOUT = 0;
	wait(T2); // How long to stay on low signal
}
```

###### Displays and Keypads
N rows by N columns. Controller may be built into the LCD (liquid crystal display) module or managed in software. Most systems have a dedicated display controller.

*Display*: challenge is to reverse polarity (so that molecules don't get "locked in") and scan the pixels at a high rate. We have 2xMxN wires controlling the pixels --> logistical problem. Pixels are subdivided into grids. Each pixel is connected to one row and one column wire -- turn on a pixel by applying electricity to its row and column.

*Keypad*: challenge is to scan (in order to read key presses) at a high rate. Keypads have rows and columns of switches that are closed by a press.

###### Stepper Motor Controller
Stepper motor rotates fixed number of degrees when given a "step" signal. Rotation is achieved by applying specific voltage sequence to coils (in order to magnetize the motor to a certain position). Controller greatly simplifies this. Stepper motors have a lot of torque.

###### Analog-Digital and Digital-Analog Converters
Physical world is analog. Computer world is digital. Conversion: given a value in an analog range, what's an equivalent digital output (and vice versa)?  

*Space resolution* - what possible values can we map to?  
*Sampling rate* - how often we do the conversion. Usually tied to the nature of the signal.

*R-2R ladder* - continually divide signal by 2 (digital to analog).  

*Comparator* - decides if a given analog input is greater than or equal to a given value. Use this to do binary search on a value. (analog to digital). Called *successsive approximation*.

----------------------------------------------------------------------
## Lecture 4
[Slides]()  
Date: 4/6/2016 and 4/11/2016  
Topics: Software Components

### Embedded System Software

#### Real-time systems
Real time systems must produce correct results in real time (i.e., meet deadlines).

###### Deadline types
*Hard realtime* system must meet all deadlines. A missed deadline is a system flaw. Vigorously tested (sometimes formally verified) for worst case conditions.

*Firm realtime* system designed to meet all deadlines but some (quantifiable) number of missed deadlines is OK. There is no benefit in continuing to compute if a deadline is missed. Hardware / software designed for average case scenario.

*Soft realtime* (most systems). Designed to meet as many deadlines as possible. Still compute after deadline is missed. Hardware / software designed for average case scenario.

###### Embedded Operating Systems
Needs: means for dynamic task creation (create, join, cancel), means for task synchronization and communication. (Posix threads are a common solution to synchronization problems).  

Hardest task is *scheduling*: Usually we have *n* things which are each easy to do, but we need to decide in what order to do these tasks.  

*Parallel* (two processors) v. *Concurrent* (two tasks being worked on at the same time).

Example: *Consumer-Producer*. Two tasks running concurrently and access the same memory `int buf[N]` (organized as a circular queue with head `h`, tail `t` and size `s`). Consumer reads from buffer and producer writes to buffer.
```
Consumer() {
	for(;;) {
		if (0 < s) { // C programming note: start with a constant
			x = buf[t];
			t = (t+1) % N;
			s--;		   // Problem!
		}
	}
}

Producer() {
	for (;;) {
		if (N > s) {
			buf[h] = rand();
			h = (h+1) % N;
			s++;		   // Problem!
		}
	}
}
```
Problem: the increment and decrement can overwrite each other because they can both access the `s` at the same time.  
How to fix? We could use a *lock* which gives exclusive access to a memory cell.
But this isn't enough. We also need *condition variables*, which allow us to call `wait(cond_var)` that freezes if  `cond_var` is 0. The `signal(cond_var)` function increments the `cond_var`.
```
(Fixed)
Consumer() {
	int x;
	for(;;) {
		wait(item_avail);
		lock(l);
		x = buf[t];
		t = (t+1) % N;
		unlock(l);
		signal(space_avail);
	}
}
Producer() {
	for(;;) {
		wait(space_avail);
		lock(l);
		buf[h] = rand();
		h = (h+1) % N;
		unlock(l);
		signal(item_avail);
	}
}
```

###### Fixed Point Arithmetic
Use integer math to emulate floating point numbers and operations. (Because normal floating point arithmetic is constantly changing where the decimal point is).  
Burden is on programmer in fixed point arithmetic. (This can be done with only integers and no support for floating points).  
Determine range and precision (*m.n*), define `+, -, *, /`, analyze for overflow. Use tables for common functions (sine, cosine etc).  
Benefits? Adjust data type size, speed, control over precision.

#### Digital Signal Processing  

###### Sensors and Actuators
Sensors transform physical stimulus into electrical current. Actuators transform commands into physical stimulus.

###### Analog / Digital Domain Conversion
*Sampling rate* (how often is the signal converted)  
Highest frequency observed vs. highest frequency I care about. We need to sample at least twice the rate of the highest frequency signal present.

*Quantization* (how many bits are used to represent a sample)  
Dynamic range: low-high (we have the range-precision tradeoff again). Resolution (difference between two consecutive values) should be smaller than the ability of the audience to distinguish between two values.  (Examples: HTML rgb colors, human ear limit is 96dB).  
Problems: underloading (underusing bits), clipping (signals outside of range), AC clipping.

*Aliasing* (erroneous signals not present in analog domain)  
Fix: sample at a higher rate, use anti-aliasing filters.  
Example: bicycle wheel that appears to be going backwards.

###### Signal Processing
Computation on signal after it has been collected.

Digital signal `S0, S1, S2, ..., Sn-1`. Can do all kinds of transformations (transpose, amplify, compose, filter, compress, archive), or post-process (spectral analysis).  

Examples: create echo by composing the current sound with a previous sound (each multiplied to make the current one louder), compress time (chipmunk), filter by averaging every pair of signals to remove noise.

A *fourier transform* (FT) converts a continuous signal into a sum of sine and cosine functions.

----------------------------------------------------------------------
## Programming Embedded Systems
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture2.pdf)  
Date: 4/18/2016  
Topics: Finite State Machines

Chapters 3,4,6,7 (12, 13 maybe).  

### Finite State Machines
Be able to convert FSM to C programs (or Java) or whatever.  
Focus on semantics of state machines for the exam.  

###### Moore vs. Mealy

###### Global variables
Two tasks should not be allowed to naively write to the shared variable.  
Give everyone a "virtual resource" and introduce a "tie-breaker" that enforces conflict resolution for the real resource.  

----------------------------------------------------------------------
## Embedded Design Domains
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture3.pdf)  
Date: 4/18/2016 and 4/20/2016  
Topics:

###### Hybrid Embedded Systems
Computation systems whose behavior is tightly integrated with the physical world. Physical states are continuous, and computation states are discrete. (Passage of time during computation affects the state of the physical world.)  

###### Sensor Networks
Motivation: tiny ad-hoc networks of sensors on a battlefield. (US gov).  
Properties: small, self-organizing, energy efficient, distributed.  
Problem: how I do I power it?  

###### Multimedia
Examples: Virtual reality, blue ray players.  
Problems: Quality of service, lots of data!

###### Consumer Electronics
Home: add computation and networking to yesterday's appliances.  
Office: integration, electronic paper (e.g., filing, printing).  
Total automation: Fantasy of home/office automation and integration.  

###### Networking Equipment
Stitching LANs (merge networks): bridge or router.  
Ports: switch or hub (transmit to all).
Common properties: large volume of highly structured data, minimal transformation.  

###### Medical Instruments
Diagnosis - data collection, analysis, sensing / actuation.  
Examples: radiation therapy, artificial hearts.  
Must be extremely reliable.  

###### Distributed and Grid Computing
Coordinated resource sharing.  
Grid is static, reliable, and has infinite resources (nearly).  
Users have limited resources.  
Middleware mitigates the resource sharing and coordination efforts.  
Example: phones and data.  
Earliest form: search engines (ask a remote server to find something for you).  

###### MEMS (Micro Electro-Mechanical Systems)
Small electro-mechanical devices.  

*MEMS Design Concerns*
Need to deal with: forces related to volume (weight and inertia) decrease in significance, but forces related to surface area (friction, surface tension).

*Inside MEMS Devices*
Mechanical: motors, gears, pivots, linkages.  
Electronic: transistors, resistors, capacitors, inductors.  
Internals are rugged, fast, use little power, small volume, cheap.  

###### Nanotechnology (even smaller than MEMS!)
Molecular scale - molecules need to be glued together one at a time. (Can't just chisel something out).  
Quantum physics becomes important - it changes properties of materials. Laws we know may or may not apply.  
Nanotechnology today is passive (e.g. synthetic material): most is concerned with controlling material properties.  

----------------------------------------------------------------------
## Models, Languages & Tools
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture4.pdf)  
Date: 4//2016  
Topics:

How do we build embedded systems?  
A *model* refers to a conceptual notion of how to capture system behavior. (A set of object with composition rules and execution semantics).  
*Languages* formally describe models of computation (with specific syntax and semantics).  
*Tools* transform a model captured in one language to a model captured in another language (e.g. compilers).  

###### Models of Computation
*Sequential model of computation* - computation as a set of sequential instructions.    
*Concurrent model of computation* - multiple threads of execution with synchronization and communication.  
*Object-oriented model of computation* - computation is a set of objects with polymorphism (a derived class can modify the behavior of its base class).  
*FSM (Finite State Machine)* - set of states and transitions.  
*DFG (Directed Flow Graph?)* - set of computation nodes and flow paths.  
*Petri Net* - set of places, transitions, edges and tokens. Tokens indicate whether or not a transition can fire. (Good for modelling and simulation of physical or social systems).  
*Kahn Process Network* - set of concurrent processes sharing data using unbounded buffers. (Buffers are used to pick up the slack of certain processes being faster than others). Used to determine what size to assign to real buffers in the system.    
*Communicating Sequential Processes (CSP)* - set of concurrent processes with no shared memory, but send/receive messages OK.  

###### Meta Models
*Discrete Events (DE)* - events occur at discrete points on a time continuum, events trigger computations, computations trigger more events. (This is necessary for digital computers - VHDL and VERILOG are the most common hardware description languages).  
*Continuous Time (CT)* - differential equations model continuous I/O response as a function of continuous time. (Note that DE can't perfectly capture continuous behavior - must lose some information).    
*Synchronous Reactive* - like DE, but with event timings snapped to a regular clock.  
*Publish and Subscribe* - (Higher level model) sending applications (publishers) publish messages without explicitly specifying recipients. Receiving applications (subscribers) receive only those messages that the subscriber has registered an interest in. Loosely coupled networked systems.  
*Map-reduce* - break a problem into a "map" phase where a bunch of processors do a little work on a chunk of data. "Reduce" phase is bringing all the data back together.  

###### VHDL (Discrete Events Model Example)
Hardware description language.  
We have processes (which can die and come back to life). It responds to sensitivity lists.
In the example below, if clock changes then the process begins.    
```
process(clock)
begin
(1)	clock <= not clock after 100ns;
end
process(clock)
begin
	count <= count + 1 after 50ns;
end
```
(1) There is a vicious circle because changing clock restarts the process.
If we add `after 100ns` we can make a proper timing diagram which is a square wave.

Software == (Digital) Hardware! Any digital circuit can be simulated by software, and vice versa. This is because both are based on discrete event simulation.    

###### Tools

*Compilers* - a native compiler outputs native machine language for the machine on which the compiler is running. A cross compiler outputs code that runs on a different machine. A parallelizing compiler (attempts to) discover opportunities for parallelism in a program (for example, a loop where the result of the previous loop is not needed). A VLIW (very long instruction word), it is possible to do multiple instructions in parallel. Special compilers translate models (FSM, Petri Net) to, for example, C code. Synthesis (translate a high-level language into a circuit - e.g. RTL).

*IDEs*  

*Simulators* - need controlability (stop, take steps forward and backward), observability (stop and watch variables). Functional (shows how the thing would behave without precise timing), Cycle-accurate (shows precise timing), Non-functional (e.g., power simulator or cache simulator where behavior isn't important), Bus-functional (precise use of bus communication).   

*Interpreters* - like simulators. Example: Virtual Machine (mimic another processor, re-popularized by JVM), Hypervisor (manages virtual machines).  

*Emulators* - FPGA (field programmable gate array).  

*Debuggers* - tools that allow you to run a program with controlability and observability.  

*Visual programming* - graphical programming.  

#### Design Flow

###### Research
*Verification* - Did we build the thing right? Simulation, formal verification (answer questions without simulation).  
*Formal Verification* - Equivalence checking (reduce to canonical form, check for equality -> not always possible), theorem proving (treat code as a black box -> two programs are equivalent if their I/O response is the same), model checking (is a property satisfied by a model?).

*Validation* - Did we build the right thing?  

*Some tools for Simulation* - `gdb` (debugger - set breakpoints, step, print values), `gcov` (what % of your code was executed?), `gprof` (where are the "hot" areas of the code - which instructions are executed the most often), `valgrind` (memory leak checking). All of these require running the code (simulation).

###### Design

###### Document
Write code and explain it.

----------------------------------------------------------------------
## Midterm Review
Date: 5/2/2016  

Moore's Law - Transistor capacity on a chip doubles every 18 months. (precise).  
Working definition of embedded systems: Everything except laptops / desktops / servers that have electricity running through and perform something intelligent. Usually embedded hardware / software forms part of a larger system and are concealed.

Processors - Difference between General Purpose (fixed width, fixed number of registers, fetches program from memory) vs. Single Purpose processor (custom data path, controller hard coded, no program memory).  

Storage - storage permanence (how long bits are held after write), writeability: manner/speed in which program memory can be written to.

Communication - parallel vs. serial (one bit at a time -> easier to build high speed because of variation in arrival times in parallel).

Pulse Width Modulation (PWM) - deliver a range of energy from a digital system to analog. Send out 0's and 1's, with T_high and T_low. Period = T_high + T_low. Duty cycle = T_high / Period.

Real-time - in addition to being correct, result must come in a timely fashion. Hard (no deadline misses, miss -> failure), firm (some misses, miss -> do not finish task), soft (some misses, miss -> continue to finish).

DSP (Digital Signal Processing) - A -> D and D -> A. Sampling (rate of conversion, should be >2 times the highest frequency) and quantization (number of bits per sample). Dynamic range (range of samples we can represent).

Control - open (no feedback), closed (feedback, three strategies: proportional (to the current error), integral (sum of all errors), derivative (change in error)).

State Machine - model with concept of states and transitions. Does things in the right sequence, but no notion of timing. Need to know how to read and understand state machine, but also translate from C to a state machine diagram.  

Synchronous State Machine - add a period. Transitions take place at a given time.

Concurrent Synchronous State Machine - two or more state machines with possibly different periods working concurrently.

Definition of Cyber-Physical System (also known as Hybrid System) - systems that interact (via sensing and actuation) with the physical world. Physical (environment) + Cyber (electronic, computation), control in between.

Distributed and Grid Computing - high-end/grid/cloud ("infinite" resources), mobile devices (limited resources). Make the right thing happen in each part.

Sensor Networks - really small devices, lots of them, limited, power is an issue, collective problem solving gives them power.  

MEMs (micro-meter, tiny systems with recognizable components, different well-known properties are relevant, properties don't change) vs. Nanosystems (nano-meter level, properties actually *change*).  

Models of Computation - objects, rules for composing objects (how to create a valid model), execution semantics (how do we "run" the model).  

Language - capture a model of computation. Can be textual (e.g. C) or pictorial (e.g. a state machine). They are both equally powerful, because they are the same.  

Tools - transform one model into another (e.g. compiler).

Petri Net - question: given a Petri Net, fill in a timeline with how many tokens are associated with each node. (find Petri Net simulation online).

Compiler - cross (compiler for a different machine) vs native (compiler for the machine where the compiler is running).

Verification (is this a correct toaster?) vs. validation (was I supposed to build a toaster?).

Verification - testing through simulation (slow!) OR formal verification (limited).

----------------------------------------------------------------------
## Optimizing Compilers
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture5.pdf)  
Date: 5/9/2016

###### Pre-processing (Example: C/C++)
Expands and substitutes `#xxx` directives.  
Lacks syntax and semantics knowledge of underlying C.  
Criticism: macros have no notion of type -> can cause unexpected results.    

*Macros* - `#define`  
Alternatives: inline functions (performance) - keyword `inline`, templates (extensible).

*Conditional compilation* - `#if - #then - #else`  
Alternative: dead code elimination.  

###### Lexical Analyzer
Convert sequence of characters into a sequence of tokens.  
Lexical rules are captured by regular expressions: FSM recognizes tokens, longest match is accepted, tokens deliminated by white-space.  
There are tools (lex, flex, etc) that automatically generate lexers.  

###### Semantic Analyzer (Parser)
Match a sequence of tokens against a formal grammar of the language. (Call the backend on a match, or emit an error if no match).  
The language is captured by a context-free grammar, which typically recognizes a superset of the language.  
Top-down / bottom-up parsers.

Note: parsers cannot find all errors (e.g., undefined variable).  

###### Code Generation
Lexer/Parser yield an internal representation: abstract syntax tree / parse tree.
Multiple passes: convert parse tree into a linear sequence of instructions (abstract 3-address code). Final passes generate executable code from abstract 3-address code.  

###### Code Optimization
Optimization modifies code to make it more efficient (e.g. speed, space or power), but it must retain program behavior.  
Kinds of optimization: algorithmic optimization (very tough & rare -> understand what you are doing and use the best algorithm), general (dataflow and loop) optimization -> make the algorithm run with fewest instructions needed, machine dependent/independent optimization.  
No universal optimization objective.  

Types of Optimization:
*Alias and pointer analysis* - a pointer may point to any memory location. Restricting the scope of reference helps with subsequent optimizations.  
*Dataflow Optimization* - common subexpression elimination, constant folding.  
*Loop Optimizations* - induction variable elimination -> all induction variables in a loop can be merged into one, loop fission, loop fusion, loop splitting (for long loop bodies -> helps program cache performance), loop inversion (while to do-while), loop-invariant code motion (move things out of the loop if they don't change), loop-nest optimization (most benefit in deeper nested statements), loop unrolling (more code, fewer branches), loop unswitching (hoist conditional statements out of loop body), software pipelining (split loop body into several sections), loop parallelization (loop body done in parallel).

Example (induction variable elimination):
```
while (i < 10) {
  j = 3 * i + 1;
  a[j] = a[j] - 2;
  i += 2;
}

// Optimized
t = 3 * i + 1;
while (t < 31) {
  // j = t;
  a[t] = a[t] - 2;
  // i += 2;
  t += 6;
}
```

*Machine dependent optimizations* - register allocation, instruction selection and scheduling.  

*General Optimizations* - dead code elimination, code factoring (opposite of code inlining - recognize repeated statements), function versioning (create simpler versin of function).

*Advanced optimizations* task partitioning, static evaluation assisted optiizations, trace assisted optimization.




----------------------------------------------------------------------
## Real-time Operating Systems
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture6.pdf)  
Date: //2016  
Topics:

----------------------------------------------------------------------
## Reconfigurable Computing
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture7.pdf)  
Date: //2016  
Topics:

----------------------------------------------------------------------
## Embedded Software Reliability
[Slides](http://www.ics.uci.edu/~givargis/courses/cs245/lectures/lecture8.pdf)  
Date: //2016  
Topics:
