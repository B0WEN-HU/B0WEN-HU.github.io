---
layout:     post
title:      GSoC19 Phase 2 Week 4
subtitle:   Weekly Report
date:       2019-07-28
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
I finished the general case of AXI-stream, using general block. You can find it [here](https://github.com/B0WEN-HU/gr-verilog/tree/axi-general) in a new branch named `axi-general`. In this block, there is no such assumption that the verilog module should be sync. But the `forecast` of the block is just as simple as follows. Should it be more sophisticated?

```c++
void
verilog_axi_ii_impl::forecast (int noutput_items, gr_vector_int &ninput_items_required)
{
   ninput_items_required[0] = noutput_items;
}
```

The template of the block was updated in this week, I add two functions, `AXI_1_transfer` and `AXI_transfer_out`, which could be used in this general block. You can find the code [here](https://github.com/B0WEN-HU/gr-verilog/blob/axi-general/templates/axi_module.cpp).

I have not fully tested this new block yet. I only passed the same small test case last week. Hopefully, I will be able to conduct a more complete test next week.

If you want to try this new block, you could just clone this branch, and build it yourself for the test. but be sure to change the template path in the `./lib/constants.cc.in` to your path of `./templates`. (Although, this is might not be a good way to do so.)

## Plan next week
Test the new `axi-general` block more fully.

## Issue(s)
I am not sure how to appropiately implement `forecast` of the block, with so little knowledge of user verilog module.

--------------------------

See you next week.