Pluto-P
=======

General Info
------------

The Pluto-P is an inexpensive ($60) FPGA board featuring the ACEX1K chip from Altera.

### Requirements

1.  A Pluto-P board

2.  An EPP-compatible parallel port, configured for EPP mode in the system BIOS

### Connectors

-   The Pluto-P board is shipped with the left connector presoldered, with the key in the indicated position. The other connectors are unpopulated. There does not seem to be a standard 12-pin IDC connector, but some of the pins of a 16P connector can hang off the board next to QA3/QZ3.

-   The bottom and right connectors are on the same .1" grid, but the left connector is not. If OUT2…OUT9 are not required, a single IDC connector can span the bottom connector and the bottom two rows of the right connector.

### Physical Pins

-   Read the ACEX1K datasheet for information about input and output voltage thresholds. The pins are all configured in "LVTTL/LVCMOS" mode and are generally compatible with 5V TTL logic.

-   Before configuration and after properly exiting Machinekit, all Pluto-P pins are tristated with weak pull-ups (20k-ohms min, 50k-ohms max). If the watchdog timer is enabled (the default), these pins are also tristated after an interruption of communication between Machinekit and the board. The watchdog timer takes approximately 6.5ms to activate. However, software bugs in the pluto\_servo firmware or Machinekit can leave the Pluto-P pins in an undefined state.

-   In pwm+dir mode, by default dir is HIGH for negative values and LOW for positive values. To select HIGH for positive values and LOW for negative values, set the corresponding dout-NN-invert parameter TRUE to invert the signal.

-   The index input is triggered on the rising edge. Initial testing has shown that the QZx inputs are particularly noise sensitive, due to being polled every 25ns. Digital filtering has been added to filter pulses shorter than 175ns (seven polling times). Additional external filtering on all input pins, such as a Schmitt buffer or inverter, RC filter, or differential receiver (if applicable) is recommended.

-   The IN1…IN7 pins have 22-ohm series resistors to their associated FPGA pins. No other pins have any sort of protection for out-of-spec voltages or currents. It is up to the integrator to add appropriate isolation and protection. Traditional parallel port optoisolator boards do not work with pluto\_servo due to the bidirectional nature of the EPP protocol.

### LED

-   When the device is unprogrammed, the LED glows faintly. When the device is programmed, the LED glows according to the duty cycle of PWM0 (**LED** = **UP0** *xor* **DOWN0**) or STEPGEN0 (**LED** = **STEP0** *xor* **DIR0**).

### Power

-   A small amount of current may be drawn from VCC. The available current depends on the unregulated DC input to the board. Alternately, regulated +3.3VDC may be supplied to the FPGA through these VCC pins. The required current is not yet known, but is probably around 50mA plus I/O current.

-   The regulator on the Pluto-P board is a low-dropout type. Supplying 5V at the power jack will allow the regulator to work properly.

### PC interface

-   Only a single pluto\_servo or pluto\_step board is supported.

### Rebuilding the FPGA firmware

The `src/hal/drivers/pluto_servo_firmware/` and `src/hal/drivers/pluto_step_firmware/` subdirectories contain the Verilog source code plus additional files used by Quartus for the FPGA firmwares. Altera’s Quartus II software is required to rebuild the FPGA firmware. To rebuild the firmware from the .hdl and other source files, open the `.qpf` file and press CTRL-L. Then, recompile Machinekit.

Like the HAL hardware driver, the FPGA firmware is licensed under the terms of the GNU General Public License.

The gratis version of Quartus II runs only on Microsoft Windows, although there is apparently a paid version that runs on Linux.

### For more information

Some additional information about it is available from <http://www.fpga4fun.com/board_pluto-P.html> and from [the developer’s blog](http://emergent.unpy.net/01165081407).

pluto-servo: Hardware PWM and quadrature counting<span id="sec:pluto-servo"></span>
-----------------------------------------------------------------------------------

The pluto\_servo system is suitable for control of a 4-axis CNC mill with servo motors, a 3-axis mill with PWM spindle control, a lathe with spindle encoder, etc. The large number of inputs allows a full set of limit switches.

This driver features:

-   4 quadrature channels with 40MHz sample rate. The counters operate in "4x" mode. The maximum useful quadrature rate is 8191 counts per Machinekit servo cycle, or about 8MHz for Machinekit’s default 1ms servo rate.

-   4 PWM channels, "up/down" or "pwm+dir" style. 4095 duty cycles from -100% to +100%, including 0%. The PWM period is approximately 19.5kHz (40MHz / 2047). A PDM-like mode is also available.

-   18 digital outputs: 10 dedicated, 8 shared with PWM functions. (Example: A lathe with unidirectional PWM spindle control may use 13 total digital outputs)

-   20 digital inputs: 8 dedicated, 12 shared with Quadrature functions. (Example: A lathe with index pulse only on the spindle may use 13 total digital inputs)

-   EPP communication with the PC. The EPP communication typically takes around 100 us on machines tested so far, enabling servo rates above 1kHz.

### Pinout

 UPx   
The "up" (up/down mode) or “pwm” (pwm+direction mode) signal from PWM generator X. May be used as a digital output if the corresponding PWM channel is unused, or the output on the channel is always negative. The corresponding digital output invert may be set to TRUE to make UPx active low rather than active high.

 DNx   
The "down" (up/down mode) or “direction” (pwm+direction mode) signal from PWM generator X. May be used as a digital output if the corresponding PWM channel is unused, or the output on the channel is never negative. The corresponding digital ouput invert may be set to TRUE to make DNx active low rather than active high.

 QAx, QBx   
The A and B signals for Quadrature counter X. May be used as a digital input if the corresponding quadrature channel is unused.

 QZx   
The Z (index) signal for quadrature counter X. May be used as a digital input if the index feature of the corresponding quadrature channel is unused.

 INx   
Dedicated digital input \#x

 OUTx   
Dedicated digital output \#x

 GND   
Ground

 VCC   
+3.3V regulated DC

![](images/pluto-pinout.png)

Figure 1. Pluto-Servo Pinout<span id="fig:Pluto-Servo-Pinout"></span>

<table>
<caption>Table 1. Pluto-Servo Alternate Pin Functions<span id="table:Pluto-Servo-Alternate-Pin"></span></caption>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Primary function</th>
<th align="left">Alternate Function</th>
<th align="left">Behavior if both functions used</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>UP0</strong></p></td>
<td align="left"><p>PWM0</p></td>
<td align="left"><p>When pwm-0-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT10</p></td>
<td align="left"><p>XOR’d with UP0 or PWM0</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP1</strong></p></td>
<td align="left"><p>PWM1</p></td>
<td align="left"><p>When pwm-1-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT12</p></td>
<td align="left"><p>XOR’d with UP1 or PWM1</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP2</strong></p></td>
<td align="left"><p>PWM2</p></td>
<td align="left"><p>When pwm-2-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT14</p></td>
<td align="left"><p>XOR’d with UP2 or PWM2</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP3</strong></p></td>
<td align="left"><p>PWM3</p></td>
<td align="left"><p>When pwm-3-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT16</p></td>
<td align="left"><p>XOR’d with UP3 or PWM3</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN0</strong></p></td>
<td align="left"><p>DIR0</p></td>
<td align="left"><p>When pwm-0-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT11</p></td>
<td align="left"><p>XOR’d with DN0 or DIR0</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN1</strong></p></td>
<td align="left"><p>DIR1</p></td>
<td align="left"><p>When pwm-1-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT13</p></td>
<td align="left"><p>XOR’d with DN1 or DIR1</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN2</strong></p></td>
<td align="left"><p>DIR2</p></td>
<td align="left"><p>When pwm-2-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT15</p></td>
<td align="left"><p>XOR’d with DN2 or DIR2</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN3</strong></p></td>
<td align="left"><p>DIR3</p></td>
<td align="left"><p>When pwm-3-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT17</p></td>
<td align="left"><p>XOR’d with DN3 or DIR3</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QZ0</strong></p></td>
<td align="left"><p>IN8</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QZ1</strong></p></td>
<td align="left"><p>IN9</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QZ2</strong></p></td>
<td align="left"><p>IN10</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QZ3</strong></p></td>
<td align="left"><p>IN11</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QA0</strong></p></td>
<td align="left"><p>IN12</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QA1</strong></p></td>
<td align="left"><p>IN13</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QA2</strong></p></td>
<td align="left"><p>IN14</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QA3</strong></p></td>
<td align="left"><p>IN15</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QB0</strong></p></td>
<td align="left"><p>IN16</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QB1</strong></p></td>
<td align="left"><p>IN17</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QB2</strong></p></td>
<td align="left"><p>IN18</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QB3</strong></p></td>
<td align="left"><p>IN19</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
</tbody>
</table>

### Input latching and output updating

-   PWM duty cycles for each channel are updated at different times.

-   Digital outputs OUT0 through OUT9 are all updated at the same time. Digital outputs OUT10 through OUT17 are updated at the same time as the PWM function they are shared with.

-   Digital inputs IN0 through IN19 are all latched at the same time.

-   Quadrature positions for each channel are latched at different times.

### HAL Functions, Pins and Parameters

A list of all *loadrt* arguments, HAL function names, pin names and parameter names is in the manual page, *pluto\_servo.9*.

### Compatible driver hardware

A schematic for a 2A, 2-axis PWM servo amplifier board is available (<http://emergent.unpy.net/projects/01148303608>). The L298 H-Bridge is inexpensive and can easily be used for motors up to 4A (one motor per L298) or up to 2A (two motors per L298) with the supply voltage up to 46V. However, the L298 does not have built-in current limiting, a problem for motors with high stall currents. For higher currents and voltages, some users have reported success with International Rectifier’s integrated high-side/low-side drivers.

Pluto-step: 300kHz Hardware Step Generator<span id="sec:Pluto-step:-Hardware-step"></span>
------------------------------------------------------------------------------------------

Pluto-step is suitable for control of a 3- or 4-axis CNC mill with stepper motors. The large number of inputs allows for a full set of limit switches.

The board features:

-   4 “step+direction” channels with 312.5kHz maximum step rate, programmable step length, space, and direction change times

-   14 dedicated digital outputs

-   16 dedicated digital inputs

-   EPP communication with the PC

### Pinout

 STEPx   
The “step” (clock) output of stepgen channel **x**

 DIRx   
The “direction” output of stepgen channel **x**

 INx   
Dedicated digital input \#x

 OUTx   
Dedicated digital output \#x

 GND   
Ground

 VCC   
+3.3V regulated DC

While the “extended main connector” has a superset of signals usually found on a Step & Direction DB25 connector—4 step generators, 9 inputs, and 6 general-purpose outputs—the layout on this header is different than the layout of a standard 26-pin ribbon cable to DB25 connector.

![](images/pluto-step-pinout.png)

Figure 2. Pluto-Step Pinout<span id="fig:Pluto-Step-Pinout"></span>

### Input latching and output updating

-   Step frequencies for each channel are updated at different times.

-   Digital outputs are all updated at the same time.

-   Digital inputs are all latched at the same time.

-   Feedback positions for each channel are latched at different times.

### Step Waveform Timings

The firmware and driver enforce step length, space, and direction change times. Timings are rounded up to the next multiple of ***1.6μs***, with a maximum of ***49.6μs***. The timings are the same as for the software stepgen component, except that “dirhold” and “dirsetup” have been merged into a single parameter “dirtime” which should be the maximum of the two, and that the same step timings are always applied to all channels.

![](images/pluto_step_waveform.png)

Figure 3. Pluto-Step Timings<span id="fig:Pluto-Step-Timings"></span>

### HAL Functions, Pins and Parameters

A list of all *loadrt* arguments, HAL function names, pin names and parameter names is in the manual page, *pluto\_step.9*.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


