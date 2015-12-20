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
<td align="left"><p>Pallet Change Pause</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M3-M4-M5">M3 M4 M5</a></p></td>
<td align="left"><p>Spindle Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M6-Tool-Change">M6</a></p></td>
<td align="left"><p>Tool Change</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M7-M8-M9">M7 M8 M9</a></p></td>
<td align="left"><p>Coolant Control</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M19">M19</a></p></td>
<td align="left"><p>Orient Spindle</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M48-M49-Override">M48 M49</a></p></td>
<td align="left"><p>Feed &amp; Spindle Overrides Enable/Disable</p></td>
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
<td align="left"><p><a href="#sec:M62-M65">M62-M65</a></p></td>
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
<td align="left"><p><a href="#sec:M70-Save-Modal-State">M70</a></p></td>
<td align="left"><p>Save Modal State</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M71-Invalidate-Stored-Modal-State">M71</a></p></td>
<td align="left"><p>Invalidate Stored Modal State</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M72-Restore-Modal-State">M72</a></p></td>
<td align="left"><p>Restore Modal State</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="#sec:M73-Save-Autorestore-Modal-State">M73</a></p></td>
<td align="left"><p>Save Autorestore Modal State</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="#sec:M100-to-M199">M100-M199</a></p></td>
<td align="left"><p>User Defined M-Codes</p></td>
</tr>
</tbody>
</table>

M0, M1 Program Pause
--------------------

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

M2, M30 Program End
-------------------

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

M60 Pallet Change Pause
-----------------------

-   *M60* - exchange pallet shuttles and then pause a running program temporarily (regardless of the setting of the optional stop switch). Pressing the cycle start button will restart the program at the following line.

M3, M4, M5 Spindle Control
--------------------------

-   *M3* - start the spindle clockwise at the *S* speed.

-   *M4* - start the spindle counterclockwise at the *S* speed.

-   *M5* - stop the spindle.

It is OK to use *M3* or *M4* if the [S](#sec:S-spindle-speed) spindle speed is set to zero. If this is done (or if the speed override switch is enabled and set to zero), the spindle will not start turning. If, later, the spindle speed is set above zero (or the override switch is turned up), the spindle will start turning. It is OK to use *M3* or *M4* when the spindle is already turning or to use *M5* when the spindle is already stopped.

M6 Tool Change
--------------

### Manual Tool Change

If the HAL component hal\_manualtoolchange is loaded, M6 will stop the spindle and prompt the user to change the tool based on the last *T-* number programmed. For more information on hal\_manualtoolchange see the ([Manual Tool Change](#sec:Manual-Tool-Change)) Section.

### Tool Changer

To change a tool in the spindle from the tool currently in the spindle to the tool most recently selected (using a T word - see Section [Select Tool](#sec:T-Select-Tool)), program *M6*. When the tool change is complete:

-   The spindle will be stopped.

-   The tool that was selected (by a T word on the same line or on any line after the previous tool change) will be in the spindle.

-   If the selected tool was not in the spindle before the tool change, the tool that was in the spindle (if there was one) will be placed back into the tool changer magazine.

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

M7, M8, M9 Coolant Control
--------------------------

-   *M7* - turn mist coolant on.

-   *M8* - turn flood coolant on.

-   *M9* - turn all coolant off.

It is OK to use any of these commands, regardless of the current coolant state.

M19 Orient Spindle
------------------

-   *M19 R- Q- \[P-\]*

-   *R* Position to rotate to from 0, valid range is 0-360 degrees

-   *Q* Number of seconds to wait until orient completes. If motion.spindle.is\_oriented does not become true within Q timeout an error occurs.

-   *P* Direction to rotate to position.

    -   *0* rotate for smallest angular movement (default)

    -   *1* always rotate clockwise (same as M3 direction)

    -   *2* always rotate counterclockwise (same as M4 direction)

M19 is cleared by any of M3,M4,M5.

Spindle orientation requires a differential encoder with an index to sense the spindle shaft position and direction of rotation.

INI Settings in the \[RS274NGC\] section.

ORIENT\_OFFSET = 0-360 (fixed offset in degrees added to M19 R word)

HAL Pins

-   *motion.spindle-orient-angle* (out float) Desired spindle orientation for M19. Value of the M19 R word parameter plus the value of the \[RS274NGC\]ORIENT\_OFFSET ini parameter.

-   *motion.spindle-orient-mode* (out s32) Desired spindle rotation mode. Reflects M19 P parameter word, Default = 0

-   *motion.spindle-orient* (out bit) Indicates start of spindle orient cycle. Set by M19. Cleared by any of M3,M4,M5. If spindle-orient-fault is not zero during spindle-orient true, the M19 command fails with an error message.

-   *motion.spindle-is-oriented* (in bit) Acknowledge pin for spindle-orient. Completes orient cycle. If spindle-orient was true when spindle-is-oriented was asserted, the spindle-orient pin is cleared and the spindle-locked pin is asserted. Also, the spindle-brake pin is asserted.

-   *motion.spindle-orient-fault* (in s32) Fault code input for orient cycle. Any value other than zero will cause the orient cycle to abort.

-   *motion.spindle-locked* (out bit) Spindle orient complete pin. Cleared by any of M3,M4,M5.

M48, M49 Speed and Feed Override Control
----------------------------------------

-   *M48* - enable the spindle speed and feed rate override controls.

-   *M49* - disable both controls.

It is OK to enable or disable the controls when they are already enabled or disabled. See the [Feed Rate](#sub:feed-rate) Section for more details.

M50 Feed Override Control
-------------------------

-   *M50 &lt;P1&gt;* - enable the feed rate override control. The P1 is optional.

-   *M50 P0* - disable the feed rate control.

While disabled the feed override will have no influence, and the motion will be executed at programmed feed rate. (unless there is an adaptive feed rate override active).

M51 Spindle Speed Override Control
----------------------------------

-   *M51 &lt;P1&gt;* - enable the spindle speed override control. The P1 is optional.

-   *M51 P0* - disable the spindle speed override control program. While disabled the spindle speed override will have no influence, and the spindle speed will have the exact program specified value of the S-word (described in [Spindle Speed](#sec:S-spindle-speed) Section).

M52 Adaptive Feed Control
-------------------------

-   *M52 &lt;P1&gt;* - use an adaptive feed. The P1 is optional.

-   *M52 P0* - stop using adaptive feed.

When adaptive feed is enabled, some external input value is used together with the user interface feed override value and the commanded feed rate to set the actual feed rate. In Machinekit, the HAL pin *motion.adaptive-feed* is used for this purpose. Values on *motion.adaptive-feed* should range from 0 (feed hold) to 1 (full speed).

M53 Feed Stop Control
---------------------

-   *M53 &lt;P1&gt;* - enable the feed stop switch. The P1 is optional. Enabling the feed stop switch will allow motion to be interrupted by means of the feed stop control. In Machinekit, the HAL pin *motion.feed-hold* is used for this purpose. A *true* value will cause the motion to stop when *M53* is active.

-   *M53 P0* - disable the feed stop switch. The state of *motion.feed-hold* will have no effect on feed when M53 is not active.

M61 Set Current Tool Number
---------------------------

-   *M61 Q-* - change the current tool number while in MDI or Manual mode. One use is when you power up Machinekit with a tool currently in the spindle you can set that tool number without doing a tool change.

It is an error if:

-   Q- is not 0 or greater

M62 to M65 Output Control
-------------------------

-   *M62 P-* - turn on digital output synchronized with motion. The P- word specifies the digital output number.

-   *M63 P-* - turn off digital output synchronized with motion. The P- word specifies the digital output number.

-   *M64 P-* - turn on digital output immediately. The P- word specifies the digital output number.

-   *M65 P-* - turn off digital output immediately. The P- word specifies the digital output number.

The P-word ranges from 0 to a default value of 3. If needed the the number of I/O can be increased by using the num\_dio parameter when loading the motion controller. See the Integrator’s Manual Configuration Section Machinekit and HAL section for more information.

The M62 & M63 commands will be queued. Subsequent commands referring to the same output number will overwrite the older settings. More than one output change can be specified by issuing more than one M62/M63 command.

The actual change of the specified outputs will happen at the beginning of the next motion command. If there is no subsequent motion command, the queued output changes won’t happen. It’s best to always program a motion G code (G0, G1, etc) right after the M62/63.

M64 & M65 happen immediately as they are received by the motion controller. They are not synchronized with movement, and they will break blending.

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
<td align="left">M62-65 will not function unless the appropriate motion.digital-out-nn pins are connected in your hal file to outputs.</td>
</tr>
</tbody>
</table>

M66 Wait on Input
-----------------

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

    M66 P0 L3 Q5 (wait up to 5 seconds for digital input 0 to turn on)

M66 wait on an input stops further execution of the program, until the selected event (or the programmed timeout) occurs.

It is an error to program M66 with both a P-word and an E-word (thus selecting both an analog and a digital input). In Machinekit these inputs are not monitored in real time and thus should not be used for timing-critical applications.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

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
<td align="left">M66 will not function unless the appropriate motion.digital-in-nn pins or motion.analog-in-nn pins are connected in your hal file to an input.</td>
</tr>
</tbody>
</table>

Example HAL Connection

    net signal-name motion.digital-in-00 <= parport.0.pin10-in

M67 Synchronized Analog Output
------------------------------

    M67 E- Q-

-   *M67* - set an analog output synchronized with motion.

-   *E-* - output number ranging from 0 to 3.

-   *Q-* - is the value to set (set to 0 to turn off).

The actual change of the specified outputs will happen at the beginning of the next motion command. If there is no subsequent motion command, the queued output changes won’t happen. It’s best to always program a motion G code (G0, G1, etc) right after the M67. M67 functions the same as M62-63.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

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
<td align="left">M67 will not function unless the appropriate motion.analog-out-nn pins are connected in your hal file to outputs.</td>
</tr>
</tbody>
</table>

M68 Analog Output
-----------------

    M68 E- Q-

-   *M68* - set an analog output immediately.

-   *E-* - output number ranging from 0 to 3.

-   *Q-* - is the value to set (set to 0 to turn off).

M68 output happen immediately as they are received by the motion controller. They are not synchronized with movement, and they will break blending. M68 functions the same as M64-65.

The number of I/O can be increased by using the num\_dio or num\_aio parameter when loading the motion controller. See the Integrator’s Manual, Core Components Section, Motion subsection, for more information.

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
<td align="left">M68 will not function unless the appropriate motion.analog-out-nn pins are connected in your hal file to outputs.</td>
</tr>
</tbody>
</table>

M70 Save Modal State<span id="sec:M70-Save-Modal-State"></span>
---------------------------------------------------------------

To explicitly save the modal state at the current call level, program *M70*. Once modal state has been saved with *M70*, it can be restored to exactly that state by executing an *M72*.

A pair of *M70* and *M72* instructions will typically be used to protect a program against inadvertant modal changes within subroutines.

The state saved consists of:

-   current G20/G21 settings (imperial/metric)

-   selected plane (G17/G18/G19 G17.1,G18.1,G19.1)

-   status of cutter compensation (G40,G41,G42,G41.1,G42,1)

-   distance mode - relative/absolute (G90/G91)

-   feed mode (G93/G94,G95)

-   current coordinate system (G54-G59.3)

-   tool length compensation status (G43,G43.1,G49)

-   retract mode (G98,G99)

-   spindle mode (G96-css or G97-RPM)

-   arc distance mode (G90.1, G91.1)

-   lathe radius/diameter mode (G7,G8)

-   path control mode (G61, G61.1, G64)

-   current feed and speed (*F* and *S* values)

-   spindle status (M3,M4,M5) - on/off and direction

-   mist (M7) and flood (M8) status

-   speed override (M51) and feed override (M50) settings

-   adaptive feed setting (M52)

-   feed hold setting (M53)

Note that in particular, the motion mode (G1 etc) is NOT restored.

*current call level* means either:

-   executing in the main program. There is a single storage location for state at the main program level; if several *M70* instructions are executed in turn, only the most recently saved state is restored when an *M72* is executed.

-   executing within a G-code subroutine. The state saved with *M70* within a subroutine behaves exactly like a local named parameter - it can be referred to only within this subroutine invocation with an *M72* and when the subroutine exits, the paramter goes away.

A recursive invocation of a subroutine introduces a new call level.

M71 Invalidate Stored Modal State<span id="sec:M71-Invalidate-Stored-Modal-State"></span>
-----------------------------------------------------------------------------------------

[Modal state saved with an *M70*](#saved_state_by_M70) or by an [*M73*](#sec:M73-Save-Autorestore-Modal-State) at the current call level is invalidated (cannot be restored from anymore).

A subsequent *M72* at the same call level will fail.

If executed in a subroutine which protects modal state by an *M73*, a subsequent return or endsub will **not** restore modal state.

The usefulness of this feature is dubious. It should not be relied upon as it might go away.

M72 Restore Modal State<span id="sec:M72-Restore-Modal-State"></span>
---------------------------------------------------------------------

[Modal state saved with an *M70*](#saved_state_by_M70) code can be restored by executing an *M72*.

The handling of G20/G21 is specially treated as feeds are interpreted differently depending on G20/G21: if length units (mm/in) are about to be changed by the restore operation, 'M72 'will restore the distance mode first, and then all other state including feed to make sure the feed value is interpreted in the correct unit setting.

It is an error to execute an *M72* with no previous *M70* save operation at that level.

The following example demonstrates saving and explicitely restoring modal state around a subroutine call using *M70* and *M72*. Note that the *imperialsub* subroutine is not "aware" of the M7x features and can be used unmodified:

M73 Save and Autorestore Modal State<span id="sec:M73-Save-Autorestore-Modal-State"></span>
-------------------------------------------------------------------------------------------

To save modal state within a subroutine, and restore state on subroutine *endsub* or any *return* path, program *M73*.

Aborting a running program in a subroutine which has an *M73* operation will **not** restore state .

Also, the normal end (*M2*) of a main program which contains an *M73* will **not** restore state.

The suggested use is at the beginning of a O-word subroutine as in the following example. Using *M73* this way enables designing subroutines which need to modify modal state but will protect the calling program against inadvertant modal changes. Note the use of [predefined named parameters](#sec:Predefined-Named-Parameters) in the *showstate* subroutine.

### Selectively restoring modal state by testing predefined parameters <span id="sec:Selectively-restoring-modal-state"></span>

Executing an *M72* or returning from a subroutine which contains an *M73* will restore [**all** modal state saved](#saved_state_by_M70).

If only some aspects of modal state should be preserved, an alternative is the usage of [predefined named parameters](#sec:Predefined-Named-Parameters), local parameters and conditional statements. The idea is to remember the modes to be restored at the beginning of the subroutine, and restore these before exiting. Here is an example, based on snippet of *nc\_files/tool-length-probe.ngc*:

M100 to M199 User Defined Commands
----------------------------------

    M1-- <P- Q->

-   *M1--* - an integer in the range of 100 - 199.

-   *P-* - a number passed to the file as the first parameter.

-   *Q-* - a number passed to the file as the second parameter.

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
<td align="left">After creating a new <em>M1nn</em> file you must restart the GUI so it is aware of the new file, otherwise you will get an <em>Unkown m code</em> error.</td>
</tr>
</tbody>
</table>

The external program named *M100* through *M199* (no extension and a capitol M) is executed with the optional P and Q values as its two arguments. Execution of the G code file pauses until the external program exits. Any valid executable file can be used. The file must be located in the search path specificed in the ini file configuration. See the ini config section of the Integrators Manual for more information on search paths.

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

-   The file name has an extension

-   The file name does not follow this format M1nn where nn = 00 through 99

-   The file name used a lower case M

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


