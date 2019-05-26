---
layout:     post
title:      GSoC weekly report 3
subtitle:   Weekly Report
date:       2019-05-26
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
I have finished the tutorials from chapter 2 to chapter 7 this week.

I have successfully made [`flicker.v`](https://github.com/B0WEN-HU/Verilator_SIM_standalone "Verilator_SIM_standalone") a `source block` in GNU Radio, and it passed the python QA code. You can find it [here](https://github.com/B0WEN-HU/gr-mytutorial "gr-mytutorial").

This `source block` was implemented by shared library (dynamic-link library) mechanism. This shared library was generated manually, and loaded in the GNU Radio block at runtime. I will explain details later.

As you can know from [`flicker.v`](https://github.com/B0WEN-HU/Verilator_SIM_standalone "Verilator_SIM_standalone"), this verilog module's output would switch between `0` and `31`. The QA code of this module is as follow. It stops the code with `head` block.

```python
    def test_001_t (self):
        # set up fg
        item_number = 10

        expected_result = (31, 0, 31, 0, 31, 0, 31, 0, 31, 0)
        
        vlmodule = mytutorial.Vflicker_sim_dl()
        relay = blocks.head(gr.sizeof_char*1, item_number)
        dst = blocks.vector_sink_b()

        self.tb.connect(vlmodule, relay)
        self.tb.connect(relay, dst)

        self.tb.run ()
        # check data
        result_data = dst.data()
        self.assertTupleEqual(expected_result, result_data)
        self.assertEqual(len(expected_result),len(result_data))
```

The shared library I used in this block was named as `libvl.so`, the name might be changed later. The ABI(Application Binary Interface) of this shared library is as follow. There must be a lot of modifications of ABI in the future work.

```c++
#pragma once

# include "Vflicker.h"
# include "verilated.h"

#ifdef __cplusplus
extern "C"
{
#endif

static Vflicker* tb;
static unsigned int cycles = 0;

void init();

void reset();

unsigned char tic();

void release();

#ifdef __cplusplus
}
#endif
```

The constructor of this `source block` was shown below. It demonstrates how the shared library was loaded, as well as how the verilog module was initialized and reset.

```c++
    /*
     * The private constructor
     */
    Vflicker_sim_dl_impl::Vflicker_sim_dl_impl()
      : gr::sync_block("Vflicker_sim_dl",
              gr::io_signature::make(0, 0, 0),
              gr::io_signature::make(1, 1, sizeof(char))),
        pHandle(NULL)
    {

      // Load the library
      this->pHandle = dlopen("/home/bowen/Documents/GR_learn/gr-mytutorial/lib/libvl.so", RTLD_LAZY);
      if(NULL == this->pHandle) {
        std::cerr << "ERROR:Cannot load the library" << std::endl;
        //return EXIT_FAILURE
      
      }

      // Find the library symbol
      void (*pInit)();
      pInit = (void (*)()) dlsym(this->pHandle, "init");
      if(NULL == pInit) {
        std::cerr << "ERROR:Cannot find the function" << std::endl;
        dlclose(this->pHandle);
        this->pHandle = NULL;
        //return EXIT_FAILURE;
      }

      // Execute init()
      
      try {
        pInit();
      } catch(...) {
        // Handle the exception
        std::cerr << "ERROR:Exception raised by function";
        //return EXIT_FAILURE;
      }

      // Find the library symbol
      
      void (*pReset)();
      pReset = (void (*)()) dlsym(this->pHandle, "reset");
      if(NULL == pReset) {
        std::cerr << "ERROR:Cannot find the function" << std::endl;
        dlclose(this->pHandle);
        this->pHandle = NULL;
        //return EXIT_FAILURE;
      }

      // Execute reset()
      
      try {
        pReset();
      } catch(...) {
        // Handle the exception
        std::cerr << "ERROR:Exception raised by function";
        //return EXIT_FAILURE;
      }

    }
```

## Plan next week
I have created a new repository named [`gr-verilog`](https://github.com/B0WEN-HU/gr-verilog "gr-verilog") on GitHub. This repository would server as the main development repository during the coding period of GSoC19. 

I will work on the overall structure of the `gr-verilog` module in the coming week. I will work on the `structure` branch of `gr-verilog`.

I will post another report on the overall structure details of `gr-verilog` module, in the next few days.

## Issue(s)
After three weeks, I will have my final exam. I may need to take some time to review for the exam, but I think I can still catch up. I will do extra work during the weekend.

--------------------------

See you next week.
