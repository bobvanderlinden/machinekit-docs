Lathe Configuration
===================

<span id="cha:lathe-configuration"></span>

Default Plane
-------------

When Machinekit’s interpreter was first written, it was designed for mills. That is why the default plane is XY (G17). A normal lathe only uses the XZ plane (G18). To change the default plane place the following line in the .ini file in the RS274NGC section.

    RS274NGC_STARTUP_CODE = G18

The above can be overwritten in a g code program so always set important things in the preamble of the g code file.

INI Settings
------------

The following .ini settings are needed for lathe mode in Axis in addition to or replacing normal settings in the .ini file.

    [DISPLAY]
    DISPLAY = axis
    LATHE = 1
    [TRAJ]
    AXES = 3
    COORDINATES = X Z
    [AXIS_0]
    ...
    [AXIS_2]
    ...

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

