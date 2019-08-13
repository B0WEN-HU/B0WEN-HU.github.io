---
layout:     post
title:      GSoC19 Phase 3 Week 2
subtitle:   Weekly Report
date:       2019-08-12
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
This week, I mainly did QA and fix bugs.

I changed some names of the function in the cpp template. It will not affect the usage of the block.

I also add another feature, which is skip output, you can now change the macro in the template to skip first n outputs of verilog module. I will add another parameter of the block, to make this feature be accessed easier.

I am considering add several new blocks, namely gr_verilog_axi_cc and gr_verilog_axi_ff.

I am working on a gr_verilog_general_ii, this will not be a block ready to use out of the box, you have to write your template yourself. But this block would give the user maximum freedom decide how to use the block and implement the function that they want. But, frankly speaking, this block is just the structure of the gr_verilog_axi_ii with more generalization.

## Plan next week
More tests of the block.

Add some new parameters of the block.

Add gr_verilog_axi_cc and gr_verilog_axi_ff.

Build gr_verilog_general_ii.

## Issue(s)

--------------------------

See you next week.