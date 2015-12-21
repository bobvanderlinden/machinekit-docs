Servo To Go Driver
==================

<span id="cha:servo-to-go-driver"></span>

The Servo-To-Go (STG) is one of the first PC motion control cards supported by Machinekit. It is an ISA card and it exists in different flavors (all supported by this driver). The board includes up to 8 channels of quadrature encoder input, 8 channels of analog input and output, 32 bits digital I/O, an interval timer with interrupt and a watchdog. For more information see the [Servo To Go](http://www.servotogo.com/) web page.

Installing
----------

    loadrt hal_stg [base=<address>] [num_chan=<nr>] [dio="<dio-string>"] [model=<model>]

The base address field is optional; if it’s not provided the driver attempts to autodetect the board. The num\_chan field is used to specify the number of channels available on the card, if not used the 8 axis version is assumed. The digital inputs/outputs configuration is determined by a config string passed to insmod when loading the module. The format consists of a four character string that sets the direction of each group of pins. Each character of the direction string is either "I" or "O". The first character sets the direction of port A (Port A - DIO.0-7), the next sets port B (Port B - DIO.8-15), the next sets port C (Port C - DIO.16-23), and the fourth sets port D (Port D - DIO.24-31). The model field can be used in case the driver doesn’t autodetect the right card version.

hint: after starting up the driver, *dmesg* can be consulted for messages relevant to the driver (e.g. autodetected version number and base address). For example:

    loadrt hal_stg base=0x300 num_chan=4 dio="IOIO"

This example installs the STG driver for a card found at the base address of 0x300, 4 channels of encoder feedback, DACs and ADCs, along with 32 bits of I/O configured like this: the first 8 (Port A) configured as Input, the next 8 (Port B) configured as Output, the next 8 (Port C) configured as Input, and the last 8 (Port D) configured as Output

    loadrt hal_stg

This example installs the driver and attempts to autodetect the board address and board model, it installs 8 axes by default along with a standard I/O setup: Port A & B configured as Input, Port C & D configured as Output.

Pins
----

-   *stg.&lt;channel&gt;.counts* - (s32) Tracks the counted encoder ticks.

-   *stg.&lt;channel&gt;.position* - (float) Outputs a converted position.

-   *stg.&lt;channel&gt;.dac-value* - (float) Drives the voltage for the corresponding DAC.

-   *stg.&lt;channel&gt;.adc-value* - (float) Tracks the measured voltage from the corresponding ADC.

-   *stg.in-&lt;pinnum&gt;* - (bit) Tracks a physical input pin.

-   *stg.in-&lt;pinnum&gt;-not* - (bit) Tracks a physical input pin, but inverted.

-   *stg.out-&lt;pinnum&gt;* - (bit) Drives a physical output pin

For each pin, &lt;channel&gt; is the axis number, and &lt;pinnum&gt; is the logic pin number of the STG if `IIOO` is defined, there are 16 input pins (in-00 .. in-15) and 16 output pins (out-00 .. out-15), and they correspond to PORTs ABCD (in-00 is PORTA.0, out-15 is PORTD.7).

The in-&lt;pinnum&gt; HAL pin is TRUE if the physical pin is high, and FALSE if the physical pin is low. The in-&lt;pinnum&gt;-not HAL pin is inverted — it is FALSE if the physical pin is high. By connecting a signal to one or the other, the user can determine the state of the input.

Parameters
----------

-   *stg.&lt;channel&gt;.position-scale* - (float) The number of counts / user unit (to convert from counts to units).

-   *stg.&lt;channel&gt;.dac-offset* - (float) Sets the offset for the corresponding DAC.

-   *stg.&lt;channel&gt;.dac-gain* - (float) Sets the gain of the corresponding DAC.

-   *stg.&lt;channel&gt;.adc-offset* - (float) Sets the offset of the corresponding ADC.

-   *stg.&lt;channel&gt;.adc-gain* - (float) Sets the gain of the corresponding ADC.

-   *stg.out-&lt;pinnum&gt;-invert* - (bit) Inverts an output pin.

The -invert parameter determines whether an output pin is active high or active low. If -invert is FALSE, setting the HAL out- pin TRUE drives the physical pin high, and FALSE drives it low. If -invert is TRUE, then setting the HAL out- pin TRUE will drive the physical pin low.

### Functions

-   *stg.capture-position* - Reads the encoder counters from the axis &lt;channel&gt;.

-   *stg.write-dacs* - Writes the voltages to the DACs.

-   *stg.read-adcs* - Reads the voltages from the ADCs.

-   *stg.di-read* - Reads physical in- pins of all ports and updates all HAL in-&lt;pinnum&gt; and in-&lt;pinnum&gt;-not pins.

-   *stg.do-write* - Reads all HAL out-&lt;pinnum&gt; pins and updates all physical output pins.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

