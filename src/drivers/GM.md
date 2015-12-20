General Mechatronics Driver
===========================

<span id="cha:gm-driver"></span>

General Mechatronics GM6-PCI card based motion control system

For detailed description, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

The GM6-PCI motion control card is based on an FPGA and a PCI bridge interface ASIC. A small automated manufacturing cell can be controlled, with a short time system integration procedure. The following figure demonstrating the typical connection of devices related to the control system:

-   It can control up to six axis, each can be stepper or CAN bus interface or analogue servo.

-   GPIO: Four time eight I/O pins are placed on standard flat cable headers.

-   RS485 I/O expander modules: RS485 bus was designed for interfacing with compact DIN-rail mounted expander modules. An 8-channel digital input, an 8-channel relay output and an analogue I/O (4x +/-10 Volts output and 8x +/-5 Volts input) modules are available now. Up to 16 modules can be connected to the bus altogether.

-   20 optically isolated input pins: Six times three for the direct connection of two end switch and one homing sensor for each axis. And additionally, two optically isolated E-stop inputs.

![GM servo control system](images/GMsystem.png)

Installing:

    loadrt hal_gm

During loading (or attempted loading) the driver prints some useful debugging messages to the kernel log, which can be viewed with dmesg.

Up to 3 boards may be used in one system.

The following connectors can be found on the GM6-PCI card:

![](images/GM_PCIpinout.png)

Figure 1. GM6-PCI card connectors and LEDs<span id="fig:PCI-card-connectors"></span>

I/O connectors
--------------

![](images/GM_IOpinout.png)

Figure 2. Pin numbering of GPIO connectors<span id="fig:pin-numbering-gpio"></span>

<table>
<caption>Table 1. Pinout of GPIO connectors<span id="table:gpio-pinout"></span></caption>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">9</th>
<th align="left">7</th>
<th align="left">5</th>
<th align="left">3</th>
<th align="left">1</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>IOx/7</p></td>
<td align="left"><p>IOx/5</p></td>
<td align="left"><p>IOx/3</p></td>
<td align="left"><p>IOx/1</p></td>
<td align="left"><p>VCC</p></td>
</tr>
</tbody>
</table>

<table>
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">10</th>
<th align="left">8</th>
<th align="left">6</th>
<th align="left">4</th>
<th align="left">2</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>GND</p></td>
<td align="left"><p>IOx/6</p></td>
<td align="left"><p>IOx/4</p></td>
<td align="left"><p>IOx/2</p></td>
<td align="left"><p>IOx/0</p></td>
</tr>
</tbody>
</table>

Each pin can be configured as digital input or output. GM6-PCI motion control card has 4 general purpose I/O (GPIO) connectors, with eight configurable I/O on each. Every GPIO pin and parameter name begins as follows:

    gm.<nr. of card>.gpio.<nr of gpio con>

,where &lt;nr of gpio con&gt; is form 0 to 3. For example:

    gm.0.gpio.0.in-0

indicates the state of the first pin of the first GPIO connector on the GM6-PCI card. Hal pins are updated by function

    gm.<nr of card>.read

### Pins

<table>
<caption>Table 2. GPIO pins<span id="table:gpio-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Output pin. Used only when GPIO is set to output.</p></td>
</tr>
</tbody>
</table>

### Parameters

<table>
<caption>Table 3. GPIO parameters<span id="table:gpio-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.is-out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, the corresponding GPIO is set to totem-pole output, other wise set to high impedance input.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-out-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, pin value will be inverted. Used when pin is configured as output.</p></td>
</tr>
</tbody>
</table>

Axis connectors
---------------

![](images/GM_AXISpinout.png)

Figure 3. Pin numbering of axis connectors<span id="fig:pin-numbering-axis"></span>

<table>
<caption>Table 4. Pinout of axis connectors<span id="table:axis-pinout"></span></caption>
<colgroup>
<col width="20%" />
<col width="80%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left"><p>1</p></td>
<td align="left"><p>Encoder A</p></td>
</tr>
<tr class="even">
<td align="left"><p>2</p></td>
<td align="left"><p>+5 Volt (PC)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>3</p></td>
<td align="left"><p>Encoder B</p></td>
</tr>
<tr class="even">
<td align="left"><p>4</p></td>
<td align="left"><p>Encoder Index</p></td>
</tr>
<tr class="odd">
<td align="left"><p>5</p></td>
<td align="left"><p>Fault</p></td>
</tr>
<tr class="even">
<td align="left"><p>6</p></td>
<td align="left"><p>Power Enabled</p></td>
</tr>
<tr class="odd">
<td align="left"><p>7</p></td>
<td align="left"><p>Step/CCW/B</p></td>
</tr>
<tr class="even">
<td align="left"><p>8</p></td>
<td align="left"><p>Direction/CW/A</p></td>
</tr>
<tr class="odd">
<td align="left"><p>9</p></td>
<td align="left"><p>Ground (PC)</p></td>
</tr>
<tr class="even">
<td align="left"><p>10</p></td>
<td align="left"><p>DAC serial line</p></td>
</tr>
</tbody>
</table>

### Axis interface modules

Small sized DIN rail mounted interface modules gives easy way of connecting different types of servo modules to the axis connectors. Seven different system configurations are presented in the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf) for evaluating typical applications. Also the detailed description of the Axis modules can be found in the System integration manual.

For evaluating the appropriate servo-drive structure the modules have to be connected as the following block diagram shows:

![](images/GM_AxisInterface.png)

Figure 4. Servo axis interfaces<span id="fig:axis-iterface"></span>

### Encoder

The GM6-PCI motion control card has six encoder modules. Each encoder module has three channels:

-   Channel-A

-   Channel-B

-   Channel-I (index)

It is able to count quadrature encoder signals or step/dir signals. Each encoder module is connected to the inputs of the corresponding RJ50 axis connector.

Every encoder pin and parameter name begins as follows:

    gm.<nr. of card>.encoder.<nr of axis>

,where &lt;nr of axis&gt; is form 0 to 5. For example:

    gm.0.encoder.0.position

refers to the position of encoder module of axis 0.

The GM6-PCI card counts the encoder signal independently from Machinekit. Hal pins are updated by function:

    gm.<nr of card>.read

#### Pins

<table>
<caption>Table 5. Encoder pins<span id="table:encoder-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.reset</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>When True, resets counts and position to zero.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.rawcounts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>The raw count is the counts, but unaffected by reset or the index pulse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.counts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position in encoder counts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position in scaled units (=.counts/.position-scale).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.index-enabled</p></td>
<td align="left"><p>(bit, IO)</p></td>
<td align="left"><p>When True, counts and position are rounded or reset (depends on index-mode) on next rising edge of channel-I. Every time position is reset because of Index, index-enabled pin is set to 0 and remain 0 until connected hal pin does not set it.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.velocity</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Velocity in scaled units per second. GM encoder uses high frequency hardware timer to measure time between encoder pulses in order to calculate velocity. It greatly reduces quantization noise as compared to simply differentiating the position output. When the measured velocity is below min-velocity-estimate, the velocity output is 0.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 6. Encoder parameters<span id="table:encoder-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and Read/Write</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.counter-mode</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, the counter counts each rising edge of the channel-A input to the direction determined by channel-B. This is useful for counting the output of a single channel (non-quadrature) or step/dir signal sensor. When false, it counts in quadrature mode.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.index-mode</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True and .index-enabled is also true, .counts and .position are rounded (based on .counts-per-rev) at rising edge of channel-I. This is useful to correct few pulses error caused by noise. In round mode, it is essential to set .counts-per-rev parameter correctly. When .index-mode is False and .index-enabled is true, .counts and .position are reset at channel-I pulse.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.counts-per-rev</p></td>
<td align="left"><p>(s32, R/V)</p></td>
<td align="left"><p>Determine how many counts are between two index pulses. It is used only in round mode, so when both .index-enabled and .index-mode parameters are True. GM encoder process encoder signal in 4x mode, so for example in case of a 500 CPR encoder it should be set to 2000. This parameter can be easily measured by setting .index-enabled True and .index-mode False (so that .counts resets at channel-I pulse), than move axis by hand and see the maximum magnitude of .counts pin in halmeter.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.index-invert</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, channel-I event (reset or round) occur on falling edge of channel-I signal, otherwise on rising edge.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.min-speed-estimate</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Determine the minimum measured velocity magnitude at which .velocity will be set as nonzero. Setting this parameter too low will cause it to take a long time for velocity to go to zero after encoder pulses have stopped arriving.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in counts per length unit. .position=.counts/.position-scale. For example, if position-scale is 2000, then 1000 counts of the encoder will produce a position of 0.5 units.</p></td>
</tr>
</tbody>
</table>

#### HAL example

Setting encoder module of axis 0 to receive 500 CPR quadrature encoder signal and use reset to round position.

    setp gm.0.encoder.0.counter-mode 0         # 0: quad, 1: stepDir
    setp gm.0.encoder.0.index-mode 1           # 0: reset pos at index, 1:round pos at index
    setp gm.0.encoder.0.counts-per-rev 2000      # GM process encoder in 4x mode, 4x500=2000
    setp gm.0.encoder.0.index-invert 0
    setp gm.0.encoder.0.min-speed-estimate 0.1 # in position unit/s
    setp gm.0.encoder.0.position-scale 20000   # 10 encoder rev cause the machine to
                                                 move one position unit (10x2000)

Connect encoder position to Machinekit position feedback:

    net Xpos-fb gm.0.encoder.0.position => axis.0.motor-pos-fb

### Stepgen module

The GM6-PCI motion control card has six stepgen modules, one for each axis. Each module has two output signals. It can produce Step/Direction, Up/Down or Quadrature (A/B) pulses. Each stepgen module is connected to the pins of the corresponding RJ50 axis connector.

Every stepgen pin and parameter name begins as follows:

    gm.<nr. of card>.stepgen.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.stepgen.0.position-cmd

refers to the position command of stepgen module of axis 0 on card 0.

The GM6-PCI card generates step pulses independently from Machinekit. Hal pins are updated by function

    gm.<nr of card>.write

#### Pins

<table>
<caption>Table 7. Stepgen module pins<span id="table:stepgen-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Stepgen produces pulses only when this pin is true.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.count-fb</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position feedback in counts unit.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-fb</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position feedback in position unit.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded position in position units. Used in position mode only.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.velocity-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded velocity in position units per second. Used in velocity mode only.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 8. Stepgen module parameters<span id="table:stepgen-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and Read/Write</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.step-type</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>When 0, module produces Step/Dir signal. When 1, it produces Up/Down step signals. And when it is 2, it produces quadrature output signals.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.control-type</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>When True, .velocity-cmd is used as reference and velocity control calculate pulse rate output. When False, .position-cmd is used as reference and position control calculate pulse rate output.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.invert-step1</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Invert the output of channel 1 (Step signal in StepDir mode)</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-step2</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Invert the output of channel 2 (Dir signal in StepDir mode)</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.maxvel</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum velocity in position units per second. If it is set to 0.0, .maxvel parameter is ignored.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.maxaccel</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum acceleration in position units per second squared. If it is set to 0.0, .maxaccel parameter is ignored.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in steps per length unit.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.steplen</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Length of step pulse in nano-seconds.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.stepspace</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Minimum time between two step pulses in nano-seconds.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dirdelay</p></td>
<td align="left"><p>(u32, R/W)</p></td>
<td align="left"><p>Minimum time between step pulse and direction change in nano-seconds.</p></td>
</tr>
</tbody>
</table>

For evaluating the appropriate values see the timing diagrams below:

![](images/GM_RefSignals.png)

Figure 5. Reference signal timing diagrams<span id="fig:refsig-timing-diagram"></span>

#### HAL example

Setting stepgen module of axis 0 to generate 1000 step pulse per position unit:

    setp gm.0.stepgen.0.step-type 0         # 0:stepDir, 1:UpDown, 2:Quad
    setp gm.0.stepgen.0.control-type 0      # 0:Pos. control, 1:Vel. Control
    setp gm.0.stepgen.0.invert-step1 0
    setp gm.0.stepgen.0.invert-step2 0
    setp gm.0.stepgen.0.maxvel 0            # do not set maxvel for step
                                            # generator, let interpolator control it.
    setp gm.0.stepgen.0.maxaccel 0          # do not set max acceleration for
                                            # step generator, let interpolator control it.
    setp gm.0.stepgen.0.position-scale 1000 # 1000 step/position unit
    setp gm.0.stepgen.0.steplen 1000        # 1000 ns = 1 us
    setp gm.0.stepgen.0.stepspace1000       # 1000 ns = 1 us
    setp gm.0.stepgen.0.dirdelay 2000       # 2000 ns = 2 us

Connect stepgen to axis 0 position reference and enable pins:

    net Xpos-cmd axis.0.motor-pos-cmd => gm.0.stepgen.0.position-cmd
    net Xen axis.0.amp-enable-out => gm.0.stepgen.0.enable

### Enable and Fault signals

The GM6-PCI motion control card has one enable output and one fault input HAL pins, both are connected to each RJ50 axis connector and to the CAN connector.

Hal pins are updated by function:

    gm.<nr of card>.read

#### Pins

<table>
<caption>Table 9. Enable and Fault signal pins<span id="table:enable-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.power-enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>If this pin is True,</p>
<p>* and Watch Dog Timer is not expired * and there is no power fault Then power enable pins of axis- and CAN connectors are set to high, otherwise set to low.</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.&lt;nr of card&gt;.power-fault</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Power fault input.</p></td>
</tr>
</tbody>
</table>

### Axis DAC

The GM6-PCI motion control card has six serial axis DAC driver modules, one for each axis. Each module is connected to the pin of the corresponding RJ50 axis connector. Every axis DAC pin and parameter name begins as follows:

    gm.<nr. of card>.dac.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.dac.0.value

refers to the output voltage of DAC module of axis 0. Hal pins are updated by function:

    gm.<nr of card>.write

#### Pins

<table>
<caption>Table 10. Axis DAC pins<span id="table:dac-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable DAC output. When enable is false, DAC output is 0.0 V.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.value</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Value of DAC output in Volts.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 11. Axis DAC parameters<span id="table:dac-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.offset</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is added to the value before the hardware is updated</p></td>
</tr>
<tr class="even">
<td align="left"><p>.high-limit</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.low-limit</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Minimum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.invert-serial</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>GM6-PCI card is communicating with DAC hardware via fast serial communication to highly reduce time delay compared to PWM. DAC module is recommended to be isolated which is negating serial communication line. In case of isolation, leave this parameter to default (0), while in case of none-isolation, set this parameter to 1.</p></td>
</tr>
</tbody>
</table>

CAN-bus servo amplifiers
------------------------

The GM6-PCI motion control card has CAN module to drive CAN servo amplifiers. Implementation of higher level protocols like CANopen is further development. Currently GM produced power amplifiers has upper level driver which export pins and parameters to HAL. They receive position reference and provide encoder feedback via CAN bus.

The frames are standard (11 bit) ID frames, with 4 byte data length. Tha baud rate is 1 Mbit. The position commad IDs for axis 0..5 are 0x10..0x15. The position feedback IDs for axis 0..5 are 0x20..0x25.

These configuration can be changed with the modifivation of hal\_gm.c and recompiling Machinekit.

Every CAN pin and parameter name begins as follows:

    gm.<nr. of card>.can-gm.<nr of axis>

,where &lt;nr of axis&gt; is form 0 to 5. For example:

    gm.0.can-gm.0.position

refers to the output position of axis 0 in position units.

Hal pins are updated by function:

    gm.<nr of card>.write

### Pins

<table>
<caption>Table 12. CAN module pins<span id="table:can-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.enable</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable sending position references.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.position-cmd</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Commanded position in position units.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.position-fb</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Feed back position in position units.</p></td>
</tr>
</tbody>
</table>

### Parameters

<table>
<caption>Table 13. CAN module parameters<span id="table:can-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in per length unit.</p></td>
</tr>
</tbody>
</table>

Watchdog timer
--------------

Watchdog timer resets at function:

    gm.<nr of card>.read

### Pins

<table>
<caption>Table 14. Watchdog pins<span id="table:watchdog-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-expired</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Indicates that watchdog timer is expired.</p></td>
</tr>
</tbody>
</table>

Watchdog timer overrun causes the set of power-enable to low in hardware.

### Parameters

<table>
<caption>Table 15. Watchdog parameters<span id="table:watchdog-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-enable</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Enable watchdog timer. It is strongly recommended to enable watchdog timer, because it can disables all the servo amplifiers by pulling down all enable signal in case of PC error.</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.&lt;nr of card&gt;.watchdog-timeout-ns</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Time interval in within the gm.&lt;nr of card&gt;.read function must be executed. The gm.&lt;nr of card&gt;.read is typically added to servo-thread, so watch timeout is typically set to 3 times of the servo period.</p></td>
</tr>
</tbody>
</table>

End-, homing- and E-stop switches
---------------------------------

![](images/GM_ENDSWpinout.png)

Figure 6. Pin numbering of homing & end switch connector<span id="fig:pin-numbering-endsw"></span>

Table 16. End- and homing switch connector pinout<span id="table:end-and-homing-switch-connector-pinout"></span>
**25**
**23**
**21**
**19**
**17**
**15**
**13**
**11**
**9**
**7**
**5**
**3**
**1**
GND

1/End-

2/End+

2/Hom-ing

3/End-

4/End+

4/Hom-ing

5/End-

6/End+

6/Hom-ing

E-Stop 2

V+ (Ext.)

**26**
**24**
**22**
**20**
**18**
**16**
**14**
**12**
**10**
**8**
**6**
**4**
**2**
GND

1/End+

1/Hom-ing

2/End-

3/End+

3/Hom-ing

4/End-

5/End+

5/Hom-ing

6/End-

E-Stop 1

V+ (Ext.)

The GM6-PCI motion control card has two limit- and one homing switch input for each axis. All the names of these pins begin as follows:

    gm.<nr. of card>.axis.<nr of axis>

,where nr of axis is form 0 to 5. For example:

    gm.0.axis.0.home-sw-in

indicates the state of the axis 0 home switch.

Hal pins are updated by function:

    gm.<nr of card>.read

### Pins

<table>
<caption>Table 17. End- and homing switch pins<span id="table:end-and-homing-switch-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.home-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Home switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.home-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated home switch input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.neg-lim-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negative limit switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.neg-lim-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated negative limit switch input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.pos-lim-sw-in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Positive limit switch input</p></td>
</tr>
<tr class="even">
<td align="left"><p>.pos-lim-sw-in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated positive limit switch input</p></td>
</tr>
</tbody>
</table>

### Parameters

<table>
<caption>Table 18. E-stop switch parameters<span id="table:e-stop-switch-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>gm.0.estop.0.in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Estop 0 input</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.0.estop.0.in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated Estop 0 input</p></td>
</tr>
<tr class="odd">
<td align="left"><p>gm.0.estop.1.in</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Estop 1 input</p></td>
</tr>
<tr class="even">
<td align="left"><p>gm.0.estop.1.in-not</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated Estop 1 input</p></td>
</tr>
</tbody>
</table>

Status LEDs
-----------

### CAN

Color: Orange

-   Blink, during data communication.

-   On, when any of the buffers are full - communication error.

-   Off, when no data communication.

### RS485

Color: Orange

-   Blink, during initialization of modules on the bus

-   On, when the data communication is up between all initialized modules.

-   Off, when any of the initialized modules dropped off because of an error.

### EMC

Color: White

-   Blink, when Machinekit is running.

-   Otherwise off.

### Boot

Color: Green

-   On, when system booted successfully.

-   Otherwise off.

### Error

Color: Red

-   Off, when there is no fault in the system.

-   Blink, when PCI communication error.

-   On, when watchdog timer overflowed.

RS485 I/O expander modules
--------------------------

These modules were developed for expanding the I/O and function capability along an RS485 line of the GM6-PCI motion control card.

Available module types:

-   8-channel relay output module - gives eight NO-NC relay output on a three pole terminal connector for each channel.

-   8-channel digital input module - gives eight optical isolated digital input pins.

-   8 channel ADC and 4-channel DAC module - gives four digital-to-analogue converter outputs and eight analogue-to-digital inputs. This module is also optically isolated from the GM6-PCI card.

**Automatic node recognizing:**

Each node connected to the bus was recognized by the GM6-PCI card automatically. During starting Machinekit, the driver export pins and parameters of all available modules automatically.

**Fault handling:**

If a module does not answer regularly the GM6-PCI card drops down the module. If a module with output do not gets data with correct CRC regularly, the module switch to error sate (green LED blinking), and turns all outputs to error sate.

**Connecting the nodes:**

The modules on the bus have to be connected in serial topology, with termination resistors on the end. The start of the topology is the PCI card, and the end is the last module.

![](images/GM_RS485topology.png)

Figure 7. Connecting the RS485 nodes to the GM6-PCI card<span id="fig:connecting-rs485"></span>

**Adressing:**

Each node on the bus has a 4 bit unique address that can be set with a red DIP switch.

**Status LED:**

A green LED indicates the status of the module:

-   Blink, when the module is only powered, but not jet identified, or when module is dropped down.

-   Off, during identification (computer is on, but Machinekit not started)

-   On, when it communicates continuously.

### Relay output module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

#### Pins

<table>
<caption>Table 19. Relay output module pins<span id="table:rs485-relay-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.relay-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Output pin for relay</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 20. Relay output module parameters<span id="table:rs485-relay-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.invert-relay-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, R/W)</p></td>
<td align="left"><p>Negate relay output pin</p></td>
</tr>
</tbody>
</table>

#### HAL example

    gm.0.rs485.0.relay-0 # First relay of the node.
    gm.0                 # Means the first GM6-PCI motion control card (PCI card address = 0)
    .rs485.0             # Select node with address 0 on the RS485 bus
    .relay-0             # Select the first relay

### Digital input module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

#### Pins

<table>
<caption>Table 21. Digital input output module pins<span id="table:rs485-input-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
</tbody>
</table>

#### HAL example

    gm.0.rs485.0.in-0 # First input of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .in-0    - Select the first digital input module

### DAC & ADC module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15.

#### Pins

<table>
<caption>Table 22. DAC &amp; ADC module pins<span id="table:rs485-dacadc-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Value of ADC input in Volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dac-enable-&lt;0-3&gt;</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>Enable DAC output. When enable is false DAC output is set to 0.0 V.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, In)</p></td>
<td align="left"><p>Value of DAC output in Volts.</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 23. DAC &amp; ADC module parameters<span id="table:rs485-dacadc-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-scale-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>The input voltage will be multiplied by scale before being output to .adc- pin.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.adc-offset-&lt;0-7&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is subtracted from the hardware input voltage after the scale multiplier has been applied.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-offset-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is added to the value before the hardware is updated.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.dac-high-limit-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Maximum output voltage of the hardware in volts.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.dac-low-limit-&lt;0-3&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Minimum output voltage of the hardware in volts.</p></td>
</tr>
</tbody>
</table>

#### HAL example

    gm.0.rs485.0.adc-0 # First analogue channel of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .adc-0   - Select the first analogue input of the module

### Teach Pendant module

For pinout, connection and electrical charasteristics of the module, please refer to the [System integration manual](http://www.generalmechatronics.com/data/products/robot_controller/PCI_UserManual_eng.pdf).

All the pins and parameters are updated by the following function:

    gm.<nr. of card>.rs485

It should be added to servo thread or other thread with larger period to avoid CPU overload. Every RS485 module pin and parameter name begins as follows:

    gm.<nr. of card>.rs485.<modul ID>

,where &lt;modul ID&gt; is form 00 to 15. Note that on the Teach Pendant module it cannot be changed, and pre-programmed as zero. Upon request it can be delivered with firmware pre-programmed different ID.

#### Pins

<table>
<caption>Table 24. Teach Pendant module pins<span id="table:rs485-teachpendant-pins"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Pins</th>
<th align="left">Type and direction</th>
<th align="left">Pin description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Value of ADC input in Volts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.enc-reset</p></td>
<td align="left"><p>(bit, In)</p></td>
<td align="left"><p>When True, resets counts and position to zero.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-counts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>Position in encoder counts.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.enc-rawcounts</p></td>
<td align="left"><p>(s32, Out)</p></td>
<td align="left"><p>The raw count is the counts, but unaffected by reset.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-position</p></td>
<td align="left"><p>(float, Out)</p></td>
<td align="left"><p>Position in scaled units (=.enc-counts/.enc-position-scale).</p></td>
</tr>
<tr class="even">
<td align="left"><p>.in-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Input pin</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.in-not-&lt;0-7&gt;</p></td>
<td align="left"><p>(bit, Out)</p></td>
<td align="left"><p>Negated input pin</p></td>
</tr>
</tbody>
</table>

#### Parameters

<table>
<caption>Table 25. Teach Pendant module parameters<span id="table:rs485-teachpendant-parameters"></span></caption>
<colgroup>
<col width="27%" />
<col width="18%" />
<col width="54%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parameters</th>
<th align="left">Type and direction</th>
<th align="left">Parameter description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>.adc-scale-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>The input voltage will be multiplied by scale before being output to .adc- pin.</p></td>
</tr>
<tr class="even">
<td align="left"><p>.adc-offset-&lt;0-5&gt;</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Offset is subtracted from the hardware input voltage after the scale multiplier has been applied.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>.enc-position-scale</p></td>
<td align="left"><p>(float, R/W)</p></td>
<td align="left"><p>Scale in per length unit.</p></td>
</tr>
</tbody>
</table>

#### HAL example

    gm.0.rs485.0.adc-0 # First analogue channel of the node.
    # gm.0     - Means the first GM6-PCI motion control card (PCI card address = 0)
    # .rs485.0 - Select node with address 0 on the RS485 bus
    # .adc-0   - Select the first analogue input of the module

Errata
------

### GM6-PCI card Errata

The revision number in this section refers to the revision of the GM6-PCI card device.

#### Rev. 1.2

-   Error: The PCI card do not boot, when Axis 1. END B switch is active (low). Found on November 16, 2013.

-   Reason: This switch is connected to a boot setting pin of FPGA

-   Problem fix/workaround: Use other switch pin, or connect only normally open switch to this switch input pin.

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


