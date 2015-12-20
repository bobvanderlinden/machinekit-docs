Machinekit User Introduction
============================

<span id="cha:machinekit-user-introduction"></span>

This Manual
-----------

The focus of this manual is on *using* Machinekit. It is intended to be used once Machinekit is installed and configured. For standard installations see the Getting Started Guide for step by step instructions to get you up and going. For detailed information on installation and configuration of Machinekit see the Integrator Manual.

How Machinekit Works<span id="how-Machinekit-works"></span>
-----------------------------------------------------------

The Enhanced Machine Controller (Machinekit) is a lot more than just another CNC mill program. It can control machine tools, robots, or other automated devices. It can control servo motors, stepper motors, relays, and other devices related to machine tools.

There are four main components to the Machinekit software:

-   a motion controller (EMCMOT)

-   a discrete I/O controller (EMCIO)

-   a task executor which coordinates them (EMCTASK)

-   and one of several graphical user interfaces.

In addition there is a layer called HAL (Hardware Abstraction Layer) which allows configuration of Machinekit without the need of recompiling.

![](images/whatstep1.png)

Figure 1. Simple Machinekit Controlled Machine<span id="fig:Typical_machine"></span>

The above figure shows a simple block diagram showing what a typical 3-axis Machinekit system might look like. This diagram shows a stepper motor system. The PC, running Linux as its operating system, is actually controlling the stepper motor drives by sending signals through the printer port. These signals (pulses) make the stepper drives move the stepper motors. The Machinekit system can also run servo motors via servo interface cards or by using an extended parallel port to connect with external control boards. As we examine each of the components that make up an Machinekit system we will remind the reader of this typical machine.

Graphical User Interfaces<span id="sub:Graphical-User-Interfaces"></span>
-------------------------------------------------------------------------

A user interface is the part of the Machinekit that the machine tool operator interacts with. The Machinekit comes with several types of user interfaces:

-   [*Axis*](#cha:axis-gui), the standard GUI interface.

![](images/axis-2.5.png)

Figure 2. Axis GUI<span id="fig:The-Axis-GUI"></span>

-   [*Touchy*](#cha:touchy-gui), a touch screen GUI.

![](images/touchy.png)

Figure 3. Touchy GUI<span id="fig:touchy-gui"></span>

-   [*NGCGUI*](#cha:ngcgui), a subroutine GUI that provides *fill in the blanks* programming of G code. It also supports concatenation of subroutine files to enable you to build a complete G code file without programming.

![](images/ngcgui.png)

Figure 4. NGCGUI GUI imbedded into Axis<span id="fig:ngcgui-gui"></span>

-   [*Mini*](#cha:mini-gui), a Tcl/Tk-based GUI

![](images/mini.png)

Figure 5. The Mini GUI<span id="fig:The-Mini-GUI"></span>

-   [*TkMachinekit*](#cha:tkmachinekit-gui), a Tcl/Tk-based GUI

![](images/tkemc.png)

Figure 6. The TkMachinekit GUI<span id="fig:The-TkMachinekit-GUI"></span>

-   [*Keystick*](#cha:keystick-gui), a character-based screen graphics program suitable for minimal installations (without the X server running).

![](images/keystick.png)

Figure 7. The Keystick GUI<span id="fig:The-Keystick-GUI"></span>

-   *Xemc*, an X-Windows program. A simulator configuration of Xemc can be ran from the configuration picker.

-   *halui* - a HAL based user interface which allows to control Machinekit using knobs and switches. See the Integrators manual for more information on halui.

-   *machinekitrsh* - a telnet based user interface which allows commands to be sent to Machinekit from remote computers.

Virtual Control Panels
----------------------

-   *PyVCP* a python based virtual control panel that can be added to the Axis GUI or be stand alone.

-   *GladeVCP* - a glade based virtual control panel that can be added to the Axis GUI or be stand alone.

Languages
---------

Machinekit uses translation files to translate Machinekit User Interfaces into many languages. You just need to log in with the language you intend to use and when you start up Machinekit it comes up in that language. If your language has not been translated contact a developer on the IRC or the mailing list if you can assist in the translation.

Thinking Like a Machine Operator<span id="sec:Thinking-Operator"></span>
------------------------------------------------------------------------

This book will not even pretend that it can teach you to run a mill or a lathe. Becoming a machinist takes time and hard work. An author once said, "We learn from experience, if at all." Broken tools, gouged vices, and scars are the evidence of lessons taught. Good part finish, close tolerances, and careful work are the evidence of lessons learned. No machine, no computer program, can take the place of human experience.

As you begin to work with the Machinekit program, you will need to place yourself in the position of operator. You need to think of yourself in the role of the one in charge of a machine. It is a machine that is either waiting for your command or executing the command that you have just given it. Throughout these pages we will give information that will help you become a good operator of the Machinekit system. You will need some information right up front here so that the following pages will make sense to you.

Modes of Operation<span id="sub:Modes-of-Operation"></span>
-----------------------------------------------------------

When Machinekit is running, there are three different major modes used for inputting commands. These are *Manual*, *Auto*, and *MDI*. Changing from one mode to another makes a big difference in the way that the Machinekit control behaves. There are specific things that can be done in one mode that cannot be done in another. An operator can home an axis in manual mode but not in auto or MDI modes. An operator can cause the machine to execute a whole file full of G-codes in the auto mode but not in manual or MDI.

In manual mode, each command is entered separately. In human terms a manual command might be *turn on coolant* or *jog X at 25 inches per minute*. These are roughly equivalent to flipping a switch or turning the hand wheel for an axis. These commands are normally handled on one of the graphical interfaces by pressing a button with the mouse or holding down a key on the keyboard. In auto mode, a similar button or key press might be used to load or start the running of a whole program of G-code that is stored in a file. In the MDI mode the operator might type in a block of code and tell the machine to execute it by pressing the &lt;return&gt; or &lt;enter&gt; key on the keyboard.

Some motion control commands are available and will cause the same changes in motion in all modes. These include *abort*, *estop*, and *feed rate override*). Commands like these should be self explanatory.

The AXIS user interface hides some of the distinctions between Auto and the other modes by making Auto-commands available at most times. It also blurs the distinction between Manual and MDI because some Manual commands like Touch Off are actually implemented by sending MDI commands. It does this by automatically changing to the mode that is needed for the action the user has requested.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


