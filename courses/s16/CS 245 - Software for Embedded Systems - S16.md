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

###### Real-time systems


----------------------------------------------------------------------
## Lecture 4
Date: 4/6/2016  
Topics:

----------------------------------------------------------------------
## Lecture 5
Date: 4/8/2016  
Topics:
