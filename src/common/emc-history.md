Machinekit History
==================

<span id="cha:machinekit-history"></span>

EMC (the Enhanced Machine Controller) was created by [NIST](http://www.nist.gov/index.html) , the National Institute of Standards and Technology, which is an agency of the Commerce Department of the United States government.

NIST first became interested in writing a motion control package as a test platform for concepts and standards. Early sponsorship from General Motors resulted in an adaptation of the fledgling version of EMC using PMAC intelligent control boards running under a *real time* version of Windows NT and controlling a large milling machine.

As is required of all *work product* of US federal government employees, the resulting software and the report about it are required to be in the public domain and a report about it was duly published, including on the Internet. It was there that Matt Shaver discovered EMC. He contacted NIST and entered into discussions with Fred Proctor about adapting the code for use in controlling less expensive hardware to be used for upgrades and replacements of CNC controls that were obsolete or just plain dead. NIST was intrigued because they too wanted something less expensive. In order to launch a cooperative effort, a formal agreement was created which guaranteed that the resulting code and design would remain in the public domain.

Early considerations focused on replacing the expensive and temperamental *real time* Windows NT system. It was proposed that a relatively new (at the time) real time extension of the Linux operating system be tried. This idea was pursued with success. Next up was the issue of the expensive intelligent motion control boards. By this time the processing power of a PC was considered great enough to directly take control of the motion routines. A quick search of available hardware resulted in the selection of a [Servo-To-Go](http://www.servotogo.com/) interface board as the first platform for letting the PC directly control the motors. Software for trajectory planning and PID loop control was added to the existing user interface and RS274 interpreter. Matt successfully used this version to upgrade a couple of machines with dead controls and this became the EMC system that first caught the attention of the outside world. Mention of EMC on the Rec.Crafts.Metalworking USENET discussion group resulted in early adopters like [Jon Elson](http://pico-systems.com/motion.html) building systems to take advantage of EMC.

NIST set up a mailing list for people interested in EMC. As time went on, others outside NIST became interested in improving EMC. Many people requested or coded small improvements to the code. Ray Henry wanted to refine the user interface. Since Ray was reluctant to try tampering with the C code in which the user interface was written, a simpler method was sought. Fred Proctor of NIST suggested a scripting language and wrote code to interface the Tcl/Tk scripting language to the internal NML communications of EMC. With this tool Ray went on to write a Tcl/Tk program that is the user interface in predominate use with EMC today.

By this time interest in EMC as beginning to pick up substantially. As more and more people began to attempt installation of EMC, the difficulty of patching a Linux kernel with the real time extensions and of compiling the EMC code became glaringly obvious. Many attempts to document the process and write scripts were attempted, some with moderate success. The problem of matching the correct version of the patches and compilers with the selected version of Linux kept cropping up. Paul Corner came to the rescue with the BDI (brain dead install) which was a CD from which a complete working system (Linux, patches, and EMC) could be installed. Paul’s BDI has evolved into a bootable CD that can be run on most PC style computers to *test drive* EMC without overwriting the existing system on the hard disk. Once the user decides that they want a permanent EMC system, the same CD can install a complete EMC/Linux environment. The BDI approach has opened the world of EMC to a much larger potential user community. As this community became larger, the EMC mailing list and code archives were moved to the SourceForge and the Machinekit web site was established.

With a larger community of users participating, EMC became a major focus of interest at the on-going CNC exhibits at NAMES and NAMES became the annual meeting event for EMC. For the first couple of years, the meetings just happened because the interested parties were at NAMES. In 2003 the EMC user community had its first announced public meeting. It was held the Monday after NAMES in the lobby of the arena where the NAMES show was held. Organization was loose, but the idea of a hardware abstraction layer (HAL) was born and the movement to restructure the code for ease of development (EMC2) was proposed.

In the spring of 2011, the Machinekit Board of Directors was contacted by a law firm representing EMC Corporation (www.emc.com) about the use of "EMC" and "EMC2" to identify the software offered on machinekit.org. EMC Corporation has registered various trademarks relating to EMC and EMC² (EMC with superscripted numeral two).

After a number of conversations with the representative of EMC Corporation, the final result is that, starting with the next major release of the software, machinekit.org will stop identifying the software using "emc" or "EMC", or those terms followed by digits. To the extent that the Machinekit Board of Directors controls the names used to identify the software offered on machinekit.org, the board has agreed to this.

As a result, it was necessary to choose a new name for the software. Of the options the board considered, there was consensus that "Machinekit" is the best option, as this has been our website’s name for years.

In preparation for the new name, we have received a sub-license of the LINUX® trademark from the Linux Foundation (www.linuxfoundation.org), protecting our use of the Machinekit name. (LINUX® is the registered trademark of Linus Torvalds in the U.S. and other countries.)

The rebranding effort will include the machinekit.org website, the IRC channels, and versions of the software and documentation starting with 2.5.0. Rebranding will begin right away.

A paper written from NIST’s perspective on the history of EMC and its transition to open source is available [here](http://www.nist.gov/manuscript-publication-search.cfm?pub_id=821651).

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

