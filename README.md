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













