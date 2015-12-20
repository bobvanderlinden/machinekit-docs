Python Interface
================

<span id="cha:python-interface"></span>

This is work in progress by Michael Haberler. Comments, fixes, and addenda are welcome, especially for PositionLogger (A bit of intent, purpose and usage would help here!)

The machinekit Python module
----------------------------

User interfaces control Machinekit activity by sending NML messages to the Machinekit task controller, and monitor results by observing the Machinekit status structure, as well as the error reporting channel.

Programmatic access to NML is through a C++ API; however, the most important parts of the NML interface to Machinekit are also available to Python programs through the `machinekit` module.

Beyond the NML interface to the command, status and error channels, the `machinekit` module also contains:

-   support for reading values from ini files

-   support for position logging (???)

Usage Patterns for the Machinekit NML interface
-----------------------------------------------

The general pattern for `machinekit` usage is roughly like this:

-   import the `machinekit` module

-   establish connections to the command, status and error NML channels as needed

-   poll the status channel, either periodically or as needed

-   before sending a command, determine from status whether it is in fact OK to do so (for instance, there is no point in sending a *Run* command if task is in the ESTOP state, or the interpreter is not idle)

-   send the command by using one of the `machinekit` command channel methods

To retrieve messages from the error channel, poll the error channel periodically, and process any messages retrieved.

-   poll the status channel, either periodically or as needed

-   print any error message FIXME: explore the exception code

`machinekit` also defines the `error` Python exception type to support error reporting.

Reading Machinekit status
-------------------------

Here is a Python fragment to explore the contents of the `machinekit.stat` object which contains some 8ß0+ values (run while machinekit is running for typical values):

    import sys
    import machinekit
    try:
        s = machinekit.stat() # create a connection to the status channel
        s.poll() # get current values
    except machinekit.error, detail:
        print "error", detail
        sys.exit(1)
    for x in dir(s):
        if not x.startswith('_'):
            print x, getattr(s,x)

Machinekit uses the default compiled-in path to the NML configuration file unless overridden, see [Reading ini file values](#sec:Python-reading-ini-values) for an example.

### machinekit.stat attributes

 **acceleration**   
*(returns float)* - default acceleration, reflects the ini entry \[TRAJ\] DEFAULT\_ACCELERATION.

 **active\_queue**   
*(returns int)* - number of motions blending.

 **actual\_position**   
*(returns tuple of floats)* - current trajectory position, (x y z a b c u v w) in machine units.

 **adaptive\_feed\_enabled**   
*(returns True/False)* - status of adaptive feedrate override (0/1).

 **ain**   
*(returns tuple of floats)* - current value of the analog input pins.

 **angular\_units**   
*(returns string)* - reflects \[TRAJ\] ANGULAR\_UNITS ini value.

 **aout**   
*(returns tuple of floats)* - current value of the analog output pins.

 **axes**   
*(returns string)* - reflects \[TRAJ\] AXES ini value.

 **axis**   
*(returns tuple of dicts)* - reflecting current axis values. See [The axis dictionary](#sec:The-Axis-dictionary).

 **axis\_mask**   
*(returns integer)* - mask of axis available as defined by \[TRAJ\] COORDINATES in the ini file. Returns the sum of the axes X=1, Y=2, Z=4, A=8, B=16, C=32, U=64, V=128, W=256.

 **block\_delete**   
*(returns integer)* - block delete currently on/off.

 **command**   
*(returns string)* - currently executing command.

 **current\_line**   
*(returns integer)* - currently executing line, int.

 **current\_vel**   
*(returns float)* - current velocity in Cartesian space.

 **cycle\_time**   
*(returns string)* - reflects \[TRAJ\] CYCLE\_TIME ini value (FIXME is this right?).

 **debug**   
*(returns integer)* - debug flag.

 **delay\_left**   
*(returns float)* - remaining time on dwell (G4) command, seconds.

 **din**   
*(returns tuple of integers)* - current value of the digital input pins.

 **distance\_to\_go**   
*(returns float)* - remaining distance of current move, as reported by trajectory planner, in Cartesian space.

 **dout**   
*(returns tuple of integers)* - current value of the digital output pins.

 **dtg**   
*(returns tuple of 9 floats)* - remaining distance of current move, as reported by trajectory planner.

 **echo\_serial\_number**   
*(returns integer)* - The serial number of the last completed command sent by a UI to task. All commands carry a serial number. Once the command has been executed, its serial number is reflected in `echo_serial_number`.

 **enabled**   
*(returns integer)* - trajectory planner enabled flag.

 **estop**   
*(returns integer)* - estop flag.

 **exec\_state**   
*(returns integer)* - task execution state. One of EXEC\_ERROR, EXEC\_DONE, EXEC\_WAITING\_FOR\_MOTION, EXEC\_WAITING\_FOR\_MOTION\_QUEUE, EXEC\_WAITING\_FOR\_PAUSE,EXEC\_WAITING\_FOR\_MOTION\_AND\_IO, EXEC\_WAITING\_FOR\_DELAY, EXEC\_WAITING\_FOR\_SYSTEM\_CMD.

 **feed\_hold\_enabled**   
*(returns integer)* - enable flag for feed hold.

 **feed\_override\_enabled**   
*(returns integer)* - enable flag for feed override.

 **feedrate**   
*(returns float)* - current feedrate override.

 **file**   
*(returns string)* - currently executing gcode file.

 **flood**   
*(returns integer)* - flood enabled.

 **g5x\_index**   
*(returns string)* - currently active coordinate system, G54=0, G55=1 etc.

 **g5x\_offset**   
*(returns tuple of floats)* - offset of the currently active coordinate system.

 **g92\_offset**   
*(returns tuple of floats)* - pose of the current g92 offset.

 **gcodes**   
*(returns tuple of 16 integers)* - currently active G-codes.

 **homed**   
*(returns integer)* - flag. 1 if homed.

 **id**   
*(returns integer)* - currently executing motion id.

 **inpos**   
*(returns integer)* - machine-in-position flag.

 **input\_timeout**   
*(returns integer)* - flag for M66 timer in progress.

 **interp\_state**   
*(returns integer)* - current state of RS274NGC interpreter. One of INTERP\_IDLE, INTERP\_READING, INTERP\_PAUSED, INTERP\_WAITING.

 **interpreter\_errcode**   
*(returns integer)* - current RS274NGC interpreter return code. One of INTERP\_OK, INTERP\_EXIT, INTERP\_EXECUTE\_FINISH, INTERP\_ENDFILE, INTERP\_FILE\_NOT\_OPEN, INTERP\_ERROR. see src/emc/nml\_intf/interp\_return.hh

 **joint\_actual\_position**   
*(returns tuple of floats)* - actual joint positions.

 **joint\_position**   
*(returns tuple of floats)* - Desired joint positions.

 **kinematics\_type**   
*(returns integer)* - identity=1, serial=2, parallel=3, custom=4 .

 **limit**   
*(returns tuple of integers)* - axis limit masks. minHardLimit=1, maxHardLimit=2, minSoftLimit=4, maxSoftLimit=8.

 **linear\_units**   
*(returns string)* - reflects \[TRAJ\]LINEAR\_UNITS ini value.

 **lube**   
*(returns integer)* - *lube on* flag.

 **lube\_level**   
*(returns integer)* - reflects *iocontrol.0.lube\_level*.

 **max\_acceleration**   
*(returns float)* - maximum acceleration. reflects \[TRAJ\] MAX\_ACCELERATION.

 **max\_velocity**   
*(returns float)* - maximum velocity. reflects \[TRAJ\] MAX\_VELOCITY.

 **mcodes**   
*(returns tuple of 10 integers)* - currently active M-codes.

 **mist**   
*(returns integer)* - *mist on* flag.

 **motion\_line**   
*(returns integer)* - source line number motion is currently executing. Relation to `id` unclear.

 **motion\_mode**   
*(returns integer)* - motion mode.

 **motion\_type**   
*(returns integer)* - trajectory planner mode. One of TRAJ\_MODE\_COORD, TRAJ\_MODE\_FREE, TRAJ\_MODE\_TELEOP.

 **optional\_stop**   
*(returns integer)* - option stop flag.

 **paused**   
*(returns integer)* - `motion paused` flag.

 **pocket\_prepped**   
*(returns integer)* - A Tx command completed, and this pocket is prepared. -1 if no prepared pocket.

 **poll()**   
- method to update current status attributes.

 **position**   
*(returns tuple of floats)* - trajectory position.

 **probe\_tripped**   
*(returns integer)* - flag, true if probe has tripped (latch)

 **probe\_val**   
*(returns integer)* - reflects value of the `motion.probe-input` pin.

 **probed\_position**   
*(returns tuple of floats)* - position where probe tripped.

 **probing**   
*(returns integer)* - flag, 1 if a probe operation is in progress.

 **program\_units**   
*(returns integer)* - one of CANON\_UNITS\_INCHES=1, CANON\_UNITS\_MM=2, CANON\_UNITS\_CM=3

 **queue**   
*(returns integer)* - current size of the trajectory planner queue.

 **queue\_full**   
*(returns integer)* - the trajectory planner queue is full.

 **read\_line**   
*(returns integer)* - line the RS274NGC interpreter is currently reading.

 **rotation\_xy**   
*(returns float)* - current XY rotation angle around Z axis.

 **settings**   
*(returns tuple of 3 floats)* - current interpreter settings. settings\[0\] = sequence number, settings\[1\] = feed rate, settings\[2\] = speed.

 **spindle\_brake**   
*(returns integer)* - value of the spindle brake flag.

 **spindle\_direction**   
*(returns integer)* - rotational direction of the spindle. forward=1, reverse=-1.

 **spindle\_enabled**   
*(returns integer)* - value of the spindle enabled flag.

 **spindle\_increasing**   
*(returns integer)* - unclear.

 **spindle\_override\_enabled**   
*(returns integer)* - value of the spindle override enabled flag.

 **spindle\_speed**   
*(returns float)* - spindle speed value, rpm, &gt; 0: clockwise, &lt; 0: counterclockwise.

 **spindlerate**   
*(returns float)* - spindle speed override scale.

 **state**   
*(returns integer)* - current command execution status. One of RCS\_DONE, RCS\_EXEC, RCS\_ERROR.

 **task\_mode**   
*(returns integer)* - current task mode. one of MODE\_MDI, MODE\_AUTO, MODE\_MANUAL.

 **task\_paused**   
*(returns integer)* - task paused flag.

 **task\_state**   
*(returns integer)* - current task state. one of STATE\_ESTOP, STATE\_ESTOP\_RESET, STATE\_ON, STATE\_OFF.

 **tool\_in\_spindle**   
*(returns integer)* - current tool number.

 **tool\_offset**   
*(returns tuple of floats)* - offset values of the current tool.

 **tool\_table**   
*(returns tuple of tool\_results)* - list of tool entries. Each entry is a sequence of the following fields: id, xoffset, yoffset, zoffset, aoffset, boffset, coffset, uoffset, voffset, woffset, diameter, frontangle, backangle, orientation. The id and orientation are integers and the rest are floats.

 **velocity**   
*(returns float)* - default velocity. reflects \[TRAJ\] DEFAULT\_VELOCITY.

### The `axis` dictionary <span id="sec:The-Axis-dictionary"></span>

The axis configuration and status values are available through a list of per-axis dictionaries. Here’s an example how to access an attribute of a particular axis:

    import machinekit
    s = machinekit.stat()
    s.poll()
    print 'Axis 1 homed: ', s.axis[1]['homed']

For each axis, the following dictionary keys are available:

 **axisType**   
*(returns integer)* - type of axis configuration parameter, reflects \[AXIS\_x\]TYPE. LINEAR=1, ANGULAR=2. See [Axis ini configuration](#sub:AXIS-section) for details.

 **backlash**   
*(returns float)* - Backlash in machine units. configuration parameter, reflects \[AXIS\_x\]BACKLASH.

 **enabled**   
*(returns integer)* - non-zero means enabled.

 **fault**   
*(returns integer)* - non-zero means axis amp fault.

 **ferror\_current**   
*(returns float)* - current following error.

 **ferror\_highmark**   
*(returns float)* - magnitude of max following error.

 **homed**   
*(returns integer)* - non-zero means has been homed.

 **homing**   
*(returns integer)* - non-zero means homing in progress.

 **inpos**   
*(returns integer)* - non-zero means in position.

 **input**   
*(returns float)* - current input position.

 **max\_ferror**   
*(returns float)* - maximum following error. configuration parameter, reflects \[AXIS\_x\]FERROR.

 **max\_hard\_limit**   
*(returns integer)* - non-zero means max hard limit exceeded.

 **max\_position\_limit**   
*(returns float)* - maximum limit (soft limit) for axis motion, in machine units.configuration parameter, reflects \[AXIS\_x\]MAX\_LIMIT.

 **max\_soft\_limit**   
non-zero means `max_position_limit` was exceeded, int

 **min\_ferror**   
*(returns float)* - configuration parameter, reflects \[AXIS\_x\]MIN\_FERROR.

 **min\_hard\_limit**   
*(returns integer)* - non-zero means min hard limit exceeded.

 **min\_position\_limit**   
*(returns float)* - minimum limit (soft limit) for axis motion, in machine units.configuration parameter, reflects \[AXIS\_x\]MIN\_LIMIT.

 **min\_soft\_limit**   
*(returns integer)* - non-zero means `min_position_limit` was exceeded.

 **output**   
*(returns float)* - commanded output position.

 **override\_limits**   
*(returns integer)* - non-zero means limits are overridden.

 **units**   
*(returns float)* - units per mm, deg for linear, angular

 **velocity**   
*(returns float)* - current velocity.

Preparing to send commands
--------------------------

Some commands can always be sent, regardless of mode and state; for instance, the `machinekit.command.abort()` method can always be called.

Other commands may be sent only in appropriate state, and those tests can be a bit tricky. For instance, an MDI command can be sent only if:

-   ESTOP has not been triggered, and

-   the machine is turned on and

-   the axes are homed and

-   the interpreter is not running and

-   the mode is set to `MDI mode`

so an appropriate test before sending an MDI command through `machinekit.command.mdi()` could be:

    import machinekit
    s = machinekit.stat()
    c = machinekit.command()

    def ok_for_mdi():
        s.poll()
        return not s.estop and s.enabled and s.homed and (s.interp_state == machinekit.INTERP_IDLE)

    if ok_for_mdi():
       c.mode(machinekit.MODE_MDI)
       c.wait_complete() # wait until mode switch executed
       c.mdi("G0 X10 Y20 Z30")

Sending commands through `machinekit.command`
---------------------------------------------

Before sending a command, initialize a command channel like so:

    import machinekit
    c = machinekit.command()

    # Usage examples for some of the commands listed below:
    c.abort()

    c.auto(machinekit.AUTO_RUN, program_start_line)
    c.auto(machinekit.AUTO_STEP)
    c.auto(machinekit.AUTO_PAUSE)
    c.auto(machinekit.AUTO_RESUME)

    c.brake(machinekit.BRAKE_ENGAGE)
    c.brake(machinekit.BRAKE_RELEASE)

    c.flood(machinekit.FLOOD_ON)
    c.flood(machinekit.FLOOD_OFF)

    c.home(2)

    c.jog(machinekit.JOG_STOP, axis)
    c.jog(machinekit.JOG_CONTINUOUS, axis, speed)
    c.jog(machinekit.JOG_INCREMENT, axis, speed, increment)

    c.load_tool_table()

    c.maxvel(200.0)

    c.mdi("G0 X10 Y20 Z30")

    c.mist(machinekit.MIST_ON)
    c.mist(machinekit.MIST_OFF)

    c.mode(machinekit.MODE_MDI)
    c.mode(machinekit.MODE_AUTO)
    c.mode(machinekit.MODE_MANUAL)

    c.override_limits()

    c.program_open("foo.ngc")
    c.reset_interpreter()

    c.set_home_parameters(jointnum, home_pos, home_offset, home_final_velocity, home_search_velocity, home_final_velocity, use_index, ignore_limits, is_shared, home_sequence, volatile_home, locking_indexer) )


    c.tool_offset(toolno, z_offset,  x_offset, diameter, frontangle, backangle, orientation)

### `machinekit.command` attributes

 `serial`   
the current command serial number

### `machinekit.command` methods:

 `abort()`   
send EMC\_TASK\_ABORT message.

 `auto(int[, int])`   
run, step, pause or resume a program.

 `brake(int)`   
engage or release spindle brake.

 `debug(int)`   
set debug level via EMC\_SET\_DEBUG message.

 `feedrate(float)`   
set the feedrate.

 `flood(int)`   
turn on/off flooding.

 `home(int)`   
home a given axis.

 `jog(int, int, [, int[,int]])`   
Syntax:
 jog(command, axis\[, velocity\[, distance\]\])
 jog(machinekit.JOG\_STOP, axis)
 jog(machinekit.JOG\_CONTINUOUS, axis, velocity)
 jog(machinekit.JOG\_INCREMENT, axis, velocity, distance)
 Constants:
 JOG\_STOP (0)
 JOG\_CONTINUOUS (1)
 JOG\_INCREMENT (2)

 `load_tool_table()`   
reload the tool table.

 `maxvel(float)`   
set maximum velocity

 `mdi(string)`   
send an MDI command. Maximum 255 chars.

 `mist(int)`   
turn on/off mist.
 Syntax:
 mist(command)
 mist(machinekit.MIST\_ON) \[(1)\]
 mist(machinekit.MIST\_OFF) \[(0)\]
 Constants:
 MIST\_ON (1)
 MIST\_OFF (0)

 `mode(int)`   
set mode (MODE\_MDI, MODE\_MANUAL, MODE\_AUTO).

 `override_limits()`   
set the override axis limits flag.

 `program_open(string)`   
open an NGC file.

 `reset_interpreter()`   
reset the RS274NGC interpreter

 `set_adaptive_feed(int)`   
set adaptive feed flag

 `set_analog_output(int, float)`   
set analog output pin to value

 `set_block_delete(int)`   
set block delete flag

 `set_digital_output(int, int)`   
set digital output pin to value

 `set_feed_hold(int)`   
set feed hold on/off

 `set_feed_override(int)`   
set feed override on/off

 `set_max_limit(int, float)`   
set max position limit for a given axis

 *set\_home\_parameters(int, float, float, float, float, float, int, int, int, int, int, int)*   
set home parameters for a given axis. All parameters must be passed to the function to succeed.

 `set_min_limit()`   
set min position limit for a given axis

 `set_optional_stop(int)`   
set optional stop on/off

 `set_spindle_override(int)`   
set spindle override flag

 `spindle(int)`   
set spindle direction. Argument one of SPINDLE\_FORWARD, SPINDLE\_REVERSE, SPINDLE\_OFF, SPINDLE\_INCREASE, SPINDLE\_DECREASE, or SPINDLE\_CONSTANT.

 `spindleoverride(float)`   
set spindle override factor

 `state(int)`   
set the machine state. Machine state should be STATE\_ESTOP, STATE\_ESTOP\_RESET, STATE\_ON, or STATE\_OFF

 `teleop_enable(int)`   
enable/disable teleop mode.

 `teleop_vector(float, float, float [,float, float, float])`   
set teleop destination vector

 `tool_offset(int, float, float, float, float, float, int)`   
set the tool offset. See usage example above.

 `traj_mode(int)`   
set trajectory mode. Mode is one of MODE\_FREE, MODE\_COORD, or MODE\_TELEOP.

 `unhome(int)`   
unhome a given axis.

 `wait_complete([float])`   
wait for completion of the last command sent. If timeout in seconds not specified, default is 1 second.

Reading the error channel
-------------------------

To handle error messages, connect to the error channel and periodically poll() it.

Note that the NML channel for error messages has a queue (other than the command and status channels), which means that the first consumer of an error message deletes that message from the queue; whether your another error message consumer (e.g. Axis) will *see* the message is dependent on timing. It is recommended to have just one error channel reader task in a setup.

    import machinekit
    e = machinekit.error_channel()

    error = e.poll()

    if error:
        kind, text = error
        if kind in (machinekit.NML_ERROR, machinekit.OPERATOR_ERROR):
            typus = "error"
        else:
            typus = "info"
            print typus, text

Reading ini file values <span id="sec:Python-reading-ini-values"></span>
------------------------------------------------------------------------

Here’s an example for reading values from an ini file through the `machinekit.ini` object:

    # run as:
    # python ini-example.py ~/emc2-dev/configs/sim/axis/axis_mm.ini

    import sys
    import machinekit

    inifile = machinekit.ini(sys.argv[1])

    # inifile.find() returns None if the key wasnt found - the
    # following idiom is useful for setting a default value:

    machine_name = inifile.find('EMC', 'MACHINE') or "unknown"
    print "machine name: ", machine_name

    # inifile.findall() returns a list of matches, or an empty list
    # if the key wasnt found:

    extensions = inifile.findall("FILTER", "PROGRAM_EXTENSION")
    print "extensions: ", extensions

    # override default NML file by ini parameter if given
    nmlfile = inifile.find("EMC", "NML_FILE")
    if nmlfile:
        machinekit.nmlfile = os.path.join(os.path.dirname(sys.argv[1]), nmlfile)

The `machinekit.positionlogger` type
------------------------------------

Some usage hints can be gleaned from `src/emc/usr_intf/gremlin/gremlin.py`.

### members

 `npts`   
number of points.

### methods

 `start(float)`   
start the position logger and run every ARG seconds

 `clear()`   
clear the position logger

 `stop()`   
stop the position logger

 `call()`   
Plot the backplot now.

 `last([int])`   
Return the most recent point on the plot or None ,

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


