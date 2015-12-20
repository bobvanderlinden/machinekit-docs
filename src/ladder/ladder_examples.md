Classicladder Examples
======================

<span id="cha:classicladder-examples"></span>

Wrapping Counter
----------------

To have a counter that *wraps around* you have to use the preset pin and the reset pin. When you create the counter set the preset at the number you wish to reach before wrapping around to 0. The logic is if the counter value is over the preset then reset the counter and if the underflow is on then set the counter value to the preset value. As you can see in the example when the counter value is greater than the counter preset the counter reset is triggered and the value is now 0. The underflow output %Q2 will set the counter value at the preset when counting backwards.

![](images/wrapping-counter.png)

Figure 1. Wrapping Counter<span id="fig:Wrapping-Counter"></span>

Reject Extra Pulses
-------------------

This example shows you how to reject extra pulses from an input. Suppose the input pulse %I0 has an annoying habit of giving an extra pulse that spoils our logic. The TOF (Timer Off Delay) prevents the extra pulse from reaching our cleaned up output %Q0. How this works is when the timer gets an input the output of the timer is on for the duration of the time setting. Using a normally closed contact %TM0.Q the output of the timer blocks any further inputs from reaching our output until it times out.

![](images/extra-pulse-reject.png)

Figure 2. Reject Extra Pulse<span id="fig:Reject-Extra-Pulse"></span>

External E-Stop
---------------

The External E-Stop example is in the /config/classicladder/cl-estop folder. It uses a pyVCP panel to simulate the external components.

To interface an external E-Stop to Machinekit and have the external E-Stop work together with the internal E-Stop requires a couple of connections through Classic Ladder.

First we have to open the E-Stop loop in the main HAL file by commenting out by adding the pound sign as shown or removing the following lines.

    # net estop-out <= iocontrol.0.user-enable-out
    # net estop-out => iocontrol.0.emc-enable-in

Next we add Classic Ladder to our custom.hal file by adding these two lines:

    loadrt classicladder_rt
    addf classicladder.0.refresh servo-thread

Next we run our config and build the ladder as shown here.

![](images/EStop_Section_Display.png)

Figure 3. E-Stop Section Display<span id="cap:E-Stop-Section-Display"></span>

After building the ladder select Save As and save the ladder as estop.clp

Now add the following line to your custom.hal file.

    # Load the ladder
    loadusr classicladder --nogui estop.clp

I/O assignments

-   %I0 = Input from the pyVCP panel simulated E-Stop (the checkbox)

-   %I1 = Input from Machinekit’s E-Stop

-   %I2 = Input from Machinekit’s E-Stop Reset Pulse

-   %I3 = Input from the pyVCP panel reset button

-   %Q0 = Ouput to Machinekit to enable

-   %Q1 = Output to external driver board enable pin (use a N/C output if your board had a disable pin)

Next we add the following lines to the custom\_postgui.hal file

    # E-Stop example using pyVCP buttons to simulate external components

    # The pyVCP checkbutton simulates a normally closed external E-Stop
    net ext-estop classicladder.0.in-00 <= pyvcp.py-estop

    # Request E-Stop Enable from Machinekit
    net estop-all-ok iocontrol.0.emc-enable-in <= classicladder.0.out-00

    # Request E-Stop Enable from pyVCP or external source
    net ext-estop-reset classicladder.0.in-03 <= pyvcp.py-reset

    # This line resets the E-Stop from Machinekit
     net emc-reset-estop iocontrol.0.user-request-enable =>
    classicladder.0.in-02

    # This line enables Machinekit to unlatch the E-Stop in Classic Ladder
    net emc-estop iocontrol.0.user-enable-out => classicladder.0.in-01

    # This line turns on the green indicator when out of E-Stop
    net estop-all-ok => pyvcp.py-es-status

Next we add the following lines to the panel.xml file. Note you have to open it with the text editor not the default html viewer.

    <pyvcp>
    <vbox>
    <label><text>"E-Stop Demo"</text></label>
    <led>
    <halpin>"py-es-status"</halpin>
    <size>50</size>
    <on_color>"green"</on_color>
    <off_color>"red"</off_color>
    </led>
    <checkbutton>
    <halpin>"py-estop"</halpin>
    <text>"E-Stop"</text>
    </checkbutton>
    </vbox>
    <button>
    <halpin>"py-reset"</halpin>
    <text>"Reset"</text>
    </button>
    </pyvcp>

Now start up your config and it should look like this.

![](images/axis_cl-estop.png)

Figure 4. AXIS E-Stop<span id="cap:AXIS-E-Stop"></span>

Note that in this example like in real life you must clear the remote E-Stop (simulated by the checkbox) before the AXIS E-Stop or the external Reset will put you in OFF mode. If the E-Stop in the AXIS screen was pressed, you must press it again to clear it. You cannot reset from the external after you do an E-Stop in AXIS.

Timer/Operate Example
---------------------

In this example we are using the Operate block to assign a value to the timer preset based on if an input is on or off.

![](images/Tmr_Section_Display.png)

Figure 5. Timer/Operate Example<span id="cap:Timer/Operate-Example"></span>

In this case %I0 is true so the timer preset value is 10. If %I0 was false the timer preset would be 5.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


