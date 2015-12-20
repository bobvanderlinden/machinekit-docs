HAL Manual V, 2015-12-20
========================

The Machinekit Team

![common/images/emc2-intro.\*]()

This handbook is a work in progress. If you are able to help with writing, editing, or graphic preparation please contact any member of the writing team or join and send an email to <emc-users@lists.sourceforge.net>.

Copyright © 2000-2012 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: This Machinekit Handbook is the product of several authors writing for linuxCNC.org. As you find it to be of value in your work, we invite you to contribute to its revision and growth. A copy of the license is included in the section entitled GNU Free Documentation License. If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330 Boston, MA 02111-1307

LINUX® is the registered trademark of Linus Torvalds in the U.S. and other countries. The registered trademark Linux® is used pursuant to a sublicense from LMI, the exclusive licensee of Linus Torvalds, owner of the mark on a world-wide basis.

Hardware Abstract Layer
=======================

HAL Introduction
----------------

<span id="cha:hal-introduction"></span>

HAL stands for Hardware Abstraction Layer. At the highest level, it is simply a way to allow a number of *building blocks* to be loaded and interconnected to assemble a complex system. The *Hardware* part is because HAL was originally designed to make it easier to configure Machinekit for a wide variety of hardware devices. Many of the building blocks are drivers for hardware devices. However, HAL can do more than just configure hardware drivers.

### HAL is based on traditional system design techniques

HAL is based on the same principles that are used to design hardware circuits and systems, so it is useful to examine those principles first.

Any system (including a CNC machine), consists of interconnected components. For the CNC machine, those components might be the main controller, servo amps or stepper drives, motors, encoders, limit switches, pushbutton pendants, perhaps a VFD for the spindle drive, a PLC to run a toolchanger, etc. The machine builder must select, mount and wire these pieces together to make a complete system.

#### Part Selection

The machine builder does not need to worry about how each individual part works. He treats them as black boxes. During the design stage, he decides which parts he is going to use - steppers or servos, which brand of servo amp, what kind of limit switches and how many, etc. The integrator’s decisions about which specific components to use is based on what that component does and the specifications supplied by the manufacturer of the device. The size of a motor and the load it must drive will affect the choice of amplifier needed to run it. The choice of amplifier may affect the kinds of feedback needed by the amp and the velocity or position signals that must be sent to the amp from a control.

In the HAL world, the integrator must decide what HAL components are needed. Usually every interface card will require a driver. Additional components may be needed for software generation of step pulses, PLC functionality, and a wide variety of other tasks.

#### Interconnection Design

The designer of a hardware system not only selects the parts, he also decides how those parts will be interconnected. Each black box has terminals, perhaps only two for a simple switch, or dozens for a servo drive or PLC. They need to be wired together. The motors connect to the servo amps, the limit switches connect to the controller, and so on. As the machine builder works on the design, he creates a large wiring diagram that shows how all the parts should be interconnected.

When using HAL, components are interconnected by signals. The designer must decide which signals are needed, and what they should connect.

#### Implementation

Once the wiring diagram is complete it is time to build the machine. The pieces need to be acquired and mounted, and then they are interconnected according to the wiring diagram. In a physical system, each interconnection is a piece of wire that needs to be cut and connected to the appropriate terminals.

HAL provides a number of tools to help *build* a HAL system. Some of the tools allow you to *connect* (or disconnect) a single *wire*. Other tools allow you to save a complete list of all the parts, wires, and other information about the system, so that it can be *rebuilt* with a single command.

#### Testing

Very few machines work right the first time. While testing, the builder may use a meter to see whether a limit switch is working or to measure the DC voltage going to a servo motor. He may hook up an oscilloscope to check the tuning of a drive, or to look for electrical noise. He may find a problem that requires the wiring diagram to be changed; perhaps a part needs to be connected differently or replaced with something completely different.

HAL provides the software equivalents of a voltmeter, oscilloscope, signal generator, and other tools needed for testing and tuning a system. The same commands used to build the system can be used to make changes as needed.

#### Summary

This document is aimed at people who already know how to do this kind of hardware system integration, but who do not know how to connect the hardware to Machinekit. See the [Remote Start Example](#sec:Remote-Start-Example) section in the HAL UI Examples documentation.

![images/remote-start.png]()

The traditional hardware design as described above ends at the edge of the main control. Outside the control are a bunch of relatively simple boxes, connected together to do whatever is needed. Inside, the control is a big mystery — one huge black box that we hope works.

HAL extends this traditional hardware design method to the inside of the big black box. It makes device drivers and even some internal parts of the controller into smaller black boxes that can be interconnected and even replaced just like the external hardware. It allows the *system wiring diagram* to show part of the internal controller, rather than just a big black box. And most importantly, it allows the integrator to test and modify the controller using the same methods he would use on the rest of the hardware.

Terms like motors, amps, and encoders are familiar to most machine integrators. When we talk about using extra flexible eight conductor shielded cable to connect an encoder to the servo input board in the computer, the reader immediately understands what it is and is led to the question, *what kinds of connectors will I need to make up each end.* The same sort of thinking is essential for the HAL but the specific train of thought may take a bit to get on track. Using HAL words may seem a bit strange at first, but the concept of working from one connection to the next is the same.

This idea of extending the wiring diagram to the inside of the controller is what HAL is all about. If you are comfortable with the idea of interconnecting hardware black boxes, you will probably have little trouble using HAL to interconnect software black boxes.

### HAL Concepts<span id="sec:HAL-Concepts"></span>

This section is a glossary that defines key HAL terms but it is a bit different than a traditional glossary because these terms are not arranged in alphabetical order. They are arranged by their relationship or flow in the HAL way of things.

 Component   
When we talked about hardware design, we referred to the individual pieces as *parts*, *building blocks*, *black boxes*, etc. The HAL equivalent is a *component* or *HAL component*. (This document uses *HAL component* when there is likely to be confusion with other kinds of components, but normally just uses *component*.) A HAL component is a piece of software with well-defined inputs, outputs, and behavior, that can be installed and interconnected as needed.

 Parameter   
Many hardware components have adjustments that are not connected to any other components but still need to be accessed. For example, servo amps often have trim pots to allow for tuning adjustments, and test points where a meter or scope can be attached to view the tuning results. HAL components also can have such items, which are referred to as *parameters*. There are two types of parameters: Input parameters are equivalent to trim pots - they are values that can be adjusted by the user, and remain fixed once they are set. Output parameters cannot be adjusted by the user - they are equivalent to test points that allow internal signals to be monitored.

 Pin   
Hardware components have terminals which are used to interconnect them. The HAL equivalent is a *pin* or *HAL pin*. (*HAL pin* is used when needed to avoid confusion.) All HAL pins are named, and the pin names are used when interconnecting them. HAL pins are software entities that exist only inside the computer.

 Physical\_Pin   
Many I/O devices have real physical pins or terminals that connect to external hardware, for example the pins of a parallel port connector. To avoid confusion, these are referred to as *physical pins*. These are the things that *stick out* into the real world.

 Signal   
In a physical machine, the terminals of real hardware components are interconnected by wires. The HAL equivalent of a wire is a *signal* or *HAL signal*. HAL signals connect HAL pins together as required by the machine builder. HAL signals can be disconnected and reconnected at will (even while the machine is running).

 Type   
When using real hardware, you would not connect a 24 volt relay output to the +/-10V analog input of a servo amp. HAL pins have the same restrictions, which are based upon their type. Both pins and signals have types, and signals can only be connected to pins of the same type. Currently there are 4 types, as follows:

-   bit - a single TRUE/FALSE or ON/OFF value

-   float - a 64 bit floating point value, with approximately 53 bits of resolution and over 1000 bits of dynamic range.

-   u32 - a 32 bit unsigned integer, legal values are 0 to 4,294,967,295

-   s32 - a 32 bit signed integer, legal values are -2,147,483,647 to +2,147,483,647

 Function   
Real hardware components tend to act immediately on their inputs. For example, if the input voltage to a servo amp changes, the output also changes automatically. However software components cannot act *automatically*. Each component has specific code that must be executed to do whatever that component is supposed to do. In some cases, that code simply runs as part of the component. However in most cases, especially in realtime components, the code must run in a specific sequence and at specific intervals. For example, inputs should be read before calculations are performed on the input data, and outputs should not be written until the calculations are done. In these cases, the code is made available to the system in the form of one or more *functions*. Each function is a block of code that performs a specific action. The system integrator can use *threads* to schedule a series of functions to be executed in a particular order and at specific time intervals.

 Thread   
A *thread* is a list of functions that runs at specific intervals as part of a realtime task. When a thread is first created, it has a specific time interval (period), but no functions. Functions can be added to the thread, and will be executed in order every time the thread runs.

As an example, suppose we have a parport component named hal\_parport. That component defines one or more HAL pins for each physical pin. The pins are described in that component’s doc section: their names, how each pin relates to the physical pin, are they inverted, can you change polarity, etc. But that alone doesn’t get the data from the HAL pins to the physical pins. It takes code to do that, and that is where functions come into the picture. The parport component needs at least two functions: one to read the physical input pins and update the HAL pins, the other to take data from the HAL pins and write it to the physical output pins. Both of these functions are part of the parport driver.

### HAL components<span id="sec:Intro-HAL-components"></span>

Each HAL component is a piece of software with well-defined inputs, outputs, and behavior, that can be installed and interconnected as needed. This section lists some of the available components and a brief description of what each does. Complete details for each component are available later in this document.

#### External Programs with HAL hooks<span id="sub:ExternalPrograms"></span>

 motion   
A realtime module that accepts NML <span class="footnote">
\[Neutral Message Language provides a mechanism for handling multiple types of messages in the same buffer as well as simplifying the interface for encoding and decoding buffers in neutral format and the configuration mechanism.\]
</span> motion commands and interacts with HAL

 iocontrol   
A user space module that accepts NML I/O commands and interacts with HAL

 classicladder   
A PLC using HAL for all I/O

 halui   
A user space program that interacts with HAL and sends NML commands, it is intended to work as a full User Interface using external knobs & switches

#### Internal Components<span id="sub:InternalComponents"></span>

 stepgen   
Software step pulse generator with position loop. See section [\[sec:Stepgen\]](#sec:Stepgen)

 encoder   
Software based encoder counter. See section [\[sec:Encoder\]](#sec:Encoder)

 pid   
Proportional/Integral/Derivative control loops. See section [\[sec:PID\]](#sec:PID)

 siggen   
A sine/cosine/triangle/square wave generator for testing. See section [\[sec:Siggen\]](#sec:Siggen)

 supply   
a simple source for testing

 blocks   
assorted useful components (mux, demux, or, and, integ, ddt, limit, wcomp, etc.)

#### Hardware Drivers<span id="sub:HardwareDrivers"></span>

 hal\_ax5214h   
A driver for the Axiom Measurement & Control AX5241H digital I/O board

 hal\_gm   
General Mechatronics GM6-PCI board

 hal\_m5i20   
Mesa Electronics 5i20 board

 hal\_motenc   
Vital Systems MOTENC-100 board

 hal\_parport   
PC parallel port.

 hal\_ppmc   
Pico Systems family of controllers (PPMC, USC and UPC)

 hal\_stg   
Servo To Go card (version 1 & 2)

 hal\_vti   
Vigilant Technologies PCI ENCDAC-4 controller

#### Tools and Utilities<span id="sub:ToolsUtilities"></span>

 halcmd   
Command line tool for configuration and tuning. See section [\[sec:Halcmd\]](#sec:Halcmd)

 halgui   
GUI tool for configuration and tuning (not implemented yet).

 halmeter   
A handy multimeter for HAL signals. See section [\[sec:Halmeter\]](#sec:Halmeter).

 halscope   
A full featured digital storage oscilloscope for HAL signals. See section [\[sec:Halscope\]](#sec:Halscope).

Each of these building blocks is described in detail in later chapters.

### Timing Issues In HAL<span id="sec:Timing-Issues"></span>

Unlike the physical wiring models between black boxes that we have said that HAL is based upon, simply connecting two pins with a hal-signal falls far short of the action of the physical case.

True relay logic consists of relays connected together, and when a contact opens or closes, current flows (or stops) immediately. Other coils may change state, etc, and it all just *happens*. But in PLC style ladder logic, it doesn’t work that way. Usually in a single pass through the ladder, each rung is evaluated in the order in which it appears, and only once per pass. A perfect example is a single rung ladder, with a NC contact in series with a coil. The contact and coil belong to the same relay.

If this were a conventional relay, as soon as the coil is energized, the contacts begin to open and de-energize it. That means the contacts close again, etc, etc. The relay becomes a buzzer.

With a PLC, if the coil is OFF and the contact is closed when the PLC begins to evaluate the rung, then when it finishes that pass, the coil is ON. The fact that turning on the coil opens the contact feeding it is ignored until the next pass. On the next pass, the PLC sees that the contact is open, and de-energizes the coil. So the relay still switches rapidly between on and off, but at a rate determined by how often the PLC evaluates the rung.

In HAL, the function is the code that evaluates the rung(s). In fact, the HAL-aware realtime version of ClassicLadder exports a function to do exactly that. Meanwhile, a thread is the thing that runs the function at specific time intervals. Just like you can choose to have a PLC evaluate all its rungs every 10 ms, or every second, you can define HAL threads with different periods.

What distinguishes one thread from another is *not* what the thread does - that is determined by which functions are connected to it. The real distinction is simply how often a thread runs.

In Machinekit you might have a 50 us thread and a 1 ms thread. These would be created based on BASE\_PERIOD and SERVO\_PERIOD, the actual times depend on the values in your ini file.

The next step is to decide what each thread needs to do. Some of those decisions are the same in (nearly) any Machinekit system—For instance, motion-command-handler is always added to servo-thread.

Other connections would be made by the integrator. These might include hooking the STG driver’s encoder read and DAC write functions to the servo thread, or hooking stepgen’s function to the base-thread, along with the parport function(s) to write the steps to the port.

Advanced HAL Tutorial
---------------------

<span id="cha:HAL-tutorial"></span>

### Introduction<span id="sec:Tutorial-Intro"></span>

Configuration moves from theory to device — HAL device that is. For those who have had just a bit of computer programming, this section is the *Hello World* of the HAL. Halrun can be used to create a working system. It is a command line or text file tool for configuration and tuning. The following examples illustrate its setup and operation.

#### Notation

Terminal commands are shown without the system prompt unless you are running *HAL*. The terminal window is in *Applications/Accessories* from the main Debian menu bar.

Terminal Command Example

    me@computer:~machinekit$ halrun
    (will be shown like the following line)
    halrun

    (the halcmd: prompt will be shown when running HAL)
    halcmd: loadrt debounce
    halcmd: show pin

#### Tab-completion

Your version of halcmd may include tab-completion. Instead of completing file names as a shell does, it completes commands with HAL identifiers. You will have to type enough letters for a unique match. Try pressing tab after starting a HAL command:

Tab Completion

    halcmd: loa<TAB>
    halcmd: load
    halcmd: loadrt
    halcmd: loadrt deb<TAB>
    halcmd: loadrt debounce

#### The RTAPI environment

RTAPI stands for Real Time Application Programming Interface. Many HAL components work in realtime, and all HAL components store data in shared memory so realtime components can access it. Normal Linux does not support realtime programming or the type of shared memory that HAL needs. Fortunately there are realtime operating systems (RTOS’s) that provide the necessary extensions to Linux. Unfortunately, each RTOS does things a little differently.

To address these differences, the Machinekit team came up with RTAPI, which provides a consistent way for programs to talk to the RTOS. If you are a programmer who wants to work on the internals of Machinekit, you may want to study *machinekit/src/rtapi/rtapi.h* to understand the API. But if you are a normal person all you need to know about RTAPI is that it (and the RTOS) needs to be loaded into the memory of your computer before you do anything with HAL.

### A Simple Example<span id="sec:Tutorial-Simple-Example"></span>

#### Loading a component

For this tutorial, we are going to assume that you have successfully installed the Live CD and, if using a RIP <span class="footnote">
\[Run In Place, when the source files have been downloaded to a user directory.\]
</span>, invoked the *rip-environment* script to prepare your shell. In that case, all you need to do is load the required RTOS and RTAPI modules into memory. Just run the following command from a terminal window:

Loading HAL

    cd machinekit
    halrun
    halcmd:

With the realtime OS and RTAPI loaded, we can move into the first example. Notice that the prompt is now shown as *halcmd:*. This is because subsequent commands will be interpreted as HAL commands, not shell commands.

For the first example, we will use a HAL component called *siggen*, which is a simple signal generator. A complete description of the *siggen* component can be found in the [Siggen](#sec:Siggen) section of this Manual. It is a realtime component, implemented as a Linux kernel module. To load *siggen* use the HAL command *loadrt*.

Loading siggen

    halcmd: loadrt siggen

#### Examining the HAL

Now that the module is loaded, it is time to introduce *halcmd* , the command line tool used to configure the HAL. This tutorial will introduce some halcmd features, for a more complete description try *man halcmd*, or see the reference in [Hal Commands](#sec:Hal-Commands) section of this document. The first halcmd feature is the *show* command. This command displays information about the current state of the HAL. To show all installed components:

Show Components

    halcmd: show comp

    Loaded HAL Components:
    ID     Type  Name                                PID   State
        3  RT    siggen                                    ready
        2  User  halcmd2177                          2177  ready

Since *halcmd* itself is a HAL component, it will always show up in the list. The number after halcmd in the component list is the process ID. It is possible to run more than one copy of halcmd at the same time (in different windows for example), so the PID is added to the end of the name to make it unique. The list also shows the *siggen* component that we installed in the previous step. The *RT* under *Type* indicates that *siggen* is a realtime component. The *User* under *Type* indicates it is a user space component.

Next, let’s see what pins *siggen* makes available:

Show Pins

    halcmd: show pin

    Component Pins:
    Owner   Type   Dir        Value  Name
         3  float  IN             1  siggen.0.amplitude
         3  bit    OUT        FALSE  siggen.0.clock
         3  float  OUT            0  siggen.0.cosine
         3  float  IN             1  siggen.0.frequency
         3  float  IN             0  siggen.0.offset
         3  float  OUT            0  siggen.0.sawtooth
         3  float  OUT            0  siggen.0.sine
         3  float  OUT            0  siggen.0.square
         3  float  OUT            0  siggen.0.triangle

This command displays all of the pins in the current HAL. A complex system could have dozens or hundreds of pins. But right now there are only nine pins. All eight of these pins are floating point, and carry data out of the *siggen* component. Since we have not yet executed the code contained within the component, some the pins have a value of zero.

The next step is to look at parameters:

Show Parameters

    halcmd: show param

    Parameters:
    Owner   Type  Dir        Value   Name
         3  s32   RO             0   siggen.0.update.time
         3  s32   RW             0   siggen.0.update.tmax

The *show param* command shows all the parameters in the HAL. Right now each parameter has the default value it was given when the component was loaded. Note the column labeled *Dir*. The parameters labeled *-W* are writable ones that are never changed by the component itself, instead they are meant to be changed by the user to control the component. We will see how to do this later. Parameters labeled *R-* are read only parameters. They can be changed only by the component. Finally, parameter labeled *RW* are read-write parameters. That means that they are changed by the component, but can also be changed by the user. Note: the parameters *siggen.0.update.time* and *siggen.0.update.tmax* are for debugging purposes, and won’t be covered in this section.

Most realtime components export one or more functions to actually run the realtime code they contain. Let’s see what function(s) *siggen* exported:

Show Functions

    halcmd: show funct

    Exported Functions:
    Owner   CodeAddr  Arg       FP   Users  Name
     00003  f801b000  fae820b8  YES      0   siggen.0.update

The siggen component exported a single function. It requires floating point. It is not currently linked to any threads, so *users* is zero.

#### Making realtime code run

To actually run the code contained in the function *siggen.0.update*, we need a realtime thread. The component called *threads* that is used to create a new thread. Lets create a thread called *test-thread* with a period of 1 ms (1,000 us or 1,000,000 ns):

    halcmd: loadrt threads name1=test-thread period1=1000000

Let’s see if that worked:

Show Threads

    halcmd: show thread

    Realtime Threads:
         Period  FP     Name               (     Time, Max-Time )
         999855  YES           test-thread (        0,        0 )

It did. The period is not exactly 1,000,000 ns because of hardware limitations, but we have a thread that runs at approximately the correct rate, and which can handle floating point functions. The next step is to connect the function to the thread:

Add Function

    halcmd: addf siggen.0.update test-thread

Up till now, we’ve been using *halcmd* only to look at the HAL. However, this time we used the *addf* (add function) command to actually change something in the HAL. We told *halcmd* to add the function *siggen.0.update* to the thread *test-thread*, and if we look at the thread list again, we see that it succeeded:

    halcmd: show thread

    Realtime Threads:
         Period  FP     Name                (     Time, Max-Time )
         999855  YES          test-thread   (        0,        0 )
                      1 siggen.0.update

There is one more step needed before the *siggen* component starts generating signals. When the HAL is first started, the thread(s) are not actually running. This is to allow you to completely configure the system before the realtime code starts. Once you are happy with the configuration, you can start the realtime code like this:

    halcmd: start

Now the signal generator is running. Let’s look at its output pins:

    halcmd: show pin

    Component Pins:
    Owner   Type  Dir         Value  Name
         3  float IN              1  siggen.0.amplitude
         3  bit   OUT         FALSE  siggen.0.clock
         3  float OUT    -0.1640929  siggen.0.cosine
         3  float IN              1  siggen.0.frequency
         3  float IN              0  siggen.0.offset
         3  float OUT    -0.4475303  siggen.0.sawtooth
         3  float OUT     0.9864449  siggen.0.sine
         3  float OUT            -1  siggen.0.square
         3  float OUT    -0.1049393  siggen.0.triangle

And let’s look again:

    halcmd: show pin

    Component Pins:
    Owner   Type  Dir         Value  Name
         3  float IN              1  siggen.0.amplitude
         3  bit   OUT         FALSE  siggen.0.clock
         3  float OUT     0.0507619  siggen.0.cosine
         3  float IN              1  siggen.0.frequency
         3  float IN              0  siggen.0.offset
         3  float OUT     -0.516165  siggen.0.sawtooth
         3  float OUT     0.9987108  siggen.0.sine
         3  float OUT            -1  siggen.0.square
         3  float OUT    0.03232994  siggen.0.triangle

We did two *show pin* commands in quick succession, and you can see that the outputs are no longer zero. The sine, cosine, sawtooth, and triangle outputs are changing constantly. The square output is also working, however it simply switches from +1.0 to -1.0 every cycle.

#### Changing Parameters

The real power of HAL is that you can change things. For example, we can use the *setp* command to set the value of a parameter. Let’s change the amplitude of the signal generator from 1.0 to 5.0:

Set Pin

    halcmd: setp siggen.0.amplitude 5

Check the parameters and pins again

    halcmd: show param

    Parameters:
    Owner   Type  Dir         Value  Name
         3  s32   RO           1754  siggen.0.update.time
         3  s32   RW          16997  siggen.0.update.tmax

    halcmd: show pin

    Component Pins:
    Owner   Type  Dir         Value  Name
         3  float IN              5  siggen.0.amplitude
         3  bit   OUT         FALSE  siggen.0.clock
         3  float OUT     0.8515425  siggen.0.cosine
         3  float IN              1  siggen.0.frequency
         3  float IN              0  siggen.0.offset
         3  float OUT      2.772382  siggen.0.sawtooth
         3  float OUT     -4.926954  siggen.0.sine
         3  float OUT             5  siggen.0.square
         3  float OUT      0.544764  siggen.0.triangle

Note that the value of parameter *siggen.0.amplitude* has changed to 5, and that the pins now have larger values.

#### Saving the HAL configuration

Most of what we have done with *halcmd* so far has simply been viewing things with the *show* command. However two of the commands actually changed things. As we design more complex systems with HAL, we will use many commands to configure things just the way we want them. HAL has the memory of an elephant, and will retain that configuration until we shut it down. But what about next time? We don’t want to manually enter a bunch of commands every time we want to use the system. We can save the configuration of the entire HAL with a single command:

Save

    halcmd: save
    # components
    loadrt threads name1=test-thread period1=1000000
    loadrt siggen
    # pin aliases
    # signals
    # nets
    # parameter values
    setp siggen.0.update.tmax 14687
    # realtime thread/function links
    addf siggen.0.update test-thread

The output of the *save* command is a sequence of HAL commands. If you start with an *empty* HAL and run all these commands, you will get the configuration that existed when the *save* command was issued. To save these commands for later use, we simply redirect the output to a file:

Save to a file

    halcmd: save all saved.hal

#### Exiting halrun

When you’re finished with your HAL session type *exit* at the *halcmd:* prompt. This will return you to the system prompt and close down the HAL session. Do not simply close the terminal window without shutting down the HAL session.

Exit HAL

    halcmd: exit

#### Restoring the HAL configuration

To restore the HAL configuration stored in *saved.hal*, we need to execute all of those HAL commands. To do that, we use *-f &lt;file name&gt;* which reads commands from a file, and *-I* (upper case i) which shows the halcmd prompt after executing the commands:

Run a Saved File

    halrun -I -f saved.hal

Notice that there is not a *start* command in saved.hal. It’s necessary to issue it again (or edit saved.hal to add it there).

#### Removing HAL from memory

If an unexpected shut down of a HAL session occurs you might have to unload HAL before another session can begin. To do this type the following command in a terminal window.

Removing HAL

    halrun -U

### Halmeter<span id="sec:Tutorial-Halmeter"></span>

You can build very complex HAL systems without ever using a graphical interface. However there is something satisfying about seeing the result of your work. The first and simplest GUI tool for the HAL is halmeter. It is a very simple program that is the HAL equivalent of the handy Fluke multimeter (or Simpson analog meter for the old timers).

We will use the siggen component again to check out halmeter. If you just finished the previous example, then you can load siggen using the saved file. If not, we can load it just like we did before:

    halrun
    halcmd: loadrt siggen
    halcmd: loadrt threads name1=test-thread period1=1000000
    halcmd: addf siggen.0.update test-thread
    halcmd: start
    halcmd: setp siggen.0.amplitude 5

At this point we have the siggen component loaded and running. It’s time to start halmeter.

Starting Halmeter

    halcmd: loadusr halmeter

The first window you will see is the *Select Item to Probe* window.

![images/halmeter-select.png]()

Figure 1. Halmeter Select Window<span id="cap:HAL-Meter-Select-Window"></span>

This dialog has three tabs. The first tab displays all of the HAL pins in the system. The second one displays all the signals, and the third displays all the parameters. We would like to look at the pin *siggen.0.cosine* first, so click on it then click the *Close* button. The probe selection dialog will close, and the meter looks something like the following figure.

![images/halmeter-1.png]()

Figure 2. Halmeter<span id="cap:Halmeter"></span>

To change what the meter displays press the *Select* button which brings back the *Select Item to Probe* window.

You should see the value changing as siggen generates its cosine wave. Halmeter refreshes its display about 5 times per second.

To shut down halmeter, just click the exit button.

If you want to look at more than one pin, signal, or parameter at a time, you can just start more halmeters. The halmeter window was intentionally made very small so you could have a lot of them on the screen at once.

### Stepgen Example

Up till now we have only loaded one HAL component. But the whole idea behind the HAL is to allow you to load and connect a number of simple components to make up a complex system. The next example will use two components.

Before we can begin building this new example, we want to start with a clean slate. If you just finished one of the previous examples, we need to remove the all components and reload the RTAPI and HAL libraries.

    halcmd: exit

#### Installing the components

Now we are going to load the step pulse generator component. For a detailed description of this component refer to the stepgen section of the Integrator Manual. In this example we will use the *velocity* control type of stepgen. For now, we can skip the details, and just run the following commands.

    halrun
    halcmd: loadrt stepgen step_type=0,0 ctrl_type=v,v
    halcmd: loadrt siggen
    halcmd: loadrt threads name1=fast fp1=0 period1=50000 name2=slow period2=1000000

The first command loads two step generators, both configured to generate stepping type 0. The second command loads our old friend siggen, and the third one creates two threads, a fast one with a period of 50 microseconds and a slow one with a period of 1 millisecond. The fast thread doesn’t support floating point functions.

As before, we can use *halcmd show* to take a look at the HAL. This time we have a lot more pins and parameters than before:

    halcmd: show pin

    Component Pins:
    Owner   Type  Dir         Value  Name
         4  float IN              1  siggen.0.amplitude
         4  bit   OUT         FALSE  siggen.0.clock
         4  float OUT             0  siggen.0.cosine
         4  float IN              1  siggen.0.frequency
         4  float IN              0  siggen.0.offset
         4  float OUT             0  siggen.0.sawtooth
         4  float OUT             0  siggen.0.sine
         4  float OUT             0  siggen.0.square
         4  float OUT             0  siggen.0.triangle
         3  s32   OUT             0  stepgen.0.counts
         3  bit   OUT         FALSE  stepgen.0.dir
         3  bit   IN          FALSE  stepgen.0.enable
         3  float OUT             0  stepgen.0.position-fb
         3  bit   OUT         FALSE  stepgen.0.step
         3  float IN              0  stepgen.0.velocity-cmd
         3  s32   OUT             0  stepgen.1.counts
         3  bit   OUT         FALSE  stepgen.1.dir
         3  bit   IN          FALSE  stepgen.1.enable
         3  float OUT             0  stepgen.1.position-fb
         3  bit   OUT         FALSE  stepgen.1.step
         3  float IN              0  stepgen.1.velocity-cmd

    halcmd: show param

    Parameters:
    Owner   Type  Dir         Value  Name
         4  s32   RO              0  siggen.0.update.time
         4  s32   RW              0  siggen.0.update.tmax
         3  u32   RW     0x00000001  stepgen.0.dirhold
         3  u32   RW     0x00000001  stepgen.0.dirsetup
         3  float RO              0  stepgen.0.frequency
         3  float RW              0  stepgen.0.maxaccel
         3  float RW              0  stepgen.0.maxvel
         3  float RW              1  stepgen.0.position-scale
         3  s32   RO              0  stepgen.0.rawcounts
         3  u32   RW     0x00000001  stepgen.0.steplen
         3  u32   RW     0x00000001  stepgen.0.stepspace
         3  u32   RW     0x00000001  stepgen.1.dirhold
         3  u32   RW     0x00000001  stepgen.1.dirsetup
         3  float RO              0  stepgen.1.frequency
         3  float RW              0  stepgen.1.maxaccel
         3  float RW              0  stepgen.1.maxvel
         3  float RW              1  stepgen.1.position-scale
         3  s32   RO              0  stepgen.1.rawcounts
         3  u32   RW     0x00000001  stepgen.1.steplen
         3  u32   RW     0x00000001  stepgen.1.stepspace
         3  s32   RO              0  stepgen.capture-position.time
         3  s32   RW              0  stepgen.capture-position.tmax
         3  s32   RO              0  stepgen.make-pulses.time
         3  s32   RW              0  stepgen.make-pulses.tmax
         3  s32   RO              0  stepgen.update-freq.time
         3  s32   RW              0  stepgen.update-freq.tmax

#### Connecting pins with signals

What we have is two step pulse generators, and a signal generator. Now it is time to create some HAL signals to connect the two components. We are going to pretend that the two step pulse generators are driving the X and Y axis of a machine. We want to move the table in circles. To do this, we will send a cosine signal to the X axis, and a sine signal to the Y axis. The siggen module creates the sine and cosine, but we need *wires* to connect the modules together. In the HAL, *wires* are called signals. We need to create two of them. We can call them anything we want, for this example they will be *X-vel* and *Y-vel*. The signal *X-vel* is intended to run from the cosine output of the signal generator to the velocity input of the first step pulse generator. The first step is to connect the signal to the signal generator output. To connect a signal to a pin we use the net command.

net command

    halcmd: net X-vel <= siggen.0.cosine

To see the effect of the *net* command, we show the signals again.

    halcmd: show sig

    Signals:
    Type          Value  Name     (linked to)
    float             0  X-vel <== siggen.0.cosine

When a signal is connected to one or more pins, the show command lists the pins immediately following the signal name. The *arrow* shows the direction of data flow - in this case, data flows from pin *siggen.0.cosine* to signal *X-vel*. Now let’s connect the *X-vel* to the velocity input of a step pulse generator.

    halcmd: net X-vel => stepgen.0.velocity-cmd

We can also connect up the Y axis signal *Y-vel*. It is intended to run from the sine output of the signal generator to the input of the second step pulse generator. The following command accomplishes in one line what two *net* commands accomplished for *X-vel*.

    halcmd: net Y-vel siggen.0.sine => stepgen.1.velocity-cmd

Now let’s take a final look at the signals and the pins connected to them.

    halcmd: show sig

    Signals:
    Type          Value  Name     (linked to)
    float             0  X-vel <== siggen.0.cosine
                               ==> stepgen.0.velocity-cmd
    float             0  Y-vel <== siggen.0.sine
                               ==> stepgen.1.velocity-cmd

The *show sig* command makes it clear exactly how data flows through the HAL. For example, the *X-vel* signal comes from pin *siggen.0.cosine*, and goes to pin *stepgen.0.velocity-cmd*.

#### Setting up realtime execution - threads and functions

Thinking about data flowing through *wires* makes pins and signals fairly easy to understand. Threads and functions are a little more difficult. Functions contain the computer instructions that actually get things done. Thread are the method used to make those instructions run when they are needed. First let’s look at the functions available to us.

    halcmd: show funct

    Exported Functions:
    Owner   CodeAddr  Arg       FP   Users  Name
     00004  f9992000  fc731278  YES      0   siggen.0.update
     00003  f998b20f  fc7310b8  YES      0   stepgen.capture-position
     00003  f998b000  fc7310b8  NO       0   stepgen.make-pulses
     00003  f998b307  fc7310b8  YES      0   stepgen.update-freq

In general, you will have to refer to the documentation for each component to see what its functions do. In this case, the function *siggen.0.update* is used to update the outputs of the signal generator. Every time it is executed, it calculates the values of the sine, cosine, triangle, and square outputs. To make smooth signals, it needs to run at specific intervals.

The other three functions are related to the step pulse generators.

The first one, *stepgen.capture\_position*, is used for position feedback. It captures the value of an internal counter that counts the step pulses as they are generated. Assuming no missed steps, this counter indicates the position of the motor.

The main function for the step pulse generator is *stepgen.make\_pulses*. Every time *make\_pulses* runs it decides if it is time to take a step, and if so sets the outputs accordingly. For smooth step pulses, it should run as frequently as possible. Because it needs to run so fast, *make\_pulses* is highly optimized and performs only a few calculations. Unlike the others, it does not need floating point math.

The last function, *stepgen.update-freq*, is responsible for doing scaling and some other calculations that need to be performed only when the frequency command changes.

What this means for our example is that we want to run *siggen.0.update* at a moderate rate to calculate the sine and cosine values. Immediately after we run *siggen.0.update*, we want to run *stepgen.update\_freq* to load the new values into the step pulse generator. Finally we need to run *stepgen.make\_pulses* as fast as possible for smooth pulses. Because we don’t use position feedback, we don’t need to run *stepgen.capture\_position* at all.

We run functions by adding them to threads. Each thread runs at a specific rate. Let’s see what threads we have available.

    halcmd: show thread

    Realtime Threads:
         Period  FP     Name               (     Time, Max-Time )
         996980  YES                  slow (        0,        0 )
          49849  NO                   fast (        0,        0 )

The two threads were created when we loaded *threads*. The first one, *slow*, runs every millisecond, and is capable of running floating point functions. We will use it for *siggen.0.update* and *stepgen.update\_freq*. The second thread is *fast*, which runs every 50 microseconds, and does not support floating point. We will use it for *stepgen.make\_pulses*. To connect the functions to the proper thread, we use the *addf* command. We specify the function first, followed by the thread.

    halcmd: addf siggen.0.update slow
    halcmd: addf stepgen.update-freq slow
    halcmd: addf stepgen.make-pulses fast

After we give these commands, we can run the *show thread* command again to see what happened.

    halcmd: show thread

    Realtime Threads:
         Period  FP     Name               (     Time, Max-Time )
         996980  YES                  slow (        0,        0 )
                      1 siggen.0.update
                      2 stepgen.update-freq
          49849  NO                   fast (        0,        0 )
                      1 stepgen.make-pulses

Now each thread is followed by the names of the functions, in the order in which the functions will run.

#### Setting parameters

We are almost ready to start our HAL system. However we still need to adjust a few parameters. By default, the siggen component generates signals that swing from +1 to -1. For our example that is fine, we want the table speed to vary from +1 to -1 inches per second. However the scaling of the step pulse generator isn’t quite right. By default, it generates an output frequency of 1 step per second with an input of 1.000. It is unlikely that one step per second will give us one inch per second of table movement. Let’s assume instead that we have a 5 turn per inch leadscrew, connected to a 200 step per rev stepper with 10x microstepping. So it takes 2000 steps for one revolution of the screw, and 5 revolutions to travel one inch. that means the overall scaling is 10000 steps per inch. We need to multiply the velocity input to the step pulse generator by 10000 to get the proper output. That is exactly what the parameter *stepgen.n.velocity-scale* is for. In this case, both the X and Y axis have the same scaling, so we set the scaling parameters for both to 10000.

    halcmd: setp stepgen.0.position-scale 10000
    halcmd: setp stepgen.1.position-scale 10000
    halcmd: setp stepgen.0.enable 1
    halcmd: setp stepgen.1.enable 1

This velocity scaling means that when the pin *stepgen.0.velocity-cmd* is 1.000, the step generator will generate 10000 pulses per second (10KHz). With the motor and leadscrew described above, that will result in the axis moving at exactly 1.000 inches per second. This illustrates a key HAL concept - things like scaling are done at the lowest possible level, in this case in the step pulse generator. The internal signal *X-vel* is the velocity of the table in inches per second, and other components such as *siggen* don’t know (or care) about the scaling at all. If we changed the leadscrew, or motor, we would change only the scaling parameter of the step pulse generator.

#### Run it!

We now have everything configured and are ready to start it up. Just like in the first example, we use the *start* command.

    halcmd: start

Although nothing appears to happen, inside the computer the step pulse generator is cranking out step pulses, varying from 10KHz forward to 10KHz reverse and back again every second. Later in this tutorial we’ll see how to bring those internal signals out to run motors in the real world, but first we want to look at them and see what is happening.

### Halscope<span id="sec:Tutorial-Halscope"></span>

The previous example generates some very interesting signals. But much of what happens is far too fast to see with halmeter. To take a closer look at what is going on inside the HAL, we want an oscilloscope. Fortunately HAL has one, called halscope.

Halscope has two parts - a realtime part that is loaded as a kernel module, and a user part that supplies the GUI and display. However, you don’t need to worry about this, because the userspace portion will automatically request that the realtime part be loaded. Also notice the first time you run halscope in a directory it gives a benign message that the file *autosave.halscope* could not be opened.

Starting Halscope

    halcmd: loadusr halscope
    halcmd: halscope: config file 'autosave.halscope' could not be opened

The scope GUI window will open, immediately followed by a *Realtime function not linked* dialog that looks like the following figure.

![images/halscope-01.png]()

Figure 3. Realtime function not linked dialog<span id="cap:Realtime-function-not-linked"></span>

This dialog is where you set the sampling rate for the oscilloscope. For now we want to sample once per millisecond, so click on the 989 us thread *slow* and leave the multiplier at 1. We will also leave the record length at 4000 samples, so that we can use up to four channels at one time. When you select a thread and then click *OK*, the dialog disappears, and the scope window looks something like the following figure.

![images/halscope-02.png]()

Figure 4. Initial scope window<span id="cap:Initial-scope-window"></span>

#### Hooking up the scope probes

At this point, Halscope is ready to use. We have already selected a sample rate and record length, so the next step is to decide what to look at. This is equivalent to hooking *virtual scope probes* to the HAL. Halscope has 16 channels, but the number you can use at any one time depends on the record length - more channels means shorter records, since the memory available for the record is fixed at approximately 16,000 samples.

The channel buttons run across the bottom of the halscope screen. Click button *1*, and you will see the *Select Channel Source* dialog as shown in the following figure. This dialog is very similar to the one used by Halmeter. We would like to look at the signals we defined earlier, so we click on the *Signals* tab, and the dialog displays all of the signals in the HAL (only two for this example).

![images/halscope-03.png]()

Figure 5. Select Channel Source<span id="cap:Select-Channel-Source"></span>

To choose a signal, just click on it. In this case, we want channel 1 to display the signal *X-vel*. Click on the Signals tab then click on *X-vel* and the dialog closes and the channel is now selected.

![images/halscope-04.png]()

Figure 6. Select Signal<span id="cap:Select-Signal"></span>

The channel 1 button is pressed in, and channel number 1 and the name *X-vel* appear below the row of buttons. That display always indicates the selected channel - you can have many channels on the screen, but the selected one is highlighted, and the various controls like vertical position and scale always work on the selected one.

![images/halscope-05.png]()

Figure 7. Halscope<span id="cap:HALScope"></span>

To add a signal to channel 2, click the *2* button. When the dialog pops up, click the *Signals* tab, then click on *Y-vel*. We also want to look at the square and triangle wave outputs. There are no signals connected to those pins, so we use the *Pins* tab instead. For channel 3, select *siggen.0.triangle* and for channel 4, select *siggen.0.square*.

#### Capturing our first waveforms

Now that we have several probes hooked to the HAL, it’s time to capture some waveforms. To start the scope, click the *Normal* button in the *Run Mode* section of the screen (upper right). Since we have a 4000 sample record length, and are acquiring 1000 samples per second, it will take halscope about 2 seconds to fill half of its buffer. During that time a progress bar just above the main screen will show the buffer filling. Once the buffer is half full, the scope waits for a trigger. Since we haven’t configured one yet, it will wait forever. To manually trigger it, click the *Force* button in the *Trigger* section at the top right. You should see the remainder of the buffer fill, then the screen will display the captured waveforms. The result will look something like the following figure.

![images/halscope-06.png]()

Figure 8. Captured Waveforms<span id="cap:Captured-Waveforms"></span>

The *Selected Channel* box at the bottom tells you that the purple trace is the currently selected one, channel 4, which is displaying the value of the pin *siggen.0.square*. Try clicking channel buttons 1 through 3 to highlight the other three traces.

#### Vertical Adjustments

The traces are rather hard to distinguish since all four are on top of each other. To fix this, we use the *Vertical* controls in the box to the right of the screen. These controls act on the currently selected channel. When adjusting the gain, notice that it covers a huge range - unlike a real scope, this one can display signals ranging from very tiny (pico-units) to very large (Tera-units). The position control moves the displayed trace up and down over the height of the screen only. For larger adjustments the offset button should be used.

![images/halscope-07.png]()

Figure 9. Vertical Adjustment<span id="cap:Vertical-Adjustment"></span>

#### Triggering

Using the *Force* button is a rather unsatisfying way to trigger the scope. To set up real triggering, click on the *Source* button at the bottom right. It will pop up the *Trigger Source* dialog, which is simply a list of all the probes that are currently connected. Select a probe to use for triggering by clicking on it. For this example we will use channel 3, the triangle wave as shown in the following figure.

![images/halscope-08.png]()

Figure 10. Trigger Source Dialog<span id="cap:Trigger-Source-Dialog"></span>

After setting the trigger source, you can adjust the trigger level and trigger position using the sliders in the *Trigger* box along the right edge. The level can be adjusted from the top to the bottom of the screen, and is displayed below the sliders. The position is the location of the trigger point within the overall record. With the slider all the way down, the trigger point is at the end of the record, and halscope displays what happened before the trigger point. When the slider is all the way up, the trigger point is at the beginning of the record, displaying what happened after it was triggered. The trigger point is visible as a vertical line in the progress box above the screen. The trigger polarity can be changed by clicking the button just below the trigger level display.

Now that we have adjusted the vertical controls and triggering, the scope display looks something like the following figure.

![images/halscope-09.png]()

Figure 11. Waveforms with Triggering<span id="cap:Waveforms-with-Triggering"></span>

#### Horizontal Adjustments

To look closely at part of a waveform, you can use the zoom slider at the top of the screen to expand the waveforms horizontally, and the position slider to determine which part of the zoomed waveform is visible. However, sometimes simply expanding the waveforms isn’t enough and you need to increase the sampling rate. For example, we would like to look at the actual step pulses that are being generated in our example. Since the step pulses may be only 50 us long, sampling at 1KHz isn’t fast enough. To change the sample rate, click on the button that displays the number of samples and sample rate to bring up the *Select Sample Rate* dialog, figure . For this example, we will click on the 50 us thread, *fast*, which gives us a sample rate of about 20KHz. Now instead of displaying about 4 seconds worth of data, one record is 4000 samples at 20KHz, or about 0.20 seconds.

![images/halscope-10.png]()

Figure 12. Sample Rate Dialog<span id="cap:Sample-Rate-Dialog"></span>

#### More Channels

Now let’s look at the step pulses. Halscope has 16 channels, but for this example we are using only 4 at a time. Before we select any more channels, we need to turn off a couple. Click on the channel 2 button, then click the *Chan Off* button at the bottom of the *Vertical* box. Then click on channel 3, turn if off, and do the same for channel 4. Even though the channels are turned off, they still remember what they are connected to, and in fact we will continue to use channel 3 as the trigger source. To add new channels, select channel 5, and choose pin *stepgen.0.dir*, then channel 6, and select *stepgen.0.step*. Then click run mode *Normal* to start the scope, and adjust the horizontal zoom to 5 ms per division. You should see the step pulses slow down as the velocity command (channel 1) approaches zero, then the direction pin changes state and the step pulses speed up again. You might want to increase the gain on channel 1 to about 20 milli per division to better see the change in the velocity command. The result should look like the following figure.

![images/halscope-11.png]()

Figure 13. Step Pulses<span id="cap:Step-Pulses"></span>

#### More samples

If you want to record more samples at once, restart realtime and load halscope with a numeric argument which indicates the number of samples you want to capture.

    halcmd: loadusr halscope 80000

If the *scope\_rt* component was not already loaded, halscope will load it and request 80000 total samples, so that when sampling 4 channels at a time there will be 20000 samples per channel. (If *scope\_rt* was already loaded, the numeric argument to halscope will have no effect).

General Reference
-----------------

### General Naming Conventions<span id="sec:GR-Naming-Conventions"></span>

Consistent naming conventions would make HAL much easier to use. For example, if every encoder driver provided the same set of pins and named them the same way it would be easy to change from one type of encoder driver to another. Unfortunately, like many open-source projects, HAL is a combination of things that were designed, and things that simply evolved. As a result, there are many inconsistencies. This section attempts to address that problem by defining some conventions, but it will probably be a while before all the modules are converted to follow them.

Halcmd and other low-level HAL utilities treat HAL names as single entities, with no internal structure. However, most modules do have some implicit structure. For example, a board provides several functional blocks, each block might have several channels, and each channel has one or more pins. This results in a structure that resembles a directory tree. Even though halcmd doesn’t recognize the tree structure, proper choice of naming conventions will let it group related items together (since it sorts the names). In addition, higher level tools can be designed to recognize such structure, if the names provide the necessary information. To do that, all HAL components should follow these rules:

-   Dots (“.”) separate levels of the hirearchy. This is analogous to the slash (“/”) in a filename.

-   Hyphens (“-”) separate words or fields in the same level of the hirearchy.

-   HAL components should not use underscores or “MixedCase”.

-   Use only lowercase letters and numbers in names.

### Hardware Driver Naming Conventions <span id="sec:GR-Driver-Naming"></span>

#### Pin/Parameter names

Hardware drivers should use five fields (on three levels) to make up a pin or parameter name, as follows:

`<device-name>.<device-num>.<io-type>.<chan-num>.<specific-name>`

The individual fields are:

 &lt;device-name&gt;   
The device that the driver is intended to work with. This is most often an interface board of some type, but there are other possibilities.

 &lt;device-num&gt;   
It is possible to install more than one servo board, parallel port, or other hardware device in a computer. The device number identifies a specific device. Device numbers start at 0 and increment.

 &lt;io-type&gt;   
Most devices provide more than one type of I/O. Even the simple parallel port has both digital inputs and digital outputs. More complex boards can have digital inputs and outputs, encoder counters, pwm or step pulse generators, analog-to-digital converters, digital-to-analog converters, or other unique capabilities. The I/O type is used to identify the kind of I/O that a pin or parameter is associated with. Ideally, drivers that implement the same I/O type, even if for very different devices, should provide a consistent set of pins and parameters and identical behavior. For example, all digital inputs should behave the same when seen from inside the HAL, regardless of the device.

 &lt;chan-num&gt;   
Virtually every I/O device has multiple channels, and the channel number identifies one of them. Like device numbers, channel numbers start at zero and increment.<span class="footnote">
\[One exception to the “channel numbers start at zero” rule is the parallel port. Its HAL pins are numbered with the corresponding pin number on the DB-25 connector. This is convenient for wiring, but inconsistent with other drivers. There is some debate over whether this is a bug or a feature.\]
</span> If more than one device is installed, the channel numbers on additional devices start over at zero. If it is possible to have a channel number greater than 9, then channel numbers should be two digits, with a leading zero on numbers less than 10 to preserve sort ordering. Some modules have pins and/or parameters that affect more than one channel. For example a PWM generator might have four channels with four independent “duty-cycle” inputs, but one “frequency” parameter that controls all four channels (due to hardware limitations). The frequency parameter should use “0-3” as the channel number.

 &lt;specific-name&gt;   
An individual I/O channel might have just a single HAL pin associated with it, but most have more than one. For example, a digital input has two pins, one is the state of the physical pin, the other is the same thing inverted. That allows the configurator to choose between active high and active low inputs. For most io-types, there is a standard set of pins and parameters, (referred to as the “canonical interface”) that the driver should implement. The canonical interfaces are described in the [Canonical Device Interfaces](#cha:Canonical-Device-Interfaces) chapter.

Examples

 motenc.0.encoder.2.position   
 — the position output of the third encoder channel on the first Motenc board.

 stg.0.din.03.in   
 — the state of the fourth digital input on the first Servo-to-Go board.

 ppmc.0.pwm.00-03.frequency   
 — the carrier frequency used for PWM channels 0 through 3 on the first Pico Systems ppmc board.

#### Function Names

Hardware drivers usually only have two kinds of HAL functions, ones that read the hardware and update HAL pins, and ones that write to the hardware using data from HAL pins. They should be named as follows:

`<device-name>-<device-num>.<io-type>-<chan-num-range>.read|write`

 &lt;device-name&gt;   
The same as used for pins and parameters.

 &lt;device-num&gt;   
The specific device that the function will access.

 &lt;io-type&gt;   
Optional. A function may access all of the I/O on a board, or it may access only a certain type. For example, there may be independent functions for reading encoder counters and reading digital I/O. If such independent functions exist, the &lt;io-type&gt; field identifies the type of I/O they access. If a single function reads all I/O provided by the board, &lt;io-type&gt; is not used. <span class="footnote">
\[Note to driver programmers: do NOT implement separate functions for different I/O types unless they are interruptible and can work in independent threads. If interrupting an encoder read, reading digital inputs, and then resuming the encoder read will cause problems, then implement a single function that does everything.\]
</span>

 &lt;chan-num-range&gt;   
Optional. Used only if the &lt;io-type&gt; I/O is broken into groups and accessed by different functions.

 read|write   
Indicates whether the function reads the hardware or writes to it.

Examples

 motenc.0.encoder.read   
 — reads all encoders on the first motenc board.

 generic8255.0.din.09-15.read   
 — reads the second 8 bit port on the first generic 8255 based digital I/O board.

 ppmc.0.write   
 — writes all outputs (step generators, pwm, DACs, and digital) on the first Pico Systems ppmc board.

Canonical Device Interfaces
---------------------------

Note

By version 2.1, the HAL drivers should have all been updated to match these specs. Send an email if you spot any problems.

### Introduction

The following sections show the pins, parameters, and functions that are supplied by “canonical devices”. All HAL device drivers should supply the same pins and parameters, and implement the same behavior.

Note that the only the `<io-type>` and `<specific-name>` fields are defined for a canonical device. The `<device-name>`, `<device-num>`, and `<chan-num>` fields are set based on the characteristics of the real device.

### Digital Input<span id="sec:CanonDigIn"></span>

The canonical digital input (I/O type field: `digin`) is quite simple.

#### Pins

-   (bit) **in** — State of the hardware input.

-   (bit) **in-not** — Inverted state of the input.

#### Parameters

-   None

#### Functions

-   (funct) **read** — Read hardware and set `in` and `in-not` HAL pins.

### Digital Output<span id="sec:CanonDigOut"></span>

The canonical digital output (I/O type field: `digout`) is also very simple.

#### Pins

-   (bit) **out** — Value to be written (possibly inverted) to the hardware output.

#### Parameters

-   (bit) **invert** — If TRUE, **out** is inverted before writing to the hardware.

#### Functions

-   (funct) **write** — Read **out** and **invert**, and set hardware output accordingly.

### Analog Input

The canonical analog input (I/O type: `adcin` ). This is expected to be used for analog to digital converters, which convert e.g. voltage to a continuous range of values.

#### Pins

-   (float) **value** — The hardware reading, scaled according to the **scale** and **offset** parameters. **Value** = ((input reading, in hardware-dependent units) \* **scale**) - **offset**

#### Parameters

-   (float) **scale** — The input voltage (or current) will be multiplied by **scale** before being output to **value**.

-   (float) **offset** — This will be subtracted from the hardware input voltage (or current) after the scale multiplier has been applied.

-   (float) **bit\_weight** — The value of one least significant bit (LSB). This is effectively the granularity of the input reading.

-   (float) **hw\_offset** — The value present on the input when 0 volts is applied to the input pin(s).

#### Functions

-   (funct) **read** — Read the values of this analog input channel. This may be used for individual channel reads, or it may cause all channels to be read

### Analog Output

The canonical analog output (I/O Type: **adcout**). This is intended for any kind of hardware that can output a more-or-less continuous range of values. Examples are digital to analog converters or PWM generators.

#### Pins

-   (float) **value** — The value to be written. The actual value output to the hardware will depend on the scale and offset parameters.

-   (bit) **enable** — If false, then output 0 to the hardware, regardless of the **value** pin.

#### Parameters

-   (float) **offset** — This will be added to the **value** before the hardware is updated

-   (float) **scale** — This should be set so that an input of 1 on the **value** pin will cause the analog output pin to read 1 volt.

-   (float) **high\_limit** (optional) — When calculating the value to output to the hardware, if **value** + **offset** is greater than **high\_limit**, then **high\_limit** will be used instead.

-   (float) **low\_limit** (optional) — When calculating the value to output to the hardware, if **value** + **offset** is less than **low\_limit**, then **low\_limit** will be used instead.

-   (float) **bit\_weight** (optional) — The value of one least significant bit (LSB), in volts (or mA, for current outputs)

-   (float) **hw\_offset** (optional) — The actual voltage (or current) that will be output if 0 is written to the hardware.

#### Functions

(funct) **write**  — This causes the calculated value to be output to the hardware. If enable is false, then the output will be 0, regardless of **value**, **scale**, and **offset**. The meaning of “0” is dependent on the hardware. For example, a bipolar 12-bit A/D may need to write 0x1FF (mid scale) to the D/A get 0 volts from the hardware pin. If enable is true, read scale, offset and value and output to the adc (**scale** \* **value**) + **offset**. If enable is false, then output 0.

HAL Tools
---------

<span id="cha:hal-tools"></span>

### Halcmd<span id="sec:Halcmd"></span>

Halcmd is a command line tool for manipulating the HAL. There is a rather complete man page for halcmd, which will be installed if you have installed Machinekit from either source or a package. If you have compiled Machinekit for “run-in-place”, the man page is not installed, but it is accessible. From the main Machinekit directory, do:

    man -M docs/man halcmd

The [HAL Tutorial](#cha:HAL-tutorial) has a number of examples of halcmd usage, and is a good tutorial for halcmd.

### Halmeter<span id="sec:Halmeter"></span>

Halmeter is a *voltmeter* for the HAL. It lets you look at a pin, signal, or parameter, and displays the current value of that item. It is pretty simple to use. Start it by typing **halmeter** in an X windows shell. Halmeter is a GUI application. It will pop up a small window, with two buttons labeled *Select* and *Exit*. Exit is easy - it shuts down the program. Select pops up a larger window, with three tabs. One tab lists all the pins currently defined in the HAL. The next lists all the signals, and the last tab lists all the parameters. Click on a tab, then click on a pin/signal/parameter. Then click on *OK*. The lists will disappear, and the small window will display the name and value of the selected item. The display is updated approximately 10 times per second. If you click *Accept* instead of *OK*, the small window will display the name and value of the selected item, but the large window will remain on the screen. This is convenient if you want to look at a number of different items quickly.

You can have many halmeters running at the same time, if you want to monitor several items. If you want to launch a halmeter without tying up a shell window, type *halmeter &* to run it in the background. You can also make halmeter start displaying a specific item immediately, by adding *pin|sig|par\[am\] &lt;name&gt;* to the command line. It will display the pin, signal, or parameter &lt;name&gt; as soon as it starts. (If there is no such item, it will simply start normally.) And finally, if you specify an item to display, you can add *-s* before the pin|sig|param to tell halmeter to use a small window. The item name will be displayed in the title bar instead of under the value, and there will be no buttons. Useful when you want a lot of meters in a small amount of screen space.

Refer to [Halmeter Tutorial](#sec:Tutorial-Halmeter) section for more information.

Halmeter can be loaded from a terminal or from Axis. Halmeter is faster than Halshow at displaying values. Halmeter has two windows, one to pick the pin, signal, or parameter to monitor and one that displays the value. Multiple Halmeters can be open at the same time. If you use a script to open multiple Halmeters you can set the position of each one with -g X Y relative to the upper left corner of your screen. For example:

    loadusr halmeter pin hm2.0.stepgen.00.velocity-fb -g 0 500

See the man page for more options. See section [Halmeter](#sec:Halmeter).

![images/hal-meter01.png]()

Figure 14. Halmeter

![images/hal-meter02.png]()

### Halscope<span id="sec:Halscope"></span>

Halscope is an *oscilloscope* for the HAL. It lets you capture the value of pins, signals, and parameters as a function of time. Complete operating instructions should be located here eventually. For now, refer to section [\[sec:Tutorial-Halscope\]](#sec:Tutorial-Halscope) in the tutorial chapter, which explains the basics.

Basic HAL Tutorial
------------------

<span id="cha:basic-hal-tutorial"></span>

### HAL Commands <span id="sec:Hal-Commands"></span>

More detailed information can be found in the man page for halcmd *man halcmd* in a terminal window. To see the HAL configuration and check the status of pins and parameters use the HAL Configuration window on the Machine menu in AXIS. To watch a pin status open the Watch tab and click on each pin you wish to watch and it will be added to the watch window.

![images/HAL\_Configuration.png]()

Figure 15. HAL Configuration Window<span id="cap:HAL-Configuration-Window"></span>

#### loadrt <span id="sub:loadrt"></span>

The command *loadrt* loads a real time HAL component. Real time component functions need to be added to a thread to be updated at the rate of the thread. You cannot load a user space component into the real time space.

The syntax and an example:

    loadrt <component> <options>

    loadrt mux4 count=1

#### addf <span id="sub:addf"></span>

The command *addf* adds a real time component function to a thread. You have to add a function from a HAL real time component to a thread to get the function to update at the rate of the thread.

If you used the Stepper Config Wizard to generate your config you will have two threads.

-   base-thread (the high-speed thread): this thread handles items that need a fast response, like making step pulses, and reading and writing the parallel port.

-   servo-thread (the slow-speed thread): this thread handles items that can tolerate a slower response, like the motion controller, ClassicLadder, and the motion command handler.

The syntax and an example:

    addf <component> <thread>

    addf mux4 servo-thread

#### loadusr <span id="sub:loadusr"></span>

The command *loadusr* loads a user space HAL component. User space programs are their own separate processes, which optionally talk to other HAL components via pins and parameters. You cannot load real time components into user space.

Flags may be one or more of the following:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">-W<br />
</td>
<td align="left"><p>to wait for the component to become ready. The component is assumed to have the same name as the first argument of the command.</p></td>
</tr>
<tr class="even">
<td align="left">-Wn &lt;name&gt;<br />
</td>
<td align="left"><p>to wait for the component, which will have the given &lt;name&gt;. This only applies if the component has a name option.</p></td>
</tr>
<tr class="odd">
<td align="left">-w<br />
</td>
<td align="left"><p>to wait for the program to exit</p></td>
</tr>
<tr class="even">
<td align="left">-i<br />
</td>
<td align="left"><p>to ignore the program return value (with -w)</p></td>
</tr>
<tr class="odd">
<td align="left">-n<br />
</td>
<td align="left"><p>name a component when it is a valid option for that component.</p></td>
</tr>
</tbody>
</table>

The syntax and examples:

    loadusr <component> <options>

    loadusr halui

    loadusr -Wn spindle gs2_vfd -n spindle

In English it means *loadusr wait for name spindle component gs2\_vfd name spindle*.

#### net <span id="sub:net"></span>

The command *net* creates a *connection* between a signal and and one or more pins. If the signal does not exist net creates the new signal. This replaces the need to use the command newsig. The optional direction indicators *⇐*, *⇒* and *&lt;⇒* are only to make it easier for humans to follow the logic and are not used by the net command.

Syntax and Example:

    net signal-name <pin-name> <optional direction> (<pin-name>|<direction>)

    net home-x axis.0.home-sw-in <= parport.0.pin-11-in

In the above example *home-x* is the signal name, *axis.0.home-sw-in* is a *Direction IN* pin, *⇐* is the optional direction indicator, and *parport.0.pin-11-in* is a *Direction OUT* pin. This may seem confusing but the in and out labels for a parallel port pin indicates the physical way the pin works not how it is handled in HAL.

A pin can be connected to a signal if it obeys the following rules:

-   An IN pin can always be connected to a signal

-   An IO pin can be connected unless there’s an OUT pin on the signal

-   An OUT pin can be connected only if there are no other OUT or IO pins on the signal

The same &lt;signal-name&gt; can be used in multiple net commands to connect additional pins, as long as the rules above are obeyed.

![images/signal-direction.png]()

Figure 16. Signal Direction<span id="cap:Signal-Direction"></span>

This example shows the signal xStep with the source being stepgen.0.out and with two readers, parport.0.pin-02-out and parport.0.pin-08-out. Basically the value of stepgen.0.out is sent to the signal xStep and that value is then sent to parport.0.pin-02-out and parport.0.pin-08-out.

    #   signal    source            destination          destination
    net xStep stepgen.0.out => parport.0.pin-02-out parport.0.pin-08-out

Since the signal xStep contains the value of stepgen.0.out (the source) you can use the same signal again to send the value to another reader. To do this just use the signal with the readers on another line.

    net xStep => parport.0.pin-02-out

I/O pins

An I/O pin like encoder.N.index-enable can be read or set as allowed by the component.

#### setp <span id="sub:setp"></span>

The command *setp* sets the value of a pin or parameter. The valid values will depend on the type of the pin or parameter. It is an error if the data types do not match.

Some components have parameters that need to be set before use. Parameters can be set before use or while running as needed. You cannot use setp on a pin that is connected to a signal.

The syntax and an example:

    setp <pin/parameter-name> <value>

    setp parport.0.pin-08-out TRUE

#### sets <span id="sub:sets"></span>

The command *sets* sets the value of a signal.

The syntax and an example:

    sets <signal-name> <value>

    net mysignal and2.0.in0 pyvcp.my-led

    sets mysignal 1

It is an error if:

-   The signal-name does not exist

-   If the signal already has a writer

-   If value is not the correct type for the signal

#### unlinkp

The command *unlinkp* unlinks a pin from the connected signal. If no signal was connected to the pin prior running the command, nothing happens. The *unlinkp* command is useful for trouble shooting.

The syntax and an example:

    unlinkp <pin-name>

    unlinkp parport.0.pin-02-out

#### Obsolete Commands

The following commands are depreciated and may be removed from future versions. Any new configuration should use the [*net*](#sub:net) command. These commands are included so older configurations will still work.

##### linksp

The command *linksp* creates a *connection* between a signal and one pin.

The syntax and an example:

    linksp <signal-name> <pin-name>
    linksp X-step parport.0.pin-02-out

The *linksp* command has been superseded by the *net* command.

##### linkps

The command *linkps* creates a *connection* between one pin and one signal. It is the same as linksp but the arguments are reversed.

The syntax and an example:

    linkps <pin-name> <signal-name>

    linkps parport.0.pin-02-out X-Step

The *linkps* command has been superseded by the *net* command.

##### newsig

the command *newsig* creates a new HAL signal by the name &lt;signame&gt; and the data type of &lt;type&gt;. Type must be *bit*, *s32*, *u32* or *float*. Error if &lt;signame&gt; all ready exists.

The syntax and an example:

    newsig <signame> <type>

    newsig Xstep bit

More information can be found in the HAL manual or the man pages for halrun.

### HAL Data <span id="sec:Hal-Data"></span>

#### Bit

A bit value is an on or off.

-   bit values = true or 1 and false or 0 (True, TRUE, true are all valid)

#### Float

A *float* is a floating point number. In other words the decimal point can move as needed.

-   float values = a 32 bit floating point value, with approximately 24 bits of resolution and over 200 bits of dynamic range.

For more information on floating point numbers see:

<http://en.wikipedia.org/wiki/Floating_point>

#### s32

An *s32* number is a whole number that can have a negative or positive value.

-   s32 values = integer numbers -2147483648 to 2147483647

#### u32

A *u32* number is a whole number that is positive only.

-   u32 values = integer numbers 0 to 4294967295

### HAL Files

If you used the Stepper Config Wizard to generate your config you will have up to three HAL files in your config directory.

-   my-mill.hal (if your config is named *my-mill*) This file is loaded first and should not be changed if you used the Stepper Config Wizard.

-   custom.hal This file is loaded next and before the GUI loads. This is where you put your custom HAL commands that you want loaded before the GUI is loaded.

-   custom\_postgui.hal This file is loaded after the GUI loads. This is where you put your custom HAL commands that you want loaded after the GUI is loaded. Any HAL commands that use pyVCP widgets need to be placed here.

### HAL Components

Two parameters are automatically added to each HAL component when it is created. These parameters allow you to scope the execution time of a component.

`.time`

`.tmax`

Time is the number of CPU cycles it took to execute the function.

Tmax is the maximum number of CPU cycles it took to execute the function. Tmax is a read/write parameter so the user can set it to 0 to get rid of the first time initialization on the function’s execution time.

### Logic Components

HAL contains several real time logic components. Logic components follow a *Truth Table* that states what the output is for any given input. Typically these are bit manipulators and follow electrical logic gate truth tables.

#### and2

The *and2* component is a two input *and* gate. The truth table below shows the output based on each combination of input.

Syntax

    and2 [count=N] | [names=name1[,name2...]]

Functions

and2.n

Pins

    and2.N.in0 (bit, in)
    and2.N.in1 (bit, in)
    and2.N.out (bit, out)

Truth Table

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">in0</th>
<th align="left">in1</th>
<th align="left">out</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
</tr>
<tr class="even">
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
</tr>
<tr class="odd">
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
</tr>
<tr class="even">
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
</tr>
</tbody>
</table>

#### not

The *not* component is a bit inverter.

Syntax

    not [count=n] | [names=name1[,name2...]]

Functions

    not.all
    not.n

Pins

    not.n.in (bit, in)
    not.n.out (bit, out)

Truth Table

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">in</th>
<th align="left">out</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
</tr>
<tr class="even">
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
</tr>
</tbody>
</table>

#### or2

The *or2* component is a two input OR gate.

Syntax

    or2[count=n] | [names=name1[,name2...]]

Functions

`or2.n`

Pins

    or2.n.in0 (bit, in)
    or2.n.in1 (bit, in)
    or2.n.out (bit, out)

Truth Table

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">in0</th>
<th align="left">in1</th>
<th align="left">out</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
</tr>
<tr class="even">
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
</tr>
<tr class="odd">
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
</tr>
<tr class="even">
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
</tr>
</tbody>
</table>

#### xor2

The *xor2* component is a two input XOR (exclusive OR)gate.

Syntax

    xor2[count=n] | [names=name1[,name2...]]

Functions

`xor2.n`

Pins

    xor2.n.in0 (bit, in)
    xor2.n.in1 (bit, in)
    xor2.n.out (bit, out)

Truth Table

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">in0</th>
<th align="left">in1</th>
<th align="left">out</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
</tr>
<tr class="even">
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>False</p></td>
</tr>
<tr class="odd">
<td align="left"><p>False</p></td>
<td align="left"><p>True</p></td>
<td align="left"><p>True</p></td>
</tr>
<tr class="even">
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
<td align="left"><p>False</p></td>
</tr>
</tbody>
</table>

#### Logic Examples

An *and2* example connecting two inputs to one output.

    loadrt and2 count=1

    addf and2.0 servo-thread

    net my-sigin1 and2.0.in0 <= parport.0.pin-11-in

    net my-sigin2 and2.0.in1 <= parport.0.pin-12-in

    net both-on parport.0.pin-14-out <= and2.0.out

In the above example one copy of and2 is loaded into real time space and added to the servo thread. Next pin 11 of the parallel port is connected to the in0 bit of the and gate. Next pin 12 is connected to the in1 bit of the and gate. Last we connect the and2 out bit to the parallel port pin 14. So following the truth table for and2 if pin 11 and pin 12 are on then the output pin 14 will be on.

### Conversion Components

#### weighted\_sum

The weighted\_sum converts a group of bits to an integer. The conversion is the sum of the *weights* of the bits that are on plus any offset. The weight of the m-th bit is 2^m. This is similar to a binary coded decimal but with more options. The *hold* bit stops processing the input changes so the *sum* will not change.

The following syntax is used to load the weighted\_sum component.

    loadrt weighted_sum wsum_sizes=size[,size,...]

Creates weighted sum groups each with the given number of input bits (size).

To update the weighted\_sum you need to attach process\_wsums to a thread.

    addf process_wsums servo-thread

This updates the weighted\_sum component.

In the following example clipped from the HAL Configuration window in Axis the bits *0* and *2* are true and there is no offset. The *weight* of 0 is 1 and the *weight* of 2 is 4 so the sum is 5.

weighted\_sum

    Component Pins:
    Owner   Type  Dir         Value  Name
        10  bit   In           TRUE  wsum.0.bit.0.in
        10  s32   I/O             1  wsum.0.bit.0.weight
        10  bit   In          FALSE  wsum.0.bit.1.in
        10  s32   I/O             2  wsum.0.bit.1.weight
        10  bit   In           TRUE  wsum.0.bit.2.in
        10  s32   I/O             4  wsum.0.bit.2.weight
        10  bit   In          FALSE  wsum.0.bit.3.in
        10  s32   I/O             8  wsum.0.bit.3.weight
        10  bit   In          FALSE  wsum.0.hold
        10  s32   I/O             0  wsum.0.offset
        10  s32   Out             5  wsum.0.sum

Halshow
-------

<span id="cha:halshow"></span>

The script halshow can help you find your way around a running HAL. This is a very specialized system and it must connect to a working HAL. It cannot run standalone because it relies on the ability of HAL to report what it knows of itself through the halcmd interface library. It is discovery based. Each time halshow runs with a different Machinekit configuration it will be different.

As we will soon see, this ability of HAL to document itself is one key to making an effective CNC system.

### Starting Halshow

Halshow is in the AXIS menu under Machine/Show HAL Configuration.

Halshow is in the TkMachinekit menu under Scripts/HAL Show.

### HAL Tree Area

At the left of its display as shown in figure [\[cap:Halshow-Layout\]](#cap:Halshow-Layout) is a tree view, somewhat like you might see with some file browsers. At the right is a tabbed notebook with tabs for show and watch.

![images/halshow-1.png]()

Figure 17. Halshow Layout<span id="cap:Halshow-Layout"></span>

The tree shows all of the major parts of a HAL. In front of each is a small plus (+) or minus (-) sign in a box. Clicking the plus will expand that tree node to display what is under it. If that box shows a minus sign, clicking it will collapse that section of the tree.

You can also expand or collapse the tree display using the Tree View menu at the upper left edge of the display. Under Tree View you will find: Expand Tree, Collapse Tree; Expand Pins, Expand Parameters, Expand Signals; and Erase Watch. (Note that Erase Watch erases *all* previously set watches, you cannot erase just one watch.)

![images/halshow-3.png]()

Figure 18. Show Menu<span id="cap:Show-Menu"></span>

### HAL Show Area

Clicking on the node name, the word "Components" for example, will show you (under the "Show" tab) all that HAL knows about the contents of that node. Figure [\[cap:Halshow-Layout\]](#cap:Halshow-Layout) shows a list exactly like you will see if you click the "Components" name while you are running a standard m5i20 servo card. The information display is exactly like those shown in traditional text based HAL analysis tools. The advantage here is that we have mouse click access, access that can be as broad or as focused as you need.

If we take a closer look at the tree display we can see that the six major parts of a HAL can all be expanded at least one level. As these levels are expanded you can get more focused with the reply when you click on the rightmost tree node. You will find that there are some HAL pins and parameters that show more than one reply. This is due to the nature of the search routines in halcmd itself. If you search one pin you may get two, like this:

    Component Pins:
    Owner  Type  Dir  Value  Name
    06     bit    -W   TRUE  parport.0.pin-10-in
    06     bit    -W  FALSE  parport.0.pin-10-in-not

The second pin’s name contains the complete name of the first.

Below the show area on the right is a set of widgets that will allow you to play with the running HAL. The commands you enter here and the effect that they have on the running HAL are not saved. They will persist as long as Machinekit remains up but are gone as soon as Machinekit is.

The entry box labeled "Test HAL Command:" will accept any of the commands listed for halcmd. These include:

-   loadrt, unloadrt (load/unload real-time module)

-   loadusr, unloadusr (load/unload user-space component)

-   addf, delf (add/delete a function to/from a real-time thread)

-   net (create a connection between two or more items)

-   setp (set parameter (or pin) to a value)

This little editor will enter a command any time you press &lt;enter&gt; or push the execute button. An error message from halcmd will show below this entry widget when these commands are not properly formed. If you are not certain how to set up a proper command you’ll need to read again the documentation on halcmd and the specific modules that you are working with.

Let’s use this editor to add a differential module to a HAL and connect it to axis position so that we could see the rate of change in position, i.e., acceleration. We first need to load a HAL component named blocks, add it to the servo thread, then connect it to the position pin of an axis. Once that is done we can find the output of the differentiator in halscope. So let’s go. (Yes, I looked this one up.)

    loadrt blocks ddt=1

Now look at the components node and you should see blocks in there someplace.

    Loaded HAL Components:
    ID Type        Name
    10 User halcmd29800
    09 User halcmd29374
    08   RT      blocks
    06   RT hal_parport
    05   RT    scope_rt
    04   RT     stepgen
    03   RT      motmod
    02 User   iocontrol

Sure enough there it is. Notice that its ID is 08. Next we need to find out what functions are available with it so we look at functions:

    Exported Functions:
    Owner  CodeAddr      Arg  FP Users Name
      08   E0B97630 E0DC7674 YES     0 ddt.0
      03   E0DEF83C 00000000 YES     1 motion-command-handler
      03   E0DF0BF3 00000000 YES     1 motion-controller
      06   E0B541FE E0DC75B8  NO     1 parport.0.read
      06   E0B54270 E0DC75B8  NO     1 parport.0.write
      06   E0B54309 E0DC75B8  NO     0 parport.read-all
      06   E0B5433A E0DC75B8  NO     0 parport.write-all
      05   E0AD712D 00000000  NO     0 scope.sample
      04   E0B618C1 E0DC7448 YES     1 stepgen.capture-position
      04   E0B612F5 E0DC7448  NO     1 stepgen.make-pulses
      04   E0B614AD E0DC7448 YES     1 stepgen.update-freq

Here we look for owner \#08 and see that blocks has exported a function named ddt.0. We should be able to add ddt.0 to the servo thread and it will do its math each time the servo thread is updated. Once again we look up the addf command and find that it uses three arguments like this:

    addf <functname> <threadname> [<position>]

We already know the functname=ddt.0 so let’s get the thread name right by expanding the thread node in the tree. Here we see two threads, servo-thread and base-thread. The position of ddt.0 in the thread is not critical. So we add the function ddt.0 to the servo-thread:

    addf ddt.0 servo-thread

This is just for viewing, so we leave position blank and get the last position in the thread. Figure [\[cap:Addf-Command\]](#cap:Addf-Command) shows the state of halshow after this command has been issued.

![images/halshow-2.png]()

Figure 19. Addf Command<span id="cap:Addf-Command"></span>

Next we need to connect this block to something. But how do we know what pins are available? The answer is to look under pins. There we find ddt and see this:

    Component Pins:
    Owner Type  Dir Value       Name
    08    float R-  0.00000e+00 ddt.0.in
    08    float -W  0.00000e+00 ddt.0.out

That looks easy enough to understand, but what signal or pin do we want to connect to it? It could be an axis pin, a stepgen pin, or a signal. We see this when we look at axis.0:

    Component Pins:
    Owner Type  Dir Value       Name
    03    float -W  0.00000e+00 axis.0.motor-pos-cmd ==> Xpos-cmd

So it looks like Xpos-cmd should be a good signal to use. Back to the editor where we enter the following command:

    linksp Xpos-cmd ddt.0.in

Now if we look at the Xpos-cmd signal using the tree node we’ll see what we’ve done:

    Signals:
    Type Value Name
    float 0.00000e+00 Xpos-cmd
    <== axis.0.motor-pos-cmd
    ==> ddt.0.in
    ==> stepgen.0.position-cmd

We see that this signal comes from axis.o.motor-pos-cmd and goes to both ddt.0.in and stepgen.0.position-cmd. By connecting our block to the signal we have avoided any complications with the normal flow of this motion command.

The HAL Show Area uses halcmd to discover what is happening in a running HAL. It gives you complete information about what it has discovered. It also updates as you issue commands from the little editor panel to modify that HAL. There are times when you want a different set of things displayed without all of the information available in this area. That is where the HAL Watch Area is of value.

### HAL Watch Area

Clicking the watch tab produces a blank canvas. You can add signals and pins to this canvas and watch their values.<span class="footnote">
\[The refresh rate of the watch display is much lower than Halmeter or Halscope. If you need good resolution of the timing of signals those tools are much more effective.\]
</span> You can add signals or pins when the watch tab is displayed by clicking on the name of it. Figure [\[cap:Watch-Display\]](#cap:Watch-Display) shows this canvas with several "bit" type signals. These signals include enable-out for the first three axes and two of the three iocontrol "estop" signals. Notice that the axes are not enabled even though the estop signals say that Machinekit is not in estop. A quick look at TkMachinekit shows that the condition of Machinekit is ESTOP RESET. The amp enables do not turn true until the machine has been turned on.

![images/halshow-4.png]()

Figure 20. <span id="cap:Watch-Display"></span>Watch Display

Watch displays bit type (binary) values using colored circles representing LEDs. They show as dark brown when a bit signal or pin is false, and as light yellow whenever that signal is true. If you select a pin or signal that is not a bit type (binary) signal, watch will show it as a numerical value.

Watch will quickly allow you to test switches or see the effect of changes that you make to Machinekit while using the graphical interface. Watch’s refresh rate is a bit slow to see stepper pulses, but you can use it for these if you move an axis very slowly or in very small increments of distance. If you’ve used IO\_Show in Machinekit, the watch page in halshow can be set up to watch a parport much as IO\_Show did.

HAL Components
--------------

<span id="cha:HAL-components"></span>

### Commands and Userspace Components<span id="sec:Commands-and-Userspace-Components"></span>

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

### Realtime Components List<span id="sec:Realtime-Components"></span>

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

#### Core Machinekit components<span id="sec:Realtime-Components-core"></span>

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

#### Logic and bitwise components<span id="sec:Realtime-Components-logic"></span>

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

#### Arithmetic and float-components<span id="sec:Realtime-Components-float"></span>

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

#### Type conversion<span id="sec:Realtime-Components-typeconvert"></span>

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

#### Hardware drivers<span id="sec:Realtime-Components-hwdrivers"></span>

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

#### Kinematics<span id="sec:Realtime-Components-kins"></span>

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

#### Motor control<span id="sec:Realtime-Components-motor"></span>

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

#### BLDC and 3-phase motor control<span id="sec:Realtime-Components-bldc"></span>

 bldc\_hall3   
3-wire Bipolar trapezoidal commutation BLDC motor driver using Hall sensors.

 clarke2   
Two input version of Clarke transform.

 clarke3   
Clarke (3 phase to cartesian) transform.

 clarkeinv   
Inverse Clarke transform.

#### Other<span id="sec:Realtime-Components-other"></span>

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

### HAL API calls

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

### RTAPI calls

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

HAL Component Descriptions
--------------------------

<span id="cha:realtime-components"></span>

### Stepgen

This component provides software based generation of step pulses in response to position or velocity commands. In position mode, it has a built in pre-tuned position loop, so PID tuning is not required. In velocity mode, it drives a motor at the commanded speed, while obeying velocity and acceleration limits. It is a realtime component only, and depending on CPU speed, etc, is capable of maximum step rates of 10kHz to perhaps 50kHz. Figure [Step Pulse Generator Block Diagram](#fig:Stepgen-Block-Diag) shows three block diagrams, each is a single step pulse generator. The first diagram is for step type *0*, (step and direction). The second is for step type *1* (up/down, or pseudo-PWM), and the third is for step types 2 through 14 (various stepping patterns). The first two diagrams show position mode control, and the third one shows velocity mode. Control mode and step type are set independently, and any combination can be selected.

Step Pulse Generator Block Diagram position mode

![images/stepgen-block-diag.png]()

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

Parameters<span id="sub:stepgen-parameters"></span>

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

![images/stepgen-type0.png]()

Figure 21. Step and Direction Timing

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

![images/stepgen-type2-4.png]()

Four-Phase Step Types

![images/stepgen-type5-10.png]()

Five-Phase Step Types

![images/stepgen-type11-14.png]()

Functions

The component exports three functions. Each function acts on all of the step pulse generators - running different generators in different threads is not supported.

-   *(funct) stepgen.make-pulses* - High speed function to generate and count pulses (no floating point).

-   *(funct) stepgen.update-freq* - Low speed function does position to velocity conversion, scaling and limiting.

-   *(funct) stepgen.capture-position* - Low speed function for feedback, updates latches and scales position.

The high speed function *stepgen.make-pulses* should be run in a very fast thread, from 10 to 50 us depending on the capabilities of the computer. That thread’s period determines the maximum step frequency, since *steplen*, *stepspace*, *dirsetup*, *dirhold*, and *dirdelay* are all rounded up to a integer multiple of the thread periond in nanoseconds. The other two functions can be called at a much lower rate.

### PWMgen

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

### Encoder

This component provides software based counting of signals from quadrature encoders. It is a realtime component only, and depending on CPU speed, latency, etc, is capable of maximum count rates of 10kHz to perhaps up to 50kHz.

The base thread should be 1/2 count speed to allow for noise and timing variation. For example if you have a 100 pulse per revolution encoder on the spindle and your maximnum RPM is 3000 the maximum base thread should be 25 us. A 100 pulse per revolution encoder will have 400 counts. The spindle speed of 3000 RPM = 50 RPS (revolutions per second). 400 \* 50 = 20,000 counts per second or 50 us between counts.

Figure [Encoder Counter Block Diagram](#fig:Encoder-Block-Diag) is a block diagram of one channel of encoder counter.

Encoder Counter Block Diagram

![images/encoder-block-diag.png]()

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

### PID

This component provides Proportional/Integral/Derivative control loops. It is a realtime component only. For simplicity, this discussion assumes that we are talking about position loops, however this component can be used to implement other feedback loops such as speed, torch height, temperature, etc. Figure [PID Loop Block Diagram](#fig:PID-block-diag) is a block diagram of a single PID loop.

PID Loop Block Diagram

![images/pid-block-diag.png]()

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

### Simulated Encoder<span id="sec:Simulated-Encoder"></span>

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

### Debounce

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

### Siggen<span id="sec:Siggen"></span>

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

### lut5

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

Parallel Port Driver
--------------------

<span id="cha:Parport"></span>

### Parport

Parport is a driver for the traditional PC parallel port. The port has a total of 17 physical pins. The original parallel port divided those pins into three groups: data, control, and status. The data group consists of 8 output pins, the control group consists of 4 pins, and the status group consists of 5 input pins.

In the early 1990’s, the bidirectional parallel port was introduced, which allows the data group to be used for output or input. The HAL driver supports the bidirectional port, and allows the user to set the data group as either input or output. If configured as output, a port provides a total of 12 outputs and 5 inputs. If configured as input, it provides 4 outputs and 13 inputs.

In some parallel ports, the control group pins are open collectors, which may also be driven low by an external gate. On a board with open collector control pins, the *x* mode allows a more flexible mode with 8 outputs, and 9 inputs. In other parallel ports, the control group has push-pull drivers and cannot be used as an input.

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
<td align="left"><div class="title">
HAL and Open Collectors
</div>
<div class="paragraph">
<p>HAL cannot automatically determine if the <em>x</em> mode bidirectional pins are actually open collectors (OC). If they are not, they cannot be used as inputs, and attempting to drive them LOW from an external source can damage the hardware.</p>
</div>
<div class="paragraph">
<p>To determine whether your port has <em>open collector</em> pins, load hal_parport in <em>x</em> mode. With no device attached, HAL should read the pin as TRUE. Next, insert a 470 ohm resistor from one of the control pins to GND. If the resulting voltage on the control pin is close to 0V, and HAL now reads the pin as FALSE, then you have an OC port. If the resulting voltage is far from 0V, or HAL does not read the pin as FALSE, then your port cannot be used in <em>x</em> mode.</p>
</div>
<div class="paragraph">
<p>The external hardware that drives the control pins should also use open collector gates (e.g., 74LS05).</p>
</div>
<div class="paragraph">
<p>On some machines, BIOS settings may affect whether <em>x</em> mode can be used. <em>SPP</em> mode is most likely to work.</p>
</div></td>
</tr>
</tbody>
</table>

No other combinations are supported, and a port cannot be changed from input to output once the driver is installed. The [Parport Block Diagram](#fig:Parport-block-diag) shows two block diagrams, one showing the driver when the data group is configured for output, and one showing it configured for input. For *x* mode, refer to the pin listing of *halcmd show pin* for pin direction assignment.

The parport driver can control up to 8 ports (defined by MAX\_PORTS in hal\_parport.c). The ports are numbered starting at zero.

#### Installing

    loadrt hal_parport cfg="<config-string>"

Using the Port Index

I/O addresses below 16 are treated as port indexes. This is the simplest way to install the parport driver and cooperates with the Linux parport\_pc driver if it is loaded. This will use the address Linux has detected for parport 0.

    loadrt hal_parport cfg="0"

Using the Port Address

The configure string consists of a hex port address, followed by an optional direction, repeated for each port. The direction is *in*, *out*, or *x* and determines the direction of the physical pins 2 through 9, and whether to create input HAL pins for the physical control pins. If the direction is not specified, the data group defaults to output. For example:

    loadrt hal_parport cfg="0x278 0x378 in 0x20A0 out"

This example installs drivers for one port at 0x0278, with pins 2-9 as outputs (by default, since neither *in* nor *out* was specified), one at 0x0378, with pins 2-9 as inputs, and one at 0x20A0, with pins 2-9 explicitly specified as outputs. Note that you must know the base address of the parallel port to properly configure the driver. For ISA bus ports, this is usually not a problem, since the port is almost always at a *well known* address, like 0278 or 0378 which is typically configured in the system BIOS. The address for a PCI card is usually shown in *lspci -v* in an *I/O ports* line, or in the kernel message log after executing *sudo modprobe -a parport\_pc*. There is no default address; if *&lt;config-string&gt;* does not contain at least one address, it is an error.

![images/parport-block-diag.png]()

Figure 22. Parport Block Diagram

#### Pins

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out* (bit) Drives a physical output pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-in* (bit) Tracks a physical input pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-in-not* (bit) Tracks a physical input pin, but inverted.

For each pin, *&lt;p&gt;* is the port number, and *&lt;n&gt;* is the physical pin number in the 25 pin D-shell connector.

For each physical output pin, the driver creates a single HAL pin, for example: *parport.0.pin-14-out*.

Pins 2 through 9 are part of the data group and are output pins if the port is defined as an output port. (Output is the default.) Pins 1, 14, 16, and 17 are outputs in all modes. These HAL pins control the state of the corresponding physical pins.

For each physical input pin, the driver creates two HAL pins, for example: *parport.0.pin-12-in* and *parport.0.pin-12-in-not*.

Pins 10, 11, 12, 13, and 15 are always input pins. Pins 2 through 9 are input pins only if the port is defined as an input port. The *-in* HAL pin is TRUE if the physical pin is high, and FALSE if the physical pin is low. The *-in-not* HAL pin is inverted — it is FALSE if the physical pin is high. By connecting a signal to one or the other, the user can determine the state of the input. In *x* mode, pins 1, 14, 16, and 17 are also input pins.

#### Parameters

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out-invert* (bit) Inverts an output pin.

-   *parport.&lt;p&gt;.pin-&lt;n&gt;-out-reset* (bit) (only for *out* pins) TRUE if this pin should be reset when the *-reset* function is executed.

-   parport.&lt;p&gt;.reset-time' (U32) The time (in nanoseconds) between a pin is set by *write* and reset by the *reset* function if it is enabled.

The *-invert* parameter determines whether an output pin is active high or active low. If *-invert* is FALSE, setting the HAL *-out* pin TRUE drives the physical pin high, and FALSE drives it low. If *-invert* is TRUE, then setting the HAL *-out* pin TRUE will drive the physical pin low.

#### Functions<span id="sub:parport-functions"></span>

-   *parport.&lt;p&gt;.read* (funct) Reads physical input pins of port *&lt;portnum&gt;* and updates HAL *-in* and *-in-not* pins.

-   *parport.read-all* (funct) Reads physical input pins of all ports and updates HAL *-in* and *-in-not* pins.

-   *parport.&lt;p&gt;.write* (funct) Reads HAL *-out* pins of port *&lt;p&gt;* and updates that port’s physical output pins.

-   *parport.write-all* (funct) Reads HAL *-out* pins of all ports and updates all physical output pins.

-   *parport.&lt;p&gt;.reset* (funct) Waits until *reset-time* has elapsed since the associated *write*, then resets pins to values indicated by *-out-invert* and *-out-invert* settings. *reset* must be later in the same thread as *write. 'If '-reset* is TRUE, then the *reset* function will set the pin to the value of *-out-invert*. This can be used in conjunction with stepgen’s *doublefreq* to produce one step per period. The [stepgen stepspace](#sub:stepgen-parameters) for that pin must be set to 0 to enable doublefreq.

The individual functions are provided for situations where one port needs to be updated in a very fast thread, but other ports can be updated in a slower thread to save CPU time. It is probably not a good idea to use both an *-all* function and an individual function at the same time.

#### Common problems

If loading the module reports

    insmod: error inserting '/home/jepler/emc2/rtlib/hal_parport.ko':
    -1 Device or resource busy

then ensure that the standard kernel module *parport\_pc* is not loaded<span class="footnote">
\[In the Machinekit packages for Debian, the file /etc/modprobe.d/emc2 generally prevents *parport\_pc* from being automatically loaded.\]
</span> and that no other device in the system has claimed the I/O ports.

If the module loads but does not appear to function, then the port address is incorrect or the *probe\_parport* module is required.

#### Using DoubleStep

To setup DoubleStep on the parallel port you must add the function parport.n.reset after parport.n.write and configure stepspace to 0 and the reset time wanted. So that step can be asserted on every period in HAL and then toggled off by parport after being asserted for time specificed by parport.n.reset-time.

For example:

    loadrt hal_parport cfg="0x378 out"
    setp parport.0.reset-time 5000
    loadrt stepgen step_type=0,0,0
    addf parport.0.read base-thread
    addf stepgen.make-pulses base-thread
    addf parport.0.write base-thread
    addf parport.0.reset base-thread
    addf stepgen.capture-position servo-thread
    ...
    setp stepgen.0.steplen 1
    setp stepgen.0.stepspace 0

More information on DoubleStep can be found on the [wiki](http://wiki.machinekit.org/cgi-bin/wiki.pl?TweakingSoftwareStepGeneration).

### probe\_parport

In modern PCs, the parallel port may require plug and play (PNP) configuration before it can be used. The *probe\_parport* module performs configuration of any PNP ports present, and should be loaded before *hal\_parport*. On machines without PNP ports, it may be loaded but has no effect.

#### Installing

    loadrt probe_parport

    loadrt hal_parport ...

If the Linux kernel prints a message similar to

    parport: PnPBIOS parport detected.

when the parport\_pc module is loaded (*sudo modprobe -a parport\_pc; sudo rmmod parport\_pc)* then use of this module is probably required.

HAL Examples
------------

All of these examples assume you are starting with a stepconf based configuration and have two threads base-thread and servo-thread. The stepconf wizard will create an empty custom.hal and a custom\_postgui.hal file. The custom.hal file will be loaded after the configuration HAL file and the custom\_postgui.hal file is loaded after the GUI has been loaded.

### Manual Toolchange

In this example it is assumed that you’re *rolling your own* configuration and wish to add the HAL Manual Toolchange window. The HAL Manual Toolchange is primarily useful if you have presettable tools and you store the offsets in the tool table. If you need to touch off for each tool change then it is best just to split up your g code. To use the HAL Manual Toolchange window you basically have to load the hal\_manualtoolchange component then send the iocontrol *tool change* to the hal\_manualtoolchange *change* and send the hal\_manualtoolchange *changed* back to the iocontrol *tool changed*. A pin is provided for an external input to indicate the tool change is complete.

This is an example of manual toolchange *with* the HAL Manual Toolchange component:

    loadusr -W hal_manualtoolchange
    net tool-change iocontrol.0.tool-change => hal_manualtoolchange.change
    net tool-changed iocontrol.0.tool-changed <= hal_manualtoolchange.changed
    net external-tool-changed hal_manualtoolchange.change_button <= parport.0.pin-12-in
    net tool-number iocontrol.0.tool-prep-number => hal_manualtoolchange.number
    net tool-prepare-loopback iocontrol.0.tool-prepare => iocontrol.0.tool-prepared

This is an example of manual toolchange *without* the HAL Manual Toolchange component:

    net tool-number <= iocontrol.0.tool-prep-number
    net tool-change-loopback iocontrol.0.tool.-change => iocontrol.0.tool-changed
    net tool-prepare-loopback iocontrol.0.tool-prepare => iocontrol.0.tool-prepared

### Compute Velocity

This example uses *ddt*, *mult2* and *abs* to compute the velocity of a single axis. For more information on the real time components see the man pages or the Realtime Components section ([\[sec:Realtime-Components\]](#sec:Realtime-Components)).

The first thing is to check your configuration to make sure you are not using any of the real time components all ready. You can do this by opening up the HAL Configuration window and look for the components in the pin section. If you are then find the .hal file that they are being loaded in and increase the counts and adjust the instance to the correct value. Add the following to your custom.hal file.

Load the realtime components.

    loadrt ddt count=1
    loadrt mult2 count=1
    loadrt abs count=1

Add the functions to a thread so it will get updated.

    addf ddt.0 servo-thread
    addf mult2.0 servo-thread
    addf abs.0 servo-thread

Make the connections.

    setp mult2.in1 60
    net xpos-cmd ddt.0.in
    net X-IPS mult2.0.in0 <= ddt.0.out
    net X-ABS abs.0.in <= mult2.0.out
    net X-IPM abs.0.out

In this last section we are setting the mult2.0.in1 to 60 to convert the inch per second to inch per minute that we get from the ddt.0.out.

The xpos-cmd sends the commanded position to the ddt.0.in. The ddt computes the derivative of the change of the input.

The ddt2.0.out is multiplied by 60 to give IPM.

The mult2.0.out is sent to the abs to get the absolute value.

The following figure shows the result when the X axis is moving at 15 IPM in the minus direction. Notice that we can get the absolute value from either the abs.0.out pin or the X-IPM signal.

![images/velocity-01.png]()

Figure 23. Velocity Example<span id="cap:Velocity-Example"></span>

### Soft Start

This example shows how the HAL components *lowpass*, *limit2* or *limit3* can be used to limit how fast a signal changes.

In this example we have a servo motor driving a lathe spindle. If we just used the commanded spindle speeds on the servo it will try to go from present speed to commanded speed as fast as it can. This could cause a problem or damage the drive. To slow the rate of change we can send the motion.spindle-speed-out through a limiter before the PID, so that the PID command value changes to new settings more slowly.

Three built-in components that limit a signal are:

-   *limit2* limits the range and first derivative of a signal.

-   *limit3* limits the range, first and second derivatives of a signal.

-   *lowpass* uses an exponentially-weighted moving average to track an input signal.

To find more information on these HAL components check the man pages.

Place the following in a text file called softstart.hal. If you’re not familiar with Linux place the file in your home directory.

    loadrt threads period1=1000000 name1=thread
    loadrt siggen
    loadrt lowpass
    loadrt limit2
    loadrt limit3
    net square siggen.0.square => lowpass.0.in limit2.0.in limit3.0.in
    net lowpass <= lowpass.0.out
    net limit2 <= limit2.0.out
    net limit3 <= limit3.0.out
    setp siggen.0.frequency .1
    setp lowpass.0.gain .01
    setp limit2.0.maxv 2
    setp limit3.0.maxv 2
    setp limit3.0.maxa 10
    addf siggen.0.update thread
    addf lowpass.0 thread
    addf limit2.0 thread
    addf limit3.0 thread
    start
    loadusr halscope

Open a terminal window and run the file with the following command.

    halrun -I softstart.hal

When the HAL Oscilloscope first starts up click *OK* to accept the default thread.

Next you have to add the signals to the channels. Click on channel 1 then select *square* from the Signals tab. Repeat for channels 2-4 and add lowpass, limit2, and limit3.

Next to set up a trigger signal click on the Source None button and select square. The button will change to Source Chan 1.

Next click on Single in the Run Mode radio buttons box. This will start a run and when it finishes you will see your traces.

To separate the signals so you can see them better click on a channel then use the Pos slider in the Vertical box to set the positions.

![images/softstart-scope.png]()

Figure 24. Softstart<span id="cap:Softstart"></span>

To see the effect of changing the set point values of any of the components you can change them in the terminal window. To see what different gain settings do for lowpass just type the following in the terminal window and try different settings.

    setp lowpass.0.gain *.01

After changing a setting run the oscilloscope again to see the change.

When you’re finished type *exit* in the terminal window to shut down halrun and close the halscope. Don’t close the terminal window with halrun running as it might leave some things in memory that could prevent EMC from loading.

For more information on Halscope see the HAL manual.

### Stand Alone HAL

In some cases you might want to run a GladeVCP screen with just HAL. For example say you had a stepper driven device that all you need is to run a stepper motor. A simple *Start/Stop* interface is all you need for your application so no need to load up and configure a full blown CNC application.

In the following example we have created a simple GladeVCP panel with one

Basic Syntax

    # load the winder.glade GUI and name it winder
    loadusr -Wn winder gladevcp -c winder -u handler.py winder.glade

    # load realtime components
    loadrt threads name1=fast period1=50000 fp1=0 name2=slow period2=1000000
    loadrt stepgen step_type=0 ctrl_type=v
    loadrt hal_parport cfg="0x378 out"

    # add functions to threads
    addf stepgen.make-pulses fast
    addf stepgen.update-freq slow
    addf stepgen.capture-position slow
    addf parport.0.read fast
    addf parport.0.write fast

    # make hal connections
    net winder-step parport.0.pin-02-out <= stepgen.0.step
    net winder-dir parport.0.pin-03-out <= stepgen.0.dir
    net run-stepgen stepgen.0.enable <= winder.start_button



    # start the threads
    start

    # comment out the following lines while testing and use the interactive
    # option halrun -I -f start.hal to be able to show pins etc.

    # wait until the gladevcp GUI named winder terminates
    waitusr winder

    # stop HAL threads
    stop

    # unload HAL all components before exiting
    unloadrt all

Comp HAL Component Generator
----------------------------

<span id="cha:comp-hal-component-generator"></span>

### Introduction

Writing a HAL component can be a tedious process, most of it in setup calls to *rtapi\_* and *hal\_* functions and associated error checking. *comp* will write all this code for you, automatically.

Compiling a HAL component is also much easier when using *comp*, whether the component is part of the Machinekit source tree, or outside it.

For instance, when coded in C, a simple component such as "ddt" is around 80 lines of code. The equivalent component is very short when written using the *comp* preprocessor:

Simple Comp Example <span id="code:simple-comp-example"></span>

    component ddt "Compute the derivative of the input function";
    pin in float in;
    pin out float out;
    variable float old;
    function _;
    license "GPL"; // indicates GPL v2 or later
    ;;
    float tmp = in;
    out = (tmp - old) / fperiod;
    old = tmp;

### Installing

If you’re working with an installed version of Machinekit you will need to install the development packages.

One method is to say following line in a terminal.

Installing Dev

    sudo apt-get install machinekit-dev

Another method is to use the Synaptic Package Manager from the main Debian menu to install machinekit-dev.

### Definitions

-   *component* - A component is a single real-time module, which is loaded with *halcmd loadrt*. One *.comp* file specifies one component.

-   *instance* - A component can have zero or more instances. Each instance of a component is created equal (they all have the same pins, parameters, functions, and data) but behave independently when their pins, parameters, and data have different values.

-   *singleton* - It is possible for a component to be a "singleton", in which case exactly one instance is created. It seldom makes sense to write a *singleton* component, unless there can literally only be a single object of that kind in the system (for instance, a component whose purpose is to provide a pin with the current UNIX time, or a hardware driver for the internal PC speaker)

### Instance creation

For a singleton, the one instance is created when the component is loaded.

For a non-singleton, the *count* module parameter determines how many numbered instances are created. If not specified, the *name* module parameter determines how many named instances are created. Otherwise, a single numbered instance is created.

### Implicit Parameters

Functions are implicitly passed the *period* parameter which is the time in nanoseconds of the last period to execute the comp. Functions which use floating-point can also refer to *fperiod* which is the floating-point time in seconds, or (period\*1e-9). This can be useful in comps that need the timing information.

### Syntax

A *.comp* file consists of a number of declarations, followed by *;;* on a line of its own, followed by C code implementing the module’s functions.

Declarations include:

-   *component HALNAME (DOC);*

-   *pin PINDIRECTION TYPE HALNAME (\[SIZE\]|\[MAXSIZE: CONDSIZE\]) (if CONDITION) (= STARTVALUE) (DOC) ;*

-   *param PARAMDIRECTION TYPE HALNAME (\[SIZE\]|\[MAXSIZE: CONDSIZE\]) (if CONDITION) (= STARTVALUE) (DOC) ;*

-   *function HALNAME (fp | nofp) (DOC);*

-   *option OPT (VALUE);*

-   *variable CTYPE STARREDNAME (\[SIZE\]);*

-   *description DOC;*

-   *see\_also DOC;*

-   *license LICENSE;*

-   *author AUTHOR;*

Parentheses indicate optional items. A vertical bar indicates alternatives. Words in *CAPITALS* indicate variable text, as follows:

-   *NAME* - A standard C identifier

-   *STARREDNAME* - A C identifier with zero or more \* before it. This syntax can be used to declare instance variables that are pointers. Note that because of the grammar, there may not be whitespace between the \* and the variable name.

-   *HALNAME* - An extended identifier. When used to create a HAL identifier, any underscores are replaced with dashes, and any trailing dash or period is removed, so that "this\_name\_" will be turned into "this-name", and if the name is "\_", then a trailing period is removed as well, so that "function \_" gives a HAL function name like "component.&lt;num&gt;" instead of "component.&lt;num&gt;."

    If present, the prefix *hal\_* is removed from the beginning of the component name when creating pins, parameters and functions.

    In the HAL identifier for a pin or parameter, \# denotes an array item, and must be used in conjunction with a *\[SIZE\]* declaration. The hash marks are replaced with a 0-padded number with the same length as the number of \# characters.

    When used to create a C identifier, the following changes are applied to the HALNAME:

    1.  Any "\#" characters, and any ".", "\_" or "-" characters immediately before them, are removed.

    2.  Any remaining "." and "-" characters are replaced with "\_".

    3.  Repeated "\_" characters are changed to a single "\\\_" character.

    A trailing "\_" is retained, so that HAL identifiers which would otherwise collide with reserved names or keywords (e.g., *min*) can be used.

    <table>
    <colgroup>
    <col width="33%" />
    <col width="33%" />
    <col width="33%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">HALNAME</th>
    <th align="left">C Identifier</th>
    <th align="left">HAL Identifier</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>x_y_z</p></td>
    <td align="left"><p>x_y_z</p></td>
    <td align="left"><p>x-y-z</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>x-y.z</p></td>
    <td align="left"><p>x_y_z</p></td>
    <td align="left"><p>x-y.z</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>x_y_z_</p></td>
    <td align="left"><p>x_y_z_</p></td>
    <td align="left"><p>x-y-z</p></td>
    </tr>
    <tr class="even">
    <td align="left"><p>x.##.y</p></td>
    <td align="left"><p>x_y(MM)</p></td>
    <td align="left"><p>x.MM.z</p></td>
    </tr>
    <tr class="odd">
    <td align="left"><p>x.##</p></td>
    <td align="left"><p>x(MM)</p></td>
    <td align="left"><p>x.MM</p></td>
    </tr>
    </tbody>
    </table>

-   *if CONDITION* - An expression involving the variable *personality* which is nonzero when the pin or parameter should be created

-   *SIZE* - A number that gives the size of an array. The array items are numbered from 0 to *SIZE*-1.

-   *MAXSIZE : CONDSIZE* - A number that gives the maximum size of the array followed by an expression involving the variable *personality* and which always evaluates to less than *MAXSIZE*. When the array is created its size will be *CONDSIZE*.

-   *DOC* - A string that documents the item. String can be a C-style "double quoted" string, like:

        "Selects the desired edge: TRUE means falling, FALSE means rising"

    or a Python-style "triple quoted" string, which may include embedded newlines and quote characters, such as:

        """The effect of this parameter, also known as "the orb of zot",
        will require at least two paragraphs to explain.

        Hopefully these paragraphs have allowed you to understand "zot"
        better."""

    The documentation string is in "groff -man" format. For more information on this markup format, see *groff\_man(7)*. Remember that comp interprets backslash escapes in strings, so for instance to set the italic font for the word *example*, write:

        "\\fIexample\\fB"

-   *TYPE* - One of the HAL types: *bit*, *signed*, *unsigned*, or *float*. The old names *s32* and *u32* may also be used, but *signed* and *unsigned* are preferred.

-   *PINDIRECTION* - One of the following: *in*, *out*, or *io*. A component sets a value for an *out* pin, it reads a value from an *in* pin, and it may read or set the value of an *io* pin.

-   *PARAMDIRECTION* - One of the following: *r* or *rw*. A component sets a value for a *r* parameter, and it may read or set the value of a *rw* parameter.

-   *STARTVALUE* - Specifies the initial value of a pin or parameter. If it is not specified, then the default is *0* or *FALSE*, depending on the type of the item.

#### HAL functions

-   *fp* - Indicates that the function performs floating-point calculations.

-   *nofp* - Indicates that it only performs integer calculations. If neither is specified, *fp* is assumed. Neither comp nor gcc can detect the use of floating-point calculations in functions that are tagged *nofp*, but use of such operations results in undefined behavior.

#### Options

The currently defined options are:

-   *option singleton yes* - (default: no) Do not create a *count* module parameter, and always create a single instance. With *singleton*, items are named *component-name.item-name* and without *singleton*, items for numbered instances are named *component-name.&lt;num&gt;.item-name*.

-   *option default\_count number* - (default: 1) Normally, the module parameter *count* defaults to 1. If specified, the *count* will default to this value instead.

-   *option count\_function yes* - (default: no) Normally, the number of instances to create is specified in the module parameter *count*; if *count\_function* is specified, the value returned by the function *int get\_count(void)* is used instead, and the *count* module parameter is not defined.

-   *option rtapi\_app no* - (default: yes) Normally, the functions *rtapi\_app\_main* and *rtapi\_app\_exit* are automatically defined. With *option rtapi\_app no*, they are not, and must be provided in the C code. When implementing your own *rtapi\_app\_main*, call the function *int export(char \*prefix, long extra\_arg)* to register the pins, parameters, and functions for *prefix*.

-   *option data TYPE* - (default: none) **deprecated** If specified, each instance of the component will have an associated data block of type *TYPE* (which can be a simple type like *float* or the name of a type created with *typedef*). In new components, *variable* should be used instead.

-   *option extra\_setup yes* - (default: no) If specified, call the function defined by *EXTRA\_SETUP* for each instance. If using the automatically defined *rtapi\_app\_main*, *extra\_arg* is the number of this instance.

-   *option extra\_cleanup yes* - (default: no) If specified, call the function defined by *EXTRA\_CLEANUP* from the automatically defined *rtapi\_app\_exit*, or if an error is detected in the automatically defined *rtapi\_app\_main*.

-   *option userspace yes* - (default: no) If specified, this file describes a userspace component, rather than a real one. A userspace component may not have functions defined by the *function* directive. Instead, after all the instances are constructed, the C function *user\_mainloop()* is called. When this function returns, the component exits. Typically, *user\_mainloop()* will use *FOR\_ALL\_INSTS()* to perform the update action for each instance, then sleep for a short time. Another common action in *user\_mainloop()* may be to call the event handler loop of a GUI toolkit.

-   *option userinit yes* - (default: no) This option is ignored if the option *userspace* (see above) is set to *no*. If *userinit* is specified, the function *userinit(argc,argv)* is called before *rtapi\_app\_main()* (and thus before the call to *hal\_init()* ). This function may process the commandline arguments or take other actions. Its return type is *void*; it may call *exit()* if it wishes to terminate rather than create a HAL component (for instance, because the commandline arguments were invalid).

If an option’s VALUE is not specified, then it is equivalent to specifying *option … yes*. The result of assigning an inappropriate value to an option is undefined. The result of using any other option is undefined.

#### License and Authorship

-   *LICENSE* - Specify the license of the module for the documentation and for the MODULE\_LICENSE() module declaration. For example, to specify that the module’s license is GPL v2 or later,

        license "GPL"; // indicates GPL v2 or later

    For additional information on the meaning of MODULE\_LICENSE() and additional license identifiers, see *&lt;linux/module.h&gt;*. or the manual page *rtapi\_module\_param(3)*

    This declaration is required.

-   *AUTHOR* - Specify the author of the module for the documentation.

#### Per-instance data storage

-   *variable CTYPE STARREDNAME;*

-   *variable CTYPE STARREDNAME\[SIZE\];*

-   *variable CTYPE STARREDNAME = DEFAULT;*

-   *variable CTYPE STARREDNAME\[SIZE\] = DEFAULT;*

    Declare a per-instance variable *STARREDNAME* of type *CTYPE*, optionally as an array of *SIZE* items, and optionally with a default value *DEFAULT*. Items with no *DEFAULT* are initialized to all-bits-zero. *CTYPE* is a simple one-word C type, such as *float*, *u32*, *s32*, int, etc. Access to array variables uses square brackets.

    If a variable is to be of a pointer type, there may not be any space between the "\*" and the variable name. Therefore, the following is acceptable:

        variable int *example;

    but the following are not:

        variable int* badexample;
        variable int * badexample;

#### Comments

C++-style one-line comments (//… ) and

C-style multi-line comments (/\* … \*/) are both supported in the declaration section.

### Restrictions

Though HAL permits a pin, a parameter, and a function to have the same name, comp does not.

Variable and function names that can not be used or are likely to cause problems include:

-   Anything beginning with **\_comp**.

-   *comp\_id*

-   *fperiod*

-   *rtapi\_app\_main*

-   *rtapi\_app\_exit*

-   *extra\_setup*

-   *extra\_cleanup*

### Convenience Macros

Based on the items in the declaration section, *comp* creates a C structure called *struct state*. However, instead of referring to the members of this structure (e.g., *\*(inst→name)* ), they will generally be referred to using the macros below. The details of *struct state* and these macros may change from one version of *comp* to the next.

-   *FUNCTION(name)* - Use this macro to begin the definition of a realtime function which was previously declared with *function NAME*. The function includes a parameter *period* which is the integer number of nanoseconds between calls to the function.

-   *EXTRA\_SETUP()* - Use this macro to begin the definition of the function called to perform extra setup of this instance. Return a negative Unix *errno* value to indicate failure (e.g., *return -EBUSY* on failure to reserve an I/O port), or 0 to indicate success.

-   *EXTRA\_CLEANUP()* - Use this macro to begin the definition of the function called to perform extra cleanup of the component. Note that this function must clean up all instances of the component, not just one. The "pin\_name", "parameter\_name", and "data" macros may not be used here.

-   *pin\_name* or *parameter\_name* - For each pin *pin\_name* or param *parameter\_name* there is a macro which allows the name to be used on its own to refer to the pin or parameter. When *pin\_name* or *parameter\_name* is an array, the macro is of the form *pin\_name(idx)* or *param\_name(idx)* where *idx* is the index into the pin array. When the array is a variable-sized array, it is only legal to refer to items up to its *condsize*.

    When the item is a conditional item, it is only legal to refer to it when its *condition* evaluated to a nonzero value.

-   *variable\_name* - For each variable *variable\_name* there is a macro which allows the name to be used on its own to refer to the variable. When *variable\_name* is an array, the normal C-style subscript is used: *variable\_name\[idx\]*

-   *data* - If "option data" is specified, this macro allows access to the instance data.

-   *fperiod* - The floating-point number of seconds between calls to this realtime function.

-   *FOR\_ALL\_INSTS() {…}* - For userspace components. This macro uses the variable *struct state 'inst* to iterate over all the defined instances. Inside the body of the loop, the *pin\_name*, *parameter\_name*, and *data* macros work as they do in realtime functions.

### Components with one function

If a component has only one function and the string "FUNCTION" does not appear anywhere after *;;*, then the portion after *;;* is all taken to be the body of the component’s single function. See the [Simple Comp](#code:simple-comp-example) for and example of this.

### Component Personality

If a component has any pins or parameters with an "if condition" or "\[maxsize : condsize\]", it is called a component with *personality*. The *personality* of each instance is specified when the module is loaded. *Personality* can be used to create pins only when needed. For instance, personality is used in the *logic* component, to allow for a variable number of input pins to each logic gate and to allow for a selection of any of the basic boolean logic functions *and*, *or*, and *xor*.

### Compiling

Place the *.comp* file in the source directory *machinekit/src/hal/components* and re-run *make*. *Comp* files are automatically detected by the build system.

If a *.comp* file is a driver for hardware, it may be placed in *machinekit/src/hal/components* and will be built unless Machinekit is configured as a userspace simulator.

### Compiling realtime components outside the source tree

*comp* can process, compile, and install a realtime component in a single step, placing *rtexample.ko* in the Machinekit realtime module directory:

    comp --install rtexample.comp

Or, it can process and compile in one step, leaving *example.ko* (or *example.so* for the simulator) in the current directory:

    comp --compile rtexample.comp

Or it can simply process, leaving *example.c* in the current directory:

    comp rtexample.comp

*comp* can also compile and install a component written in C, using the *--install* and *--compile* options shown above:

    comp --install rtexample2.c

man-format documentation can also be created from the information in the declaration section:

    comp --document rtexample.comp

The resulting manpage, *example.9* can be viewed with

    man ./example.9

or copied to a standard location for manual pages.

### Compiling userspace components outside the source tree

*comp* can process, compile, install, and document userspace components:

    comp usrexample.comp
    comp --compile usrexample.comp
    comp --install usrexample.comp
    comp --document usrexample.comp

This only works for *.comp* files, not for *.c* files.

### Examples

#### constant

Note that the declaration "function \_" creates functions named "constant.0", etc.

    component constant;
    pin out float out;
    param r float value = 1.0;
    function _;
    license "GPL"; // indicates GPL v2 or later
    ;;
    FUNCTION(_) { out = value; }

#### sincos

This component computes the sine and cosine of an input angle in radians. It has different capabilities than the "sine" and "cosine" outputs of siggen, because the input is an angle, rather than running freely based on a "frequency" parameter.

The pins are declared with the names *sin\_* and *cos\_* in the source code so that they do not interfere with the functions *sin()* and *cos()*. The HAL pins are still called *sincos.&lt;num&gt;.sin*.

    component sincos;
    pin out float sin_;
    pin out float cos_;
    pin in float theta;
    function _;
    license "GPL"; // indicates GPL v2 or later
    ;;
    #include <rtapi_math.h>
    FUNCTION(_) { sin_ = sin(theta); cos_ = cos(theta); }

#### out8

This component is a driver for a *fictional* card called "out8", which has 8 pins of digital output which are treated as a single 8-bit value. There can be a varying number of such cards in the system, and they can be at various addresses. The pin is called *out\_* because *out* is an identifier used in *&lt;asm/io.h&gt;*. It illustrates the use of *EXTRA\_SETUP* and *EXTRA\_CLEANUP* to request an I/O region and then free it in case of error or when the module is unloaded.

    component out8;
    pin out unsigned out_ "Output value; only low 8 bits are used";
    param r unsigned ioaddr;

    function _;

    option count_function;
    option extra_setup;
    option extra_cleanup;
    option constructable no;

    license "GPL"; // indicates GPL v2 or later
    ;;
    #include <asm/io.h>

    #define MAX 8
    int io[MAX] = {0,};
    RTAPI_MP_ARRAY_INT(io, MAX, "I/O addresses of out8 boards");

    int get_count(void) {
        int i = 0;
        for(i=0; i<MAX && io[i]; i++) { /* Nothing */ }
        return i;
    }

    EXTRA_SETUP() {
        if(!rtapi_request_region(io[extra_arg], 1, "out8")) {
            // set this I/O port to 0 so that EXTRA_CLEANUP does not release the IO
            // ports that were never requested.
            io[extra_arg] = 0;
            return -EBUSY;
        }
        ioaddr = io[extra_arg];
        return 0; }

    EXTRA_CLEANUP() {
        int i;
        for(i=0; i < MAX && io[i]; i++) {
            rtapi_release_region(io[i], 1);
        }
    }

    FUNCTION(_) { outb(out_, ioaddr); }

#### hal\_loop

    component hal_loop;
    pin out float example;

This fragment of a component illustrates the use of the *hal\_* prefix in a component name. *loop* is the name of a standard Linux kernel module, so a *loop* component might not successfully load if the Linux *loop* module was also present on the system.

When loaded, *halcmd show comp* will show a component called *hal\_loop*. However, the pin shown by *halcmd show pin* will be *loop.0.example*, not *hal-loop.0.example*.

#### arraydemo

This realtime component illustrates use of fixed-size arrays:

    component arraydemo "4-bit Shift register";
    pin in bit in;
    pin out bit out-# [4];
    function _ nofp;
    license "GPL"; // indicates GPL v2 or later
    ;;
    int i;
    for(i=3; i>0; i--) out(i) = out(i-1);
    out(0) = in;

#### rand

This userspace component changes the value on its output pin to a new random value in the range (0,1) about once every 1ms.

    component rand;
    option userspace;

    pin out float out;
    license "GPL"; // indicates GPL v2 or later
    ;;
    #include <unistd.h>

    void user_mainloop(void) {
        while(1) {
            usleep(1000);
            FOR_ALL_INSTS() out = drand48();
        }
    }

#### logic

This realtime component shows how to use "personality" to create variable-size arrays and optional pins.

    component logic "Machinekit HAL component providing experimental logic functions";
    pin in bit in-##[16 : personality & 0xff];
    pin out bit and if personality & 0x100;
    pin out bit or if personality & 0x200;
    pin out bit xor if personality & 0x400;
    function _ nofp;
    description """
    Experimental general 'logic function' component.  Can perform 'and', 'or'
    and 'xor' of up to 16 inputs.  Determine the proper value for 'personality'
    by adding:
    .IP \\(bu 4
    The number of input pins, usually from 2 to 16
    .IP \\(bu
    256 (0x100)  if the 'and' output is desired
    .IP \\(bu
    512 (0x200)  if the 'or' output is desired
    .IP \\(bu
    1024 (0x400)  if the 'xor' (exclusive or) output is desired""";
    license "GPL"; // indicates GPL v2 or later
    ;;
    FUNCTION(_) {
        int i, a=1, o=0, x=0;
        for(i=0; i < (personality & 0xff); i++) {
            if(in(i)) { o = 1; x = !x; }
            else { a = 0; }
        }
        if(personality & 0x100) and = a;
        if(personality & 0x200) or = o;
        if(personality & 0x400) xor = x;
    }

A typical load line for this component might be

    loadrt logic count=3 personality=0x102,0x305,0x503

which creates the following pins:

-   A 2-input AND gate: logic.0.and, logic.0.in-00, logic.0.in-01

-   5-input AND and OR gates: logic.1.and, logic.1.or, logic.1.in-00, logic.1.in-01, logic.1.in-02, logic.1.in-03, logic.1.in-04,

-   3-input AND and XOR gates: logic.2.and, logic.2.xor, logic.2.in-00, logic.2.in-01, logic.2.in-02

Creating Userspace Python Components
------------------------------------

### Basic usage

A userspace component begins by creating its pins and parameters, then enters a loop which will periodically drive all the outputs from the inputs. The following component copies the value seen on its input pin (*passthrough.in*) to its output pin (*passthrough.out*) approximately once per second.

    #!/usr/bin/python
    import hal, time
    h = hal.component("passthrough")
    h.newpin("in", hal.HAL_FLOAT, hal.HAL_IN)
    h.newpin("out", hal.HAL_FLOAT, hal.HAL_OUT)
    h.ready()
    try:
        while 1:
            time.sleep(1)
            h['out'] = h['in']
    except KeyboardInterrupt:
        raise SystemExit

Copy the above listing into a file named "passthrough", make it executable (*chmod +x*), and place it on your *$PATH*. Then try it out:

    halrun

    halcmd: loadusr passthrough

    halcmd: show pin

        Component Pins:
        Owner Type  Dir     Value  Name
         03   float IN          0  passthrough.in
         03   float OUT         0  passthrough.out

    halcmd: setp passthrough.in 3.14

    halcmd: show pin

        Component Pins:
        Owner Type  Dir     Value  Name
         03   float IN       3.14  passthrough.in
         03   float OUT      3.14  passthrough.out

### Userspace components and delays

If you typed “show pin” quickly, you may see that *passthrough.out* still had its old value of 0. This is because of the call to *time.sleep(1)*, which makes the assignment to the output pin occur at most once per second. Because this is a userspace component, the actual delay between assignments can be much longer if the memory used by the passthrough component is swapped to disk, the assignment could be delayed until that memory is swapped back in.

Thus, userspace components are suitable for user-interactive elements such as control panels (delays in the range of milliseconds are not noticed, and longer delays are acceptable), but not for sending step pulses to a stepper driver board (delays must always be in the range of microseconds, no matter what).

### Create pins and parameters

    h = hal.component("passthrough")

The component itself is created by a call to the constructor *hal.component*. The arguments are the HAL component name and (optionally) the prefix used for pin and parameter names. If the prefix is not specified, the component name is used.

    h.newpin("in", hal.HAL_FLOAT, hal.HAL_IN)

Then pins are created by calls to methods on the component object. The arguments are: pin name suffix, pin type, and pin direction. For parameters, the arguments are: parameter name suffix, parameter type, and parameter direction.

<table>
<caption>Table 3. HAL Option Names<span id="cap:HAL-Option-Names"></span></caption>
<colgroup>
<col width="42%" />
<col width="14%" />
<col width="14%" />
<col width="14%" />
<col width="14%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Pin and Parameter Types:</strong></p></td>
<td align="left"><p>HAL_BIT</p></td>
<td align="left"><p>HAL_FLOAT</p></td>
<td align="left"><p>HAL_S32</p></td>
<td align="left"><p>HAL_U32</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>Pin Directions:</strong></p></td>
<td align="left"><p>HAL_IN</p></td>
<td align="left"><p>HAL_OUT</p></td>
<td align="left"><p>HAL_IO</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>Parameter Directions:</strong></p></td>
<td align="left"><p>HAL_RO</p></td>
<td align="left"><p>HAL_RW</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

The full pin or parameter name is formed by joining the prefix and the suffix with a ".", so in the example the pin created is called *passthrough.in*.

    h.ready()

Once all the pins and parameters have been created, call the *.ready()* method.

#### Changing the prefix

The prefix can be changed by calling the *.setprefix()* method. The current prefix can be retrieved by calling the *.getprefix()* method.

### Reading and writing pins and parameters

For pins and parameters which are also proper Python identifiers, the value may be accessed or set using the attribute syntax:

    h.out = h.in

For all pins, whether or not they are also proper Python identifiers, the value may be accessed or set using the subscript syntax:

    h['out'] = h['in']

#### Driving output (HAL\_OUT) pins

Periodically, usually in response to a timer, all HAL\_OUT pins should be "driven" by assigning them a new value. This should be done whether or not the value is different than the last one assigned. When a pin is connected to a signal, its old output value is not copied into the signal, so the proper value will only appear on the signal once the component assigns a new value.

#### Driving bidirectional (HAL\_IO) pins

The above rule does not apply to bidirectional pins. Instead, a bidirectional pin should only be driven by the component when the component wishes to change the value. For instance, in the canonical encoder interface, the encoder component only sets the *index-enable* pin to **FALSE** (when an index pulse is seen and the old value is **TRUE**), but never sets it to **TRUE**. Repeatedly driving the pin **FALSE** might cause the other connected component to act as though another index pulse had been seen.

### Exiting

A *halcmd unload* request for the component is delivered as a *KeyboardInterrupt* exception. When an unload request arrives, the process should either exit in a short time, or call the *.exit()* method on the component if substantial work (such as reading or writing files) must be done to complete the shutdown process.

### Project ideas

-   Create an external control panel with buttons, switches, and indicators. Connect everything to a microcontroller, and connect the microcontroller to the PC using a serial interface. Python has a very capable serial interface module called [pyserial](http://pyserial.sourceforge.net/) (Debian package name “python-serial”, in the universe repository)

-   Attach a [LCDProc](http://lcdproc.omnipotent.net/)-compatible LCD module and use it to display a digital readout with information of your choice (Debian package name “lcdproc”, in the universe repository)

-   Create a virtual control panel using any GUI library supported by Python (gtk, qt, wxwindows, etc)

Hardware Drivers
================

AX5214H Driver
--------------

<span id="cha:ax5214-driver"></span>

The Axiom Measurement & Control AX5214H is a 48 channel digital I/O board. It plugs into an ISA bus, and resembles a pair of 8255 chips. In fact it may be a pair of 8255 chips, but I’m not sure. If/when someone starts a driver for an 8255 they should look at the ax5214 code, much of the work is already done.

### Installing

    loadrt hal_ax5214h cfg="<config-string>"

The config string consists of a hex port address, followed by an 8 character string of "I" and "O" which sets groups of pins as inputs and outputs. The first two character set the direction of the first two 8 bit blocks of pins (0-7 and 8-15). The next two set blocks of 4 pins (16-19 and 20-23). The pattern then repeats, two more blocks of 8 bits (24-31 and 32-39) and two blocks of 4 bits (40-43 and 44-47). If more than one board is installed, the data for the second board follows the first. As an example, the string *"0x220 IIIOIIOO 0x300 OIOOIOIO"* installs drivers for two boards. The first board is at address 0x220, and has 36 inputs (0-19 and 24-39) and 12 outputs (20-23 and 40-47). The second board is at address 0x300, and has 20 inputs (8-15, 24-31, and 40-43) and 28 outputs (0-7. 16-23, 32-39, and 44-47). Up to 8 boards may be used in one system.

### Pins

-   *(bit) ax5214.&lt;boardnum&gt;.out-&lt;pinnum&gt;* — Drives a physical output pin.

-   *(bit) ax5214.&lt;boardnum&gt;.in-&lt;pinnum&gt;* — Tracks a physical input pin.

-   *(bit) ax5214.&lt;boardnum&gt;.in-&lt;pinnum&gt;-not* — Tracks a physical input pin, inverted.

For each pin, &lt;boardnum&gt; is the board number (starts at zero), and &lt;pinnum&gt; is the I/O channel number (0 to 47).

Note that the driver assumes active LOW signals. This is so that modules such as OPTO-22 will work correctly (TRUE means output ON, or input energized). If the signals are being used directly without buffering or isolation the inversion needs to be accounted for. The in- HAL pin is TRUE if the physical pin is low (OPTO-22 module energized), and FALSE if the physical pin is high (OPTO-22 module off). The in-&lt;pinnum&gt;-not HAL pin is inverted — it is FALSE if the physical pin is low (OPTO-22 module energized). By connecting a signal to one or the other, the user can determine the state of the input.

### Parameters

-   *(bit) ax5214.&lt;boardnum&gt;.out-&lt;pinnum&gt;-invert* — Inverts an output pin.

The -invert parameter determines whether an output pin is active high or active low. If -invert is FALSE, setting the HAL out- pin TRUE drives the physical pin low, turning ON an attached OPTO-22 module, and FALSE drives it high, turning OFF the OPTO-22 module. If -invert is TRUE, then setting the HAL out- pin TRUE will drive the physical pin high and turn the module OFF.

### Functions

-   *(funct) ax5214.&lt;boardnum&gt;.read* — Reads all digital inputs on one board.

-   *(funct) ax5214.&lt;boardnum&gt;.write* — Writes all digital outputs on one board.

GS2 VFD Driver
--------------

<span id="cha:gs2-vfd-driver"></span>

This is a userspace HAL program for the GS2 series of VFD’s at Automation Direct.

This component is loaded using the halcmd "loadusr" command:

    loadusr -Wn spindle-vfd gs2_vfd -n spindle-vfd

The above command says: loadusr, wait for named to load, component gs2\_vfd, named spindle-vfd

### Command Line Options

-   *-b or --bits &lt;n&gt;* (default 8) Set number of data bits to &lt;n&gt;, where n must be from 5 to 8 inclusive

-   *-d or --device &lt;path&gt;* (default /dev/ttyS0) Set the name of the serial device node to use

-   *-g or --debug* Turn on debugging messages. This will also set the verbose flag. Debug mode will cause all modbus messages to be printed in hex on the terminal.

-   *-n or --name &lt;string&gt;* (default gs2\_vfd) Set the name of the HAL module. The HAL comp name will be set to &lt;string&gt;, and all pin and parameter names will begin with &lt;string&gt;.

-   *-p or --parity {even,odd,none}* (default odd) Set serial parity to even, odd, or none.

-   *-r or --rate &lt;n&gt;* (default 38400) Set baud rate to &lt;n&gt;. It is an error if the rate is not one of the following: 110, 300, 600, 1200, 2400, 4800, 9600, 19200, 38400, 57600, 115200

-   *-s or --stopbits {1,2}* (default 1) Set serial stop bits to 1 or 2

-   *-t or --target &lt;n&gt;* (default 1) Set MODBUS target (slave) number. This must match the device number you set on the GS2.

-   *-v or --verbose* Turn on debug messages.

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

### Pins

Where &lt;n&gt; is gs2\_vfd or the name given during loading with the -n option.

-   *&lt;n&gt;.DC-bus-volts* (float, out) The DC bus voltage of the VFD

-   *&lt;n&gt;.at-speed* (bit, out) when drive is at commanded speed

-   *&lt;n&gt;.err-reset* (bit, in) reset errors sent to VFD

-   *&lt;n&gt;.firmware-revision* (s32, out) from the VFD

-   *&lt;n&gt;.frequency-command* (float, out) from the VFD

-   *&lt;n&gt;.frequency-out* (float, out) from the VFD

-   *&lt;n&gt;.is-stopped* (bit, out) when the VFD reports 0 Hz output

-   *&lt;n&gt;.load-percentage* (float, out) from the VFD

-   *&lt;n&gt;.motor-RPM* (float, out) from the VFD

-   *&lt;n&gt;.output-current* (float, out) from the VFD

-   *&lt;n&gt;.output-voltage* (float, out) from the VFD

-   *&lt;n&gt;.power-factor* (float, out) from the VFD

-   *&lt;n&gt;.scale-frequency* (float, out) from the VFD

-   *&lt;n&gt;.speed-command* (float, in) speed sent to VFD in RPM It is an error to send a speed faster than the Motor Max RPM as set in the VFD

-   *&lt;n&gt;.spindle-fwd* (bit, in) 1 for FWD and 0 for REV sent to VFD

-   *&lt;n&gt;.spindle-rev* (bit, in) 1 for REV and 0 if off

-   *&lt;n&gt;.spindle-on* (bit, in) 1 for ON and 0 for OFF sent to VFD

-   *&lt;n&gt;.status-1* (s32, out) Drive Status of the VFD (see the GS2 manual)

-   *&lt;n&gt;.status-2* (s32, out) Drive Status of the VFD (see the GS2 manual)

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
<td align="left">The status value is a sum of all the bits that are on. So a 163 which means the drive is in the run mode is the sum of 3 (run) + 32 (freq set by serial) + 128 (operation set by serial).</td>
</tr>
</tbody>
</table>

### Parameters

Where &lt;n&gt; is gs2\_vfd or the name given during loading with the -n option.

-   *&lt;n&gt;.error-count* (s32, RW)

-   *&lt;n&gt;.loop-time* (float, RW) how often the modbus is polled (default 0.1)

-   *&lt;n&gt;.nameplate-HZ* (float, RW) Nameplate Hz of motor (default 60)

-   *&lt;n&gt;.nameplate-RPM* (float, RW) Nameplate RPM of motor (default 1730)

-   *&lt;n&gt;.retval* (s32, RW) the return value of an error in HAL

-   *&lt;n&gt;.tolerance* (s32, RW) speed tolerance (default 0.01)

For an example of using this component to drive a spindle see the [GS2 Spindle](#cha:gs2-spindle) example.

VFS11 VFD Driver
----------------

<span id="cha:vfs11-vfd-driver"></span>

This is a userspace HAL program to control the S11 series of VFD’s from Toshiba.

vfs11\_vfd supports serial and TCP connections. Serial connections may be RS232 or RS485. RS485 is supported in full- and half-duplex mode. TCP connections may be passive (wait for incoming connection), or active outgoing connections, which may be useful to connect to TCP-based devices or through a terminal server.

Regardless of the connection type, vfs11\_vfd operates as a Modbus master.

This component is loaded using the halcmd "loadusr" command:

    loadusr -Wn spindle-vfd vfs11_vfd -n spindle-vfd

The above command says: loadusr, wait for named to load, component vfs11\_vfd, named spindle-vfd

### Command Line Options

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

### Pins

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

### Parameters

Where &lt;n&gt; is `vfs11_vfd` or the name given during loading with the -n option.

-   *&lt;n&gt;.frequency-limit* (float, RO) upper limit read from VFD setup.

-   *&lt;n&gt;.loop-time* (float, RW) how often the Modbus is polled (default interval 0.1 seconds)

-   *&lt;n&gt;.nameplate-HZ* (float, RW) Nameplate Hz of motor (default 50). Used to calculate target frequency (together with nameplate-RPM ) for a target RPM value as given by speed-command.

-   *&lt;n&gt;.nameplate-RPM* (float, RW) Nameplate RPM of motor (default 1410)

-   *&lt;n&gt;.rpm-limit* (float, RW) do-not-exceed soft limit for motor RPM (defaults to nameplate-RPM ).

-   *&lt;n&gt;.tolerance* (float, RW) speed tolerance (default 0.01) for determining wether spindle is at speed (0.01 meaning: output frequency is within 1% of target frequency)

### INI file configuration

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

### HAL example

### Panel operation

The vfs11\_vfd driver takes precedence over panel control while it is enabled (see *enable* pin), effectively disabling the panel. Clearing the *enable* pin re-enables the panel. Pins and parameters can still be set, but will not be written to the VFD untile the *enable* pin is set. Operating parameters are still read while bus control is disabled. Exiting the vfs11\_vfd driver in a controlled way will release the VFD from the bus and restore panel control.

See the EMC2 Integrators Manual for more information. For a detailed register description of the Toshiba VFD’s, see the "TOSVERT VF-S11 Communications Function Instruction Manual" (Toshiba document number E6581222) and the "TOSVERT VF-S11 Instruction manual" (Toshiba document number E6581158).

### Error Recovery

`vfs11_vfd` recovers from I/O errors as follows: First, all HAL pins are set to default values, and the driver will sleep for `RECONNECT_DELAY` seconds (default 1 second).

-   Serial (`TYPE=rtu`) mode: on error, close and reopen the serial port.

-   TCP server (`TYPE=tcpserver`) mode: on losing the TCP connection, the driver will go back to listen for incoming connections.

-   TCP client (`TYPE=tcpclient`) mode: on losing the TCP connection, the driver will reconnect to *TCPDEST:PORTNO*.

### Configuring the VFS11 VFD for Modbus usage

#### Connecting the Serial Port

The VF-S11 has an RJ-45 jack for serial communication. Unfortunately, it does not have a standard RS-232 plug and logic levels. The Toshiba-recommended way is: connect the USB001Z USB-to-serial conversion unit to the drive, and plug the USB port into the PC. A cheaper alternative is a homebrew interface ( [hints from Toshiba support](http://git.mah.priv.at/gitweb/vfs11-vfd.git/blob_plain/refs/heads/f12-prod:/VFS11-RJ45_e.pdf), [circuit diagram](http://git.mah.priv.at/gitweb/vfs11-vfd.git/blob_plain/refs/heads/f12-prod:/vfs11-rs232.pdf)).

Note: the 24V output from the VFD has no short-circuit protection.

Serial port factory defaults are 9600/8/1/even, the protocol defaults to the proprietary "Toshiba Inverter Protocol".

#### Modbus setup

Several parameters need setting before the VF-S11 will talk to this module. This can either be done manually with the control panel, or over the serial link - Toshiba supplies a Windows application called *PCM001Z* which can read/set parameters in the VFD. Note - PCM001Z only talks the Toshiba inverter protocol. So the last parameter which you’d want to change is the protocol - set from Toshiba Inverter Protocol to Modbus; thereafter, the Windows app is useless.

To increase the upper frequency limit, the UL and FH parameters must be changed on the panel. I increased them from 50 to 80.

See dump-params.mio for a description of non-standard VF-S11 parameters of my setup. This file is for the [modio Modbus interactive utility](http://git.mah.priv.at/gitweb/modio.git).

### Programming Note

The vfs11\_vfd driver uses the [libmodbus version 3](http://www.libmodbus.org) library which is more recent than the version 2 code used in `gs2_vfd`.

The Debian `libmodbus5` and `libmodbus-dev` packages are only available starting from Debian 12 (*Precise Pengolin*). Moreover, these packages lack support for the MODBUS\_RTS\_MODE\_\* flags. Therefore, building vfs11\_vfd using this library might generate a warning if RTS\_MODE= is specified in the ini file.

To use the full functionality on lucid and precise: \* remove the libmodbus packages: `sudo apt-get remove libmodbus5 libmodbus-dev` \* build and install libmodbus version 3 from source as outlined [here](https://github.com/stephane/libmodbus/blob/master/README.rst).

Libmodbus does not build on Debian Hardy, hence vfs11\_vfd is not available on hardy.

Mesa HostMot2 Driver
--------------------

<span id="cha:mesa-hostmot2-driver"></span>

### Introduction

HostMot2 is an FPGA configuration developed by Mesa Electronics for their line of *Anything I/O* motion control cards. The firmware is open source, portable and flexible. It can be configured (at compile-time) with zero or more instances (an object created at runtime) of each of several Modules: encoders (quadrature counters), PWM generators, and step/dir generators. The firmware can be configured (at run-time) to connect each of these instances to pins on the I/O headers. I/O pins not driven by a Module instance revert to general-purpose bi-directional digital I/O.

### Firmware Binaries

50 Pin Header FPGA cards

Several pre-compiled HostMot2 firmware binaries are available for the different Anything I/O boards. (This list is incomplete, check the hostmot2-firmware distribution for up-to-date firmware lists.)

-   3x20 (144 I/O pins): using hm2\_pci module

    -   24-channel servo

    -   16-channel servo plus 24 step/dir generators

-   5i22 (96 I/O pins): using hm2\_pci module

    -   16-channel servo

    -   8-channel servo plus 24 step/dir generators

-   5i20, 5i23, 4i65, 4i68 (72 I/O pins): using hm2\_pci module

    -   12-channel servo

    -   8-channel servo plus 4 step/dir generators

    -   4-channel servo plus 8 step/dir generators

-   7i43 (48 I/O pins): using hm2\_7i43 module

    -   8-channel servo (8 PWM generators & 8 encoders)

    -   4-channel servo plus 4 step/dir generators

DB25 FPGA cards

The 5i25 Superport FPGA card is preprogrammed when purchased and does not need a firmware binary.

### Installing Firmware

Depending on how you installed Machinekit you may have to open the Synaptic Package Manager from the System menu and install the package for your Mesa card. The quickest way to find them is to do a search for *hostmot2* in the Synaptic Package Manager. Mark the firmware for installation, then apply.

### Loading HostMot2

The Machinekit support for the HostMot2 firmware is split into a generic driver called *hostmot2* and two low-level I/O drivers for the Anything I/O boards. The low-level I/O drivers are *hm2\_7i43* and *hm2\_pci* (for all the PCI- and PC-104/Plus-based Anything I/O boards). The hostmot2 driver must be loaded first, using a HAL command like this:

    loadrt hostmot2

See the hostmot2(9) man page for details.

The hostmot2 driver by itself does nothing, it needs access to actual boards running the HostMot2 firmware. The low-level I/O drivers provide this access. The low-level I/O drivers are loaded with commands like this:

    loadrt hm2_pci config="firmware=hm2/5i20/SVST8_4.BIT
           num_encoders=3 num_pwmgens=3 num_stepgens=1"

The config parameters are described in the hostmot2 man page.

### Watchdog

The HostMot2 firmware may include a watchdog Module; if it does, the hostmot2 driver will use it.

The watchdog must be petted by Machinekit periodically or it will bite.

When the watchdog bites, all the board’s I/O pins are disconnected from their Module instances and become high-impedance inputs (pulled high), and all communication with the board stops. The state of the HostMot2 firmware modules is not disturbed (except for the configuration of the I/O Pins). Encoder instances keep counting quadrature pulses, and pwm- and step-generators keep generating signals (which are not relayed to the motors, because the I/O Pins have become inputs).

Resetting the watchdog resumes communication and resets the I/O pins to the configuration chosen at load-time.

If the firmware includes a watchdog, the following HAL objects will be exported:

#### Pins:

-   *has\_bit* - (bit i/o) True if the watchdog has bit, False if the watchdog has not bit. If the watchdog has bit and the has\_bit bit is True, the user can reset it to False to resume operation.

#### Parameters:

-   *timeout\_ns* - (u32 read/write) Watchdog timeout, in nanoseconds. This is initialized to 1,000,000,000 (1 second) at module load time. If more than this amount of time passes between calls to the pet\_watchdog() function, the watchdog will bite.

#### Functions:

-   *pet\_watchdog()* - Calling this function resets the watchdog timer and postpones the watchdog biting until timeout\_ns nanoseconds later. This function should be added to the servo thread.

### HostMot2 Functions

-   *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.read* - Read all inputs, update input HAL pins.

-   *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.write* - Write all outputs.

-   *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.pet-watchdog* - Pet the watchdog to keep it from biting us for a while.

-   *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.read\_gpio* - Read the GPIO input pins only. (This function is not available on the 7i43 due to limitations of the EPP bus.)

-   *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.write\_gpio* - Write the GPIO control registers and output pins only. (This function is not available on the 7i43 due to limitations of the EPP bus.)

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
<td align="left"><div class="paragraph">
<p>The above <em>read_gpio</em> and <em>write_gpio</em> functions should not normally be needed, since the GPIO bits are read and written along with everything else in the standard <em>read</em> and <em>write</em> functions above, which are normally run in the servo thread.</p>
</div>
<div class="paragraph">
<p>The <em>read_gpio</em> and <em>write_gpio</em> functions were provided in case some very fast (frequently updated) I/O is needed. These functions should be run in the base thread. If you have need for this, please send an email and tell us about it, and what your application is.</p>
</div></td>
</tr>
</tbody>
</table>

### Pinouts

The hostmot2 driver does not have a particular pinout. The pinout comes from the firmware that the hostmot2 driver sends to the Anything I/O board. Each firmware has different pinout, and the pinout depends on how many of the available encoders, pwmgens, and stepgens are used. To get a pinout list for your configuration after loading Machinekit in the terminal window type:

    dmesg > hm2.txt

The resulting text file will contain lots of information as well as the pinout for the HostMot2 and any error and warning messages.

To reduce the clutter by clearing the message buffer before loading Machinekit type the following in the terminal window:

    sudo dmesg -c

Now when you run Machinekit and then do a *dmesg &gt; hm2.txt* in the terminal only the info from the time you loaded Machinekit will be in your file along with your pinout. The file will be in the current directory of the terminal window. Each line will contain the card name, the card number, the I/O Pin number, the connector and pin, and the usage. From this printout you will know the physical connections to your card based on your configuration.

An example of a 5i20 configuration:

    [HOSTMOT2]
    DRIVER=hm2_pci
    BOARD=5i20
    CONFIG="firmware=hm2/5i20/SVST8_4.BIT num_encoders=1 num_pwmgens=1 num_stepgens=3"

The above configuration produced this printout.

    [ 1141.053386] hm2/hm2_5i20.0: 72 I/O Pins used:
    [ 1141.053394] hm2/hm2_5i20.0: IO Pin 000 (P2-01): IOPort
    [ 1141.053397] hm2/hm2_5i20.0: IO Pin 001 (P2-03): IOPort
    [ 1141.053401] hm2/hm2_5i20.0: IO Pin 002 (P2-05): Encoder #0, pin B (Input)
    [ 1141.053405] hm2/hm2_5i20.0: IO Pin 003 (P2-07): Encoder #0, pin A (Input)
    [ 1141.053408] hm2/hm2_5i20.0: IO Pin 004 (P2-09): IOPort
    [ 1141.053411] hm2/hm2_5i20.0: IO Pin 005 (P2-11): Encoder #0, pin Index (Input)
    [ 1141.053415] hm2/hm2_5i20.0: IO Pin 006 (P2-13): IOPort
    [ 1141.053418] hm2/hm2_5i20.0: IO Pin 007 (P2-15): PWMGen #0, pin Out0 (PWM or Up) (Output)
    [ 1141.053422] hm2/hm2_5i20.0: IO Pin 008 (P2-17): IOPort
    [ 1141.053425] hm2/hm2_5i20.0: IO Pin 009 (P2-19): PWMGen #0, pin Out1 (Dir or Down) (Output)
    [ 1141.053429] hm2/hm2_5i20.0: IO Pin 010 (P2-21): IOPort
    [ 1141.053432] hm2/hm2_5i20.0: IO Pin 011 (P2-23): PWMGen #0, pin Not-Enable (Output)
    <snip>...
    [ 1141.053589] hm2/hm2_5i20.0: IO Pin 060 (P4-25): StepGen #2, pin Step (Output)
    [ 1141.053593] hm2/hm2_5i20.0: IO Pin 061 (P4-27): StepGen #2, pin Direction (Output)
    [ 1141.053597] hm2/hm2_5i20.0: IO Pin 062 (P4-29): StepGen #2, pin (unused) (Output)
    [ 1141.053601] hm2/hm2_5i20.0: IO Pin 063 (P4-31): StepGen #2, pin (unused) (Output)
    [ 1141.053605] hm2/hm2_5i20.0: IO Pin 064 (P4-33): StepGen #2, pin (unused) (Output)
    [ 1141.053609] hm2/hm2_5i20.0: IO Pin 065 (P4-35): StepGen #2, pin (unused) (Output)
    [ 1141.053613] hm2/hm2_5i20.0: IO Pin 066 (P4-37): IOPort
    [ 1141.053616] hm2/hm2_5i20.0: IO Pin 067 (P4-39): IOPort
    [ 1141.053619] hm2/hm2_5i20.0: IO Pin 068 (P4-41): IOPort
    [ 1141.053621] hm2/hm2_5i20.0: IO Pin 069 (P4-43): IOPort
    [ 1141.053624] hm2/hm2_5i20.0: IO Pin 070 (P4-45): IOPort
    [ 1141.053627] hm2/hm2_5i20.0: IO Pin 071 (P4-47): IOPort
    [ 1141.053811] hm2/hm2_5i20.0: registered
    [ 1141.053815] hm2_5i20.0: initialized AnyIO board at 0000:02:02.0

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
<td align="left">That the I/O Pin nnn will correspond to the pin number shown on the HAL Configuration screen for GPIOs. Some of the Stepgen, Encoder and PWMGen will also show up as GPIOs in the HAL Configuration screen.</td>
</tr>
</tbody>
</table>

### PIN Files

The default pinout is described in a .PIN file (human-readable text). When you install a firmware package the .PIN files are installed in

    /usr/share/doc/hostmot2-firmware-<board>/

### Firmware

The selected firmware (.BIT file) and configuration is uploaded from the PC motherboard to the Mesa mothercard on Machinekit startup. If you are using Run In Place, you must still install a hostmot2-firmware-&lt;board&gt; package. There is more information about firmware and configuration in the *Configurations* section.

### HAL Pins

The HAL pins for each configuration can be seen by opening up *Show HAL Configuration* from the Machine menu. All the HAL pins and parameters can be found there. The following figure is of the 5i20 configuration used above.

![images/5i20-halpins.png]()

Figure 25. 5i20 HAL Pins<span id="cap:5i20-HAL-Pins"></span>

### Configurations

The Hostmot2 firmware is available in several versions, depending on what you are trying to accomplish. You can get a reminder of what a particular firmware is for by looking at the name. Let’s look at a couple of examples.

In the 7i43 (two ports), SV8 (*Servo 8*) would be for having 8 servos or fewer, using the *classic* 7i33 4-axis (per port) servo board. So 8 servos would use up all 48 signals in the two ports. But if you only needed 3 servos, you could say *num\_encoders=3* and *num\_pwmgens=3* and recover 5 servos at 6 signals each, thus gaining 30 bits of GPIO.

Or, in the 5i22 (four ports), SVST8\_24 (*Servo 8, Stepper 24*) would be for having 8 servos or fewer (7i33 x2 again), and 24 steppers or fewer (7i47 x2). This would use up all four ports. If you only needed 4 servos you could say *num\_encoders=4* and *num\_pwmgens=4* and recover 1 port (and save a 7i33). And if you only needed 12 steppers you could say *num\_stepgens=12* and free up one port (and save a 7i47). So in this way we can save two ports (48 bits) for GPIO.

Here are tables of the firmwares available in the official packages. There may be additional firmwares available at the Mesanet.com website that have not yet made it into the Machinekit official firmware packages, so check there too.

3x20 (6-port various) Default Configurations (The 3x20 comes in 1M, 1.5M, and 2M gate versions. So far, all firmware is available in all gate sizes.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWMGen</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV24</p></td>
<td align="left"><p>24</p></td>
<td align="left"><p>24</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST16_24</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>24</p></td>
<td align="left"><p>0</p></td>
</tr>
</tbody>
</table>

5i22 (4-port PCI) Default Configurations (The 5i22 comes in 1M and 1.5M gate versions. So far, all firmware is available in all gate sizes.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV16</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>72</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_24</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>24</p></td>
<td align="left"><p>0</p></td>
</tr>
</tbody>
</table>

5i23 (3-port PCI) Default Configurations (The 5i23 has 400k gates.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST2_8</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>8 (tbl5)</p></td>
<td align="left"><p>12</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>48</p></td>
</tr>
<tr class="even">
<td align="left"><p>SV12_2X7I48_72</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>24</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SV12IM_2X7I48_72</p></td>
<td align="left"><p>12 (+IM)</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>12</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST4_8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8 (tbl5)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_4</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4 (tbl5)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_4IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_8IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVTP6_7I39</p></td>
<td align="left"><p>6</p></td>
<td align="left"><p>0 (6 BLDC)</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
</tbody>
</table>

5i20 (3-port PCI) Default Configurations (The 5i20 has 200k gates.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST2_8</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>8 (tbl5)</p></td>
<td align="left"><p>12</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>48</p></td>
</tr>
<tr class="even">
<td align="left"><p>SV12_2X7I48_72</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>24</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SV12IM_2X7I48_72</p></td>
<td align="left"><p>12 (+IM)</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>12</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_4</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4 (tbl5)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_4IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8</p></td>
</tr>
</tbody>
</table>

4i68 (3-port PC/104) Default Configurations (The 4i68 has 400k gates.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>48</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST4_8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_4</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_4IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_8IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
</tr>
</tbody>
</table>

4i65 (3-port PC/104) Default Configurations (The 4i65 has 200k gates.)

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST8_4</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST8_4IM2</p></td>
<td align="left"><p>8 (+IM)</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>8</p></td>
</tr>
</tbody>
</table>

7i43 (2-port parallel) 400k gate versions, Default Configurations

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST4_4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4 (tbl5)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST4_6</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>6 (tbl3)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST4_12</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>24</p></td>
</tr>
</tbody>
</table>

7i43 (2-port parallel) 200k gate versions, Default Configurations

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Firmware</th>
<th align="left">Encoder</th>
<th align="left">PWM</th>
<th align="left">StepGen</th>
<th align="left">GPIO</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>SV8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST4_4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4 (tbl5)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="odd">
<td align="left"><p>SVST4_6</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>6 (tbl3)</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>SVST2_4_7I47</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>24</p></td>
</tr>
</tbody>
</table>

Even though several cards may have the same named .BIT file you cannot use a .BIT file that is not for that card. Different cards have different clock frequencies so make sure you load the proper .BIT file for your card. Custom hm2 firmwares can be created for special applications and you may see some custom hm2 firmwares in the directories with the default ones.

When you load the board-driver (hm2\_pci or hm2\_7i43), you can tell it to disable instances of the three primary modules (pwmgen, stepgen, and encoder) by setting the count lower. Any I/O pins belonging to disabled module instances become GPIOs.

### GPIO

General Purpose I/O pins on the board which are not used by a module instance are exported to HAL as *full* GPIO pins. Full GPIO pins can be configured at run-time to be inputs, outputs, or open drains, and have a HAL interface that exposes this flexibility. I/O pins that are owned by an active module instance are constrained by the requirements of the owning module, and have a restricted HAL interface.

GPIOs have names like *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.gpio.&lt;IONum&gt;.* IONum. is a three-digit number. The mapping from IONum to connector and pin-on-that-connector is written to the syslog when the driver loads, and it’s documented in Mesa’s manual for the Anything I/O boards.

The hm2 GPIO representation is modeled after the Digital Inputs and Digital Outputs described in the Canonical Device Interface (part of the HAL General Reference document).

GPIO pins default to input.

#### Pins

-   *in* - (Bit, Out) Normal state of the hardware input pin. Both full GPIO pins and I/O pins used as inputs by active module instances have this pin.

-   *in\_not* - (Bit, Out) Inverted state of the hardware input pin. Both full GPIO pins and I/O pins used as inputs by active module instances have this pin.

-   *out* - (Bit, In) Value to be written (possibly inverted) to the hardware output pin. Only full GPIO pins have this pin.

#### Parameters

-   *invert\_output* - (Bit, RW) This parameter only has an effect if the *is\_output* parameter is true. If this parameter is true, the output value of the GPIO will be the inverse of the value on the *out* HAL pin. Only full GPIO pins and I/O pins used as outputs by active module instances have this parameter. To invert an active module pin you have to invert the GPIO pin not the module pin.

-   *is\_opendrain* - (Bit, RW) This parameter only has an effect if the *is\_output* parameter is true. If this parameter is false, the GPIO behaves as a normal output pin: the I/O pin on the connector is driven to the value specified by the *out* HAL pin (possibly inverted), and the value of the *in* and *in\_not* HAL pins is undefined. If this parameter is true, the GPIO behaves as an open-drain pin. Writing 0 to the *out* HAL pin drives the I/O pin low, writing 1 to the *out* HAL pin puts the I/O pin in a high-impedance state. In this high-impedance state the I/O pin floats (weakly pulled high), and other devices can drive the value; the resulting value on the I/O pin is available on the *in* and *in\_not* pins. Only full GPIO pins and I/O pins used as outputs by active module instances have this parameter.

-   *is\_output* - (Bit, RW) If set to 0, the GPIO is an input. The I/O pin is put in a high-impedance state (weakly pulled high), to be driven by other devices. The logic value on the I/O pin is available in the *in* and *in\_not* HAL pins. Writes to the *out* HAL pin have no effect. If this parameter is set to 1, the GPIO is an output; its behavior then depends on the *is\_opendrain* parameter. Only full GPIO pins have this parameter.

### StepGen

Stepgens have names like *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.stepgen.&lt;Instance&gt;.*. *Instance* is a two-digit number that corresponds to the HostMot2 stepgen instance number. There are *num\_stepgens* instances, starting with 00.

Each stepgen allocates 2-6 I/O pins (selected at firmware compile time), but currently only uses two: Step and Direction outputs.<span class="footnote">
\[At present, the firmware supports multi-phase stepper outputs, but the driver doesn’t. Interested volunteers are solicited.\]
</span>

The stepgen representation is modeled on the stepgen software component. Stepgen default is active high step output (high during step time low during step space). To invert a StepGen output pin you invert the corresponding GPIO pin that is being used by StepGen. To find the GPIO pin being used for the StepGen output run dmesg as shown above.

Each stepgen instance has the following pins and parameters:

#### Pins

-   *control-type* - (Bit, In) Switches between position control mode (0) and velocity control mode (1). Defaults to position control (0).

-   *counts* - (s32, Out) Feedback position in counts (number of steps).

-   *enable* - (Bit, In) Enables output steps. When false, no steps are generated.

-   *position-cmd* - (Float, In) Target position of stepper motion, in user-defined position units.

-   *position-fb* - (Float, Out) Feedback position in user-defined position units (counts / position\_scale).

-   *velocity-cmd* - (Float, In) Target velocity of stepper motion, in user-defined position units per second. This pin is only used when the stepgen is in velocity control mode (control-type=1).

-   *velocity-fb* - (Float, Out) Feedback velocity in user-defined position units per second.

#### Parameters

-   *dirhold* - (u32, RW) Minimum duration of stable Direction signal after a step ends, in nanoseconds.

-   *dirsetup* - (u32, RW) Minimum duration of stable Direction signal before a step begins, in nanoseconds.

-   *maxaccel* - (Float, RW) Maximum acceleration, in position units per second per second. If set to 0, the driver will not limit its acceleration.

-   *maxvel* - (Float, RW) Maximum speed, in position units per second. If set to 0, the driver will choose the maximum velocity based on the values of steplen and stepspace (at the time that maxvel was set to 0).

-   *position-scale* - (Float, RW) Converts from counts to position units. position = counts / position\_scale

-   *step\_type* - (u32, RW) Output format, like the step\_type modparam to the software stegen(9) component. 0 = Step/Dir, 1 = Up/Down, 2 = Quadrature. In Quadrature mode (step\_type=2), the stepgen outputs one complete Gray cycle (00 -&gt; 01 -&gt; 11 -&gt; 10 -&gt; 00) for each *step* it takes.

-   *steplen* - (u32, RW) Duration of the step signal, in nanoseconds.

-   *stepspace* - (u32, RW) Minimum interval between step signals, in nanoseconds.

#### Output Parameters

The Step and Direction pins of each StepGen have two additional parameters. To find which I/O pin belongs to which step and direction output run dmesg as described above.

-   *invert\_output* - (Bit, RW) This parameter only has an effect if the *is\_output* parameter is true. If this parameter is true, the output value of the GPIO will be the inverse of the value on the *out* HAL pin.

-   *is\_opendrain* - (Bit, RW) If this parameter is false, the GPIO behaves as a normal output pin: the I/O pin on the connector is driven to the value specified by the *out* HAL pin (possibly inverted). If this parameter is true, the GPIO behaves as an open-drain pin. Writing 0 to the *out* HAL pin drives the I/O pin low, writing 1 to the *out* HAL pin puts the I/O pin in a high-impedance state. In this high-impedance state the I/O pin floats (weakly pulled high), and other devices can drive the value; the resulting value on the I/O pin is available on the *in* and *in\_not* pins. Only full GPIO pins and I/O pins used as outputs by active module instances have this parameter.

### PWMGen

PWMgens have names like *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.pwmgen.&lt;Instance&gt;.*. *Instance* is a two-digit number that corresponds to the HostMot2 pwmgen instance number. There are *num\_pwmgens* instances, starting with 00.

In HM2, each pwmgen uses three output I/O pins: Not-Enable, Out0, and Out1. To invert a PWMGen output pin you invert the corresponding GPIO pin that is being used by PWMGen. To find the GPIO pin being used for the PWMGen output run dmesg as shown above.

The function of the Out0 and Out1 I/O pins varies with output-type parameter (see below).

The hm2 pwmgen representation is similar to the software pwmgen component. Each pwmgen instance has the following pins and parameters:

#### Pins

-   *enable* - (Bit, In) If true, the pwmgen will set its Not-Enable pin false and output its pulses. If *enable* is false, pwmgen will set its Not-Enable pin true and not output any signals.

-   *value* - (Float, In) The current pwmgen command value, in arbitrary units.

#### Parameters

-   *output-type* - (s32, RW) This emulates the output\_type load-time argument to the software pwmgen component. This parameter may be changed at runtime, but most of the time you probably want to set it at startup and then leave it alone. Accepted values are 1 (PWM on Out0 and Direction on Out1), 2 (Up on Out0 and Down on Out1), 3 (PDM mode, PDM on Out0 and Dir on Out1), and 4 (Direction on Out0 and PWM on Out1, *for locked antiphase*).

-   *scale* - (Float, RW) Scaling factor to convert *value* from arbitrary units to duty cycle: dc = value / scale. Duty cycle has an effective range of -1.0 to +1.0 inclusive, anything outside that range gets clipped.

-   *pdm\_frequency* - (u32, RW) This specifies the PDM frequency, in Hz, of all the pwmgen instances running in PDM mode (mode 3). This is the *pulse slot frequency*; the frequency at which the pdm generator in the Anything I/O board chooses whether to emit a pulse or a space. Each pulse (and space) in the PDM pulse train has a duration of 1/pdm\_frequency seconds. For example, setting the pdm\_frequency to 2e6 (2 MHz) and the duty cycle to 50% results in a 1 MHz square wave, identical to a 1 MHz PWM signal with 50% duty cycle. The effective range of this parameter is from about 1525 Hz up to just under 100 MHz. Note that the max frequency is determined by the ClockHigh frequency of the Anything I/O board; the 5i20 and 7i43 both have a 100 MHz clock, resulting in a 100 Mhz max PDM frequency. Other boards may have different clocks, resulting in different max PDM frequencies. If the user attempts to set the frequency too high, it will be clipped to the max supported frequency of the board.

-   *pwm\_frequency* - (u32, RW) This specifies the PWM frequency, in Hz, of all the pwmgen instances running in the PWM modes (modes 1 and 2). This is the frequency of the variable-duty-cycle wave. Its effective range is from 1 Hz up to 193 KHz. Note that the max frequency is determined by the ClockHigh frequency of the Anything I/O board; the 5i20 and 7i43 both have a 100 MHz clock, resulting in a 193 KHz max PWM frequency. Other boards may have different clocks, resulting in different max PWM frequencies. If the user attempts to set the frequency too high, it will be clipped to the max supported frequency of the board. Frequencies below about 5 Hz are not terribly accurate, but above 5 Hz they’re pretty close.

#### Output Parameters

The output pins of each PWMGen have two additional parameters. To find which I/O pin belongs to which output run dmesg as described above.

-   *invert\_output* - (Bit, RW) This parameter only has an effect if the *is\_output* parameter is true. If this parameter is true, the output value of the GPIO will be the inverse of the value on the *out* HAL pin.

-   *is\_opendrain* - (Bit, RW) If this parameter is false, the GPIO behaves as a normal output pin: the I/O pin on the connector is driven to the value specified by the *out* HAL pin (possibly inverted). If this parameter is true, the GPIO behaves as an open-drain pin. Writing 0 to the *out* HAL pin drives the I/O pin low, writing 1 to the *out* HAL pin puts the I/O pin in a high-impedance state. In this high-impedance state the I/O pin floats (weakly pulled high), and other devices can drive the value; the resulting value on the I/O pin is available on the *in* and *in\_not* pins. Only full GPIO pins and I/O pins used as outputs by active module instances have this parameter.

### Encoder

Encoders have names like *hm2\_&lt;BoardType&gt;.&lt;BoardNum&gt;.encoder.&lt;Instance&gt;.*. *Instance* is a two-digit number that corresponds to the HostMot2 encoder instance number. There are *num\_encoders* instances, starting with 00.

Each encoder uses three or four input I/O pins, depending on how the firmware was compiled. Three-pin encoders use A, B, and Index (sometimes also known as Z). Four-pin encoders use A, B, Index, and Index-mask.

The hm2 encoder representation is similar to the one described by the Canonical Device Interface (in the HAL General Reference document), and to the software encoder component. Each encoder instance has the following pins and parameters:

#### Pins

-   *count* - (s32, Out) Number of encoder counts since the previous reset.

-   *index-enable* - (Bit, I/O) When this pin is set to True, the count (and therefore also position) are reset to zero on the next Index (Phase-Z) pulse. At the same time, index-enable is reset to zero to indicate that the pulse has occurred.

-   *position* - (Float, Out) Encoder position in position units (count / scale).

-   *rawcounts* - (s32, Out) Total number of encoder counts since the start, not adjusted for index or reset.

-   *reset* - (Bit, In) When this pin is TRUE, the count and position pins are set to 0. (The value of the velocity pin is not affected by this.) The driver does not reset this pin to FALSE after resetting the count to 0, that is the user’s job.

-   *velocity* - (Float, Out) Estimated encoder velocity in position units per second.

#### Parameters

-   *counter-mode* - (Bit, RW) Set to False (the default) for Quadrature. Set to True for Up/Down or for single input on Phase A. Can be used for a frequency to velocity converter with a single input on Phase A when set to true.

-   *filter* - (Bit, RW) If set to True (the default), the quadrature counter needs 15 clocks to register a change on any of the three input lines (any pulse shorter than this is rejected as noise). If set to False, the quadrature counter needs only 3 clocks to register a change. The encoder sample clock runs at 33 MHz on the PCI Anything I/O cards and 50 MHz on the 7i43.

-   *index-invert* - (Bit, RW) If set to True, the rising edge of the Index input pin triggers the Index event (if index-enable is True). If set to False, the falling edge triggers.

-   *index-mask* - (Bit, RW) If set to True, the Index input pin only has an effect if the Index-Mask input pin is True (or False, depending on the index-mask-invert pin below).

-   *index-mask-invert* - (Bit, RW) If set to True, Index-Mask must be False for Index to have an effect. If set to False, the Index-Mask pin must be True.

-   *scale* - (Float, RW) Converts from *count* units to *position* units. A quadrature encoder will normally have 4 counts per pulse so a 100 PPR encoder would be 400 counts per revolution. In *.counter-mode* a 100 PPR encoder would have 100 counts per revelution as it only uses the rising edge of A and direction is B.

-   *vel-timeout* - (Float, RW) When the encoder is moving slower than one pulse for each time that the driver reads the count from the FPGA (in the hm2\_read() function), the velocity is harder to estimate. The driver can wait several iterations for the next pulse to arrive, all the while reporting the upper bound of the encoder velocity, which can be accurately guessed. This parameter specifies how long to wait for the next pulse, before reporting the encoder stopped. This parameter is in seconds.

### 5i25 Configuration

#### Firmware

The 5i25 firmware comes preloaded for the daughter card it is purchased with. So the *firmware=xxx.BIT* is not part of the hm2\_pci configuration string when using a 5i25.

#### Configuration

Example configurations of the 5i25/7i76 and 5i25/7i77 cards are included in the [Configuration Selector](#sub:configuration-selector).

If you like to roll your own configuration the following examples show how to load the drivers in the HAL file.

5i25 + 7i76 Card

    # load the generic driver
    loadrt hostmot2

    # load the PCI driver and configure
    loadrt hm2_pci config="num_encoders=1 num_stepgens=5 sserial_port_0=0XXX"

5i25 + 7i77 Card

    # load the generic driver
    loadrt hostmot2

    # load the PCI driver and configure
    loadrt hm2_pci config="num_encoders=6 num_pwmgens=6 sserial_port_0=0XXX"

#### SSERIAL Configuration

The *sserial\_port\_0=0XXX* configuration string sets some options for the smart serial daughter card. These options are specific for each daughter card. See the Mesa manual for more information on the exact usuage.

#### 7i77 Limits

The minlimit and maxlimit are bounds on the pin value (in this case the analog out value) fullscalemax is the scale factor.

These are by default set to the analog in or analog range (most likely in volts).

So for example on the 7I77 +-10V analog outputs, the default values are:

minlimit -10 maxlimit +10 maxfullscale 10

If you wanted to say scale the analog out of a channel to IPS for a velocity mode servo (say 24 IPS max) you could set the limits like this:

minlimit -24 maxlimit +24 maxfullscale 24

If you wanted to scale the analog out of a channel to RPM for a 0 to 6000 RPM spindle with 0-10V control you could set the limits like this:

minlimit 0 maxlimit 6000 maxfullscale 6000 (this would prevent unwanted negative output voltages from being set)

### Example Configurations

Several example configurations for Mesa hardware are included with Machinekit. The configurations are located in the hm2-servo and hm2-stepper sections of the [Configuration Selector](#sub:configuration-selector). Typically you will need the board installed for the configuration you pick to load. The examples are a good place to start and will save you time. Just pick the proper example from the Machinekit Configuration Selector and save a copy to your computer so you can edit it. To see the exact pins and parameters that your configuration gave you, open the Show HAL Configuration window from the Machine menu, or do dmesg as outlined above.

Motenc Driver
-------------

<span id="cha:montec-driver"></span>

Vital Systems Motenc-100 and Motenc-LITE

The Vital Systems Motenc-100 and Motenc-LITE are 8- and 4-channel servo control boards. The Motenc-100 provides 8 quadrature encoder counters, 8 analog inputs, 8 analog outputs, 64 (68?) digital inputs, and 32 digital outputs. The Motenc-LITE has only 4 encoder counters, 32 digital inputs and 16 digital outputs, but it still has 8 analog inputs and 8 analog outputs. The driver automatically identifies the installed board and exports the appropriate HAL objects.

Installing:

    loadrt hal_motenc

During loading (or attempted loading) the driver prints some useful debugging messages to the kernel log, which can be viewed with dmesg.

Up to 4 boards may be used in one system.

### Pins

In the following pins, parameters, and functions, &lt;board&gt; is the board ID. According to the naming conventions the first board should always have an ID of zero. However this driver sets the ID based on a pair of jumpers on the board, so it may be non-zero even if there is only one board.

-   *(s32) motenc.&lt;board&gt;.enc-&lt;channel&gt;-count* - Encoder position, in counts.

-   *(float) motenc.&lt;board&gt;.enc-&lt;channel&gt;-position* - Encoder position, in user units.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-index* - Current status of index pulse input.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-idx-latch* - Driver sets this pin true when it latches an index pulse (enabled by latch-index). Cleared by clearing latch-index.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-latch-index* - If this pin is true, the driver will reset the counter on the next index pulse.

-   *(bit) motenc.&lt;board&gt;.enc-&lt;channel&gt;-reset-count* - If this pin is true, the counter will immediately be reset to zero, and the pin will be cleared.

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-value* - Analog output value for DAC (in user units, see -gain and -offset)

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-value* - Analog input value read by ADC (in user units, see -gain and -offset)

-   *(bit) motenc.&lt;board&gt;.in-&lt;channel&gt;* - State of digital input pin, see canonical digital input.

-   *(bit) motenc.&lt;board&gt;.in-&lt;channel&gt;-not* - Inverted state of digital input pin, see canonical digital input.

-   *(bit) motenc.&lt;board&gt;.out-&lt;channel&gt;* - Value to be written to digital output, seen canonical digital output.

-   *(bit) motenc.&lt;board&gt;.estop-in* - Dedicated estop input, more details needed.

-   *(bit) motenc.&lt;board&gt;.estop-in-not* - Inverted state of dedicated estop input.

-   *(bit) motenc.&lt;board&gt;.watchdog-reset* - Bidirectional, - Set TRUE to reset watchdog once, is automatically cleared.

### Parameters

-   *(float) motenc.&lt;board&gt;.enc-&lt;channel&gt;-scale* - The number of counts / user unit (to convert from counts to units).

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-offset* - Sets the DAC offset.

-   *(float) motenc.&lt;board&gt;.dac-&lt;channel&gt;-gain* - Sets the DAC gain (scaling).

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-offset* - Sets the ADC offset.

-   *(float) motenc.&lt;board&gt;.adc-&lt;channel&gt;-gain* - Sets the ADC gain (scaling).

-   *(bit) motenc.&lt;board&gt;.out-&lt;channel&gt;-invert* - Inverts a digital output, see canonical digital output.

-   *(u32) motenc.&lt;board&gt;.watchdog-control* - Configures the watchdog. The value may be a bitwise OR of the following values:

<table>
<colgroup>
<col width="12%" />
<col width="12%" />
<col width="75%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Bit #</th>
<th align="left">Value</th>
<th align="left">Meaning</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>0</p></td>
<td align="left"><p>1</p></td>
<td align="left"><p>Timeout is 16ms if set, 8ms if unset</p></td>
</tr>
<tr class="even">
<td align="left"><p>1</p></td>
<td align="left"><p>2</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>2</p></td>
<td align="left"><p>4</p></td>
<td align="left"><p>Watchdog is enabled</p></td>
</tr>
<tr class="even">
<td align="left"><p>3</p></td>
<td align="left"><p>8</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>4</p></td>
<td align="left"><p>16</p></td>
<td align="left"><p>Watchdog is automatically reset by DAC writes (the HAL dac-write function)</p></td>
</tr>
</tbody>
</table>

Typically, the useful values are 0 (watchdog disabled) or 20 (8ms watchdog enabled, cleared by dac-write).

-   *(u32) motenc.&lt;board&gt;.led-view* - Maps some of the I/O to onboard LEDs.

### Functions

-   *(funct) motenc.&lt;board&gt;.encoder-read* - Reads all encoder counters.

-   *(funct) motenc.&lt;board&gt;.adc-read* - Reads the analog-to-digital converters.

-   *(funct) motenc.&lt;board&gt;.digital-in-read* - Reads digital inputs.

-   *(funct) motenc.&lt;board&gt;.dac-write* - Writes the voltages to the DACs.

-   *(funct) motenc.&lt;board&gt;.digital-out-write* - Writes digital outputs.

-   *(funct) motenc.&lt;board&gt;.misc-update* - Updates misc stuff.

Opto22 Driver
-------------

<span id="cha:opto22-driver"></span>

PCI AC5 ADAPTER CARD / HAL DRIVER

### The Adapter Card

This is a card made by Opto22 for adapting the PCI port to solid state relay racks such as their standard or G4 series. It has 2 ports that can control up to 24 points each and has 4 on board LEDs. The ports use 50 pin connectors the same as Mesa boards. Any relay racks/breakout boards thats work with Mesa Cards should work with this card with the understanding any encoder counters, PWM, etc., would have to be done in software. The AC5 does not have any *smart* logic on board, it is just an adapter.

See the manufacturer’s website for more info:

<http://www.opto22.com/site/pr_details.aspx?cid=4&item=PCI-AC5>

I would like to thank Opto22 for releasing info in their manual, easing the writing of this driver!

### The Driver

This driver is for the PCI AC5 card and will not work with the ISA AC5 card. The HAL driver is a realtime module. It will support 4 cards as is (more cards are possible with a change in the source code). Load the basic driver like so:

    loadrt opto_ac5

This will load the driver which will search for max 4 boards. It will set I/O of each board’s 2 ports to a default setting. The default configuration is for 12 inputs then 12 outputs. The pin name numbers correspond to the position on the relay rack. For example the pin names for the default I/O setting of port 0 would be:

-   *opto\_ac5.0.port0.in-00* - They would be numbered from 00 to 11

-   *opto\_ac5.0.port0.out-12* - They would be numbered 12 to 23 port 1 would be the same.

### Pins

-   *opto\_ac5.\[BOARDNUMBER\].port\[PORTNUMBER\].in-\[PINNUMBER\] OUT bit* -

-   *opto\_ac5.\[BOARDNUMBER\].port\[PORTNUMBER\].in-\[PINNUMBER\]-not OUT bit* - Connect a HAL bit signal to this pin to read an I/O point from the card. The PINNUMBER represents the position in the relay rack. Eg. PINNUMBER 0 is position 0 in a Opto22 relay rack and would be pin 47 on the 50 pin header connector. The -not pin is inverted so that LOW gives TRUE and HIGH gives FALSE.

-   *opto\_ac5.\[BOARDNUMBER\].port\[PORTNUMBER\].out-\[PINNUMBER\] IN bit* - Connect a HAL bit signal to this pin to write to an I/O point of the card. The PINNUMBER represents the position in the relay rack.Eg. PINNUMBER 23 is position 23 in a Opto22 relay rack and would be pin 1 on the 50 pin header connector.

-   *opto\_ac5.\[BOARDNUMBER\].led\[NUMBER\] OUT bit* - Turns one of the 4 onboard LEDs on/off. LEDs are numbered 0 to 3.

BOARDNUMBER can be 0-3 PORTNUMBER can be 0 or 1. Port 0 is closest to the card bracket.

### Parameters

-   *opto\_ac5.\[BOARDNUMBER\].port\[PORTNUMBER\].out-\[PINNUMBER\]-invert W bit* - When TRUE, invert the meaning of the corresponding -out pin so that TRUE gives LOW and FALSE gives HIGH.

### FUNCTIONS

-   *opto\_ac5.0.digital-read* - Add this to a thread to read all the input points.

-   *opto\_ac5.0.digital-write* - Add this to a thread to write all the output points and LEDs.

For example the pin names for the default I/O setting of port 0 would be:

    opto_ac5.0.port0.in-00

They would be numbered from 00 to 11

    opto_ac5.0.port0.out-12

They would be numbered 12 to 23 port 1 would be the same.

### Configuring I/O Ports

To change the default setting load the driver something like so:

    loadrt opto_ac5 portconfig0=0xffff portconfig1=0xff0000

Of course changing the numbers to match the I/O you would like. Each port can be set up different.

Here’s how to figure out the number: The configuration number represents a 32 bit long code to tell the card which I/O points are output vrs input. The lower 24 bits are the I/O points of one port. The 2 highest bits are for 2 of the on board LEDs. A one in any bit position makes the I/O point an output. The two highest bits must be output for the LEDs to work. The driver will automatically set the two highest bits for you, we won’t talk about them.

The easiest way to do this is to fire up the calculator under APPLICATIONS/ACCESSORIES. Set it to scientific (click view). Set it BINARY (radio button Bin). Press 1 for every output you want and/or zero for every input. Remember that HAL pin 00 corresponds to the rightmost bit. 24 numbers represent the 24 I/O points of one port. So for the default setting (12 inputs then 12 outputs) you would push 1 twelve times (thats the outputs) then 0 twelve times (thats the inputs). Notice the first I/O point is the lowest (rightmost) bit. (that bit corresponds to HAL pin 00 .looks backwards) You should have 24 digits on the screen. Now push the Hex radio button. The displayed number (fff000) is the configport number ( put a *0x* in front of it designating it as a HEX number).

Another example: To set the port for 8 outputs and 16 inputs (the same as a Mesa card). Here is the 24 bits represented in a BINARY number. Bit 1 is the rightmost number.

000000000000000011111111

16 zeros for the 16 inputs and 8 ones for the 8 outputs

Which converts to FF on the calculator so 0xff is the number to use for portconfig0 and/or portconfig1 when loading the driver.

### Pin Numbering

HAL pin 00 corresponds to bit 1 (the rightmost) which represents position 0 on an Opto22 relay rack. HAL pin 01 corresponds to bit 2 (one spot to the left of the rightmost) which represents position 1 on an Opto22 relay rack. HAL pin 23 corresponds to bit 24 (the leftmost) which represents position 23 on an Opto22 relay rack.

HAL pin 00 connects to pin 47 on the 50 pin connector of each port. HAL pin 01 connects to pin 45 on the 50 pin connector of each port. HAL pin 23 connects to pin 1 on the 50 pin connector of each port.

Note that Opto22 and Mesa use opposite numbering systems: Opto22 position 23 = connector pin 1, and the position goes down as the connector pin number goes up. Mesa Hostmot2 position 1 = connector pin 1, and the position number goes up as the connector pin number goes up.

Pico Drivers
------------

<span id="cha:pico-drivers"></span>

Pico Systems has a family of boards for doing analog servo, stepper, and PWM (digital) servo control. The boards connect to the PC through a parallel port working in EPP mode. Although most users connect one board to a parallel port, in theory any mix of up to 8 or 16 boards can be used on a single parport. One driver serves all types of boards. The final mix of I/O depends on the connected board(s). The driver doesn’t distinguish between boards, it simply numbers I/O channels (encoders, etc) starting from 0 on the first board. The driver is named hal\_ppmc.ko The analog servo interface is also called the PPMC for Parallel Port Motion Control. There is also the Universal Stepper Controller, abbreviated the USC. And the Universal PWM Controller, or UPC.

Installing:

    loadrt hal_ppmc port_addr=<addr1>[,<addr2>[,<addr3>...]]

The *port\_addr* parameter tells the driver what parallel port(s) to check. By default, *&lt;addr1&gt;* is 0x0378, and *&lt;addr2&gt;* and following are not used. The driver searches the entire address space of the enhanced parallel port(s) at *port\_addr*, looking for any board(s) in the PPMC family. It then exports HAL pins for whatever it finds. During loading (or attempted loading) the driver prints some useful debugging messages to the kernel log, which can be viewed with *dmesg*.

Up to 3 parport busses may be used, and each bus may have up to 8 (or possibly 16 PPMC) devices on it.

### Command Line Options

There are several options that can be specified on the loadrt command line. First, the USC and UPC can have an 8-bit DAC added for spindle speed control and similar functions. This can be specified with the extradac=*0xnn\[,0xmm\]* parameter. The part enclosed in \[ \] allows you to specify this option on more than one board of the system. The first hex digit tells which EPP bus is being referred to, it corresponds to the order of the port addresses in the port\_addr parameter, where &lt;addr1&gt; would be zero here. So, for the first EPP bus, the first USC or UPC board would be described as *0x00*, the second USC or UPC on the same bus would be *0x02*. (Note that each USC or UPC takes up two addresses, so if one is at 00, the next would have to be 02.)

Alternatively, the 8 digital output pins can be used as additional digital outputs, it works the same way as above with the syntax : extradout=0xnn'. The extradac and extradout options are mutually exclusive on each board, you can only specify one.

The UPC and PPMC encoder boards can timestamp the arrival of encoder counts to refine the derivation of axis velocity. This derived velocity can be fed to the PID hal component to produce smoother D term response. The syntax is : timestamp=*0xnn\[,0xmm\]*, this works the same way as above to select which board is being configured. Default is to not enable the timestamp option. If you put this option on the command line, it enables the option. The first *n* selects the EPP bus, the second one matches the address of the board having the option enabled. The driver checks the revision level of the board to make sure it has firmware supporting the feature, and produces an error message if the board does not support it.

The PPMC encoder board has an option to select the encoder digital filter frequeency. (The UPC has the same ability via DIP switches on the board.) Since the PPMC encoder board doesn’t have these extra DIP switches, it needs to be selected via a command-line option. By default, the filter runs at 1 MHz, allowing encoders to be counted up to about 900 KHz (depending on noise and quadrature accuracy of the encoder.) The options are 1, 2.5, 5 and 10 MHz. These are set with a parameter of 1,2,5 and 10 (decimal) which is specified as the hex digit "A". These are specified in a manner similar to the above options, but with the frequency setting to the left of the bus/address digits. So, to set 5 MHz on the encoder board at address 3 on the first EPP bus, you would write : enc\_clock=*0x503*

### Pins

In the following pins, parameters, and functions, &lt;port&gt; is the parallel port ID. According to the naming conventions the first port should always have an ID of zero. All the boards have some method of setting the address on the EPP bus. USC and UPC have simple provisions for only two addresses, but jumper foil cuts allow up to 4 boards to be addressed. The PPMC boards have 16 possible addresses. In all cases, the driver enumerates the boards by type and exports the appropriate HAL pins. For instance, the encoders will be enumerated from zero up, in the same order as the address switches on the board specify. So, the first board will have encoders 0 — 3, the second board would have encoders 4 — 7. The first column after the bullet tells which boards will have this HAL pin or parameter associated with it. All means that this pin is available on all three board types. Option means that this pin will only be exported when that option is enabled by an optional parameter in the loadrt HAL command. These options require the board to have a sufficient revision level to support the feature.

-   *(All s32 output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.count* - Encoder position, in counts.

-   *(All s32 output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.delta* - Change in counts since last read, in raw encoder count units.

-   *(All float output) 'ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.velocity* - Velocity scaled in user units per second. On PPMC and USC this is derived from raw encoder counts per servo period, and hence is affected by encoder granularity. On UPC boards with the 8/21/09 and later firmware, velocity estimation by timestamping encoder counts can be used to improve the smoothness of this velocity output. This can be fed to the PID HAL component to produce a more stable servo response. This function has to be enabled in the HAL command line that starts the PPMC driver, with the timestamp=0x00 option.

-   *(All float output) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.position* - Encoder position, in user units.

-   *(All bit bidir) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.index-enable* - Connect to axis.\#.index-enable for home-to-index. This is a bidirectional HAL signal. Setting it to true causes the encoder hardware to reset the count to zero on the next encoder index pulse. The driver will detect this and set the signal back to false.

-   *(PPMC float output) ppmc.&lt;port&gt;.DAC.&lt;channel&gt;.value* - sends a signed value to the 16-bit Digital to Analog Converter on the PPMC DAC16 board commanding the analog output voltage of that DAC channel.

-   *(UPC bit input) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.enable* - Enables a PWM generator.

-   *(UPC float input) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.value* - Value which determines the duty cycle of the PWM waveforms. The value is divided by *pwm.&lt;channel&gt;.scale*, and if the result is 0.6 the duty cycle will be 60%, and so on. Negative values result in the duty cycle being based on the absolute value, and the direction pin is set to indicate negative.

-   *(USC bit input) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.enable* - Enables a step pulse generator.

-   *(USC float input) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.velocity* - Value which determines the step frequency. The value is multiplied by *stepgen.&lt;channel&gt;.scale* , and the result is the frequency in steps per second. Negative values result in the frequency being based on the absolute value, and the direction pin is set to indicate negative.

-   *(All bit output) ppmc.&lt;port&gt;.din.&lt;channel&gt;.in* - State of digital input pin, see canonical digital input.

-   *(All bit output) ppmc.&lt;port&gt;.din.&lt;channel&gt;.in-not* - Inverted state of digital input pin, see canonical digital input.

-   *(All bit input) ppmc.&lt;port&gt;.dout.&lt;channel&gt;.out* - Value to be written to digital output, see canonical digital output.

-   *(Option float input) ppmc.&lt;port&gt;.DAC8-&lt;channel&gt;.value* - Value to be written to analog output, range from 0 to 255. This sends 8 output bits to J8, which should have a Spindle DAC board connected to it. 0 corresponds to zero Volts, 255 corresponds to 10 Volts. The polarity of the output can be set for always minus, always plus, or can be controlled by the state of SSR1 (plus when on) and SSR2 (minus when on). You must specify extradac = 0x00 on the HAL command line that loads the PPMC driver to enable this function on the first USC ur UPC board.

-   *(Option bit input) ppmc.&lt;port&gt;.dout.&lt;channel&gt;.out* - Value to be written to one of the 8 extra digital output pins on J8. You must specify extradout = 0x00 on the HAL command line that loads the ppmc driver to enable this function on the first USC or UPC board. extradac and extradout are mutually exclusive features as they use the same signal lines for different purposes. These output pins will be enumerated after the standard digital outputs of the board.

### Parameters

-   *(All float) ppmc.&lt;port&gt;.encoder.&lt;channel&gt;.scale* - The number of counts / user unit (to convert from counts to units).

-   *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel-range&gt;.freq* - The PWM carrier frequency, in Hz. Applies to a group of four consecutive PWM generators, as indicated by *&lt;channel-range&gt;*. Minimum is 610Hz, maximum is 500KHz.

-   *(PPMC float) ppmc.&lt;port&gt;.DAC.&lt;channel&gt;.scale* - Sets scale of DAC16 output channel such that an output value equal to the 1/scale value will produce an output of + or - value Volts. So, if the scale parameter is 0.1 and you send a value of 0.5, the output will be 5.0 Volts.

-   *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.scale* - Scaling for PWM generator. If *scale* is X, then the duty cycle will be 100% when the *value* pin is X (or -X).

-   *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.max-dc* - Maximum duty cycle, from 0.0 to 1.0.

-   *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.min-dc* - Minimum duty cycle, from 0.0 to 1.0.

-   *(UPC float) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.duty-cycle* - Actual duty cycle (used mostly for troubleshooting.)

-   *(UPC bit) ppmc.&lt;port&gt;.pwm.&lt;channel&gt;.bootstrap* - If true, the PWM generator will generate a short sequence of pulses of both polarities when E-stop goes false, to reset the shutdown latches on some PWM servo drives.

-   *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.setup-time* - Sets minimum time between direction change and step pulse, in units of 100ns. Applies to a group of four consecutive step generators, as indicated by *&lt;channel-range&gt;*. Values between 200 ns and 25.5 us can be specified.

-   *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.pulse-width* - Sets width of step pulses, in units of 100ns. Applies to a group of four consecutive step generators, as indicated by *&lt;channel-range&gt;*. Values between 200 ns and 25.5 us may be specified.

-   *(USC u32) ppmc.&lt;port&gt;.stepgen.&lt;channel-range&gt;.pulse-space-min* - Sets minimum time between pulses, in units of 100ns. Applies to a group of four consecutive step generators, as indicated by *&lt;channel-range&gt;*. Values between 200 ns and 25.5 us can be specified. The maximum step rate is: <span class="image"> ![images/pico-ppmc-math.png]() </span>

-   *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.scale* - Scaling for step pulse generator. The step frequency in Hz is the absolute value of *velocity* \* *scale*.

-   *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.max-vel* - The maximum value for *velocity*. Commands greater than *max-vel* will be clamped. Also applies to negative values. (The absolute value is clamped.)

-   *(USC float) ppmc.&lt;port&gt;.stepgen.&lt;channel&gt;.frequency* - Actual step pulse frequency in Hz (used mostly for troubleshooting.)

-   *(Option float) ppmc.&lt;port&gt;.DAC8.&lt;channel&gt;.scale* - Sets scale of extra DAC output such that an output value equal to scale gives a magnitude of 10.0 V output. (The sign of the output is set by jumpers and/or other digital outputs.)

-   *(Option bit) ppmc.&lt;port&gt;.dout.&lt;channel&gt;.invert* - Inverts a digital output, see canonical digital output.

-   *(Option bit) ppmc.&lt;port&gt;.dout.&lt;channel&gt;.invert* - Inverts a digital output pin of J8, see canonical digital output.

### Functions

-   *(All funct) ppmc.&lt;port&gt;.read* - Reads all inputs (digital inputs and encoder counters) on one port. These reads are organized into blocks of contiguous registers to be read in a block to minimize CPU overhead.

-   *(All funct) ppmc.&lt;port&gt;.write* - Writes all outputs (digital outputs, stepgens, PWMs) on one port. These writes are organized into blocks of contiguous registers to be written in a block to minimize CPU overhead.

Pluto P Driver
--------------

<span id="cha:pluto-p-driver"></span>

### General Info

The Pluto-P is a FPGA board featuring the ACEX1K chip from Altera.

#### Requirements

1.  A Pluto-P board

2.  An EPP-compatible parallel port, configured for EPP mode in the system BIOS or a PCI EPP compatible parallel port card.

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
<td align="left">The Pluto P board requires EPP mode. Netmos98xx chips do not work in EPP mode. The Pluto P board will work on some computers and not on others. There is no known pattern to which computers work and which don’t work.</td>
</tr>
</tbody>
</table>

For more information on PCI EPP compatible parallel port cards see the [Machinekit Supported Hardware](http://wiki.machinekit.org/cgi-bin/wiki.pl?Machinekit_Supported_Hardware) page on the wiki.

#### Connectors

-   The Pluto-P board is shipped with the left connector presoldered, with the key in the indicated position. The other connectors are unpopulated. There does not seem to be a standard 12-pin IDC connector, but some of the pins of a 16P connector can hang off the board next to QA3/QZ3.

-   The bottom and right connectors are on the same .1" grid, but the left connector is not. If OUT2…OUT9 are not required, a single IDC connector can span the bottom connector and the bottom two rows of the right connector.

#### Physical Pins

-   Read the ACEX1K datasheet for information about input and output voltage thresholds. The pins are all configured in *LVTTL/LVCMOS* mode and are generally compatible with 5V TTL logic.

-   Before configuration and after properly exiting Machinekit, all Pluto-P pins are tristated with weak pull-ups (20k-ohms min, 50k-ohms max). If the watchdog timer is enabled (the default), these pins are also tristated after an interruption of communication between Machinekit and the board. The watchdog timer takes approximately 6.5ms to activate. However, software bugs in the pluto\_servo firmware or Machinekit can leave the Pluto-P pins in an undefined state.

-   In pwm+dir mode, by default dir is HIGH for negative values and LOW for positive values. To select HIGH for positive values and LOW for negative values, set the corresponding dout-NN-invert parameter TRUE to invert the signal.

-   The index input is triggered on the rising edge. Initial testing has shown that the QZx inputs are particularly noise sensitive, due to being polled every 25ns. Digital filtering has been added to filter pulses shorter than 175ns (seven polling times). Additional external filtering on all input pins, such as a Schmitt buffer or inverter, RC filter, or differential receiver (if applicable) is recommended.

-   The IN1…IN7 pins have 22-ohm series resistors to their associated FPGA pins. No other pins have any sort of protection for out-of-spec voltages or currents. It is up to the integrator to add appropriate isolation and protection. Traditional parallel port optoisolator boards do not work with pluto\_servo due to the bidirectional nature of the EPP protocol.

#### LED

-   When the device is unprogrammed, the LED glows faintly. When the device is programmed, the LED glows according to the duty cycle of PWM0 (*LED* = *UP0* *xor* *DOWN0*) or STEPGEN0 (*LED* = *STEP0* *xor* *DIR0*).

#### Power

-   A small amount of current may be drawn from VCC. The available current depends on the unregulated DC input to the board. Alternately, regulated +3.3VDC may be supplied to the FPGA through these VCC pins. The required current is not yet known, but is probably around 50mA plus I/O current.

-   The regulator on the Pluto-P board is a low-dropout type. Supplying 5V at the power jack will allow the regulator to work properly.

#### PC interface

-   Only a single pluto\_servo or pluto\_step board is supported.

#### Rebuilding the FPGA firmware

The *src/hal/drivers/pluto\_servo\_firmware/* and *src/hal/drivers/pluto\_step\_firmware/* subdirectories contain the Verilog source code plus additional files used by Quartus for the FPGA firmwares. Altera’s Quartus II software is required to rebuild the FPGA firmware. To rebuild the firmware from the .hdl and other source files, open the *.qpf* file and press CTRL-L. Then, recompile Machinekit.

Like the HAL hardware driver, the FPGA firmware is licensed under the terms of the GNU General Public License.

The gratis version of Quartus II runs only on Microsoft Windows, although there is apparently a paid version that runs on Linux.

#### For more information

Some additional information about it is available from [KNJC LLC](http://www.knjn.com/FPGA-Parallel.html) and from [the developer’s blog](http://emergent.unpy.net/01165081407).

### Pluto Servo<span id="sec:pluto-servo"></span>

The pluto\_servo system is suitable for control of a 4-axis CNC mill with servo motors, a 3-axis mill with PWM spindle control, a lathe with spindle encoder, etc. The large number of inputs allows a full set of limit switches.

This driver features:

-   4 quadrature channels with 40MHz sample rate. The counters operate in *4x* mode. The maximum useful quadrature rate is 8191 counts per Machinekit servo cycle, or about 8MHz for Machinekit’s default 1ms servo rate.

-   4 PWM channels, *up/down* or *pwm+dir* style. 4095 duty cycles from -100% to +100%, including 0%. The PWM period is approximately 19.5kHz (40MHz / 2047). A PDM-like mode is also available.

-   18 digital outputs: 10 dedicated, 8 shared with PWM functions. (Example: A lathe with unidirectional PWM spindle control may use 13 total digital outputs)

-   20 digital inputs: 8 dedicated, 12 shared with Quadrature functions. (Example: A lathe with index pulse only on the spindle may use 13 total digital inputs)

-   EPP communication with the PC. The EPP communication typically takes around 100 us on machines tested so far, enabling servo rates above 1kHz.

#### Pinout

-   *UPx* - The *up* (up/down mode) or *pwm* (pwm+direction mode) signal from PWM generator X. May be used as a digital output if the corresponding PWM channel is unused, or the output on the channel is always negative. The corresponding digital output invert may be set to TRUE to make UPx active low rather than active high.

-   *DNx* - The *down* (up/down mode) or *direction* (pwm+direction mode) signal from PWM generator X. May be used as a digital output if the corresponding PWM channel is unused, or the output on the channel is never negative. The corresponding digital ouput invert may be set to TRUE to make DNx active low rather than active high.

-   *QAx, QBx* - The A and B signals for Quadrature counter X. May be used as a digital input if the corresponding quadrature channel is unused.

-   *QZx* - The Z (index) signal for quadrature counter X. May be used as a digital input if the index feature of the corresponding quadrature channel is unused.

-   *INx* - Dedicated digital input \#x

-   *OUTx* - Dedicated digital output \#x

-   *GND* - Ground

-   *VCC* - +3.3V regulated DC

![images/pluto-pinout.png]()

Figure 26. Pluto-Servo Pinout<span id="fig:Pluto-Servo-Pinout"></span>

<table>
<caption>Table 4. Pluto-Servo Alternate Pin Functions<span id="table:Pluto-Servo-Alternate-Pin"></span></caption>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Primary function</th>
<th align="left">Alternate Function</th>
<th align="left">Behavior if both functions used</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>UP0</strong></p></td>
<td align="left"><p>PWM0</p></td>
<td align="left"><p>When pwm-0-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT10</p></td>
<td align="left"><p>XOR’d with UP0 or PWM0</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP1</strong></p></td>
<td align="left"><p>PWM1</p></td>
<td align="left"><p>When pwm-1-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT12</p></td>
<td align="left"><p>XOR’d with UP1 or PWM1</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP2</strong></p></td>
<td align="left"><p>PWM2</p></td>
<td align="left"><p>When pwm-2-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT14</p></td>
<td align="left"><p>XOR’d with UP2 or PWM2</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>UP3</strong></p></td>
<td align="left"><p>PWM3</p></td>
<td align="left"><p>When pwm-3-pwmdir is TRUE, this pin is the PWM output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT16</p></td>
<td align="left"><p>XOR’d with UP3 or PWM3</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN0</strong></p></td>
<td align="left"><p>DIR0</p></td>
<td align="left"><p>When pwm-0-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT11</p></td>
<td align="left"><p>XOR’d with DN0 or DIR0</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN1</strong></p></td>
<td align="left"><p>DIR1</p></td>
<td align="left"><p>When pwm-1-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT13</p></td>
<td align="left"><p>XOR’d with DN1 or DIR1</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN2</strong></p></td>
<td align="left"><p>DIR2</p></td>
<td align="left"><p>When pwm-2-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT15</p></td>
<td align="left"><p>XOR’d with DN2 or DIR2</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>DN3</strong></p></td>
<td align="left"><p>DIR3</p></td>
<td align="left"><p>When pwm-3-pwmdir is TRUE, this pin is the DIR output</p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p>OUT17</p></td>
<td align="left"><p>XOR’d with DN3 or DIR3</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QZ0</strong></p></td>
<td align="left"><p>IN8</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QZ1</strong></p></td>
<td align="left"><p>IN9</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QZ2</strong></p></td>
<td align="left"><p>IN10</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QZ3</strong></p></td>
<td align="left"><p>IN11</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QA0</strong></p></td>
<td align="left"><p>IN12</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QA1</strong></p></td>
<td align="left"><p>IN13</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QA2</strong></p></td>
<td align="left"><p>IN14</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QA3</strong></p></td>
<td align="left"><p>IN15</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QB0</strong></p></td>
<td align="left"><p>IN16</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QB1</strong></p></td>
<td align="left"><p>IN17</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>QB2</strong></p></td>
<td align="left"><p>IN18</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>QB3</strong></p></td>
<td align="left"><p>IN19</p></td>
<td align="left"><p>Read same value</p></td>
</tr>
</tbody>
</table>

#### Input latching and output updating

-   PWM duty cycles for each channel are updated at different times.

-   Digital outputs OUT0 through OUT9 are all updated at the same time. Digital outputs OUT10 through OUT17 are updated at the same time as the pwm function they are shared with.

-   Digital inputs IN0 through IN19 are all latched at the same time.

-   Quadrature positions for each channel are latched at different times.

#### HAL Functions, Pins and Parameters

A list of all *loadrt* arguments, HAL function names, pin names and parameter names is in the manual page, *pluto\_servo.9*.

#### Compatible driver hardware

A schematic for a 2A, 2-axis PWM servo amplifier board is available from the ([the software developer](http://emergent.unpy.net/projects/01148303608)). The L298 H-Bridge can be used for motors up to 4A (one motor per L298) or up to 2A (two motors per L298) with the supply voltage up to 46V. However, the L298 does not have built-in current limiting, a problem for motors with high stall currents. For higher currents and voltages, some users have reported success with International Rectifier’s integrated high-side/low-side drivers.

### Pluto Step<span id="sec:Pluto-step:-Hardware-step"></span>

Pluto-step is suitable for control of a 3- or 4-axis CNC mill with stepper motors. The large number of inputs allows for a full set of limit switches.

The board features:

-   4 *step+direction* channels with 312.5kHz maximum step rate, programmable step length, space, and direction change times

-   14 dedicated digital outputs

-   16 dedicated digital inputs

-   EPP communication with the PC

#### Pinout

-   *STEPx* - The *step* (clock) output of stepgen channel *x*

-   *DIRx* - The *direction* output of stepgen channel *x*

-   *INx* - Dedicated digital input \#x

-   *OUTx* - Dedicated digital output \#x

-   *GND* - Ground

-   *VCC* - +3.3V regulated DC

While the *extended main connector* has a superset of signals usually found on a Step & Direction DB25 connector—4 step generators, 9 inputs, and 6 general-purpose outputs—the layout on this header is different than the layout of a standard 26-pin ribbon cable to DB25 connector.

![images/pluto-step-pinout.png]()

Figure 27. Pluto-Step Pinout<span id="fig:Pluto-Step-Pinout"></span>

#### Input latching and output updating

-   Step frequencies for each channel are updated at different times.

-   Digital outputs are all updated at the same time.

-   Digital inputs are all latched at the same time.

-   Feedback positions for each channel are latched at different times.

#### Step Waveform Timings

The firmware and driver enforce step length, space, and direction change times. Timings are rounded up to the next multiple of 1.6μs, with a maximum of 49.6μs. The timings are the same as for the software stepgen component, except that *dirhold* and *dirsetup* have been merged into a single parameter *dirtime* which should be the maximum of the two, and that the same step timings are always applied to all channels.

![images/pluto\_step\_waveform.png]()

Figure 28. Pluto-Step Timings<span id="fig:Pluto-Step-Timings"></span>

#### HAL Functions, Pins and Parameters

A list of all *loadrt* arguments, HAL function names, pin names and parameter names is in the manual page, *pluto\_step.9*.

Servo To Go Driver
------------------

<span id="cha:servo-to-go-driver"></span>

The Servo-To-Go (STG) is one of the first PC motion control cards supported by Machinekit. It is an ISA card and it exists in different flavors (all supported by this driver). The board includes up to 8 channels of quadrature encoder input, 8 channels of analog input and output, 32 bits digital I/O, an interval timer with interrupt and a watchdog. For more information see the [Servo To Go](http://www.servotogo.com/) web page.

### Installing

    loadrt hal_stg [base=<address>] [num_chan=<nr>] [dio="<dio-string>"] [model=<model>]

The base address field is optional; if it’s not provided the driver attempts to autodetect the board. The num\_chan field is used to specify the number of channels available on the card, if not used the 8 axis version is assumed. The digital inputs/outputs configuration is determined by a config string passed to insmod when loading the module. The format consists of a four character string that sets the direction of each group of pins. Each character of the direction string is either "I" or "O". The first character sets the direction of port A (Port A - DIO.0-7), the next sets port B (Port B - DIO.8-15), the next sets port C (Port C - DIO.16-23), and the fourth sets port D (Port D - DIO.24-31). The model field can be used in case the driver doesn’t autodetect the right card version.

hint: after starting up the driver, *dmesg* can be consulted for messages relevant to the driver (e.g. autodetected version number and base address). For example:

    loadrt hal_stg base=0x300 num_chan=4 dio="IOIO"

This example installs the STG driver for a card found at the base address of 0x300, 4 channels of encoder feedback, DACs and ADCs, along with 32 bits of I/O configured like this: the first 8 (Port A) configured as Input, the next 8 (Port B) configured as Output, the next 8 (Port C) configured as Input, and the last 8 (Port D) configured as Output

    loadrt hal_stg

This example installs the driver and attempts to autodetect the board address and board model, it installs 8 axes by default along with a standard I/O setup: Port A & B configured as Input, Port C & D configured as Output.

### Pins

-   *stg.&lt;channel&gt;.counts* - (s32) Tracks the counted encoder ticks.

-   *stg.&lt;channel&gt;.position* - (float) Outputs a converted position.

-   *stg.&lt;channel&gt;.dac-value* - (float) Drives the voltage for the corresponding DAC.

-   *stg.&lt;channel&gt;.adc-value* - (float) Tracks the measured voltage from the corresponding ADC.

-   *stg.in-&lt;pinnum&gt;* - (bit) Tracks a physical input pin.

-   *stg.in-&lt;pinnum&gt;-not* - (bit) Tracks a physical input pin, but inverted.

-   *stg.out-&lt;pinnum&gt;* - (bit) Drives a physical output pin

For each pin, &lt;channel&gt; is the axis number, and &lt;pinnum&gt; is the logic pin number of the STG if `IIOO` is defined, there are 16 input pins (in-00 .. in-15) and 16 output pins (out-00 .. out-15), and they correspond to PORTs ABCD (in-00 is PORTA.0, out-15 is PORTD.7).

The in-&lt;pinnum&gt; HAL pin is TRUE if the physical pin is high, and FALSE if the physical pin is low. The in-&lt;pinnum&gt;-not HAL pin is inverted — it is FALSE if the physical pin is high. By connecting a signal to one or the other, the user can determine the state of the input.

### Parameters

-   *stg.&lt;channel&gt;.position-scale* - (float) The number of counts / user unit (to convert from counts to units).

-   *stg.&lt;channel&gt;.dac-offset* - (float) Sets the offset for the corresponding DAC.

-   *stg.&lt;channel&gt;.dac-gain* - (float) Sets the gain of the corresponding DAC.

-   *stg.&lt;channel&gt;.adc-offset* - (float) Sets the offset of the corresponding ADC.

-   *stg.&lt;channel&gt;.adc-gain* - (float) Sets the gain of the corresponding ADC.

-   *stg.out-&lt;pinnum&gt;-invert* - (bit) Inverts an output pin.

The -invert parameter determines whether an output pin is active high or active low. If -invert is FALSE, setting the HAL out- pin TRUE drives the physical pin high, and FALSE drives it low. If -invert is TRUE, then setting the HAL out- pin TRUE will drive the physical pin low.

#### Functions

-   *stg.capture-position* - Reads the encoder counters from the axis &lt;channel&gt;.

-   *stg.write-dacs* - Writes the voltages to the DACs.

-   *stg.read-adcs* - Reads the voltages from the ADCs.

-   *stg.di-read* - Reads physical in- pins of all ports and updates all HAL in-&lt;pinnum&gt; and in-&lt;pinnum&gt;-not pins.

-   *stg.do-write* - Reads all HAL out-&lt;pinnum&gt; pins and updates all physical output pins.

General Mechatronics Driver
---------------------------

<span id="cha:gm-driver"></span>

General Mechatronics GM6-PCI card based motion control system

For detailed description, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

The GM6-PCI motion control card is based on an FPGA and a PCI bridge interface ASIC. A small automated manufacturing cell can be controlled, with a short time system integration procedure. The following figure demonstrating the typical connection of devices related to the control system:

-   It can control up to six axis, each can be stepper or CAN bus interface or analogue servo.

-   GPIO: Four time eight I/O pins are placed on standard flat cable headers.

-   RS485 I/O expander modules: RS485 bus was designed for interfacing with compact DIN-rail mounted expander modules. An 8-channel digital input, an 8-channel relay output and an analogue I/O (4x +/-10 Volts output and 8x +/-5 Volts input) modules are available now. Up to 16 modules can be connected to the bus altogether.

-   20 optically isolated input pins: Six times three for the direct connection of two end switch and one homing sensor for each axis. And additionally, two optically isolated E-stop inputs.

![GM servo control system]()

Installing:

    loadrt hal_gm

During loading (or attempted loading) the driver prints some useful debugging messages to the kernel log, which can be viewed with dmesg.

Up to 3 boards may be used in one system.

The following connectors can be found on the GM6-PCI card:

![images/GM\_PCIpinout.png]()

Figure 29. GM6-PCI card connectors and LEDs<span id="fig:PCI-card-connectors"></span>

### I/O connectors

![images/GM\_IOpinout.png]()

Figure 30. Pin numbering of GPIO connectors<span id="fig:pin-numbering-gpio"></span>

<table>
<caption>Table 5. Pinout of GPIO connectors<span id="table:gpio-pinout"></span></caption>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">9</th>
<th align="left">7</th>
<th align="left">5</th>
<th align="left">3</th>
<th align="left">1</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>IOx/7</p></td>
<td align="left"><p>IOx/5</p></td>
<td align="left"><p>IOx/3</p></td>
<td align="left"><p>IOx/1</p></td>
<td align="left"><p>VCC</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">10</th>
<th align="left">8</th>
<th align="left">6</th>
<th align="left">4</th>
<th align="left">2</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>GND</p></td>
<td align="left"><p>IOx/6</p></td>
<td align="left"><p>IOx/4</p></td>
<td align="left"><p>IOx/2</p></td>
<td align="left"><p>IOx/0</p></td>
</tr>
</tbody>
</table>

Each pin can be configured as digital input or output. GM6-PCI motion control card has 4 general purpose I/O (GPIO) connectors, with eight configurable I/O on each. Every GPIO pin and parameter name begins as follows:

    gm.<nr. of card>.gpio.<nr of gpio con>

,where &lt;nr of gpio con&gt; is form 0 to 3. For example:

    gm.0.gpio.0.in-0

indicates the state of the first pin of the first GPIO connector on the GM6-PCI card. Hal pins are updated by function

    gm.<nr of card>.read

#### Pins

<table>
<caption>Table 6. GPIO pins<span id="table:gpio-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Output pin. Used only when GPIO is set to output.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 7. GPIO parameters<span id="table:gpio-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.is-out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, the corresponding GPIO is set to totem-pole output, other wise set to high impedance input.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, pin value will be inverted. Used when pin is configured as output.</p></td>
</tr>
</tbody>
</table>

### Axis connectors

![images/GM\_AXISpinout.png]()

Figure 31. Pin numbering of axis connectors<span id="fig:pin-numbering-axis"></span>

<table>
<caption>Table 8. Pinout of axis connectors<span id="table:axis-pinout"></span></caption>
<colgroup>
<col width="20%" />
<col width="80%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>Encoder A</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>+5 Volt (PC)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>Encoder B</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Encoder Index</p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Fault</p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Power Enabled</p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Step/CCW/B</p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>Direction/CW/A</p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>Ground (PC)</p></td>
</tr>
<tr class="even">
<td align="left"><p>10</p></td>
<td align="left"><p>DAC serial line</p></td>
</tr>
</tbody>
</table>

#### Axis interface modules

Small sized DIN rail mounted interface modules gives easy way of connecting different types of servo modules to the axis connectors. Seven different system configurations are presented in the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf) for evaluating typical applications. Also the detailed description of the Axis modules can be found in the System integration manual.

For evaluating the appropriate servo-drive structure the modules have to be connected as the following block diagram shows:

![images/GM\_AxisInterface.png]()

Figure 32. Servo axis interfaces<span id="fig:axis-iterface"></span>

#### Encoder

The GM6-PCI motion control card has six encoder modules. Each encoder module has three channels:

-   Channel-A

-   Channel-B

-   Channel-I (index)

It is able to count quadrature encoder signals or step/dir signals. Each encoder module is connected to the inputs of the corresponding RJ50 axis connector.

Every encoder pin and parameter name begins as follows:

    gm.<nr. of card>.encoder.<nr of axis>

,where &lt;nr of axis&gt; is form 0 to 5. For example:

    gm.0.encoder.0.position

refers to the position of encoder module of axis 0.

The GM6-PCI card counts the encoder signal independently from Machinekit. Hal pins are updated by function:

    gm.<nr of card>.read

##### Pins

<table>
<caption>Table 9. Encoder pins<span id="table:encoder-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.reset</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>When True, resets counts and position to zero.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.rawcounts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>The raw count is the counts, but unaffected by reset or the index pulse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.counts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position in encoder counts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position in scaled units (=.counts/.position-scale).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.index-enabled</p></td>
<td align="left"><p>(bit, IO)</p></td>
<td align="left"><p>When True, counts and position are rounded or reset (depends on index-mode) on next rising edge of channel-I. Every time position is reset because of Index, index-enabled pin is set to 0 and remain 0 until connected hal pin does not set it.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.velocity</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Velocity in scaled units per second. GM encoder uses high frequency hardware timer to measure time between encoder pulses in order to calculate velocity. It greatly reduces quantization noise as compared to simply differentiating the position output. When the measured velocity is below min-velocity-estimate, the velocity output is 0.</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 10. Encoder parameters<span id="table:encoder-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and Read/Write</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.counter-mode</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, the counter counts each rising edge of the channel-A input to the direction determined by channel-B. This is useful for counting the output of a single channel (non-quadrature) or step/dir signal sensor. When false, it counts in quadrature mode.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.index-mode</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True and .index-enabled is also true, .counts and .position are rounded (based on .counts-per-rev) at rising edge of channel-I. This is useful to correct few pulses error caused by noise. In round mode, it is essential to set .counts-per-rev parameter correctly. When .index-mode is False and .index-enabled is true, .counts and .position are reset at channel-I pulse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.counts-per-rev</p></td>
<td align="left"><p>(s32, R/V)</p></td>
<td align="left"><p>Determine how many counts are between two index pulses. It is used only in round mode, so when both .index-enabled and .index-mode parameters are True. GM encoder process encoder signal in 4x mode, so for example in case of a 500 CPR encoder it should be set to 2000. This parameter can be easily measured by setting .index-enabled True and .index-mode False (so that .counts resets at channel-I pulse), than move axis by hand and see the maximum magnitude of .counts pin in halmeter.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.index-invert</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, channel-I event (reset or round) occur on falling edge of channel-I signal, otherwise on rising edge.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.min-speed-estimate</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Determine the minimum measured velocity magnitude at which .velocity will be set as nonzero. Setting this parameter too low will cause it to take a long time for velocity to go to zero after encoder pulses have stopped arriving.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in counts per length unit. .position=.counts/.position-scale. For example, if position-scale is 2000, then 1000 counts of the encoder will produce a position of 0.5 units.</p></td>
</tr>
</tbody>
</table>

##### HAL example

Setting encoder module of axis 0 to receive 500 CPR quadrature encoder signal and use reset to round position.

    setp gm.0.encoder.0.counter-mode 0         # 0: quad, 1: stepDir
    setp gm.0.encoder.0.index-mode 1           # 0: reset pos at index, 1:round pos at index
    setp gm.0.encoder.0.counts-per-rev 2000      # GM process encoder in 4x mode, 4x500=2000
    setp gm.0.encoder.0.index-invert 0
    setp gm.0.encoder.0.min-speed-estimate 0.1 # in position unit/s
    setp gm.0.encoder.0.position-scale 20000   # 10 encoder rev cause the machine to
                                                 move one position unit (10x2000)

Connect encoder position to Machinekit position feedback:

    net Xpos-fb gm.0.encoder.0.position => axis.0.motor-pos-fb

#### Stepgen module

The GM6-PCI motion control card has six stepgen modules, one for each axis. Each module has two output signals. It can produce Step/Direction, Up/Down or Quadrature (A/B) pulses. Each stepgen module is connected to the pins of the corresponding RJ50 axis connector.

Every stepgen pin and parameter name begins as follows:

    gm.<nr. of card>.stepgen.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.stepgen.0.position-cmd

refers to the position command of stepgen module of axis 0 on card 0.

The GM6-PCI card generates step pulses independently from Machinekit. Hal pins are updated by function

    gm.<nr of card>.write

##### Pins

<table>
<caption>Table 11. Stepgen module pins<span id="table:stepgen-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Stepgen produces pulses only when this pin is true.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.count-fb</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position feedback in counts unit.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-fb</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position feedback in position unit.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded position in position units. Used in position mode only.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.velocity-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded velocity in position units per second. Used in velocity mode only.</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 12. Stepgen module parameters<span id="table:stepgen-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and Read/Write</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.step-type</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>When 0, module produces Step/Dir signal. When 1, it produces Up/Down step signals. And when it is 2, it produces quadrature output signals.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.control-type</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, .velocity-cmd is used as reference and velocity control calculate pulse rate output. When False, .position-cmd is used as reference and position control calculate pulse rate output.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.invert-step1</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Invert the output of channel 1 (Step signal in StepDir mode)</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-step2</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Invert the output of channel 2 (Dir signal in StepDir mode)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.maxvel</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum velocity in position units per second. If it is set to 0.0, .maxvel parameter is ignored.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.maxaccel</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum acceleration in position units per second squared. If it is set to 0.0, .maxaccel parameter is ignored.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in steps per length unit.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.steplen</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Length of step pulse in nano-seconds.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.stepspace</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Minimum time between two step pulses in nano-seconds.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dirdelay</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Minimum time between step pulse and direction change in nano-seconds.</p></td>
</tr>
</tbody>
</table>

For evaluating the appropriate values see the timing diagrams below:

![images/GM\_RefSignals.png]()

Figure 33. Reference signal timing diagrams<span id="fig:refsig-timing-diagram"></span>

##### HAL example

Setting stepgen module of axis 0 to generate 1000 step pulse per position unit:

    setp gm.0.stepgen.0.step-type 0         # 0:stepDir, 1:UpDown, 2:Quad
    setp gm.0.stepgen.0.control-type 0      # 0:Pos. control, 1:Vel. Control
    setp gm.0.stepgen.0.invert-step1 0
    setp gm.0.stepgen.0.invert-step2 0
    setp gm.0.stepgen.0.maxvel 0            # do not set maxvel for step
                                            # generator, let interpolator control it.
    setp gm.0.stepgen.0.maxaccel 0          # do not set max acceleration for
                                            # step generator, let interpolator control it.
    setp gm.0.stepgen.0.position-scale 1000 # 1000 step/position unit
    setp gm.0.stepgen.0.steplen 1000        # 1000 ns = 1 us
    setp gm.0.stepgen.0.stepspace1000       # 1000 ns = 1 us
    setp gm.0.stepgen.0.dirdelay 2000       # 2000 ns = 2 us

Connect stepgen to axis 0 position reference and enable pins:

    net Xpos-cmd axis.0.motor-pos-cmd => gm.0.stepgen.0.position-cmd
    net Xen axis.0.amp-enable-out => gm.0.stepgen.0.enable

#### Enable and Fault signals

The GM6-PCI motion control card has one enable output and one fault input HAL pins, both are connected to each RJ50 axis connector and to the CAN connector.

Hal pins are updated by function:

    gm.<nr of card>.read

##### Pins

<table>
<caption>Table 13. Enable and Fault signal pins<span id="table:enable-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.power-enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>If this pin is True,</p>
<p>* and Watch Dog Timer is not expired * and there is no power fault Then power enable pins of axis- and CAN connectors are set to high, otherwise set to low.</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.&lt;nr of card&gt;.power-fault</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Power fault input.</p></td>
</tr>
</tbody>
</table>

#### Axis DAC

The GM6-PCI motion control card has six serial axis DAC driver modules, one for each axis. Each module is connected to the pin of the corresponding RJ50 axis connector. Every axis DAC pin and parameter name begins as follows:

    gm.<nr. of card>.dac.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.dac.0.value

refers to the output voltage of DAC module of axis 0. Hal pins are updated by function:

    gm.<nr of card>.write

##### Pins

<table>
<caption>Table 14. Axis DAC pins<span id="table:dac-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable DAC output. When enable is false, DAC output is 0.0 V.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.value</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Value of DAC output in Volts.</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 15. Axis DAC parameters<span id="table:dac-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.offset</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is added to the value before the hardware is updated</p></td>
</tr>
<tr class="even">
<td align="left"><p>.high-limit</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.low-limit</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Minimum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-serial</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>GM6-PCI card is communicating with DAC hardware via fast serial communication to highly reduce time delay compared to PWM. DAC module is recommended to be isolated which is negating serial communication line. In case of isolation, leave this parameter to default (0), while in case of none-isolation, set this parameter to 1.</p></td>
</tr>
</tbody>
</table>

### CAN-bus servo amplifiers

The GM6-PCI motion control card has CAN module to drive CAN servo amplifiers. Implementation of higher level protocols like CANopen is further development. Currently GM produced power amplifiers has upper level driver which export pins and parameters to HAL. They receive position reference and provide encoder feedback via CAN bus.

The frames are standard (11 bit) ID frames, with 4 byte data length. Tha baud rate is 1 Mbit. The position commad IDs for axis 0..5 are 0x10..0x15. The position feedback IDs for axis 0..5 are 0x20..0x25.

These configuration can be changed with the modifivation of hal\_gm.c and recompiling Machinekit.

Every CAN pin and parameter name begins as follows:

    gm.<nr. of card>.can-gm.<nr of axis>

,where &lt;nr of axis&gt; is form 0 to 5. For example:

    gm.0.can-gm.0.position

refers to the output position of axis 0 in position units.

Hal pins are updated by function:

    gm.<nr of card>.write

#### Pins

<table>
<caption>Table 16. CAN module pins<span id="table:can-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable sending position references.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded position in position units.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-fb</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Feed back position in position units.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 17. CAN module parameters<span id="table:can-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in per length unit.</p></td>
</tr>
</tbody>
</table>

### Watchdog timer

Watchdog timer resets at function:

    gm.<nr of card>.read

#### Pins

<table>
<caption>Table 18. Watchdog pins<span id="table:watchdog-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-expired</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Indicates that watchdog timer is expired.</p></td>
</tr>
</tbody>
</table>

Watchdog timer overrun causes the set of power-enable to low in hardware.

#### Parameters

<table>
<caption>Table 19. Watchdog parameters<span id="table:watchdog-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-enable</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Enable watchdog timer. It is strongly recommended to enable watchdog timer, because it can disables all the servo amplifiers by pulling down all enable signal in case of PC error.</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-timeout-ns</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Time interval in within the gm.&lt;nr of card&gt;.read function must be executed. The gm.&lt;nr of card&gt;.read is typically added to servo-thread, so watch timeout is typically set to 3 times of the servo period.</p></td>
</tr>
</tbody>
</table>

### End-, homing- and E-stop switches

![images/GM\_ENDSWpinout.png]()

Figure 34. Pin numbering of homing & end switch connector<span id="fig:pin-numbering-endsw"></span>

Table 20. End- and homing switch connector pinout<span id="table:end-and-homing-switch-connector-pinout"></span>
**25**
**23**
**21**
**19**
**17**
**15**
**13**
**11**
**9**
**7**
**5**
**3**
**1**
GND

1/End-

2/End+

2/Hom-ing

3/End-

4/End+

4/Hom-ing

5/End-

6/End+

6/Hom-ing

E-Stop 2

V+ (Ext.)

**26**
**24**
**22**
**20**
**18**
**16**
**14**
**12**
**10**
**8**
**6**
**4**
**2**
GND

1/End+

1/Hom-ing

2/End-

3/End+

3/Hom-ing

4/End-

5/End+

5/Hom-ing

6/End-

E-Stop 1

V+ (Ext.)

The GM6-PCI motion control card has two limit- and one homing switch input for each axis. All the names of these pins begin as follows:

    gm.<nr. of card>.axis.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.axis.0.home-sw-in

indicates the state of the axis 0 home switch.

Hal pins are updated by function:

    gm.<nr of card>.read

#### Pins

<table>
<caption>Table 21. End- and homing switch pins<span id="table:end-and-homing-switch-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.home-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Home switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.home-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated home switch input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.neg-lim-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negative limit switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.neg-lim-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated negative limit switch input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.pos-lim-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Positive limit switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.pos-lim-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated positive limit switch input</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 22. E-stop switch parameters<span id="table:e-stop-switch-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.0.estop.0.in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Estop 0 input</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.0.estop.0.in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated Estop 0 input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>gm.0.estop.1.in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Estop 1 input</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.0.estop.1.in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated Estop 1 input</p></td>
</tr>
</tbody>
</table>

### Status LEDs

#### CAN

Color: Orange

-   Blink, during data communication.

-   On, when any of the buffers are full - communication error.

-   Off, when no data communication.

#### RS485

Color: Orange

-   Blink, during initialization of modules on the bus

-   On, when the data communication is up between all initialized modules.

-   Off, when any of the initialized modules dropped off because of an error.

#### EMC

Color: White

-   Blink, when Machinekit is running.

-   Otherwise off.

#### Boot

Color: Green

-   On, when system booted successfully.

-   Otherwise off.

#### Error

Color: Red

-   Off, when there is no fault in the system.

-   Blink, when PCI communication error.

-   On, when watchdog timer overflowed.

### RS485 I/O expander modules

These modules were developed for expanding the I/O and function capability along an RS485 line of the GM6-PCI motion control card.

Available module types:

-   8-channel relay output module - gives eight NO-NC relay output on a three pole terminal connector for each channel.

-   8-channel digital input module - gives eight optical isolated digital input pins.

-   8 channel ADC and 4-channel DAC module - gives four digital-to-analogue converter outputs and eight analogue-to-digital inputs. This module is also optically isolated from the GM6-PCI card.

**Automatic node recognizing:**

Each node connected to the bus was recognized by the GM6-PCI card automatically. During starting Machinekit, the driver export pins and parameters of all available modules automatically.

**Fault handling:**

If a module does not answer regularly the GM6-PCI card drops down the module. If a module with output do not gets data with correct CRC regularly, the module switch to error sate (green LED blinking), and turns all outputs to error sate.

**Connecting the nodes:**

The modules on the bus have to be connected in serial topology, with termination resistors on the end. The start of the topology is the PCI card, and the end is the last module.

![images/GM\_RS485topology.png]()

Figure 35. Connecting the RS485 nodes to the GM6-PCI card<span id="fig:connecting-rs485"></span>

**Adressing:**

Each node on the bus has a 4 bit unique address that can be set with a red DIP switch.

**Status LED:**

A green LED indicates the status of the module:

-   Blink, when the module is only powered, but not jet identified, or when module is dropped down.

-   Off, during identification (computer is on, but Machinekit not started)

-   On, when it communicates continuously.

#### Relay output module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

##### Pins

<table>
<caption>Table 23. Relay output module pins<span id="table:rs485-relay-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.relay-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Output pin for relay</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 24. Relay output module parameters<span id="table:rs485-relay-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.invert-relay-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Negate relay output pin</p></td>
</tr>
</tbody>
</table>

##### HAL example

    gm.0.rs485.0.relay-0 # First relay of the node.
    gm.0                 # Means the first GM6-PCI motion control card (PCI card address = 0)
    .rs485.0             # Select node with address 0 on the RS485 bus
    .relay-0             # Select the first relay

#### Digital input module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

##### Pins

<table>
<caption>Table 25. Digital input output module pins<span id="table:rs485-input-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
</tbody>
</table>

##### HAL example

    gm.0.rs485.0.in-0 # First input of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .in-0    - Select the first digital input module

#### DAC & ADC module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

##### Pins

<table>
<caption>Table 26. DAC &amp; ADC module pins<span id="table:rs485-dacadc-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Value of ADC input in Volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dac-enable-&lt;0-3&gt;</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable DAC output. When enable is false DAC output is set to 0.0 V.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Value of DAC output in Volts.</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 27. DAC &amp; ADC module parameters<span id="table:rs485-dacadc-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-scale-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>The input voltage will be multiplied by scale before being output to .adc- pin.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.adc-offset-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is subtracted from the hardware input voltage after the scale multiplier has been applied.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-offset-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is added to the value before the hardware is updated.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dac-high-limit-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-low-limit-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Minimum output voltage of the hardware in volts.</p></td>
</tr>
</tbody>
</table>

##### HAL example

    gm.0.rs485.0.adc-0 # First analogue channel of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .adc-0   - Select the first analogue input of the module

#### Teach Pendant module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15. Note that on the Teach Pendant module it cannot be changed, and pre-programmed as zero. Upon request it can be delivered with firmware pre-programmed different ID.

##### Pins

<table>
<caption>Table 28. Teach Pendant module pins<span id="table:rs485-teachpendant-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Value of ADC input in Volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.enc-reset</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>When True, resets counts and position to zero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-counts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position in encoder counts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.enc-rawcounts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>The raw count is the counts, but unaffected by reset.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-position</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position in scaled units (=.enc-counts/.enc-position-scale).</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
</tbody>
</table>

##### Parameters

<table>
<caption>Table 29. Teach Pendant module parameters<span id="table:rs485-teachpendant-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-scale-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>The input voltage will be multiplied by scale before being output to .adc- pin.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.adc-offset-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is subtracted from the hardware input voltage after the scale multiplier has been applied.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in per length unit.</p></td>
</tr>
</tbody>
</table>

##### HAL example

    gm.0.rs485.0.adc-0 # First analogue channel of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .adc-0   - Select the first analogue input of the module

### Errata

#### GM6-PCI card Errata

The revision number in this section refers to the revision of the GM6-PCI card device.

##### Rev. 1.2

-   Error: The PCI card do not boot, when Axis 1. END B switch is active (low). Found on November 16, 2013.

-   Reason: This switch is connected to a boot setting pin of FPGA

-   Problem fix/workaround: Use other switch pin, or connect only normally open switch to this switch input pin.

Legal Section
-------------

### Copyright Terms

Copyright (c) 2000-2013 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: "This Machinekit Handbook is the product of several authors writing for machinekit.io As you find it to be of value in your work, we invite you to contribute to its revision and growth." A copy of the license is included in the section entitled "GNU Free Documentation License". If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307

### GNU Free Documentation License

GNU Free Documentation License Version 1.1, March 2000

Copyright © 2000 Free Software Foundation, Inc. 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA Everyone is permitted to copy and distribute verbatim copies of this license document, but changing it is not allowed.

**0. PREAMBLE**

The purpose of this License is to make a manual, textbook, or other written document "free" in the sense of freedom: to assure everyone the effective freedom to copy and redistribute it, with or without modifying it, either commercially or noncommercially. Secondarily, this License preserves for the author and publisher a way to get credit for their work, while not being considered responsible for modifications made by others.

This License is a kind of "copyleft", which means that derivative works of the document must themselves be free in the same sense. It complements the GNU General Public License, which is a copyleft license designed for free software.

We have designed this License in order to use it for manuals for free software, because free software needs free documentation: a free program should come with manuals providing the same freedoms that the software does. But this License is not limited to software manuals; it can be used for any textual work, regardless of subject matter or whether it is published as a printed book. We recommend this License principally for works whose purpose is instruction or reference.

**1. APPLICABILITY AND DEFINITIONS**

This License applies to any manual or other work that contains a notice placed by the copyright holder saying it can be distributed under the terms of this License. The "Document", below, refers to any such manual or work. Any member of the public is a licensee, and is addressed as "you".

A "Modified Version" of the Document means any work containing the Document or a portion of it, either copied verbatim, or with modifications and/or translated into another language.

A "Secondary Section" is a named appendix or a front-matter section of the Document that deals exclusively with the relationship of the publishers or authors of the Document to the Document’s overall subject (or to related matters) and contains nothing that could fall directly within that overall subject. (For example, if the Document is in part a textbook of mathematics, a Secondary Section may not explain any mathematics.) The relationship could be a matter of historical connection with the subject or with related matters, or of legal, commercial, philosophical, ethical or political position regarding them.

The "Invariant Sections" are certain Secondary Sections whose titles are designated, as being those of Invariant Sections, in the notice that says that the Document is released under this License.

The "Cover Texts" are certain short passages of text that are listed, as Front-Cover Texts or Back-Cover Texts, in the notice that says that the Document is released under this License.

A "Transparent" copy of the Document means a machine-readable copy, represented in a format whose specification is available to the general public, whose contents can be viewed and edited directly and straightforwardly with generic text editors or (for images composed of pixels) generic paint programs or (for drawings) some widely available drawing editor, and that is suitable for input to text formatters or for automatic translation to a variety of formats suitable for input to text formatters. A copy made in an otherwise Transparent file format whose markup has been designed to thwart or discourage subsequent modification by readers is not Transparent. A copy that is not "Transparent" is called "Opaque".

Examples of suitable formats for Transparent copies include plain ASCII without markup, Texinfo input format, LaTeX input format, SGML or XML using a publicly available DTD, and standard-conforming simple HTML designed for human modification. Opaque formats include PostScript, PDF, proprietary formats that can be read and edited only by proprietary word processors, SGML or XML for which the DTD and/or processing tools are not generally available, and the machine-generated HTML produced by some word processors for output purposes only.

The "Title Page" means, for a printed book, the title page itself, plus such following pages as are needed to hold, legibly, the material this License requires to appear in the title page. For works in formats which do not have any title page as such, "Title Page" means the text near the most prominent appearance of the work’s title, preceding the beginning of the body of the text.

**2. VERBATIM COPYING**

You may copy and distribute the Document in any medium, either commercially or noncommercially, provided that this License, the copyright notices, and the license notice saying this License applies to the Document are reproduced in all copies, and that you add no other conditions whatsoever to those of this License. You may not use technical measures to obstruct or control the reading or further copying of the copies you make or distribute. However, you may accept compensation in exchange for copies. If you distribute a large enough number of copies you must also follow the conditions in section 3.

You may also lend copies, under the same conditions stated above, and you may publicly display copies.

**3. COPYING IN QUANTITY**

If you publish printed copies of the Document numbering more than 100, and the Document’s license notice requires Cover Texts, you must enclose the copies in covers that carry, clearly and legibly, all these Cover Texts: Front-Cover Texts on the front cover, and Back-Cover Texts on the back cover. Both covers must also clearly and legibly identify you as the publisher of these copies. The front cover must present the full title with all words of the title equally prominent and visible. You may add other material on the covers in addition. Copying with changes limited to the covers, as long as they preserve the title of the Document and satisfy these conditions, can be treated as verbatim copying in other respects.

If the required texts for either cover are too voluminous to fit legibly, you should put the first ones listed (as many as fit reasonably) on the actual cover, and continue the rest onto adjacent pages.

If you publish or distribute Opaque copies of the Document numbering more than 100, you must either include a machine-readable Transparent copy along with each Opaque copy, or state in or with each Opaque copy a publicly-accessible computer-network location containing a complete Transparent copy of the Document, free of added material, which the general network-using public has access to download anonymously at no charge using public-standard network protocols. If you use the latter option, you must take reasonably prudent steps, when you begin distribution of Opaque copies in quantity, to ensure that this Transparent copy will remain thus accessible at the stated location until at least one year after the last time you distribute an Opaque copy (directly or through your agents or retailers) of that edition to the public.

It is requested, but not required, that you contact the authors of the Document well before redistributing any large number of copies, to give them a chance to provide you with an updated version of the Document.

**4. MODIFICATIONS**

You may copy and distribute a Modified Version of the Document under the conditions of sections 2 and 3 above, provided that you release the Modified Version under precisely this License, with the Modified Version filling the role of the Document, thus licensing distribution and modification of the Modified Version to whoever possesses a copy of it. In addition, you must do these things in the Modified Version:

1.  Use in the Title Page (and on the covers, if any) a title distinct from that of the Document, and from those of previous versions (which should, if there were any, be listed in the History section of the Document). You may use the same title as a previous version if the original publisher of that version gives permission. B. List on the Title Page, as authors, one or more persons or entities responsible for authorship of the modifications in the Modified Version, together with at least five of the principal authors of the Document (all of its principal authors, if it has less than five). C. State on the Title page the name of the publisher of the Modified Version, as the publisher. D. Preserve all the copyright notices of the Document. E. Add an appropriate copyright notice for your modifications adjacent to the other copyright notices. F. Include, immediately after the copyright notices, a license notice giving the public permission to use the Modified Version under the terms of this License, in the form shown in the Addendum below. G. Preserve in that license notice the full lists of Invariant Sections and required Cover Texts given in the Document’s license notice. H. Include an unaltered copy of this License. I. Preserve the section entitled "History", and its title, and add to it an item stating at least the title, year, new authors, and publisher of the Modified Version as given on the Title Page. If there is no section entitled "History" in the Document, create one stating the title, year, authors, and publisher of the Document as given on its Title Page, then add an item describing the Modified Version as stated in the previous sentence. J. Preserve the network location, if any, given in the Document for public access to a Transparent copy of the Document, and likewise the network locations given in the Document for previous versions it was based on. These may be placed in the "History" section. You may omit a network location for a work that was published at least four years before the Document itself, or if the original publisher of the version it refers to gives permission. K. In any section entitled "Acknowledgements" or "Dedications", preserve the section’s title, and preserve in the section all the substance and tone of each of the contributor acknowledgements and/or dedications given therein. L. Preserve all the Invariant Sections of the Document, unaltered in their text and in their titles. Section numbers or the equivalent are not considered part of the section titles. M. Delete any section entitled "Endorsements". Such a section may not be included in the Modified Version. N. Do not retitle any existing section as "Endorsements" or to conflict in title with any Invariant Section.

If the Modified Version includes new front-matter sections or appendices that qualify as Secondary Sections and contain no material copied from the Document, you may at your option designate some or all of these sections as invariant. To do this, add their titles to the list of Invariant Sections in the Modified Version’s license notice. These titles must be distinct from any other section titles.

You may add a section entitled "Endorsements", provided it contains nothing but endorsements of your Modified Version by various parties—for example, statements of peer review or that the text has been approved by an organization as the authoritative definition of a standard.

You may add a passage of up to five words as a Front-Cover Text, and a passage of up to 25 words as a Back-Cover Text, to the end of the list of Cover Texts in the Modified Version. Only one passage of Front-Cover Text and one of Back-Cover Text may be added by (or through arrangements made by) any one entity. If the Document already includes a cover text for the same cover, previously added by you or by arrangement made by the same entity you are acting on behalf of, you may not add another; but you may replace the old one, on explicit permission from the previous publisher that added the old one.

The author(s) and publisher(s) of the Document do not by this License give permission to use their names for publicity for or to assert or imply endorsement of any Modified Version.

**5. COMBINING DOCUMENTS**

You may combine the Document with other documents released under this License, under the terms defined in section 4 above for modified versions, provided that you include in the combination all of the Invariant Sections of all of the original documents, unmodified, and list them all as Invariant Sections of your combined work in its license notice.

The combined work need only contain one copy of this License, and multiple identical Invariant Sections may be replaced with a single copy. If there are multiple Invariant Sections with the same name but different contents, make the title of each such section unique by adding at the end of it, in parentheses, the name of the original author or publisher of that section if known, or else a unique number. Make the same adjustment to the section titles in the list of Invariant Sections in the license notice of the combined work.

In the combination, you must combine any sections entitled "History" in the various original documents, forming one section entitled "History"; likewise combine any sections entitled "Acknowledgements", and any sections entitled "Dedications". You must delete all sections entitled "Endorsements."

**6. COLLECTIONS OF DOCUMENTS**

You may make a collection consisting of the Document and other documents released under this License, and replace the individual copies of this License in the various documents with a single copy that is included in the collection, provided that you follow the rules of this License for verbatim copying of each of the documents in all other respects.

You may extract a single document from such a collection, and distribute it individually under this License, provided you insert a copy of this License into the extracted document, and follow this License in all other respects regarding verbatim copying of that document.

**7. AGGREGATION WITH INDEPENDENT WORKS**

A compilation of the Document or its derivatives with other separate and independent documents or works, in or on a volume of a storage or distribution medium, does not as a whole count as a Modified Version of the Document, provided no compilation copyright is claimed for the compilation. Such a compilation is called an "aggregate", and this License does not apply to the other self-contained works thus compiled with the Document, on account of their being thus compiled, if they are not themselves derivative works of the Document.

If the Cover Text requirement of section 3 is applicable to these copies of the Document, then if the Document is less than one quarter of the entire aggregate, the Document’s Cover Texts may be placed on covers that surround only the Document within the aggregate. Otherwise they must appear on covers around the whole aggregate.

**8. TRANSLATION**

Translation is considered a kind of modification, so you may distribute translations of the Document under the terms of section 4. Replacing Invariant Sections with translations requires special permission from their copyright holders, but you may include translations of some or all Invariant Sections in addition to the original versions of these Invariant Sections. You may include a translation of this License provided that you also include the original English version of this License. In case of a disagreement between the translation and the original English version of this License, the original English version will prevail.

**9. TERMINATION**

You may not copy, modify, sublicense, or distribute the Document except as expressly provided for under this License. Any other attempt to copy, modify, sublicense or distribute the Document is void, and will automatically terminate your rights under this License. However, parties who have received copies, or rights, from you under this License will not have their licenses terminated so long as such parties remain in full compliance.

**10. FUTURE REVISIONS OF THIS LICENSE**

The Free Software Foundation may publish new, revised versions of the GNU Free Documentation License from time to time. Such new versions will be similar in spirit to the present version, but may differ in detail to address new problems or concerns. See <http://www.gnu.org/copyleft/>.

Each version of the License is given a distinguishing version number. If the Document specifies that a particular numbered version of this License "or any later version" applies to it, you have the option of following the terms and conditions either of that specified version or of any later version that has been published (not as a draft) by the Free Software Foundation. If the Document does not specify a version number of this License, you may choose any version ever published (not as a draft) by the Free Software Foundation.

**ADDENDUM**: How to use this License for your documents

To use this License in a document you have written, include a copy of the License in the document and put the following copyright and license notices just after the title page:

Copyright (c) YEAR YOUR NAME. Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with the Invariant Sections being LIST THEIR TITLES, with the Front-Cover Texts being LIST, and with the Back-Cover Texts being LIST. A copy of the license is included in the section entitled "GNU Free Documentation License".

If you have no Invariant Sections, write "with no Invariant Sections" instead of saying which ones are invariant. If you have no Front-Cover Texts, write "no Front-Cover Texts" instead of "Front-Cover Texts being LIST"; likewise for Back-Cover Texts.

If your document contains nontrivial examples of program code, we recommend releasing these examples in parallel under your choice of free software license, such as the GNU General Public License, to permit their use in free software.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


