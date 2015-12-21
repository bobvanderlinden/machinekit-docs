<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="machine-setting-up-examples.md">Setting up examples</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

Linear Delta FDM printer setup
==============================

This guide is meant as a start for setting up the machine in a way that will enable you to get a machine running. Other instructions of guides, which focus on calibrating an extruder, temperature PID loops or working with a GUI will have a better place in a separate document. Preferably linked to at the end of this one and in the [setting-up page summary](machine-setting-up-examples.md).

If this type of machine does not act as a FDM machine, but as a mill for example then this document also acts as a setup guide. Most information is equivalent.

Whether you use some scripts or leveling feature later on to make re-calibrating of endstops and/or delta radius easier / less time consuming, I would advice you to at least do a full setup and configuration manually to understand what happens exactly.

Contents
========

1.  [Dictionary](#dictionary)

2.  [The Frame](#frame)

3.  [Connecting](#connecting)

4.  [Configuration Files](#configuration)

5.  [Endstops](#endstops)

6.  [Delta Radius](#delta-radius)

---

<span id="dictionary"></span>Dictionary
---------------------------------------

These instructions will talk about towers, axes, joints, motors and connectors and other parts of the machine. Below you’ll find a short summary of meaningful names.

##### Axes

The axes which are mentioned are the cartesian `X`, `Y` and `Z` axis who together describe a coordinate in cartesian space by (`X`,`Y`,`Z`).

##### Tower

The vertical actuator on which the carriage runs. To eliminate confusion between motors, joints, axes, cartesian space, connectors etc. these 3 towers are called `TOWER A`, `TOWER B` and `TOWER C`.

Go sit before your machine and see that they are located (maybe put a yellow sticky note on the tower afterwards):

-   `TOWER A`: front left, location (-X,-Y)

-   `TOWER B`: front right, location (+X,-Y)

-   `TOWER C`: back middle, location (0,+Y)

##### Carriage

This vertical moving platform connects a set of rods to a tower. It is moved by something like a spindle or a timing belt.

##### Effector

The moving platform which is connected to towers by 3 sets of rods.

##### Parallelogram

The 2 rods from each carriage together with a side of the effector form a parallelogram. The 3 parallelograms together constrain the machine in such a way that the effector cannot rotate around the `x`, `y` and `z` axes (roll `a`, pitch `b` and yaw `c`) degrees of freedom

##### Extruder

The thing that drives the filament into the Hot End

##### Hot End

The shaft that melts the filament. The hot end is mounted below the effector and the nozzle should be the lowest point.

If your machine has a drill bit or functions as a mill then this drill or mill bit also is the lowest point beneath the effector.

<span id="frame"></span>The Frame
---------------------------------

The frame is the base of everything. All further setup depends on the frame and the bed so do this as accurate as possible. If you run into a problem first fix it so you have a good foundation.

#### Towers

The first thing to do is to make sure the 3 columns are parallel and on the correct distances from each other. The more the position corresponds with the theoretical position, the better the dimensions of the printed part will be.

It’s also important to have the 3 columns running parallel to each other. Without this parallelity the part dimensions will not be uniform in height. One way to start would be to set the towers vertical with a level, and measure the distances from one to the other 2.

#### The Bed

The bed should be perpendicular to the 3 parallel towers. Preferably the bed is constrained on 3 points near the tower so adjusting a point directly influences the angle of the bed at the tower you are setting up.

1.  Put the bed on the 3 points near the towers

2.  Get a carpenters angle and put this on the bed. Point 1 side upwards, parallel to the tower, and the other side on the bed pointing towards the middle of the bed.

+ Preferably, get a bright area after the gap between the angle and tower and move the angle towards the tower. If the gap is not fully closed over the full length you should use the adjusting point to get close the gap.

<span id="connecting"></span>Connecting
---------------------------------------

Most important thing to do here is that we make a clear distinction of the naming of axes, joints, towers and connectors.

When you use some sort of configuration (most likely a BeagleBone Black with a 3D printing cape) you probably have motor connections named on the board like `X`, `Y` and `Z`. I like to remember the mnemonic "CAB", like in "Taxi!".

Here `CAB` uses the `XYZ` connectors. You just remember `CAB` and connect:

-   motor of `TOWER C` ⇒ `connector X` (which is normally `joint[0]`)

-   motor of `TOWER A` ⇒ `connector Y` (which is normally `joint[1]`)

-   motor of `TOWER B` ⇒ `connector Z` (which is normally `joint[2]`)

<span id="configuration"></span>Configuration Files
---------------------------------------------------

When you start Machinekit you get to choose which configuration you want to use choose one with "lineardelta" or equivalent. Its not really important what the name is though. You can make a "my machine" configuration if you want.

Scroll around the configuration (`config` directory in machinekit) directory to see if your hardware setup has one. If it’s not there then you must make one.

It’s important to understand that the `INI` file is just loaded once, during startup. So any changes you make in your `INI` file are only loaded during the start.

A configuration consists of an `INI` file and a `HAL` file. More info can be found in the legacy LinuxCNC documentation. For now below are the parts which are important for the machine setup.

### INI-file

When making a configuration file for this type of machine you should look at the following sections. Below examples with comments about their use.

    [EMC]
    # below text will show in the GUI for example
    MACHINE = type the name of your machine here

    [MACHINE]
    DELTA_R = 158.55 #here the delta radius is given
    CF_ROD = 326.37  #here the rod length given

    [HAL]
    # this file will hold the settings of which
    # software pin is wired to which hardware pin
    HALFILE = the-location-of-the-hal-file.hal

    [AXIS_n] #where n is 0, 1, 2 and 3
    TYPE =              LINEAR

    # for our linear delta type machine we need
    # to have the value of MAX_VELOCITY way below
    # the value of STEPGEN_MAX_VEL.
    #
    # why? do you ask...
    #
    # MAX_VELOCITY of [AXIS_0] is the velocity of
    # the cartesian x-axis, where STEPGEN_MAX_VEL
    # is the max velocity of of JOINT[0].
    #
    # because of the kinematics of our machine
    # the joint must be able to move and accelerate
    # a lot quicker than the cartesian axis.
    # Especially if the effector is moving at the
    # edge of the working area (radius)
    #
    # this is confusing, I know
    MAX_VELOCITY =      250.0
    STEPGEN_MAX_VEL =   390

    # the same goes for the acceleration settings.
    # here again the difference between cartesian
    # and joint setting
    MAX_ACCELERATION =  1100.0
    STEPGEN_MAX_ACC =   5000

    # this is the scale of the motor.
    # simply make positive/negative to
    # change the direction
    SCALE =  -128

    # when homing the value of HOME_OFFSET is used
    # for setting the JOINT[n] position.
    # different
    HOME =              710.00
    HOME_OFFSET =       711.10
    # speed (up) when homing
    HOME_SEARCH_VEL =   30.0
    # speed down (slow) when homing
    HOME_LATCH_VEL =    -1.0

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
<td align="left">more info about <code>HOMING</code> can be found in <a href="../../src/config/ini_homing.md" class="uri">../../src/config/ini_homing.md</a></td>
</tr>
</tbody>
</table>

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
<td align="left">since this setup document is about calibrating the mechanics, the <code>INI</code> values that are needed for the extruder are better of in a separate setup document. Could be linked to from here if/whenever it exists.</td>
</tr>
</tbody>
</table>

### HAL-file

The `HAL` is the Hardware Abstraction Layer. If you are not familiar with this then please visit the [HAL index](../../machinekit-documentation/index-HAL.md) with especially the [HAL intro](../../src/hal/intro.md) before continuing. The `HALFILE` in the above mentioned `INI` settings contains all the "wiring logic" of the machine.

We’ll not dig deeply here, but some lines are worth mentioning

#### net signal source target

Reading a `HAL` file can be very energy draining. Just remember the following:

A `signal` is the "wire" linking `pins`, where the `source pin` is mentioned after the `signal` and the `target pin` is mentioned after the `source pin`

Ych!…. just repeat 10 times ***NOW*** and you’ll know it the 11th time when you pull your hair and grind your teeth reading thru a `HAL` FILE:

⇒ ⇒ <span class="red">**NET SIGNAL SOURCE TARGET**</span> ⇐ ⇐

for example:

    net the-signal-from-pinA-to-pinB pinA pinB

which is the same as:

    net the-signal-from-pinA-to-pinB pinA
    net the-signal-from-pinA-to-pinB pinB

<span id="endstops"></span>Endstops
-----------------------------------

The goal of this section is getting the exact height of the endswitch of a joint with respect to the bed. This is important since the nozzle will have to travel in a straight line across the bed and printed layers.

You have to understand that `HOMING` sets the height of the tower joint (`TOWER C` which is `JOINT[0]`). So when the homing routine is done then your machine knows the height of the carriages, and in turn calculates the cartesian (`X`,`Y`,`Z`) position.

you can view the `JOINTS` position in the `joint mode` and the cartesian values in the `world mode`. you can switch between them in the GUI.

### Use of debug pins in kinematics

1.  home the machine

2.  go to a position near a tower within the `MDI mode`. This is where you manually give a command to move the effector go to the tower coordinates. Like `G1 X0 Y150 Z50 F2000`. This should result in the effector being at the position of the tower at the back.

+

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
<td align="left"><code>Y150</code> in the example should be your delta radius value. Furthermore the rods of the parallelogram should be as-good-as vertical. Big deviation means you have to look into the reason &quot;why&quot;. A small deviation is no problem since the cosine part (<code>Z</code>) of the (<code>X</code>,<code>Y</code>) error is very small and has very little influence of the height (position of the tower)</td>
</tr>
</tbody>
</table>

1.  go down with the nozzle until you experience friction between nozzle and bed with the "dragging-paper-method" or to a specified height of a dial caliper.

2.  **write down the cartesian Z-position**.

3.  go to another tower and move down until you have the nozzle at the same height as `TOWER C` + 5 mm.

4.  go down in little steps, and change from the terminal the joint debug pins. These pins move the effector lower/higher without effecting the joint\[n\] position or on:

+

+ you should be able to notice (paper-friction-test, or see the dial caliper change) the effector move. Do this in little steps to prevent following errors until you have the same dragging-paper-friction situation at the same cartesian Z-position as noted with `TOWER C`. . return to step 5 for the remaining tower.

### Setting `HOME_OFFSET` values

Now that we know the errors of our endswitches, we can either correct them in our `INI` file (which will require a restart for them to be loaded)

or…

1.  we can without having to restart value you found in the previous steps:

2.  After you have done this for all the offsets you want to change you need to home the machine again, which will result in the joints getting an other position the moment the homing sequence is done.

3.  Set all the debug pins `lineardeltakins.J{n}off` to zero.

4.  Re-home for good measure.

5.  **CHANGE THESE VALUES IN YOUR INI FILE!**

<span id="delta-radius"></span>Delta Radius
-------------------------------------------

If the `delta radius` is not correct, the movement of the effector will be convex or concave instead of straight.

We use the same way we calibrated the endstops in the previous section for finetuning the `delta radius`.

### Setting `Delta Radius` value from terminal

1.  go to `TOWER C` and find the Z-value like in steps 3 & 4 of [section 5.1](#endstops)

2.  like in steps 5 and following move the effector down, but this time at (0,0) so the effector will be above the center.

3.  like in step 6, only this time from the terminal:

4.  Decrease this value if the effector is too low (too little play between hot end and bed). Decreasing raises the effector.

5.  Increase this value if there is too much space between nozzle and bed. increasing means lowering the effector.

6.  This value now will be changed, and is not effected by homing.

7.  **change the value in your INI file!**

---

Done!
=====

Your movement should run straight above the bed. It is highly recommended to use a dial caliper because it just makes life easy. If you don’t have access to one you can get the same results with the "scraping-paper-method". Don’t worry about that.

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
<td align="left"><div class="paragraph">
<p>todo:</p>
</div>
<div class="ulist">
<ul>
<li><p>document and link to calibrating extrusion do not exist</p></li>
<li><p>smart scripts and possibly other tools</p></li>
<li><p>GUI and other setup instructions</p></li>
</ul>
</div></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="machine-setting-up-examples.md">Setting up examples</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

