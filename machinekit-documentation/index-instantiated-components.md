Index of Instantiated HAL Components
====================================

The essential difference between normal realtime components and instantiable components,

is that *normal components* are loaded once only and have to create as many copies of the

component as they need, all at once, there and then.

Instantiable components load the *base component* and from that can create instances.

of the component whenever they are required.

So instances can be created in separate hal files, on-the-fly or whatever.

Amoungst many advantages, it opens up huge flexibility in configuration.

Further info [here](../src/hal/new-instantiated-components.md)

The available Instantiated HAL Components are:
----------------------------------------------

-   [abs](components/abs.md)

-   [abs\_s32](components/abs_s32.md)

-   [and2](components/and2.md)

-   [andn](components/andn.md)

-   [at\_pid](components/at_pid.md)

-   [bin2gray](components/bin2gray.md)

-   [biquad](components/biquad.md)

-   [bitslice](components/bitslice.md)

-   [bitwise](components/bitwise.md)

-   [bldc\_hall3](components/bldc_hall3.md)

-   [blend](components/blend.md)

-   [clarke2](components/clarke2.md)

-   [clarke3](components/clarke3.md)

-   [clarkeinv](components/clarkeinv.md)

-   [comp](components/comp.md)

-   [constant](components/constant.md)

-   [ddt](components/ddt.md)

-   [deadzone](components/deadzone.md)

-   [debounce](components/debounce.md)

-   [div2](components/div2.md)

-   [edge](components/edge.md)

-   [estop\_latch](components/estop_latch.md)

-   [feedcomp](components/feedcomp.md)

-   [flipflop](components/flipflop.md)

-   [gantry](components/gantry.md)

-   [gearchange](components/gearchange.md)

-   [gray2bin](components/gray2bin.md)

-   [hbridge](components/hbridge.md)

-   [hypot](components/hypot.md)

-   [idb](components/idb.md)

-   [ilowpass](components/ilowpass.md)

-   [integ](components/integ.md)

-   [invert](components/invert.md)

-   [io\_muxn](components/io_muxn.md)

-   [joyhandle](components/joyhandle.md)

-   [knob2float](components/knob2float.md)

-   [latencybins](components/latencybins.md)

-   [led\_dim](components/led_dim.md)

-   [lgantry](components/lgantry.md)

-   [limit1](components/limit1.md)

-   [limit2](components/limit2.md)

-   [limit3](components/limit3.md)

-   [lincurve](components/lincurve.md)

-   [lowpass](components/lowpass.md)

-   [lut5](components/lut5.md)

-   [lutn](components/lutn.md)

-   [maj3](components/maj3.md)

-   [match8](components/match8.md)

-   [minmax](components/minmax.md)

-   [mult2](components/mult2.md)

-   [multiclick](components/multiclick.md)

-   [multiswitch](components/multiswitch.md)

-   [mux16](components/mux16.md)

-   [mux2](components/mux2.md)

-   [mux4](components/mux4.md)

-   [mux8](components/mux8.md)

-   [muxn](components/muxn.md)

-   [muxn\_u32](components/muxn_u32.md)

-   [near](components/near.md)

-   [neg](components/neg.md)

-   [not](components/not.md)

-   [offset](components/offset.md)

-   [oneshot](components/oneshot.md)

-   [or2](components/or2.md)

-   [orient](components/orient.md)

-   [orn](components/orn.md)

-   [out\_to\_io](components/out_to_io.md)

-   [pid](components/pid.md)

-   [reset](components/reset.md)

-   [safety\_latch](components/safety_latch.md)

-   [sample\_hold](components/sample_hold.md)

-   [scale](components/scale.md)

-   [select8](components/select8.md)

-   [selectn](components/selectn.md)

-   [sphereprobe](components/sphereprobe.md)

-   [stats](components/stats.md)

-   [sum2](components/sum2.md)

-   [thc](components/thc.md)

-   [thcud](components/thcud.md)

-   [threadtest](components/threadtest.md)

-   [time](components/time.md)

-   [timedelay](components/timedelay.md)

-   [toggle](components/toggle.md)

-   [toggle2nist](components/toggle2nist.md)

-   [tristate\_bit](components/tristate_bit.md)

-   [tristate\_float](components/tristate_float.md)

-   [updown](components/updown.md)

-   [wcomp](components/wcomp.md)

-   [wcompn](components/wcompn.md)

-   [weighted\_sum](components/weighted_sum.md)

-   [xor2](components/xor2.md)

### This listing is manually generated when new components are added or removed

To ensure your copy repo is up to date, do frequent pulls from the machinekit-docs master repo

------------------------------------------------------------------------

Last updated 2015-12-20 22:15:56 CET


