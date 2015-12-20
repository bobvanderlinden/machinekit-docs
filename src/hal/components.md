HAL Components
==============

<span id="cha:HAL-components"></span>

Commands and Userspace Components<span id="sec:Commands-and-Userspace-Components"></span>
-----------------------------------------------------------------------------------------

All of the commands in the following list have man pages. Some will have expanded descriptions, some will have limited descriptions. Also, all of the components listed below have man pages. From these two lists you know what components exist, and you can use *man n name* to get additional information. To view the information in the man page, in a terminal window type:

    man axis (or perhaps 'man 1 axis' if your system requires it.)

Now you can also view the Machinekit specific Instantiated Component man pages online [Here](../../machinekit-documentation/index-instantiated-components.md)

 axis   
AXIS Machinekit (The Enhanced Machine Controller) Graphical User Interface.

 axis-remote   
AXIS Remote Interface.

 comp   
Build, compile and install Machinekit HAL components.

 emc   
Machinekit (The Enhanced Machine Controller).

 gladevcp   
Virtual Control Panel for Machinekit based on Glade, Gtk and HAL widgets.

 gs2   
HAL userspace component for Automation Direct GS2 VFD’s.

 halcmd   
Manipulate the Enhanced Machine Controller HAL from the command line.

 hal\_input   
Control HAL pins with any Linux input device, including USB HID devices.

 halmeter   
Observe HAL pins, signals, and parameters.

 halrun   
Manipulate the Enhanced Machine Controller HAL from the command line.

 halsampler   
Sample data from HAL in realtime.

 halstreamer   
Stream file data into HAL in real time.

 halui   
Observe HAL pins and command Machinekit through NML.

 io   
Accepts NML I/O commands, interacts with HAL in userspace.

 iocontrol   
Accepts NML I/O commands, interacts with HAL in userspace.

 pyvcp   
Virtual Control Panel for Machinekit.

 shuttlexpress   
control HAL pins with the ShuttleXpress device made by Contour Design.

Realtime Components List<span id="sec:Realtime-Components"></span>
------------------------------------------------------------------

Some of these will have expanded descriptions from the man pages. Some will have limited descriptions. All of the components have man pages. From this list you know what components exist and can use *man n name* to get additional information in a terminal window.

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
<td align="left">If the component requires a floating point thread that is usually the slower servo-thread.</td>
</tr>
</tbody>
</table>

### Core Machinekit components<span id="sec:Realtime-Components-core"></span>

 motion   
Accepts NML motion commands, interacts with HAL in realtime.

 axis   
Accepts NML motion commands, interacts with HAL in realtime.

 classicladder   
Realtime software PLC based on ladder logic. See Classic Ladder manual for more information.

 gladevcp   
Displays Virtual Control Panels built with GTK/Glade.

 threads   
Creates hard realtime HAL threads.

### Logic and bitwise components<span id="sec:Realtime-Components-logic"></span>

 and2   
Two-input AND gate. For out to be true both inputs must be true.

 not   
Inverter.

 or2   
Two-input OR gate.

 xor2   
Two-input XOR (exclusive OR) gate.

 debounce   
Filter noisy digital inputs. [Description](#sec:Debounce)

 edge   
Edge detector.

 flipflop   
D type flip-flop.

 oneshot   
One-shot pulse generator.

 logic   
General logic function component.

 lut5   
A 5-input logic function based on a look-up table. [Description](#sec:lut5)

 match8   
8-bit binary match detector.

 select8   
8-bit binary match detector.

### Arithmetic and float-components<span id="sec:Realtime-Components-float"></span>

 abs   
<span id="sub:abs"></span>Compute the absolute value and sign of the input signal.

 blend   
Perform linear interpolation between two values.

 comp   
Two input comparator with hysteresis.

 constant   
Use a parameter to set the value of a pin.

 sum2   
Sum of two inputs (each with a gain) and an offset.

 counter   
Counts input pulses (deprecated). Use the [encoder](#sec:Encoder) component.

 updown   
Counts up or down, with optional limits and wraparound behavior.

 ddt   
Compute the derivative of the input function.

 deadzone   
Return the center if within the threshold.

 hypot   
Three-input hypotenuse (Euclidean distance) calculator.

 mult2   
Product of two inputs.

 mux16   
Select from one of sixteen input values.

 mux2   
Select from one of two input values.

 mux4   
Select from one of four input values.

 mux8   
Select from one of eight input values.

 near   
Determine whether two values are roughly equal.

 offset   
Adds an offset to an input, and subtracts it from the feedback value.

 integ   
Integrator.

 invert   
Compute the inverse of the input signal.

 wcomp   
Window comparator.

 weighted\_sum   
Convert a group of bits to an integer.

 biquad   
Biquad IIR filter

 lowpass   
Low-pass filter

 limit1   
Limit the output signal to fall between min and max. <span class="footnote">
\[When the input is a position, this means that the *position* is limited.\]
</span>

 limit2   
Limit the output signal to fall between min and max. Limit its slew rate to less than maxv per second. <span class="footnote">
\[When the input is a position, this means that *position* and *velocity* are limited.\]
</span>

 limit3   
Limit the output signal to fall between min and max. Limit its slew rate to less than maxv per second. Limit its second derivative to less than MaxA per second squared. <span class="footnote">
\[When the input is a position, this means that the *position*, *velocity*, and *acceleration* are limited.\]
</span>

 maj3   
Compute the majority of 3 inputs.

 scale   
Applies a scale and offset to its input.

### Type conversion<span id="sec:Realtime-Components-typeconvert"></span>

 conv\_bit\_s32   
Convert a value from bit to s32.

 conv\_bit\_u32   
Convert a value from bit to u32.

 conv\_float\_s32   
Convert a value from float to s32.

 conv\_float\_u32   
Convert a value from float to u32.

 conv\_s32\_bit   
Convert a value from s32 to bit.

 conv\_s32\_float   
Convert a value from s32 to float.

 conv\_s32\_u32   
Convert a value from s32 to u32.

 conv\_u32\_bit   
Convert a value from u32 to bit.

 conv\_u32\_float   
Convert a value from u32 to float.

 conv\_u32\_s32   
Convert a value from u32 to s32.

### Hardware drivers<span id="sec:Realtime-Components-hwdrivers"></span>

 hm2\_7i43   
HAL driver for the Mesa Electronics 7i43 EPP Anything IO board with HostMot2.

 hm2\_pci   
HAL driver for the Mesa Electronics 5i20, 5i22, 5i23, 4i65, and 4i68 Anything I/O boards, with HostMot2 firmware.

 hostmot2   
HAL driver for the Mesa Electronics HostMot2 firmware.

 mesa\_7i65   
Support for the Mesa 7i65 eight-axis servo card.

 pluto\_servo   
Hardware driver and firmware for the Pluto-P parallel-port FPGA, for use with servos.

 pluto\_step   
Hardware driver and firmware for the Pluto-P parallel-port FPGA, for use with steppers.

 thc   
Torch Height Control using a Mesa THC card.

 serport   
Hardware driver for the digital I/O bits of the 8250 and 16550 serial port.

### Kinematics<span id="sec:Realtime-Components-kins"></span>

 kins   
kinematics definitions for Machinekit.

 gantrykins   
A kinematics module that maps one axis to multiple joints.

 genhexkins   
Gives six degrees of freedom in position and orientation (XYZABC). The location of the motors is defined at compile time.

 genserkins   
Kinematics that can model a general serial-link manipulator with up to 6 angular joints.

 maxkins   
Kinematics for a tabletop 5 axis mill named *max* with tilting head (B axis) and horizontal rotary mounted to the table (C axis). Provides UVW motion in the rotated coordinate system. The source file, maxkins.c, may be a useful starting point for other 5-axis systems.

 tripodkins   
The joints represent the distance of the controlled point from three predefined locations (the motors), giving three degrees of freedom in position (XYZ).

 trivkins   
There is a 1:1 correspondence between joints and axes. Most standard milling machines and lathes use the trivial kinematics module.

 pumakins   
Kinematics for PUMA-style robots.

 rotatekins   
The X and Y axes are rotated 45 degrees compared to the joints 0 and 1.

 scarakins   
Kinematics for SCARA-type robots.

### Motor control<span id="sec:Realtime-Components-motor"></span>

 at\_pid   
Proportional/integral/derivative controller with auto tuning.

 pid   
Proportional/integral/derivative controller. [Description](#sec:PID)

 pwmgen   
Software PWM/PDM generation. [Description](#sec:PWMgen)

 encoder   
Software counting of quadrature encoder signals. [Description](#sec:Encoder).

 stepgen   
Software step pulse generation. [Description](#sec:Stepgen).

### BLDC and 3-phase motor control<span id="sec:Realtime-Components-bldc"></span>

 bldc\_hall3   
3-wire Bipolar trapezoidal commutation BLDC motor driver using Hall sensors.

 clarke2   
Two input version of Clarke transform.

 clarke3   
Clarke (3 phase to cartesian) transform.

 clarkeinv   
Inverse Clarke transform.

### Other<span id="sec:Realtime-Components-other"></span>

 charge\_pump   
Creates a square-wave for the *charge pump* input of some controller boards. The *Charge Pump* should be added to the base thread function. When enabled the output is on for one period and off for one period. To calculate the frequency of the output 1/(period time in seconds x 2) = hz. For example if you have a base period of 100,000ns that is 0.0001 seconds and the formula would be 1/(0.0001 x 2) = 5,000 hz or 5 Khz.

 encoder\_ratio   
An electronic gear to synchronize two axes.

 estop\_latch   
ESTOP latch.

 feedcomp   
Multiply the input by the ratio of current velocity to the feed rate.

 gearchange   
Select from one of two speed ranges.

 ilowpass   
<span id="ilowpass"></span> While it may find other applications, this component was written to create smoother motion while jogging with an MPG.

In a machine with high acceleration, a short jog can behave almost like a step function. By putting the ilowpass component between the MPG encoder counts output and the axis jog-counts input, this can be smoothed.

Choose scale conservatively so that during a single session there will never be more than about 2e9/scale pulses seen on the MPG. Choose gain according to the smoothing level desired. Divide the axis.N.jog-scale values by scale.

 joyhandle   
Sets nonlinear joypad movements, deadbands and scales.

 knob2float   
Convert counts (probably from an encoder) to a float value.

 minmax   
Track the minimum and maximum values of the input to the outputs.

 sample\_hold   
Sample and Hold.

 sampler   
Sample data from HAL in real time.

 siggen   
Signal generator. [Description](#sec:Siggen).

 sim\_encoder   
Simulated quadrature encoder. [Description](#sec:Simulated-Encoder).

 sphereprobe   
Probe a pretend hemisphere.

 steptest   
Used by Stepconf to allow testing of acceleration and velocity values for an axis.

 streamer   
Stream file data into HAL in real time.

 supply   
Set output pins with values from parameters (deprecated).

 threadtest   
Component for testing thread behavior.

 time   
Accumulated run-time timer counts HH:MM:SS of *active* input.

 timedelay   
The equivalent of a time-delay relay.

 timedelta   
Component that measures thread scheduling timing behavior.

 toggle2nist   
Toggle button to nist logic.

 toggle   
Push-on, push-off from momentary pushbuttons.

 tristate\_bit   
Place a signal on an I/O pin only when enabled, similar to a tristate buffer in electronics.

 tristate\_float   
Place a signal on an I/O pin only when enabled, similar to a tristate buffer in electronics.

 watchdog   
Monitor one to thirty-two inputs for a *heartbeat*.

HAL API calls
-------------

    hal_add_funct_to_thread.3hal
    hal_bit_t.3hal
    hal_create_thread.3hal
    hal_del_funct_from_thread.3hal
    hal_exit.3hal
    hal_export_funct.3hal
    hal_float_t.3hal
    hal_get_lock.3hal
    hal_init.3hal
    hal_link.3hal
    hal_malloc.3hal
    hal_param_bit_new.3hal
    hal_param_bit_newf.3hal
    hal_param_float_new.3hal
    hal_param_float_newf.3hal
    hal_param_new.3hal
    hal_param_s32_new.3hal
    hal_param_s32_newf.3hal
    hal_param_u32_new.3hal
    hal_param_u32_newf.3hal
    hal_parport.3hal
    hal_pin_bit_new.3hal
    hal_pin_bit_newf.3hal
    hal_pin_float_new.3hal
    hal_pin_float_newf.3hal
    hal_pin_new.3hal
    hal_pin_s32_new.3hal
    hal_pin_s32_newf.3hal
    hal_pin_u32_new.3hal
    hal_pin_u32_newf.3hal
    hal_ready.3hal
    hal_s32_t.3hal
    hal_set_constructor.3hal
    hal_set_lock.3hal
    hal_signal_delete.3hal
    hal_signal_new.3hal
    hal_start_threads.3hal
    hal_type_t.3hal
    hal_u32_t.3hal
    hal_unlink.3hal
    intro.3hal
    undocumented.3hal

RTAPI calls
-----------

    EXPORT_FUNCTION.3rtapi
    MODULE_AUTHOR.3rtapi
    MODULE_DESCRIPTION.3rtapi
    MODULE_LICENSE.3rtapi
    RTAPI_MP_ARRAY_INT.3rtapi
    RTAPI_MP_ARRAY_LONG.3rtapi
    RTAPI_MP_ARRAY_STRING.3rtapi
    RTAPI_MP_INT.3rtapi
    RTAPI_MP_LONG.3rtapi
    RTAPI_MP_STRING.3rtapi
    intro.3rtapi
    rtapi_app_exit.3rtapi
    rtapi_app_main.3rtapi
    rtapi_clock_set_period.3rtapi
    rtapi_delay.3rtapi
    rtapi_delay_max.3rtapi
    rtapi_exit.3rtapi
    rtapi_get_clocks.3rtapi
    rtapi_get_msg_level.3rtapi
    rtapi_get_time.3rtapi
    rtapi_inb.3rtapi
    rtapi_init.3rtapi
    rtapi_module_param.3rtapi
    RTAPI_MP_ARRAY_INT.3rtapi
    RTAPI_MP_ARRAY_LONG.3rtapi
    RTAPI_MP_ARRAY_STRING.3rtapi
    RTAPI_MP_INT.3rtapi
    RTAPI_MP_LONG.3rtapi
    RTAPI_MP_STRING.3rtapi
    rtapi_mutex.3rtapi
    rtapi_outb.3rtapi
    rtapi_print.3rtap
    rtapi_prio.3rtapi
    rtapi_prio_highest.3rtapi
    rtapi_prio_lowest.3rtapi
    rtapi_prio_next_higher.3rtapi
    rtapi_prio_next_lower.3rtapi
    rtapi_region.3rtapi
    rtapi_release_region.3rtapi
    rtapi_request_region.3rtapi
    rtapi_set_msg_level.3rtapi
    rtapi_shmem.3rtapi
    rtapi_shmem_delete.3rtapi
    rtapi_shmem_getptr.3rtapi
    rtapi_shmem_new.3rtapi
    rtapi_snprintf.3rtapi
    rtapi_task_delete.3rtpi
    rtapi_task_new.3rtapi
    rtapi_task_pause.3rtapi
    rtapi_task_resume.3rtapi
    rtapi_task_start.3rtapi
    rtapi_task_wait.3rtapi

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


