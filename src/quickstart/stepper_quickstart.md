Stepper Quickstart
==================

<span id="cha:stepper-quickstart"></span>

This section assumes you have done a standard install from the Live CD. After installation it is recommended that you connect the computer to the Internet and wait for the update manager to pop up and get the latest updates for Machinekit and Debian before continuing. For more complex installations see the Integrator Manual.

Latency Test
------------

The Latency Test determines how late your computer processor is in responding to a request. Some hardware can interrupt the processing which could cause missed steps when running a CNC machine. This is the first thing you need to do. Follow the instructions [here](../install/Latency_Test.md) to run the latency test.

Sherline
--------

If you have a Sherline several predefined configurations are provided. This is on the main menu CNC/EMC then pick the Sherline configuration that matches yours and save a copy.

Xylotex
-------

If you have a Xylotex you can skip the following sections and go straight to the [Stepper Config Wizard](#cha:stepconf-wizard). Machinekit has provided quick setup for the Xylotex machines.

Machine Information
-------------------

Gather the information about each axis of your machine.

Drive timing is in nano seconds. If you’re unsure about the timing many popular drives are included in the stepper configuration wizard. Note some newer Gecko drives have different timing than the original one. A [list](http://wiki.machinekit.org/) is also on the user maintained Machinekit wiki site of more drives.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Axis</th>
<th align="left">Drive Type</th>
<th align="left">Step Time ns</th>
<th align="left">Step Space ns</th>
<th align="left">Dir. Hold ns</th>
<th align="left">Dir. Setup ns</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Pinout Information
------------------

Gather the information about the connections from your machine to the PC parallel port.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Output Pin</th>
<th align="left">Typ. Function</th>
<th align="left">If Different</th>
<th align="left">Input Pin</th>
<th align="left">Typ. Function</th>
<th align="left">If Different</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>E-Stop Out</p></td>
<td align="left"><p></p></td>
<td align="left"><p>10</p></td>
<td align="left"><p>X Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>X Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>11</p></td>
<td align="left"><p>Y Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>X Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>Z Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Y Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>13</p></td>
<td align="left"><p>A Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Y Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>15</p></td>
<td align="left"><p>Probe In</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Z Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Z Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>A Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>A Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>14</p></td>
<td align="left"><p>Spindle CW</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>16</p></td>
<td align="left"><p>Spindle PWM</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>17</p></td>
<td align="left"><p>Amplifier Enable</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Note any pins not used should be set to Unused in the drop down box. These can always be changed later by running Stepconf again.

Mechanical Information
----------------------

Gather information on steps and gearing. The result of this is steps per user unit which is used for SCALE in the .ini file.

<table>
<colgroup>
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
<col width="16%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Axis</th>
<th align="left">Steps/Rev.</th>
<th align="left">Micro Steps</th>
<th align="left">Motor Teeth</th>
<th align="left">Leadscrew Teeth</th>
<th align="left">Leadscrew Pitch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

-   *Steps per revolution* - is how many stepper-motor-steps it takes to turn the stepper motor one revolution. Typical is 200.

-   *Micro Steps* - is how many steps the drive needs to move the stepper motor one full step. If microstepping is not used, this number will be 1. If microstepping is used the value will depend on the stepper drive hardware.

-   *Motor Teeth and Leadscrew Teeth* - is if you have some reduction (gears, chain, timing belt, etc.) between the motor and the leadscrew. If not, then set these both to 1.

-   *Leadscrew Pitch* - is how much movement occurs (in user units) in one leadscrew turn. If you’re setting up in inches then it is inches per turn. If you’re setting up in millimeters then it is millimeters per turn.

The net result you’re looking for is how many CNC-output-steps it takes to move one user unit (inches or mm).

Example 1. Units inches

    Stepper         = 200 steps per revolution
    Drive           =  10 micro steps per step
    Motor Teeth     =  20
    Leadscrew Teeth =  40
    Leadscrew Pitch =   0.2000 inches per turn

From the above information, the leadscrew moves 0.200 inches per turn. - The motor turns 2.000 times per 1 leadscrew turn. - The drive takes 10 microstep inputs to make the stepper step once. - The drive needs 2000 steps to turn the stepper one revolution. So the scale needed is:

![](images/step-calc-inch-math.png)

Example 2. Units mm

        Stepper         = 200 steps per revolution
        Drive           =   8 micro steps per step
        Motor Teeth     =  30
        Leadscrew Teeth =  90
        Leadscrew Pitch =   5.00 mm per turn

From the above information: - The leadscrew moves 5.00 mm per turn. - The motor turns 3.000 times per 1 leadscrew turn. - The drive takes 8 microstep inputs to make the stepper step once. - The drive needs 1600 steps to turn the stepper one revolution. So the scale needed is:

![](images/step-calc-mm-math.png)

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


