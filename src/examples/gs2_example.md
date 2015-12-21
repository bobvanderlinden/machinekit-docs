GS2 Spindle
===========

<span id="cha:gs2-spindle"></span>

This example shows the connections needed to use an Automation Direct GS2 VFD to drive a spindle. The spindle speed and direction is controlled by Machinekit.

Using the GS2 component involves very little to set up. We start with a Stepconf Wizard generated config. Make sure the pins with "Spindle CW" and "Spindle PWM" are set to unused in the parallel port setup screen.

In the custom.hal file we place the following to connect Machinekit to the GS2 and have Machinekit control the drive.

GS2 Example

    # load the user space component for the Automation Direct GS2 VFD's
    loadusr -Wn spindle-vfd gs2_vfd -r 9600 -p none -s 2 -n spindle-vfd

    # connect the spindle direction pin to the GS2
    net gs2-fwd spindle-vfd.spindle-fwd <= motion.spindle-forward

    # connect the spindle on pin to the GS2
    net gs2-run spindle-vfd.spindle-on <= motion.spindle-on

    # connect the GS2 at speed to the motion at speed
    net gs2-at-speed motion.spindle-at-speed <= spindle-vfd.at-speed

    # connect the spindle RPM to the GS2
    net gs2-RPM spindle-vfd.speed-command <= motion.spindle-speed-out

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
<td align="left">The transmission speed might be able to be faster depending on the exact envirnment. Both the drive and the command line options must match. To check for transmission errors add the <em>-v</em> command line option and run from a terminal.</td>
</tr>
</tbody>
</table>

On the GS2 drive itself you need to set a couple of things before the modbus communications will work. Other parameters might need to be set based on your physical requirements but these are beyond the scope of this manual. Refer to the GS2 manual that came with the drive for more information on the drive parameters.

-   The communications switches must be set to RS-232C

-   The motor parameters must be set to match the motor

-   P3.00 (Source of Operation Command) must be set to Operation determined by RS-485 interface, 03 or 04

-   P4.00 (Source of Frequency Command) must be set to Frequency determined by RS232C/RS485 communication interface, 05

-   P9.01 (Transmission Speed) must be set to 9600 baud, 01

-   P9.02 (Communication Protocol) must be set to "Modbus RTU mode, 8 data bits, no parity, 2 stop bits", 03

A PyVCP panel based on this example is [here](#gs2-rpm-meter).

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

