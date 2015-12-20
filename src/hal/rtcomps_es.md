HAL Component Descriptions
==========================

Stepgen
-------

This component provides software based generation of step pulses in response to position or velocity commands. In position mode, it has a built in pre-tuned position loop, so PID tuning is not required. In velocity mode, it drives a motor at the commanded speed, while obeying velocity and acceleration limits. It is a realtime component only, and depending on CPU speed, etc, is capable of maximum step rates of 10kHz to perhaps 50kHz. The following figure shows three block diagrams, each is a single step pulse generator. The first diagram is for step type *0*, (step and direction). The second is for step type *1* (up/down, or pseudo-PWM), and the third is for step types 2 through 14 (various stepping patterns). The first two diagrams show position mode control, and the third one shows velocity mode. Control mode and step type are set independently, and any combination can be selected.

Step Pulse Generator Block Diagram position mode

![](images/stepgen-block-diag.png)

Installing

    halcmd: loadrt stepgen step_type=<type-array> [ctrl_type=<ctrl_array>]

*&lt;type-array&gt;* is a series of comma separated decimal integers. Each number causes a single step pulse generator to be loaded, the value of the number determines the stepping type. *&lt;ctrl\_array&gt;* is a comma separated series of *p* or *v* characters, to specify position or velocity mode. *ctrl\_type* is optional, if ommitted, all of the step generators will be position mode.

For example:

    halcmd: loadrt stepgen step_type=0,0,2 ctrl_type=p,p,v

will install three step generators. The first two use step type *0* (step and direction) and run in position mode. The last one uses step type *2* (quadrature) and runs in velocity mode. The default value for *&lt;config-array&gt;* is *0,0,0* which will install three type *0* (step/dir) generators. The maximum number of step generators is 8 (as defined by MAX\_CHAN in stepgen.c). Each generator is independent, but all are updated by the same function(s) at the same time. In the following descriptions, *&lt;chan&gt;* is the number of a specific generator. The first generator is number 0. .Removing

    halcmd: unloadrt stepgen

Pins

Each step pulse generator will have only some of these pins, depending on the step type and control type selected.

-   *(float) stepgen.&lt;chan&gt;.position-cmd* - Desired motor position, in position units (position mode only).

-   *(float) stepgen.&lt;chan&gt;.velocity-cmd* - Desired motor velocity, in position units per second (velocity mode only).

-   *(s32) stepgen.&lt;chan&gt;.counts* - Feedback position in counts, updated by *capture\_position()*.

-   *(float) stepgen.&lt;chan&gt;.position-fb* - Feedback position in position units, updated by *capture\_position()*.

-   *(bit) stepgen.&lt;chan&gt;.enable* - Enables output steps - when false, no steps are generated.

-   *(bit) stepgen.&lt;chan&gt;.step* - Step pulse output (step type 0 only).

-   *(bit) stepgen.&lt;chan&gt;.dir* - Direction output (step type 0 only).

-   *(bit) stepgen.&lt;chan&gt;.up* - UP pseudo-PWM output (step type 1 only).

-   *(bit) stepgen.&lt;chan&gt;.down* - DOWN pseudo-PWM output (step type 1 only).

-   *(bit) stepgen.&lt;chan&gt;.phase-A* - Phase A output (step types 2-14 only).

-   *(bit) stepgen.&lt;chan&gt;.phase-B* - Phase B output (step types 2-14 only).

-   *(bit) stepgen.&lt;chan&gt;.phase-C* - Phase C output (step types 3-14 only).

-   *(bit) stepgen.&lt;chan&gt;.phase-D* - Phase D output (step types 5-14 only).

-   *(bit) stepgen.&lt;chan&gt;.phase-E* - Phase E output (step types 11-14 only).

Parameters

-   *(float) stepgen.&lt;chan&gt;.position-scale* - Steps per position unit. This parameter is used for both output and feedback.

-   *(float) stepgen.&lt;chan&gt;.maxvel* - Maximum velocity, in position units per second. If 0.0, has no effect.

-   *(float) stepgen.&lt;chan&gt;.maxaccel* - Maximum accel/decel rate, in positions units per second squared. If 0.0, has no effect.

-   *(float) stepgen.&lt;chan&gt;.frequency* - The current step rate, in steps per second.

-   *(float) stepgen.&lt;chan&gt;.steplen* - Length of a step pulse (step type 0 and 1) or minimum time in a given state (step types 2-14), in nano-seconds.

-   *(float) stepgen.&lt;chan&gt;.stepspace* - Minimum spacing between two step pulses (step types 0 and 1 only), in nano-seconds. Set to 0 to enable the stepgen *doublefreq* function. To use *doublefreq* the [parport reset function](#sub:parport-functions) must be enabled.

-   *(float) stepgen.&lt;chan&gt;.dirsetup* - Minimum time from a direction change to the beginning of the next step pulse (step type 0 only), in nanoseconds.

-   *(float) stepgen.&lt;chan&gt;.dirhold* - Minmum time from the end of a step pulse to a direction change (step type 0 only), in nanoseconds.

-   *(float) stepgen.&lt;chan&gt;.dirdelay* - Minmum time any step to a step in the opposite direction (step types 1-14 only), in nano-seconds.

-   *(s32) stepgen.&lt;chan&gt;.rawcounts* - The raw feedback count, updated by *make\_pulses()*.

In position mode, the values of maxvel and maxaccel are used by the internal position loop to avoid generating step pulse trains that the motor cannot follow. When set to values that are appropriate for the motor, even a large instantaneous change in commanded position will result in a smooth trapezoidal move to the new location. The algorithm works by measuring both position error and velocity error, and calculating an acceleration that attempts to reduce both to zero at the same time. For more details, including the contents of the *control equation* box, consult the code.

In velocity mode, maxvel is a simple limit that is applied to the commanded velocity, and maxaccel is used to ramp the actual frequency if the commanded velocity changes abruptly. As in position mode, proper values for these parameters ensure that the motor can follow the generated pulse train.

Step Type 0

Step type 0 is the standard step and direction type. When configured for step type 0, there are four extra parameters that determine the exact timing of the step and direction signals. In the following figure the meaning of these parameters is shown. The parameters are in nanoseconds, but will be rounded up to an integer multiple of the thread period for the threaed that calls *make\_pulses()*. For example, if *make\_pulses()* is called every 16 us, and steplen is 20000, then the step pulses will be 2 x 16 = 32 us long. The default value for all four of the parameters is 1ns, but the automatic rounding takes effect the first time the code runs. Since one step requires *steplen* ns high and *stepspace* ns low, the maximum frequency is 1,000,000,000 divided by *(steplen+stepspace)*. If *maxfreq* is set higher than that limit, it will be lowered automatically. If maxfreq is zero, it will remain zero, but the output frequency will still be limited.

When using the parallel port driver the step frequency can be doubled using the [parport reset](#sub:parport-functions) function together with stepgen’s *doublefreq* setting.

![](images/stepgen-type0.png)

Figure 1. Step and Direction Timing

Step Type 1

Step type 1 has two outputs, up and down. Pulses appear on one or the other, depending on the direction of travel. Each pulse is *steplen* ns long, and the pulses are separated by at least *stepspace* ns. The maximum frequency is the same as for step type 0. If *maxfreq* is set higher than the limit it will be lowered. If *maxfreq* is zero, it will remain zero but the output frequency will still be limited.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Warning
</div></td>
<td align="left">Do not use the parport reset function with step types 2 - 14. Unexpected results can happen.</td>
</tr>
</tbody>
</table>

Step Type 2 - 14

Step types 2 through 14 are state based, and have from two to five outputs. On each step, a state counter is incremented or decremented. Figures [Two-and-Three-Phase](#fig:Two-and-Three-Phase), [Four-Phase](#fig:Four-Phase), and [Five-Phase](#fig:Five-Phase) show the output patterns as a function of the state counter. The maximum frequency is 1,000,000,000 divided by *steplen*, and as in the other modes, *maxfreq* will be lowered if it is above the limit.

Two-and-Three-Phase Step Types

![](images/stepgen-type2-4.png)

Four-Phase Step Types

![](images/stepgen-type5-10.png)

Five-Phase Step Types

![](images/stepgen-type11-14.png)

Functions

The component exports three functions. Each function acts on all of the step pulse generators - running different generators in different threads is not supported.

-   *(funct) stepgen.make-pulses* - High speed function to generate and count pulses (no floating point).

-   *(funct) stepgen.update-freq* - Low speed function does position to velocity conversion, scaling and limiting.

-   *(funct) stepgen.capture-position* - Low speed function for feedback, updates latches and scales position.

The high speed function *stepgen.make-pulses* should be run in a very fast thread, from 10 to 50 us depending on the capabilities of the computer. That thread’s period determines the maximum step frequency, since *steplen*, *stepspace*, *dirsetup*, *dirhold*, and *dirdelay* are all rounded up to a integer multiple of the thread periond in nanoseconds. The other two functions can be called at a much lower rate.

PWMgen
------

This component provides software based generation of PWM (Pulse Width Modulation) and PDM (Pulse Density Modulation) waveforms. It is a realtime component only, and depending on CPU speed, etc, is capable of PWM frequencies from a few hundred Hertz at pretty good resolution, to perhaps 10KHz with limited resolution.

Installing

    loadrt pwmgen output_type=<config-array>

The *&lt;config-array&gt;* is a series of comma separated decimal integers. Each number causes a single PWM generator to be loaded, the value of the number determines the output type. The following example will install three PWM generators. There is no default value, if *&lt;config-array&gt;* is not specified, no PWM generators will be installed. The maximum number of frequency generators is 8 (as defined by MAX\_CHAN in pwmgen.c). Each generator is independent, but all are updated by the same function(s) at the same time. In the following descriptions, *&lt;chan&gt;* is the number of a specific generator. The first generator is number 0.

Example

    loadrt pwmgen output_type=0,1,2

Removing

    unloadrt pwmgen

Output Types

The PWM generator supports three different *output types*.

-   *Output type 0* - PWM output pin only. Only positive commands are accepted, negative values are treated as zero (and will be affected by the parameter *min-dc* if it is non-zero).

-   *Output type 1* - PWM/PDM and direction pins. Positive and negative inputs will be output as positive and negative PWM. The direction pin is false for positive commands, and true for negative commands. If your control needs positive PWM for both CW and CCW use the [abs](#sub:abs) component to convert your PWM signal to positive value when a negative input is input.

-   *Output type 2* - UP and DOWN pins. For positive commands, the PWM signal appears on the up output, and the down output remains false. For negative commands, the PWM signal appears on the down output, and the up output remains false. Output type 2 is suitable for driving most H-bridges.

Pins

Each PWM generator will have the following pins:

-   *(float) pwmgen.&lt;chan&gt;.value* - Command value, in arbitrary units. Will be scaled by the *scale* parameter (see below).

-   *(bit) pwmgen.&lt;chan&gt;.enable* - Enables or disables the PWM generator outputs.

Each PWM generator will also have some of these pins, depending on the output type selected:

-   *(bit) pwmgen.&lt;chan&gt;.pwm* - PWM (or PDM) output, (output types 0 and 1 only).

-   *(bit) pwmgen.&lt;chan&gt;.dir* - Direction output (output type 1 only).

-   *(bit) pwmgen.&lt;chan&gt;.up* - PWM/PDM output for positive input value (output type 2 only).

-   *(bit) pwmgen.&lt;chan&gt;.down* - PWM/PDM output for negative input value (output type 2 only).

Parameters

-   *(float) pwmgen.&lt;chan&gt;.scale* - Scaling factor to convert *value* from arbitrary units to duty cycle.

-   *(float) pwmgen.&lt;chan&gt;.pwm-freq* - Desired PWM frequency, in Hz. If 0.0, generates PDM instead of PWM. If set higher than internal limits, next call of *update\_freq()* will set it to the internal limit. If non-zero, and *dither* is false, next call of *update\_freq()* will set it to the nearest integer multiple of the *make\_pulses()* function period.

-   *(bit) pwmgen.&lt;chan&gt;.dither-pwm* - If true, enables dithering to achieve average PWM frequencies or duty cycles that are unobtainable with pure PWM. If false, both the PWM frequency and the duty cycle will be rounded to values that can be achieved exactly.

-   *(float) pwmgen.&lt;chan&gt;.min-dc* - Minimum duty cycle, between 0.0 and 1.0 (duty cycle will go to zero when disabled, regardless of this setting).

-   *(float) pwmgen.&lt;chan&gt;.max-dc* - Maximum duty cycle, between 0.0 and 1.0.

-   *(float) pwmgen.&lt;chan&gt;.curr-dc* - Current duty cycle - after all limiting and rounding (read only).

Functions

The component exports two functions. Each function acts on all of the PWM generators - running different generators in different threads is not supported.

-   *(funct) pwmgen.make-pulses* - High speed function to generate PWM waveforms (no floating point).

-   *(funct) pwmgen.update* - Low speed function to scale and limit value and handle other parameters.

The high speed function *pwmgen.make-pulses* should be run in a very fast thread, from 10 to 50 us depending on the capabilities of the computer. That thread’s period determines the maximum PWM carrier frequency, as well as the resolution of the PWM or PDM signals. The other function can be called at a much lower rate.

Encoder
-------

This component provides software based counting of signals from quadrature encoders. It is a realtime component only, and depending on CPU speed, latency, etc, is capable of maximum count rates of 10kHz to perhaps up to 50kHz.

The base thread should be 1/2 count speed to allow for noise and timing variation. For example if you have a 100 pulse per revolution encoder on the spindle and your maximnum RPM is 3000 the maximum base thread should be 25 us. A 100 pulse per revolution encoder will have 400 counts. The spindle speed of 3000 RPM = 50 RPS (revolutions per second). 400 \* 50 = 20,000 counts per second or 50 us between counts.

Figure [Encoder Counter Block Diagram](#fig:Encoder-Block-Diag) is a block diagram of one channel of encoder counter.

Encoder Counter Block Diagram

![](images/encoder-block-diag.png)

Installing

    halcmd: loadrt encoder [num_chan=<counters>]

*&lt;counters&gt;* is the number of encoder counters that you want to install. If *numchan* is not specified, three counters will be installed. The maximum number of counters is 8 (as defined by MAX\_CHAN in encoder.c). Each counter is independent, but all are updated by the same function(s) at the same time. In the following descriptions, *&lt;chan&gt;* is the number of a specific counter. The first counter is number 0.

Removing

    halcmd: unloadrt encoder

Pins

-   *encoder.&lt;chan&gt;.counter-mode* (bit, I/O) (default: FALSE) - Enables counter mode. When true, the counter counts each rising edge of the phase-A input, ignoring the value on phase-B. This is useful for counting the output of a single channel (non-quadrature) sensor. When false, it counts in quadrature mode.

-   *encoder.&lt;chan&gt;.counts* (s32, Out) - Position in encoder counts.

-   *encoder.&lt;chan&gt;.counts-latched* (s32, Out) - Not used at this time.

-   *encoder.&lt;chan&gt;.index-enable* (bit, I/O) - When True, *counts* and *position are* reset to zero on next rising edge of Phase Z. At the same time, *index-enable* is reset to zero to indicate that the rising edge has occoured. The *index-enable* pin is bi-directional. If *index-enable* is False, the Phase Z channel of the encoder will be ignored, and the counter will count normally. The encoder driver will never set *index-enable* True. However, some other component may do so.

-   *encoder.&lt;chan&gt;.latch-falling* (bit, In) (default: TRUE) - Not used at this time.

-   *encoder.&lt;chan&gt;.latch-input* (bit, In) (default: TRUE) - Not used at this time.

-   *encoder.&lt;chan&gt;.latch-rising* (bit, In) - Not used at this time.

-   *encoder.&lt;chan&gt;.min-speed-estimate* (float, in) - Determine the minimum true velocity magnitude at which velocity will be estimated as nonzero and postition-interpolated will be interpolated. The units of *min-speed-estimate* are the same as the units of *velocity* . Scale factor, in counts per length unit. Setting this parameter too low will cause it to take a long time for velocity to go to 0 after encoder pulses have stopped arriving.

-   *encoder.&lt;chan&gt;.phase-A* (bit, In) - Phase A of the quadrature encoder signal.

-   *encoder.&lt;chan&gt;.phase-B* (bit, In) - Phase B of the quadrature encoder signal.

-   *encoder.&lt;chan&gt;.phase-Z* (bit, In) - Phase Z (index pulse) of the quadrature encoder signal.

-   *encoder.&lt;chan&gt;.position* (float, Out) - Position in scaled units (see *position-scale*).

-   *encoder.&lt;chan&gt;.position-interpolated* (float, Out) - Position in scaled units, interpolated between encoder counts. The *position-interpolated* attempts to interpolate between encoder counts, based on the most recently measured velocity. Only valid when velocity is approximately constant and above *min-speed-estimate*. Do not use for position control, since its value is incorrect at low speeds, during direction reversals, and during speed changes. However, it allows a low ppr encoder (including a one pulse per revolution *encoder*) to be used for lathe threading, and may have other uses as well.

-   *encoder.&lt;chan&gt;.position-latched (float, Out)* - Not used at this time.

-   *encoder.&lt;chan&gt;.position-scale (float, I/O)* - Scale factor, in counts per length unit. For example, if position-scale is 500, then 1000 counts of the encoder will be reported as a position of 2.0 units.

-   *encoder.&lt;chan&gt;.rawcounts (s32, In)* - The raw count, as determined by update-counters. This value is updated more frequently than counts and position. It is also unaffected by reset or the index pulse.

-   *encoder.&lt;chan&gt;.reset* (bit, In) - When True, force *counts* and *position* to zero immediately.

-   *encoder.&lt;chan&gt;.velocity* (float, Out) - Velocity in scaled units per second. *encoder* uses an algorithm that greatly reduces quantization noise as compared to simply differentiating the *position* output. When the magnitude of the true velocity is below min-velocity-estimate, the velocity output is 0.

-   *encoder.&lt;chan&gt;.x4-mode (bit, I/O) (default: TRUE)* - Enables times-4 mode. When true, the counter counts each edge of the quadrature waveform (four counts per full cycle). When false, it only counts once per full cycle. In counter-mode, this parameter is ignored. The 1x mode is useful for some jogwheels.

Parameters

-   *encoder.&lt;chan&gt;.capture-position.time (s32, RO)*

-   *encoder.&lt;chan&gt;.capture-position.tmax (s32, RW)*

-   *encoder.&lt;chan&gt;.update-counters.time (s32, RO)*

-   *encoder.&lt;chan&gt;.update-counter.tmax (s32, RW)*

Functions

The component exports two functions. Each function acts on all of the encoder counters - running different counters in different threads is not supported.

-   *(funct) encoder.update-counters* - High speed function to count pulses (no floating point).

-   *(funct) encoder.capture-position* - Low speed function to update latches and scale position.

PID
---

This component provides Proportional/Integral/Derivative control loops. It is a realtime component only. For simplicity, this discussion assumes that we are talking about position loops, however this component can be used to implement other feedback loops such as speed, torch height, temperature, etc. Figure [PID Loop Block Diagram](#fig:PID-block-diag) is a block diagram of a single PID loop.

PID Loop Block Diagram

![](images/pid-block-diag.png)

Installing

    halcmd: loadrt pid [num_chan=<loops>] [debug=1]

*&lt;loops&gt;* is the number of PID loops that you want to install. If *numchan* is not specified, one loop will be installed. The maximum number of loops is 16 (as defined by MAX\_CHAN in pid.c). Each loop is completely independent. In the following descriptions, *&lt;loopnum&gt;* is the loop number of a specific loop. The first loop is number 0.

If *debug=1* is specified, the component will export a few extra parameters that may be useful during debugging and tuning. By default, the extra parameters are not exported, to save shared memory space and avoid cluttering the parameter list.

Removing

    halcmd: unloadrt pid

Pins

The three most important pins are

-   *(float) pid.&lt;loopnum&gt;.command* - The desired position, as commanded by another system component.

-   *(float) pid.&lt;loopnum&gt;.feedback* - The present position, as measured by a feedback device such as an encoder.

-   *(float) pid.&lt;loopnum&gt;.output* - A velocity command that attempts to move from the present position to the desired position.

For a position loop, *command* and *feedback* are in position units. For a linear axis, this could be inches, mm, meters, or whatever is relevant. Likewise, for an angular axis, it could be degrees, radians, etc. The units of the *output* pin represent the change needed to make the feedback match the command. As such, for a position loop *Output* is a velocity, in inches/sec, mm/sec, degrees/sec, etc. Time units are always seconds, and the velocity units match the position units. If command and feedback are in meters, then output is in meters per second.

Each loop has two pins which are used to monitor or control the general operation of the component.

-   *(float) pid.&lt;loopnum&gt;.error* - Equals *.command* minus *.feedback*.

-   *(bit) pid.&lt;loopnum&gt;.enable* - A bit that enables the loop. If *.enable* is false, all integrators are reset, and the output is forced to zero. If *.enable* is true, the loop operates normally.

Pins used to report saturation. Saturation occurs when the output of the PID block is at its maximum or minimum limit.

-   *(bit) pid.&lt;loopnum&gt;.saturated* - True when output is saturated.

-   *(float) pid.&lt;loopnum&gt;.saturated\_s* - The time the output has been saturated.

-   *(s32) pid.&lt;loopnum&gt;.saturated\_count* - The time the output has been saturated.

Parameters

The PID gains, limits, and other *tunable* features of the loop are implemented as parameters.

-   *(float) pid.&lt;loopnum&gt;.Pgain* - Proportional gain

-   *(float) pid.&lt;loopnum&gt;.Igain* - Integral gain

-   *(float) pid.&lt;loopnum&gt;.Dgain* - Derivative gain

-   *(float) pid.&lt;loopnum&gt;.bias* - Constant offset on output

-   *(float) pid.&lt;loopnum&gt;.FF0* - Zeroth order feedforward - output proportional to command (position).

-   *(float) pid.&lt;loopnum&gt;.FF1* - First order feedforward - output proportional to derivative of command (velocity).

-   *(float) pid.&lt;loopnum&gt;.FF2* - Second order feedforward - output proportional to 2nd derivative of command (acceleration)<span class="footnote">
    \[FF2 is not currently implemented, but it will be added. Consider this note a “FIXME” for the code\]
    </span>.

-   *(float) pid.&lt;loopnum&gt;.deadband* - Amount of error that will be ignored

-   *(float) pid.&lt;loopnum&gt;.maxerror* - Limit on error

-   *(float) pid.&lt;loopnum&gt;.maxerrorI* - Limit on error integrator

-   *(float) pid.&lt;loopnum&gt;.maxerrorD* - Limit on error derivative

-   *(float) pid.&lt;loopnum&gt;.maxcmdD* - Limit on command derivative

-   *(float) pid.&lt;loopnum&gt;.maxcmdDD* - Limit on command 2nd derivative

-   *(float) pid.&lt;loopnum&gt;.maxoutput* - Limit on output value

All of the *max* limits are implemented such that if the parameter value is zero, there is no limit.

If *debug=1* was specified when the component was installed, four additional parameters will be exported:

-   *(float) pid.&lt;loopnum&gt;.errorI* - Integral of error.

-   *(float) pid.&lt;loopnum&gt;.errorD* - Derivative of error.

-   *(float) pid.&lt;loopnum&gt;.commandD* - Derivative of the command.

-   *(float) pid.&lt;loopnum&gt;.commandDD* - 2nd derivative of the command.

Functions

The component exports one function for each PID loop. This function performs all the calculations needed for the loop. Since each loop has its own function, individual loops can be included in different threads and execute at different rates.

-   *(funct) pid.&lt;loopnum&gt;.do\_pid\_calcs* - Performs all calculations for a single PID loop.

If you want to understand the exact algorithm used to compute the output of the PID loop, refer to figure [PID Loop Block Diagram](#fig:PID-block-diag), the comments at the beginning of *emc2/src/hal/components/pid.c* , and of course to the code itself. The loop calculations are in the C function *calc\_pid()*.

Simulated Encoder<span id="sec:Simulated-Encoder"></span>
---------------------------------------------------------

The simulated encoder is exactly that. It produces quadrature pulses with an index pulse, at a speed controlled by a HAL pin. Mostly useful for testing.

Installing

    halcmd: loadrt sim-encoder num_chan=<number>

*&lt;number&gt;* is the number of encoders that you want to simulate. If not specified, one encoder will be installed. The maximum number is 8 (as defined by MAX\_CHAN in sim\_encoder.c).

Removing

    halcmd: unloadrt sim-encoder

Pins

-   *(float) sim-encoder.&lt;chan-num&gt;.speed* - The speed command for the simulated shaft.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-A* - Quadrature output.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-B* - Quadrature output.

-   *(bit) sim-encoder.&lt;chan-num&gt;.phase-Z* - Index pulse output.

When *.speed* is positive, *.phase-A* leads *.phase-B*.

Parameters

-   *(u32) sim-encoder.&lt;chan-num&gt;.ppr* - Pulses Per Revolution.

-   *(float) sim-encoder.&lt;chan-num&gt;.scale* - Scale Factor for *speed*. The default is 1.0, which means that *speed* is in revolutions per second. Change to 60 for RPM, to 360 for degrees per second, 6.283185 for radians per seconed, etc.

Note that pulses per revolution is not the same as counts per revolution. A pulse is a complete quadrature cycle. Most encoder counters will count four times during one complete cycle.

Functions

The component exports two functions. Each function affects all simulated encoders.

-   *(funct) sim-encoder.make-pulses* - High speed function to generate quadrature pulses (no floating point).

-   *(funct) sim-encoder.update-speed* - Low speed function to read *speed*, do scaling, and set up *make-pulses*.

Debounce
--------

Debounce is a realtime component that can filter the glitches created by mechanical switch contacts. It may also be useful in other applications where short pulses are to be rejected.

Installing

    halcmd: loadrt debounce cfg=<config-string>

*&lt;config-string&gt;* is a series of comma separated decimal integers. Each number installs a group of identical debounce filters, the number determines how many filters are in the group.

For example:

    halcmd: loadrt debounce cfg=1,4,2

will install three groups of filters. Group 0 contains one filter, group 1 contains four, and group 2 contains two filters. The default value for *&lt;config-string&gt;* is *"1"* which will install a single group containing a single filter. The maximum number of groups 8 (as defined by MAX\_GROUPS in debounce.c). The maximum number of filters in a group is limited only by shared memory space. Each group is completely independent. All filters in a single group are identical, and they are all updated by the same function at the same time. In the following descriptions, *&lt;G&gt;* is the group number and *&lt;F&gt;* is the filter number within the group. The first filter is group 0, filter 0.

Removing

    halcmd: unloadrt debounce

Pins

Each individual filter has two pins.

-   *(bit) debounce.&lt;G&gt;.&lt;F&gt;.in* - Input of filter *&lt;F&gt;* in group *&lt;G&gt;*.

-   *(bit) debounce.&lt;G&gt;.&lt;F&gt;.out* - Output of filter *&lt;F&gt;* in group *&lt;G&gt;*.

Parameters

Each group of filters has one parameter<span class="footnote">
\[Each individual filter also has an internal state variable. There is a compile time switch that can export that variable as a parameter. This is intended for testing, and simply wastes shared memory under normal circumstances.\]
</span>.

-   *(s32) debounce.&lt;G&gt;.delay* - Filter delay for all filters in group *&lt;G&gt;*.

The filter delay is in units of thread periods. The minimum delay is zero. The output of a zero delay filter exactly follows its input - it doesn’t filter anything. As *delay* increases, longer and longer glitches are rejected. If *delay* is 4, all glitches less than or equal to four thread periods will be rejected.

Functions

Each group of filters has one function, which updates all the filters in that group *simultaneously*. Different groups of filters can be updated from different threads at different periods.

-   *(funct) debounce.&lt;G&gt;* - Updates all filters in group *&lt;G&gt;*.

Siggen<span id="sec:Siggen"></span>
-----------------------------------

Siggen is a realtime component that generates square, triangle, and sine waves. It is primarily used for testing.

Installing

    halcmd: loadrt siggen [num_chan=<chans>]

*&lt;chans&gt;* is the number of signal generators that you want to install. If *numchan* is not specified, one signal generator will be installed. The maximum number of generators is 16 (as defined by MAX\_CHAN in siggen.c). Each generator is completely independent. In the following descriptions, *&lt;chan&gt;* is the number of a specific signal generator (the numbers start at 0).

Removing

    halcmd: unloadrt siggen

Pins

Each generator has five output pins.

-   *(float) siggen.&lt;chan&gt;.sine* - Sine wave output.

-   *(float) siggen.&lt;chan&gt;.cosine* - Cosine output.

-   *(float) siggen.&lt;chan&gt;.sawtooth* - Sawtooth output.

-   *(float) siggen.&lt;chan&gt;.triangle* - Triangle wave output.

-   *(float) siggen.&lt;chan&gt;.square* - Square wave output.

All five outputs have the same frequency, amplitude, and offset.

In addition to the output pins, there are three control pins:

-   *(float) siggen.&lt;chan&gt;.frequency* - Sets the frequency in Hertz, default value is 1 Hz.

-   *(float) siggen.&lt;chan&gt;.amplitude* - Sets the peak amplitude of the output waveforms, default is 1.

-   *(float) siggen.&lt;chan&gt;.offset* - Sets DC offset of the output waveforms, default is 0.

For example, if *siggen.0.amplitude* is 1.0 and *siggen.0.offset* is 0.0, the outputs will swing from -1.0 to +1.0. If *siggen.0.amplitude* is 2.5 and *siggen.0.offset* is 10.0, then the outputs will swing from 7.5 to 12.5.

Parameters

None. <span class="footnote">
\[Prior to version 2.1, frequency, amplitude, and offset were parameters. They were changed to pins to allow control by other components.\]
</span>

Functions

-   *(funct) siggen.&lt;chan&gt;.update* - Calculates new values for all five outputs.

lut5
----

The lut5 component is a 5 input logic component based on a look up table.

-   *lut5* does not require a floating point thread.

Installing

    loadrt lut5 [count=N|names=name1[,name2...]]
    addf lut5.N servo-thread | base-thread
    setp lut5.N.function 0xN

Computing Function

To compute the hexadecimal number for the function starting from the top put a 1 or 0 to indicate if that row would be true or false. Next write down every number in the output column starting from the top and writing them from right to left. This will be the binary number. Using a calculator with a program view like the one in Debian enter the binary number and then convert it to hexadecimal and that will be the value for function.

<table>
<caption>Table 1. Look Up Table</caption>
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
<th align="left">Bit 4</th>
<th align="left">Bit 3</th>
<th align="left">Bit 2</th>
<th align="left">Bit 1</th>
<th align="left">Bit 0</th>
<th align="left">Output</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Two Input Example

In the following table we have selected the output state for each line that we wish to be true.

<table>
<caption>Table 2. Look Up Table</caption>
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
<th align="left">Bit 4</th>
<th align="left">Bit 3</th>
<th align="left">Bit 2</th>
<th align="left">Bit 1</th>
<th align="left">Bit 0</th>
<th align="left">Output</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
</tr>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>1</p></td>
</tr>
</tbody>
</table>

Looking at the output column of our example we want the output to be on when Bit 0 or Bit 0 and Bit1 is on and nothing else. The binary number is *b1010* (rotate the output 90 degrees CW). Enter this number into the calculator then change the display to hexadecimal and the number needed for function is *0xa*. The hexadecimal prefix is *0x*.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


