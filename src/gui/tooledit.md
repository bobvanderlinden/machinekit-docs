Tool Edit GUI
=============

<span id="cha:tooledit-gui"></span>

Tool Edit Version Requirements
------------------------------

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
<td align="left">The following configurable features for <em>tooledit</em> are available in versions 2.5.1 and later. In 2.5.0 the <em>tooledit</em> gui is without the configurable features.</td>
</tr>
</tbody>
</table>

![](images/tooledit.png)

The *tooledit* program can update the tool table file with edited changes by using the SaveFile button. The SaveFile button updates the system file but a separate action is required to update the tool table data used by a running Machinekit instance. With the axis GUI, both the file and the current tool table data used by Machinekit can be updated with the ReloadTable button. This button is enabled only when the machine is ON and IDLE.

Column Sorting
--------------

The tool table display can be sorted on any column in ascending order by clicking on the column header. A second click sorts in descending order. Column sorting requires that the machine is configured with the default tcl version &gt;= 8.5.

![](images/tooledit-sort.png)

On Debian Lucid 10.04 tcl/tk8.4 is the default. You can add tcl/tk8.5 with the commands:

    sudo apt-get install tcl8.5 tk8.5

Depending upon other applications installed on the system, it may be necessary to enable tcl/tk8.5 with the commands:

    sudo update-alternatives --config tclsh  ;# select the option for tclsh8.5
    sudo update-alternatives --config wish   ;# select the option for wish8.5

Column Selection
----------------

By default, the *tooledit* program will display all possible tool table parameter columns. Since few machines use all parameters, the columns displayed can be limited with the following ini file setting:

![](images/tooledit-columns.png)

INI File Syntax

    [DISPLAY]
    TOOL_EDITOR = tooledit column_name column_name ...

Example for Z and DIAM columns

    [DISPLAY]
    TOOL_EDITOR = tooledit Z DIAM

Stand Alone Use
---------------

The *tooledit* program can also be invoked as a standalone program. For example, if the program is in the user PATH, typing *tooledit* will show the usage syntax:

Stand Alone

    tooledit
    Usage:
    tooledit filename
    tooledit [column_1 ... column_n] filename

To synchronize a standalone *tooledit* with a running Machinekit application, the filename must resolve to the same \[EMCIO\]TOOL\_TABLE filename specified in the Machinekit ini file.

When using the program *tooledit* while Machinekit is running, gcode command execution or other programs may alter tool table data and the tool table file. File changes are detected by *tooledit* and a message is displayed:

    Warning: File changed by another process

The *tooledit* tool table display can be updated to read the modified file with the ReRead button.

he tool table is specified in the ini file with an entry:

    [EMCIO]TOOL_TABLE = tool_table_filename

The tool table file can be edited with any simple text editor (not a word processor).

The axis GUI can optionally use an ini file setting to specify the tool editor program:

    [DISPLAY]TOOL_EDITOR = path_to_editor_program

By default, the program named *tooledit* is used. This editor supports all tool table parameters, allows addition and deletion of tool entries, and performs a number of validity checks on parameter values.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET

