<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p></p></td>
<td align="left"><p><a href="../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p></p></td>
</tr>
</tbody>
</table>

1.  [Getting Help](#getting-help)

2.  [Before you ask: try yourself](#start-investigating)

3.  [How to report an Error](#reporting-an-error)

4.  [Before you hit: *Submit new issue*](#before-you-hit-send)

<span id="getting-help"></span>Getting help
===========================================

Nothing is more irritating

-   for you: to look silly. Everybody wants to look smart.

-   for the others: responding to silly questions, and to answer the same questions time and again.

-   extra irritating for everyone involved: answering a question which you could easily have answered yourself.

So: please make it easy for others to help you - pose a smart question. And to do so, please stick to the following rules and recommendations:

<span id="start-investigating"></span> Before you ask: try yourself
===================================================================

1.  Search your soul: **did it work before?** If yes - what did I change? and if I did - what was the last known-to-work status, and what happened after that? It is remotely possible that cosmic rays caused the error. But usually it’s something you did. What was it?

2.  start the application from the shell/a terminal window.

    1.  If you do not know what a terminal window or a shell is: read up on it elsewhere to understand that concept. Sorry, this group is about machinekit, not a Linux Basics bootcamp. It is the wrong place to ask such questions, and it is unlikely to be warmly welcome.

3.  if starting from the shell (for example, *machinekit some/path/to/my/config.ini*) reports an error: **read that error text, and try to make sense of it first**.

4.  **Read `/var/log/linuxcnc.log` and try to figure what is going wrong**. The error messages therein usually give a clear indication what failed, and sometimes also why. Some error messages even include suggestions how to remedy the situation.

5.  Repeat: Looking at `/var/log/linuxcnc.log` is NOT optional. **Do so now, before you report an error. No exceptions**.

6.  Search the Machinekit google group for a question already asked. There is [a search field where you can type your keywords](https://groups.google.com/forum/#!searchin/machinekit/please$20type$20your$20keywords$20here$20!).

7.  Do a search in the [Machinekit-docs search bar](https://github.com/machinekit/machinekit-docs/search?utf8=%E2%9C%93&q=Please+type+in+as+much+of+your+keywords+as+you+know+of!&type=Code).

8.  Do a search in the [Machinekit search bar](https://github.com/machinekit/machinekit/search?utf8=%E2%9C%93&q=Please+type+in+as+much+of+your+keywords+as+you+know+of!&type=Code).

9.  Have a look and the [search the issue tracker](https://github.com/machinekit/machinekit/issues?utf8=%E2%9C%93&q=Please+use+this+box+to+search+the+issue+tracker+list)

10. If no answer for your problem can be found **please report an issue [here](https://github.com/machinekit/machinekit/issues)**.

11. If you’re **not sure** if you should ask on the list, then **open a new issue first** and **then ask** on the Machinekit list. Follow the steps outlined under [How to report an Error](#reporting-an-error).

    -   Reporting errors on the Machinekit list is NOT a replacement and not recommended, as it does not improve the issue tracker content. This is why you should create an issue. That is what the tracker ist is for: helping others later to resolve a similar problem like you just encountered, and to track all the steps needed to resolve an issue **in a single place**. The mailing list is unsuited to track complex issues, and resolutions are hard to find. Also, if it was a new problem, and got resolved, that problem and the resolution will not be in the tracker. So i

<span id="reporting-an-error"></span>How to report an Error
===========================================================

So you did your homework as above, and still no resolution: file a [new issue](https://github.com/machinekit/machinekit/issues).

1.  Make up a **descriptive title**: *the foobar HAL component does not load* is a lot more helpful than *Yikes, Machinekit bombs on startup*. Narrow things down a bit, at least to a particular subsystem - it will improve your chances for a quick resolution. For example, *Gcode-interpreter does not add 1 to 1 properly* will likely raise the attention of persons familiar with the interpreter more reliably than stating *Machinekit math broken*.

2.  Include **all** the relevant detail:

    1.  **platform**. Add the output of the `uname -a`.

    2.  If you are running **Xenomai**, include the output of `dmesg |grep Xenomai` .

    3.  Before you run the program, do this to **increase the level of detail in the log**: `export DEBUG=5`

    4.  Paste the **complete output of the shell command** if any.

    5.  Use your text editor to **copy the contents of `/var/log/linuxcnc.log`** to [pastebin.com](http://pastebin.com) and put the generated URL like this one <http://pastebin.com/q3w7K4py> into your message. **Only paste the last run which failed**, not all of the log file. You find the beginning of the last run by moving to the end of the log, then searching backwards for the string `startup pid=` . Include everything from there to the end of the log file.

    6.  Report the **configuration**. If you used an unmodified stock configuration, past a link to the [pathname on github](https://github.com/machinekit/machinekit).

    7.  If you have a custom configuration, **fork the [machinekit repository on github](https://github.com/machinekit/machinekit) and push your changes there**.

        1.  This may sound daunting, but learning to use github is really easy - there are excellent online tutorials - and once you push you config, you have a remote backup. That can come really handy at times. So - instead of wasting a day on reinventing your config after a crash, invest some hours on learning the github basics - great payoff.

        2.  if you cannot use github for some reason, include all pertinent files like HAL or INI: upload them on [pastebin.com](http://pastebin.com) and add links in the error report.

        3.  Other than pastebin of a file, the copy in github shows the change history - i.e. what was changed, and why. So it is much easier on readers to look through git changes than to look at say a hal file with hundreds of lines and no history.

    8.  Report the **software version**. Depending if you have a

        1.  **package install**: add the output of `apt-cache showpkg machinekit|head -5`

        2.  **install from source**: run `git log|head -5` and include that in your report.

3.  If you are reporting an issue relating to interpreter, task or iocontrol:

    1.  unfortunately this part still uses different flags to enable logging, and different log output. To see what is going on, do this:

        1.  modify the DEBUG value, EMC section your INI file - start with 0x7fffff

        2.  run machinekit from a terminal window like so: *machinekit myconfig.ini |& tee /tmp/log.txt* . This will show the log both in your window as well as store it in /tmp/log.txt. When done, upload log.txt for others to see. Complete - no excerpts, no random copy and paste please.

4.  Please **avoid attaching screendumps** in the message if possible. Rather copy and paste text. This is why:

    1.  it is very hard to copy-paste when replying and pointing out stuff if somebody has to re-type from your screendump. So that won’t happen and you won’t get answered.

    2.  On mobile devices attachments are not always displayed, or displayed in insufficient resolution. In that case they are useless for the person trying to help you. And thus you won’t get answered.

<span id="before-you-hit-send"></span>Before you hit *Submit new issue*
=======================================================================

When you send an error report, you are asking for other people’s time to look at it. Make it easy for them. And - if you make it easy for them, it will increase your chances of getting a quick, spot-on reply. So it’s all in your enligthened self-interest to make it easy for them.

Whatever you write: **before hitting *send*, read your report through the eyes of a reader**:

1.  Readers are not standing behind you. They have no idea:

    1.  what you are actually trying to achieve. Sometimes there is an easier way to achieve what you are trying to do, and you will not hear about it if you do not let them know - because right now you are just trying to fix some nasty low-level issue. Share your big picture - surprising improvements may come from it.

    2.  if you try to run Machinekit on a washer and dryer, on a PC, or the WhizBang3000 board. Tell them.

    3.  what configuration you are running. Tell them.

    4.  which modifications, if any, you made. Tell them. Make them available for others to see, by pushing them to your github machinekit fork, and referring to them.

2.  so please help sour readers, by including **all** the facts. The readers are not a Forensics Team equipped to figure those facts out because you omitted them, and you are wasting **their** time, which is impolite. If you are appealing to other folk’s Crystal Ball: it is known to be a very erratic and extremely slow diagnostic device.

3.  the first step in fixing an error is to reproduce it. That means **an error report should include all the details to actually run the failing configuration**.

**If you have a conjecture what the cause of the error might be - say so, but AFTER reporting ALL the facts first**, and clearly marked it as your suspicion. For a reader trying to help, few things are more annoying than disentangling an inconclusive mixup of factoids and conjectures. One of them is: only conjectures, no facts. Being clear helps - all of us.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


