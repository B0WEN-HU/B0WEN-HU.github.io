---
layout:     post
title:      Implementation of Shell_cmd Shared_lib and Verilog_data
subtitle:   Weekly Report
date:       2019-06-09
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
I have completed and test the code of `Shell_cmd` and `Shared_lib` this week, and I construct most part of the code of `Verilog_data` but have not test it yet. You can find them [here](https://github.com/B0WEN-HU/gr-verilog/tree/structure/include) in their folders.

Since this week I was moving my development environment from `Ubuntu 18.04 LTS` to `Ubuntu 19.04`, I did most of the code on my laptop, so I just push them onto GitHub yesterday. `Shell_cmd` `Shared_lib` and `Verilog_data` are relatively independent parts, so it did not cause too many troubles.

`Verilog_data` is significantly complicated than `Shell_cmd` or `Shared_lib`, so I would like to post an extra report to explain how this class was implemented, in a few days. I am not sure whether this is the appropriate way to implement the class, but this is the best method that I can come up with.

## Plan next week
I will post a report to explain how `Verilog_data` was implemented, and some details about how the shared library would work with `Verilog_data` in my vision.

## Issue(s)
I leave the `Verilog_data::parse_port` unfinished, this would originally be the work I planed on the timeline next week, but I will have my final exam in the next week. I will still try my best not to left behind the schedule though.

The `Verilog_data::parse_port` will be a tough problem I suppose, I have mentioned it sevral times, I plan to parse the port information from the Verilator-generated cpp file rather than the original verilog file. I do not know whether it is a good decision, so I leave the flexibility for future modifications.

--------------------------

See you next week.