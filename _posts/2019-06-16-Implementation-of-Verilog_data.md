---
layout:     post
title:      Implementation of Verilog_data
subtitle:   Verilog_data class
date:       2019-06-16
author:     Bowen
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
   - GSoC
   - GNU Radio
---

This is a report about the Implementation of `Verilog_data` class.

--------------------------

The `Verilog_data` class is the one of the most important classes in this project. Not only does it work only in block code, but it also performs important functions in the shared library. You could find the code [here](https://github.com/B0WEN-HU/gr-verilog/tree/structure/include/Verilog_data).

The `Verilog_data` class build and store the port information of the verilog module, and it could take the `const char *port_name` then manipulate the corresponding port of `Verilator` code. Due to lack of reflection mechanism in C++, I can only implement this myself.

There are two `STL containers` in the code of `Verilog_data`, a `map` and a `vector`. The `vector` stores the information of each port. The `map` stores the `<std::string, vec_iter>` pair, so we can find the information of a port by the name of the port as long as we initialize it in this data structure, basically handmade reflection.

``` c++
typedef typename std::vector<Port_info<VLmodule>>::iterator vec_iter;

std::map<std::string, vec_iter> port_map;

std::vector<Port_info<VLmodule>> port_list;
```

The `Verilog_data` was implemented by template, which is a tough work. Because, I should make sure it will work alright both in the block an in the shared library. The prime difference between code in block and in shared library is that, we actually do not need the template at all in block. `Verilog_data` in block should only have the information of the ports, it will not interact directly with module ports, so it will not accept a `Verilator-generated class`. We could just use a empty struct in the block.

``` c++
struct Void_module{

  // This is just a placeholder for the template
  
  // Void_module should be used in main code of the block
  
  // The real template <Vmodule> should be used in the shared library
  
};
```

Another tricky point is that, how should I access the members(ports) of a `Verilator-generated class` with different width?

I use a very complicated solution here, you could find that in the code below of `Port_info` class, we need a member pointer that could handle different width. Fortunately, there are only four kinds of data width used in `Verilator`. I use a union to store the member pointer of the `template <class VLmodule>`.

``` c++
class Port_info{

      /* ... */

      // port data

      PORT_IO_TYPE iotype;

      PORT_TYPE value;

      enum {UINT8, UINT16, UINT32, UINT64} port_width;



      union

      {

        // template <Vmodule>

        void *void_p;

        uint8_t  VLmodule::*uint8_p;

        uint16_t VLmodule::*uint16_p;

        uint32_t VLmodule::*uint32_p;

        uint64_t VLmodule::*uint64_p;

      } port_ptr;

      /* ... */
};
```

Through this way, we could handle the demand we need, get a string then interact with corresponding port of `Verilator` code.

--------------------------

There might be more features of `Verilog_data` in the future.
