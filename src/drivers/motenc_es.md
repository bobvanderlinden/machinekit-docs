Motenc Driver
=============

<span id="cha:montec-driver"></span>

Vital Systems Motenc-100 and Motenc-LITE

The Vital Systems Motenc-100 and Motenc-LITE are 8- and 4-channel servo control boards. The Motenc-100 provides 8 quadrature encoder counters, 8 analog inputs, 8 analog outputs, 64 (68?) digital inputs, and 32 digital outputs. The Motenc-LITE has only 4 encoder counters, 32 digital inputs and 16 digital outputs, but it still has 8 analog inputs and 8 analog outputs. The driver automatically identifies the installed board and exports the appropriate HAL objects.

Installing:

    loadrt hal_motenc

During loading (or attempted loading) the driver prints some useful debugging messages to the kernel log, which can be viewed with dmesg.

Up to 4 boards may be used in one system.

Pins
----

In the following pins, parameters, and functions, &lt;board&gt; is the board ID. According to the naming conventions the first board should always have an ID of zero. However this driver sets the ID based on a pair of jumpers on the board, so it may be non-zero even if there is only one board.

-   *(s32) motenc.&lt;board&gt;.enc-&lt;channel&gt;-count* - Encoder position, in counts.

-   *(float) motenc.&lt;board&gt;.enc-&lt;channel&gt;-position* - Encoder position, in user units.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-index* - Current status of index pulse input.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-idx-latch* - Driver sets this pin true when it latches an index pulse (enabled by latch-index). Cleared by clearing latch-index.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-latch-index* - If this pin is true, the driver will reset the counter on the next index pulse.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-reset-count* - If this pin is true, the counter will immediately be reset to zero, and the pin will be cleared.

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-value* - Analog output value for DAC (in user units, see -gain and -offset)

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-value* - Analog input value read by ADC (in user units, see -gain and -offset)

-   *(bit) motenc.&lt;board&gt;.in-&lt;channel&gt;* - State of digital input pin, see canonical digital input.

-   *(bit) motenc.&lt;board&gt;.in-&lt;channel&gt;-not* - Inverted state of digital input pin, see canonical digital input.

-   *(bit) motenc.&lt;board&gt;.out-&lt;channel&gt;* - Value to be written to digital output, seen canonical digital output.

-   *(bit) motenc.&lt;board&gt;.estop-in* - Dedicated estop input, more details needed.

-   *(bit) motenc.&lt;board&gt;.estop-in-not* - Inverted state of dedicated estop input.

-   *(bit) motenc.&lt;board&gt;.watchdog-reset* - Bidirectional, - Set TRUE to reset watchdog once, is automatically cleared.

Parameters
----------

-   *(float) motenc.&lt;board&gt;.enc-&lt;channel&gt;-scale* - The number of counts / user unit (to convert from counts to units).

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-offset* - Sets the DAC offset.

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-gain* - Sets the DAC gain (scaling).

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-offset* - Sets the ADC offset.

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-gain* - Sets the ADC gain (scaling).

-   *(bit) motenc.&lt;board&gt;.out-&lt;channel&gt;-invert* - Inverts a digital output, see canonical digital output.

-   *(u32) motenc.&lt;board&gt;.watchdog-control* - Configures the watchdog. The value may be a bitwise OR of the following values:

<table>
<colgroup>
<col width="12%" />
<col width="12%" />
<col width="75%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Bit #</th>
<th align="left">Value</th>
<th align="left">Meaning</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>Timeout is 16ms if set, 8ms if unset</p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>Watchdog is enabled</p></td>
</tr>
<tr class="even">
<td align="left"><p>3</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>4</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>Watchdog is automatically reset by DAC writes (the HAL dac-write function)</p></td>
</tr>
</tbody>
</table>

Typically, the useful values are 0 (watchdog disabled) or 20 (8ms watchdog enabled, cleared by dac-write).

-   *(u32) motenc.&lt;board&gt;.led-view* - Maps some of the I/O to onboard LEDs.

Functions
---------

-   *(funct) motenc.&lt;board&gt;.encoder-read* - Reads all encoder counters.

-   *(funct) motenc.&lt;board&gt;.adc-read* - Reads the analog-to-digital converters.

-   *(funct) motenc.&lt;board&gt;.digital-in-read* - Reads digital inputs.

-   *(funct) motenc.&lt;board&gt;.dac-write* - Writes the voltages to the DACs.

-   *(funct) motenc.&lt;board&gt;.digital-out-write* - Writes digital outputs.

-   *(funct) motenc.&lt;board&gt;.misc-update* - Updates misc stuff.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


