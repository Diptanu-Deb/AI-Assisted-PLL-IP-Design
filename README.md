# AI-Assisted-PLL-IP-Design
VSD Internship on AI Assisted PLL IP Design  
## Introduction
A clock multiplier Phase-Locked Loop (PLL) is a negative feedback control system that generates a high-frequency output clock whose phase is related to the low frequency input clock. The need for PLLs is widespread as they find application in analog, digital, RF and communication systems. 

The low-frequency clock can be generated using a crystal with frequencies up to 200 MHz. Generating a high-frequency clock with high spectral purity is not possible with a crystal and this is where PLLs come into play. 

A simple PLL consists of a Phase and Frequency Detector (PFD), Charge Pump (CP), Loop Filter (LF), Voltage Controlled Oscillator (VCO) and a frequency divider in the feeback path.   
The processors used in our Gadgets have various modules inside serving different purposes. These modules may include CPU, GPU, RAM, WiFi module, Audio module, and so on.  
The interesting fact is that each of these modules may not run at a particular clock frequency. For example, the CPU would need a high clock frequency for performing calculations faster, while the Display module may not.  
This is where a Phase-Locked Loop circuit comes into the picture. As shown in Figure above, a PLL is installed at the clock entry point of each above module to produce the desired frequency at which the module would operate.  

## Phase Frequency Detector (PFD)

The Phase Frequency Detector (PFD) is the first and one of the most important blocks in a Charge-Pump PLL (CP-PLL). It compares the phase and frequency of the reference clock (REF_CLK) and the feedback clock (FB_CLK) from the frequency divider. Based on the comparison, it generates two digital signals:

1.UP – Increases the VCO control voltage, causing the VCO frequency to increase.    

2.DOWN – Decreases the VCO control voltage, causing the VCO frequency to decrease.  

### Block Diagram
         REF_CLK
             │
             ▼
      +----------------+
      |                |
      |      PFD       |------> UP
      |                |
      |                |------> DOWN
      +----------------+
             ▲
             │
          FB_CLK
    
    Purpose of the PFD

#### The PFD detects:  
Phase difference between the clocks.  
Frequency difference between the clocks.  

Unlike a simple XOR phase detector, a PFD can detect whether one clock is running faster or slower than the other, making it suitable for wide frequency acquisition.  

### Phase/Frequency Locking Concept in a PLL

The phase/frequency locking concept is the fundamental operating principle of a Phase-Locked Loop (PLL). A PLL continuously compares a reference clock with a feedback clock and adjusts the output frequency of the Voltage-Controlled Oscillator (VCO) until both clocks have the same frequency and a constant phase relationship.  

### What is Locking?

A PLL is said to be locked when:

The output frequency (after the feedback divider) equals the reference frequency.  
The phase difference between the reference and feedback clocks is constant (ideally zero or a fixed offset).  

Mathematically:

fREF=fFB
	​
Since the feedback clock is obtained by dividing the VCO output:

fFB=fVCO/N
	​
Therefore, when locked:

fVCO=N×fREF
	​
where:

fREF = Reference clock frequency
fFB= Feedback clock frequency
fVCO= VCO output frequency
N = Divider ratio

PLL Block Diagram:

          
          Reference Clock
                │
                ▼
      +-------------------+
      |       PFD         |
      +-------------------+
          │          │
         UP        DOWN
          │          │
          ▼          ▼
      +-------------------+
      |   Charge Pump     |
      +-------------------+
                │
                ▼
      +-------------------+
      |   Loop Filter     |
      +-------------------+
                │
             Vctrl
                │
                ▼
      +-------------------+
      |       VCO         |
      +-------------------+
                │
          Output Clock
                │
                ▼
        Divide by N
                │
                └───────────────► Feedback Clock

## Phase Locking

Phase locking means that the rising edges of the reference clock and the feedback clock occur at the same instant (or maintain a constant phase difference).

Before Lock
Reference :  ↑      ↑      ↑

Feedback  :      ↑      ↑      ↑

The clocks are not aligned.

Phase error exists.
PFD generates UP or DOWN pulses.
The VCO frequency is adjusted.
During Locking
Reference :  ↑      ↑      ↑

Feedback  :    ↑      ↑      ↑

The phase error decreases with each cycle.

After Lock
Reference :  ↑      ↑      ↑      ↑

Feedback  :  ↑      ↑      ↑      ↑

The edges overlap.

Phase error ≈ 0.
UP = DOWN = 0 (ideally).
Control voltage remains nearly constant.
Frequency Locking

Frequency locking means the frequencies become equal.

Case 1: VCO Too Slow
Reference Clock

↑    ↑    ↑    ↑

Feedback Clock

↑       ↑       ↑

The feedback clock has a lower frequency.

The PFD detects:

Reference leads.  
UP pulses are generated.  
Charge pump increases Vctrl.  
VCO frequency increases.  

Case 2: VCO Too Fast  
Reference Clock

↑      ↑      ↑

Feedback Clock

↑   ↑   ↑   ↑

The feedback clock has a higher frequency.

The PFD detects:

Feedback leads.  
DOWN pulses are generated.  
Charge pump decreases Vctrl.  
VCO frequency decreases.  

Case 3: Frequency Locked
Reference

↑     ↑     ↑

Feedback

↑     ↑     ↑

Both clocks have the same frequency.

Lock Acquisition Process

The PLL reaches lock in four stages:

1. Initial State
VCO starts at an arbitrary frequency.
Large phase and frequency error.  
2. Frequency Acquisition
PFD detects frequency difference.
Charge pump changes Vctrl.
VCO frequency moves toward the desired value.  
3. Phase Alignment
Frequencies become equal.
Remaining phase error is corrected.  
4. Locked State
Frequencies are equal.
Phase error is nearly zero.
VCO operates at a stable frequency.  \

Example

Suppose:

Reference clock = 100 MHz

Divider = 8

Desired VCO frequency:

fVCO=8×100 MHz=800 MHz

Initially
Reference =100 MHz

VCO =720 MHz

Divider output =90 MHz

Since:

90 MHz <100 MHz

The PFD generates UP pulses.

The charge pump increases the control voltage.

The VCO frequency gradually increases:

720 MHz

↓

740 MHz

↓

770 MHz

↓

790 MHz

↓

800 MHz

The divider output becomes:

800/8

=

100 MHz

The PLL is now frequency locked.

Lock Time

Lock time is the time required for the PLL to transition from an unlocked state to a stable locked state after power-up or a change in reference frequency.

It depends on:

1.Loop filter design  
2.Charge pump current  
3.VCO gain (KVCO)  
4.Divider ratio  
5.Initial frequency error  

A faster lock time is desirable in many communication and clock-generation applications, but it often involves trade-offs with stability and jitter.

### UP/DOWN Pulse Generation

The Phase Frequency Detector (PFD) generates two output signals—UP and DOWN—to indicate whether the Voltage-Controlled Oscillator (VCO) frequency should be increased or decreased. These pulses are then applied to the Charge Pump, which adjusts the control voltage (Vctrl) of the VCO.  

PFD Block Diagram
           REF_CLK
              │
              ▼
        +-------------+
        |             |
        |     PFD     |
        |             |
        +-------------+
         │         │
        UP       DOWN
         │         │
         ▼         ▼
      Charge Pump
           │
        Loop Filter
           │
         VCO
Principle of Operation

The PFD compares the rising edges of:

Reference Clock (REF_CLK)
Feedback Clock (FB_CLK)

Depending on which clock edge arrives first, it generates either an UP or a DOWN pulse.

Case 1: Reference Clock Leads

If the reference clock edge arrives before the feedback clock:

REF_CLK :  ↑--------↑--------↑

FB_CLK  :      ↑--------↑--------↑

UP       :  ─────┐      ─────┐
                 │           │
                 └────       └────

DOWN     : __________________________
Operation
Rising edge of REF_CLK sets the UP flip-flop.
UP becomes HIGH.
It remains HIGH until the feedback clock arrives.
Rising edge of FB_CLK sets the DOWN flip-flop momentarily.
Reset logic immediately resets both outputs.

Result:

UP pulse generated.
Pulse width equals the time difference between REF_CLK and FB_CLK.
Charge pump sources current.
VCO frequency increases.
Case 2: Feedback Clock Leads

If the feedback clock edge arrives first:

REF_CLK :      ↑--------↑--------↑

FB_CLK  :  ↑--------↑--------↑

DOWN     : ─────┐      ─────┐
                │           │
                └────       └────

UP       : __________________________
Operation
FB_CLK arrives first.
DOWN becomes HIGH.
DOWN remains HIGH until REF_CLK arrives.
Reset logic clears both outputs.

Result:

DOWN pulse generated.
Charge pump sinks current.
VCO frequency decreases.
Case 3: Locked Condition

When both clocks are aligned:

REF_CLK : ↑     ↑     ↑

FB_CLK  : ↑     ↑     ↑

UP       : __________________

DOWN     : __________________

Ideally:

No UP pulse.  
No DOWN pulse.  
Control voltage remains constant.  
PLL stays locked.  

Note: In practical implementations, very narrow correction pulses may still appear to compensate for small phase errors and maintain lock.  

Internal PFD Circuit

A conventional PFD uses:

Two positive-edge-triggered D flip-flops  
One asynchronous reset gate   
                 D = 1
                  │
      REF_CLK ───►┌────────┐
                  │  DFF1  │
                  └───┬────┘
                      │
                     UP
                      │
                      ├──────────┐
                      │          │
                      ▼          ▼
                  +----------------+
                  |  RESET Logic   |
                  +----------------+
                      ▲          ▲
                      │          │
                     DOWN        │
                  ┌───┴────┐     │
      FB_CLK ───► │  DFF2  │◄────┘
                  └────────┘
#### Working

Both D inputs are tied to logic HIGH.  
REF_CLK sets the UP output.  
FB_CLK sets the DOWN output.  
When both outputs become HIGH, the reset gate clears both flip-flops.  

#### Pulse Width

The width of the UP or DOWN pulse is proportional to the phase difference.

| Phase Difference | Pulse Width  |
| ---------------- | ------------ |
| Small            | Narrow pulse |
| Medium           | Medium pulse |
| Large            | Wide pulse   |


Larger phase error → longer pulse → more charge transferred → faster frequency correction.  
##### Truth Table

| Condition    | UP | DOWN | PLL Action             |
| ------------ | -- | ---- | ---------------------- |
| REF leads FB | 1  | 0    | Increase VCO frequency |
| FB leads REF | 0  | 1    | Decrease VCO frequency |
| REF = FB     | 0  | 0    | Maintain lock          |
