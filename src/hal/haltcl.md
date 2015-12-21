HAL TCL Files
=============

<span id="cha:hal-tcl"></span>

The halcmd language excels in specifiying components and connections but offers no computational capabilities. As a result, ini files are limited in the clarity and brevity that is possible with higher level languages.

The haltcl facility provides a means to use tcl scripting and its features for computation, looping, branching, procedures, etc. in ini files. To use this functionality, you use the tcl language and the extension .tcl for halfiles.

The .tcl extension is understood by the main script (machinekit) that processes ini files. Haltcl files are identified in the the HAL section of ini files (just like .hal files).

Example

    [HAL]
    HALFILE = conventional_file.hal
    HALFILE = tcl_based_file.tcl

With appropriate care, .hal and .tcl files can be intermixed.

Compatibility
-------------

The halcmd language used in .hal files has a simple syntax that is actually a subset of the more powerful general-purpose tcl scripting language.

Haltcl Commands
---------------

Haltcl files use the tcl scripting language augmented with the specific commands of the Machinekit hardware abstraction layer (HAL). The hal-specific commands are.

    addf, alias,
    delf, delsig,
    getp, gets
    ptype,
    stype,
    help,
    linkpp, linkps, linksp, list, loadrt, loadusr, lock,
    net, newsig,
    save, setp, sets, show, source, start, status, stop,
    unalias, unlinkp, unload, unloadrt, unloadusr, unlock,
    waitusr

Two special cases occur for the *gets* and *list* commands due to conflicts with tcl builtin commands. For haltcl, these commands must be preceded with the keyword *hal*.

    halcmd   haltcl
    ------   ------
    gets     hal gets
    list     hal list

Haltcl Inifile variables
------------------------

Inifile variables are accessible by both halcmd and haltcl but with differing syntax.

Machinekit ini files use SECTION and ITEM specifiers to identify configuration items.

    [SECTION_A]
    ITEM1 = value_1
    ITEM2 = value_2
    ...
    [SECTION_B]
    ...

The ini file values are accessible by text substition in .hal files using the form.

    [SECTION]ITEM

The same ini file values are accessible in .tcl files using the form of a tcl global array variable.

    $::SECTION(ITEM)

For example, an ini file item like:

    [AXIS_0]
    MAX_VELOCITY = 4

is expressed as \[AXIS\_0\]MAX\_VELOCITY in .hal files for halcmd and as $::AXIS\_0(MAX\_VELOCITY) in .tcl files for haltcl

Converting .hal files to .tcl files
-----------------------------------

Existing .hal files can be converted to .tcl files by hand editing to adapt to the differences mentioned above. The process can be automated with scripts that convert using these substitutions.

    [SECTION]ITEM ---> $::SECTION(ITEM)
    gets          ---> hal gets
    list          ---> hal list

Haltcl Notes
------------

In haltcl, the value argument for the *sets* and *setp* commands is implicitly treated as an expression in the tcl language.

Example

    # set gain to convert deg/sec to units/min for AXIS_0 radius
    setp scale.0.gain 6.28/360.0*$::AXIS_0(radius)*60.0

Whitespace in the bare expression is not allowed, use quotes for that:

    setp scale.0.gain "6.28 / 360.0 * $::AXIS_0(radius) * 60.0"

In other contexts, such as *loadrt*, you must explicitly use the tcl expr command (\[expr {}\]) for computational expressions.

Example

    loadrt motion base_period=[expr {500000000/$::TRAJ(MAX_PULSE_RATE)}]

Haltcl Examples
---------------

Consider the topic of *stepgen headroom*. Software stepgen runs best with an acceleration constraint that is "a bit higher" than the one used by the motion planner. So, when using halcmd files, we force inifiles to have a manually calculated value.

    [AXIS_0]
    MAXACCEL = 10.0
    STEPGEN_MAXACCEL = 10.5

With haltcl, you can use tcl commands to do the computation and eliminate the STEPGEN\_MAXACCEL inifile item altogether.

    setp stepgen.0.maxaccel $::AXIS_0(MAXACCEL)*1.05

Another haltcl feature is looping and testing. For example, many simulator configurations use "core\_sim.hal" or "core\_sim9.hal" hal files. These differ because of the requirement to connect more or fewer axes. The following haltcl code would work for any combination of axes in a trivkins machine.

    # Create position, velocity and acceleration signals for each axis
    set ddt 0
    foreach axis {X Y Z A B C U V W} axno {0 1 2 3 4 5 6 7 8} {
      # 'list pin' returns an empty list if the pin doesn't exist
      if {[hal list pin axis.$axno.motor-pos-cmd] == {}} {
        continue
      }
      net ${axis}pos axis.$axno.motor-pos-cmd => axis.$axno.motor-pos-fb \
                                              => ddt.$ddt.in
      net ${axis}vel <= ddt.$ddt.out
      incr ddt
      net ${axis}vel => ddt.$ddt.in
      net ${axis}acc <= ddt.$ddt.out
      incr ddt
    }
    puts [show sig *vel]
    puts [show sig *acc]

Haltcl Interactive
------------------

The halrun command recognizes haltcl files. With the -T option, haltcl can be run interaactively as a tcl interpreter. This capability is useful for testing and for standalone hal applications.

Example

    $ halrun -T haltclfile.tcl

Haltcl Distribution Examples (sim)
----------------------------------

The configs/sim/axis/simtcl directory includes an ini file that uses a .tcl file to demonstrate a haltcl configuration in conjunction with the usage of twopass processing. The example shows the use of tcl procedures, looping, the use of comments, and output to the terminal.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

