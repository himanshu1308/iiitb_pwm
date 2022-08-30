# PWM(Pulse width modulation) Generator
This Project is about design and simulation of PWM generator with variable duty cycle.
# Introduction
By dividing an electrical signal into discrete pieces, pulse-width modulation (PWM) or pulse-duration modulation (PDM) is a technique for lowering the average power produced by an electrical signal. By rapidly flipping the switch between the supply and the load on and off, the average amount of voltage (and current) provided to the load is managed. The total power provided to the load increases while the switch is on for a longer period of time compared to when it is off. Depending on the load and application, the power supply's switching rate (or frequency) might change significantly.
<h3>Duty Cycle </h3>
A low duty cycle equates to low power because the electricity is off for the most of the time; the word duty cycle reflects the ratio of "on" time to the regular interval or "period" of time. Duty cycle is measured in percentages, with 100 percent representing total on. A digital signal has a duty cycle of 50% and looks like a "square" wave when it is on for 50% of the time and off for the other 50%. A digital signal has a duty cycle of >50% when it spends more time in the on state than the off state. A digital signal has a duty cycle of 50% when it spends 50% more time in the off state than the on state.

![a](https://user-images.githubusercontent.com/44607144/183282794-9bc2fee1-2038-48eb-874a-a50057372058.png)

```Duty cycle = Ton/T```
# Application
* Servos
* Telecommunications
* Power Delievery
* Voltage regulation
* Audio effects and amplification
* Soft blinking LED indicator
# Block Diagram
This PWM generator generates 10Mhz signal. We can control duty cycles in steps of 10%. The default duty cycle is 50%. Along with clock signal we provide another two external signals to increase and decrease the duty cycle.

![imgonline-com-ua-ReplaceColor-6REIi8mgUY1QWcM6](https://user-images.githubusercontent.com/44607144/183286473-e6c86f38-18d9-43a0-b5cf-532212e1797a.jpg)



The key components we need for this particular circuit are an n-bit counter and comparator. The duty passed to the comparator is compared to the counter's current value. If the counter's current value is less than the required value, the comparator produces a high output. Similar to this, if the counter's current value is higher than its duty, the comparator's output will be low. Since the counter starts at zero, the comparator initially produces a large output, which decreases as the counter approaches its duty. Therefore, we can manage duty cycle by managing duty.


![imgonline-com-ua-ReplaceColor-wXK8pJ09VILEr](https://user-images.githubusercontent.com/44607144/183286538-7cf5b813-cd0d-4a8c-8ea9-8d2a3c393f44.jpg)


Since the counter is sequential and the comparator is a combinational circuit, there may be an intermediate state like 111 that is higher or lower than duty when counting from 011 to 100 as a result of inappropriate delays. A glitch might result from this. The output of the comparator is routed through a D flipflop in order to prevent these errors.

# Iverilog
Icarus Verilog is a Verilog simulation and synthesis tool. It operates as a compiler, compiling source code written in Verilog (IEEE-1364) into some target format. For batch simulation, the compiler can generate an intermediate form called vvp assembly. This intermediate form is executed by the ``vvp'' command. For synthesis, the compiler generates netlists in the desired format.
# GTK Wave
GTKWave is a fully featured GTK+ based wave viewer for Unix and Win32 which reads LXT, LXT2, VZT, FST, and GHW files as well as standard Verilog VCD/EVCD files and allows their viewing.
<h3> Installing Iverilog and GTKWave</h3>
open terminal and type following commands

* sudo apt-get update from terminal.
* sudo apt-get install iverilog.
* sudo apt-get install gtkwave.

# Functional Characterstics


![Screenshot from 2022-08-08 19-55-35](https://user-images.githubusercontent.com/44607144/183441376-346143ff-c51a-460c-9918-6df338a2922b.png)


# Synthesis Of Verilog Code
<h3> About Yosys </h3>
This is a framework for RTL synthesis tools. It currently has extensive Verilog-2005 support and provides a basic set of synthesis algorithms for various application domains.

Yosys can be adapted to perform any synthesis job by combining the existing passes (algorithms) using synthesis scripts and adding additional passes as needed by extending the yosys C++ code base.

Yosys is free software licensed under the ISC license (a GPL compatible license that is similar in terms to the MIT license or the 2-clause BSD license).
More information and documentation can be found on the Yosys web site:
* https://yosyshq.net/yosys/

* To install yosys, You can follow this link https://github.com/YosysHQ/yosys
<h3> Synthesis </h3>
The straightforward RTL design is converted via synthesis into a gate-level netlist with all the limitations provided by the designer. Synthesis is a technique that, in the simplest words possible, turns an abstract design into a correctly implemented device in terms of logic gates.

<h3>Synthesizer</h3>
It is a tool that helps designer in synthesis. it first converts RTL into simple logic gates, mapping those gates to logic gates available in technology library and finally optimise the the mapped netlist keeping the constraints set by designer intact.

<h3>  To Synthesize</h3>

```
$   yosys
$   yosys>    script yosys_run.sh
```
# Gate Level Simulation
By running the test bench with the netlist file produced by the synthesis as the design under test, GLS generates the simulation results. Since Netlist and RTL code are logically equivalent, the same test bench can be utilised. This is done to check the design's logical accuracy once it has been synthesised. Making sure the design's timing is satisfied.

Folllowing are the commands to run the GLS simulation:
```
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 primitives.v sky130_fd_sc_hd.v iiitb_pwm_gen_synth.v iiitb_pwm_gen_tb.v
./a.out
gtkwave pwm.vcd
```
I have primitives.v and sky123_fd_sc_hd.v in same folder with design files, if not so than kindly provide whole path in the 1st line of commands above.

The output waveform for the RTL design file should match the gtkwave output for the netlist. We can use the same testbench and compare the waveforms because the netlist and design code have the same set of inputs and outputs.

#### GLS waveform
![Screenshot from 2022-08-18 14-29-57](https://user-images.githubusercontent.com/44607144/185355510-0035c2d2-235d-43be-8009-f043619074ef.png)

We can observe that output of functional simulation is matching with the output of gate level simulation, this means that the netlist generated by yosys is correct.

# Physical Design

## Overview of Physical Design Flow

Place and Route (PnR), the central component of any ASIC implementation, is carried out by a number of important open source tools that are integrated into Openlane flow. The phases and associated tools that Openlane calls for the above-described features are listed below:

* #### Synthesis 
  - Generating gate-level netlist ([yosys](https://github.com/YosysHQ/yosys)).
  - Performing cell mapping ([abc](https://github.com/YosysHQ/yosys)).
  - Performing pre-layout STA ([OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA)).
* #### Floorplanning
  - Defining the core area for the macro as well as the cell sites and the tracks ([init_fp](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/init_fp)).
  - Placing the macro input and output ports ([ioplacer](https://github.com/The-OpenROAD-Project/ioPlacer/)).
  - Generating the power distribution network ([pdn](https://github.com/The-OpenROAD-Project/pdn/)).
* #### Placement
  - Performing global placement ([RePLace](https://github.com/The-OpenROAD-Project/RePlAce)).
  - Perfroming detailed placement to legalize the globally placed components ([OpenDP](https://github.com/The-OpenROAD-Project/OpenDP)).
* #### Clock Tree Synthesis (CTS)
  - Synthesizing the clock tree ([TritonCTS](https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/TritonCTS)).
* #### Routing
  - Performing global routing to generate a guide file for the detailed router ([FastRoute](https://github.com/The-OpenROAD-Project/FastRoute/tree/openroad)).
  - Performing detailed routing ([TritonRoute](https://github.com/The-OpenROAD-Project/TritonRoute))
* #### GDSII Generation
  - Streaming out the final GDSII layout file from the routed def ([Magic](https://github.com/RTimothyEdwards/magic)).
## OpenLane

OpenLANE is an opensource tool or flow used for opensource tape-outs. The OpenLANE flow comprises a variety of tools such as Yosys, ABC, OpenSTA, Fault, OpenROAD app, Netgen and Magic which are used to harden chips and macros, i.e. generate final GDSII from the design RTL. The primary goal of OpenLANE is to produce clean GDSII with no human intervention. OpenLANE has been tuned to function for the Google-Skywater130 Opensource Process Design Kit.

![OLF](https://user-images.githubusercontent.com/44607144/187524153-774861d2-20f5-459a-a1e5-cac6c19926b0.png)
More about Openlane at : https://github.com/The-OpenROAD-Project/OpenLane

### Instalaation Instructions 
```
apt install -y build-essential python3 python3-venv python3-pip
```



# Contributors
* **Himanshu Kumar Rai**
* **Kunal Ghosh**
# Acknowledgments
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd.
# Contact Information
* Himanshu Kumar Rai, MS, International Institute of Information Technology, Bangalore himanshukumar.rai@iiitb.ac.in
* Kunal Ghosh, Director, VSD Corp. Pvt. Ltd. kunalghosh@gmail.com

# Refrences:
* FPGA4Student https://www.fpga4student.com/2017/08/verilog-code-for-pwm-generator.html
* https://en.wikipedia.org/wiki/Pulse-width_modulation













