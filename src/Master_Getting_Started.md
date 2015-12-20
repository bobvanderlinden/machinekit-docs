Getting Started V, 2015-12-20
=============================

The Machinekit Team

![common/images/emc2-intro.\*]()

This handbook is a work in progress. If you are able to help with writing, editing, or graphic preparation please contact any member of the writing team or join and send an email to <emc-users@lists.sourceforge.net>.

Copyright © 2000-2012 Machinekit.org

Permission is granted to copy, distribute and/or modify this document under the terms of the GNU Free Documentation License, Version 1.1 or any later version published by the Free Software Foundation; with no Invariant Sections, no Front-Cover Texts, and one Back-Cover Text: This Machinekit Handbook is the product of several authors writing for linuxCNC.org. As you find it to be of value in your work, we invite you to contribute to its revision and growth. A copy of the license is included in the section entitled GNU Free Documentation License. If you do not find the license you may order a copy from Free Software Foundation, Inc. 59 Temple Place, Suite 330 Boston, MA 02111-1307

LINUX® is the registered trademark of Linus Torvalds in the U.S. and other countries. The registered trademark Linux® is used pursuant to a sublicense from LMI, the exclusive licensee of Linus Torvalds, owner of the mark on a world-wide basis.

Getting Started
===============

System Requirements
-------------------

<span id="cha:system-requirements"></span>

### Minimum Requirements

The minimum system to run Machinekit and Debian may vary depending on the exact usage.

Stepper systems in general require faster threads to generate step pulses than servo systems. Keep in mind that the Latency Test numbers are more important than the processor speed for software step generation.

More information on the Latency Test is [Here](../install/Latency_Test.md)

Machinekit and Debian should run reasonably well on a computer with the following minimum hardware specification. These numbers are not the absolute minimum but will give reasonable performance for most stepper systems.

-   1.2 GHz x86 processor

-   512 MB RAM up to 2 GB recommended

-   8 GB hard disk space minimum

-   Graphics card capable of at least 1024x768 resolution, which is not using the NVidia or ATI fglrx proprietary drivers, and which is not an onboard video chipset that shares main memory with the CPU

-   A network or Internet connection (not strictly needed, but very useful for updates and for communicating with the Machinekit community)

### Problematic Hardware

#### Laptops

Laptops are not generally suited to real time software step generation. Again a Latency Test run for an extended time will give you the info you need to determine suitability.

#### Video Cards

If your installation pops up with 800 x 600 screen resolution then most likely Debian does not recognize your video card or monitor. Onboard video many times causes bad real time performance.

About Machinekit
----------------

### The Software

-   Machinekit (the Enhanced Machine Control) is a software system for computer control of machine tools such as milling machines and lathes, robots such as puma and scara and other computer controlled machines up to 9 axes.

-   Machinekit is free software with open source code. Current versions of Machinekit are entirely licensed under the GNU General Public License and Lesser GNU General Public License (GPL and LGPL)

-   Machinekit provides:

    -   a graphical user interface (actually several interfaces to choose from)

    -   an interpreter for *G-code* (the RS-274 machine tool programming language)

    -   a realtime motion planning system with look-ahead

    -   operation of low-level machine electronics such as sensors and motor drives

    -   an easy to use *breadboard* layer for quickly creating a unique configuration for your machine

    -   a software PLC programmable with ladder diagrams

    -   easy installation with a Live-CD

-   It does not provide drawing (CAD - Computer Aided Design) or G-code generation from the drawing (CAM - Computer Automated Manufacturing) functions.

-   It can simultaneously move up to 9 axes and supports a variety of interfaces.

-   The control can operate true servos (analog or PWM) with the feedback loop closed by the Machinekit software at the computer, or open loop with step-servos or stepper motors.

-   Motion control features include: cutter radius and length compensation, path deviation limited to a specified tolerance, lathe threading, synchronized axis motion, adaptive feedrate, operator feed override, and constant velocity control.

-   Support for non-Cartesian motion systems is provided via custom kinematics modules. Available architectures include hexapods (Stewart platforms and similar concepts) and systems with rotary joints to provide motion such as PUMA or SCARA robots.

-   Machinekit runs on Linux using real time extensions.

### The Operating System

Debian has been chosen because it fits perfectly into the Open Source views of Machinekit:

-   Debian will always be free of charge, and there is no extra fee for the *enterprise edition*, we make our very best work available to everyone on the same Free terms.

-   Machinekit is paired with the LTS versions of Debian which provide support and security fixes from the Debian team for 3 - 5 years.

-   Debian uses the very best in translations and accessibility infrastructure that the Free Software community has to offer, to make Debian usable for as many people as possible.

-   The Debian community is entirely committed to the principles of free software development; we encourage people to use open source software, improve it and pass it on.

### Getting Help

#### IRC

IRC stands for Internet Relay Chat. It is a live connection to other Machinekit users. The Machinekit IRC channel is \#machinekit on freenode.

The simplest way to get on the IRC is to use the embedded java client on this [page](http://www.machinekit.org/index.php/english/community).

 Some IRC etiquette   
-   Ask specific questions… Avoid questions like "Can someone help me?".

-   If you’re really new to all this, think a bit about your question before typing it. Make sure you give enough information so someone can solve your question.

-   Have some patience when waiting for an answer, sometimes it takes a while to formulate an answer or everyone might be busy working or something.

-   Set up your IRC account with your unique name so people will know who you are. If you use the java client, use the same name every time you log in. This helps people remember who you are and if you have been on before many will remember the past discussions which saves time on both ends.

 Sharing Files   
The most common way to share files on the IRC is to upload the file to one of the following or a similar service and paste the link:

-   *For text* - <http://pastebin.com/> , <http://pastie.org/>, <https://gist.github.com/>

-   *For pictures* - <http://imagebin.org/> , <http://imgur.com/> , <http://bayimg.com/>

-   *For files* - <https://filedropper.com/> , <http://filefactory.com/> , <http://1fichier.com/>

#### Mailing List

An Internet Mailing List is a way to put questions out for everyone on that list to see and answer at their convenience. You get better exposure to your questions on a mailing list than on the IRC but answers take longer. In a nutshell you e-mail a message to the list and either get daily digests or individual replies back depending on how you set up your account.

The [emc-users mailing list](https://lists.sourceforge.net/lists/listinfo/emc-users)

#### Machinekit Wiki

A Wiki site is a user maintained web site that anyone can add to or edit.

The user maintained Machinekit Wiki site contains a wealth of information and tips at:

[http://wiki.machinekit.org](http://wiki.machinekit.org/)

### Getting Machinekit

#### Normal Download

Download the Live CD from:

[the Machinekit homepage at www.machinekit.org](http://www.machinekit.org/)

and follow the Download link.

#### Multi-session Download

If the file is too large to download in one session because of a bad or slow Internet connection, use *wget* to allow resuming after an interrupted download.

 Wget Linux   
Open a terminal window. In Debian it is Applications/Accessories/Terminal. Use *cd* to change to the directory where you would like to store the ISO. Use *mkdir* to create a new directory if needed.

Note that actual file names may change so you might have to go to <http://www.machinekit.org/> and follow the Download link to get the actual file name. In most browsers you can right click on the link and select Copy Link Location or similar, then paste the link into the terminal window with a right mouse click and select Paste.

Debian 10.04 Lucid Lynx and Machinekit (current release)

To get the Debian 10.04 Lucid Lynx version, copy one of these in the terminal window and press enter:

For the USA mirror: wget <http://machinekit.org/iso/ubuntu-10.04-machinekit3-i386.iso>

For the European mirror: wget <http://dsplabs.upt.ro/~juve/emc/get.php?file=ubuntu-10.04-machinekit3-i386.iso>

The md5sum of the above file is: *76dc2416b917679b71255e464ede84ec*

To continue a partial download that was interrupted add the -c option to wget:

wget -c <http://machinekit.org/iso/ubuntu-10.04-machinekit3-i386.iso>

To stop a download use Ctrl-C or close the terminal window.

Debian 8.04 Hardy Heron and Machinekit (older)

If your hardware requires an older version of Debian, you can download Debian 8.04 and upgrade to the latest Machinekit version by following the instructions on the Machinekit.org download page.

<http://machinekit.org/index.php/english/download>

After the download is complete you will find the ISO file in the directory that you selected. Next we will burn the CD.

 Wget Windows   
The wget program is also available for Windows from:

<http://gnuwin32.sourceforge.net/packages/wget.htm>

Follow the instructions on the web page for downloading and installing the windows version of the wget program.

To run wget open a command prompt window.

In most Windows it is Programs/Accessories/Command Prompt

First you have to change to the directory where wget is installed in.

Typically it is in C:\\Program Files\\GnuWin32\\bin so in the Command Prompt window type:

    cd C:\Program Files\GnuWin32\bin

and the prompt should change to: *C:\\Program Files\\GnuWin32\\bin&gt;*

Type the wget command into the window and press enter as above.

#### Burning the CD

Machinekit is distributed as CD image files, called ISOs. To install Machinekit, you first need to burn the ISO file onto a CD. You need a working CD/DVD burner and an 80 minute (700 Mb) CD for this. If the CD writing fails, try writing at a slower burn speed.

 Verify md5sum in Linux   
Before burning a CD, it is highly recommended that you verify the md5sum (hash) of the .iso file.

Open a terminal window. In Debian it is Applications/Accessories/Terminal.

Change to the directory where the ISO was downloaded to.

    cd download_directory

Then run the md5sum command with the file name you saved.

    md5sum -b ubuntu-10.04-machinekit1-i386.iso

The md5sum should print out a single line after calculating the hash. On slower computers this might take a minute or two.

    76dc2416b917679b71255e464ede84ec *ubuntu-10.04-machinekit3-i386.iso

Now compare it to the md5sum value that it should be.

If you downloaded the md5sum as well as the iso, you can ask the md5sum program to do the checking for you. In the same directory:

    md5sum -c ubuntu-10.04-machinekit1-i386.iso.md5

If all is well, after a short delay the terminal will print:

    ubuntu-10.04-machinekit1-i386.iso: OK

 Burning the ISO in Linux   
1.  Insert a blank CD into your burner. A *CD/DVD Creator* or *Choose Disc Type* window will pop up. Close this, as we will not be using it.

2.  Browse to the downloaded ISO image in the file browser.

3.  Right click on the ISO image file and choose Write to Disc.

4.  Select the write speed. If you are burning a Debian Live CD, it is recommended that you write at the lowest possible speed.

5.  Start the burning process.

6.  If a *choose a file name for the disc image* window pops up, just pick OK.

 Verify md5sum with Windows   
Before burning a CD, it is highly recommended that you verify the md5 sum (hash) of the .iso file, to ensure that you got a good download.

Windows does not come with a md5sum program. You will have to download and install one to check the md5sum. More information can be found at:

<https://help.ubuntu.com/community/HowToMD5SUM>

 Burning the ISO in Windows   
1.  Download and install Infra Recorder, a free and open source image burning program: <http://infrarecorder.org/>

2.  Insert a blank CD in the drive and select Do nothing or Cancel if an auto-run dialog pops up.

3.  Open Infra Recorder, and select the *Actions* menu, then *Burn image*.

#### Testing Machinekit

With the Live CD in the CD/DVD drive shut down the computer then turn the computer back on. This will boot the computer from the Live CD. Once the computer has booted up you can try out Machinekit without installing it. You can not create custom configurations or modify most system settings like screen resolution unless you install Machinekit.

To try out Machinekit from the Applications/CNC menu pick Machinekit. Then select a sim configuration to try out.

To see if your computer is suitable for software step pulse generation run the Latency Test as shown [here](#latency-test).

#### Installing Machinekit

If you like what you see, just click the Install icon on the desktop, answer a few questions (your name, timezone, password) and the install completes in a few minutes. Make sure you write down the name you used and the password. Once the install process is complete and you go on line the update manager will pop up and allow you to upgrade to the latest stable version of Machinekit.

#### Updates to Machinekit

With the normal install the Update Manager will notify you of updates to Machinekit when you go on line and allow you to easily upgrade with no Linux knowledge needed. If you want to upgrade to 10.04 from 8.04 a clean install from the Live-CD is recommended. It is OK to upgrade everything except the operating system when asked to.

Warning: Do not upgrade Debian to a new but non-LTS version (like 8.04 to 8.10) as it will prevent Machinekit from running.

#### Install Problems

In rare cases you might have to reset the BIOS to default settings if during the Live CD install it cannot recognize the hard drive during the boot up.

Updating Machinekit
-------------------

### Updating from 2.4.x to 2.5.x

As of version 2.5.0, the name of the project has changed from EMC2 to Machinekit. All programs with "emc" in the name have been changed to "machinekit" instead. All documentation has been updated.

Additionally, the name of the debian package containing the software has changed. Unfortunately this breaks automatic upgrades. To upgrade from emc2 2.4.X to machinekit 2.5.X, do the following:

#### On Debian Lucid 10.04

First you need to tell your computer where to find the new Machinekit software:

-   Click on the System menu in the top panel and select Administration→Software Sources.

-   Select the Other Software tab.

-   Select the entry that says

        http://machinekit.org/lucid lucid base emc2.4

        or

        http://machinekit.org/lucid lucid base emc2.4-sim

        and click the Edit button.

-   In the Components field, change `emc2.4` to `machinekit2.5`, or change `emc2.4-sim` to `machinekit2.5-sim`.

-   Click the OK button.

-   Back in the Software Sources window, Other Software tab, click the Close button.

-   It will pop up a window informing you that the information about available software is out-of-date. Click the Reload button.

Now your computer knows about the new software, next we need to tell it to install it:

-   Click on the System menu in the top panel and select Administration→Synaptic Package Manager

-   In the Quick Search bar at the top, type `machinekit`.

-   Click the check box to mark the new machinekit package for installation.

-   Click the Apply button, and let your computer install the new package. The old emc 2.4 package will be automatically removed to make room for the new Machinekit 2.5 package.

#### On Debian Hardy 8.04

First you need to tell your computer where to find the new Machinekit software:

-   Click on the System menu in the top panel and select Administration→Synaptic Package Manager

-   Go to Settings→Repositories.

-   Select the "Third-Party Software" tab.

-   Select the entry that says

        http://machinekit.org/hardy hard emc2.4

        or

        http://machinekit.org/hardy hardy emc2.4-sim

        and click the Edit button.

-   In the Components field, change `emc2.4` to `machinekit2.5` or `emc2.4-sim` to `machinekit2.5-sim`.

-   Click the OK button.

-   Back in the Software Sources window, click the Close button.

-   Back in the Synaptic Package Manager window, click the Reload button.

Now your computer knows about the new software, next we need to tell it to install it:

-   In the Synaptic Package Manager, click the Search button.

-   In the Find dialog window that pops up, type `machinekit` and click the Search button.

-   Click the check-box to mark the machinekit package for installation.

-   Click the Apply button, and let your computer install the new package. The old emc 2.4 package will be automatically removed to make room for the new Machinekit 2.5 package.

### Config changes

The user configs moved from $HOME/emc2 to $HOME/machinekit, so you will need to rename your directory, or move your files to the new place.

The hostmot2 watchdog in Machinekit 2.5 does not start running until the HAL threads start running. This means it now tolerates a timeout on the order of the servo thread period, instead of requiring a timeout that’s on the order of the time between loading the driver and starting the HAL threads. This typically means a few milliseconds (a few times the servo thread period) instead of many hundreds of milliseconds. The default has been lowered from 1 second to 5 milliseconds. You generally don’t need to set the hm2 watchdog timeout any more, unless you’ve changed your servo thread period.

The old driver for the Mesa 5i20, hal\_m5i20, has been removed after being deprecated in favor of hostmot2 since early 2009 (version 2.3.) If you are still using this driver, you will need to build a new configuration using the hostmot2 driver. Pncconf may help you do this, and we have some sample configs (hm2-servo and hm2-stepper) that act as examples.

### Upgrading from 2.3.x to 2.4.x

The following instructions only apply to Debian 8.04 "Hardy Heron". Machinekit 2.4 is not available for older releases of Debian.

Because there are several minor incompatibilities between 2.3.5 and 2.4.x, your existing install will not automatically be updated to 2.4.x. If you want to run 2.4.x, change to the Machinekit-2.4 repository by following these instructions:

run System/Administration/Synaptic Package Manager

go to Settings/Repositories

In the list of Third-Party software there should be at least two lines for machinekit.org.

For each of them:

-   Select the line and click Edit

-   On the Components line, change emc2.3 to emc2.4

-   Click OK

-   Close the *Software Preferences* window

-   Click *Reload* as instructed

-   Click *Mark All Upgrades*

Mesa card and hostmot2 users:

If you use a mesa card, find the proper hostmot2-firmware package for your card and mark it for installation. Hint: do a search for *hostmot2-firmware* in the synaptic package manager.

-   Click *Apply*

### Changes between 2.3.x and 2.4.x

Once you have done the upgrade, update any custom configurations by following these instructions:

#### emc.nml changes (2.3.x to 2.4.x)

For configurations that have not customized emc.nml, remove or comment out the inifile line NML\_FILE = emc.nml. This will cause the most up to date version of emc.nml to be used.

For configurations that have customized emc.nml, a change similar to this one is required.

Failure to do this can cause an error like this one:

    libnml/buffer/physmem.cc 143: PHYSMEM_HANDLE:
    Can't write 10748 bytes at offset 60 from buffer of size 10208.

#### tool table changes (2.3.x to 2.4.x)

The format of the tool table has been changed incompatibly. The documentation shows the new format. The tool table will automatically be converted to the new format.

#### hostmot2 firmware images (2.3.x to 2.4.x)

The hostmot2 firmware images are now a separate package. You can:

-   Continue using an already-installed *emc2-firmware-mesa-\** 2.3.x package

-   Install the new packages from the synaptic package manager. The new packages are named *hostmot2-firmware-\**

-   Download the firmware images as tar files from <http://emergent.unpy.net/01267622561> and install them manually

Stepper Quickstart
------------------

<span id="cha:stepper-quickstart"></span>

This section assumes you have done a standard install from the Live CD. After installation it is recommended that you connect the computer to the Internet and wait for the update manager to pop up and get the latest updates for Machinekit and Debian before continuing. For more complex installations see the Integrator Manual.

### Latency Test

The Latency Test determines how late your computer processor is in responding to a request. Some hardware can interrupt the processing which could cause missed steps when running a CNC machine. This is the first thing you need to do. Follow the instructions [here](../install/Latency_Test.md) to run the latency test.

### Sherline

If you have a Sherline several predefined configurations are provided. This is on the main menu CNC/EMC then pick the Sherline configuration that matches yours and save a copy.

### Xylotex

If you have a Xylotex you can skip the following sections and go straight to the [Stepper Config Wizard](#cha:stepconf-wizard). Machinekit has provided quick setup for the Xylotex machines.

### Machine Information

Gather the information about each axis of your machine.

Drive timing is in nano seconds. If you’re unsure about the timing many popular drives are included in the stepper configuration wizard. Note some newer Gecko drives have different timing than the original one. A [list](http://wiki.machinekit.org/) is also on the user maintained Machinekit wiki site of more drives.

<table>
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
<th align="left">Axis</th>
<th align="left">Drive Type</th>
<th align="left">Step Time ns</th>
<th align="left">Step Space ns</th>
<th align="left">Dir. Hold ns</th>
<th align="left">Dir. Setup ns</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

### Pinout Information

Gather the information about the connections from your machine to the PC parallel port.

<table>
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
<th align="left">Output Pin</th>
<th align="left">Typ. Function</th>
<th align="left">If Different</th>
<th align="left">Input Pin</th>
<th align="left">Typ. Function</th>
<th align="left">If Different</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>E-Stop Out</p></td>
<td align="left"><p></p></td>
<td align="left"><p>10</p></td>
<td align="left"><p>X Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>X Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>11</p></td>
<td align="left"><p>Y Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>X Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>12</p></td>
<td align="left"><p>Z Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Y Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p>13</p></td>
<td align="left"><p>A Limit/Home</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Y Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p>15</p></td>
<td align="left"><p>Probe In</p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Z Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Z Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>A Step</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>A Direction</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>14</p></td>
<td align="left"><p>Spindle CW</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>16</p></td>
<td align="left"><p>Spindle PWM</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>17</p></td>
<td align="left"><p>Amplifier Enable</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

Note any pins not used should be set to Unused in the drop down box. These can always be changed later by running Stepconf again.

### Mechanical Information

Gather information on steps and gearing. The result of this is steps per user unit which is used for SCALE in the .ini file.

<table>
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
<th align="left">Axis</th>
<th align="left">Steps/Rev.</th>
<th align="left">Micro Steps</th>
<th align="left">Motor Teeth</th>
<th align="left">Leadscrew Teeth</th>
<th align="left">Leadscrew Pitch</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>X</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p>Y</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="odd">
<td align="left"><p>Z</p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
<tr class="even">
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

-   *Steps per revolution* - is how many stepper-motor-steps it takes to turn the stepper motor one revolution. Typical is 200.

-   *Micro Steps* - is how many steps the drive needs to move the stepper motor one full step. If microstepping is not used, this number will be 1. If microstepping is used the value will depend on the stepper drive hardware.

-   *Motor Teeth and Leadscrew Teeth* - is if you have some reduction (gears, chain, timing belt, etc.) between the motor and the leadscrew. If not, then set these both to 1.

-   *Leadscrew Pitch* - is how much movement occurs (in user units) in one leadscrew turn. If you’re setting up in inches then it is inches per turn. If you’re setting up in millimeters then it is millimeters per turn.

The net result you’re looking for is how many CNC-output-steps it takes to move one user unit (inches or mm).

Example 1. Units inches

    Stepper         = 200 steps per revolution
    Drive           =  10 micro steps per step
    Motor Teeth     =  20
    Leadscrew Teeth =  40
    Leadscrew Pitch =   0.2000 inches per turn

From the above information, the leadscrew moves 0.200 inches per turn. - The motor turns 2.000 times per 1 leadscrew turn. - The drive takes 10 microstep inputs to make the stepper step once. - The drive needs 2000 steps to turn the stepper one revolution. So the scale needed is:

![images/step-calc-inch-math.png]()

Example 2. Units mm

        Stepper         = 200 steps per revolution
        Drive           =   8 micro steps per step
        Motor Teeth     =  30
        Leadscrew Teeth =  90
        Leadscrew Pitch =   5.00 mm per turn

From the above information: - The leadscrew moves 5.00 mm per turn. - The motor turns 3.000 times per 1 leadscrew turn. - The drive takes 8 microstep inputs to make the stepper step once. - The drive needs 1600 steps to turn the stepper one revolution. So the scale needed is:

![images/step-calc-mm-math.png]()

Stepper Configuration Wizard
----------------------------

<span id="cha:stepconf-wizard"></span>

Machinekit is capable of controlling a wide range of machinery using many different hardware interfaces.

Stepconf is a program that generates configuration files for Machinekit for a specific class of CNC machine: those that are controlled via a *standard parallel port*, and controlled by signals of type *step & direction*.

Stepconf is installed when you install Machinekit and is in the CNC menu.

Stepconf places a file in the machinekit/config directory to store the choices for each configuration you create. When you change something, you need to pick the file that matches your configuration name. The file extension is .stepconf.

The Stepconf Wizard works best with at least 800 x 600 screen resolution.

Step by Step Instructions
-------------------------

### Start Page<span id="sec:Entry-Page"></span>

![images/stepconf-config.png]()

Figure 1. Entry Page<span id="cap:Entry-Page"></span>

-   *Create New* - Creates a fresh configuration.

-   *Modify* - Modify an existing configuration. After selecting this a file picker pops up so you can select the .stepconf file for modification. If you made any modifications to the main .hal or the .ini file these will be lost. Modifications to custom.hal and custom\_postgui.hal will not be changed by the Stepconf Wizard. Stepconf will highlight the lastconf that was built.

These next options wiil be recorded in a preference file for the next run of Stepconf.

-   *Create Desktop Shortcut* - This will place a link on your desktop to the files.

-   *Create Desktop Launcher* - This will place a launcher on your desktop to start your application.

-   *Create Simulated Hardware* - This allows you to build a config for testing, even if you don’t have the actual hardware.

### Basic Information<span id="sec:Basic-Information"></span>

![images/stepconf-basic.png]()

Figure 2. Basic Information Page<span id="cap:Basic-Information-Page"></span>

-   *Machine Name* - Choose a name for your machine. Use only uppercase letters, lowercase letters, digits, - and \_.

-   *Axis Configuration* - Choose XYZ (Mill), XYZA (4-axis mill) or XZ (Lathe).

-   *Machine Units* - Choose Inch or mm. All subsequent entries will be in the chosen units. Changing this also changes the default values in the Axes section. If you change this after selecting values in any of the axes sections, they will be over-written by the default values of the selected units.

-   *Driver Type* - If you have one of the stepper drivers listed in the pull down box, choose it. Otherwise, select *Other* and find the timing values in your driver’s data sheet and enter them as *nano seconds* in the *Driver Timing Settings*. If the data sheet gives a value in microseconds, multiply by 1000. For example, enter 4.5us as 4500ns.

A list of some popular drives, along with their timing values, is at [Stepper Drive Timing](http://wiki.linuxcnc.org/cgi-bin/wiki.pl?Stepper_Drive_Timing).

Additional signal conditioning or isolation such as optocouplers and RC filters on break out boards can impose timing constraints of their own, in addition to those of the driver. You may find it necessary to add some time to the drive requirements to allow for this.

The Machinekit Configuration Selector has configs for Sherline already configured.

-   *Step Time* - How long the step pulse is *on* in nano seconds. If your not sure about this setting a value of 10,000 will work with most drives.

-   *Step Space* - Minimum time between step pulses in nano seconds. If your not sure about this setting a value of 10,000 will work with most drives.

-   *Direction Hold* - How long the direction pin is held after a change of direction in nanoseconds. If your not sure about this setting a value of 200,000 will work with most drives.

-   *Direction Setup* - How long before a direction change after the last step pulse in nanoseconds. If your not sure about this setting a value of 200,000 will work with most drives.

-   *One / Two Parport* - Select how many parallel port are to be configured.

-   *Base Period Maximum Jitter* - Enter the result of the Latency Test here. To run a latency test press the *Test Base Period Jitter* button. See the [Latency Test](../install/Latency_Test.md) section for more details.

-   *Max Step Rate* -Stepconf automatically calculates the Max Step Rate based on the driver characteristics entered and the latency test result.

-   *Min Base Period* - Stepconf automatically determines the Min Base Period based on the driver characteristics entered and latency test result.

Troubleshooting SMI Issues (Machinekit.org Wiki)

Fixing Realtime problems caused by SMI on Debian is covered [Here](http://wiki.linuxcnc.org/cgi-bin/wiki.pl?FixingSMIIssues)

The important numbers are the *max jitter*. In the example above 9075 nanoseconds, or 9.075 microseconds, is the highest jitter. Record this number, and enter it in the Base Period Maximum Jitter box.

If your Max Jitter number is less than about 15-20 microseconds (15000-20000 nanoseconds), the computer should give very nice results with software stepping. If the max latency is more like 30-50 microseconds, you can still get good results, but your maximum step rate might be a little disappointing, especially if you use microstepping or have very fine pitch leadscrews. If the numbers are 100 us or more (100,000 nanoseconds), then the PC is not a good candidate for software stepping. Numbers over 1 millisecond (1,000,000 nanoseconds) mean the PC is not a good candidate for Machinekit, regardless of whether you use software stepping or not.

### Parallel Port Setup<span id="sec:Parallel-Port-Setup"></span>

![images/stepconf-pinout.png]()

Figure 3. Parallel Port Setup Page<span id="cap:Parallel-Port-Setup"></span>

You may specify the address as a hexidecimal (often 0x378) or as linux’s default port number (probably 0)

For each pin, choose the signal which matches your parallel port pinout. Turn on the *invert* check box if the signal is inverted (0V for true/active, 5V for false/inactive).

-   *Output pinout presets* - Automatically set pins 2 through 9 according to the Sherline standard (Direction on pins 2, 4, 6, 8) or the Xylotex standard (Direction on pins 3, 5, 7, 9).

-   *Inputs and Outputs* - If the input or output is not used set the option to *Unused*.

-   *External E Stop* - This can be selected from an input pin drop down box. A typical E Stop chain uses all normally closed contacts.

-   *Homing & Limit Switches* - These can be selected from an input pin drop down box for most configurations.

-   *Charge Pump* - If your driver board requires a charge pump signal select Charge Pump from the drop down list for the output pin you wish to connect to your charge pump input. The charge pump output is connected to the base thread by Stepconf. The charge pump output will be about 1/2 of the maximum step rate shown on the Basic Machine Configuration page.

### Parallel Port 2 Setup<span id="sec:Parallel-Port-2-Setup"></span>

![images/stepconf-parport2.png]()

Figure 4. Parallel Port 2 Setup Page<span id="cap:Parallel-Port-2-Setup"></span>

The second Parallel port (if selected) can be configured and It’s pins assigned on this page.
 No step and direction signals can be selected.
 You may select in or out to maximizes the number of input/output pins that are available.
 You may specify the address as a hexidecimal (often 0x278) or as linux’s default port number (probably 1).

### Axis Configuration<span id="sec:Axis-Configuration"></span>

![images/stepconf-axis.png]()

Figure 5. Axis Configuration Page<span id="cap:Axis-Configuration-Page"></span>

-   *Motor Steps Per Revolution* - The number of full steps per motor revolution. If you know how many degrees per step the motor is (e.g., 1.8 degree), then divide 360 by the degrees per step to find the number of steps per motor revolution.

-   *Driver Microstepping* - The amount of microstepping performed by the driver. Enter *2* for half-stepping.

-   *Pulley Ratio* - If your machine has pulleys between the motor and leadscrew, enter the ratio here. If not, enter *1:1*.

-   *Leadscrew Pitch* - Enter the pitch of the leadscrew here. If you chose *Inch* units, enter the number of threads per inch If you chose *mm* units, enter the number of millimeters per revolution (e.g., enter 2 for 2mm/rev). If the machine travels in the wrong direction, enter a negative number here instead of a positive number, or invert the direction pin for the axis.

-   *Maximum Velocity* -Enter the maximum velocity for the axis in units per second.

-   *Maximum Acceleration* - The correct values for these items can only be determined through experimentation. See [Finding Maximum Velocity](#finding-maximum-velocity) to set the speed and [Finding Maximum Acceleration](#finding-maximum-acceleration) to set the acceleration.

-   *Home Location* - The position the machine moves to after completing the homing procedure for this axis. For machines without home switches, this is the location the operator manually moves the machine to before pressing the Home button. If you combine the home and limit switches you must move off of the switch to the home position or you will get a joint limit error.

-   *Table Travel* - The range of travel for that axis based on the machine origin. The home location must be inside the *Table Travel* and not equal to one of the Table Travel values.

-   *Home Switch Location* - The location at which the home switch trips or releases reletive to the machine origin. This item and the two below only appear when Home Switches were chosen in the Parallel Port Pinout. If you combine home and limit switches the home switch location can not be the same as the home position or you will get a joint limit error.

-   *Home Search Velocity* - The velocity to use when searching for the home switch. If the switch is near the end of travel, this velocity must be chosen so that the axis can decelerate to a stop before hitting the end of travel. If the switch is only closed for a short range of travel (instead of being closed from its trip point to one end of travel), this velocity must be chosen so that the axis can decelerate to a stop before the switch opens again, and homing must always be started from the same side of the switch. If the machine moves the wrong direction at the beginning of the homing procedure, negate the value of *Home Search Velocity*.

-   *Home Latch Direction* - Choose *Same* to have the axis back off the switch, then approach it again at a very low speed. The second time the switch closes, the home position is set. Choose *Opposite* to have the axis back off the switch and when the switch opens, the home position is set.

-   *Time to accelerate to max speed* - Time to reach maximum speed calculated from *Max Acceleration* and *Max Velocity*.

-   *Distance to accelerate to max speed* - Distance to reach maximum speed from a standstill.

-   *Pulse rate at max speed* - Information computed based on the values entered above. The greatest *Pulse rate at max speed* determines the *BASE\_PERIOD*. Values above 20000Hz may lead to slow response time or even lockups (the fastest usable pulse rate varies from computer to computer)

-   *Axis SCALE* - The number that will be used in the ini file \[SCALE\] setting. This is how many steps per user unit.

-   *Test this axis* - This will open a window to allow testing for each axis. This can be used after filling out all the information for this axis.

#### Test This Axis

![images/stepconf-test.png]()

Figure 6. Test This Axis<span id="cap:Test-This-Axis"></span>

Test this axis is a basic tester that only outputs step and direction signals to try different values for acceleration and velocity.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">In order to use test this axis you have to manually enable the axis if this is required. If your driver has a charge pump you will have to bypass it. Test this axis does not react to limit switch inputs. Use with caution.</td>
</tr>
</tbody>
</table>

##### Finding Maximum Velocity<span id="finding-maximum-velocity"></span>

Begin with a low Acceleration (for example, **`2 inches/s2`** or **`50 mm/s2`**) and the velocity you hope to attain. Using the buttons provided, jog the axis to near the center of travel. Take care because with a low acceleration value, it can take a surprising distance for the axis to decelerate to a stop.

After gaging the amount of travel available, enter a safe distance in Test Area, keeping in mind that after a stall the motor may next start to move in an unexpected direction. Then click Run. The machine will begin to move back and forth along this axis. In this test, it is important that the combination of Acceleration and Test Area allow the machine to reach the selected Velocity and *cruise* for at least a short distance — the more distance, the better this test is. The formula **`d = 0.5 * v * v/a`** gives the minimum distance required to reach the specified velocity with the given acceleration. If it is convenient and safe to do so, push the table against the direction of motion to simulate cutting forces. If the machine stalls, reduce the speed and start the test again.

If the machine did not obviously stall, click the *Run* button off. The axis now returns to the position where it started. If the position is incorrect, then the axis stalled or lost steps during the test. Reduce Velocity and start the test again.

If the machine doesn’t move, stalls, or loses steps, no matter how low you turn Velocity, verify the following:

-   Correct step waveform timings

-   Correct pinout, including *Invert* on step pins

-   Correct, well-shielded cabling

-   Physical problems with the motor, motor coupling, leadscrew, etc.

Once you have found a speed at which the axis does not stall or lose steps during this testing procedure, reduce it by 10% and use that as the axis *Maximum Velocity*.

##### Finding Maximum Acceleration<span id="finding-maximum-acceleration"></span>

With the Maximum Velocity you found in the previous step, enter the acceleration value to test. Using the same procedure as above, adjust the Acceleration value up or down as necessary. In this test, it is important that the combination of Acceleration and Test Area allow the machine to reach the selected Velocity. Once you have found a value at which the axis does not stall or lose steps during this testing procedure, reduce it by 10% and use that as the axis Maximum Acceleration.

### Spindle Configuration<span id="sec:Spindle-Configuration"></span>

![images/stepconf-spindle.png]()

Figure 7. Spindle Configuration Page<span id="cap:Spindle-Configuration-Page"></span>

This page only appears when *Spindle PWM* is chosen in the *Parallel Port Pinout* page for one of the outputs.

#### Spindle Speed Control<span id="spindle-speed-control"></span>

If *Spindle PWM* appears on the pinout, the following information should be entered:

-   *PWM Rate* - The *carrier frequency* of the PWM signal to the spindle. Enter *0* for PDM mode, which is useful for generating an analog control voltage. Refer to the documentation for your spindle controller for the appropriate value.

-   *Speed 1 and 2, PWM 1 and 2* - The generated configuration file uses a simple linear relationship to determine the PWM value for a given RPM value. If the values are not known, they can be determined. For more information see [Determining Spindle Calibration](#determining-spindle-calibration).

#### Spindle-synchronized motion<span id="sub:Spindle-synchronized-motion-lathe"></span>

When the appropriate signals from a spindle encoder are connected to Machinekit via HAL, Machinekit supports lathe threading. These signals are:

-   *Spindle Index* - Is a pulse that occurs once per revolution of the spindle.

-   *Spindle Phase A* - This is a pulse that occurs in multiple equally-spaced locations as the spindle turns.

-   *Spindle Phase B (optional)* - This is a second pulse that occurs, but with an offset from Spindle Phase A. The advantages to using both A and B are direction sensing, increased noise immunity, and increased resolution.

If *Spindle Phase A* and *Spindle Index* appear on the pinout, the following information should be entered:

-   *Use Spindle-At-Speed* - With encoder feedback one can choose to have machinekit wait for the spindle to reach the commanded speed before feed moves. Select this option and set the *close enough* scale.

-   *Speed Display Filter Gain* - Setting for adjusting the stability of the visual spindle speed display.

-   *Cycles per revolution* - The number of cycles of the *Spindle A* signal during one revolution of the spindle. This option is only enabled when an input has been set to *Spindle Phase A*

-   *Maximum speed in thread* - The maximum spindle speed used in threading. For a high spindle RPM or a spindle encoder with high resolution, a low value of *BASE\_PERIOD* is required.

#### Determining Spindle Calibration<span id="determining-spindle-calibration"></span>

Enter the following values in the Spindle Configuration page:

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>Speed 1:</p></td>
<td align="left"><p>0</p></td>
<td align="left"><p>PWM 1:</p></td>
<td align="left"><p>0</p></td>
</tr>
<tr class="even">
<td align="left"><p>Speed 2:</p></td>
<td align="left"><p>1000</p></td>
<td align="left"><p>PWM 2:</p></td>
<td align="left"><p>1</p></td>
</tr>
</tbody>
</table>

Finish the remaining steps of the configuration process, then launch Machinekit with your configuration. Turn the machine on and select the MDI tab. Start the spindle turning by entering: *M3 S100*. Change the spindle speed by entering a different S-number: *S800*. Valid numbers (at this point) range from 1 to 1000.

For two different S-numbers, measure the actual spindle speed in RPM. Record the S-numbers and actual spindle speeds. Run Stepconf again. For *Speed* enter the measured speed, and for *PWM* enter the S-number divided by 1000.

Because most spindle drivers are somewhat nonlinear in their response curves, it is best to:

-   Make sure the two calibration speeds are not too close together in RPM

-   Make sure the two calibration speeds are in the range of speeds you will typically use while milling

For instance, if your spindle will go from 0 RPM to 8000 RPM, but you generally use speeds from 400 RPM (10%) to 4000 RPM (100%), then find the PWM values that give 1600 RPM (40%) and 2800 RPM (70%).

### Options<span id="sec:Advanced-Configuration-Options"></span>

![images/stepconf-advanced.png]()

Figure 8. Advanced Configuration<span id="cap:Advanced-Configuration"></span>

-   *Include Halui* - This will add the Halui user interface component. See the Integrator Manual for more information on Halui.

-   *Include pyVCP* - This option adds the pyVCP panel base file or a sample file to work on. See the Integrator Manual for more information on pyVCP.

-   *Include ClassicLadder PLC* - This option will add the ClassicLadder PLC (Programmable Logic Controller). See the Integrator Manual for more information on ClassicLadder.

-   *Onscreen Prompt For Tool Change* - If this box is checked, Machinekit will pause and prompt you to change the tool when *M6* is encountered. This feature is usually only useful if you have presettable tools.

### Machine Configuration Complete<span id="sub:Machine-Configuration-Complete"></span>

Click *Apply* to write the configuration files. Later, you can re-run this program and tweak the settings you entered before.

### Axis Travel, Home Location, and Home Switch Location<span id="sec:Axis-Travel-Home"></span>

For each axis, there is a limited range of travel. The physical end of travel is called the *hard stop*.

Before the *hard stop* there is a *limit switch*. If the limit switch is encountered during normal operation, Machinekit shuts down the motor amplifier. The distance between the *hard stop* and *limit switch* must be long enough to allow an unpowered motor to coast to a stop.

Before the *limit switch* there is a *soft limit*. This is a limit enforced in software after homing. If a MDI command or g code program would pass the soft limit, it is not executed. If a jog would pass the soft limit, it is terminated at the soft limit.

The *home switch* can be placed anywhere within the travel (between hard stops). As long as external hardware does not deactivate the motor amplifiers when the limit switch is reached, one of the limit switches can be used as a home switch.

The *zero position* is the location on the axis that is 0 in the machine coordinate system. Usually the *zero position* will be within the *soft limits*. On lathes, constant surface speed mode requires that machine *X=0* correspond to the center of spindle rotation when no tool offset is in effect.

The *home position* is the location within travel that the axis will be moved to at the end of the homing sequence. This value must be within the *soft limits*. In particular, the *home position* should never be exactly equal to a *soft limit*.

#### Operating without Limit Switches<span id="sub:Operating-without-Limit"></span>

A machine can be operated without limit switches. In this case, only the soft limits stop the machine from reaching the hard stop. Soft limits only operate after the machine has been homed.

#### Operating without Home Switches<span id="sub:Operating-without-Home"></span>

A machine can be operated without home switches. If the machine has limit switches, but no home switches, it is best to use a limit switch as the home switch (e.g., choose *Minimum Limit + Home X* in the pinout). If the machine has no switches at all, or the limit switches cannot be used as home switches for another reason, then the machine must be homed *by eye* or by using match marks. Homing by eye is not as repeatable as homing to switches, but it still allows the soft limits to be useful.

#### Home and Limit Switch wiring options<span id="sub:Home-and-Limit"></span>

The ideal wiring for external switches would be one input per switch. However, the PC parallel port only offers a total of 5 inputs, while there are as many as 9 switches on a 3-axis machine. Instead, multiple switches are wired together in various ways so that a smaller number of inputs are required.

The figures below show the general idea of wiring multiple switches to a single input pin. In each case, when one switch is actuated, the value seen on INPUT goes from logic HIGH to LOW. However, Machinekit expects a TRUE value when a switch is closed, so the corresponding *Invert* box must be checked on the pinout configuration page. The pull up resistor show in the diagrams pulls the input high until the connection to ground is made and then the input goes low. Otherwise the input might float between on and off when the circuit is open. Typically for a parallel port you might use 47k.

Normally Closed Switches<span id="cap:Normally-Closed-Switches"></span>

Wiring N/C switches in series (simplified diagram)

![images/switch-nc-series.png]()

Normally Open Switches<span id="cap:Normally-Open-Switches"></span>

Wiring N/O switches in parallel (simplified diagram)

![images/switch-no-parallel.png]()

The following combinations of switches are permitted in Stepconf:

-   Combine home switches for all axes

-   Combine limit switches for all axes

-   Combine both limit switches for one axis

-   Combine both limit switches and the home switch for one axis

-   Combine one limit switch and the home switch for one axis

Mesa Configuration Wizard
-------------------------

PNCconf is made to help build configurations that utilize specific Mesa *Anything I/O* products.

It can configure closed loop servo systems or hardware stepper systems. It uses a similar *wizard* approach as Stepconf (used for software stepping, parallel port driven systems).

There are two trains of thought when using PNCconf:

One is to use PNCconf to always configure your system - if you decide to change options, reload PNCconf and allow it to configure the new options. This will work well if your machine is fairly standard and you can use custom files to add non standard features. PNCconf tries to work with you in this regard.

The other is to use PNCconf to build a config that is close to what you want and then hand edit everything to tailor it to your needs. This would be the choice if you need extensive modifications beyond PNCconf’s scope or just want to tinker with / learn about Machinekit

You navigate the wizard pages with the forward, back, and cancel buttons there is also a help button that gives some help information about the pages, diagrams and an output page.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">PNCconf’s help page should have the most up to date info and has additional details.</td>
</tr>
</tbody>
</table>

Step by Step Instructions
-------------------------

![images/pncconf-splash.png]()

Figure 9. PnCConf Splash

### Create or Edit

This allows you to select a previously saved configuration or create a new one. If you pick *Modify a configuration* and then press next a file selection box will show. Pncconf preselects your last saved file. Choose the the config you wish to edit. It also allows you to select desktop shortcut / launcher options. A desktop shortcut will place a folder icon on the desktop that points to your new configuration files. Otherwise you would have to look in your home folder under machinekit/configs.

A Desktop launcher will add an icon to the desktop for starting your config directly. You can also launch it under Applications/cnc/emc2 and selecting your config name.

### Basic Machine Information

![images/pncconf-basic.png]()

Figure 10. PnCConf Basic

 Machine Basics   
If you use a name with spaces PNCconf will replace the spaces with underscore (as a loose rule Linux doesn’t like spaces in names) Pick an axis configuration - this selects what type of machine you are building and what axes are available. The Machine units selector allows data entry of metric or imperial units in the following pages.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">Defaults are not converted when using metric so make sure they are sane values!</td>
</tr>
</tbody>
</table>

 Computer Response Time   
The servo period sets the heart beat of the system. Latency refers to the amount of time the computer can be longer then that period. Just like a railroad, Machinekit requires everything on a very tight and consistent time line or bad things happen. Machinekit requires and uses a *real time* operating system, which just means it has a low latency ( lateness ) response time when Machinekit requires its calculations and when doing Machinekits calculations it cannot be interrupted by lower priority requests (such as user input to screen buttons or drawing etc).

Testing the latency is very important and a key thing to check early. Luckily by using the Mesa card to do the work that requires the fastest response time (encoder counting and PWM generation) we can endure a lot more latency then if we used the parallel port for these things. The standard test in Machinekit is checking the BASE period latency (even though we are not using a base period). If you press the *test base period jitter* button, this launches the latency test window ( you can also load this directly from the applications/cnc panel ). The test mentions to run it for a few minutes but the longer the better. consider 15 minutes a bare minimum and overnight even better. At this time use the computer to load things, use the net, use USB etc we want to know the worst case latency and to find out if any particular activity hurts our latency. We need to look at base period jitter. Anything under 20000 is excellent - you could even do fast software stepping with the machine 20000 - 50000 is still good for software stepping and fine for us. 50000 - 100000 is really not that great but could still be used with hardware cards doing the fast response stuff. So anything under 100000 is useable to us. If the latency is disappointing or you get a bad hiccup periodically you may still be able to improve it.

Now we are happy with the latency and must pick a servo period. In most cases a servo period of 1000000 ns is fine ( that gives a 1 kHz servo calculation rate - 1000 calculations a second) if you are building a closed loop servo system that controls torque (current) rather then velocity (voltage) a faster rate would be better - something like 200000 (5 kHz calculation rate). The problem with lowering the servo rate is that it leaves less time available for the computer to do other things besides Machinekit’s calculations. Typically the display (GUI) becomes less responsive. You must decide on a balance. Keep in mind that if you tune your closed loop servo system then change the servo period you probably will need to tune them again.

 I/O Control Ports/Boards   
PNCconf is capable of configuring machines that have up to two Mesa boards and three parallel ports. Parallel ports can only be used for simple low speed (servo rate) I/O.

 Mesa   
You must choose at least one Mesa board as PNCconf will not configure the parallel ports to count encoders or output step or PWM signals. The mesa cards available in the selection box are based on what PNCconf finds for firmware on the systems. There are options to add custom firmware and/or *blacklist* (ignore) some firmware or boards using a preference file. If no firmware is found PNCconf will show a warning and use internal sample firmware - no testing will be possible. One point to note is that if you choose two PCI Mesa cards there currently is no way to predict which card is 0 and which is 1 - you must test - moving the cards could change their order. If you configure with two cards both cards must be installed for tests to function.

 Parallel Port   
Up to 3 parallel ports (referred to as parports) can be used as simple I/O. You must set the address of the parport. You can either enter the Linux parallel port numbering system (0,1,or 2) or enter the actual address. The address for an on board parport is often 0x0278 or 0x0378 (written in hexadecimal) but can be found in the BIOS page. The BIOS page is found when you first start your computer you must press a key to enter it (such as F2). On the BIOS page you can find the parallel port address and set the mode such as SPP, EPP, etc on some computers this info is displayed for a few seconds during start up. For PCI parallel port cards the address can be found by pressing the *parport address search* button. This pops up the help output page with a list of all the PCI devices that can be found. In there should be a reference to a parallel port device with a list of addresses. One of those addresses should work. Not all PCI parallel ports work properly. Either type can be selected as *in* (maximum amount of input pins) or *out* (maximum amount of output pins)

 GUI Frontend list   
This specifies the graphical display screens Machinekit will use. Each one has different option.

AXIS

-   fully supports lathes.

-   is the most developed and used frontend

-   is designed to be used with mouse and keyboard

-   is tkinter based so integrates PYVCP (python based virtual control panels) naturally.

-   has a 3D graphical window.

-   allows VCP integrated on the side or in center tab

TOUCHY

-   Touchy was designed to be used with a touchscreen, some minimal physical switches and a MPG wheel.

-   requires cycle-start, abort, and single-step signals and buttons

-   It also requires shared axis MPG jogging to be selected.

-   is GTK based so integrates GLADE VCP (virtual control panels) naturally.

-   allows VCP panels integrated in the center Tab

-   has no graphical window

-   look can be changed with custom themes

MINI

-   standard on OEM Sherline machines

-   does not use Estop

-   no VCP integration

TkMachinekit

-   hi contrast bright blue screen

-   separate graphics window

-   no VCP integration

### External Configuration

This page allows you to select external controls such as for jogging or overrides.

![images/pncconf-external.png]()

Figure 11. GUI External

If you select a Joystick for jogging, You will need it always connected for Machinekit to load. To use the analog sticks for useful jogging you probably need to add some custom HAL code. MPG jogging requires a pulse generator connected to a MESA encoder counter. Override controls can either use a pulse generator (MPG) or switches (such as a rotary dial). External buttons might be used with a switch based OEM joystick.

 Joystick jogging   
Requires a custom *device rule* to be installed in the system. This is a file that Machinekit uses to connect to LINUX’s device list. PNCconf will help to make this file.

*Search for device rule* will search the system for rules, you can use this to find the name of devices you have already built with PNCconf.

*Add a device rule* will allow you to configure a new device by following the prompts. You will need your device available.

*test device* allows you to load a device, see its pin names and check its functions with halmeter.

joystick jogging uses HALUI and hal\_input components.

 External buttons   
allows jogging the axis with simple buttons at a specified jog rate. Probably best for rapid jogging.

 MPG Jogging   
Allows you to use a Manual Pulse Generator to jog the machine’s axis.

MPG’s are often found on commercial grade machines. They output quadrature pulses that can be counted with a MESA encoder counter. PNCconf allows for an MPG per axis or one MPG shared with all axis. It allows for selection of jog speeds using switches or a single speed.

The selectable increments option uses the mux16 component. This component has options such as debounce and gray code to help filter the raw switch input.

 Overrides   
PNCconf allows overrides of feedrates and/or spindle speed using a pulse generator (MPG) or switches (eg. rotary).

### GUI Configuration

Here you can set defaults for the display screens, add virtual control panels (VCP), and set some Machinekit options..

![images/pncconf-gui.png]()

Figure 12. GUI Configuration

 Frontend GUI Options   
The default options allows general defaults to be chosen for any display screen.

AXIS defaults are options specific to AXIS. If you choose size , position or force maximize options then PNCconf will ask if it’s alright to overwrite a preference file (.axisrc). Unless you have manually added commands to this file it is fine to allow it. Position and force max can be used to move AXIS to a second monitor if the system is capable.

Touchy defaults are options specific to Touchy. Most of Touchy’s options can be changed while Touchy is running using the preference page. Touchy uses GTK to draw its screen, and GTK supports themes. Themes controls the basic look and feel of a program. You can download themes from the net or edit them yourself. There are a list of the current themes on the computer that you can pick from. To help some of the text to stand out PNCconf allows you to override the Themes’s defaults. The position and force max options can be used to move Touchy to a second monitor if the system is capable.

 VCP options   
Virtual Control Panels allow one to add custom controls and displays to the screen. AXIS and Touchy can integrate these controls inside the screen in designated positions. There are two kinds of VCPs - pyVCP which uses *Tkinter* to draw the screen and GLADE VCP that uses *GTK* to draw the screen.

 PyVCP   
PyVCPs screen XML file can only be hand built. PyVCPs fit naturally in with AXIS as they both use TKinter.

HAL pins are created for the user to connect to inside their custom HAL file. There is a sample spindle display panel for the user to use as-is or build on. You may select a blank file that you can later add your controls *widgets* to or select a spindle display sample that will display spindle speed and indicate if the spindle is at requested speed.

PNCconf will connect the proper spindle display HAL pins for you. If you are using AXIS then the panel will be integrated on the right side. If not using AXIS then the panel will be separate *stand-alone* from the frontend screen.

You can use the geometry options to size and move the panel, for instance to move it to a second screen if the system is capable. If you press the *Display sample panel* button the size and placement options will be honoured.

 GLADE VCP   
GLADE VCPs fit naturally inside of TOUCHY screen as they both use GTK to draw them, but by changing GLADE VCP’s theme it can be made to blend pretty well in AXIS. (try Redmond)

It uses a graphical editor to build its XML files. HAL pins are created for the user to connect to, inside of their custom HAL file.

GLADE VCP also allows much more sophisticated (and complicated) programming interaction, which PNCconf currently doesn’t leverage. (see GLADE VCP in the manual)

PNCconf has sample panels for the user to use as-is or build on. With GLADE VCP PNCconf will allow you to select different options on your sample display.

Under *sample options* select which ones you would like. The zero buttons use HALUI commands which you could edit later in the HALUI section.

Auto Z touch-off also requires the classicladder touch-off program and a probe input selected. It requires a conductive touch-off plate and a grounded conductive tool. For an idea on how it works see: [Here](http://wiki.linuxcnc.org/cgi-bin/wiki.pl?ClassicLadderExamples#Single_button_probe_touchoff)

Under *Display Options*, size, position, and force max can be used on a *stand-alone* panel for such things as placing the screen on a second monitor if the system is capable.

You can select a GTK theme which sets the basic look and feel of the panel. You Usually want this to match the frontend screen. These options will be used if you press the *Display sample button*. With GLADE VCP depending on the frontend screen, you can select where the panel will display.

You can force it to be stand-alone or with AXIS it can be in the center or on the right side, with Touchy it can be in the center.

 Defaults and Options   
-   Require homing before MDI / Running

    -   If you want to be able to move the machine before homing uncheck this checkbox.

-   Popup Tool Prompt

    -   Choose between an on screen prompt for tool changes or export standard signal names for a User supplied custom tool changer Hal file

-   Leave spindle on during tool change:

    -   Used for lathes

-   Force individual manual homing

-   Move spindle up before tool change

-   Restore joint position after shutdown

    -   Used for non-trivial kinematics machines

-   Random position toolchangers

    -   Used for toolchangers that do not return the tool to the same pocket. You will need to add custom HAL code to support toolchangers.

### Mesa Configuration

The Mesa configuration pages allow one to utilize different firmwares. On the basic page you selected a Mesa card here you pick the available firmware and select what and how many components are available.

![images/pncconf-mesa-config.png]()

Figure 13. Mesa Configuration

Parport address is used only with Mesa parport card, the 7i43. An onboard parallel port usually uses 0x278 or 0x378 though you should be able to find the address from the BIOS page. The 7i43 requires the parallel port to use the EPP mode, again set in the BIOS page. If using a PCI parallel port the address can be searched for by using the search button on the basic page.

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
<td align="left">Many PCI cards do not support the EPP protocol properly.</td>
</tr>
</tbody>
</table>

PDM PWM and 3PWM base frequency sets the balance between ripple and linearity. If using Mesa daughter boards the docs for the board should give recommendations

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">It’s important to follow these to avoid damage and get the best performance.</td>
</tr>
</tbody>
</table>

    The 7i33 requires PDM and a PDM base frequency of 6 mHz
    The 7i29 requires PWM and a PWM base frequency of 20 Khz
    The 7i30 requires PWM and a PWM base frequency of 20 Khz
    The 7i40 requires PWM and a PWM base frequency of 50 Khz
    The 7i48 requires UDM and a PWM base frequency of 24 Khz

Watchdog time out is used to set how long the MESA board will wait before killing outputs if communication is interrupted from the computer. Please remember Mesa uses *active low* outputs meaning that when the output pin is on, it is low (approx 0 volts) and if it’s off the output in high (approx 5 volts) make sure your equipment is safe when in the off (watchdog bitten) state.

You may choose the number of available components by deselecting unused ones. Not all component types are available with all firmware.

Choosing less then the maximum number of components allows one to gain more GPIO pins. If using daughter boards keep in mind you must not deselect pins that the card uses. For instance some firmware supports two 7i33 cards, If you only have one you may deselect enough components to utilize the connector that supported the second 7i33. Components are deselected numerically by the highest number first then down with out skipping a number. If by doing this the components are not where you want them then you must use a different firmware. The firmware dictates where, what and the max amounts of the components. Custom firmware is possible, ask nicely when contacting the Machinekit developers and Mesa. Using custom firmware in PNCconf requires special procedures and is not always possible - Though I try to make PNCconf as flexible as possible.

After choosing all these options press the *Accept Component Changes* button and PNCconf will update the I/O setup pages. Only I/O tabs will be shown for available connectors, depending on the Mesa board.

### Mesa I/O Setup

The tabs are used to configure the input and output pins of the Mesa boards. PNCconf allows one to create custom signal names for use in custom HAL files.

![images/pncconf-mesa-io2.png]()

Figure 14. Mesa I/O C2

On this tab with this firmware the components are setup for a 7i33 daughter board, usually used with closed loop servos. Note the component numbers of the encoder counters and PWM drivers are not in numerical order. This follows the daughter board requirements.

![images/pncconf-mesa-io3.png]()

Figure 15. Mesa I/O C3

On this tab all the pins are GPIO. Note the 3 digit numbers - they will match the HAL pin number. GPIO pins can be selected as input or output and can be inverted.

![images/pncconf-mesa-io4.png]()

Figure 16. Mesa I/O C4

On this tab there are a mix of step generators and GPIO. Step generators output and direction pins can be inverted. Note that inverting a Step Gen-A pin (the step output pin) changes the step timing. It should match what your controller expects.

### Parport configuration

![images/pncconf-parport.png]()

The parallel port can be used for simple I/O similar to Mesa’s GPIO pins.

### Axis Configuration

![images/pncconf-axis-drive.png]()

Figure 17. Axis Drive Configuration

This page allows configuring and testing of the motor and/or encoder combination . If using a servo motor an open loop test is available, if using a stepper a tuning test is available.

 Open Loop Test   
An open loop test is important as it confirms the direction of the motor and encoder. The motor should move the axis in the positive direction when the positive button is pushed and also the encoder should count in the postie direction. The axis movement should follow the Machinery’s Handbook <span class="footnote">
\["axis nomenclature" in the chapter "Numerical Control" in the "Machinery’s Handbook" published by Industrial Press.\]
</span> standards or AXIS graphical display will not make much sense. Hopefully the help page and diagrams can help figure this out. Note that axis directions are based on TOOL movement not table movement. There is no acceleration ramping with the open loop test so start with lower DAC numbers. By moving the axis a known distance one can confirm the encoder scaling. The encoder should count even without the amp enabled depending on how power is supplied to the encoder.

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
<td align="left">If the motor and encoder do not agree on counting direction then the servo will run away when using PID control.</td>
</tr>
</tbody>
</table>

Since at the moment PID settings can not be tested in PNCconf the settings are really for when you re-edit a config - enter your tested PID settings.

DAC scaling, max output and offset are used to tailor the DAC output.

 Compute DAC   
These two values are the scale and offset factors for the axis output to the motor amplifiers. The second value (offset) is subtracted from the computed output (in volts), and divided by the first value (scale factor), before being written to the D/A converters. The units on the scale value are in true volts per DAC output volts. The units on the offset value are in volts. These can be used to linearize a DAC.

Specifically, when writing outputs, the Machinekit first converts the desired output in quasi-SI units to raw actuator values, e.g., volts for an amplifier DAC. This scaling looks like: The value for scale can be obtained analytically by doing a unit analysis, i.e., units are \[output SI units\]/\[actuator units\]. For example, on a machine with a velocity mode amplifier such that 1 volt results in 250 mm/sec velocity, Note that the units of the offset are in machine units, e.g., mm/sec, and they are pre-subtracted from the sensor readings. The value for this offset is obtained by finding the value of your output which yields 0.0 for the actuator output. If the DAC is linearized, this offset is normally 0.0.

The scale and offset can be used to linearize the DAC as well, resulting in values that reflect the combined effects of amplifier gain, DAC non-linearity, DAC units, etc. To do this, follow this procedure:

-   Build a calibration table for the output, driving the DAC with a desired voltage and measuring the result:

<table>
<caption>Table 1. Output Voltage Measurements</caption>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><strong>Raw</strong></p></td>
<td align="left"><p><strong>Measured</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>-10</p></td>
<td align="left"><p><strong>-9.93</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>-9</p></td>
<td align="left"><p><strong>-8.83</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>0</p></td>
<td align="left"><p><strong>-0.96</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p><strong>-0.03</strong></p></td>
</tr>
<tr class="even">
<td align="left"><p>9</p></td>
<td align="left"><p><strong>9.87</strong></p></td>
</tr>
<tr class="odd">
<td align="left"><p>10</p></td>
<td align="left"><p><strong>10.07</strong></p></td>
</tr>
</tbody>
</table>

-   Do a least-squares linear fit to get coefficients a, b such that meas=a\*raw+b

-   Note that we want raw output such that our measured result is identical to the commanded output. This means

    -   cmd=a\*raw+b

    -   raw=(cmd-b)/a

-   As a result, the a and b coefficients from the linear fit can be used as the scale and offset for the controller directly.

MAX OUTPUT: The maximum value for the output of the PID compensation that is written to the motor amplifier, in volts. The computed output value is clamped to this limit. The limit is applied before scaling to raw output units. The value is applied symmetrically to both the plus and the minus side.

**Tuning Test** The tuning test unfortunately only works with stepper based systems. Again confirm the directions on the axis is correct. Then test the system by running the axis back and forth, If the acceleration or max speed is too high you will lose steps. While jogging, Keep in mind it can take a while for an axis with low acceleration to stop. Limit switches are not functional during this test. You can set a pause time so each end of the test movement. This would allow you to set up and read a dial indicator to see if you are loosing steps.

**Stepper Timing** Stepper timing needs to be tailored to the step controller’s requirements. Pncconf supplies some default controller timing or allows custom timing settings . See [here](http://wiki.machinekit.org/cgi-bin/wiki.pl?Stepper_Drive_Timing) for some more known timing numbers (feel free to add ones you have figured out). If in doubt use large numbers such as 5000 this will only limit max speed.

**Brushless Motor Control** These options are used to allow low level control of brushless motors using special firmware and daughter boards. It also allows conversion of HALL sensors from one manufacturer to another. It is only partially supported and will require one to finish the HAL connections. Contact the mail-list or forum for more help.

![images/pncconf-scale-calc.png]()

Figure 18. Axis Scale Calculation

The scale settings can be directly entered or one can use the *calculate scale* button to assist. Use the check boxes to select appropriate calculations. Note that *pulley teeth* requires the number of teeth not the gear ratio. Worm turn ratio is just the opposite it requires the gear ratio. If your happy with the scale press apply otherwise push cancel and enter the scale directly.

![images/pncconf-axis-config.png]()

Figure 19. Axis Configuration

Also refer to the diagram tab for two examples of home and limit switches. These are two examples of many different ways to set homing and limits.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Important
</div></td>
<td align="left">It is very important to start with the axis moving in the right direction or else getting homing right is very difficult!</td>
</tr>
</tbody>
</table>

Remember positive and negative directions refer to the TOOL not the table as per the Machinists handbook.

On a typical knee or bed mill

-   when the TABLE moves out that is the positive Y direction

-   when the TABLE moves left that is the positive X direction

-   when the TABLE moves down that is the positive Z direction

-   when the HEAD moves up that is the positive Z direction

On a typical lathe

-   when the TOOL moves right, away from the chuck

-   that is the positive Z direction

-   when the TOOL moves toward the operator

-   that is the positive X direction. Some lathes have X

-   opposite (eg tool on back side), that will work fine but

-   AXIS graphical display can not be made to reflect this.

When using homing and / or limit switches Machinekit expects the HAL signals to be true when the switch is being pressed / tripped. If the signal is wrong for a limit switch then Machinekit will think the machine is on end of limit all the time. If the home switch search logic is wrong Machinekit will seem to home in the wrong direction. What it actually is doing is trying to BACK off the home switch.

Decide on limit switch location.

Limit switches are the back up for software limits in case something electrical goes wrong eg. servo runaway. Limit switches should be placed so that the machine does not hit the physical end of the axis movement. Remember the axis will coast past the contact point if moving fast. Limit switches should be *active low* on the machine. eg. power runs through the switches all the time - a loss of power (open switch) trips. While one could wire them the other way, this is fail safe. This may need to be inverted so that the HAL signal in Machinekit in *active high* - a TRUE means the switch was tripped. When starting Machinekit if you get an on-limit warning, and axis is NOT tripping the switch, inverting the signal is probably the solution. (use HALMETER to check the corresponding HAL signal eg. axis.0.pos-lim-sw-in X axis positive limit switch)

Decide on the home switch location.

If you are using limit switches You may as well use one as a home switch. A separate home switch is useful if you have a long axis that in use is usually a long way from the limit switches or moving the axis to the ends presents problems of interference with material. eg a long shaft in a lathe makes it hard to home to limits with out the tool hitting the shaft, so a separate home switch closer to the middle may be better. If you have an encoder with index then the home switch acts as a course home and the index will be the actual home location.

Decide on the MACHINE ORIGIN position.

MACHINE ORIGIN is what Machinekit uses to reference all user coordinate systems from. I can think of little reason it would need to be in any particular spot. There are only a few G codes that can access the MACHINE COORDINATE system.( G53, G30 and G28 ) If using tool-change-at-G30 option having the Origin at the tool change position may be convenient. By convention, it may be easiest to have the ORIGIN at the home switch.

Decide on the (final) HOME POSITION.

this just places the carriage at a consistent and convenient position after Machinekit figures out where the ORIGIN is.

Measure / calculate the positive / negative axis travel distances.

Move the axis to the origin. Mark a reference on the movable slide and the non-moveable support (so they are in line) move the machine to the end of limits. Measure between the marks that is one of the travel distances. Move the table to the other end of travel. Measure the marks again. That is the other travel distance. If the ORIGIN is at one of the limits then that travel distance will be zero.

 (machine) ORIGIN   
The Origin is the MACHINE zero point. (not the zero point you set your cutter / material at). Machinekit uses this point to reference everything else from. It should be inside the software limits. Machinekit uses the home switch location to calculate the origin position (when using home switches or must be manually set if not using home switches.

 Travel distance   
This is the maximum distance the axis can travel in each direction. This may or may not be able to be measured directly from origin to limit switch. The positive and negative travel distances should add up to the total travel distance.

 POSITIVE TRAVEL DISTANCE   
This is the distance the Axis travels from the Origin to the positive travel distance or the total travel minus the negative travel distance. You would set this to zero if the origin is positioned at the positive limit. The will always be zero or a positive number.

 NEGATIVE TRAVEL DISTANCE   
This is the distance the Axis travels from the Origin to the negative travel distance. or the total travel minus the positive travel distance. You would set this to zero if the origin is positioned at the negative limit. This will always be zero or a negative number. If you forget to make this negative PNCconf will do it internally.

 (Final) HOME POSITION   
This is the position the home sequence will finish at. It is referenced from the Origin so can be negative or positive depending on what side of the Origin it is located. When at the (final) home position if you must move in the Positive direction to get to the Origin, then the number will be negative.

 HOME SWITCH LOCATION   
This is the distance from the home switch to the Origin. It could be negative or positive depending on what side of the Origin it is located. When at the home switch location if you must move in the Positive direction to get to the Origin, then the number will be negative. If you set this to zero then the Origin will be at the location of the limit switch (plus distance to find index if used)

 Home Search Velocity   
Course home search velocity in units per minute.

 Home Search Direction   
Sets the home switch search direction either negative (ie. towards negative limit switch) or positive (ie. towards positive limit switch)

 Home Latch Velocity   
Fine Home search velocity in units per minute

 Home Final Velocity   
Velocity used from latch position to (final) home position in units per minute. Set to 0 for max rapid speed

 Home latch Direction   
Allows setting of the latch direction to the same or opposite of the search direction.

 Use Encoder Index For Home   
Machinekit will search for an encoder index pulse while in the latch stage of homing.

 Use Compensation File   
Allows specifying a Comp filename and type. Allows sophisticated compensation. See Manual.

 Use Backlash Compensation   
Allows setting of simple backlash compensation. Can not be used with Compensation File. See Manual.

![images/pncconf-diagram-lathe.png]()

Figure 20. AXIS Help Diagram

The diagrams should help to demonstrate an example of limit switches and standard axis movement directions. In this example the Z axis was two limit switches, the positive switch is shared as a home switch. The MACHINE ORIGIN (zero point) is located at the negative limit. The left edge of the carriage is the negative trip pin and the right the positive trip pin. We wish the FINAL HOME POSITION to be 4 inches away from the ORIGIN on the positive side. If the carriage was moved to the positive limit we would measure 10 inches between the negative limit and the negative trip pin.

### Spindle Configuration

If you select spindle signals then this page is available to configure spindle control.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Tip
</div></td>
<td align="left">Many of the option on this page will not show unless the proper option was selected on previous pages!</td>
</tr>
</tbody>
</table>

![images/pncconf-spindle-config.png]()

Figure 21. Spindle Configuration

This page is similar to the axis motor configuration page.

There are some differences:

-   Unless one has chosen a stepper driven spindle there is no acceleration or velocity limiting.

-   There is no support for gear changes or ranges.

-   If you picked a VCP spindle display option then spindle-at-speed scale and filter settings may be shown.

-   Spindle-at-speed allows Machinekit to wait till the spindle is at the requested speed before moving the axis. This is particularly handy on lathes with constant surface feed and large speed diameter changes. It requires either encoder feedback or a digital spindle-at-speed signal typically connected to a VFD drive.

-   If using encoder feedback, you may select a spindle-at-speed scale setting that specifies how close the actual speed must be to the requested speed to be considered at-speed.

-   If using encoder feedback, the VCP speed display can be erratic - the filter setting can be used to smooth out the display. The encoder scale must be set for the encoder count / gearing used.

-   If you are using a single input for a spindle encoder you must add the line: setp hm2\_7i43.0.encoder.00.counter-mode 1 (changing the board name and encoder number to your requirements) into a custom HAL file. See the Hostmot2 section on encoders for more info about counter mode.

### Advanced Options

This allows setting of HALUI commands and loading of classicladder and sample ladder programs. If you selected GLADE VCP options such as for zeroing axis, there will be commands showing. See the manual about info on HALUI for using custom halcmds. There are several ladder program options. The Estop program allows an external ESTOP switch or the GUI frontend to throw an Estop. It also has a timed lube pump signal. The Z auto touch-off is with a touch-off plate, the GLADE VCP touch-off button and special HALUI commands to set the current user origin to zero and rapid clear. The serial modbus program is basically a blank template program that sets up classicladder for serial modbus. See the classicladder section in the manual.

![images/pncconf-advanced.png]()

Figure 22. Advanced Options

### HAL Components

On this page you can add additional HAL components you might need for custom HAL files. In this way one should not have to hand edit the main HAL file, while still allowing user needed components.

![images/pncconf-hal.png]()

Figure 23. HAL Components

The first selection is components that pncconf uses internally. You may configure pncconf to load extra instances of the components for your custom HAL file.

Select the number of instances your custom file will need, pncconf will add what it needs after them.

Meaning if you need 2 and pncconf needs 1 pncconf will load 3 instances and use the last one.

 Custom Component Commands   
This selection will allow you to load HAL components that pncconf does not use. Add the loadrt or loadusr command, under the heading *loading command* Add the addf command under the heading *Thread command*. The components will be added to the thread between reading of inputs and writing of outputs, in the order you write them in the *thread command*.

### Advanced Usage Of PNCconf

PNCconf does its best to allow flexible customization by the user. PNCconf has support for custom signal names, custom loading of components, custom HAL files and custom firmware.

There are also signal names that PNCconf always provides regardless of options selected, for user’s custom HAL files With some thought most customizations should work regardless if you later select different options in PNCconf.

Eventually if the customizations are beyond the scope of PNCconf’s frame work you can use PNCconf to build a base config or use one of Machinekit’s sample configurations and just hand edit it to what ever you want.

 Custom Signal Names   
If you wish to connect a component to something in a custom HAL file write a unique signal name in the combo entry box. Certain components will add endings to your custom signal name:

Encoders will add &lt; customname &gt; +:

-   position

-   count

-   velocity

-   index-enable

-   reset

Steppers add:

-   enable

-   counts

-   position-cmd

-   position-fb

-   velocity-fb

PWM add:

-   enable

-   value

GPIO pins will just have the entered signal name connected to it

In this way one can connect to these signals in the custom HAL files and still have the option to move them around later.

 Custom Signal Names   
The Hal Components page can be used to load components needed by a user for customization.

 Loading Custom Firmware   
PNCconf searches for firmware on the system and then looks for the XML file that it can convert to what it understands. These XML files are only supplied for officially released firmware from the Machinekit team. To utilize custom firmware one must convert it to an array that PNCconf understands and add its filepath to PNCconf’s preference file. By default this path searches the desktop for a folder named custom\_firmware and a file named firmware.py.

The hidden preference file is in the user’s home file, is named .pncconf-preferences and require one to select *show hidden files* to see and edit it. The contents of this file can be seen when you first load PNCconf - press the help button and look at the output page.

Ask on the Machinekit mail-list or forum for info about converting custom firmware. Not all firmware can be utilized with PNCconf.

 Custom HAL Files   
There are four custom files that you can use to add HAL commands to:

-   custom.hal is for HAL commands that don’t have to be run after the GUI frontend loads. It is run after the configuration-named HAL file.

-   custom\_postgui.hal is for commands that must be run after AXIS loads or a standalone PYVCP display loads.

-   custom\_gvcp.hal is for commands that must be run after glade VCP is loaded.

-   shutdown.hal is for commands to run when Machinekit shuts down in a controlled manner.

Running Machinekit
------------------

<span id="cha:running-emc"></span>

### Invoking Machinekit

After installation, Machinekit starts just like any other Linux program: run it from the terminal by issuing the command *machinekit*,

### Configuration Selector

By default, the Configuration Selector dialog is shown when you first run Machinekit. Your own personalized configurations are shown at the top of the list, followed by sample configurations. Because each sample configuration is for a different type of hardware interface, almost all will not run without the hardware installed. The configurations listed under the category *sim* run entirely without attached hardware.

![images/configuration-selector.png]()

Figure 24. Machinekit Configuration Selector<span id="cap:Machinekit-Configuration-Selector"></span>

Click any of the listed configurations to display specific information about it. Double-click a configuration or click OK to start the configuration. Select *Create Desktop Shortcut* and then click OK to add an icon on the Debian desktop to directly launch this configuration without showing the Configuration Selector screen.

When you select a configuration from the Sample Configurations section, it will automaticly place a copy of that config in the emc/configs directory.

### Next steps in configuration

After finding the sample configuration that uses the same interface hardware as your machine, and saving a copy to your home directory, you can customize it according to the details of your machine. Refer to the Integrator Manual for topics on configuration.

Linux FAQ
---------

<span id="cha:linux-faq"></span>

These are some basic Linux commands and techniques for new to Linux users. More complete information can be found on the web or by using the man pages.

### Man Pages<span id="sec:Man-Pages"></span>

Man pages are automatically generated manual pages in most cases. Man pages are usually available for most programs and commands in Linux.

To view a man page open up a terminal window by going to *Applications &gt; Accessories &gt; Terminal*. For example if you wanted to find out something about the find command in the terminal window type:

    man find

Use the Page Up and Page Down keys to view the man page and the Q key to quit viewing.

### List Modules

Sometimes when troubleshooting you need to get a list of modules that are loaded. In a terminal window type:

    lsmod

If you want to send the output from lsmod to a text file in a terminal window type:

    lsmod > mymod.txt

The resulting text file will be located in the home directory if you did not change directories when you opened up the terminal window and it will be named mymod.txt or what ever you named it.

### Editing a Root File<span id="sec:Editing-a-Root-File"></span>

When you open the file browser and you see the Owner of the file is root you must do extra steps to edit that file. Editing some root files can have bad results. Be careful when editing root files. Generally, you can open and view most root files, but they will open in *read only* mode.

#### The Command Line Way

Open up *Applications &gt; Accessories &gt; Terminal*.

In the terminal window type

    sudo gedit

Open the file with File &gt; Open &gt; Edit

#### The GUI Way

1.  Right click on the desktop and select Create Launcher

2.  Type a name in like sudo edit

3.  Type *gksudo "gnome-open %u"* as the command and save the launcher to your desktop

4.  Drag a file onto your launcher to open and edit

#### Root Access

In Debian you can become root by typing in "sudo -i" in a terminal window then typing in your password. Be careful, because you can really foul things up as root if you don’t know what you’re doing.

### Terminal Commands<span id="sec:Terminal-Commands"></span>

#### Working Directory

To find out the path to the present working directory in the terminal window type:

    pwd

#### Changing Directories

To move up one level in the terminal window type:

    cd ..

To move up two levels in the terminal window type:

    cd ../..

To move down to the emc2/configs subdirectory in the terminal window type:

    cd emc2/configs

#### Listing files in a directory

To view a list of all the files and subdirectories in the terminal window type:

    dir

or

    ls

#### Finding a File

The find command can be a bit confusing to a new Linux user. The basic syntax is:

    find starting-directory parameters actions

For example to find all the .ini files in your emc2 directory you first need to use the pwd command to find out the directory.
 Open a new terminal window and type:

    pwd

And pwd might return the following result:

    /home/joe

With this information put the command together like this:

    find /home/joe/machinekit -name \*.ini -print

The -name is the name of the file your looking for and the -print tells it to print out the result to the terminal window. The \\\*.ini tells find to return all files that have the .ini extension. The backslash is needed to escape the shell meta-characters. See the find man page for more information on find.

#### Searching for Text

    grep -irl 'text to search for' *

This will find all the files that contain the *text to search for* in the current directory and all the subdirectories below it, while ignoring the case. The -i is for ignore case and the -r is for recursive (include all subdirectories in the search). The -l option will return a list of the file names, if you leave the -l off you will also get the text where each occourance of the "text to search for" is found. The \* is a wild card for search all files. See the grep man page for more information.

#### Bootup Messages

To view the bootup messages use "dmesg" from the command window. To save the bootup messages to a file use the redirection operator, like this:

    dmesg > bootmsg.txt

The contents of this file can be copied and pasted on line to share with people trying to help you diagnose your problem.

To clear the message buffer type this:

    sudo dmesg -c

This can be helpful to do just before launching Machinekit, so that there will only be a record of information related to the current launch of Machinekit.

To find the built in parallel port address use grep to filter the information out of dmesg.

After boot up open a terminal and type:

    dmesg|grep parport

### Convenience Items

#### Terminal Launcher

If you want to add a terminal launcher to the panel bar on top of the screen you typically can right click on the panel at the top of the screen and select "Add to Panel". Select Custom Application Launcher and Add. Give it a name and put gnome-terminal in the command box.

### Hardware Problems

#### Hardware Info

To find out what hardware is connected to your motherboard in a terminal window type:

    lspci -v

### Paths

Relative Paths

Relative paths are based on the startup directory which is the directory containing the ini file. Using relative paths can facilitate relocation of configurations but requires a good understanding of linux path specifiers.

       ./f0        is the same as f0, e.g., a file named f0 in the startup directory
       ../f1       refers to a file f1 in the parent directory
       ../../f2    refers to a file f2 in the parent of the parent directory
       ../../../f3 etc.

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


