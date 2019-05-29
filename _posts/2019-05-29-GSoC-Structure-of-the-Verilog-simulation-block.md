---
layout:     post
title:      Structure of the Verilog simulation block
subtitle:   Overall Code Structure
date:       2019-05-29
author:     Bowen
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
   - GSoC
   - GNU Radio
---

This is a report about the overall structure of my GSoC project, cycle-accurate Verilog simulation integration.

--------------------------

Though some of the details have been discussed in my proposal, I have some new insights on this task.

I create a block named `verilog_ii`, this is a `sync block` that takes in unsigned integer and output unsigned integer.

I plan to do most of the work in the block's constructor. There are several class I will create and include in the block, namely `Shell_cmd`(the class that call bash command), `Shared_lib`(the class that handle the shred library), `Verilog_data`(the class that store the port map and other input/ouput data), etc.

It will be better to demonstrate this by codes directly. So I will show some of the codes here. You can find the repository [here](https://github.com/B0WEN-HU/gr-verilog/tree/structure "gr-verilog structure")


The `verilog_ii_impl.h` and `verilog_ii_impl.cc` are as follow. You can find it [here](https://github.com/B0WEN-HU/gr-verilog/blob/structure/lib/verilog_ii_impl.h "verilog_ii_impl.h") and [here](https://github.com/B0WEN-HU/gr-verilog/blob/structure/lib/verilog_ii_impl.cc "verilog_ii_impl.cc").

`verilog_ii_impl.h`:
``` c
#ifndef INCLUDED_VERILOG_VERILOG_II_IMPL_H
#define INCLUDED_VERILOG_VERILOG_II_IMPL_H

#include <verilog/verilog_ii.h>
#include <string>

#define SLASH "/"

namespace gr {
  namespace verilog {

    typedef unsigned int ITYPE;
    typedef unsigned int OTYPE;

    class verilog_ii_impl : public verilog_ii
    {
     private:
      /* gr::verilog::verilog_ii private member variables  */
      
      // The path and name of user's verilog module
      // Construct by (const char *filename)
      std::string verilog_module_path;
      std::string verilog_module_name;
      
      // The path of makefile template
      std::string makefile_template_path;

      // The path of cpp template 
      std::string cpp_template_path;

      // The class that call the shell command make
      // Use sh_make.cmd(std::string cmd) to call bash
      // Shell_cmd sh_make;

      // The class that store the verilog module data
      // Including port map and input/output data
      // Use verilog_data.set_input( (ITYPE) in[i] ) to set input data
      // Usr verilog_data.set_output( (OTYPE) out[i] ) to set output data
      Verilog_data verilog_data;

      // The class that control the shared library
      // Use verilog_module_so.load_lib(std::string verilog_module_path, std::string lib_name) to load library
      // Use verilog_module_so.find_func(std::string func_name) to get the function
      // Use verilog_module_so.release_lib(std::string string lib_name) to release the library
      Shared_lib verilog_module_so;

      // typedef void (*Simulation_func)(const verilog_data.get_input_addr(), verilog_data.get_output_addr());
      Simulation_func sim;
      
      /* gr::verilog::verilog_ii private member variables  */



      /* gr::verilog::verilog_ii private member functions */

      /* Construct routine */
      // The function that call Verilator (Makefile) to generate the cpp code
      bool generate_verilator_file() throw(std::runtime_error);

      // Parse the Verilator generate file and extract the port map
      // The port map should be stored in Veriloag_data verilog_data
      bool init_port_map() throw(std::logic_error);

      // The function that call g++ (Makefile) to generate the shared library
      // There might be some modifications on the tempalte cpp interface file
      bool generate_so() throw(std::runtime_error);

      // The function that load the shared library that generated above
      // with the Shared_lib verilog_module_so
      bool load_lib() throw(std::runtime_error);
      /* Construct routine */

      /* gr::verilog::verilog_ii private member functions */

     public:
      verilog_ii_impl(const char *filename);
      ~verilog_ii_impl();

      // Where all the action really happens
      int work(int noutput_items,
         gr_vector_const_void_star &input_items,
         gr_vector_void_star &output_items);
    };

  } // namespace verilog
} // namespace gr

#endif /* INCLUDED_VERILOG_VERILOG_II_IMPL_H */
```

`verilog_ii_impl.cc`:
``` c
#ifdef HAVE_CONFIG_H
#include "config.h"
#endif

#include <gnuradio/io_signature.h>
#include "verilog_ii_impl.h"

namespace gr {
  namespace verilog {

    verilog_ii::sptr
    verilog_ii::make(const char *filename)
    {
      return gnuradio::get_initial_sptr
        (new verilog_ii_impl(filename));
    }

    /*
     * The private constructor
     */
    verilog_ii_impl::verilog_ii_impl(const char *filename)
      : gr::sync_block("verilog_ii",
              gr::io_signature::make(1, 1, sizeof(ITYPE)),
              gr::io_signature::make(1, 1, sizeof(OTYPE)))
    {
      /* Get module_name and module_path */
      std::string filename_temp(filename);
      std::size_t filename_pos = filename_temp.rfind(SLASH);
      if (std::string::npos == filename_pos) {
        /* TODO: raise_error() */
      }
      this->verilog_module_name = filename_temp.substr(filename_pos + 1);
      this->verilog_module_path = filename_temp.substr(0, filename_pos + 1);

      /* Initialize makefile_template_path and cpp_template_path */
      // TODO: #define MAKEFILE_TEMPLATE_PATH "$(prefix)/gnuradio/gr-verilog/lib"
      this->makefile_template_path = MAKEFILE_TEMPLATE_PATH;
      // TODO: #define CPP_TEMPLATE_PATH "$(prefix)/gnuradio/gr-verilog/lib"
      this->cpp_template_path = CPP_TEMPLATE_PATH;

      /* Call Verilator (Makefile) to generate the cpp code */
      // There will be a Shell_cmd object created in the function to
      // run configure.sh
      // configure.sh will copy the makefile template and modify it
      // for the verilog module
      // the Shell_cmd will run make at the verilog module path
      try {
        this->generate_verilator_file();
      } catch (...) {
        /* TODO: Handle the error */
      }
      

      /* Initialize port_map that stored in verilator_data */
      // The port map is the structure of input/output ports of
      // verilog module
      // Verilog_data should have a method to parse the code(.cpp/.v) and
      // extract the port structure of verilog module.
      try {
        this->init_port_map();
      } catch (...) {
        /* TODO: Handle the error */
      }
      

      /* Generate shared library and initialize verilog_module_so */
      // There will be a Shell_cmd object created in the function to
      // run make at the verilog module path
      try {
        this->generate_so();
      } catch (...) {
        /* TODO: Handle the error */
      }
      
      /* Load the shared library that generated above */
      // The function should also initialize the external veriable
      // in the shred library
      // Call verilog_module_so.find_func(VERILOG_INIT_FUNC)
      // Call verilog_module_so.find_func(VERILOG_RESET_FUNC)
      // shared library function reset() should be defalt generated
      // There would be a function called general_sim()
      // general_sim() could accept input and output of all port with
      // the help of port map stored in the verilog_data
      try {
        this->load_lib();
      } catch (...) {
        /* TODO: Handle the error */
      }

      /* Initialize sim */
      this->sim = NULL;
    }

    /*
     * Our virtual destructor.
     */
    verilog_ii_impl::~verilog_ii_impl()
    {
    }

    int
    verilog_ii_impl::work(int noutput_items,
        gr_vector_const_void_star &input_items,
        gr_vector_void_star &output_items)
    {
      const ITYPE *in = (const ITYPE *) input_items[0];
      OTYPE *out = (OTYPE *) output_items[0];
      if (NULL == this->sim)
      {
        // TODO: #define VERILOG_SIMULATION_FUNC "$(the_name_of_the_function)"
        try {
          // Find the function of simulation in the shared library
          sim = verilog_module_so.find_func(VERILOG_SIMULATION_FUNC);
        } catch (...) {
          /* TODO: Handle the error */
        }
      }

      // Do <+signal processing+>
      for (unsigned int i = 0; i < noutput_items; i++)
      {
        this->verilog_data.set_input(in[i]);
        this->verilog_data.set_output(out[i]);
        this->verilog_data.convert();
        sim(verilog_data.get_input_addr(), verilog_data.get_output_addr());
      }

      // Tell runtime system how many output items we produced.
      return noutput_items;
    }

  } /* namespace verilog */
} /* namespace gr */
```

--------------------------

There still left the some issues, I will cover them in the weekly report this weekend.
