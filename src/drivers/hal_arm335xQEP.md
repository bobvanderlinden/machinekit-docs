arm355x eQEP encoder driver
===========================

<span id="cha:arm355xQEP-driver"></span>

hal\_arm335xQEP is a driver for the 3 hardware quadrature decoders built into the Beaglebone.

Installing:

    loadrt hal_arm335xQEP encoders=eQEP{0-2}[,eQEP{0-2}][,eQEP{0-2}]

In the following Pins and Parameters, &lt;name&gt; is one of eQEP0, eQEP1 or eQEP2 and referes to the names of the hardware instance specified on the loadrt line.

Pins
----

-   *(s32) &lt;name&gt;.counts* - Position in encoder counts.

-   *(bit) &lt;name&gt;.index-enable* - When true, counts and position are reset to zero on the next rising edge of the QEPI signal. At the same time, index-enable is reset to zero to indicate that the rising edge has occurred.

-   *(float) &lt;name&gt;.position* - Position in scaled units (see position-scale).

-   *(float) &lt;name&gt;.position-interpolated* - Position in scaled units, interpolated between encoder countsd.

-   *(float) &lt;name&gt;.position-scale* - Scale factor in counts per length unit. For example of position-scale is 500, then 1000 counts of the encoder will be reported as a position of 2.0 units.

-   *(s32) &lt;name&gt;.rawcounts* - The raw counts from the hardware register, unaffected by reset or index events.

-   *(bit) &lt;name&gt;.reset* - When true, counts and position are reset to zero.

-   *(float) &lt;name&gt;.velocity* - Velocity in scaled units per second, When the magnitude of the true velocity is below the min-speed-estimate, the velocity output is 0.

Parameters
----------

-   *(float) &lt;name&gt;.min-speed-estimate* - Determine the minimum true velocity magnitude at which velocity will be estimated as non zero and position-interpolated will be interpolated. The units of min-speed-estimate are the same as the units of velocity. Setting this parameter too low will cause it to take a lomg time for velocity to go to 0 after the encoder pulses have stopped arriving.

-   *(bit) &lt;name&gt;.counter-mode* - When true QEPA input will provide the clock for the position counter and the QEPB input will have the direction information, defaults to false (quadrature count mode).

-   *(bit) &lt;name&gt;.x2-mode* - When true count rising and falling edges in counter mode, defaults to false;

-   *(bit) &lt;name&gt;.invert-A* - invert polarity of QEPA input.

-   *(bit) &lt;name&gt;.invert-B* - invert polarity of QEPB input.

-   *(bit) &lt;name&gt;.invert-Z* - invert polarity of QEPI input.

Functions
---------

-   *(funct) eqep.update* - Reads all hardware counters and updates calculated values, for all decoders.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


