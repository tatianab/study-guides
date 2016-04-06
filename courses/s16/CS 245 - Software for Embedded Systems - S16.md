CS 245 - Software for Embedded Systems - S16
======================================================================
## Readings

### [Trends in Embedded Systems](http://www.ics.uci.edu/~givargis/courses/cs245/papers/MartinZurawski.pdf)

###### Research problem
###### Claimed contribution
###### Methodology / argument
###### Conclusions

----------------------------------------------------------------------
## Lecture 1
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
## Lecture 2 / 3
Date: 3/30/2016 and 4/4/2016    
Topics: Hardware Components (Processors, Storage, Communication, Peripherals)  

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
Date: 4/6/2016  
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
Digital signal `S0, S1, S2, ..., Sn-1`. Can do all kinds of transformations (transpose, amplify, compose, filter, compress, archive), or post-process (spectral analysis).  

Example: create echo by composing the current sound with a previous sound (each multiplied to make the current one louder).  
Example: compress time.


----------------------------------------------------------------------
## Lecture 4
Date: 4/6/2016  
Topics:

----------------------------------------------------------------------
## Lecture 5
Date: 4/8/2016  
Topics:
