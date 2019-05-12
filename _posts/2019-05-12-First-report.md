---
layout:     post
title:      First Report
subtitle:   Hello World, Hello Blog
date:       2019-05-12
author:     Bowen
header-img: img/post-bg-map.jpg
catalog: true
tags:
    - GSoC, GNU Radio
---

Hi, This is my first report since GSoC started.
--------------------------
## Progress this week
I have set up the working environment and read some tutorials for GNU Radio development in the last few days.

My current working environment is `Ubuntu 18.04 LTS` installed `GNU Radio` `version 3.7.11` by `apt`, but I am considering switch to newest `Ubuntu 19.04` which shipped with `GNU Radio` `version 3.7.13` by `apt`.

I am following the tutorial and created [my first module](https://github.com/B0WEN-HU/gr-howto "gr-howto") `gr-howto` (This may not be a good name, so I will change the module name in the coming week). I will continue to read tutorials, hopefully I could finish them (from chapter 2 to chapter 7) in the next week.

I also managed to run the simulation of a simple Verilog module with Verilator, it works as aticipated, you can find it [here](https://github.com/B0WEN-HU/Verilator_SIM_standalone "Verilator_SIM_standalone"). This is just a standalone simulation program, in order to let the `GNU Radio` work with the Verilator-generated file, there are much work to do.

## Plan next week
Finish all the [tutorials](https://tutorials.gnuradio.org "tutorials") from chapter 2 to chapter 7.

Try to make the Verilator related header file available in GNU Radio blocks. A source block like [this (flicker.v)](https://github.com/B0WEN-HU/Verilator_SIM_standalone "Verilator_SIM_standalone") would be a good start.

## Issue(s)
Change the name of the module from `gr-howto` to `gr-myfistmodule`.
--------------------------

See you next week.