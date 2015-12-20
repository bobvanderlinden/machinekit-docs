<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="getting-started-platform.md">Back to Getting Started</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

Installing Packages
===================

Follow these steps to configure Apt and install a kernel and Machinekit packages:

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
<td align="left"><div class="paragraph">
<p>The kernel-naming convention used in these packages may change as experience is accumulated, especially with ARM-based systems. Be sure to check back here before installing a new kernel.</p>
</div></td>
</tr>
</tbody>
</table>

1.  **[Configure Apt](#configure-apt)**

    1.  [Configure Apt for i686](#configure-APT-i686-amd64-arm7)

    2.  [Configure Apt for arm6 (Raspberry)](#configure-APT-i686-arm6)

2.  <span id="install-kernels"></span>**[Install a realtime kernel](#install-RT-kernel)**

    1.  [Xenomai realtime kernel (all platforms)](#rt-kernel-xenomai)

    2.  [RT-PREEMPT realtime kernel (i686 and amd64)](#rt-kernel-rt-preempt)

    3.  [RTAI realtime kernel (x86 and amd64)](#rt-kernel-rtai)

3.  <span id="install-runtime-packs"></span>**[Install runtime packages](#install-runtime-packages)**

4.  **[Post installation](#post-installation)**

<span id="configure-apt"></span>Configure Apt
---------------------------------------------

### <span id="configure-APT-i686-amd64-arm7"></span>Configure Apt for i686, amd64 and arm7 (Beaglebone)

Copy and paste the following into a shell to configure the package archive:

    sudo sh -c \
      "echo 'deb http://deb.dovetail-automata.com wheezy main' > \
      /etc/apt/sources.list.d/machinekit.list; \
      apt-get update ; \
      apt-get install dovetail-automata-keyring"
    sudo apt-get update

continue with:

<table>
<colgroup>
<col width="10%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="#install-RT-kernel">install your kernel</a></p></td>
</tr>
</tbody>
</table>

### <span id="configure-APT-i686-arm6"></span>Configure Apt for arm6 (Raspberry)

Copy and paste the following into a shell to configure the package archive:

    sudo sh -c \
      "apt-key adv --keyserver hkp://keys.gnupg.net --recv-key 49550439; \
      echo 'deb http://0ptr.link/raspbian wheezy main' > \
      /etc/apt/sources.list.d/rpi-machinekit.list"
    sudo apt-get update

continue with:

<table>
<colgroup>
<col width="10%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="#install-RT-kernel">install your kernel</a></p></td>
</tr>
</tbody>
</table>

<span id="install-RT-kernel"></span>Install a realtime kernel
-------------------------------------------------------------

-   [Xenomai realtime kernel (all platforms)](#rt-kernel-xenomai)

-   [RT-PREEMPT realtime kernel (i686 and amd64)](#rt-kernel-rt-preempt)

-   [RTAI realtime kernel (x86 and amd64)](#rt-kernel-rtai)

### <span id="rt-kernel-xenomai"></span>Xenomai realtime kernel (all platforms)

Choose and copy and paste the following into a shell to install your kernel:

    sudo apt-get install linux-image-xenomai.x86-amd64        # amd64

    sudo apt-get install linux-image-xenomai.x86-686-pae      # i686

    sudo apt-get install linux-image-xenomai.beaglebone-omap  # beaglebone

    sudo apt-get install linux-image-xenomai                  # raspberry

continue with (choose):

[install runtime packages](#install-runtime-packages)

[install machinekit development packages](../developing/machinekit-developing.md)

### <span id="rt-kernel-rt-preempt"></span>RT-PREEMPT realtime kernel (i686 or amd64)

Choose and copy and paste the following into a shell to install your kernel:

    sudo apt-get install linux-image-rt-686-pae   # i686

    sudo apt-get install linux-image-rt-amd64     # amd64

continue with (choose):

[install runtime packages](#install-runtime-packages)

[install machinekit development packages](../developing/machinekit-developing.md)

### <span id="rt-kernel-rtai"></span>RTAI realtime kernel (x86 and amd64)

Choose and copy and paste the following into a shell to install your kernel:

    sudo apt-get install linux-image-rtai.x86-686-pae # i686

    sudo apt-get install linux-image-rtai.x86-amd64   # amd64

continue with (choose):

[install runtime packages](#install-runtime-packages)

[install machinekit development packages](../developing/machinekit-developing.md)

<span id="install-runtime-packages"></span>Install runtime packages
-------------------------------------------------------------------

For those wanting just Machinekit binaries, the following should install the main *machinekit* package for your kernel choice (multiple kernels and flavors possible):

    sudo apt-get install machinekit-rt-preempt

    sudo apt-get install machinekit-xenomai

    sudo apt-get install machinekit-rtai-kernel

    sudo apt-get install machinekit-posix # non-RT (aka 'simulator mode')

<span id="post-installation"></span>Post installation
-----------------------------------------------------

Some platforms need additional steps/instructions. Please see below for more information about specific platforms.

### <span id="post-installation-raspberry"></span>Raspberry

The kernel image needs to be copied to the boot partition like so:

    sudo mv /boot/kernel.img /boot/kernel.img.bck
    sudo cp /boot/vmlinuz* /boot/kernel.img

### <span id="post-installation-beaglebone"></span>Beaglebone

Please see [Alex’s installation hints](https://github.com/strahlex/asciidoc-sandbox/wiki/Creating-a-Machinekit-Debian-Image)

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="getting-started-platform.md">Back to Getting Started</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


