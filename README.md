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
     ```     REF_CLK
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
    ```
    Purpose of the PFD

#### The PFD detects:  
Phase difference between the clocks.  
Frequency difference between the clocks.  

Unlike a simple XOR phase detector, a PFD can detect whether one clock is running faster or slower than the other, making it suitable for wide frequency acquisition.  

          
