Unified Build for Multiple RT Operating Systems
===============================================

<span id="cha:ubpoos"></span>

This document describes the Unified RTOS Build, work by Michael Haberler, John Morris and Charles Steinkuehler that integrates new real-time thread flavors into Machinekit/Machinekit.

The Rationale for Multiple Realtime Operating Systems Support
-------------------------------------------------------------

### Considerations

A key aspect of Machinekit is its realtime capability. Up to about 2011, Machinekit essentially relied on a single realtime Linux extension, namely RTAI (<http://www.rtai.org>). Originally there was a second option, RT-Linux, but that product faded away over time and support for it in Machinekit has suffered substantial "bit rot." Realistically, RT-Linux has not been a viable option for Machinekit for years.

As the old adage goes, *don't put all your eggs in one basket*. Reliance on RTAI has become a more pressing issue over time as some question marks have arisen over RTAI’s long term viability and the robustness of its community. Several alternatives - some of them freely available open source - have appeared, namely the RT-PREEMPT Linux realtime patch (<https://rt.wiki.kernel.org>) and the Xenomai Realtime kernel (<http://www.xenomai.org>). Regardless, there have been no coordinated attempts to widen the Machinekit support base for other realtime operating systems (from here on called 'RTOS').

There have been some isolated efforts to remedy this single-source situation a bit, namely the MiniEMC2 Project by Sergey Kaydalov (Xenomai-based) and work by Michael Büsch to support a RT-PREEMPT patched kernel, as well as several others. Various of them have been based on outdated versions of Machinekit or have relied on custom kernels, making them rather less useful.

The original focus of the RTOS work therefore has been to bring together these isolated efforts, bring them up to date, integrate them into the Machinekit build process, and provide means to make the process of supporting another future RTOS option easier.

A second aspect has been its reliance on the x86 architecture, largely driven by the fact that RTAI is not available for non-x86 architectures. While mostly written in portable languages, some x86 assumptions and inline assembly code have crept in over time into the Machinekit code base, meaning builds for other architectures were challenging.

A third aspect of Machinekit has been its packaging. While the current method of distribution and installation continues to work for serious users, its relatively monolithic nature hinders the wider acceptance of Machinekit. Getting Machinekit into major software repositories has been hampered by the fact that a build based on a non-mainstream kernel extension is difficult to get accepted for inclusion in major distributions like Debian or Fedora. However, since RT-PREEMPT is becoming more mainstream, and a stock RT-PREEMPT kernel is in fact now available through the Debian wheezy package stream, this is becoming an option which wasn’t available before.

### Evolution of the work

The original focus of the recent RTOS work in Machinekit was to bring together the isolated efforts mentioned above, bring them up to date, integrate them into the Machinekit build process, and provide means to make the process of supporting another future RTOS option easier.

The result of this work has been branches which enable the use of RTAI, Xenomai or RT-PREEMPT kernels as a build-time option; that is, one had to build Machinekit for a particular kernel, and all the kernel-specific support would be linked into this binary. That binary would run only on the kernel it was built for, but not any others.

While it has resulted in usable and useful code, this approach has several downsides. First, only an RT-PREEMPT build might make it into Debian; any package based on RTAI or Xenomai is still unlikely to be accepted. Second, the per-RTOS build approach increases the build-distribute-support burden: now there is one package per RTOS. Also, having only one option, but not others, being available through simple standard commands like `apt-get install machinekit` is less than helpful.

Viewing more closely, this was essentially a design and build system defect of Machinekit, which has used a monolithic build approach. The supporting operating systems have for almost two decades provided a mechanism to separate the upgrade of supporting infrastructure - like runtime libraries - from the upgrade of applications proper, however, Machinekit has not made this transition until recently.

Therefore, the driving factor for the *unified build* work on Machinekit has been: separate out the specifics of a particular RTOS from the rest of the software build by packaging the RTOS-specific parts into shared libraries and runtime-loadable modules. There is no particular magic behind this step, it is essentially tracking the status quo how modern software packages are built.

The aim of the *unified build* work can be described as follows:

-   make Machinekit’s major software components build without regard of any specific realtime operating system by providing an RTOS-independent abstraction layer

-   package those specifics as separately built loadable (and distributable, hence upgradable modules and libraries), likely as packages

-   provide hooks in Machinekit to determine the underlying RTOS, and load those parts of the RTOS-specific modules as needed.

The upsides of such an approach are manifold:

-   the main Machinekit build, together with its Debian-compatible RTOS support, becomes amenable for major distro inclusion

-   the build and distribution logistics for the Machinekit project are substantially simplified

-   bugs in the supporting infrastructure can be fixed by upgrading a support package, or maybe just a single file - without wholesale pulling of repositories and constant rebuilds

-   due to the RTOS abstraction layer, it becomes rather easy to include support for a new RTOS option should it arise.

-   the drive to remove x86 dependencies has resulted in a code base which not only builds but also routinely runs on other architectures, giving a welcome exposure to remaining architecture dependencies (which have turned out to be rather few)

There are downsides: - the abstraction layer and some of the RTOS support has not seen wide use yet, meaning bugs are likely. - the build process has become substantially more complex, suggesting work will be needed for new platforms.

New Features
------------

 support for Xenomai and RT-PREEMPT realtime threads besides RTAI   
There should be minimal user configuration changes for using the new RT options.

 kernel autodetection   
The *unified build* branch will detect the RT features of the running kernel and choose an appropriate thread flavor.

 runtime loading of support modules   
All thread-specific code has been wrapped into shared objects and libraries which are loaded on demand. This enables fixes, upgrades or tests by just exchanging a file.

 unified thread status reporting   
The procfs support (/proc/rtapi etc) is useful for kernel threads only. This has been superseded by status reporting through halcmd which applies to all threads, and includes support for thread-system specific values.

 multiple instance support   
RTAPI and HAL have been extended to support several instances running in parallel on a single CPU. Kernel threads instances export a new sysfs entry /proc/rtapi/instance - without that it is rather cumbersome to determine the instance id - something which is relevant for startup and collision detection.

 common shared memory subsystem   
The deprecated System V IPC code has been removed and replaced by Posix shared memory. Alternatively, userland flavors may (kernel thread flavors must) use the new common shared memory driver (shmdrv). This removes the need to employ RTOS-specific allocators, hence their restrictions do not apply. The common shared memory driver has the important property that shared memory segments created in a user process can be attached later by kernel modules, which significantly simplifies startup, shutdown and - later - interworking of instances (for instance cross-linking of pins and ringbuffers). See also the section *Multiple instances and the shmdrv shared memory driver*.

 common startup/shutdown   
some effort has been made to create a common flow of startup and shutdown sequence. This work is not complete (see below under *Further work*).

 unified logging   
All logging out of RTAPI, RT HAL and user HAL components goes through a single, operating system independent channel which works identically for userland as well as kernel thread flavors.

 separate global log levels for user components and RT   
these loglevels can be set at startup, or through halcmd (`log rt <level>` and `log user <level>`). The user logging level applies to all processes, as it is now a global variable.

 debut of ringbuffer code   
This work by Pavel Shramov and myself \[MH\] will form a key element of the subsequent new middleware infrastructure which will replace NML. At the moment it is used for the unified logging code and works flawless (kudos to Pavel!). See src/rtapi/ring.h.

 no more inline assembly   
The last remnants of x86 inline assembly code have been removed and replaced by equivalent gcc/llvm intrinsic operations, meaning the core code should compile on pretty much any modern architecture. (src/rtapi/rtapi\_bitops.h)

 HAL segment size configurable   
This used to be a compiled-in constant. It is now a startup environment variable (HAL\_SIZE).

 exception handling - separating mechanism and policy   
The rather ad-hoc reporting of RTAPI exceptions (like realtime delays, traps due to invalid floating point operations etc) have been replaced by a redefinable exception handler which works identically across all flavors. The core RTAPI code supports collecting such exceptions and funneling through this exception handler; however, it is now possible to define - through a normal HAL component - how these exceptions are dealt with (see src/hal/components/rtmon.comp). There is a default exception handler in place which just logs exceptions.

 support for thread-specific RT status collection   
Status collection for RT threads is important to track down sources of delays, but it incurs overhead. By making this an optional RTAPI method which can be called by a thread function this can be applied as needed, using a standard mechanism.

 single `./configure && make` run builds many RT options   
The Unified Build feature reworks the build system and runtime scripts so that binaries for many RT thread flavors and many kernels may be built in a single run and installed on a single host without conflict. In use, thread flavors applicable to the current running environment are user-selectable with a sensible default. These features simplify distribution and remove barriers for including Machinekit packages in the big distributions.

Principles of Operations
------------------------

The overall structure and cooperation of major components is a bit different from the past modus operandi.

### Major data structures

Before the *unified build* work was undertaken, the RT build (RTAI) used a shared memory segment at the RTAPI layer and a segment at the HAL layer. The *simulator mode* only used the HAL segment, no RTAPI shared memory segment.

In contrast to the earlier approach, the shared memory segments in use in the *unified build* branch are:

#### The Global Data Segment

This is a per-instance shared memory segment which is assumed to exist before any RT operations start (either flavor). It carries parameters which apply globally to the instance (thread flavor, log levels, thread statistics etc). It also carries the ring buffer transporting messages generated by rtapi\_print\_msg() et al from wherever they were generated into the rtapi\_msgd address space, from which the messages are logged to syslog and - optionally - to stderr.

By intent, this segment must work with any thread flavor as-is (i.e. without layout changes). Any structures which are thread flavor specific hence must be represented as union types - see for instance the thread statistics structs (see rtapi\_threadstatus\_t in src/rtapi/rtapi\_global.h and src/rtapi/rtapi\_exception.h (admittedly disputable file naming)).

The driving factor for introducing this segment was recognizing the fact that there needs to be a mechanism to coordinate per-instance operations; the options are too diverse and heuristic in detection to relegate decisions of per-instance nature to autodetection mechanisms at lower levels.

#### The HAL Data Segment

Besides small changes in per-object (thread, component, pin etc) structures there are no major changes except provisions for a configurable segment size, plus data structures and macros/functions to access foreign instance HAL data segments. This is not used extensively in the current branch.

#### The RTAPI Data Segment

The RTAPI data segment is essentially unchanged in layout respective to previous versions.

One major difference is that the userland thread flavors do not employ a shared memory segment for RTAPI data as it is all local variables in the rtapi\_app process. In retrospect this lack of uniformity was a mistake, although not a showstopper.

### Relation of the major data structures

The obvious candidates for the global segment is the logging ringbuffer, plus key parameters driving overall instance parameters. As it is known to exist when any RT operations commences, over time some parameters and statistics structures have found a better place here than in one of the other segments.

I \[MH\] have considered merging the global and RTAPI data segments; however, from a stability perspective it is a good decision to relegate access to RTAPI data to *need to know* entities.

It would be a worthwhile effort to reintroduce the RTAPI shared memory segment for userland threads flavor; provisions have been made for this at the per-flavor configuration information (follow the logic of FLAVOR\_RTAPI\_DATA\_IN\_SHM usage to see how).

### Multiple instances and the role of the `shmdrv` shared memory driver

Running multiple RTAPI instances side by side will make sense eventually, for instance for multi-spindle setups. However, these instances will need to interact in some way at the HAL level, and that feature (tentatively called *crosslinking*) applies to pins, and messages through ringbuffers, and it is already working in a development branch.

For this to work, the prerequisite is that instances access the HAL data segment of a foreign instance. That in turn suggests that access to shared memory segments must happen in a uniform (i.e. thread- flavor unspecific) way, and regardless whether the instance is a kernel-threads or user-threads flavor. The status ante however was that flavors employed all sorts of different shared memory mechanisms - the RTAI-specific method, the Xenomai-specific method, and the deprecated System V IPC calls for the simulator build. However, for RTAPI purposes there is absolutely no reason to use the flavor-specific shm API’s since there is no shared memory allocation or deallocation in an RT thread; all this happens during module init and exit routines, and therefore in a non-RT context.

The solution looks as follows:

-   as long as only userland threads instances are used, Posix shared memory does the job.

-   if userland and kernel threads instances are used, Posix shm - as a user process API - cannot be used, in which all instances use the shared memory driver.

Hence, userland thread flavors use the shmdrv method if the corresponding kernel module is loaded, or Posix shm otherwise; kernel threads instances (RTAI, Xenomai) must use the shmdrv facilities and do so through an in-kernel API (see src/rtapi/shmdrv/shmdrv.h for the kernel and userland API’s; the latter is provided through the routines in src/rtapi/shmdrv/shmdrvapi.c).

Hence, shmdrv does not fit the normal module loading and unloading scheme very well as its lifetime transcends a particular instance using it. Consider the following scenario:

-   a userland threads and a kernel threads instance are to be run, and have HAL crosslinking capabilities.

-   first, shmdrv must be loaded

-   now userland or kernel threads instance can be started and will cooperate fine through the shmdrv API.

-   when either instance is shut down, the other instance continues to use the shmdrv API - either in-kernel or from userland through mmap().

Hence, neither instance shutdown may unload shmdrv (and it will not succeed anyway). Normally, the method to employ is reference counts, allowing an unload to succeed when the last reference has gone away. However, due to current restrictions of how shm segments are handled there is a chance that orphaned shared memory segments will *hang around* making problems on instance restart. This is currently being addressed (tracker entry \#26).

A key reason why shmdrv was done is the sequencing of operations; some of the flavor-specific shm API’s do not support access of a shm segment by a kernel module which was previously created by a user process. This is a severe restriction not only for instance interoperability, but also for startup and shutdown.

### Major Processes

#### The rtapi\_msgd Process

The primary purpose of the rtapi\_msgd process is to create, populate and service the per-instance global\_data\_t shared memory segment. In detail, the jobs are:

-   determine the thread flavor applicable to this instance, and set variables accordingly

-   accept per-instance options, like RT and userland message levels, HAL data segment size, HAL stack size etc, use of the shmdrv shared memory driver etc

-   populate the global segment with these values

-   poll the message ring buffer for new messages generated by rtapi\_print\_msg() in other components and log them to syslog (optionally to stderr too).

-   in case of userland thread flavors, observe the rtapi\_app process (see below) and shut down if it goes away.

The rtapi\_msgd changes its argv to `msgd:<instance number>` once started successfully to aid duplicate startup attempt detection, and instance shutdown.

#### The rtapi\_app Process (Userland threads)

This is based largely on the sim\_rtapi\_app process used in the *simulator environment* in previous releases. It is present only in userland thread flavors, and is the process context where RT threads run. What is does is:

-   attach to the global segment prepared by rtapi\_msgd, inheriting essential parameters and data structure access

-   harden memory for RT use (pre-faulting and locking memory etc)

-   privilege handling - RT process access I/O hardware

-   load the rtapi.so and hal\_lib.so components applicable for the thread flavor

-   accept commands over a Unix domain socket

The commands accepted are all generated by halcmd (for instance `loadrt compname` causing rtapi\_app to find and dlopen() the corresponding shared object, and calling the rtapi\_app\_init() functions on load, as well as rtapi\_app\_exit() on `unloadrt compname`.

It is possible to manually call rtapi\_app for debugging purposes; see scripts/realtime and the halcmd code in hal/utils how to do that.

The rtapi\_app program changes its argv to `rtapi:<instance number>` once started successfully.

### Kernel threads

With RTAI and Xenomai-kernel flavors, there is no corresponding rtapi\_app process since HAL modules are just kernel modules. There is no conceptual change here - modules are inserted by the setuid module\_helper.

Tested Operating Systems
------------------------

 rtai 2.6.32-122-rtai   
as used in the 10.04LTS live CD

 rtai 3.5.7   
Schooner/Arceye/Mick Private Bin kernel - reported to work, Axis screenshot seen. See the Machinekit Forum (<http://forum.machinekit.org>) and the emc-developer email list for ongoing discussions.

 xenomai 3.5.7-2.6.2.1 i686 and x86\_64   
John’s \[JM\] Xenomai kernel, see <http://wiki.machinekit.org/cgi-bin/wiki.pl?XenomaiKernelPackages>

 3.2.0-4-rt-amd64   
as per wheezy distro (x86\_64)

 3.8.13xenomai-bone23   
xenomai 2.6.2.1 for beaglebone running wheezy

No attempt has been made, and none will be made, for the hardy RTAI kernel.

### Tested Distros

Debian Lucid, Debian Precise, Debian Wheezy

### i386/x86\_64 Compatibility

The OS architecture (i386/i686 versus x86\_64) must be identical in the build and run environments - building Machinekit on say an i386 kernel and trying to run the result when booting an x86\_64 kernel will not work.

### Universal Build Changes

The Universal Build supports building for all RT environments in a single `./configure && make` run, and supports simultaneous installation and execution of all RT environments on a single host.

The run-time setup and teardown processes needed new mechanisms for retrieving flavor configuration and for locating separate run-time binaries with separate paths when setting up or tearing down the various RT environments.

In turn, the build system needed new mechanisms for configuring and building for each target RT flavor, keeping all build objects separate to maintain dependency integrity.

The changes to the run-time and build systems to effect these requirements are described here.

#### Run-Time System Changes

With the Universal Build, support for many RT environments may be simultaneously installed on a host system. Each of the five supported RT flavors has its own separate configuration and its own set of RTAPI and support binaries that may not be mixed with other RT flavors. With support for multiple RT flavors installed on a single host, for example, it is possible for an operator to boot a Xenomai kernel and run Machinekit in any of the POSIX, Xenomai userland or Xenomai kernel RT thread flavors (chosen at run-time). She may then shut down and boot an RT\_PREEMPT kernel, and then run Machinekit in either of the POSIX or RT\_PREEMPT RT thread flavors.

To make this possible requires compiling and installing binaries separately when those depend upon flavor, enabling configuration to be separately specified for each flavor, and refactoring run-time initialization code to enable selecting a flavor and loading matching configuration and binaries.

Flavor configuration is separated by replacing the old `rtapi.conf` shell script with an INI-style `etc/machinekit/rtapi.ini` containing per-flavor configuration sections.

Per-flavor binary files are kept separate by adding flavor and kernel version to ensure non-conflicting file paths. RTAPI modules are built into `rtlib/<flavor>` for userland styles and `rtlib/<flavor>/<kernel-version>` for kernel styles. Userland style modules have a matching `bin/rtapi_app_<flavor>` executable. The ULAPI module is built into `lib/ulapi-<flavor>.so`.

Run-time environment initialization starts in the `realtime` script. It obtains run-time parameters from `flavor` executable output and the `rtapi.ini` configuration file. It starts `rtapi_msgd`, before performing flavor-specific initialization, described next.

For kernel threads systems, the script runs `libexec/machinekit_module_helper` to load each kernel module listed in `rtapi.ini`. `machinekit_module_helper` looks for the named kernel module in `rtlib/<flavor>/<kernel-version>`, and optionally in the `RTDIR` parameter from `rtapi.ini` (needed for RTAI), and loads the module.

For userland threads, the `realtime` script start the `rtapi_app` executable defined in `rtapi.ini`, `libexec/rtapi_app_<flavor>` by default. During the build, the linker sets an `rpath` pointing to the modules directory, `rtlib/<flavor>` for run-in-place builds, so that `rtapi_app` may `dlopen()` the module with no need to read module path location from the configuration file.

On the ULAPI side, `libmachinekithal.so` again is given an `rpath` to the `lib` directory so that `ulapi-<flavor>.so` may be loaded without reading external configuration.

At this point, the realtime environment setup is complete. Taking down the environment is simple: for userland threads, `rtapi_app` is shut down; for all threads, `rtapi_msgd` is shut down, any kernel modules are unloaded, and if needed, shmdrv is unloaded.

#### Build System Changes

In order to build multiple RT thread systems in a single run, both build parameters and intermediate build objects for each flavor must be kept separate, requiring extensive changes to `src/configure.in`, `src/Makefile`, and several other files.

Most of the Autoconf configuration was refactored. A new section detects each of the supported RT thread flavors. Another new section automatically detects kernel sources, classifying them into lists based on RT capabilities.

RT thread flavor parameters must be passed from the configure script into `Makefile.inc` separately. For example, the value of `RTFLAGS` is different for Xenomai user and RTAI kernel threads, and so `XENOMAI_THREADS_RTFLAGS` and `RTAI_KERNEL_THREADS_RTFLAGS` are passed separately. During the thread-specific `make modules` run, a `THREADS` variable is set so that something like `RTFLAGS := $($(THREADS)_THREADS_RTFLAGS)` does the right thing.

The list of all detected thread flavors to be built is in `Makefile.inc` in the `BUILD_THREAD_FLAVORS` variable. For kernel thread flavors, the kernel source directories are listed by flavor in `XENOMAI_KERNEL_THREADS_KERNEL_DIRS` and `RTAI_KERNEL_THREADS_KERNEL_DIRS`.

Running `make` starts a top-level build that looks much the same for the parts of Machinekit not affected by the RT flavor. The top-level build `modules:` target, however, does not itself build any flavor-specific objects. Instead, it executes second-level `make modules` runs, one run for each configured userland RT thread flavor and one more run for each unique combination of kernel thread flavor and kernel source directory.

These second-level `make modules` runs build the RTAPI binaries and matching ULAPI module, keeping both build results and intermediate build objects separate for each flavor. The three categories of userland RTAPI, kbuild RTAPI and ULAPI objects each had special considerations to enable separate builds.

Userland RTAPI sources simply build into the RT flavor-specific subdirectory of ‘objects\`, such as \`objects/xenomai’.

Linux kbuild provides no simple way to specify a location for intermediate build objects. For kernel thread flavors, `modules:` target works around this limitation by creating a tree of hard links to the original sources under `objects/<flavor>/<kernel-version>`. Then kbuild is run with that as the top-level modules directory. This works fine most of the time, except during development when a new file is added to the original source tree, it is not automatically hard linked into the object tree.

The ULAPI sources in the `rtapi/` directory must also be built separately for each flavor. Limitations in the `Makefile` from e.g. `TOOBJS` requires source file paths not to overlap in order that object file paths also do not overlap. This was overcome by creating one link in `rtapi/` to the current directory for each RT flavor so that e.g. `rtapi/rtapi_task.c` can instead be compiled from `rtapi/posix/rtapi_task.c` with the result going into `objects/rtapi/posix/rtapi_task.o`.

Installation
------------

### Preparing Linux Logging

All Machinekit-related log messages go through rtapi\_msgd, which logs them to the syslog *LOCAL1* facility. This includes messages generated by kernel RT components; it does not include any messages which are generated by various supporting components which use *printk* (I think I caught most of these though; please report if you discover such a case).

The `make` process will check if logging is properly configured; if not, you will get a message like this:

    /etc/rsyslog.d/machinekit.conf does not exist - consider running 'sudo make log'

In this case, just run:

    $ sudo make log

This step does change the rsyslog configuration by copying rtapi/rsyslogd-machinekit.conf to /etc/rsyslog.d/machinekit.conf, and restarting rsyslog.

Once done, you can watch the logfile like so:

    $  tail -f /var/log/machinekit.log

### Packages required

Install the following packages:

    $ sudo apt-get install  libudev-dev libmodbus-dev libboost-python-dev

If you want to build the emcweb Web UI (--enable-emcweb), you also need these:

    $ sudo apt-get install  libboost-serialization-dev libboost-thread-dev

### Configuring and Building: The Basic procedure

In case you have an existing `machinekit` directory and want to add this branch, run this:

    $ cd machinekit
    $ git remote add github-mah https://github.com/mhaberler/machinekit.git
    $ git fetch github-mah
    $ git checkout -b unified-build-candidate-3  github-mah/unified-build-candidate-3

To clone a new copy:

    $ git clone --branch unified-build-candidate-3 --origin github-mah https://github.com/mhaberler/machinekit.git [<directory>]

In case you want to check out a development branch other than unified-build-candidate-3, replace the name as appropriate (for instance, unified-build-candidate-3-joints\_axes4 which contains the current status of the joints\_axes4 development branch, or ubc3-circular-blend-arc-alpha, which contains Rob Ellenberg’s new trajectory planner work).

The simplest way to compile this package is:

1.  `cd` to the `src` directory under the directory containing the package’s source code.

2.  Type `./autogen.sh` to regenerate files necessary for the following steps.

3.  Type `./configure` to configure the package for your system. If you’re using `csh` on an old version of System V, you might need to type `sh ./configure` instead to prevent `csh` from trying to execute `configure` itself. Running `configure` takes a while. While running, it prints some messages telling which features it is checking for.

4.  Type `make` to compile the package.

5.  Type `sudo make setuid` to set permissions.

6.  Type `source scripts/rip-environment` to set up the environment.

7.  Type `machinekit` to test the software.

#### The Configure script

The `configure` autoconf script attempts to guess correct values for various system-dependent variables used during compilation, and places those values in several files, such as `Makefile.inc` and `rtapi.ini`. It also creates a shell script `config.status` that can be run in the future to recreate the current configuration, a file `config.cache` that saves the results of its tests to speed up reconfiguring, and a file `config.log` containing compiler output (useful mainly for debugging `configure`).

#### Real-time Thread Support: the "Flavors"

To run a particular flavor, two conditions must be satisfied:

1.  Machinekit must have been built to support this flavor

2.  the running kernel must be compatible with the desired flavor.

The following thread flavor names are understood (`FLAVOR` environment variable):

 rtai-kernel   
the traditional RTAI threading system, compiled as .ko kernel modules. Compatible with RTAI kernels only.

 posix   
Normal Posix threads, runs on any Linux kernel. No realtime properties. This is what used to be 'sim\` or 'simulator mode\`. Runs on any Linux kernel.

 rt-preempt   
RT-hardened Posix threads running on a kernel with the RT-PREEMPT patch applied (see <https://rt.wiki.kernel.org/index.php/Main_Page>) Compatible with RT-PREEMPT kernels, but will also run on Xenomai kernels (the results of doing so have not been evaluated)

 xenomai   
Xenomai user process RT threads. Requires a Xenomai-patched Linux kernel (see www.xenomai.org). Runs on Xenomai kernels only.

 xenomai-kernel   
Xenomai kernel RT threads, also using kernel modules. Runs on Xenomai kernels only. While build support is in place, this is deprecated and not recommended for use.

Each of the RT thread flavors requires special kernel support. Xenomai and RTAI kernel packages are available from the project, and RT\_PREEMPT kernel packages are available from upstream vendors and third-party package repositories. Please install one of these RT kernels (refer to the documentation of the project on how to do that).

#### Optional Features

If multiple RT flavors are available, Machinekit will attempt to detect and build for all of them. A subset may be selected on the configure command line:

 `./configure --with-xenomai --with-posix`   
Build only Xenomai and POSIX userland threads. No other flavors will be built.

 `./configure --with-posix --with-rtai-kernel`   
Build only POSIX userland and RTAI kernel threads. If more than one set of RTAI kernel headers is found, modules will be built for all of them.

 `./configure --with-xenomai-kernel-sources=~/src/linux-3.5.7-xenomai`   
Build all detected RT thread flavors. In addition to standard locations for kernel sources, also look for Xenomai headers in a non-standard location.

 `./configure --prefix=/usr/local`   
Specify a location for system installation. By default, Machinekit will build to "run in place" out of the build directory.

 `./configure --enable-build-documentation`   
Enable generating documentation from source. Building documentation is disabled by default because of the long compilation time.

Run ‘./configure --help’ for more details on these and other available options.

#### Configure Options

`configure` recognizes the following options to control how it operates:

 `--cache-file=FILE`   
Use and save the results of the tests in FILE instead of `./config.cache`. Set FILE to `/dev/null` to disable caching, for debugging `configure`.

 `--help`   
Print a summary of the options to `configure`, and exit.

 `--quiet`
 `--silent`
 `-q`   
Do not print messages saying which checks are being made. To suppress all normal output, redirect it to `/dev/null` (any error messages will still be shown).

 `--version`   
Print the version of Autoconf used to generate the `configure` script, and exit.

Options to the realtime script
------------------------------

To start the realtime environment, do as usual:

    $ realtime start

To stop, execute

    $ realtime stop

The realtime script reads default values from etc/machinekit/rtapi.ini; most values here will never need to be changed.

The following defaults from rtapi.ini can be overridden via environment variables:

 `DEBUG=<integer>`   
set the rt and user logging level (0..5, the maximum). A lot of detail will be logged to /var/log/machinekit.log. If you suspect problems, run `DEBUG=5 realtime start`.

 `FLAVOR=<flavor name> <machinekit command>`   
Start a particular (non-default) thread flavor. FLAVOR must be one of: `rtai-kernel`, `rt-preempt`, `xenomai`, `posix`, `xenomai-kernel`.

 `HAL_SIZE=<number>`   
The default size of the HAL data shared memory segment is 262000. A larger size can be set via this variable.

 `MSGD_OPTS=<options to rtapi_msgd>`   
extra startup options can be passed to rtapi\_msgd. A useful one is `--stderr` which causes rtapi\_msgd to write all log output to stderr as well:

 `RTAPI_APP_OPTS=<options to rtapi_app>`   
extra startup options can be passed to rtapi\_app. The only meaningful option here is `--drivers` which enables I/O for the `posix` flavor. This requires the `sudo make setuid` step.

 `USE_SHMDRV=yes`   
Meaningful only for userland thread flavors. Forces the use of the common shared memory driver even for userland threads instances (normally it would default to Posix shared memory). This is relevant only in the future scenario where interworking between kernel and user threads instances is desired, so ignore for now.

 `INSTANCE=<instance number>`   
Instances are numbered 0-31. By default the instance number is 0; another instance can be referred to by the INSTANCE environment variable. See the section *Running instances side by side*.

### Startup Option Usage Examples

#### Run a *sim* (Posix threads) instance

    $ export FLAVOR=posix
    $ realtime start
    $ haldcmd -f -k

#### Capturing the complete log of a single session

    DEBUG=5 MSGD_OPTS="--stderr" realtime start >logfile 2>&1

#### Running realtime with a larger HAL segment

    HAL_SIZE=512000 realtime start

#### Running the *Posix* Flavor and enable I/O through drivers

    RTAPI_APP_OPTS="--drivers" FLAVOR=posix realtime start

### Running instances side by side:

#### Status of Multiple Instance support

The status of instance support for running several side-by-side instances of machinekit on a single host is:

1.  support in RTAPI/HAL as well as startup/shutdown is feature complete

2.  support for multiple instances in NML is currently at a *gross hack* level - the issue is the TCP port number usage. It might not make sense to fix this as NML is being replaced anyway.

3.  the machinekit script needs work - the first instance to shut down kills the other instances too.

#### Running separate HAL/RTAPI instances

 INSTANCE=2 realtime start   
starts the instance \#2 of RTAPI/HAL

 INSTANCE=3 machinekit   
starts the instance \#3 of RTAPI/HAL and Machinekit (see restrictions noted above)

### Isolating and Reporting an error

-   After building in the `src` directory as outlined above, execute as usual `. ../scripts/rip-environment`

-   Make sure logging is set up as outlined in the *Preparing Linux Logging* section above.

-   watch the file /var/log/machinekit.log, for instance with `tail -f   /var/log/machinekit.log` in a separate terminal window.

-   Verify that logging works - do a `realtime start` followed by a `realtime stop`. There should be a few lines of log entries added.

-   First, verify basic health of the build: Please run the `runtests` script and save the list of failed tests if any. This can take a long time, it’s more than 120 tests by now.

-   during the `runtests` step, log file entries should appear in /var/log/machinekit.log.

-   run the failed configuration with increased logging detail in a terminal window like so: `DEBUG=5 machinekit <yourconfig.ini>` and save the output to a file; running the configuration from the machinekit config selector will make you miss likely important output.

-   pastebin the list of failed tests, the console output, /var/log/machinekit.log and the configuration files if not using a stock configuration.

-   if the error is verified to be genuine **please add an issue to the tracker: <https://github.com/zultron/machinekit/issues?&state=open>**.

Man pages for exception handler, update\_stats
----------------------------------------------

TBD

Building the Xenomai kernel for the BeagleBone board
----------------------------------------------------

run configure like so:

    $ ./configure --with-platform=beaglebone --with-xenomai --with-posix

This will build both the realtime and *simulator* (Posix) flavor.

Building the Xenomai kernel for the Raspberry Pi
------------------------------------------------

run configure like so:

    $ ./configure --with-platform=raspberry --with-xenomai --with-posix

This will build both the realtime and *simulator* (Posix) flavor.

Current runtests failures
-------------------------

hm2-idrom fails on the Beaglebone (naturally - no PCI support; this can be ignored).

Issue Tracker
-------------

The issue tracker for the Unified Build development is here:

<https://github.com/zultron/machinekit/issues?&state=open>

Feel free to add issues so they are not lost.

Issues
------

 hal\_lib.c   
contains some undocumented new methods. They do not impact HAL functionality.

 rtapi\_msgd naming   
this name rtapi\_msgd is a bit misleading - it sets up the per-instance global data segment which is essential for Machinekit operation.

 history cleaning, and squashing out *wip* and *FIXME* commits   
DONE

Remaining Work
--------------

### Short term

These are features which can be added as the branch matures:

 RTAPI shutdown exception   
The exception handler feature currently has no way to signal an impending RTAPI shutdown, which would be very valuable to for instance cause an estop first thing. Again, this would be easier to do if we had a proper RT demon.

 RTAPI status reporting ala /proc/rtapi   
The is currently no equivalent for userland threads flavors; it should be straighforward to add along the lines of thread status reporting.

 logging   
The rtapi\_set\_logtag("string") was intended to mark a log message with the origin (user process, RT, kernel etc). It is a bit halfbaked idea; a better solution would be to extend the first argument (message level) to rtapi\_print\_msg() to support an origin enumeration type (note message level only needs 3 bits of the 32bit integer parameter, so there are lots of bit left to tag the origin and the change is backwards-compatible). This would make writing log messages much more uniform and less verbose, while supporting automatic filtering by origin for multiple publish channels in a future version.

### Longer Term plans

#### Unified command API to the RT environment

I \[MH\] think once we have the new middleware infrastructure in place it makes sense to fold the kernel threads startup/shutdown/module loading functions into a common RTAPI demon, which would handle all RT commands alike regardless of kernel-versus-userland threads. This would make it much easier on the using side to script commands for startup, shutdown and loading.

That really makes sense only once we have the new middleware stack (zmq/protobuf) in place - the RT environment should be addressable over this vehicle like any other entity, not with arcane shell scripts run from here and there. It makes no sense anymore to do that in NML.

Currently rtapi\_msgd is a standalone process and it will evolve to support a publish functionality; arbitrary clients may subscribe to one of the channels to receive updates. This might well be folded into the common RTAPI demon, taking out some complexity of startup and shutdown.

#### Unified thread creation API

The current method of creating an RT thread for kernel thread flavors stands improvement. A common RTAPI demon could do this for userland and kernel thread flavors just alike, using a simple procfs interface for thread creation/deletion like shown here: <http://tinyurl.com/mowmmyl>

#### Use the Xenomai posix threads skin

The Xenomai code currently uses the *native skin*. Using the *Posix skin* instead would allow merging all of Xenomai, Posix and RT-PREEMPT into a single code base, easing maintenence a bit. Not very important.

Miscellaneous Notes
-------------------

### Thread status display in halcmd

After RT threads are started. the `show thread <threadname>` command will display details like so:

    $ halcmd -f -k
    halcmd: loadrt threads
    halcmd: show thread thread1
    Realtime Threads (flavor: xenomai) :
         Period  FP     Name               (     Time, Max-Time )
        1000000  YES               thread1 (        0,        0 )

    Lowlevel thread statistics for 'thread1':

        updates=455 api_err=0       other_err=0
        wait_errors=454     overruns=2598   modeswitches=0  contextswitches=734
        pagefaults=0        exectime=158813uS       status=0x300180

The values are as returned by the underlying system calls and might need code and manual reading to understand exactly. Some of the values (in particular execution times) seem not to make much sense.

### Displaying Thread Status on RT-PREEMPT

The RT threads are named like in HAL (but with the instance number suffixed). Example for ps output of instance 0 on RT-PREEMPT:

    mah@wheezy:~$ ps -Leo pid,tid,class,rtprio,stat,comm,wchan |grep `pidof rtapi:0`
     4880  4880 TS       - SLsl rtapi:0         ?
     4880  4883 FF      98 RLsl fast:0          ?
     4880  4884 FF      97 SLsl slow:0          ?

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

