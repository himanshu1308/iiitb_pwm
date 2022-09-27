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
After running
```stat```
it gives stat as shown below:

![stat](https://user-images.githubusercontent.com/44607144/187744985-725850f4-6366-489e-ba71-68bb38aeaa5a.png)


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
#### Python Installation
```
apt install -y build-essential python3 python3-venv python3-pip
```
#### Docker Installation
```
$ sudo apt-get remove docker docker-engine docker.io containerd runc (removes older version of docker if installed)

$ sudo apt-get update

$ sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
$ sudo mkdir -p /etc/apt/keyrings

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

$ echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
$ sudo apt-get update

$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

$ apt-cache madison docker-ce (copy the version string you want to install)

$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin (paste the version string copies in place of <VERSION_STRING>)

$ sudo docker run hello-world (If the docker is successfully installed u will get a success message here)
```
for more about docker installation you can check the following link: https://docs.docker.com/engine/install/ubuntu/
After installing Docker we are ready for OpenLane installation.

#### OpenLane Installation

first go to home directory->
```

$ git clone https://github.com/The-OpenROAD-Project/OpenLane.git

$ cd OpenLane/

$ make

$ make test
```
If it ended with saying Basic test passed then open lane installed succesfully.

## Magic

The venerable VLSI layout tool Magic was created in the 1980s at Berkeley by John Ousterhout, who is now best known for creating the scripting language Tcl. Magic has maintained its popularity among colleges and small businesses in great part because of its permissive Berkeley open-source licence. The open-source licence has made it possible for VLSI experts with a programming inclination to put innovative ideas into practise and support magic's further advancement in fabrication technology. The well-designed basic algorithms, however, are what give magic the most of its notoriety. Even those who finally rely on commercial tools for their product design flow frequently identify magic as the most user-friendly tool for circuit layout.
You can find More about Magic at: http://opencircuitdesign.com/magic/index.html

### Installation 
Before installation Magic install the following softwares first:
```
$   sudo apt-get install m4
$   sudo apt-get install tcsh
$   sudo apt-get install csh
$   sudo apt-get install libx11-dev
$   sudo apt-get install tcl-dev tk-dev
$   sudo apt-get install libcairo2-dev
$   sudo apt-get install mesa-common-dev libglu1-mesa-dev
$   sudo apt-get install libncurses-dev
```
After running all the above commands we will finally proceed for magic installation.

#### Magic Installation

First go to home directory and open terminal and write the following commands-> 
```
$   git clone https://github.com/RTimothyEdwards/magic
$   cd magic/
$   ./configure
$   sudo make
$   sudo make install
```
After installation type *magic* in terminal a magic window will pop up, which means magic is installed properly.


![magic_inst](https://user-images.githubusercontent.com/44607144/187528985-31be0280-1cf8-45d6-be31-c650d4c85377.png)

## Klayout

KLayout is a GDS and OASIS file viewer.KLayout is published under GNU public license GPL version 2 or any later version (www.gnu.org) in compliance with the requirements for using the Qt open source licence.

#### Installation 
```
$ sudo apt-get install klayout
```
To open just type Klayout in terminal, a window will appear as shown below.

![klayou](https://user-images.githubusercontent.com/44607144/187532283-6520bd75-87a3-4ea2-b619-6739544743bb.png)


## NgSpice

ngspice is the open source spice simulator for electric and electronic circuits. ngspice offers a wealth of device models for active, passive, analog, and digital elements. Model parameters are provided by our collections, by the semiconductor device manufacturers, or from semiconductor foundries. The user adds her circuits as a netlist, and the output is one or more graphs of currents, voltages and other electrical quantities or is saved in a data file. It  does not provide schematic entry. Its input is command line or file based. There are however third party interfaces available.

#### Installation
```
$ sudo apt-get install ngspice
```
# Creating a custom Inverter Cell

open the openlane folder in terminal, and run the following commands:
```
$ git clone https://github.com/nickson-jose/vsdstdcelldesign.git

$ cd vsdstdcelldesign

$  cp ./libs/sky130A.tech sky130A.tech

$ magic -![cell_design](https://user-images.githubusercontent.com/44607144/187679016-4073b574-d562-4b93-bfaa-a4b057ebaa6d.png)
T sky130A.tech sky130_inv.mag &
```

![cell_design](https://user-images.githubusercontent.com/44607144/187680892-324061b8-e310-4bee-9618-bb334427096e.png)


In Sky130 the first layer is called the local interconnect layer or Locali.

To verify whether the layout is that of CMOS inverter, verification of P-diffusiona nd N-diffusion regions with Polysilicon can be observed.

Other verification steps are to check drain and source connections. The drains of both PMOS and NMOS must be connected to output port (here, Y) and the sources of both must be connected to power supply VDD (here, VPWR).
*LEF or library exchange format:* A format that tells us about cell boundaries, VDD and GND lines. It contains no info about the logic of circuit and is also used to protect the IP.
*SPICE extraction:* Within the Magic environment, following commands are used in tkcon to achieve .mag to .spice extraction:
```
extract all
ext2spice cthresh 0 rethresh 0
ext2spice
```
![magic1](https://user-images.githubusercontent.com/44607144/187679209-07e76256-9a77-44ed-87fe-0edcd0edc664.png)

This generates the sky130_in.spice file. This SPICE deck is edited to include pshort.lib and nshort.lib which are the PMOS and NMOS libraries respectively. In addition, the minimum grid size of inverter is measured from the magic layout and incorporated into the deck as: .option scale=0.01u. The model names in the MOSFET definitions are changed to pshort.model.0 and nshort.model.0 respectively for PMOS and NMOS.

Finally voltage sources and simulation commands are defined as follows:
```
VDD VPWR 0 3.3V
VSS VGND 0 0
Va A VGND PUSLE(0V 3.3V 0 0.1ns 0.1 ns 2ns 4ns)
.tran 1n 20n
.control
run 
.endc
.end
```
The final sky130_inv.spice file is modified to:

![spice file](https://user-images.githubusercontent.com/44607144/187679532-0eb3552a-aa26-4cf6-a8e1-7b5dcb47d766.png)

For simulation, ngspice is invoked in the terminal:
```
ngspice sky130_inv.spice
```
The output "y" is to be plotted with "time" and swept over the input "a":

```
plot y vs time a
```


![ng_spice](https://user-images.githubusercontent.com/44607144/187679809-52e78e3b-5f1f-4b0b-be3c-37faed631552.png)

The waveform obtained is shown below:


![ng_plot](https://user-images.githubusercontent.com/44607144/187679907-8a7c6278-e2b1-4f7b-af6c-6d2c9217e52a.png)

Four timing parameters are used to characterize the inverter standard cell:

1. Rise transition: Time taken for the output to rise from 20% of max value to 80% of max value
2. Fall transition- Time taken for the output to fall from 80% of max value to 20% of max value
3. Cell rise delay = time(50% output rise) - time(50% input fall)
4. Cell fall delay  = time(50% output fall) - time(50% input rise)

The above timing parameters can be computed by noting down various values from the ngspice waveform.

```Rise transition = (2.23843 - 2.17935) = 59.08ps```

```Fall transition = (4.09291 - 4.05004) = 42.87ps```

```Cell rise delay = (2.20636 - 2.15) = 56.36ps```

```Cell fall delay = (4.07479 - 4.05) = 24.79ps```

## Creating Port definition for custom inverter cell
```
$ magic -T sky130A.tech sky130_vsdinv.mag
```
In Magic Layout window, first source the .mag file for the design (here inverter). Then Edit >> Text which opens up a dialogue box.



![portA](https://user-images.githubusercontent.com/44607144/187693242-62de42df-7d77-4111-adcf-cfde02484d23.JPG)


![portY](https://user-images.githubusercontent.com/44607144/187693284-43e43bb0-aa04-4e2b-8200-987c29faada7.JPG)

![portVPWR](https://user-images.githubusercontent.com/44607144/187693317-761ee7c2-8cdf-4e69-9a59-76f674e23879.JPG)

![portVGND](https://user-images.githubusercontent.com/44607144/187693437-8cf50fe2-6fdf-4196-97a9-8f3dea7b3347.JPG)

![port_class_use](https://user-images.githubusercontent.com/44607144/187694055-cd3c1b30-a729-4253-94a9-21483cd66fb7.JPG)

### Standard cell Lef generation

Before the CMOS Inverter standard cell LEF is extracted, the purpose of ports must be defined:

Select port A in magic:
```
port class input
port use signal
```
Select Y area
```
port class output
port class signal
```
Select VPWR area
```
port class inout
port use power
```

Select VGND area
```
port class inout
port use ground
```

LEF extraction can be carried out in tkcon as follows:

```
lef write
```
# Generating Layout
## Interactive Mode 
The layout is generated using OpenLane. To run a custom design on openlane, Navigate to the openlane folder and run the following commands:
```
$ cd designs

$ mkdir iiitb_pwm_gen

$ cd iiitb_pwm_gen

$ mkdir src

$ touch config.json

$ cd src

$ touch iiitb_pwm_gen.v
```

Copy ```sky130_fd_sc_hd__fast.lib```, ```sky130_fd_sc_hd__slow.lib```, ```sky130_fd_sc_hd__typical.lib```and ```sky130_vsdinv.lef``` files to src folder in your design.

The contents of src file is shown below:

![sourcefile](https://user-images.githubusercontent.com/44607144/187699339-92d58636-c4b2-4466-99b2-ff65197ccef3.png)

The contents of json file should be modified as:

![json](https://user-images.githubusercontent.com/44607144/187699801-175a52d8-6931-4893-abab-60fd1c2c2e7e.png)


Save all the changes made above and Navigate to the openlane folder in terminal and give the following command :
```
$ sudo make mount
$ ./flow.tcl -interactive
```
A tcl console will be opened, now we will give following commands:
```
% package require openlane 0.9
% prep -design iiitb_pwm_gen
```
The following commands are to merge external the lef files to the merged.nom.lef. In our case sky130_vsdiat is getting merged to the lef file
```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
The contents of the merged.nom.lef file should contain the Macro definition of sky130_vsdinv

![merged](https://user-images.githubusercontent.com/44607144/187702425-e31e6355-f211-436f-99e3-4768add7ec45.png)

# Synthesis
```
% run_synthesis
```
### Synthesis Report

![stat_synth](https://user-images.githubusercontent.com/44607144/187703037-cb0a06c0-c471-4723-8dd9-e6ce087b4b53.png)

* Calcuation of Flop Ratio:
```
Flop ratio = Number of D Flip flops 
             ______________________
             Total Number of cells
```
             
The flop ratio of my design is 0.21229

The sky130_vsdinv should also reflect in your netlist after synthesis

![inv_netlist](https://user-images.githubusercontent.com/44607144/187710602-6d56682e-1e91-4ee7-a27d-41888a06cd75.png)


*Timing contraints of design:*

![timing const](https://user-images.githubusercontent.com/44607144/187705920-e7c6a25c-c640-441f-a3f0-47e110383352.png)

## Floorplan
In the VLSI physical design, floorplanning is an essential design step, as it determines the size, shape, and locations of modules in a chip and as such it estimates the total chip area, the interconnects, and, delay.
```
% run_floorplan
```
#### Floorplan Results 
* Die Area

![die area](https://user-images.githubusercontent.com/44607144/187708931-7c96b758-215d-4165-a67c-be92fa9eca5c.png)

* Core area

![core area](https://user-images.githubusercontent.com/44607144/187709019-46a255e4-1ba4-48d2-a833-cb04e8e5e17d.png)

Post the floorplan run, a .def file will have been created within the results/floorplan directory. We may review floorplan files by checking the floorplan.tcl. The system defaults will have been overriden by switches set in conifg.tcl and further overriden by switches set in sky130A_sky130_fd_sc_hd_config.tcl.

For floorplan view navigate to /home/himanshu/OpenLane/designs/iiitb_pwm_gen/runs/RUN_2022.08.30_14.11.34/results/floorplan and then type the following magic command :
```
magic -T /home/himanshu/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech read lef ../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &
```
![fpcmd](https://user-images.githubusercontent.com/44607144/187711778-34e7a4a6-5d8c-495a-8685-625dadf6efa9.png)

*Floorplan View*

![floorplan_1](https://user-images.githubusercontent.com/44607144/187711929-6eedce34-bbb2-4d45-8dea-1439e3f85892.png)

## Placement
Placement is the process of placing the standard cells inside the core boundary in an optimal location. The tool tries to place the standard cell in such a way that the design should have minimal congestions and the best timing.

```
% run_placement
```
Navigate to /home/himanshu/OpenLane/designs/iiitb_pwm_gen/runs/RUN_2022.08.30_14.11.34/results/placement and type magic command:

```
magic -T /home/himanshu/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech read lef ../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &
```
![pcmd](https://user-images.githubusercontent.com/44607144/187713407-8a96d8e6-8f32-402d-88cd-0d854809f172.png)

*Placement View*


![placement](https://user-images.githubusercontent.com/44607144/187713554-e47c12cf-6706-4116-878b-4f48fbd8280d.png)

![place2](https://user-images.githubusercontent.com/44607144/187713842-abcaa520-b0c1-4a68-9e81-0e0985d6fbc2.png)

*sky130_vsdinv placement view*

![vsdinvview](https://user-images.githubusercontent.com/44607144/187714527-b9ded0f5-856f-47f5-8747-952886e11e87.png)

## Clock Tree Synthesis(CTS)
Clock Tree Synthesis is a technique for distributing the clock equally among all sequential parts of a VLSI design. The purpose of Clock Tree Synthesis is to reduce skew and delay. Clock Tree Synthesis is provided the placement data as well as the clock tree limitations as input.

```
% run_cts
```
## Routing
Making physical connections between signal pins using metal layers are called Routing. Routing is the stage after CTS and optimization where exact paths for the interconnection of standard cells and macros and I/O pins are determined.

```
% run_routing
```
Navigate to /home/himanshu/OpenLane/designs/iiitb_pwm_gen/runs/RUN_2022.08.30_14.11.34/results/placement and type magic command:

```
magic -T /home/himanshu/OpenLane/pdks/sky130A/libs.tech/magic/sky130A.tech read lef ../../tmp/merged.nom.lef def read iiitb_pwm_gen.def &
```
![routcmd](https://user-images.githubusercontent.com/44607144/187716273-fc8fb165-c338-4556-9e51-1fe967773c1c.png)

*Routing View*

![routing](https://user-images.githubusercontent.com/44607144/187715850-edf04072-2b0c-4196-9f48-ad6d2eb7dd16.png)


![routing2](https://user-images.githubusercontent.com/44607144/187716598-be790a9e-de74-4847-9388-011e9b6f0f79.png)

*sky130_vsdinv routing view*


![vsdinv_routing](https://user-images.githubusercontent.com/44607144/187716720-6e9063d7-bf35-4007-b014-d19a46343c41.png)

*Area Report*

![area](https://user-images.githubusercontent.com/44607144/187734979-a3921cd6-60e1-49f0-a6a0-40ecb9684eea.png)

# Viewing Layout in klayout

![kl1](https://user-images.githubusercontent.com/44607144/187735941-01d77ed5-8feb-4c58-80d9-73d207318587.png)

## Non Interactive Mode 

Here we are generating the layout in the non-interactive mode or the automatic mode. In this we cant interact with the flow in the middle of each stage of the flow.The flow completes all the stages starting from synthesis until you obtain the final layout and the reports of various stages which specify the violations and problems if present during the flow.

```
$ cd designs

$ mkdir iiitb_pwm_gen

$ cd iiitb_pwm_gen

$ mkdir src

$ touch config.json

$ cd src

$ touch iiitb_pwm_gen.v
```
Copy ```sky130_fd_sc_hd__fast.lib```, ```sky130_fd_sc_hd__slow.lib```, ```sky130_fd_sc_hd__typical.lib```and ```sky130_vsdinv.lef``` files to src folder in your design.

The contents of src file is shown below:

![sourcefile](https://user-images.githubusercontent.com/44607144/187699339-92d58636-c4b2-4466-99b2-ff65197ccef3.png)

The contents of json file should be modified as:

![json](https://user-images.githubusercontent.com/44607144/187699801-175a52d8-6931-4893-abab-60fd1c2c2e7e.png)

Save all the changes made above and Navigate to the openlane folder in terminal and give the following command :

```
sudo make mount
./flow.tcl -design iiitb_pwm_gen
```
All the steps will be automated and all the files will be generated.


![nonint1](https://user-images.githubusercontent.com/44607144/187742255-61f9dc8f-b092-4c5d-8efc-28e4d5e2a041.png)


After running all the steps flow complete message will apperas shown below.


![flowcomp](https://user-images.githubusercontent.com/44607144/187742016-4c2f0b4f-cfa1-44a4-af58-3c06a999e0d9.png)

After the flow complete message open terminal and write following commands to view layout in klayout:
```
cd /home/himanshu/OpenLane/designs/iiitb_pwm_gen/runs/RUN_2022.08.31_17.08.00/results/signoff
klayout iiitb_pwm_gen.mag
```
![klayout1](https://user-images.githubusercontent.com/44607144/187743942-66528e66-baa2-4fcc-923e-9283d00d72b2.png)

*Layout View*
![klayout2](https://user-images.githubusercontent.com/44607144/187743956-f544cd60-6e06-4e79-81c2-93dd5b5342f5.png)
# Post Layout Results
### gate count 
![stat](https://user-images.githubusercontent.com/44607144/192511160-2a3056f2-ecab-434e-84c2-ed8b4c55b4af.png)
**Gate count = 179**
### Area


![area](https://user-images.githubusercontent.com/44607144/192512642-f352fcb5-400e-4c44-b374-0b7d00a9da8d.png)

**Area = 11773.920 um^2**
### Performance 

![command](https://user-images.githubusercontent.com/44607144/192513247-96304b73-58ab-4f43-a97f-ee7f7b3b3846.png)

![timing](https://user-images.githubusercontent.com/44607144/192513286-946b2c32-2b0b-49da-9d7c-3ccec891c413.png)

**performance = 1/(clock period-slack = 1/(10-6.63)ps =296.74GHz)**
### Flop/standard cell 
![stat](https://user-images.githubusercontent.com/44607144/192515140-8eeddec9-d8a7-418e-a77f-ffe03d5631d6.png)

**Flop ratio = 40/179 = 0.2234**

### Power Performance 

![power](https://user-images.githubusercontent.com/44607144/192515474-d5a5ed04-ee7c-4da8-8c88-8f520ed15ff4.png)









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
* https://www.klayout.de/
* https://docs.docker.com/engine/install/ubuntu/
* https://ngspice.sourceforge.io/













