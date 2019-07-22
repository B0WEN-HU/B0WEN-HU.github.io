---
layout:     post
title:      GSoC19 Phase 2 Week 3
subtitle:   Weekly Report
date:       2019-07-21
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
I moved the `verilog_axi_ii` block to a new branch `axi`, you can find it [here](https://github.com/B0WEN-HU/gr-verilog/tree/axi).

With the help of Marcus, I add some extra code to make sure the block could find the path of templates. In the new branch `axi`, you can find the [templates](https://github.com/B0WEN-HU/gr-verilog/tree/axi/templates) in the `./templates` folder.

I modified some of the code of cpp template `axi_module.cpp`, instead of just offer a single `void AXI_transfer(const unsigned int &gr_input, unsigned int &gr_output, unsigned int &time)`, I think there should be more kinds of interactives between the block and the verilog module, so I offer more function in this template.

`verilog_axi_ii` assumes the input and output should be synchronized, which may not be the real case in the AXI-stream interface. I am working on the general case of AXI-stream situation.

I am working on some more test cases as well, including `doubler.v`, which generates the x2 of input, and a `FIFO_sync.v`

## Plan next week
Implement the general case of AXI-stream, using general block.

## Issue(s)
I am not sure how to implement `forecast` of the block, with so little knowledge of user verilog module.

--------------------------

See you next week.