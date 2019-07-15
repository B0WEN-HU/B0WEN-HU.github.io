---
layout:     post
title:      GSoC19 Phase 2 Week 2
subtitle:   Weekly Report
date:       2019-07-14
author:     Bowen
header-img: img/post-bg-map.jpg
catalog: true
tags:
   - GSoC
   - GNU Radio
---

Hi, this is my weekly report this week.

--------------------------

## Progress this week
I am keeping working on the `verilog_axi_ii` block this week.

I successfully built and tested the block. There are still some unfinished parts, but the major function has been constructed. 

You can find the repository [here](https://github.com/B0WEN-HU/gr-verilog/tree/structure) (It has a little bit mess due to some unfinished codes.)

You should keep your Verilog module's interface as below, to work with this block.

``` c++
/*

 * AXI signals:

 * ACLK               input

 * ARESETn            input
 
 * TVALID_IN          input
 
 * TREADY_IN          output
 
 * TVALID_OUT         output
 
 * TREADY_OUT         input
 
 * TDATA_IN[31:0]     input
 
 * TDATA_OUT[31:0]    output
 
 */
```

If you want to build the block by yourself, you may need to change some micros in the `verilog_axi_ii_impl.cc`. You need to change to the path where you keep the source files. Frankly speaking, it is inconvenient, I will find a better way in the coming week.

``` c++
#define GNU_RADIO_PREFIX "/home/bowen/Documents/GSoC/gr-verilog/"
```

## Plan next week
Finish the error-handle part of the code, and solve the path problem.

## Issue(s)
The path problem

--------------------------

See you next week.
