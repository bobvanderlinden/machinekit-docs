GS2 VFD Driver
==============

<span id="cha:gs2-vfd-driver"></span>

This is a userspace HAL program for the GS2 series of VFD’s at Automation Direct.

This component is loaded using the halcmd "loadusr" command:

    loadusr -Wn spindle-vfd gs2_vfd -n spindle-vfd

The above command says: loadusr, wait for named to load, component gs2\_vfd, named spindle-vfd

Command Line Options
--------------------

-   *-b or --bits &lt;n&gt;* (default 8) Set number of data bits to &lt;n&gt;, where n must be from 5 to 8 inclusive

-   *-d or --device &lt;path&gt;* (default /dev/ttyS0) Set the name of the serial device node to use

-   *-g or --debug* Turn on debugging messages. This will also set the verbose flag. Debug mode will cause all modbus messages to be printed in hex on the terminal.

-   *-n or --name &lt;string&gt;* (default gs2\_vfd) Set the name of the HAL module. The HAL comp name will be set to &lt;string&gt;, and all pin and parameter names will begin with &lt;string&gt;.

-   *-p or --parity {even,odd,none}* (default odd) Set serial parity to even, odd, or none.

-   *-r or --rate &lt;n&gt;* (default 38400) Set baud rate to &lt;n&gt;. It is an error if the rate is not one of the following: 110, 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200

-   *-s or --stopbits {1,2}* (default 1) Set serial stop bits to 1 or 2

-   *-t or --target &lt;n&gt;* (default 1) Set MODBUS target (slave) number. This must match the device number you set on the GS2.

-   *-v or --verbose* Turn on debug messages.

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
<td align="left">That if there are serial configuration errors, turning on verbose may result in a flood of timeout errors.</td>
</tr>
</tbody>
</table>

Pins
----

Where &lt;n&gt; is gs2\_vfd or the name given during loading with the -n option.

-   *&lt;n&gt;.DC-bus-volts* (float, out) The DC bus voltage of the VFD

-   *&lt;n&gt;.at-speed* (bit, out) when drive is at commanded speed

-   *&lt;n&gt;.err-reset* (bit, in) reset errors sent to VFD

-   *&lt;n&gt;.firmware-revision* (s32, out) from the VFD

-   *&lt;n&gt;.frequency-command* (float, out) from the VFD

-   *&lt;n&gt;.frequency-out* (float, out) from the VFD

-   *&lt;n&gt;.is-stopped* (bit, out) when the VFD reports 0 Hz output

-   *&lt;n&gt;.load-percentage* (float, out) from the VFD

-   *&lt;n&gt;.motor-RPM* (float, out) from the VFD

-   *&lt;n&gt;.output-current* (float, out) from the VFD

-   *&lt;n&gt;.output-voltage* (float, out) from the VFD

-   *&lt;n&gt;.power-factor* (float, out) from the VFD

-   *&lt;n&gt;.scale-frequency* (float, out) from the VFD

-   *&lt;n&gt;.speed-command* (float, in) speed sent to VFD in RPM It is an error to send a speed faster than the Motor Max RPM as set in the VFD

-   *&lt;n&gt;.spindle-fwd* (bit, in) 1 for FWD and 0 for REV sent to VFD

-   *&lt;n&gt;.spindle-rev* (bit, in) 1 for REV and 0 if off

-   *&lt;n&gt;.spindle-on* (bit, in) 1 for ON and 0 for OFF sent to VFD

-   *&lt;n&gt;.status-1* (s32, out) Drive Status of the VFD (see the GS2 manual)

-   *&lt;n&gt;.status-2* (s32, out) Drive Status of the VFD (see the GS2 manual)

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
<td align="left">The status value is a sum of all the bits that are on. So a 163 which means the drive is in the run mode is the sum of 3 (run) + 32 (freq set by serial) + 128 (operation set by serial).</td>
</tr>
</tbody>
</table>

Parameters
----------

Where &lt;n&gt; is gs2\_vfd or the name given during loading with the -n option.

-   *&lt;n&gt;.error-count* (s32, RW)

-   *&lt;n&gt;.loop-time* (float, RW) how often the modbus is polled (default 0.1)

-   *&lt;n&gt;.nameplate-HZ* (float, RW) Nameplate Hz of motor (default 60)

-   *&lt;n&gt;.nameplate-RPM* (float, RW) Nameplate RPM of motor (default 1730)

-   *&lt;n&gt;.retval* (s32, RW) the return value of an error in HAL

-   *&lt;n&gt;.tolerance* (s32, RW) speed tolerance (default 0.01)

For an example of using this component to drive a spindle see the [GS2 Spindle](#cha:gs2-spindle) example.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


