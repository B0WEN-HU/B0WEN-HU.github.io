---
layout:     post
title:      GSoC weekly report #2
subtitle:   Weekly Report
date:       2019-05-19
author:     Bowen
header-img: img/post-bg-map.jpg
catalog: true
tags:
   - GSoC
   - GNU Radio
---

Hi, This is my weekly report this week.

--------------------------

## Progress this week
I have finished the [tutorials](https://tutorials.gnuradio.org "tutorials") from chapter 2 to chapter 6 this week.

I changed the module name `gr-howto` to [`gr-mytutorial`](https://github.com/B0WEN-HU/gr-mytutorial).

## Plan next week
Finish the [tutorials](https://tutorials.gnuradio.org "tutorials") chapter 7 left in the next week.

Try to make the Verilator related header file available in GNU Radio blocks.

## Issue(s)
Verilator will compile Verilog file into C++, we could run simulation with the class it offers. But we do not know the member names(ports of Verilog module) in that class unless we have some way to parse either Verilator-generated C++ file or Verilog source file (Verilator do offer a XML file output which contains the class name, but this file changes with version of Verilator according to the author.). Due to the lack of reflection mechanism in C++, I may have to write the class names right in to code of simulation.

I need to extract the ports name which is `clk` `rst_n` `dout` in [flicker.v](https://github.com/B0WEN-HU/Verilator_SIM_standalone "Verilator_SIM_standalone"). I am not sure, would it be appropriate if I just use regular expression to read the C++ header file shown below.

The Verilog source file is like:
``` verilog
module flicker(clk, rst_n, dout);
parameter WIDTH = 5;
input clk, rst_n;
output reg [WIDTH-1:0] dout;
//...
```
The Verilator generated C++ header file:
``` C++
VL_MODULE(Vflicker) {
  public:
    
    // PORTS
    // The application code writes and reads these signals to
    // propagate new values into/out from the Verilated model.
    VL_IN8(clk,0,0);
    VL_IN8(rst_n,0,0);
    VL_OUT8(dout,4,0);

    //...
```

--------------------------

See you next week.
