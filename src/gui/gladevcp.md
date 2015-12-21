Glade Virtual Control Panel
===========================

<span id="cha:glade-vcp"></span>

What is GladeVCP?
-----------------

GladeVCP is an Machinekit component which adds the ability to add a new user interface panel to Machinekit user interfaces like:

    -Axis
    -Touchy
    -Gscreen
    -Gmoccapy

Unlike PyVCP, GladeVCP is not limited to displaying and setting HAL pins, as arbitrary actions can be executed in Python code - in fact, a complete Machinekit user interface could be built with GladeVCP and Python.

GladeVCP uses the [Glade](http://glade.gnome.org/) WYSIWYG user interface editor, which makes it easy to create visually pleasing panels. It relies on the [PyGTK](http://www.pygtk.org/) bindings to the rich [GTK+](http://www.gtk.org/) widget set, and in fact all of these may be used in a GladeVCP application - not just the specialized widgets for interacting with HAL and Machinekit, which are documented here.

### PyVCP versus GladeVCP at a glance

Both support the creation of panels with *HAL widgets* - user interface elements like LED’s, buttons, sliders etc whose values are linked to a HAL pin, which in turn interfaces to the rest of Machinekit.

**PyVCP:**

-   widget set: uses TkInter widgets

-   user interface creation: "edit XML file / run result / evaluate looks" cycle

-   no support for embedding user-defined event handling

-   no Machinekit interaction beyond HAL pin I/O supported

**GladeVCP:**

-   widget set: relies on the [GTK+](http://www.gtk.org/) widget set.

-   user interface creation: uses the [Glade](http://glade.gnome.org/) WYSIWYG user interface editor

-   any HAL pin change may be directed to call back into a user-defined Python event handler

-   any GTK signal (key/button press, window, I/O, timer, network events) may be associated with user-defined handlers in Python

-   direct Machinekit interaction: arbitrary command execution, like initiating MDI commands to call a G-code subroutine, plus support for status change operations through Action Widgets

-   several independent GladeVCP panels may be run in different tabs

-   separation of user interface appearance and functionality: change appearance without touching any code

A Quick Tour with the Example Panel
-----------------------------------

GladeVCP panel windows may be run in three different setups:

-   always visible integrated into Axis at the right side, exactly like PyVCP panels

-   as a tab in Axis,Touchy, Gscreen, or Gmoccapy; in Axis this would create a third tab besides the Preview and DRO tabs which must be raised explicitly

-   as a standalone toplevel window, which can be iconifyed/deiconified independent of the main window.

Installed Machinekit

If you’re using an installed version of Machinekit the examples shown below are in the [configuration picker](#cha:starting-machinekit) in the *Sample Configurations &gt; apps &gt; gladevcp* branch.

Git Checkout

The following instructions only apply if you’re using a git checkout. Open a terminal and change to the directory created by git then issue the commands as shown.

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
<td align="left">For the following commands to work on your git checkout you must first run <em>make</em> then run <em>sudo make setuid</em> then run <em>. ./scripts/rip-environment</em>. More information about a git checkout is on the machinekit wiki page.</td>
</tr>
</tbody>
</table>

Run the sample GladeVCP panel integrated into Axis like PyVCP as follows:

    $ cd configs/sim/axis/gladevcp
    $ machinekit gladevcp_panel.ini

![](images/example-panel-small.png)

Run the same panel, but as a tab inside Axis:

    $ cd configs/sim/axis/gladevcp
    $ machinekit gladevcp_tab.ini

![](images/example-tabbed-small.png)

To run this panel inside *Touchy*:

    $ cd configs/sim/touchy/gladevcp
    $ machinekit gladevcp_touchy.ini

![](images/touchy-tab-33.png)

Functionally these setups are identical - they only differ in screen real estate requirements and visibility. Since it is possible to run several GladeVCP components in parallel (with different HAL component names), mixed setups are possible as well - for instance a panel on the right hand side, and one or more tabs for less-frequently used parts of the interface.

### Exploring the example panel

While running configs/sim/axis/gladevcp\_panel.ini or configs/sim/axis/gladevcp\_tab.ini, explore *Show HAL Configuration* - you will find the *gladevcp* HAL component and may observe their pin values while interacting with the widgets in the panel. The HAL setup can be found in *configs/axis/gladevcp/manual-example.hal*.

The example panel has two frames at the bottom. The panel is configured so that resetting ESTOP activates the Settings frame and turning the machine on enables the Commands frame at the bottom. The HAL widgets in the Settings frame are linked to LEDs and labels in the *Status* frame, and to the current and prepared tool number - play with them to see the effect. Executing the *T&lt;toolnumber&gt;* and *M6* commands in the MDI window will change the current and prepared tool number fields.

The buttons in the *Commands* frame are *MDI Action widgets* - pressing them will execute an MDI command in the interpreter. The third button *Execute Oword subroutine* is an advanced example - it takes several HAL pin values from the *Settings* frame, and passes them as parameters to the Oword subroutine. The actual parameters received by the routine are displayed by *(DEBUG, )* commands - see *../../nc\_files/oword.ngc* for the subroutine body.

To see how the panel is integrated into Axis, see the *\[DISPLAY\]GLADEVCP* statement in configs/sim/axis/gladevcp/gladevcp\_panel.ini, the *\[DISPLAY\]EMBED\** statement in configs/sim/axis/gladevcp/gladevcp\_tab.ini and *\[HAL\]POSTGUI\_HALFILE* statements in both configs/sim/axis/gladevcp/gladevcp\_tab.ini and configs/sim/axis/gladevcp/gladevcp\_panel.ini

### Exploring the User Interface description

The user interface is created with the glade UI editor - to explore it, you need to have [glade installed](#gladevcp:Prerequisites). To edit the user interface, run the command

    $ glade configs/axis/gladevcp/manual-example.ui

(The required glade program may be named glade-gtk2 on more recent systems.)

The center window shows the appearance of the UI. All user interface objects and support objects are found in the right top window, where you can select a specific widget (or by clicking on it in the center window). The properties of the selected widget are displayed, and can be changed, in the right bottom window.

To see how MDI commands are passed from the MDI Action widgets, explore the widgets listed under *Actions* in the top right window, and in the right bottom window, under the *General* tab, the *MDI command* property.

### Exploring the Python callback

See how a Python callback is integrated into the example:

-   in glade, see the `hits` label widget (a plain GTK+ widget)

-   in the `button1` widget, look at the *Signals* tab, and find the signal *pressed* associated with the handler *on\_button\_press*

-   in hitcounter.py, see the method *on\_button\_press* and see how it sets the label property in the *hits* object

The is just touching upon the concept - the callback mechanism will be handled in more detail in the [GladeVCP Programming](#gladevcp:GladeVCP_Programming) section.

Creating and Integrating a Glade user interface
-----------------------------------------------

### Prerequisite: Glade installation

To view or modify Glade UI files, you need glade installed - it is not needed just to run a GladeVCP panel. If the glade command is missing, install it with the command:

    $ sudo apt-get install glade

Verify the version number to be greater than 3.6.7:

    $ glade --version
    glade3 3.6.7

(On recent systems, the required glade package is glade-gtk2)

### Running Glade to create a new user interface

This section just outlines the initial Machinekit-specific steps. For more information and a tutorial on glade, see <http://glade.gnome.org>. Some glade tips & tricks may also be found on [youtube](http://www.youtube.com).

Either modify an existing UI component by running `glade <file>.ui` or start a new one by just running the `glade` command from the shell.

-   If Machinekit was not installed from a package, the Machinekit shell environment needs to be set up with `. <machinekitdir>/scripts/rip-environment`, otherwise glade won’t find the Machinekit-specific widgets.

-   When asked for unsaved Preferences, just accept the defaults and hit *Close*.

-   From *Toplevel* (left pane), pick *Window* (first icon) as top level window, which by default will be named *window1*. Do not change this name - GladeVCP relies on it.

-   In the left tab, scroll down and expand *HAL Python* and *EMC Actions*.

-   add a container like a HAL\_Box or a HAL\_Table from *HAL Python* to the frame

-   pick and place some elements like LED, button, etc. within a container

This will look like so:

![](images/glade-manual-small.png)

Glade tends to write a lot of messages to the shell window, which mostly can be ignored. Select *File→Save as*, give it a name like *myui.ui* and make sure it’s saved as *GtkBuilder* file (radio button left bottom corner in Save dialog). GladeVCP will also process the older *libglade* format correctly but there is no point in using it. The convention for GtkBuilder file extension is *.ui*.

### Testing a panel

You’re now ready to give it a try (while Machinekit, e.g. Axis is running) it with:

    gladevcp myui.ui

GladeVCP creates a HAL component named like the basename of the UI file - *myui* in this case - unless overriden by the `-c <component name>` option. If running Axis, just try *Show HAL configuration* and inspect its pins.

You might wonder why widgets contained a *HAL\_Hbox* or *HAL\_Table* appear greyed out (inactive). HAL containers have an associated HAL pin which is off by default, which causes all contained widgets to render inactive. A common use case would be to associate these container HAL pins with `halui.machine.is-on` or one of the `halui.mode.` signals, to assure some widgets appear active only in a certain state.

To just activate a container, execute the HAL command `setp gladevcp.<container-name> 1`.

### Preparing the HAL command file

The suggested way of linking HAL pins in a GladeVCP panel is to collect them in a separate file with extension `.hal`. This file is passed via the `POSTGUI_HALFILE=` option in the `HAL` section of your ini file.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Caution
</div></td>
<td align="left">Do not add the GladeVCP HAL command file to the Axis <code>[HAL]HALFILE=</code> ini section, this will not have the desired effect - see the following sections.</td>
</tr>
</tbody>
</table>

### Integrating into Axis like PyVCP

Place the GladeVCP panel in the righthand side panel by specifying the following in the ini file:

    [DISPLAY]
    # add GladeVCP panel where PyVCP used to live:
    GLADEVCP= -u ./hitcounter.py ./manual-example.ui

    [HAL]
    # HAL commands for GladeVCP components in a tab must be executed via POSTGUI_HALFILE
    POSTGUI_HALFILE =  ./manual-example.hal

    [RS274NGC]
    # gladevcp Demo specific Oword subs live here
    SUBROUTINE_PATH = ../../nc_files/gladevcp_lib

The HAL component name of a GladeVCP application started with the GLADEVCP option is fixed: `gladevcp`. The command line actually run by Axis in the above configuration is as follows:

    halcmd loadusr -Wn gladevcp gladevcp -c gladevcp -x {XID} <arguments to GLADEVCP>

This means you may add arbitrary gladevcp options here, as long as they dont collide with the above command line options.

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
<td align="left">The file specifiers like ./hitcounter.py, ./manual-example.ui, etc. indicate that the files are located in the same directory as the ini file. You might have to copy them to you directory (alternatively, specify a correct absolute or relative path to the file(s))</td>
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
<td align="left">The <code>[RS274NGC]SUBROUTINE_PATH=</code> option is only set so the example panel will find the Oword subroutine (oword.ngc) for the MDI Command widget. It might not be needed in your setup. The relative path specifier ../../nc_files/gladevcp_lib is constructed to work with directories copied by the configuration picker and when using a run-in-place setup.</td>
</tr>
</tbody>
</table>

### Integrating into Axis as a tab next to DRO and Preview

To do so, edit your .ini file and add to the DISPLAY and HAL sections of ini file as follows:

    [DISPLAY]
    # add GladeVCP panel as a tab next to Preview/DRO:
    EMBED_TAB_NAME=GladeVCP demo
    EMBED_TAB_COMMAND=halcmd loadusr -Wn gladevcp gladevcp -c gladevcp -x {XID} -u ./gladevcp/hitcounter.py ./gladevcp/manual-example.ui

    [HAL]
    # HAL commands for GladeVCP components in a tab must be executed via POSTGUI_HALFILE
    POSTGUI_HALFILE =  ./gladevcp/manual-example.hal

    [RS274NGC]
    # gladevcp Demo specific Oword subs live here
    SUBROUTINE_PATH = ../../nc_files/gladevcp_lib

Note the *halcmd loadusr* way of starting the tab command - this assures that *POSTGUI\_HALFILE* will only be run after the HAL component is ready. In rare cases you might run a a command here which uses a tab but does not have an associated HAL component. Such a command can be started without *halcmd loadusr*, and this signifies to Axis that it does not have to wait for a HAL component since there is none.

When changing the component name in the above example, note that the names used in `-Wn <component>` and `-c <component>` must be identical.

Try it out by running Axis - there should be a new tab called *GladeVCP demo* near the DRO tab. Select that tab, you should see the example panel nicely fit within Axis.

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
<td align="left">Make sure the UI file is the last option passed to GladeVCP in both the <code>GLADEVCP=</code> and <code>EMBED_TAB_COMMAND=</code> statements.</td>
</tr>
</tbody>
</table>

### Integrating into Touchy

To do add a GladeVCP tab to *Touchy*, edit your .ini file as follows:

    [DISPLAY]
    # add GladeVCP panel as a tab
    EMBED_TAB_NAME=GladeVCP demo
    EMBED_TAB_COMMAND=gladevcp -c gladevcp -x {XID} -u ./hitcounter.py -H ./gladevcp-touchy.hal  ./manual-example.ui

    [RS274NGC]
    # gladevcp Demo specific Oword subs live here
    SUBROUTINE_PATH = ../../nc_files/gladevcp_lib

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
<td align="left">The file specifiers like ./hitcounter.py, ./manual-example.ui, etc. indicate that the files are located in the same directory as the ini file. You might have to copy them to you directory (alternatively, specify a correct absolute or relative path to the file(s))</td>
</tr>
</tbody>
</table>

Note the following differences to the Axis tab setup:

-   The HAL command file is slightly modified since *Touchy* does not use the *halui* components so its signals are not available and some shortcuts have been taken.

-   there is no *POSTGUI\_HALFILE=* ini option, but passing the HAL command file on the *EMBED\_TAB\_COMMAND=* line is ok

-   the *halcmd loaduser -Wn …* incantation is not needed.

GladeVCP command line options
-----------------------------

See also *man gladevcp* . These are the gladevcp command line options:

Usage: gladevcp \[options\] myfile.ui

Options:

 -h, --help   
show this help message and exit

 -c NAME   
Set component name to NAME. Default is base name of UI file

 -d   
Enable debug output

 -g GEOMETRY   
Set geometry WIDTHxHEIGHT+XOFFSET+YOFFSET. Values are in pixel units, XOFFSET/YOFFSET is referenced from top left of screen. Use -g WIDTHxHEIGHT for just setting size or -g +XOFFSET+YOFFSET for just position

 -H FILE   
execute hal statements from FILE with halcmd after the component is set up and ready

 -m MAXIMUM   
force panel window to maximize. Together with the -g geometry option one can move the panel to a second monitor and force it to use all of the screen

 -t THEME   
set gtk theme. Default is system theme. Different panels can have different themes. An example theme can be found in the [EMC Wiki](http://wiki.machinekit.org/cgi-bin/wiki.pl?GTK_Themes).

 -x XID   
Re-parent GladeVCP into an existing window XID instead of creating a new top level window

 -u FILE   
Use File’s as additional user defined modules with handlers

 -U USEROPT   
pass USEROPTs to Python modules

Understanding the gladeVCP startup process
------------------------------------------

The integration steps outlined above look a bit tricky, and they are. It does therefore help to understand the startup process of Machinekit and how this relates to gladeVCP.

The normal Machinekit startup process does the following:

-   the realtime environment is started

-   all HAL components are loaded

-   the HAL components are linked together through the .hal cmd scripts

-   task, iocontrol and eventually the user interface is started

-   pre-gladeVCP the assumption was: by the time the UI starts, all of HAL is loaded, plumbed and ready to go

The introduction of gladeVCP brought the following issue:

-   gladeVCP panels need to be embedded in a master GUI window setup, e.g. Axis, or Touchy, Gscreen, or Gmoccapy (embedded window or as an embedded tab)

-   this requires the master GUI to run before the gladeVCP window can be hooked into the master GUI

-   however gladeVCP is also a HAL component, and creates HAL pins of its own.

-   as a consequence, all HAL plumbing involving gladeVCP HAL pins as source or destination must be run **after** the GUI has been set up

This is the purpose of the `POSTGUI_HALFILE`. This ini option is inspected by the GUIs. If a GUI detects this option, it runs the corresponding HAl file after any embedded gladVCP panel is set up. However, it does not check whether a gladeVCP panel is actually used, in which case the HAL cmd file is just run normally. So if you do NOT start gladeVCP through `GLADEVCP` or `EMBED_TAB` etc, but later in a separate shell window or some other mechanism, a HAL command file in `POSTGUI_HALFILE` will be executed too early. Assuming gladeVCP pins are referenced herein, this will fail with an error message indicating that the gladeVCP HAL component is not available.

So, in case you run gladeVCP from a separate shell window (i.e. not started by the GUI in an embedded fashion):

-   you cannot rely on the `POSTGUI_HALFILE` ini option causing the HAL commands being run *at the right point in time*, so comment that out in the ini file

-   explicitly pass the HAL command file which refers to gladeVCP pins to gladeVCP with the *-H &lt;halcmd file&gt;* option (see previous section).

HAL Widget reference
--------------------

GladeVcp includes a collection of Gtk widgets with attached HAL pins called HAL Widgets, intended to control, display or otherwise interact with the Machinekit HAL layer. They are intended to be used with the Glade user interface editor. With proper installation, the HAL Widgets should show up in Glade’s *HAL Python* widget group. Many HAL specific fields in the Glade *General* section have an associated mouse-over tool tip.

HAL signals come in two variants, bits and numbers. Bits are off/on signals. Numbers can be "float", "s32" or "u32". For more information on HAL data types see the [HAL manual](#sec:Hal-Data). The GladeVcp widgets can either display the value of the signal with an indicator widget, or modify the signal value with a control widget. Thus there are four classes of GladeVcp widgets that you can connect to a HAL signal. Another class of helper widgets allow you to organize and label your panel.

-   Widgets for indicating "bit" signals: [HAL\_LED](#gladevcp:HAL_LED)

-   Widgets for controlling "bit" signals: [HAL\_Button HAL\_RadioButton HAL\_CheckButton](#gladevcp:HAL_Buttons)

-   Widgets for indicating "number" signals: [HAL\_Label](#gladevcp:HAL_Label), [HAL\_ProgressBar](#gladevcp:HAL_ProgressBar), [HAL\_HBar and HAL\_VBar](#gladevcp:HAL_Bars), [HAL\_Meter](#gladevcp:HAL_Meter)

-   Widgets for controlling "number" signals: [HAL\_SpinButton](#gladevcp:HAL_SpinButton), [HAL\_HScale and HAL\_VScale](#gladevcp:HAL_Scales), [Jog Wheel](#gladevcp:JogWheel)

-   Sensitive control widgets: [State\_Sensitive\_Table HAL\_Table and HAL\_HBox](#gladevcp:HAL_Table)

-   Tool Path preview: [HAL\_Gremlin](#gladevcp:HAL_Gremlin)

-   Widgets to show axis positions: [DRO Widget](#gladevcp::DRO_widget), [Combi DRO Widget](#gladevcp::Combi_DRO)

-   Widgets for file handling: [IconView File Selection](#gladevcp::IconView)

-   Widgets for display/edit of all axes offsets: [OffsetPage](#gladevcp::Offsetpage)

-   Widgets for display/edit of all tool offsets: [Tooloffset editor](#gladevcp::tooledit)

-   Widget for Gcode display and edit: [HAL\_Sourceview](#gladevcp::HAL_sourceview)

-   widget for MDI input and history display: [MDI History](#gladevcp:MDI_History)

### Widget and HAL pin naming

Most HAL widgets have a single associated HAL pin with the same HAL name as the widget (glade: General→Name).

Exceptions to this rule currently are.

-   *HAL\_Spinbutton* and *HAL\_ComboBox*, which have two pins: a `<widgetname>-f` (float) and a `<widgetname>-s` (s32) pin

-   *HAL\_ProgressBar*, which has a `<widgetname>-value` input pin, and a `<widgetname>-scale` input pin.

### Python attributes and methods of HAL Widgets

HAL widgets are instances of GtKWidgets and hence inherit the methods, properties and signals of the applicable GtkWidget class. For instance, to figure out which GtkWidget-related methods, properties and signals a *HAL\_Button* has, lookup the description of [GtkButton](http://www.pygtk.org/docs/pygtk/class-gtkbutton.html) in the [PyGtk Reference Manual](http://www.pygtk.org/docs/pygtk).

An easy way to find out the inheritance relationship of a given HAL widget is as follows: run glade, place the widget in a window, and select it; then choose the *Signals* tab in the *Properties* window. For example, selecting a *HAL\_LED* widget, this will show that a *HAL\_LED* is derived from a *GtkWidget*, which in turn is derived from a *GtkObject*, and eventually a *GObject*.

HAL Widgets also have a few HAL-specific Python attributes:

 hal\_pin   
the underlying HAL pin Python object in case the widget has a single pin type

 hal\_pin\_s, hal\_pin\_f   
the S32 and float pins of the *HAL\_Spinbutton* and *HAL\_ComboBox* widgets - note these widgets do not have a *hal\_pin* attribute!

 hal\_pin\_scale   
the float input pin of *HAL\_ProgressBar* widget representing the maximum absolute value of input.

The are several HAL-specific methods of HAL Widgets, but the only relevant method is:

 &lt;halpin&gt;.get()   
Retrieve the value of the current HAL pin, where *&lt;halpin&gt;* is the applicable HAL pin name listed above.

### Setting pin and widget values

As a general rule, if you need to set a HAL output widget’s value from Python code, do so by calling the underlying Gtk *setter* (e.g. `set_active()`, `set_value()`) - do not try to set the associated pin’s value by `halcomp[pinname] = value` directly because the widget will not take notice of the change!.

It might be tempting to *set HAL widget input pins* programmatically. Note this defeats the purpose of an input pin in the first place - it should be linked to, and react to signals generated by other HAL components. While there is currently no write protection on writing to input pins in HAL Python, this doesn’t make sense. You might use setp pinname value in the associated halfile for testing though.

It is perfectly OK to set an output HAL pin’s value with `halcomp[pinname] = value` provided this HAL pin is not associated with a widget, that is, has been created by the `hal_glib.GPin(halcomp.newpin(<name>,<type>,<direction>)` method (see GladeVCP Programming for an example).

### The hal-pin-changed signal

Event-driven programming means that the UI tells your code when "something happens" - through a callback, like when a button was pressed. The output HAL widgets (those which display a HAL pin’s value) like LED, Bar, VBar, Meter etc, support the *hal-pin-changed* signal which may cause a callback into your Python code when - well, a HAL pin changes its value. This means there’s no more need for permanent polling of HAL pin changes in your code, the widgets do that in the background and let you know.

Here is an example how to set a `hal-pin-changed` signal for a HAL\_LED in the Glade UI editor:

![](images/hal-pin-change-66.png)

The example in `configs/apps/gladevcp/complex` shows how this is handled in Python.

### Buttons

This group of widgets are derived from various Gtk buttons and consists of HAL\_Button, HAL\_ToggleButton, HAL\_RadioButton and CheckButton widgets. All of them have a single output BIT pin named identical to the widget. Buttons have no additional properties compared to their base Gtk classes.

-   HAL\_Button: instantaneous action, does not retain state. Important signal: `pressed`

-   HAL\_ToggleButton, HAL\_CheckButton: retains on/off state. Important signal: `toggled`

-   HAL\_RadioButton: a one-of-many group. Important signal: `toggled` (per button).

-   Important common methods: `set_active()`, `get_active()`

-   Important properties: `label`, `image`

Check button: <span class="image"> ![](images/checkbutton.png) </span> Radio buttons: <span class="image"> ![](images/radiobutton.png) </span> Toggle button: <span class="image"> ![](images/button.png) </span> .

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
<td align="left"><div class="paragraph">
<p>Defining radio button groups in Glade:</p>
</div>
<div class="ulist">
<ul>
<li><p>decide on default active button</p></li>
<li><p>in the other button’s <em>General→Group</em> select the default active button’s name in the <em>Choose a Radio Button in this project</em> dialog.</p></li>
</ul>
</div>
<div class="paragraph">
<p>See <code>configs/apps/gladevcp/by-widget/</code> for a GladeVCP applications and UI file for working with radio buttons.</p>
</div></td>
</tr>
</tbody>
</table>

### Scales

HAL\_HScale and HAL\_VScale are derived from the GtkHScale and GtkVScale respectively. They have one output FLOAT pin with name equal to widget name. Scales have no additional properties.

To make a scale useful in Glade, add an *Adjustment* (General→Adjustment→New or existing adjustment) and edit the adjustment object. It defines the default/min/max/increment values. Also, set adjustment *Page size* and *Page increment* to zero to avoid warnings.

Example HAL\_HScale: <span class="image"> ![](images/hscale.png) </span> .

### SpinButton

HAL SpinButton is derived from GtkSpinButton and holds two pins:

 &lt;widgetname&gt;-f   
out FLOAT pin

 &lt;widgetname&gt;-s   
out S32 pin

To be useful, Spinbuttons need an adjustment value like scales, see above.

Example SpinButton: <span class="image"> ![](images/spinbutton.png) </span> .

### Jog Wheel

The jogwheel widget simulates a real jogwheel.
 It can be operated with the mouse. You can just use the mouse wheel, while the mouse cursor is over the JogWheel widget,
 or you push the left mouse button and move the cursor in circular direction to increase or degrease the counts.

-   Counterclockwise = reduce counts

-   Clockwise = increase counts

-   Wheel up = increase counts

-   Wheel down = reduce counts

As moving the mouse the drag and drob way may be faster than the widget can update itself, you may loose counts turning to fast. It is recommended to use the mouse wheel, and only for very rough movements the drag and drob way.

JogWheel exports it’s count value as hal pin:

 &lt;widgetname&gt;-s   
out S32 pin

It has the following properties:

 size   
Sets the size in pixel of the widget, allowed values are in the range of 100 to 500 default = 200

 cpr   
Sets the Counts per Revolution, allowed values are in the range from 25 to 100 default = 40

 show\_counts   
Set this to False, if you want to hide the counts display in the middle of the widget.

 label   
Set the content of the label witch may be shown over the counts value. The purpose is to give the user an idea about the usage of that jogwheel. If the label given is longer than 12 Characters, it will be cut to 12 Characters.

 Direct program control   

    There a couple ways to directly control the widget using Python.

    Using goobject to set the above listed properties:
        [widget name].set_property("size",int(value))
        [widget name].set_property("cpr",int(value))
        [widget name].set_property("show_counts, True)

    There are two python methods:
        [widget name].get_value()
        Will return the counts value as integer
        [widget name].set_label("string")
        Sets the label content with "string"

Example JogWheel:

![](images/JogWheel.png)

### Label

HAL\_Label is a simple widget based on GtkLabel which represents a HAL pin value in a user-defined format.

 label\_pin\_type   
The pin’s HAL type (0:S32, 1:float, 2:U32), see also the tooltip on 'General→HAL pin type '(note this is different from PyVCP which has three label widgets, one for each type).

 text\_template   
Determines the text displayed - a Python format string to convert the pin value to text. Defaults to `%s` (values are converted by the str() function) but may contain any legit as an argument to Pythons format() method.
 Example: `Distance: %.03f` will display the text and the pin value with 3 fractional digits padded with zeros for a FLOAT pin.

### Containers: HAL\_HideTable HAL\_Table State\_Sensitive\_Table and HAL\_HBox

These containers are meant to be used to sensitize (grey out) or hide their children.
 Insensitived children will not respond to input.
 HAL\_HideTable has one HAL BIT input pin which controls if it’s child widgets are hidden or not.
 If the pin is low then child widgets are visible which is the default state.
 HAL\_Table and HAL\_Hbox have one HAL BIT input pin which controls if their child widgets are sensitive or not.
 If the pin is low then child widgets are inactive which is the default state.
 State\_Sensitive\_table responds to the state to machinekit’s interpreter.
 optionally selectable to respond to *must-be-all-homed*,*must-be-on* and *must-be-idle*
 You can combine them. It will always be insensitive at Estop.
 \* HAL\_Hbox is depreceiated - use HAL\_Table.
 If current panels use it it won’t fail. You just won’t find it in the GLADE editor anymore.
 Future vesions of gladeVCP may remove this widget completely and then you will need to update the panel.

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
<td align="left">If you find some part of your GladeVCP application is <em>grayed out</em> (insensitive), see whether a HAL_Table pin is unset or unconnected.</td>
</tr>
</tbody>
</table>

### LED

The hal\_led simulates a real indicator LED.
 It has a single input BIT pin which controls it’s state: ON or OFF.
 LEDs have several properties which control their look and feel:

 on\_color   
a String defining ON color of LED. May be any valid gtk.gdk.Color name. Not working on Debian 8.04.

 off\_color   
String defining OFF color of LED. May be any valid gtk.gdk.Color name or special value `dark`. `dark` means that OFF color will be set to 0.4 value of ON color. Not working on Debian 8.04.

 pick\_color\_on, pick\_color\_off   
Colors for ON and OFF states may be represented as `#RRRRGGGGBBBB` strings. These are optional properties which have precedence over `on_color` and `off_color`.

 led\_size   
LED radius (for square - half of LED’s side)

 led\_shape   
LED Shape. Valid values are 0 for round, 1 for oval and 2 for square shapes.

 led\_blink\_rate   
if set and LED is ON then it’s blinking. Blink period is equal to "led\_blink\_rate" specified in milliseconds.

 create hal pin   
select/deselect making of HAL pin to control LED. With no HAL pin created LED can be controlled with a python function. As an input widget, LED also supports the `hal-pin-changed signal`. If you want to get a notification in your code when the LED’s HAL pin was changed, then connect this signal to a handler, for example `on_led_pin_changed` and provide the handler as follows:

    def on_led_pin_changed(self,hal_led,data=None):
        print "on_led_pin_changed() - HAL pin value:",hal_led.hal_pin.get()

This will be called at any edge of the signal and also during program start up to report the current value.

Example LEDs: <span class="image"> ![](images/leds.png) </span> .

### ProgressBar

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
<td align="left">This widget might go away. Use the HAL_HBar and HAL_VBar widgets instead.</td>
</tr>
</tbody>
</table>

The HAL\_ProgressBar is derived from gtk.ProgressBar and has two float HAL input pins:

 &lt;widgetname&gt;   
the current value to be displayed

 &lt;widgetname&gt;-scale   
the maximum absolute value of input

It has the following properties:

 scale   
value scale. set maximum absolute value of input. Same as setting the &lt;widgetname&gt;.scale pin. A float, range from -2<sup>24\\ to\\ +2</sup>24.

 green\_limit   
green zone limit lower limit

 yellow\_limit   
yellow zone limit lower limit

 red\_limit   
red zone limit lower limit

 text\_template   
Text template to display the current value of the `<widgetname>` pin. Python formatting may be used for dict `{"value":value}`

Example HAL\_ProgressBar: <span class="image"> ![](images/progressbar2.png) </span> .

### ComboBox

HAL\_ComboBox is derived from gtk.ComboBox. It enables choice of a value from a dropdown list.

It exports two HAL pins:

 &lt;widgetname&gt;-f   
the current value, type FLOAT

 &lt;widgetname&gt;-s   
the current value, type S32

It has the following property which can be set in Glade:

 column   
the column index, type S32, defaults to -1, range from -1..100 .

In default mode this widgets sets the pins to the index of the chosen list entry. So if your widget has three labels, it may only assume values 0,1 and 2.

In column mode (column &gt; -1), the value reported is chosen from the ListStore array as defined in Glade. So typically your widget definition would have two columns in the ListStore , one with text displayed in the dropdown, and an int or float value to use for that choice.

There’s an example in `configs/apps/by-widget/combobox.{py,ui}` which uses column mode to pick a float value from the ListStore.

If you’re confused like me about how to edit ComboBox ListStores and CellRenderer, see <http://www.youtube.com/watch?v=Z5_F-rW2cL8>.

### Bars

HAL Bar and VBar widgets for horizontal and vertical bars representing float values. They have one input FLOAT hal pin. Both bars have the following properties:

 invert   
Swap min and max direction. An inverted HBar grows from right to left, an inverted VBar from top to bottom.

 min, max   
Minimum and maximum value of desired range. It is not an error condition if the current value is outside this range.

 show limits   
Used to select/deselect the limits text on bar.

 zero   
Zero point of range. If it’s inside of min/max range then the bar will grow from that value and not from the left (or right) side of the widget. Useful to represent values that may be both positive or negative.

 force\_width, force\_height   
Forced width or height of widget. If not set then size will be deduced from packing or from fixed widget size and bar will fill whole area.

 text\_template   
Like in Label sets text format for min/max/current values. Can be used to turn off value display.

 value   
Sets the bar display to the value entered: used only for testing in GLADE editor. The vaue will be set from A HAL pin.

 target value   
Sets the target line to the value entered: used only for testing in GLADE editor. The value will can be set in a Python function

 target\_width   
Width of the line that marks the target value.

 bg\_color   
Background (inactive) color of bar.

 target\_color   
Color of the the target line.

 z0\_color, z1\_color, z2\_color   
Colors of different value zones. Defaults are `green`, `yellow` and `red`. For description of zones see `z*_border` properties.

 z0\_border, z1\_border   
Define up bounds of color zones. By default only one zone is enabled. If you want more then one zone set `z0_border` and `z1_border` to desired values so zone 0 will fill from 0 to first border, zone 1 will fill from first to second border and zone 2 — from last border to 1. Borders are set as fractions, values from 0 to 1.

Horizontal bar: <span class="image"> ![](images/hal_hbar.png) </span> Vertical bar: <span class="image"> ![](images/vscale.png) </span> .

### Meter

HAL Meter is a widget similar to PyVCP meter - it represents a float value and has one input FLOAT hal pin. HAL Meter has the following properties:

 min, max   
Minimum and maximum value of desired range. It is not an error condition if the current value is outside this range.

 force\_size   
Forced diameter of widget. If not set then size will be deduced from packing or from fixed widget size and meter will fill all available space with respect to aspect ratio.

 text\_template   
Like in Label sets text format for current value. Can be used to turn off value display.

 label   
Large label above center of meter.

 sublabel   
Small label below center of meter.

 bg\_color   
Background color of meter.

 z0\_color, z1\_color, z2\_color   
Colors of different value zones. Defaults are `green`, `yellow` and `red`. For description of zones see `z*_border` properties.

 z0\_border, z1\_border   
Define up bounds of color zones. By default only one zone is enabled. If you want more then one zone set `z0_border` and `z1_border` to desired values so zone 0 will fill from min to first border, zone 1 will fill from first to second border and zone 2 — from last border to max. Borders are set as values in range min-max.

Example HAL Meters: <span class="image"> ![](images/hal_meter.png) </span> . === HAL\_Graph

This widget is for plotting values over time.

### Gremlin tool path preview for .ngc files

Gremlin is a plot preview widget similar to the Axis preview window. It assumes a running Machinekit environment like Axis or Touchy. To connect to it, inspects the INI\_FILE\_NAME environment variable. Gremlin displays the current .ngc file - it does monitor for changes and reloads the ngc file if the file name in Axis/Touchy changes. If you run it in a GladeVCP application when Machinekit is not running, you might get a traceback because the Gremlin widget can’t find Machinekit status, like the current file name.

Gremlin does not export any HAL pins. It has the following properties:

 show tool speed   
This displays the tool speed. Defaults true

 show commanded   
This selects the DRO to use commanded or actual values. Defaults true

 use metric units   
This selects the DRO to use metric or imperial units. Defaults true

 show rapids   
This tells the plotter to show the rapid moves. Defaults true

 show DTG   
This selects the DRO to display the distance-to-go value. Defaults true

 show relative   
This selects the DRO to show values relative to user system or machine cordinates. Defaults true

 show live plot   
This tells the plotter to draw or not. Defaults true

 show limits   
This tells the plotter to show the machine’s limits. Defaults true

 show lathe radius   
This selects the DRO to display the X axis in radius or diameter, if in lathe mode (selectable in the INI file with LATHE = 1). Defaults false

 show extents   
This tells the plotter to show the extents. Defaults true

 show tool   
This tells the plotter to draw the tool. Defaults true

 show program   
TODO

 use joints mode   
Used in non trivialkins machines (eg robots). Defaults false

 grid size   
Sets the size of the grid. which is only visible in the X, Y and Z view. Defaults to 0

 use default mouse controls   
This disables the default mouse controls. This is most useful when using a touchscreen as the default controls do not work well. You can programically add controls using python and the handler file technique. Defaults to *True*

 view   
may be any of `x`, `y`, *y2* , `z`, *z2* , `p` (perspective) . Defaults to `z` view.

 enable\_dro   
boolean; whether to draw a DRO on the plot or not. Defaults to `True`

 mouse\_btn\_mode   
integer; mouse button handling, leads to different functions of the button 0 = default: left rotate, middle move, right zoom 1 = left zoom, middle move, right rotate 2 = left move, middle rotate, right zoom 3 = left zoom, middle rotate, right move 4 = left move, middle zoom, right rotate 5 = left rotate, middle zoom, right move

 Direct program control   
There a couple ways to directly control the widget using Python.

    Using goobject to set the above listed properties:
        [widget name].set_property('view','P')
        [widget name].set_property('metric_units',False)
        [widget name].set_property('use_default_controls',False)
        [widget name].set_property('enable_dro' False))
        [widget name].set_property('show_program', False)
        [widget name].set_property('show_limits', False)
        [widget name].set_property('show_extents_option', False)
        [widget name].set_property('show_live_plot', False)
        [widget name].set_property('show_tool', False)
        [widget name].set_property('show_lathe_radius',True)
        [widget name].set_property('show_dtg',True)
        [widget name].set_property('show_velocity',False)
        [widget name].set_property('mouse_btn_mode', 4)

    There are python methods:
        [widget name].show_offsets = True
        [widget name].grid_size =  .75
        [widget name].select_fire(event.x,event.y)
        [widget name].select_prime(event.x,event.y)
        [widget name].start_continuous_zoom(event.y)
        [widget name].set_mouse_start(0,0)
        [widget name].gremlin.zoom_in()
        [widget name].gremlin.zoom_out()
        [widget name].get_zoom_distance()
        [widget name].set_zoom_distance(dist)
        [widget name].clear_live_plotter()
        [widget name].rotate_view(x,y)
        [widget name].pan(x,y)

 Hints   
-   If you set all the plotting options false but show\_offsets true you get an offsets page instead of a graphics plot.

-   If you get the zoom distance before changing the view then reset the zoom distance, it’s much more user friendly.

-   if you select an element in the preview, the selected element will be used as rotation center point

Example: <span class="image"> ![](images/gremlin.png) </span>

### HAL\_Offset

The HAL\_Offset widget is used to display the offset of a single axis. It has the following properties:

 Joint Number   
Used to select which axis (technically which joint) is displayed. On a trivialkins machine (mill, lathe, router) axis vrs joint number are:

        0:X  1:Y  2:Z  3:A  4:B  5:C  6:U  7:V  8:W
    Text template for metric units::
        You can use python formatting to display the position with different precision.
    Text template for imperial units::
        You can use python formatting to display the position with different precision.
    Reference Type::
        0:G5x 1:tool 2:G92 3:Rotation around Z

### DRO widget

The DRO widget is used to display the current axis position. It has the following properties:

 Actual Position   
select actual (feedback) position or commanded position.

 Text template for metric units   
You can use python formatting to display the position with different precision.

 Text template for imperial units   
You can use python formatting to display the position with different precision.

 Reference Type   
Absolute (machine origin), Relative (to current user coordinate origin - G5x) or Distance-to-go (relative to current user coordinate origin)

 Joint Number   
Used to select which axis (technically which joint) is displayed. On a trivialkins machine (mill, lathe, router) axis vrs joint number are:

    0:X  1:Y  2:Z  3:A  4:B  5:C  6:U  7:V  8:W

 Display units   
Used to toggle the display units between metric and imperial.

 Hints   
-   If you want the display to be right justified, set the X align to 1.0

-   If you want different colors or size or text change the attributes in the glade editor (eg scale is a good way to change the size of the text)

-   The background of the widget is actually see through - so if you place if over an image the DRO numbers will show on top of it with no backgroud. There is a special technique to do this. See the animated function diagrams below.

-   The DRO widget is a modified gtk label widget. As such much or what can be done to a gtk label can be done to DRO widget.

 Direct program control   
There a couple ways to directly control the widget using Python.

    Using goobject to set the above listed properties:
        [widget name].set_property("display_units_mm",True)
        [widget name].set_property("actual",True)
        [widget name].set_property("mm_text_template","%f")
        [widget name].set_property("imperial_text_template","%f")
        [widget name].set_property("Joint_number",3)
        [widget name].set_property("reference_type",3)

    There are two python methods:
        [widget name].set_dro_inch()
        [widget name].set_dro_metric()

### Combi\_DRO widget

The Combi\_DRO widget is used to display the current , the relative axis position and the distance to go in one DRO.
 By clicking on the DRO the Order of the DRO will toggle around.
 In Relative Mode the actual coordinate system will be displayed.

It has the following properties:

 joint\_number   
Used to select which axis (technically which joint) is displayed.
 On a trivialkins machine (mill, lathe, router) axis vrs. joint number are:
 *0:X 1:Y 2:Z etc*

 actual   
select actual (feedback) or commanded position.

 metric\_units   
Used to toggle the display units between metric and imperial.

 auto\_units   
Units will toggle between metric and imperial according to the active gcode being G20 or G21

 diameter   
Whether to display position as diameter or radius, in diameter mode the DRO will display the joint value multiplied by 2

 mm\_text\_template   
You can use python formatting to display the position with different precision.
 default is "%10.3f"

 imperial\_text\_template   
You can use python formatting to display the position with different precision.
 default is "%9.4f"

 homed\_color   
The foreground color of the DRO numbers if the joint is homed
 default is green

 unhomed\_color   
The foreground color of the DRO numbers if the joint is not homed
 default is red

 abs\_color   
the background color of the DRO, if main DRO shows absolute coordinates
 default is blue

 rel\_color   
the background color of the DRO, if main DRO shows relative coordinates
 default is black

 dtg\_color   
the background color of the DRO, if main DRO shows distance to go
 default is yellow

 font\_size   
The font size of the big numbers, the small ones will be 2.5 times smaller, the value must be an integer in the range of 8 to 96,
 default is 25

 Direct program control   

Using goobject to set the above listed properties:

    [widget name].set_property(property,value)

There are several python methods to control the widget:

    [widget name].set_to_inch(state)
        sets the DRO to show imperial units
        state = boolean (True or False)

    [widget name].set_auto_units(state)
        if True the DRO will change units according to active gcode (G20 / G21)
        state = boolean (True or False)
        Default is True

    [widget name].set_to_diameter(state)
        if True the DRO will show the diameter not the radius, specially needed for lathes
        the DRO will display the axis value multiplied by 2
        state = boolean (True or False)
        Default is False

    [widget name].toggle_readout()
        toggles the order of the DRO in the widget

    [widget name].change_axisletter(letter)
        changes the automatically given axis letter
        very useful to change an lathe DRO from X to R or D
        letter = string

    [widget name].get_order()
        returns the order of the DRO in the widget mainly used to maintain them consistent
        the order will also be transmitted with the clicked signal
        returns a list containing the order

    [widget name].set_order(order)
        sets the order of the DRO, mainly used to maintain them consistent
        order = list object, must be one of
                ["Rel", "Abs", "DTG"]
                ["DTG", "Rel", "Abs"]
                ["Abs", "DTG", "Rel"]
        Default = ["Rel", "Abs", "DTG"]

    [widget name].get_position()
        returns the position of the DRO as a list of floats
        the order is independent of the order shown on the DRO
        and will be given as [Absolute , relative , DTG]
        Absolute = the machine coordinates, depends on the actual property
                       will give actual or commanded position
        Relative = will be the coordinates of the actual coordinate system
        DTG = the distance to go, will mostly be 0, as this function should not be used
                  while the machine is moving, because of time delays

The widget will emit the following signals:

    clicked
        This signal is emitted, when the user has clicked on the Combi_DRO widget,
        it will send the following data:
        widget = widget object = The widget object that sends the signal
        joint_number = integer = The joint number of the DRO, where '0:X  1:Y  2:Z  etc'
        order = list object = the order of the DRO in that widget
                              the order may be used to set other Combi_DRO widgets to the same order with [widget name].set_order(order)

    units_changed
        This signal is emitted, if the DRO units are changed, it will send the following data:
        widget = widget object = The widget object that sends the signal
        metric_units = boolean = True if the DRO does display metric units, False in case of imperial display

    system_changed
        This signal is emitted, if the DRO units are changed, it will send the following data:
        widget = widget object = The widget object that sends the signal
        system = string = The actual coordinate system. Will be one of
                          G54 G55 G56 G57 G58 G59 G59.1 G59.2 G59.3
                          or Rel if non has been selected at all, what will only happen in Glade with no machinekit running

There are some information you can get through commands, witch may be of ineterst for you:

    [widget name].system
        The actual system, as mentioned in the system_changed signal

    [widget name].homed
        True if the joint is homed

    [widget name].machine_units
        0 if Imperial, 1 if Metric

Example, Three Combi\_DRO in a window
 X = Relative Mode
 Y = Absolute Mode
 Z = DTG Mode

![](images/combi_dro.png)

### IconView (File selection) widget

This is touch screen friendly widget to select a file and to change directories.

The widget has the following properties:

 icon\_size   
Sets the size of the displayed icon.
 Allowed values are integers in the range from 12 to 96
 default is 48

 start\_dir   
Sets the directory to start in when the widget is shown first time,
 must be a string, containing a valid directory path,
 default is "/"

 jump\_to\_dir   
Sets the directory "jump to" directory, witch is selected by the corresponding button in the bottom button list, the 5th button counting from the left,
 must be a string, containing a valid directory path,
 default is "~"

 filetypes   
Sets the file filter for the objects to be shown
 Must be a string containing a comma separated list of extensions to be shown
 Default is "ngc,py"

 sortorder   
Sets the sorting order of the displayed icon must be an integer value from 0 to 3, where
 0 = ASCENDING (sorted according to file names)
 1 = DESCENDING (sorted according to file names)
 2 = FOLDERFIRST (show the folders first, then the files)
 3 = FILEFIRST (show the files first, then the folders),
 Default = 2 = FOLDERFIRST

 Direct program control   

Using goobject to set the above listed properties:

    [widget name].set_property(property,Value)

There are python methods to control the widget:

    [widget name].show_buttonbox(state)
        if False the bottom button box will be hidden, this is helpful in custom screens,
        with special buttons layouts to not alter the layout of the GUI, good example
        for that is gmoccapy
        state = boolean (True or False)
        Default is True

    [widget name].show_filelabel(state)
        if True the file label (between the IconView window and the bottom button box will be shown.
        Hiding this label may save place, but showing it is very useful for debugging reasons,
        state = boolean (True or False)
        Default is True

    [widget name].set_icon_size(iconsize)
        sets the icon size
        must be an integer in the range from 12 to 96
        Default = 48

    [widget name].set_directory(directory)
        Allows to set an directory to be shown
        directory = string (a valid file path)

    [widget name].set_filetypes(filetypes)
        sets the file filter to be used, only files with the given extensions will be shown
        filetypes = string containing a comma separated list of extensions
        Default = "ngc,py"

    [widget name].get_selected()
        Returns the path of the selected file, or None if an directory has been selected

    [widget name].refresh_filelist()
        Refreshes the filelist, needed if you add a file without changing the directory

If the button box has been hidden, you can reach the functions of this button through it’s clicked signals like so:

    [widget name].btn_home.emit("clicked")
    [widget name].btn_jump_to.emit("clicked")
    [widget name].btn_sel_prev.emit("clicked")
    [widget name].btn_sel_next.emit("clicked")
    [widget name].btn_get_selected.emit("clicked")
    [widget name].btn_dir_up.emit("clicked")
    [widget name].btn_exit.emit("clicked")

The widget will emit the following signals:

    selected
        This signal is emitted, when the user selects an icon, it will return a string containing a
        file path if a file has been selected, or None if an directory has been selected
    sensitive
        This signal is emitted, when the buttons change there state from sensitive to not sensitive or vice versa.
        This signal is usefull to mantain surrounding GUI synconized with the button of the widget. See gmoccapy as example.
        It will return the buttonname and the new state. Buttonname is one of "btn_home", "btn_dir_up", "btn_sel_prev",
        "btn_sel_next", "btn_jump_to" or "btn_select". State is a boolean and will be True or False.
    exit
        This signal is Emmit, when the exit button has been pressed to close the IconView
        mostly needed if the application is started as stand alone.

Example:

![](images/iconview.png)

### Calculator widget

This is a simple calculator widget, that can be used for numerical input.
 You can preset the display and retrieve the result or that preset value.
 It has the following properties:

 Is editable   
This allows the entry display to be typed into from a keyboard.

 Set Font   
This allows you to set the font of the display.

 Direct program control   
There a couple ways to directly control the widget using Python.

    Using goobject to set the above listed properties:
        [widget name].set_property("is_editable",True)
        [widget name].set_property("font","sans 25")

    There are python methods:
       [widget name].set_value(2.5)
            This presets the display and is recorded.
       [widget name].set_font("sans 25")
       [widget name].set_editable(True)
       [widget name].get_value()
            Returns the calculated value - a float.
       [widget name].set_editable(True)
       [widget name].get_preset_value()
            Returns the recorded value: a float.

### Tooleditor widget

This is a tooleditor widget for displaying and modifying a tool editor file.
 It checks the current file once a second to see if machinekit updated it.
 It has the following properties:

 Hidden Columns   
This will hide the given columns: The columns are designated (in order) as such:
 s,t,p,x,y,z,a,b,c,u,v,w,d,i,j,q,;
 You can hide any number of columns including the select and comments

 Direct program control   
There a couple ways to directly control the widget using Python.

    using goobject to set the above listed properties:
        [widget name].set_properties('hide_columns','uvwijq')
            This would hide the uvwij and q columns and show all others.

    There are python methods:
        [widget name].set_visible("ijq",False)
            Would hide ij and Q columns and leave the rest as they were.
        [widget name].set_filename(path_to_file)
            Sets and loads the tool file.
        [widget name].reload(None)
            Reloads the current toolfile

![](images/gtk-tooledit.png)

### Offsetpage

The Offsetpage widget is used to display/edit the offsets of all the axes.
 It has convience buttons for zeroing G92 and Rotation-Around-Z offsets.
 It will only allow you to select the edit mode when the machine is on and idle.
 You can directly edit the offsets in the table at this time. Unselect the edit
 button to allow the OffsetPage to reflect changes.

It has the following properties:

 Hidden Columns   
A no-space list of columns to hide: The columns are designated (in order) as such:
 xyzabcuvwt
 You can hide any of the columns.

 Hidden Rows   
A no-space list of rows to hide: the rows are designated (in order) as such
 0123456789abc
 You can hide any of the rows.

 Pango Font   
Sets text font type and size

 HighLight color   
when editing this is the high light color

 Active color   
when OffsetPage detects an active user coordinate system it will use this
 color for the text

 Text template for metric units   
You can use python formatting to display the position with different precision.

 Text template for imperial units   
You can use python formatting to display the position with different precision.

 Direct program control   
There a couple ways to directly control the widget using Python.

    Using goobject to set the above listed properties:
    [widget name].set_property("highlight_color",gtk.gdk.Color('blue'))
    [widget name].set_property("foreground_color",gtk.gdk.Color('black'))
    [widget name].set_property("hide_columns","xyzabcuvwt")
    [widget name].set_property("hide_rows","123456789abc")
    [widget name].set_property("font","sans 25")

    There are python methods to control the widget:
    [widget name].set_filename("../../../configs/sim/gscreen/gscreen_custom/sim.var")
    [widget name].set_col_visible("Yabuvw",False)
    [widget name].set_row_visible("456789abc",False)
    [widget name].set_to_mm()
    [widget name].set_to_inch()
    [widget name].hide_button_box(True)
    [widget name].set_font("sans 20")
    [widget name].set_highlight_color("violet")
    [widget name].set_foreground_color("yellow")
    [widget name].mark_active("G55")
        Allows you to directly set a row to highlight.
        (eg in case you wish to use your own navigation controls. See Gmoccapy
    [widget name].selection_mask = ("Tool","Rot","G5x")
        These rows are NOT selectable in edit mode.
    [widget name].set_names([['G54','Default'],["G55","Vice1"],['Rot','Rotational']])
        This allows you to set the text of the 'T' column of each/any row.
        This is a list of a list of offset-name/user-name pairs.
        The default text is the same as the offset name.
    [widget name].get_names()
        This returns a list of a list of row-keyword/user-name pairs.
        The user name column is editable, so saving this list is user friendly.
        see set_names above.

![](images/offsetpage.png)

### HAL\_sourceview widget

This is for displaying and simple editing of Gcode.
 It looks for .ngc highlight specs in ~/share/gtksourceview-2.0/language-specs/ The current running line will be highlighted.
 With external python glue code:
 \*It can search for text, undo and redo changes.
 \*It can be used for program line selection.

 Direct program control   
There are python methods to control the widget:

    [widget name].redo()
        redo one level of changes.
    [widget name].undo()
        undo one level of changes
    [widget name].text_search(direction=True,mixed_case=True,text='G92')
        Searches forward (direction = True) or back, +
        Searches with mixed case (mixed_case = True) or exact match
    [widget name].set_line_number(linenumber)
        Sets the line to high light. Uses the sourceview line numbers.
    [widget name].get_line_number()
        returns the currenly high lighted line.
    [widget name].line_up()
        Moves the High lighted line up one line
    [widget name].line_down()
        Moves the High lighted line down one line
    [widget name].load_file('filename')
        loads a file. Using None (not a filename string) will reload the same program.
    [widget name].get_filename()

![](images/hal_sourceview.png)

### MDI history

This is for displaying and entering MDI codes.
 It will automatically grey out when MDI is not available.
 Eg during Estop and program running.

### Animated function diagrams: HAL widgets in a bitmap

For some applications it might be desirable to have background image - like a functional diagram - and position widgets at appropriate places in that diagram. A good combination is setting a bitmap background image, like from a .png file, making the gladevcp window fixed-size, and use the glade Fixed widget to position widgets on this image.

The code for the below example can be found in `configs/apps/gladevcp/animated-backdrop`:

<span class="image"> ![](images/small-screenshot.png) </span>

Action Widgets reference
------------------------

GladeVcp includes a collection of "canned actions" called EMC Action Widgets for the Glade user interface editor. Other than HAL widgets, which interact with HAL pins, EMC Actions interact with Machinekit and the G-code interpreter.

EMC Action Widgets are derived from the Gtk.Action widget. The Action widget in a nutshell:

-   it is an object available in Glade

-   it has no visual appearance by itself

-   it’s purpose: associate a visible, sensitive UI component like menu, toolbutton, button with a command. See these widget’s *General→Related Action* property.

-   the "canned action" will be executed when the associated UI component is triggered (button press, menu click..)

-   it provides an easy way to execute commands without resorting to Python programming.

The appearance of EMC Actions in Glade is roughly as follows:

![](images/emc-actions.png)

Tooltip hovers provide a description.

### EMC Action widgets

EMC Action widgets are one-shot type widgets. They implement a single action and are for use in simple buttons, menu entries or radio/check groups.

### EMC ToggleAction widgets

These are bi-modal widgets. They implement two actions or use a second (usually pressed) state to indicate that currently an action is running. Toggle actions are aimed for use in ToggleButtons, ToggleToolButtons or toggling menu items. A simplex example is the ESTOP toggle button.

Currently the following widgets are available:

-   The ESTOP toggle sends ESTOP or ESTOP\_RESET commands to Machinekit depending on it’s state.

-   The ON/OFF toggle sends STATE\_ON and STATE\_OFF commands.

-   Pause/Resume sends AUTO\_PAUSE or AUTO\_RESUME commands.

The following toggle actions have only one associated command and use the *pressed* state to indicate that the requested operation is running:

-   The Run toggle sends an AUTO\_RUN command and waits in the pressed state until the interpreter is idle again.

-   The Stop toggle is inactive until the interpreter enters the active state (is running G-code) and then allows user to send AUTO\_ABORT command.

-   The MDI toggle sends given MDI command and waits for its completion in *pressed* inactive state.

### The Action\_MDI Toggle and Action\_MDI widgets

These widgets provide a means to execute arbitrary MDI commands. The Action\_MDI widget does not wait for command completion as the Action\_MDI Toggle does, which remains disabled until command complete.

### A simple example: Execute MDI command on button press

`configs/apps/gladevcp/mdi-command-example/whoareyou.ui` is a Glade UI file which conveys the basics:

Open it in Glade and study how it’s done. Start Axis, and then start this from a terminal window with `gladevcp whoareyou.ui`. See the `hal_action_mdi1` Action and it’s `MDI command` property - this just executes `(MSG, "Hi, I’m an EMC_Action_MDI")` so there should be a message popup in Axis like so:

![](images/whoareyou.png)

You’ll notice that the button associated with the Action\_MDI action is grayed out if the machine is off, in E-Stop or the interpreter is running. It will automatically become active when the machine is turned on and out of E-Stop, and the program is idle.

### Parameter passing with Action\_MDI and ToggleAction\_MDI widgets

Optionally, *MDI command* strings may have parameters substituted before they are passed to the interpreter. Parameters currently may be names of HAL pins in the GladeVCP component. This is how it works:

-   assume you have a *HAL SpinBox* named `speed`, and you want to pass it’s current value as a parameter in an MDI command.

-   The HAL SpinBox will have a float-type HAL pin named speed-f (see HalWidgets description).

-   To substitute this value in the MDI command, insert the HAL pin name enclosed like so: `${pin-name}`

-   for the above HAL SpinBox, we could use `(MSG, "The speed is:    ${speed-f}")` just to show what’s happening.

The example UI file is `configs/apps/gladevcp/mdi-command-example/speed.ui`. Here’s what you get when running it:

![](images/speed.png)

### An advanced example: Feeding parameters to an O-word subroutine

It’s perfectly OK to call an O-word subroutine in an MDI command, and pass HAL pin values as actual parameters. An example UI file is in `configs/apps/gladevcp/mdi-command-example/owordsub.ui`.

Place `nc_files/gladevcp_lib/oword.ngc` so Axis can find it, and run `gladevcp owordsub.ui` from a terminal window. This looks like so:

![](images/oword.png)

### Preparing for an MDI Action, and cleaning up afterwards

The Machinekit G-Code interpreter has a single global set of variables, like feed, spindle speed, relative/absolute mode and others. If you use G code commands or O-word subs, some of these variables might get changed by the command or subroutine - for example, a probing subroutine will very likely set the feed value quite low. With no further precautions, your previous feed setting will be overwritten by the probing subroutine’s value.

To deal with this surprising and undesirable side effect of a given O-word subroutine or G-code statement executed with an Machinekit ToggleAction\_MDI, you might associate pre-MDI and post-MDI handlers with a given Machinekit ToggleAction\_MDI. These handlers are optional and provide a way to save any state before executing the MDI Action, and to restore it to previous values afterwards. The signal names are `mdi-command-start` and `mdi-command-stop`; the handler names can be set in Glade like any other handler.

Here’s an example how a feed value might be saved and restored by such handlers (note that Machinekit command and status channels are available as `self.machinekit` and `self.stat` through the EMC\_ActionBase class:

        def on_mdi_command_start(self, action, userdata=None):
            action.stat.poll()
            self.start_feed = action.stat.settings[1]

        def on_mdi_command_stop(self, action, userdata=None):
            action.machinekit.mdi('F%.1f' % (self.start_feed))
            while action.machinekit.wait_complete() == -1:
                pass

Only the Action\_MDI Toggle widget supports these signals.

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
<td align="left">In a later release of Machinekit, the new M-codes M70-M72 are available which make it saving state before a subroutine call, and restoring state on return much easier.</td>
</tr>
</tbody>
</table>

### Using the Machinekit Stat object to deal with status changes

Many actions depend on Machinekit status - is it in manual, MDI or auto mode? is a program running, paused or idle? You cannot start an MDI command while a G-code program is running, so this needs to be taken care of. Many Machinekit actions take care of this themselves, and related buttons and menu entries are deactivated when the operation is currently impossible.

When using Python event handlers - which are at a lower level than Actions - one needs to take care of dealing with status dependencies oneself. For this purpose, there’s the Machinekit Stat widget: to associate Machinekit status changes with event handlers.

Machinekit Stat has no visible component - you just add it to your UI with Glade. Once added, you can associate handlers with its following signals:

-   state-related: emitted when E-Stop condition occurs, is reset, machine is turned on, or is turned off

    -   `state-estop`

    -   `state-estop-reset`

    -   `state-on`,

    -   `state-off`

-   mode-related: emitted when Machinekit enters that particular mode

    -   `mode-manual`

    -   `mode-mdi`

    -   `mode-auto`

-   interpreter-related: emitted when the G-code interpreter changes into that mode

    -   `interp-run`

    -   `interp-idle`

    -   `interp-paused`

    -   `interp-reading`

    -   `interp-waiting`

    -   `file-loaded`

    -   `line-changed`

-   homing-related: emitted when machinekit is homed or not

    -   `all-homed`

    -   `not-all-homed`

GladeVCP Programming
--------------------

### User Defined Actions

Most widget sets, and their associated user interface editors, support the concept of callbacks - functions in user-written code which are executed when *something happens* in the UI - events like mouse clicks, characters typed, mouse movement, timer events, window hiding and exposure and so forth.

HAL output widgets typically map input-type events like a button press to a value change of the associated HAL pin by means of such a - predefined - callback. Within PyVCP, this is really the only type of event handling supported - doing something more complex, like executing MDI commands to call a G-code subroutine, is not supported.

Within GladeVCP, HAL pin changes are just one type of the general class of events (called signals) in GTK+. Most widgets may originate such signals, and the Glade editor supports associating such a signal with a Python method or function name.

If you decide to use user-defined actions, your job is to write a Python module whose class methods - or in the simple case, just functions - can be referred to in Glade as event handlers. GladeVCP provides a way to import your module(s) at startup and will automatically link your event handlers with the widget signals as set in the Glade UI description.

### An example: adding custom user callbacks in Python

This is just a minimal example to convey the idea - details are laid out in the rest of this section.

GladeVCP can not only manipulate or display HAL pins, you can also write regular event handlers in Python. This could be used, among others, to execute MDI commands. Here’s how you do it:

Write a Python module like so and save as e.g. handlers.py:

    nhits = 0
    def on_button_press(gtkobj,data=None):
        global nhits nhits += 1 gtkobj.set_label("hits: %d" % nhits)

In Glade, define a button or HAL button, select the *Signals* tab, and in the GtkButton properties select the *pressed* line. Enter *on\_button\_press* there, and save the Glade file.

Then add the option *-u handlers.py* to the gladevcp command line. If your event handlers are spread over several files, just add multiple *-u &lt;pyfilename&gt;* options.

Now, pressing the button should change its label since it’s set in the callback function.

What the `-u` flag does: all Python functions in this file are collected and setup as potential callback handlers for your Gtk widgets - they can be referenced from Glade *Signals* tabs. The callback handlers are called with the particular object instance as parameter, like the GtkButton instance above, so you can apply any GtkButton method from there.

Or do some more useful stuff, like calling an MDI command!

### HAL value change events

HAL input widgets, like a LED, automatically associate their HAL pin state (on/off) with the optical appearance of the widget (LED lit/dark).

Beyond this builtin functionality, one may associate a change callback with any HAL pin, including those of predefined HAL widgets. This fits nicely with the event-driven structure of a typical widget application: every activity, be it mouse click, key, timer expired, or the change of a HAL pin’s value, generates a callback and is handled by the same orthogonal mechanism.

For user-defined HAL pins not associated with a particular HAL widget, the signal name is *value-changed*. See the [Adding HAL pins](#gladevcp:Adding_HAL_pins) section below for details.

HAL widgets come with a pre-defined signal called *hal-pin-changed*. See the [Hal Widgets section](#gladevcp::hal-pin-changed_signal) for details.

### Programming model

The overall approach is as follows:

-   design your UI with Glade, and set signal handlers where you want actions associated with a widget

-   write a Python module which contains callable objects (see *handler models* below)

-   pass your module’s path name to gladevcp with the *-u &lt;module&gt;* option

-   gladevcp imports the module, inspects it for signal handlers and connects them to the widget tree

-   the main event loop is run.

#### The simple handler model

For simple tasks it’s sufficient to define functions named after the Glade signal handlers. These will be called when the corresponding event happens in the widget tree. Here’s a trivial example - it assumes that the *pressed* signal of a Gtk Button or HAL Button is linked to a callback called *on\_button\_press*:

    nhits = 0
    def on_button_press(gtkobj,data=None):
        global nhits
        nhits += 1
        gtkobj.set_label("hits: %d" % nhits)

Add this function to a Python file and run as follows:

    gladevcp -u <myhandler>.py mygui.ui

Note communication between handlers has to go through global variables, which does not scale well and is positively un-pythonic. This is why we came up with the class-based handler model.

#### The class-based handler model

The idea here is: handlers are linked to class methods. The underlying class(es) are instantiated and inspected during GladeVCP startup and linked to the widget tree as signal handlers. So the task now is to write:

-   one or more several class definition(s) with one or several methods, in one module or split over several modules,

-   a function *get\_handlers* in each module which will return a list of class instances to GladeVCP - their method names will be linked to signal handlers

Here is a minimum user-defined handler example module:

    class MyCallbacks :
        def on_this_signal(self,obj,data=None):
            print "this_signal happened, obj=",obj

    def get_handlers(halcomp,builder,useropts):
        return [MyCallbacks ()]

Now, *on\_this\_signal* will be available as signal handler to your widget tree.

#### The get\_handlers protocol

If during module inspection GladeVCP finds a function `get_handlers`, it calls it as follows:

    get_handlers(halcomp,builder,useropts)

the arguments are:

-   halcomp - refers to the HAL component under construction

-   builder - widget tree - result of reading the UI definition (either referring to a GtkBuilder or libglade-type object)

-   useropts - a list of strings collected from the gladevcp command line `-U <useropts>` option

GladeVCP then inspects the list of class instances and retrieves their method names. Qualifying method names are connected to the widget tree as signal handlers. Only method names which do not begin with an *\_* (underscore) are considered.

Note that regardless whether you’re using the libglade or the new GtkBuilder format for your Glade UI, widgets can always be referred to as `builder.get_object(<widgetname>)`. Also, the complete list of widgets is available as `builder.get_objects()` regardless of UI format.

### Initialization sequence

It is important to know in which state of affairs your `get_handlers()` function is called so you know what is safe to do there and what not. First, modules are imported and initialized in command line order. After successful import, `get_handlers()` is called in the following state:

-   the widget tree is created, but not yet realized (no toplevel `window.show()` has been executed yet)

-   the halcomp HAL component is set up and all HAL widget’s pins have already been added to it

-   it is safe to add more HAL pins because `halcomp.ready()` has not yet been called at this point, so you may add your own pins, for instance in the class `__init__()` method.

Once all modules have been imported and method names extracted, the following steps happen:

-   all qualifying method names will be connected to the widget tree with `connect_signals()/signal_autoconnect()` (depending on the type of UI imported - GtkBuilder vs the old libglade format).

-   the HAL component is finalized with halcomp.ready()

-   if a window ID was passed as argument, the widget tree is re-parented to run in this window, and Glade’s toplevel window1 is abandoned (see FAQ)

-   if a HAL command file was passed with `-H halfile`, it is executed with halcmd

-   the Gtk main loop is run.

So when your handler class is initialized, all widgets are existent but not yet realized (displayed on screen). And the HAL component isn’t ready as well, so its unsafe to access pins values in your `__init__()` method.

If you want to have a callback to execute at program start after it is safe to access HAL pins, then a connect a handler to the realize signal of the top level window1 (which might be its only real purpose). At this point GladeVCP is done with all setup tasks, the halfile has been run, and GladeVCP is about to enter the Gtk main loop.

### Multiple callbacks with the same name

Within a class, method names must be unique. However, it is OK to have multiple class instances passed to GladeVCP by get\_handlers() with identically named methods. When the corresponding signal occurs, these methods will be called in definition order - module by module, and within a module, in the order class instances are returned by `get_handlers()`.

### The GladeVCP `-U <useropts>` flag

Instead of extending GladeVCP for any conceivable option which could potentially be useful for a handler class, you may use the -U &lt;useroption&gt; flag (repeatedly if you wish). This flag collects a list of &lt;useroption&gt; strings. This list is passed to the get\_handlers() function (useropts argument). Your code is free to interpret these strings as you see fit. An possible usage would be to pass them to the Python exec function in your `get_handlers()` as follows:

    debug = 0
    ...
    def get_handlers(halcomp,builder,useropts):
        ...
        global debug # assuming there's a global var
        for cmd in useropts:
            exec cmd in globals()

This way you can pass arbitrary Python statements to your module through the gladevcp -U option, for example:

    gladevcp -U debug=42 -U "print 'debug=%d' % debug" ...

This should set debug to 2 and confirm that your module actually did it.

### Persistent variables in GladeVCP

A annoying aspect of GladeVCP in its earlier form and pyvcp is the fact that you may change values and HAL pins through text entry, sliders, spin boxes, toggle buttons etc, but their settings are not saved and restored at the next run of Machinekit - they start at the default value as set in the panel or widget definition.

GladeVCP has an easy-to-use mechanism to save and restore the state of HAL widgets, and program variables (in fact any instance attribute of type int, float, bool or string).

This mechanism uses the popular *.ini* file format to save and reload persistent attributes.

#### Persistence, program versions and the signature check

Imagine renaming, adding or deleting widgets in Glade: an .ini file lying around from a previous program version, or an entirely different user interface, would be not be able to restore the state properly since variables and types might have changed.

GladeVCP detects this situation by a signature which depends on all object names and types which are saved and to be restored. In the case of signature mismatch, a new .ini file with default settings is generated.

### Using persistent variables

If you want any of Gtk widget state, HAL widgets output pin’s values and/or class attributes of your handler class to be retained across invocations, proceed as follows:

-   import the `gladevcp.persistence` module

-   decide which instance attributes, and their default values you want to have retained, if any

-   decide which widgets should have their state retained

-   describe these decisions in your handler class' `init()` method through a nested dictionary as follows:

    def __init__(self, halcomp,builder,useropts):
        self.halcomp = halcomp
        self.builder = builder
        self.useropts = useropts
        self.defaults = {
            # the following names will be saved/restored as method attributes
            # the save/restore mechanism is strongly typed - the variables type will be derived from the type of the
            # initialization value. Currently supported types are: int, float, bool, string
            IniFile.vars : { 'nhits' : 0, 'a': 1.67, 'd': True ,'c' : "a string"},
            # to save/restore all widget's state which might remotely make sense, add this:
            IniFile.widgets : widget_defaults(builder.get_objects())
            # a sensible alternative might be to retain only all HAL output widgets' state:
            # IniFile.widgets: widget_defaults(select_widgets(self.builder.get_objects(), hal_only=True,output_only = True)),
        }

Then associate an .ini file with this descriptor:

    self.ini_filename = __name__ + '.ini'
    self.ini = IniFile(self.ini_filename,self.defaults,self.builder)
    self.ini.restore_state(self)

After `restore_state()`, self will have attributes set if as running the following:

    self.nhits = 0
    self.a = 1.67
    self.d = True
    self.c = "a string"

Note that types are saved and preserved on restore. This example assumes that the ini file didn’t exist or had the default values from self.defaults.

After this incantation, you can use the following IniFil methods:

 ini.save\_state(obj)   
saves objs’s attributes as per IniFil.vars dictionary and the widget state as described in IniFile.widgets in self.defaults

 ini.create\_default\_ini()   
create a .ini file with default values

 ini.restore\_state(obj)   
restore HAL out pins and obj’s attributes as saved/initialized to default as above

### Saving the state on Gladvcp shutdown

To save the widget and/or variable state on exit, proceed as follows:

-   select some interior widget (type is not important, for instance a table).

-   in the *Signals* tab, select *GtkObject*. It should show a *destroy* signal in the first column.

-   add the handler name, e.g. *on\_destroy* to the second column.

-   add a Python handler like below:

    import gtk
    ...
    def on_destroy(self,obj,data=None):
        self.ini.save_state(self)

This will save state and shutdown GladeVCP properly, regardless whether the panel is embedded in Axis, or a standalone window.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Caution
</div></td>
<td align="left">Do not use <code>window1</code> (the toplevel window) to connect a <code>destroy</code> event. Due to the way a GladeVCP panel interacts with Axis if a panel is embedded within Axis, <strong>window1 will not receive destroy events properly</strong>. However, since on shutdown all widgets are destroyed, anyone will do. Recommended: use a second-level widget - for instance, if you have a table container in your panel, use that.</td>
</tr>
</tbody>
</table>

Next time you start the GladeVCP application, the widgets should come up in the state when the application was closed.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><div class="title">
Caution
</div></td>
<td align="left">The <em>GtkWidget</em> line has a similarly sounding <em>destroy-event</em> - <strong>dont use that to connect to the <em>on_destroy</em> handler, it wont work</strong> - make sure you use the <em>destroy</em> event from the <em>GtkObject</em> line.</td>
</tr>
</tbody>
</table>

### Saving state when Ctrl-C is pressed

By default, the reaction of GladeVCP to a Ctrl-C event is to just exit - `without` saving state. To make sure that this case is covered, add a handler call `on_unix_signal` which will be automatically be called on Ctrl-C (actuall on the SIGINT and SIGTERM signals). Example

    def on_unix_signal(self,signum,stack_frame):
        print "on_unix_signal(): signal %d received, saving state" % (signum)
        self.ini.save_state(self)

### Hand-editing .ini files

You can do that, but note that the values in self.defaults override your edits if there is a syntax or type error in your edit. The error is detected, a console message will hint about that happened, and the bad inifile will be renamed to have the .BAD suffix. Subsequent bad ini files overwrite earlier .BAD files.

### Adding HAL pins

If you need HAL pins which are not associated with a specific HAL widget, add them as follows:

    import hal_glib
    ...
    # in your handler class __init__():
    self.example_trigger = hal_glib.GPin(halcomp.newpin('example-trigger', hal.HAL_BIT, hal.HAL_IN))

To get a callback when this pin’s value changes, associate a `value-change` callback with this pin, add:

    self.example_trigger.connect('value-changed', self._on_example_trigger_change)

and define a callback method (or function, in this case leave out the `self` parameter):

    # note '_' - this method will not be visible to the widget tree
    def _on_example_trigger_change(self,pin,userdata=None):
        print "pin value changed to:" % (pin.get())

### Adding timers

Since GladeVCP uses Gtk widgets which rely on the [GObject](http://www.pygtk.org/pygtk2reference/gobject-functions.html) base class, the full glib functionally is available. Here is an example for a timer callback:

    def _on_timer_tick(self,userdata=None):
        ...
        return True # to restart the timer; return False for on-shot
    ...
    # demonstrate a slow background timer - granularity is one second
    # for a faster timer (granularity 1 ms), use this:
    # glib.timeout_add(100, self._on_timer_tick,userdata) # 10Hz
    glib.timeout_add_seconds(1, self._on_timer_tick)

### Setting HAL widget properties programmatically

With glade, widget properties are typically set fixed while editing. You can, however, set widget properties at runtime, for instance from ini file values, which would typically be done in the handler initialisation code. Setting properties from HAL pin values is possible, too.

In the following example (assuming a HAL Meter widget called `meter`), the meter’s min value is set from an INI file parameter at startup, and the max value is set via a HAL pin, which causese the widget’s scale to readjust dynamically:

    import machinekit
    import os
    import hal
    import hal_glib

    class HandlerClass:

        def _on_max_value_change(self,hal_pin,data=None):
            self.meter.max = float(hal_pin.get())
            self.meter.queue_draw() # force a widget redraw

        def __init__(self, halcomp,builder,useropts):
            self.builder = builder

            # hal pin with change callback.
            # When the pin's value changes the callback is executed.
            self.max_value = hal_glib.GPin(halcomp.newpin('max-value',  hal.HAL_FLOAT, hal.HAL_IN))
            self.max_value.connect('value-changed', self._on_max_value_change)

            inifile = machinekit.ini(os.getenv("INI_FILE_NAME"))
            mmin = float(inifile.find("METER", "MIN") or 0.0)
            self.meter = self.builder.get_object('meter')
            self.meter.min = mmin


    def get_handlers(halcomp,builder,useropts):
        return [HandlerClass(halcomp,builder,useropts)]

### Examples, and rolling your own GladeVCP application

Visit `machinekit_root_directory/configs/apps/gladevcp` for running examples and starters for your own projects.

FAQ
---

1.  *I get an unexpected unmap event in my handler function right after startup. What’s this?*

    This is a consequence of your Glade UI file having the window1 Visible property set to True, together with re-parenting the GladeVCP window into Axis or touchy. The GladeVCP widget tree is created, including a top level window, and then *reparented into Axis*, leaving that toplevel window laying around orphaned. To avoid having this useless empty window hanging around, it is unmapped (made invisible), which is the cause of the unmap signal you get. Suggested fix: set window1.visible to False, and ignore an initial unmap event.

2.  *My GladeVCP program starts, but no window appears where I expect it to be?*

    The window Axis allocates for GladeVCP will obtain the *natural size* of all its child widgets combined. It’s the child widget’s job to request a size (width and/or height). However, not all widgets do request a width greater than 0, for instance the Graph widget in its current form. If there’s such a widget in your Glade file and it’s the one which defines the layout you might want to set its width explicitly. Note that setting the window1 width and height properties in Glade does not make sense because this window will be orphaned during re-parenting and hence its geometry will have no impact on layout (see above). The general rule is: if you manually run a UI file with *gladevcp &lt;uifile&gt;* and its window has reasonable geometry, it should come up in Axis properly as well.

3.  *I want a blinking LED, but it wont blink*

    I ticked the checkbutton to let it blink with 100msec interval. It wont blink, and I get a startup warning: Warning: value "0" of type ‘gint’ is invalid or out of range for property ‘led-blink-rate’ of type ‘gint’? This seems to be a glade bug. Just type over the blink rate field, and save again - this works for me.

4.  *My gladevcp panel in Axis doesnt save state when I close Axis, although I defined an on\_destroy handler linked to the window destroy signal*

    Very likely this handler is linked to window1, which due to reparenting isnt usable for this purpose. Please link the on\_destroy handler to the destroy signal of an interior window. For instance, I have a notebook inside window1, and linked on\_destroy to the notebooks destroy signal, and that works fine. It doesnt work for window1.

5.  *I want to set the background color or text of a HAL\_Label widget depending on its HAL pin value*

    See the example in configs/apps/gladevcp/colored-label. Setting the background color of a GtkLabel widget (and HAL\_Label is derived from GtkLabel) is a bit tricky. The GtkLabel widget has no window object of its own for performance reasons, and only window objects can have a background color. The solution is to enclose the Label in an EventBox container, which has a window but is otherwise invisible - see the coloredlabel.ui file.

6.  *I defined a `hal_spinbutton` widget in glade, and set a default `value` property in the corresponding adjustment. It comes up with zero?*

    this is due to a bug in the old Gtk version distributed with Debian 8.04 and 10.04, and is likely to be the case for all widgets using adjustment. The workaround mentione for instance in <http://osdir.com/ml/gtk-app-devel-list/2010-04/msg00129.html> does not reliably set the HAL pin value, it is better to set it explicitly in an `on_realize` signal handler during widget creation. See the example in `configs/apps/gladevcp/by-widget/spinbutton.{ui,py}`.

Troubleshooting
---------------

-   make sure you have the development version of Machinekit installed. You don’t need the axisrc file any more, this was mentioned in the old GladeVcp wiki page.

-   run GladeVCP or Axis from a terminal window. If you get Python errors, check whether there’s still a `/usr/lib/python2.6/dist-packages/hal.so` file lying around besides the newer `/usr/lib/python2.6/dist-packages/_hal.so` (note underscore); if yes, remove the `hal.so` file. It has been superseded by hal.py in the same directory and confuses the import mechanism.

-   if you’re using run-in-place, do a *make clean* to remove any accidentally left over hal.so file, then *make*.

-   if you’re using *HAL\_table* or *HAL\_HBox* widgets, be aware they have an HAL pin associated with it which is off by default. This pin controls whether these container’s children are active or not.

Implementation note: Key handling in Axis
-----------------------------------------

We believe key handling works OK, but since it is new code, we’re telling about it you so you can watch out for problems; please let us know of errors or odd behavior. This is the story:

Axis uses the TkInter widget set. GladeVCP applications use Gtk widgets and run in a separate process context. They are hooked into Axis with the Xembed protocol. This allows a child application like GladeVCP to properly fit in a parent’s window, and - in theory - have integrated event handling.

However, this assumes that both parent and child application properly support the Xembed protocol, which Gtk does, but TkInter doesn’t. A consequence of this is that certain keys would not be forwarded from a GladeVCP panel to Axis properly under all circumstances. One of these situations was the case when an Entry, or SpinButton widget had focus: in this case, for instance an Escape key would not have been forwarded to Axis and cause an abort as it should, with potentially disastrous consequences.

Therefore, key events in GladeVCP are explicitly handled, and selectively forwarded to Axis, to assure that such situations cannot arise. For details, see the `keyboard_forward()` function in `lib/python/gladevcp/xembed.py`.

Adding Custom Widgets
---------------------

The Machinekit Wiki has information on adding custom widgets to GladeVCP. [GladeVCP Custom Widgets](http://wiki.machinekit.org/cgi-bin/wiki.pl?GladeVCP_Custom_Widgets)

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

