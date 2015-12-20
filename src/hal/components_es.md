HAL Components
==============

<span id="cha:HAL-components"></span>

Commands and Userspace Components<span id="sec:Commands-and-Userspace-Components"></span>
-----------------------------------------------------------------------------------------

All of the commands in the following list have man pages. Some will have expanded descriptions, some will have limited descriptions. Also, all of the components listed below have man pages. From these two lists you know what components exist, and you can use *man n name* to get additional information. To view the information in the man page, in a terminal window type:

    man axis (or perhaps 'man 1 axis' if your system requires it.)

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">axis<br />
</td>
<td align="left"><p>AXIS Machinekit (The Enhanced Machine Controller) Graphical User Interface.</p></td>
</tr>
<tr class="even">
<td align="left">axis-remote<br />
</td>
<td align="left"><p>AXIS Remote Interface.</p></td>
</tr>
<tr class="odd">
<td align="left">comp<br />
</td>
<td align="left"><p>Build, compile and install Machinekit HAL components.</p></td>
</tr>
<tr class="even">
<td align="left">emc<br />
</td>
<td align="left"><p>Machinekit (The Enhanced Machine Controller).</p></td>
</tr>
<tr class="odd">
<td align="left">gladevcp<br />
</td>
<td align="left"><p>Virtual Control Panel for Machinekit based on Glade, Gtk and HAL widgets.</p></td>
</tr>
<tr class="even">
<td align="left">gs2<br />
</td>
<td align="left"><p>HAL userspace component for Automation Direct GS2 VFD’s.</p></td>
</tr>
<tr class="odd">
<td align="left">halcmd<br />
</td>
<td align="left"><p>Manipulate the Enhanced Machine Controller HAL from the command line.</p></td>
</tr>
<tr class="even">
<td align="left">hal_input<br />
</td>
<td align="left"><p>Control HAL pins with any Linux input device, including USB HID devices.</p></td>
</tr>
<tr class="odd">
<td align="left">halmeter<br />
</td>
<td align="left"><p>Observe HAL pins, signals, and parameters.</p></td>
</tr>
<tr class="even">
<td align="left">halrun<br />
</td>
<td align="left"><p>Manipulate the Enhanced Machine Controller HAL from the command line.</p></td>
</tr>
<tr class="odd">
<td align="left">halsampler<br />
</td>
<td align="left"><p>Sample data from HAL in realtime.</p></td>
</tr>
<tr class="even">
<td align="left">halstreamer<br />
</td>
<td align="left"><p>Stream file data into HAL in real time.</p></td>
</tr>
<tr class="odd">
<td align="left">halui<br />
</td>
<td align="left"><p>Observe HAL pins and command Machinekit through NML.</p></td>
</tr>
<tr class="even">
<td align="left">io<br />
</td>
<td align="left"><p>Accepts NML I/O commands, interacts with HAL in userspace.</p></td>
</tr>
<tr class="odd">
<td align="left">iocontrol<br />
</td>
<td align="left"><p>Accepts NML I/O commands, interacts with HAL in userspace.</p></td>
</tr>
<tr class="even">
<td align="left">pyvcp<br />
</td>
<td align="left"><p>Virtual Control Panel for Machinekit.</p></td>
</tr>
<tr class="odd">
<td align="left">shuttlexpress<br />
</td>
<td align="left"><p>control HAL pins with the ShuttleXpress device made by Contour Design.</p></td>
</tr>
</tbody>
</table>

Realtime Components<span id="sec:Realtime-Components"></span>
-------------------------------------------------------------

Some of these will have expanded descriptions from the man pages. Some will have limited descriptions. All of the components have man pages. From this list you know what components exist and can use *man n name* to get additional information in a terminal window.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">abs<br />
</td>
<td align="left"><p>[<span id="sub:abs"></span>Compute the absolute value and sign of the input signal.</p></td>
</tr>
<tr class="even">
<td align="left">and2<br />
</td>
<td align="left"><p>Two-input AND gate. For out to be true both inputs must be true.</p></td>
</tr>
<tr class="odd">
<td align="left">at_pid<br />
</td>
<td align="left"><p>Proportional/integral/derivative controller with auto tuning.</p></td>
</tr>
<tr class="even">
<td align="left">axis<br />
</td>
<td align="left"><p>Accepts NML motion commands, interacts with HAL in realtime.</p></td>
</tr>
<tr class="odd">
<td align="left">biquad<br />
</td>
<td align="left"><p>Biquad IIR filter</p></td>
</tr>
<tr class="even">
<td align="left">bldc_hall3<br />
</td>
<td align="left"><p>3-wire Bipolar trapezoidal commutation BLDC motor driver using Hall sensors.</p></td>
</tr>
<tr class="odd">
<td align="left">blend<br />
</td>
<td align="left"><p>Perform linear interpolation between two values.</p></td>
</tr>
<tr class="even">
<td align="left">charge_pump<br />
</td>
<td align="left"><p>Create a square-wave for the <em>charge pump</em> input of some controller boards.</p></td>
</tr>
<tr class="odd">
<td align="left">clarke2<br />
</td>
<td align="left"><p>Two input version of Clarke transform.</p></td>
</tr>
<tr class="even">
<td align="left">clarke3<br />
</td>
<td align="left"><p>Clarke (3 phase to cartesian) transform.</p></td>
</tr>
<tr class="odd">
<td align="left">clarkeinv<br />
</td>
<td align="left"><p>Inverse Clarke transform.</p></td>
</tr>
<tr class="even">
<td align="left">classicladder<br />
</td>
<td align="left"><p>Realtime software PLC based on ladder logic. See Classic Ladder manual for more information.</p></td>
</tr>
<tr class="odd">
<td align="left">comp<br />
</td>
<td align="left"><p>Two input comparator with hysteresis.</p></td>
</tr>
<tr class="even">
<td align="left">constant<br />
</td>
<td align="left"><p>Use a parameter to set the value of a pin.</p></td>
</tr>
<tr class="odd">
<td align="left">conv_bit_s32<br />
</td>
<td align="left"><p>Convert a value from bit to s32.</p></td>
</tr>
<tr class="even">
<td align="left">conv_bit_u32<br />
</td>
<td align="left"><p>Convert a value from bit to u32.</p></td>
</tr>
<tr class="odd">
<td align="left">conv_float_s32<br />
</td>
<td align="left"><p>Convert a value from float to s32.</p></td>
</tr>
<tr class="even">
<td align="left">conv_float_u32<br />
</td>
<td align="left"><p>Convert a value from float to u32.</p></td>
</tr>
<tr class="odd">
<td align="left">conv_s32_bit<br />
</td>
<td align="left"><p>Convert a value from s32 to bit.</p></td>
</tr>
<tr class="even">
<td align="left">conv_s32_float<br />
</td>
<td align="left"><p>Convert a value from s32 to float.</p></td>
</tr>
<tr class="odd">
<td align="left">conv_s32_u32<br />
</td>
<td align="left"><p>Convert a value from s32 to u32.</p></td>
</tr>
<tr class="even">
<td align="left">conv_u32_bit<br />
</td>
<td align="left"><p>Convert a value from u32 to bit.</p></td>
</tr>
<tr class="odd">
<td align="left">conv_u32_float<br />
</td>
<td align="left"><p>Convert a value from u32 to float.</p></td>
</tr>
<tr class="even">
<td align="left">conv_u32_s32<br />
</td>
<td align="left"><p>Convert a value from u32 to s32.</p></td>
</tr>
<tr class="odd">
<td align="left">counter<br />
</td>
<td align="left"><p>Counts input pulses (deprecated). Use the <em>encoder</em> component See <a href="#sec:Encoder">[sec:Encoder]</a>.</p></td>
</tr>
<tr class="even">
<td align="left">ddt<br />
</td>
<td align="left"><p>Compute the derivative of the input function.</p></td>
</tr>
<tr class="odd">
<td align="left">deadzone<br />
</td>
<td align="left"><p>Return the center if within the threshold.</p></td>
</tr>
<tr class="even">
<td align="left">debounce<br />
</td>
<td align="left"><p>Filter noisy digital inputs, for more information see <a href="#sec:Debounce">[sec:Debounce]</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">edge<br />
</td>
<td align="left"><p>Edge detector.</p></td>
</tr>
<tr class="even">
<td align="left">encoder<br />
</td>
<td align="left"><p>Software counting of quadrature encoder signals, for more information see <a href="#sec:Encoder">[sec:Encoder]</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">encoder_ratio<br />
</td>
<td align="left"><p>An electronic gear to synchronize two axes.</p></td>
</tr>
<tr class="even">
<td align="left">estop_latch<br />
</td>
<td align="left"><p>ESTOP latch.</p></td>
</tr>
<tr class="odd">
<td align="left">feedcomp<br />
</td>
<td align="left"><p>Multiply the input by the ratio of current velocity to the feed rate.</p></td>
</tr>
<tr class="even">
<td align="left">flipflop<br />
</td>
<td align="left"><p>D type flip-flop.</p></td>
</tr>
<tr class="odd">
<td align="left">gantrykins<br />
</td>
<td align="left"><p>A kinematics module that maps one axis to multiple joints.</p></td>
</tr>
<tr class="even">
<td align="left">gearchange<br />
</td>
<td align="left"><p>Select from one of two speed ranges.</p></td>
</tr>
<tr class="odd">
<td align="left">genhexkins<br />
</td>
<td align="left"><p>Gives six degrees of freedom in position and orientation (XYZABC). The location of the motors is defined at compile time.</p></td>
</tr>
<tr class="even">
<td align="left">genserkins<br />
</td>
<td align="left"><p>Kinematics that can model a general serial-link manipulator with up to 6 angular joints.</p></td>
</tr>
<tr class="odd">
<td align="left">gladevcp<br />
</td>
<td align="left"><p>Displays Virtual Control Panels built with GTK/Glade.</p></td>
</tr>
<tr class="even">
<td align="left">hm2_7i43<br />
</td>
<td align="left"><p>HAL driver for the Mesa Electronics 7i43 EPP Anything IO board with HostMot2.</p></td>
</tr>
<tr class="odd">
<td align="left">hm2_pci<br />
</td>
<td align="left"><p>HAL driver for the Mesa Electronics 5i20, 5i22, 5i23, 4i65, and 4i68 Anything I/O boards, with HostMot2 firmware.</p></td>
</tr>
<tr class="even">
<td align="left">hostmot2<br />
</td>
<td align="left"><p>HAL driver for the Mesa Electronics HostMot2 firmware.</p></td>
</tr>
<tr class="odd">
<td align="left">hypot<br />
</td>
<td align="left"><p>Three-input hypotenuse (Euclidean distance) calculator.</p></td>
</tr>
<tr class="even">
<td align="left">ilowpass<br />
</td>
<td align="left"><p>Low-pass filter with integer inputs and outputs.</p></td>
</tr>
<tr class="odd">
<td align="left">integ<br />
</td>
<td align="left"><p>Integrator.</p></td>
</tr>
<tr class="even">
<td align="left">invert<br />
</td>
<td align="left"><p>Compute the inverse of the input signal.</p></td>
</tr>
<tr class="odd">
<td align="left">joyhandle<br />
</td>
<td align="left"><p>Sets nonlinear joypad movements, deadbands and scales.</p></td>
</tr>
<tr class="even">
<td align="left">kins<br />
</td>
<td align="left"><p>kinematics definitions for Machinekit.</p></td>
</tr>
<tr class="odd">
<td align="left">knob2float<br />
</td>
<td align="left"><p>Convert counts (probably from an encoder) to a float value.</p></td>
</tr>
<tr class="even">
<td align="left">limit1<br />
</td>
<td align="left"><p>Limit the output signal to fall between min and max. <span class="footnote"><br />
[When the input is a position, this means that the <em>position</em> is limited.]<br />
</span></p></td>
</tr>
<tr class="odd">
<td align="left">limit2<br />
</td>
<td align="left"><p>Limit the output signal to fall between min and max. Limit its slew rate to less than maxv per second. <span class="footnote"><br />
[When the input is a position, this means that <em>position</em> and <em>velocity</em> are limited.]<br />
</span></p></td>
</tr>
<tr class="even">
<td align="left">limit3<br />
</td>
<td align="left"><p>Limit the output signal to fall between min and max. Limit its slew rate to less than maxv per second. Limit its second derivative to less than MaxA per second squared. <span class="footnote"><br />
[When the input is a position, this means that the <em>position</em>, <em>velocity</em>, and <em>acceleration</em> are limited.]<br />
</span></p></td>
</tr>
<tr class="odd">
<td align="left">logic<br />
</td>
<td align="left"><p>Experimental general logic function component.</p></td>
</tr>
<tr class="even">
<td align="left">lowpass<br />
</td>
<td align="left"><p>Low-pass filter</p></td>
</tr>
<tr class="odd">
<td align="left">lut5<br />
</td>
<td align="left"><p>Arbitrary 5-input logic function based on a look-up table.</p></td>
</tr>
<tr class="even">
<td align="left">maj3<br />
</td>
<td align="left"><p>Compute the majority of 3 inputs.</p></td>
</tr>
<tr class="odd">
<td align="left">match8<br />
</td>
<td align="left"><p>8-bit binary match detector.</p></td>
</tr>
<tr class="even">
<td align="left">maxkins<br />
</td>
<td align="left"><p>Kinematics for a tabletop 5 axis mill named <em>max</em> with tilting head (B axis) and horizontal rotary mounted to the table (C axis). Provides UVW motion in the rotated coordinate system. The source file, maxkins.c, may be a useful starting point for other 5-axis systems.</p></td>
</tr>
<tr class="odd">
<td align="left">mesa_7i65<br />
</td>
<td align="left"><p>Support for the Mesa 7i65 eight-axis servo card.</p></td>
</tr>
<tr class="even">
<td align="left">minmax<br />
</td>
<td align="left"><p>Track the minimum and maximum values of the input to the outputs.</p></td>
</tr>
<tr class="odd">
<td align="left">motion<br />
</td>
<td align="left"><p>Accepts NML motion commands, interacts with HAL in realtime.</p></td>
</tr>
<tr class="even">
<td align="left">mult2<br />
</td>
<td align="left"><p>Product of two inputs.</p></td>
</tr>
<tr class="odd">
<td align="left">mux16<br />
</td>
<td align="left"><p>Select from one of sixteen input values.</p></td>
</tr>
<tr class="even">
<td align="left">mux2<br />
</td>
<td align="left"><p>Select from one of two input values.</p></td>
</tr>
<tr class="odd">
<td align="left">mux4<br />
</td>
<td align="left"><p>Select from one of four input values.</p></td>
</tr>
<tr class="even">
<td align="left">mux8<br />
</td>
<td align="left"><p>Select from one of eight input values.</p></td>
</tr>
<tr class="odd">
<td align="left">near<br />
</td>
<td align="left"><p>Determine whether two values are roughly equal.</p></td>
</tr>
<tr class="even">
<td align="left">not<br />
</td>
<td align="left"><p>Inverter.</p></td>
</tr>
<tr class="odd">
<td align="left">offset<br />
</td>
<td align="left"><p>Adds an offset to an input, and subtracts it from the feedback value.</p></td>
</tr>
<tr class="even">
<td align="left">oneshot<br />
</td>
<td align="left"><p>One-shot pulse generator.</p></td>
</tr>
<tr class="odd">
<td align="left">or2<br />
</td>
<td align="left"><p>Two-input OR gate.</p></td>
</tr>
<tr class="even">
<td align="left">pid<br />
</td>
<td align="left"><p>Proportional/integral/derivative controller, for more information see <a href="#sec:PID">[sec:PID]</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">pluto_servo<br />
</td>
<td align="left"><p>Hardware driver and firmware for the Pluto-P parallel-port FPGA, for use with servos.</p></td>
</tr>
<tr class="even">
<td align="left">pluto_step<br />
</td>
<td align="left"><p>Hardware driver and firmware for the Pluto-P parallel-port FPGA, for use with steppers.</p></td>
</tr>
<tr class="odd">
<td align="left">pumakins<br />
</td>
<td align="left"><p>Kinematics for PUMA-style robots.</p></td>
</tr>
<tr class="even">
<td align="left">pwmgen<br />
</td>
<td align="left"><p>Software PWM/PDM generation, for more information see <a href="#sec:PWMgen">[sec:PWMgen]</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">rotatekins<br />
</td>
<td align="left"><p>The X and Y axes are rotated 45 degrees compared to the joints 0 and 1.</p></td>
</tr>
<tr class="even">
<td align="left">sample_hold<br />
</td>
<td align="left"><p>Sample and Hold.</p></td>
</tr>
<tr class="odd">
<td align="left">sampler<br />
</td>
<td align="left"><p>Sample data from HAL in real time.</p></td>
</tr>
<tr class="even">
<td align="left">scale<br />
</td>
<td align="left"><p>Applies a scale and offset to its input.</p></td>
</tr>
<tr class="odd">
<td align="left">scarakins<br />
</td>
<td align="left"><p>Kinematics for SCARA-type robots.</p></td>
</tr>
<tr class="even">
<td align="left">select8<br />
</td>
<td align="left"><p>8-bit binary match detector.</p></td>
</tr>
<tr class="odd">
<td align="left">serport<br />
</td>
<td align="left"><p>Hardware driver for the digital I/O bits of the 8250 and 16550 serial port.</p></td>
</tr>
<tr class="even">
<td align="left">siggen<br />
</td>
<td align="left"><p>Signal generator, for more information see <a href="#sec:Siggen">[sec:Siggen]</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">sim_encoder<br />
</td>
<td align="left"><p>Simulated quadrature encoder, for more information see <a href="#sec:Simulated-Encoder">[sec:Simulated-Encoder]</a>.</p></td>
</tr>
<tr class="even">
<td align="left">sphereprobe<br />
</td>
<td align="left"><p>Probe a pretend hemisphere.</p></td>
</tr>
<tr class="odd">
<td align="left">stepgen<br />
</td>
<td align="left"><p>Software step pulse generation, for more information see <a href="#sec:Stepgen">[sec:Stepgen]</a>.</p></td>
</tr>
<tr class="even">
<td align="left">steptest<br />
</td>
<td align="left"><p>Used by Stepconf to allow testing of acceleration and velocity values for an axis.</p></td>
</tr>
<tr class="odd">
<td align="left">streamer<br />
</td>
<td align="left"><p>Stream file data into HAL in real time.</p></td>
</tr>
<tr class="even">
<td align="left">sum2<br />
</td>
<td align="left"><p>Sum of two inputs (each with a gain) and an offset.</p></td>
</tr>
<tr class="odd">
<td align="left">supply<br />
</td>
<td align="left"><p>Set output pins with values from parameters (deprecated).</p></td>
</tr>
<tr class="even">
<td align="left">thc<br />
</td>
<td align="left"><p>Torch Height Control using a Mesa THC card.</p></td>
</tr>
<tr class="odd">
<td align="left">threads<br />
</td>
<td align="left"><p>Creates hard realtime HAL threads.</p></td>
</tr>
<tr class="even">
<td align="left">threadtest<br />
</td>
<td align="left"><p>Component for testing thread behavior.</p></td>
</tr>
<tr class="odd">
<td align="left">time<br />
</td>
<td align="left"><p>Accumulated run-time timer counts HH:MM:SS of <em>active</em> input.</p></td>
</tr>
<tr class="even">
<td align="left">timedelay<br />
</td>
<td align="left"><p>The equivalent of a time-delay relay.</p></td>
</tr>
<tr class="odd">
<td align="left">timedelta<br />
</td>
<td align="left"><p>Component that measures thread scheduling timing behavior.</p></td>
</tr>
<tr class="even">
<td align="left">toggle2nist<br />
</td>
<td align="left"><p>Toggle button to nist logic.</p></td>
</tr>
<tr class="odd">
<td align="left">toggle<br />
</td>
<td align="left"><p>Push-on, push-off from momentary pushbuttons.</p></td>
</tr>
<tr class="even">
<td align="left">tripodkins<br />
</td>
<td align="left"><p>The joints represent the distance of the controlled point from three predefined locations (the motors), giving three degrees of freedom in position (XYZ).</p></td>
</tr>
<tr class="odd">
<td align="left">tristate_bit<br />
</td>
<td align="left"><p>Place a signal on an I/O pin only when enabled, similar to a tristate buffer in electronics.</p></td>
</tr>
<tr class="even">
<td align="left">tristate_float<br />
</td>
<td align="left"><p>Place a signal on an I/O pin only when enabled, similar to a tristate buffer in electronics.</p></td>
</tr>
<tr class="odd">
<td align="left">trivkins<br />
</td>
<td align="left"><p>There is a 1:1 correspondence between joints and axes. Most standard milling machines and lathes use the trivial kinematics module.</p></td>
</tr>
<tr class="even">
<td align="left">updown<br />
</td>
<td align="left"><p>Counts up or down, with optional limits and wraparound behavior.</p></td>
</tr>
<tr class="odd">
<td align="left">watchdog<br />
</td>
<td align="left"><p>Monitor one to thirty-two inputs for a <em>heartbeat</em>.</p></td>
</tr>
<tr class="even">
<td align="left">wcomp<br />
</td>
<td align="left"><p>Window comparator.</p></td>
</tr>
<tr class="odd">
<td align="left">weighted_sum<br />
</td>
<td align="left"><p>Convert a group of bits to an integer.</p></td>
</tr>
<tr class="even">
<td align="left">xor2<br />
</td>
<td align="left"><p>Two-input XOR (exclusive OR) gate.</p></td>
</tr>
</tbody>
</table>

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
    undocumented.3rtapi

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


