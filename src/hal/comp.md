Comp HAL Component Generator
============================

<span id="cha:comp-hal-component-generator"></span>

Introduction
------------

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

Installing
----------

If you’re working with an installed version of Machinekit you will need to install the development packages.

One method is to say following line in a terminal.

Installing Dev

    sudo apt-get install machinekit-dev

Another method is to use the Synaptic Package Manager from the main Debian menu to install machinekit-dev.

Definitions
-----------

-   *component* - A component is a single real-time module, which is loaded with *halcmd loadrt*. One *.comp* file specifies one component.

-   *instance* - A component can have zero or more instances. Each instance of a component is created equal (they all have the same pins, parameters, functions, and data) but behave independently when their pins, parameters, and data have different values.

-   *singleton* - It is possible for a component to be a "singleton", in which case exactly one instance is created. It seldom makes sense to write a *singleton* component, unless there can literally only be a single object of that kind in the system (for instance, a component whose purpose is to provide a pin with the current UNIX time, or a hardware driver for the internal PC speaker)

Instance creation
-----------------

For a singleton, the one instance is created when the component is loaded.

For a non-singleton, the *count* module parameter determines how many numbered instances are created. If not specified, the *name* module parameter determines how many named instances are created. Otherwise, a single numbered instance is created.

Implicit Parameters
-------------------

Functions are implicitly passed the *period* parameter which is the time in nanoseconds of the last period to execute the comp. Functions which use floating-point can also refer to *fperiod* which is the floating-point time in seconds, or (period\*1e-9). This can be useful in comps that need the timing information.

Syntax
------

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

### HAL functions

-   *fp* - Indicates that the function performs floating-point calculations.

-   *nofp* - Indicates that it only performs integer calculations. If neither is specified, *fp* is assumed. Neither comp nor gcc can detect the use of floating-point calculations in functions that are tagged *nofp*, but use of such operations results in undefined behavior.

### Options

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

### License and Authorship

-   *LICENSE* - Specify the license of the module for the documentation and for the MODULE\_LICENSE() module declaration. For example, to specify that the module’s license is GPL v2 or later,

        license "GPL"; // indicates GPL v2 or later

    For additional information on the meaning of MODULE\_LICENSE() and additional license identifiers, see *&lt;linux/module.h&gt;*. or the manual page *rtapi\_module\_param(3)*

    This declaration is required.

-   *AUTHOR* - Specify the author of the module for the documentation.

### Per-instance data storage

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

### Comments

C++-style one-line comments (//… ) and

C-style multi-line comments (/\* … \*/) are both supported in the declaration section.

Restrictions
------------

Though HAL permits a pin, a parameter, and a function to have the same name, comp does not.

Variable and function names that can not be used or are likely to cause problems include:

-   Anything beginning with **\_comp**.

-   *comp\_id*

-   *fperiod*

-   *rtapi\_app\_main*

-   *rtapi\_app\_exit*

-   *extra\_setup*

-   *extra\_cleanup*

Convenience Macros
------------------

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

Components with one function
----------------------------

If a component has only one function and the string "FUNCTION" does not appear anywhere after *;;*, then the portion after *;;* is all taken to be the body of the component’s single function. See the [Simple Comp](#code:simple-comp-example) for and example of this.

Component Personality
---------------------

If a component has any pins or parameters with an "if condition" or "\[maxsize : condsize\]", it is called a component with *personality*. The *personality* of each instance is specified when the module is loaded. *Personality* can be used to create pins only when needed. For instance, personality is used in the *logic* component, to allow for a variable number of input pins to each logic gate and to allow for a selection of any of the basic boolean logic functions *and*, *or*, and *xor*.

Compiling
---------

Place the *.comp* file in the source directory *machinekit/src/hal/components* and re-run *make*. *Comp* files are automatically detected by the build system.

If a *.comp* file is a driver for hardware, it may be placed in *machinekit/src/hal/components* and will be built unless Machinekit is configured as a userspace simulator.

Compiling realtime components outside the source tree
-----------------------------------------------------

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

Compiling userspace components outside the source tree
------------------------------------------------------

*comp* can process, compile, install, and document userspace components:

    comp usrexample.comp
    comp --compile usrexample.comp
    comp --install usrexample.comp
    comp --document usrexample.comp

This only works for *.comp* files, not for *.c* files.

Examples
--------

### constant

Note that the declaration "function \_" creates functions named "constant.0", etc.

    component constant;
    pin out float out;
    param r float value = 1.0;
    function _;
    license "GPL"; // indicates GPL v2 or later
    ;;
    FUNCTION(_) { out = value; }

### sincos

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

### out8

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

### hal\_loop

    component hal_loop;
    pin out float example;

This fragment of a component illustrates the use of the *hal\_* prefix in a component name. *loop* is the name of a standard Linux kernel module, so a *loop* component might not successfully load if the Linux *loop* module was also present on the system.

When loaded, *halcmd show comp* will show a component called *hal\_loop*. However, the pin shown by *halcmd show pin* will be *loop.0.example*, not *hal-loop.0.example*.

### arraydemo

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

### rand

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

### logic

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

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

