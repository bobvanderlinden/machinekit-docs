<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="developing.md">Back to Developing</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

<span id="packages-developing"></span>Development packages
----------------------------------------------------------

For those wanting to build Machinekit from sources and install a development environment, install developing packages:

    sudo apt-get install libczmq-dev python-zmq libjansson-dev \
      libwebsockets-dev libxenomai-dev python-pyftpdlib

Further more, add wheezy-backports in the package archive for cython 0.19:

    sudo sh -c \
      "echo 'deb http://ftp.us.debian.org/debian wheezy-backports main' > \
      /etc/apt/sources.list.d/wheezy-backports.list"
    sudo apt-get update
    sudo apt-get install -t wheezy-backports cython

Full package listing for building and developing on Jessie (subject to constant update)

    sudo apt-get install -y  build-essential debhelper kernel-package libpth-dev libgtk2.0-dev tcl8.6-dev tk8.6-dev \

    bwidget python-old-doctools python-tk python-dev libglu1-mesa-dev libgtk2.0-dev libgnomeprintui2.2-dev \

    libncurses5-dev libxaw7-dev gettext libreadline-gplv2-dev lyx texlive-extra-utils imagemagick texinfo groff \

    libmodbus-dev;

    sudo apt-get install -y  libudev-dev libmodbus-dev libboost-python-dev libboost-serialization-dev \

    libboost-thread-dev libtk-img automake autoconf libtool libusb-dev;

    sudo apt-get install -y  automake1.11 libtool liburiparser-dev cmake libssl-dev  openssl python-setuptools \

    libusb-1.0-0-dev libudev-dev  uuid-dev libavahi-client-dev libavahi-compat-libdnssd-dev  avahi-daemon \

    libprotobuf-dev protobuf-compiler python-protobuf libprotoc-dev uuid-runtime python-avahi python-netifaces \

    avahi-discover;

    sudo apt-get install -y  python-nose check pyftpd libczmq-dev python-zmq libjansson-dev libwebsockets-dev \

    python-pyftpdlib libzmq3-dev ;

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p><a href="developing.md">Back to Developing</a></p></td>
<td align="left"><p><a href="../../index.md">Back to the main Documents index</a></p></td>
<td align="left"><p><a href="../documentation-matrix.md">Documentation matrix</a></p></td>
</tr>
</tbody>
</table>

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


