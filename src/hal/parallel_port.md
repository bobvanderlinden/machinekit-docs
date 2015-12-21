Parallel Port Driver
====================

<span id="cha:Parport"></span>

Parport
-------

Parport is a driver for the traditional PC parallel port. The port has a total of 17 physical pins. The original parallel port divided those pins into three groups: data, control, and status. The data group consists of 8 output pins, the control group consists of 4 pins, and the status group consists of 5 input pins.

In the early 1990’s, the bidirectional parallel port was introduced, which allows the data group to be used for output or input. The HAL driver supports the bidirectional port, and allows the user to set the data group as either input or output. If configured as output, a port provides a total of 12 outputs and 5 inputs. If configured as input, it provides 4 outputs and 13 inputs.

In some parallel ports, the control group pins are open collectors, which may also be driven low by an external gate. On a board with open collector control pins, the *x* mode allows a more flexible mode with 8 outputs, and 9 inputs. In other parallel ports, the control group has push-pull drivers and cannot be used as an input.

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
<td align="left"><div class="title">
HAL and Open Collectors
</div>
<div class="paragraph">
<p>HAL cannot automatically determine if the <em>x</em> mode bidirectional pins are actually open collectors (OC). If they are not, they cannot be used as inputs, and attempting to drive them LOW from an external source can damage the hardware.</p>
</div>
<div class="paragraph">
<p>To determine whether your port has <em>open collector</em> pins, load hal_parport in <em>x</em> mode. With no device attached, HAL should read the pin as TRUE. Next, insert a 470 ohm resistor from one of the control pins to GND. If the resulting voltage on the control pin is close to 0V, and HAL now reads the pin as FALSE, then you have an OC port. If the resulting voltage is far from 0V, or HAL does not read the pin as FALSE, then your port cannot be used in <em>x</em> mode.</p>
</div>
<div class="paragraph">
<p>The external hardware that drives the control pins should also use open collector gates (e.g., 74LS05).</p>
</div>
<div class="paragraph">
<p>On some machines, BIOS settings may affect whether <em>x</em> mode can be used. <em>SPP</em> mode is most likely to work.</p>
</div></td>
</tr>
</tbody>
</table>

No other combinations are supported, and a port cannot be changed from input to output once the driver is installed. The [Parport Block Diagram](#fig:Parport-block-diag) shows two block diagrams, one showing the driver when the data group is configured for output, and one showing it configured for input. For *x* mode, refer to the pin listing of *halcmd show pin* for pin direction assignment.

The parport driver can control up to 8 ports (defined by MAX\_PORTS in hal\_parport.c). The ports are numbered starting at zero.

### Installing

    loadrt hal_parport cfg="<config-string>"

Using the Port Index

I/O addresses below 16 are treated as port indexes. This is the simplest way to install the parport driver and cooperates with the Linux parport\_pc driver if it is loaded. This will use the address Linux has detected for parport 0.

    loadrt hal_parport cfg="0"

Using the Port Address

The configure string consists of a hex port address, followed by an optional direction, repeated for each port. The direction is *in*, *out*, or *x* and determines the direction of the physical pins 2 through 9, and whether to create input HAL pins for the physical control pins. If the direction is not specified, the data group defaults to output. For example:

    loadrt hal_parport cfg="0x278 0x378 in 0x20A0 out"

This example installs drivers for one port at 0x0278, with pins 2-9 as outputs (by default, since neither *in* nor *out* was specified), one at 0x0378, with pins 2-9 as inputs, and one at 0x20A0, with pins 2-9 explicitly specified as outputs. Note that you must know the base address of the parallel port to properly configure the driver. For ISA bus ports, this is usually not a problem, since the port is almost always at a *well known* address, like 0278 or 0378 which is typically configured in the system BIOS. The address for a PCI card is usually shown in *lspci -v* in an *I/O ports* line, or in the kernel message log after executing *sudo modprobe -a parport\_pc*. There is no default address; if *&lt;config-string&gt;* does not contain at least one address, it is an error.

![](images/parport-block-diag.png)

Figure 1. Parport Block Diagram

### Pins

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out* (bit) Drives a physical output pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-in* (bit) Tracks a physical input pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-in-not* (bit) Tracks a physical input pin, but inverted.

For each pin, *&lt;p&gt;* is the port number, and *&lt;n&gt;* is the physical pin number in the 25 pin D-shell connector.

For each physical output pin, the driver creates a single HAL pin, for example: *parport.0.pin-14-out*.

Pins 2 through 9 are part of the data group and are output pins if the port is defined as an output port. (Output is the default.) Pins 1, 14, 16, and 17 are outputs in all modes. These HAL pins control the state of the corresponding physical pins.

For each physical input pin, the driver creates two HAL pins, for example: *parport.0.pin-12-in* and *parport.0.pin-12-in-not*.

Pins 10, 11, 12, 13, and 15 are always input pins. Pins 2 through 9 are input pins only if the port is defined as an input port. The *-in* HAL pin is TRUE if the physical pin is high, and FALSE if the physical pin is low. The *-in-not* HAL pin is inverted — it is FALSE if the physical pin is high. By connecting a signal to one or the other, the user can determine the state of the input. In *x* mode, pins 1, 14, 16, and 17 are also input pins.

### Parameters

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out-invert* (bit) Inverts an output pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out-reset* (bit) (only for *out* pins) TRUE if this pin should be reset when the *-reset* function is executed.

-   parport.&lt;p&gt;.reset-time' (U32) The time (in nanoseconds) between a pin is set by *write* and reset by the *reset* function if it is enabled.

The *-invert* parameter determines whether an output pin is active high or active low. If *-invert* is FALSE, setting the HAL *-out* pin TRUE drives the physical pin high, and FALSE drives it low. If *-invert* is TRUE, then setting the HAL *-out* pin TRUE will drive the physical pin low.

### Functions<span id="sub:parport-functions"></span>

-   *parport.&lt;p&gt;.read* (funct) Reads physical input pins of port *&lt;portnum&gt;* and updates HAL *-in* and *-in-not* pins.

-   *parport.read-all* (funct) Reads physical input pins of all ports and updates HAL *-in* and *-in-not* pins.

-   *parport.&lt;p&gt;.write* (funct) Reads HAL *-out* pins of port *&lt;p&gt;* and updates that port’s physical output pins.

-   *parport.write-all* (funct) Reads HAL *-out* pins of all ports and updates all physical output pins.

-   *parport.&lt;p&gt;.reset* (funct) Waits until *reset-time* has elapsed since the associated *write*, then resets pins to values indicated by *-out-invert* and *-out-invert* settings. *reset* must be later in the same thread as *write. 'If '-reset* is TRUE, then the *reset* function will set the pin to the value of *-out-invert*. This can be used in conjunction with stepgen’s *doublefreq* to produce one step per period. The [stepgen stepspace](#sub:stepgen-parameters) for that pin must be set to 0 to enable doublefreq.

The individual functions are provided for situations where one port needs to be updated in a very fast thread, but other ports can be updated in a slower thread to save CPU time. It is probably not a good idea to use both an *-all* function and an individual function at the same time.

### Common problems

If loading the module reports

    insmod: error inserting '/home/jepler/emc2/rtlib/hal_parport.ko':
    -1 Device or resource busy

then ensure that the standard kernel module *parport\_pc* is not loaded<span class="footnote">
\[In the Machinekit packages for Debian, the file /etc/modprobe.d/emc2 generally prevents *parport\_pc* from being automatically loaded.\]
</span> and that no other device in the system has claimed the I/O ports.

If the module loads but does not appear to function, then the port address is incorrect or the *probe\_parport* module is required.

### Using DoubleStep

To setup DoubleStep on the parallel port you must add the function parport.n.reset after parport.n.write and configure stepspace to 0 and the reset time wanted. So that step can be asserted on every period in HAL and then toggled off by parport after being asserted for time specificed by parport.n.reset-time.

For example:

    loadrt hal_parport cfg="0x378 out"
    setp parport.0.reset-time 5000
    loadrt stepgen step_type=0,0,0
    addf parport.0.read base-thread
    addf stepgen.make-pulses base-thread
    addf parport.0.write base-thread
    addf parport.0.reset base-thread
    addf stepgen.capture-position servo-thread
    ...
    setp stepgen.0.steplen 1
    setp stepgen.0.stepspace 0

More information on DoubleStep can be found on the [wiki](http://wiki.machinekit.org/cgi-bin/wiki.pl?TweakingSoftwareStepGeneration).

probe\_parport
--------------

In modern PCs, the parallel port may require plug and play (PNP) configuration before it can be used. The *probe\_parport* module performs configuration of any PNP ports present, and should be loaded before *hal\_parport*. On machines without PNP ports, it may be loaded but has no effect.

### Installing

    loadrt probe_parport

    loadrt hal_parport ...

If the Linux kernel prints a message similar to

    parport: PnPBIOS parport detected.

when the parport\_pc module is loaded (*sudo modprobe -a parport\_pc; sudo rmmod parport\_pc)* then use of this module is probably required.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

