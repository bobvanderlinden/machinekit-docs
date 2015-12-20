Stepper Configuration
=====================

<span id="stepper-config"></span>

Introduction<span id="sec:Introduction"></span>
-----------------------------------------------

The preferred way to set up a standard stepper machine is with the Step Configuration Wizard. See the Getting Started Guide.

This chapter describes some of the more common settings for manually setting up a stepper based system. Because of the various possibilities of configuring Machinekit, it is very hard to document them all, and keep this document relatively short.

The most common Machinekit usage is for stepper based systems. These systems are using stepper motors with drives that accept step & direction signals.

It is one of the simpler setups, because the motors run open-loop (no feedback comes back from the motors), yet the system needs to be configured properly so the motors don’t stall or lose steps.

Most of this chapter is based on the sample config released along with Machinekit. The config is called stepper, and usually it is found in */etc/emc2/sample-configs/stepper*.

Maximum step rate<span id="sec:Maximum-step-rate"></span>
---------------------------------------------------------

With software step generation, the maximum step rate is one step per two BASE\_PERIODs for step-and-direction output. The maximum requested step rate is the product of an axis' MAX\_VELOCITY and its INPUT\_SCALE. If the requested step rate is not attainable, following errors will occur, particularly during fast jogs and G0 moves.

If your stepper driver can accept quadrature input, use this mode. With a quadrature signal, one step is possible for each BASE\_PERIOD, doubling the maximum step rate.

The other remedies are to decrease one or more of: the BASE\_PERIOD (setting this too low will cause the machine to become unresponsive or even lock up), the INPUT\_SCALE (if you can select different step sizes on your stepper driver, change pulley ratios, or leadscrew pitch), or the MAX\_VELOCITY and STEPGEN\_MAXVEL.

If no valid combination of BASE\_PERIOD, INPUT\_SCALE, and MAX\_VELOCITY is acceptable, then consider using hardware step generation (such as with the Machinekit-supported Universal Stepper Controller, Mesa cards, and others.)

Pinout<span id="sec:Pinout"></span>
-----------------------------------

One of the major flaws in Machinekit was that you couldn’t specify the pinout without recompiling the source code. Machinekit is far more flexible, and now (thanks to the Hardware Abstraction Layer) you can easily specify which signal goes where. See the HAL manual for more detailed information on HAL.

As it is described in the HAL Introduction and tutorial, we have signals, pins and parameters inside the HAL.

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
<td align="left">We are presenting one axis to keep it short, all others are similar.</td>
</tr>
</tbody>
</table>

The ones relevant for our pinout are:

    signals: Xstep, Xdir & Xen
    pins: parport.0.pin-XX-out & parport.0.pin-XX-in

Depending on what you have chosen in your .ini file you are using either standard\_pinout.hal or xylotex\_pinout.hal. These are two files that instruct the HAL how to link the various signals & pins. Further on we’ll investigate the standard\_pinout.hal.

### standard\_pinout.hal<span id="sub:standard_pinout.hal"></span>

This file contains several HAL commands, and usually looks like this:

    # standard pinout config file for 3-axis steppers
    # using a parport for I/O
    #
    # first load the parport driver
    loadrt hal_parport cfg="0x0378"
    #
    # next connect the parport functions to threads
    # read inputs first
    addf parport.0.read base-thread 1
    # write outputs last
    addf parport.0.write base-thread -1
    #
    # finally connect physical pins to the signals
    net Xstep => parport.0.pin-03-out
    net Xdir  => parport.0.pin-02-out
    net Ystep => parport.0.pin-05-out
    net Ydir  => parport.0.pin-04-out
    net Zstep => parport.0.pin-07-out
    net Zdir  => parport.0.pin-06-out

    # create a signal for the estop loopback
    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

    # create signals for tool loading loopback
    net tool-prep-loop iocontrol.0.tool-prepare iocontrol.0.tool-prepared
    net tool-change-loop iocontrol.0.tool-change iocontrol.0.tool-changed

    # connect "spindle on" motion controller pin to a physical pin
    net spindle-on motion.spindle-on => parport.0.pin-09-out

    ###
    ### You might use something like this to enable chopper drives when machine ON
    ### the Xen signal is defined in core_stepper.hal
    ###

    # net Xen => parport.0.pin-01-out

    ###
    ### If you want active low for this pin, invert it like this:
    ###

    # setp parport.0.pin-01-out-invert 1

    ###
    ### A sample home switch on the X axis (axis 0).  make a signal,
    ### link the incoming parport pin to the signal, then link the signal
    ### to Machinekit's axis 0 home switch input pin
    ###

    # net Xhome parport.0.pin-10-in => axis.0.home-sw-in

    ###
    ### Shared home switches all on one parallel port pin?
    ### that's ok, hook the same signal to all the axes, but be sure to
    ### set HOME_IS_SHARED and HOME_SEQUENCE in the ini file.  See the
    ### user manual!
    ###

    # net homeswitches <= parport.0.pin-10-in
    # net homeswitches => axis.0.home-sw-in
    # net homeswitches => axis.1.home-sw-in
    # net homeswitches => axis.2.home-sw-in

    ###
    ### Sample separate limit switches on the X axis (axis 0)
    ###

    # net X-neg-limit parport.0.pin-11-in => axis.0.neg-lim-sw-in
    # net X-pos-limit parport.0.pin-12-in => axis.0.pos-lim-sw-in

    ###
    ### Just like the shared home switches example, you can wire together
    ### limit switches.  Beware if you hit one, Machinekit will stop but can't tell
    ### you which switch/axis has faulted.  Use caution when recovering from this.
    ###

    # net Xlimits parport.0.pin-13-in => axis.0.neg-lim-sw-in axis.0.pos-lim-sw-in

The lines starting with *\#* are comments, and their only purpose is to guide the reader through the file.

### Overview<span id="sub:Overview-standard_pinout.hal"></span>

There are a couple of operations that get executed when the standard\_pinout.hal gets executed/interpreted:

-   The Parport driver gets loaded (see the Parport section of the HAL Manual for details)

-   The read & write functions of the parport driver get assigned to the base thread <span class="footnote">
    \[the fastest thread in the Machinekit setup, usually the code gets executed every few tens of microseconds\]
    </span>

-   The step & direction signals for axes X,Y,Z get linked to pins on the parport

-   Further I/O signals get connected (estop loopback, toolchanger loopback)

-   A spindle-on signal gets defined and linked to a parport pin

### Changing the standard\_pinout.hal<span id="sub:Changing-standard_pinout.hal"></span>

If you want to change the standard\_pinout.hal file, all you need is a text editor. Open the file and locate the parts you want to change.

If you want for example to change the pin for the X-axis Step & Directions signals, all you need to do is to change the number in the *parport.0.pin-XX-out* name:

    net Xstep parport.0.pin-03-out
    net Xdir  parport.0.pin-02-out

can be changed to:

    net Xstep parport.0.pin-02-out
    net Xdir  parport.0.pin-03-out

or basically any other *out* pin you like.

Hint: make sure you don’t have more than one signal connected to the same pin.

### Changing polarity of a signal<span id="sub:Changing-the-polarity"></span>

If external hardware expects an “active low” signal, set the corresponding *-invert* parameter. For instance, to invert the spindle control signal:

    setp parport.0.pin-09-invert TRUE

### Adding PWM Spindle Speed Control<span id="sub:PWM-Spindle-Speed"></span>

If your spindle can be controlled by a PWM signal, use the *pwmgen* component to create the signal:

    loadrt pwmgen output_type=0
    addf pwmgen.update servo-thread
    addf pwmgen.make-pulses base-thread
    net spindle-speed-cmd motion.spindle-speed-out => pwmgen.0.value
    net spindle-on motion.spindle-on => pwmgen.0.enable
    net spindle-pwm pwmgen.0.pwm => parport.0.pin-09-out
    setp pwmgen.0.scale 1800 # Change to your spindle’s top speed in RPM

This assumes that the spindle controller’s response to PWM is simple: 0% PWM gives 0 RPM, 10% PWM gives 180 RPM, etc. If there is a minimum PWM required to get the spindle to turn, follow the example in the *nist-lathe* sample configuration to use a *scale* component.

### Adding an enable signal<span id="sub:Adding-enable-signal"></span>

Some amplifiers (drives) require an enable signal before they accept and command movement of the motors. For this reason there are already defined signals called *Xen*, *Yen*, *Zen*.

To connect them use the following example:

    net Xen parport.0.pin-08-out

You can either have one single pin that enables all drives; or several, depending on the setup you have. Note, however, that usually when one axis faults, all the other drives will be disabled as well, so having only one enable signal / pin for all drives is a common practice.

### External ESTOP button

As you can see in the [standard\_pinout.hal](#sub:standard_pinout.hal) file by default the stepper configuration assumes no external ESTOP button. <span class="footnote">
\[An extensive explanation of hooking up ESTOP circuitry is explained in the wiki.machinekit.org and elsewhere in the Integrator Manual\]
</span>

To add a simple external button you need to replace the line:

    net estop-loop iocontrol.0.user-enable-out iocontrol.0.emc-enable-in

with

    net estop-loop parport.0.pin-01-in iocontrol.0.emc-enable-in

This assumes an ESTOP switch connected to pin 01 on the parport. As long as the switch will stay pushed<span class="footnote">
\[make sure you use a maintained switch for ESTOP.\]
</span>, Machinekit will be in the ESTOP state. When the external button gets released Machinekit will immediately switch to the ESTOP-RESET state, and all you need to do is switch to Machine On and you’ll be able to continue your work with Machinekit.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


