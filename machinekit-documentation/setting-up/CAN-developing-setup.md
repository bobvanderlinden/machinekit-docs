<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="developing-setting-up.md">Back to setting up for development</a></p></td>
<td align="left"><p><a href="../developing/CAN-developing.md">Back to CAN developing</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

install can-utils and wireshark by opening a terminal.

    me@VM:~$ sudo apt-get install can-utils wireshark

Next action will be to start the can interface when starting up.

    me@VM:~$ sudo gedit /etc/modules

and add `vcan` to this file then save and exit.

then add the following to the next file

    me@VM:~$ sudo gedit /etc/rc.local

and add \`\`\` ip link add dev vcan0 type vcan ip link set up vcan0 \`\`\`

Make sure that Wireshark can sniff packets without running as a root. This information tells us all what we need to know.

    me@VM:~$ less /usr/share/doc/wireshark-common/README.Debian

so when we have read this we do:

    me@VM:~$ dpkg-reconfigure wireshark-common
    me@VM:~$ sudo usermod -a -G wireshark me

"me" with your {own username}.

Reboot your machine, log in, and start wireshark. you should see the following screen. Notice "vcan0"

![](images/wireshark-startup.png)

start capturing the vcan0 interface by selecting the `vcan0` interface and clicking `start`.

Open a terminal and give the following command:

    me@VM:~$ cansend vcan0 001#04.01.00.00.0f.ff.e7.00

Return to Wireshark and note that the you see the communication.

![](images/captured-can-packet.png)

You are now ready to return to the the [CAN developing page](../developing/CAN-developing.md)

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="developing-setting-up.md">Back to setting up for development</a></p></td>
<td align="left"><p><a href="../developing/CAN-developing.md">Back to CAN developing</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


