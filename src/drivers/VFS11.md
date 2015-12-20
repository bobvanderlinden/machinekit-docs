VFS11 VFD Driver
================

<span id="cha:vfs11-vfd-driver"></span>

This is a userspace HAL program to control the S11 series of VFD’s from Toshiba.

vfs11\_vfd supports serial and TCP connections. Serial connections may be RS232 or RS485. RS485 is supported in full- and half-duplex mode. TCP connections may be passive (wait for incoming connection), or active outgoing connections, which may be useful to connect to TCP-based devices or through a terminal server.

Regardless of the connection type, vfs11\_vfd operates as a Modbus master.

This component is loaded using the halcmd "loadusr" command:

    loadusr -Wn spindle-vfd vfs11_vfd -n spindle-vfd

The above command says: loadusr, wait for named to load, component vfs11\_vfd, named spindle-vfd

Command Line Options
--------------------

*vfs11\_vfd* is mostly configured through inifile options. The command line options are:

-   *-n or --name &lt;halname&gt;* : set the HAL component name

-   *-I or --ini &lt;inifilename&gt;* : take configuration from this ini file. Defaults to environment variable INI\_FILE\_NAME.

-   *-S or --section &lt;section name&gt;* : take configuration from this section in the ini file. Defaults to *VFS11*.

-   *-d or --debug* enable debug messages on console output.

-   *-m or --modbus-debug* enable modbus messages on console output

-   *-r or --report-device* report device properties on console at startup

Debugging can be toggled by sending a USR1 signal to the vfs11\_vfd process. Modbus debugging can be toggled by sending a USR2 signal to vfs11\_vfd process (example: `` kill -USR1 `pidof vfs11_vfd` ``).

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

Where &lt;n&gt; is `vfs11_vfd` or the name given during loading with the -n option.

-   *&lt;n&gt;.acceleration-pattern* (bit, in) when true, set acceleration and deceleration times as defined in registers F500 and F501 respecitvely. Used in PID loops to choose shorter ramp times to avoid oscillation.

-   *&lt;n&gt;.alarm-code* (s32, out) non-zero if drive is in alarmed state. Bitmap describing alarm information (see register FC91 description). Use err-reset (see below) to clear the alarm.

-   *&lt;n&gt;.at-speed* (bit, out) when drive is at commanded speed (see speed-tolerance below)

-   *&lt;n&gt;.current-load-percentage* (float, out) reported from the VFD

-   *&lt;n&gt;.dc-brake* (bit, in) engage the DC brake. Also turns off spindle-on.

-   *&lt;n&gt;.enable* (bit, in) enable the VFD. If false, all operating parameters are still read but control is released and panel control is enabled (subject to VFD setup).

-   *&lt;n&gt;.err-reset* (bit, in) reset errors (alarms a.k.a Trip and e-stop status). Resetting the VFD may cause a 2-second delay until it’s rebooted and Modbus is up again.

-   *&lt;n&gt;.estop* (bit, in) put the VFD into emergency-stopped status. No operation possible until cleared with err-reset or powercycling.

-   *&lt;n&gt;.frequency-command* (float, out) current target frequency in HZ as set through speed-command (which is in RPM), from the VFD

-   *&lt;n&gt;.frequency-out* (float, out) current output frequency of the VFD

-   *&lt;n&gt;.inverter-load-percentage* (float, out) current load report from VFD

-   *&lt;n&gt;.is-e-stopped* (bit, out) the VFD is in emergency stop status (blinking "E" on panel). Use err-reset to reboot the VFD and clear the e- stop status.

-   *&lt;n&gt;.is-stopped* (bit, out) true when the VFD reports 0 Hz output

-   *&lt;n&gt;.max-rpm* (float, R) actual RPM limit based on maximum frequency the VFD may generate, and the motors nameplate values. For instance, if nameplate-HZ is 50, and nameplate-RPM\_ is 1410, but the VFD may generate up to 80Hz, then max- rpm would read as 2256 (80\*1410/50). The frequency limit is read from the VFD at startup. To increase the upper frequency limit, the UL and FH parameters must be changed on the panel. See the VF-S11 manual for instructions how to set the maximum frequency.

-   *&lt;n&gt;.modbus-ok* (bit, out) true when the Modbus session is successfully established and the last 10 transactions returned without error.

-   *&lt;n&gt;.motor-RPM* (float, out) estimated current RPM value, from the VFD

-   *&lt;n&gt;.output-current-percentage* (float, out) from the VFD

-   *&lt;n&gt;.output-voltage-percentage* (float, out) from the VFD

-   *&lt;n&gt;.output-voltage* (float, out) from the VFD

-   *&lt;n&gt;.speed-command* (float, in) speed sent to VFD in RPM. It is an error to send a speed faster than the Motor Max RPM as set in the VFD

-   *&lt;n&gt;.spindle-fwd* (bit, in) 1 for FWD and 0 for REV, sent to VFD

-   *&lt;n&gt;.spindle-on* (bit, in) 1 for ON and 0 for OFF sent to VFD, only on when running

-   *&lt;n&gt;.spindle-rev* (bit, in) 1 for ON and 0 for OFF, only on when running

-   *&lt;n&gt;.jog-mode* (bit, in) 1 for ON and 0 for OFF, enables the VF-S11 *jog mode*. Speed control is disabled, and the output frequency is determined by register F262 (preset to 5Hz). This might be useful for spindle orientation. In normal mode, the VFD shuts off if the frequency drops below 12Hz.

-   *&lt;n&gt;.status* (s32, out) Drive Status of the VFD (see the TOSVERT VF-S11 Communications Function Instruction Manual, register FD01). A bitmap.

-   *&lt;n&gt;.trip-code* (s32, out) trip code if VF-S11 is in tripped state.

-   *&lt;n&gt;.error-count* (s32, out) number of Modbus transactions which returned an error

-   *&lt;n&gt;.max-speed* (bit, in) ignore the loop-time paramater and run Modbus at maximum speed, at the expense of higher CPU usage. Suggested use during spindle positioning.

Parameters
----------

Where &lt;n&gt; is `vfs11_vfd` or the name given during loading with the -n option.

-   *&lt;n&gt;.frequency-limit* (float, RO) upper limit read from VFD setup.

-   *&lt;n&gt;.loop-time* (float, RW) how often the Modbus is polled (default interval 0.1 seconds)

-   *&lt;n&gt;.nameplate-HZ* (float, RW) Nameplate Hz of motor (default 50). Used to calculate target frequency (together with nameplate-RPM ) for a target RPM value as given by speed-command.

-   *&lt;n&gt;.nameplate-RPM* (float, RW) Nameplate RPM of motor (default 1410)

-   *&lt;n&gt;.rpm-limit* (float, RW) do-not-exceed soft limit for motor RPM (defaults to nameplate-RPM ).

-   *&lt;n&gt;.tolerance* (float, RW) speed tolerance (default 0.01) for determining wether spindle is at speed (0.01 meaning: output frequency is within 1% of target frequency)

INI file configuration
----------------------

This lists all options understood by vfs11\_vfd. Typical setups for RS232, RS485 and TCP can be found in *src/hal/user\_comps/vfs11\_vfd/\*.ini*.

    [VFS11]
    # serial connection
    TYPE=rtu

    # serial port
    DEVICE=/dev/ttyS0

    # TCP server - wait for incoming connection
    TYPE=tcpserver

    # tcp portnumber for TYPE=tcpserver or tcpclient
    PORT=1502

    # TCP client - active outgoing connection
    TYPE=tcpclient

    # destination to connect to if TYPE=tcpclient
    TCPDEST=192.168.1.1

    #---------- meaningful only if TYPE=rtu -------
    # serial device detail
    # 5 6 7 8
    BITS= 5

    # even odd none
    PARITY=none

    # 110, 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200
    BAUD=19200

    # 1 2
    STOPBITS=1

    #rs232 rs485
    SERIAL_MODE=rs485

    # up down none
    # this feature might not work with a stock Debian
    # libmodbus5/libmodbus-dev package, and generate a warning
    # execution will continue as if RTS_MODE=up were given.
    RTS_MODE=up
    #---------------------

    # modbus timers in seconds
    # inter-character timer
    BYTE_TIMEOUT=0.5
    # packet timer
    RESPONSE_TIMEOUT=0.5

    # target modbus ID
    TARGET=1

    # on I/O failure, try to reconnect after sleeping
    # for RECONNECT_DELAY seconds
    RECONNECT_DELAY=1

    # misc flags
    DEBUG=10
    MODBUS_DEBUG=0
    POLLCYCLES=10

HAL example
-----------

Panel operation
---------------

The vfs11\_vfd driver takes precedence over panel control while it is enabled (see *enable* pin), effectively disabling the panel. Clearing the *enable* pin re-enables the panel. Pins and parameters can still be set, but will not be written to the VFD untile the *enable* pin is set. Operating parameters are still read while bus control is disabled. Exiting the vfs11\_vfd driver in a controlled way will release the VFD from the bus and restore panel control.

See the EMC2 Integrators Manual for more information. For a detailed register description of the Toshiba VFD’s, see the "TOSVERT VF-S11 Communications Function Instruction Manual" (Toshiba document number E6581222) and the "TOSVERT VF-S11 Instruction manual" (Toshiba document number E6581158).

Error Recovery
--------------

`vfs11_vfd` recovers from I/O errors as follows: First, all HAL pins are set to default values, and the driver will sleep for `RECONNECT_DELAY` seconds (default 1 second).

-   Serial (`TYPE=rtu`) mode: on error, close and reopen the serial port.

-   TCP server (`TYPE=tcpserver`) mode: on losing the TCP connection, the driver will go back to listen for incoming connections.

-   TCP client (`TYPE=tcpclient`) mode: on losing the TCP connection, the driver will reconnect to *TCPDEST:PORTNO*.

Configuring the VFS11 VFD for Modbus usage
------------------------------------------

### Connecting the Serial Port

The VF-S11 has an RJ-45 jack for serial communication. Unfortunately, it does not have a standard RS-232 plug and logic levels. The Toshiba-recommended way is: connect the USB001Z USB-to-serial conversion unit to the drive, and plug the USB port into the PC. A cheaper alternative is a homebrew interface ( [hints from Toshiba support](http://git.mah.priv.at/gitweb/vfs11-vfd.git/blob_plain/refs/heads/f12-prod:/VFS11-RJ45_e.pdf), [circuit diagram](http://git.mah.priv.at/gitweb/vfs11-vfd.git/blob_plain/refs/heads/f12-prod:/vfs11-rs232.pdf)).

Note: the 24V output from the VFD has no short-circuit protection.

Serial port factory defaults are 9600/8/1/even, the protocol defaults to the proprietary "Toshiba Inverter Protocol".

### Modbus setup

Several parameters need setting before the VF-S11 will talk to this module. This can either be done manually with the control panel, or over the serial link - Toshiba supplies a Windows application called *PCM001Z* which can read/set parameters in the VFD. Note - PCM001Z only talks the Toshiba inverter protocol. So the last parameter which you’d want to change is the protocol - set from Toshiba Inverter Protocol to Modbus; thereafter, the Windows app is useless.

To increase the upper frequency limit, the UL and FH parameters must be changed on the panel. I increased them from 50 to 80.

See dump-params.mio for a description of non-standard VF-S11 parameters of my setup. This file is for the [modio Modbus interactive utility](http://git.mah.priv.at/gitweb/modio.git).

Programming Note
----------------

The vfs11\_vfd driver uses the [libmodbus version 3](http://www.libmodbus.org) library which is more recent than the version 2 code used in `gs2_vfd`.

The Debian `libmodbus5` and `libmodbus-dev` packages are only available starting from Debian 12 (*Precise Pengolin*). Moreover, these packages lack support for the MODBUS\_RTS\_MODE\_\* flags. Therefore, building vfs11\_vfd using this library might generate a warning if RTS\_MODE= is specified in the ini file.

To use the full functionality on lucid and precise: \* remove the libmodbus packages: `sudo apt-get remove libmodbus5 libmodbus-dev` \* build and install libmodbus version 3 from source as outlined [here](https://github.com/stephane/libmodbus/blob/master/README.rst).

Libmodbus does not build on Debian Hardy, hence vfs11\_vfd is not available on hardy.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


