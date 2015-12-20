M Codes
=======

<span id="cha:m-codes"></span>

M Code Quick Reference Table <span id="m-code-quick-reference"></span>
----------------------------------------------------------------------

<table>
<colgroup>
<col width="28%" />
<col width="71%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Code</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="#sec:M0-M1">M0 M1</a></p></td>
<td align="left"><p>Program Pause</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M2-M30">M2 M30</a></p></td>
<td align="left"><p>Program End</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M60">M60</a></p></td>
<td align="left"><p>Program Pause</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M60">M60</a></p></td>
<td align="left"><p>Program End</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M3-M4-M5">M3 M4 M5</a></p></td>
<td align="left"><p>Spindle Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M6-Tool-Change">M6</a></p></td>
<td align="left"><p>Tool Change</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M7-M8-M9">M7 M8 M9</a></p></td>
<td align="left"><p>Coolant Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M48-M49-Override">M48-M49</a></p></td>
<td align="left"><p>Feed &amp; Spindle Overrides</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M50-Feed-Override">M50</a></p></td>
<td align="left"><p>Feed Override Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M51-Spindle-Override">M51</a></p></td>
<td align="left"><p>Spindle Override Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M52-Adaptive-Feed-Control">M52</a></p></td>
<td align="left"><p>Adaptive Feed Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M53-Feed-Stop-Control">M53</a></p></td>
<td align="left"><p>Feed Stop Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M61-Set-Current-Tool-Number">M61</a></p></td>
<td align="left"><p>Set Current Tool Number</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M62-M65">M62-to-65</a></p></td>
<td align="left"><p>Output Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M66-Input-Control">M66</a></p></td>
<td align="left"><p>Input Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M67-Analog-Output">M67</a></p></td>
<td align="left"><p>Analog Output Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M68-Analog-Output">M68</a></p></td>
<td align="left"><p>Analog Output Control</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M100-to-M199">M100-M199</a></p></td>
<td align="left"><p>User Defined M codes</p></td>
</tr>
</tbody>
</table>

M0, M1 Program Pause<span id="sec:M0-M1"></span>
------------------------------------------------

-   *M0* - pause a running program temporarily. Machinekit remains in the Auto Mode so MDI and other manual actions are not enabled. Pressing the cycle start button will restart the program at the following line.

-   *M1* - pause a running program temporarily if the optional stop switch is on. Machinekit remains in the Auto Mode so MDI and other manual actions are not enabled. Pressing the cycle start button will restart the program at the following line.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">It is OK to program <em>M0</em> and <em>M1</em> in MDI mode, but the effect will probably not be noticeable, because normal behavior in MDI mode is to stop after each line of input anyway.</td>
</tr>
</tbody>
</table>

M2, M30 Program End<span id="sec:M2-M30"></span>
------------------------------------------------

-   *M2* - end the program. Pressing cycle start will start the program at the beginning of the file.

-   *M30* - exchange pallet shuttles and end the program. Pressing cycle start will start the program at the beginning of the file.

Both of these commands have the following effects:

1.  Change from Auto mode to MDI mode.

2.  Origin offsets are set to the default (like *G54*).

3.  Selected plane is set to XY plane (like *G17*).

4.  Distance mode is set to absolute mode (like *G90*).

5.  Feed rate mode is set to units per minute (like *G94*).

6.  Feed and speed overrides are set to ON (like *M48*).

7.  Cutter compensation is turned off (like *G40*).

8.  The spindle is stopped (like *M5*).

9.  The current motion mode is set to feed (like *G1*).

10. Coolant is turned off (like *M9*).

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Note
</div></td>
<td align="left">Lines of code after M2/M30 will not be executed. Pressing cycle start will start the program at the beginning of the file.</td>
</tr>
</tbody>
</table>

M60 Program Pause<span id="sec:M60"></span>
-------------------------------------------

-   *M60* - exchange pallet shuttles and then pause a running program temporarily (regardless of the setting of the optional stop switch). Pressing the cycle start button will restart the program at the following line.

M3, M4, M5 Spindle Control<span id="sec:M3-M4-M5"></span>
---------------------------------------------------------

-   *M3* - start the spindle clockwise at the *S* speed.

-   *M4* - start the spindle counterclockwise at the *S* speed.

-   *M5* - stop the spindle.

It is OK to use *M3* or *M4* if the [S](#sec:S-spindle-speed) spindle speed is set to zero. If this is done (or if the speed override switch is enabled and set to zero), the spindle will not start turning. If, later, the spindle speed is set above zero (or the override switch is turned up), the spindle will start turning. It is OK to use *M3* or *M4* when the spindle is already turning or to use *M5* when the spindle is already stopped.

M6 Tool Change<span id="sec:M6-Tool-Change"></span>
---------------------------------------------------

### Manual Tool Change

If the HAL component hal\_manualtoolchange is loaded, M6 will stop the spindle and prompt the user to change the tool based on the last *T-* number programmed. For more information on hal\_manualtoolchange see the ([Manual Tool Change](#sec:Manual-Tool-Change)) Section.

### Tool Changer

To change a tool in the spindle from the tool currently in the spindle to the tool most recently selected (using a T word - see Section [Select Tool](#sec:T-Select-Tool)), program *M6*. When the tool change is complete:

-   The spindle will be stopped.

-   The tool that was selected (by a T word on the same line or on any line after the previous tool change) will be in the spindle. The T number is an integer giving the changer slot of the tool (not its id).

-   If the selected tool was not in the spindle before the tool change, the tool that was in the spindle (if there was one) will be in its changer slot.

-   If configured in the .ini file some axis positions may move when a M6 is issued. See the EMCIO section of the Integrator’s Manual for more information on tool change options.

-   No other changes will be made. For example, coolant will continue to flow during the tool change unless it has been turned off by an *M9*.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Warning
</div></td>
<td align="left">The tool length offset is not changed by <em>M6</em>, use <em>G43</em> after the <em>M6</em> to change the tool length offset.</td>
</tr>
</tbody>
</table>

The tool change may include axis motion. It is OK (but not useful) to program a change to the tool already in the spindle. It is OK if there is no tool in the selected slot; in that case, the spindle will be empty after the tool change. If slot zero was last selected, there will definitely be no tool in the spindle after a tool change. The tool changer will have to be setup to perform the tool change in hal and possibly classicladder.

M7, M8, M9 Coolant Control<span id="sec:M7-M8-M9"></span>
---------------------------------------------------------

-   *M7* - turn mist coolant on.

-   *M8* - turn flood coolant on.

-   *M9* - turn all coolant off.

It is OK to use any of these commands, regardless of the current coolant state.

M48, M49 Override Control<span id="sec:M48-M49-Override"></span>
----------------------------------------------------------------

-   *M48* - enable the spindle speed and feed rate override controls.

-   *M49* - disable both controls.

It is OK to enable or disable the controls when they are already enabled or disabled. See the [Feed Rate](#sub:feed-rate) Section for more details.

M50 Feed Override Control<span id="sec:M50-Feed-Override"></span>
-----------------------------------------------------------------

-   *M50 &lt;P1&gt;* - enable the feed rate override control. The P1 is optional.

-   *M50 P0* - disable the feed rate control.

While disabled the feed override will have no influence, and the motion will be executed at programmed feed rate. (unless there is an adaptive feed rate override active).

M51 Spindle Speed Override Control<span id="sec:M51-Spindle-Override"></span>
-----------------------------------------------------------------------------

-   *M51 &lt;P1&gt;* - enable the spindle speed override control. The P1 is optional.

-   *M51 P0* - disable the spindle speed override control program. While disabled the spindle speed override will have no influence, and the spindle speed will have the exact program specified value of the S-word (described in [Spindle Speed](#sec:S-spindle-speed) Section).

M52 Adaptive Feed Control<span id="sec:M52-Adaptive-Feed-Control"></span>
-------------------------------------------------------------------------

-   *M52 &lt;P1&gt;* - use an adaptive feed. The P1 is optional.

-   *M52 P0* - stop using adaptive feed.

When adaptive feed is enabled, some external input value is used together with the user interface feed override value and the commanded feed rate to set the actual feed rate. In Machinekit, the HAL pin *motion.adaptive-feed* is used for this purpose. Values on *motion.adaptive-feed* should range from 0 (feed hold) to 1 (full speed).

M53 Feed Stop Control<span id="sec:M53-Feed-Stop-Control"></span>
-----------------------------------------------------------------

-   *M53 &lt;P1&gt;* - enable the feed stop switch. The P1 is optional. Enabling the feed stop switch will allow motion to be interrupted by means of the feed stop control. In Machinekit, the HAL pin *motion.feed-hold* is used for this purpose. A *true* value will cause the motion to stop when *M53* is active.

-   *M53 P0* - disable the feed stop switch. The state of *motion.feed-hold* will have no effect on feed when M53 is not active.

M61 Set Current Tool Number<span id="sec:M61-Set-Current-Tool-Number"></span>
-----------------------------------------------------------------------------

-   *M61 Q-* - change the current tool number while in MDI or Manual mode. One use is when you power up Machinekit with a tool currently in the spindle you can set that tool number without doing a tool change.

It is an error if:

-   Q- is not 0 or greater

M62 to M65 Output Control<span id="sec:M62-M65"></span>
-------------------------------------------------------

-   *M62 P-* - turn on digital output synchronized with motion. The P- word specifies the digital output number.

-   *M63 P-* - turn off digital output synchronized with motion. The P- word specifies the digital output number.

-   *M64 P-* - turn on digital output immediately. The P- word specifies the digital output number.

-   *M65 P-* - turn off digital output immediately. The P- word specifies the digital output number.

The P-word ranges from 0 to a default value of 3. If needed the the number of I/O can be increased by using the num\_dio parameter when loading the motion controller. See the Integrator’s Manual Configuration Section Machinekit and HAL section for more information.

The M62 & M63 commands will be queued. Subsequent commands referring to the same output number will overwrite the older settings. More than one output change can be specified by issuing more than one M62/M63 command.

The actual change of the specified outputs will happen at the beginning of the next motion command. If there is no subsequent motion command, the queued output changes won’t happen. It’s best to always program a motion G code (G0, G1, etc) right after the M62/63.

M64 & M65 happen immediately as they are received by the motion controller. They are not synchronized with movement, and they will break blending.

M66 Wait on Input<span id="sec:M66-Input-Control"></span>
---------------------------------------------------------

    M66 P- | E- <L->

-   *P-* - specifies the digital input number from 0 to 3.

-   *E-* - specifies the analog input number from 0 to 3.

-   *L-* - specifies the wait mode type.

    -   *Mode 0: IMMEDIATE* - no waiting, returns immediately. The current value of the input is stored in parameter \#5399

    -   *Mode 1: RISE* - waits for the selected input to perform a rise event.

    -   *Mode 2: FALL* - waits for the selected input to perform a fall event.

    -   *Mode 3: HIGH* - waits for the selected input to go to the HIGH state.

    -   *Mode 4: LOW* - waits for the selected input to go to the LOW state.

-   *Q-* - specifies the timeout in seconds for waiting. If the timeout is exceeded, the wait is interrupt, and the variable \#5399 will be holding the value -1. The Q value is ignored if the L-word is zero (IMMEDIATE). A Q value of zero is an error if the L-word is non-zero.

-   Mode 0 is the only one permitted for an analog input.

M66 Example Lines

    M66 P0 L3 (wait for digital input 0 to turn on)
    M66 E1 L1 (wait for analog input 1 to rise)

M66 wait on an input stops further execution of the program, until the selected event (or the programmed timeout) occurs.

It is an error to program M66 with both a P-word and an E-word (thus selecting both an analog and a digital input).In Machinekit these inputs are not monitored in real time and thus should not be used for timing-critical applications.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

M67 Synchronized Analog Output<span id="sec:M67-Analog-Output"></span>
----------------------------------------------------------------------

    M67 E- Q-

-   *M67* - set an analog output synchronized with motion.

-   *E-* - output number ranging from 0 to 3.

-   *Q-* - is the value to set (set to 0 to turn off).

The actual change of the specified outputs will happen at the beginning of the next motion command. If there is no subsequent motion command, the queued output changes won’t happen. It’s best to always program a motion G code (G0, G1, etc) right after the M67. M67 functions the same as M62-63.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

M68 Analog Output<span id="sec:M68-Analog-Output"></span>
---------------------------------------------------------

    M68 E- Q-

-   *M68* - set an analog output immediately.

-   *E-* - output number ranging from 0 to 3.

-   *Q-* - is the value to set (set to 0 to turn off).

M68 output happen immediately as they are received by the motion controller. They are not synchronized with movement, and they will break blending. M68 functions the same as M64-65.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

M100 to M199 User Defined Commands<span id="sec:M100-to-M199"></span>
---------------------------------------------------------------------

    M1-- <P- Q->

-   *M1--* - an integer in the range of 100 - 199.

-   *P-* - a number passed to the file as the first parameter.

-   *Q-* - a number passed to the file as the second parameter.

The external program *M1nn* must be in the directory named in \[DISPLAY\] PROGRAM\_PREFIX in the ini file and is executed with the P and Q values as its two arguments. Execution of the RS274/NGC file pauses until the invoked program exits. Any valid executable file can be used.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Warning
</div></td>
<td align="left">Do not use a word processor to create or edit the files. A word processor will leave unseen codes that will cause problems and may prevent a bash or python file from working. Use a text editor like Gedit in Debian or Notepad++ in other operating systems to create or edit the files.</td>
</tr>
</tbody>
</table>

The error *Unknown M code used* denotes one of the following

-   The specified User Defined Command does not exist

-   The file is not an executable file

For example to open and close a collet closer that is controlled by a parallel port pin using a bash script file using M101 and M102. Create two files named M101 and M102. Set them as executable files (typically right click/properties/permissions) before running Machinekit. Make sure the parallel port pin is not connected to anything in a HAL file.

M101 Example File

    #!/bin/bash
    # file to turn on parport pin 14 to open the collet closer
    halcmd setp parport.0.pin-14-out True
    exit 0

M102 Example File

    #!/bin/bash
    # file to turn off parport pin 14 to open the collet closer
    halcmd setp parport.0.pin-14-out False
    exit 0

To pass a variable to a M1nn file you use the P and Q option like this:

    M100 P123.456 Q321.654

M100 Example file

    #!/bin/bash
    voltage=$1
    feedrate=$2
    halcmd setp thc.voltage $voltage
    halcmd setp thc.feedrate $feedrate
    exit 0

To display a graphic message and stop until the message window is closed use a graphic display program like Eye of Gnome to display the graphic file. When you close it the program will resume.

M110 Example file

    #!/bin/bash
    eog /home/john/machinekit/nc_files/message.png
    exit 0

To display a graphic message and continue processing the G code file suffix an ampersand to the command.

M110 Example display and keep going

    #!/bin/bash
    eog /home/john/machinekit/nc_files/message.png &
    exit 0

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


