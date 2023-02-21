# SKY130-OpenSTA-workshop

The following repository consists of knowledge gained and steps followed while doing the Sign-off Timing Analysis - Basics to Advanced Using [OpenSTA/SKY130](https://github.com/The-OpenROAD-Project/OpenSTA/blob/master/doc/OpenSTA.pdf) workshop. The [workshop](https://www.vlsisystemdesign.com/sign-off-timing-analysis-basics-to-advanced/) covers all the basic concepts in STA and Timing constraints using open soucrce EDA tools such as OpenSTA/SKY130. 

# Table of Contents 
  * Sky130 Day 1
    * Day 1 Lectures 
      * STA Definition
      * Timing Paths
      * Timing Path elements
      * Setup and Hold Checks
      * Slack Calculation
      * SDC Overview 
      * Clocks
      * Generated Clocks
      * Boundary Constraints 
    * Day 1 Lab
      * OpenSTA Introduction
      * Inputs to OpenSTA 
      * Constraints creation 
      * OpenSTA runscripts 
  * Sky130 Day 2
    * Day 2 Lectures
      * Other Timing Checks
      * Design Rule Checks
      * Latch Timing
      * STA text Report 
    * Day 2 Lab
      * Liberty Files
      * SPEF
      * Timing Reports
  * Sky130 Day 3
    * Day 3 Lectures
      * Multiple Clocks
      * Timing Arcs and Timing Sense 
      * Cell Delays and Clock Network
      * Setup and Hold Detailed 
      * STA Text Report  
    * Day 3 Lab  
      * Reg to Reg STA analysis 
      * Slack Computation 
      * Review Setup Check Report 
     
     
# Day 1 
## Lectures
### STA Definition

Static Timing Analysis (STA) is a technique used in digital circuit design to verify the timing performance of a circuit. Timing constraints are specifications for the timing behavior of the circuit, including maximum and minimum delay times, clock frequency, setup and hold times, clock skew, and jitter. These constraints are used by the STA tool to perform timing analysis and ensure the circuit meets its timing requirements. Timing constraints are typically specified using a Hardware Description Language (HDL) or a Constraints file, and they are critical in the digital circuit design process to ensure proper functionality.

#### STA Features

* Static in nature: STA is a static analysis technique, which means that it does not involve simulation or execution of the circuit. Instead, it analyzes the timing behavior of the circuit based on its structure and timing constraints.
* Exhaustive: STA performs an exhaustive analysis of the circuit's timing behavior, checking all possible paths and scenarios to ensure that the circuit meets its timing requirements.
* Functionality: STA does not verify the functional correctness of the circuit, only its timing performance. It assumes that the circuit is functionally correct and focuses on ensuring that the timing requirements are met.
* Conservative: STA takes a conservative approach to timing analysis, assuming the worst-case scenario for all timing constraints. This ensures that the circuit meets its timing requirements even under worst-case conditions, but can result in overly pessimistic timing analysis results.
* Synchronous Design: STA is primarily used for synchronous digital circuit designs, which rely on a clock signal to synchronize the inputs and outputs. Asynchronous designs, which do not use a clock signal, require different timing analysis techniques.

#### Inputs to STA 

* Netlist: is a structural representation of the digital circuit design in Verilog or VHDL. It describes the components of the circuit, their connections, and the hierarchy of the design.
* SDC or Constraints file: A Synopsys Design Constraints (SDC) file or other constraints file is a text file that specifies the timing constraints of the digital circuit design. These constraints include the maximum and minimum delay times of each component, the setup and hold times of each data signal, the clock frequency, and the clock skew
* Logic Libraries: are collections of pre-characterized cells that are used to represent the components of the digital circuit design in the netlist. The logic libraries provide the timing information necessary to analyze the timing performance of the circuit.

### Timing Paths 

-> What do you mean by Timing Paths?
* It Refer to the logical paths a signal takes through a digital circuit from its source to its destination, including sequential and combinational elements. STA analyzes timing paths to determine their delay, setup and hold times, and other timing parameters specified in the constraints. Timing paths are categorized into combinatorial and sequential, and the critical path is the longest path in the design with the maximum operating frequency.

### Timing Path Elements
   
Timing path elements in STA are the start point, where a signal originates, the end point, where it terminates, and the combinational logic elements, such as gates, that the signal passes through. Timing paths are traced to determine the overall delay and timing performance of the digital circuit.

* Start Point: Is the point where the signal originates or enters the digital circuit. This point is typically an input port of the design, where the signal is first introduced to the circuit.
* End Point: Is the point where the signal terminates or leaves the digital circuit. This point is typically an output port of the design, where the signal is outputted from the circuit.
* Combinational Logic: Combinational logic elements are the building blocks of a digital circuit and are used to perform logic operations on the signals passing through the circuit. These elements do not store any information, and the output of a combinational logic element is solely determined by the input values at that moment.

### Setup and Hold Checks

->What is Setup Check?
* Is the minimum time that the data must be stable before the clock edge, and if this time is not met, it can lead to setup violations, resulting in incorrect data being stored in the sequential element. The setup check is essential to ensure correct timing behavior of a digital circuit and prevent data loss or other timing-related issues.
* The setup time of a flip-flop depends on the technology node, operating conditions, and other factors. The value of the setup time is usually provided in the logic libraries.

-> What is Hold Check?
* Is the minimum amount of time that the data must remain stable after the clock edge, and if this time is not met, it can lead to hold violations, resulting in incorrect data being stored in the sequential element. The hold check is necessary to prevent issues such as data corruption, metastability, and other timing-related problems in digital circuits.

### Slack Calculation 

Setup and hold slack is defined as the difference between data required time and data arrivals time. 

>Setup slack = Data required time - Data arrival time

-> What is Data Arrival Time?
* The time taken by the signal to travel from the start point to the end point of the digital circuit. 

-> What is Data Required Time? 
* The time for the clock to traverse through the clock path of the digital circuit. 

-> What is Slack? 
* It is difference between the desired arrival times and the actual arrival time for a signal. 
* Positive Slack indicates that the design is meeting the timing and still it can be improved. 
* Zero slack means that the design is critically working at the desired frequency. 
* Negative slack means, design has not achieved the specified timings at the specified frequency.
* Slack has to be positive always and negative slack indicates a violation in timing. 

### SDC Overview 

#### Constraint for Timing 
Over here, we set clocks definition, clock group, clock latency, clock uncertainty, clock transition, input delay, output delay, timing derates etc.

Eg: 
> Create_Clock - The create_clock command creates a clock object in the current design. This command defines the specified source_objects as a clock source.
> Create_generated_clock - The create_generated_clock command creates a generated clock object. A pin or port could be specified for the generated clock object. Generated clock follows the master clock, so whenever the master clock changes generated clock will change automatically.


#### Constraints for Area and Power

The constraints for area and power are specified in the SDC file to ensure that the design meets the required performance, power consumption, and die area.

Eg:
->set_max_area: Specifies the maximum allowed area for the design
->set_max_dynamic_power: Specifies the maximum allowed dynamic power consumption


#### Constraints for Design Rules

They ensure that the design meets the required physical design rules and can be successfully manufactured

Eg:
->set_max_capacitance: Specifies the maximum allowed capacitance on a net or port
->set_min_transition: Specifies the maximum allowed input transition time


#### Constraints for Interfaces

Interfaces in digital design refer to the standard communication protocols that are used to connect different parts of a system.

Eg:
->set_input_delay: Specifies the input delay for an interface signal
->set_driving_cell: tells the synthesis tool which cell to use as the source of the signal on the specified net or pin.
->set_load: Specifies the load capacitance of a particular net or pin


#### Constraints for Modes 

Refers to the different operational modes of a design, such as power-on reset, test mode, or low-power mode.

Eg: 
-> set_case_analysis: Specifies whether or not case analysis should be performed during timing analysis. 
-> set_logic_dc: Used to set the logical value of a signal during static timing analysis.
-> set_logic_one: Used to set the logical value of a signal to a constant one during static timing analysis.

### Clocks

* Create_clock 
  * Is a command used in timing constraint files to define a clock signal that is used to synchronize different parts of a design.
  * It specifies the name of the clock signal, its frequency, and its phase.
* Create_clock -add switch
  * Is an extension of the "create_clock" command, which allows specifying a switch that is used to enable or disable the clock signal.  
  * This switch can be useful in power optimization techniques like clock gating, where the clock signal is selectively gated to reduce power consumption.

### Generated Clocks 

* Is a command used to create a virtual clock signal from one or more physical clock signals.
* takes various input parameters, such as the name of the virtual clock signal, the relationship between the virtual and physical clock signals, and the timing characteristics of the virtual clock signal.
* here is an example 
``` create_generated_clock -divide_by 2 -source C1 -master_clock CLK1 ```

## Lab 
### Opensta Introduction

OpenSTA is a static timing verifier that operates at the gate level and can be used to verify the timing of a design using various file formats, including Verilog netlists, Liberty libraries, SDC timing constraints, SDF delay annotations, and SPEF parasitics. It is a stand-alone executable that can be easily integrated with other tools as a timing engine. OpenSTA is designed to access host netlist data structures without duplication using a network adapter. It supports query-based incremental updates of delays, arrival, and required times, and features a simulator to propagate constants from constraints and tie high/low netlist.

### Inputs to Opensta

#### Steps to follow 
* Accessing the directory:
  * git clone https://github.com/vikkisachdeva/openSTA_sta_workshop
  * cd to lab1 directory using following unix command “cd lab1”
  
* Accessing the verilog file
  * Type “ls” and you will notice one of the file named “simple.v”
  * simple.v is our design in Verilog format
  * Open file using “leafpad simple.v”
  * This file at the bottom will contain standard cells or library cell instantiations as shown in the image
  ![image](https://user-images.githubusercontent.com/62239145/220377617-b081c6a2-83cd-40ba-85d1-120682eb1da5.png)

  
* Accessing the Library file 
  * Type "cd .." to leave the lab1 directory.
  * Type “ls” and you will notice one of the file named “sky130_fd_sc_hd_tt_025C_1v80.lib”

![image](https://user-images.githubusercontent.com/62239145/220379700-a147eb92-3cfd-42de-a396-6e91f8a6189e.png)

  * Open file using “leafpad sky130_fd_sc_hd_tt_025C_1v80.lib”.
  * This contains the library cells that were instantiated in the verilog file.

* Accessing the constraints file
  * Now go back to the lab1 directory by typing "cd lab1" on the terminal
  * Type “ls” and you will notice one of the file named “simple.sdc”
  * To open it enter the leafpad command
  ![image](https://user-images.githubusercontent.com/62239145/220380211-4c76b373-1ae7-4e79-ad10-c8a8c1546aa0.png)
* Runscript
  * The runscript is where you define all the commands you want to run in the openSTA tool
  * "leafpad run.tcl" to have a look at it
  
  ![image](https://user-images.githubusercontent.com/62239145/220382383-8382070f-bdbf-44eb-b7fb-0fb18051564c.png)
  
  * the comments show that the library files, constraint file and netlist are being included which are our 3 inputs for the STA operation. 
  * to execute type "sta run.tcl -exit | tee run.log"
  * execution results are shown in the next section

### Results 

* for start point: inp1 (input port clocked by tau2015_clk) 
* endpoint: out (failling edge-triggered flip-flop clcked by tau2015_clk)

    we get these final results
![image](https://user-images.githubusercontent.com/62239145/220327015-698ee6c9-b4b4-4fb1-b359-0d4f57c3e27c.png)
   as you can see negative slack which means issues still persist 

* for start point: inp1 (input port clocked by tau2015_clk)
* endpoint: f1 (failling edge-triggered flip-flop clcked by tau2015_clk)

we get these final results
![image](https://user-images.githubusercontent.com/62239145/220332172-79da3d2e-f4df-44e1-8a3d-84befcb78093.png)

Positive slack which means the design is meeting the timing requirements.

# Day 2 
## Lectures 
### Other Timing Check 

There are more timing checks to note while performing STA. Apart from the basic setup and hold checks which happen in data pins and clock pins. 

* Clock Gating Check:
  * Is a process that ensures that the clock gating logic is properly designed and does not introduce timing violations.  
  * The check includes checks for propagation delay, skew, and signal integrity to ensure the proper operation of the circuit.

* Async Pin Checks:
  * Is a process that ensures that the asynchronous inputs are properly synchronized and do not cause timing violations in the design.
  * The check includes setup and hold time checks, recovery time checks, pulse width checks, skew checks, and fanout checks to ensure the proper operation of the circuit.

* Data to Data Checks:
  * Are a set of checks that ensure that the data path in a digital circuit meets the timing requirements. 
  * The checks can be performed using slack-based or delay-based methods and include maximum and minimum data delay checks, data transition checks, data skew checks, and data pulse width checks. 

![image](https://user-images.githubusercontent.com/62239145/220392877-f91380b9-ccc2-4d72-b106-7a1cb3acf1ab.png)


### Design Rule Checks

There are more sets of checks which are done by the STA, called the Design Rule Checks.

* Slew/Transition Analysis 
  * Is a process of analyzing the timing behavior of a digital circuit with respect to the output signal transitions.
  * The analysis involves several checks, including maximum slew, minimum slew, slew rate, and slew propagation delay checks. 

* load analysis 
  * Analyzing the timing behavior of a digital circuit with respect to the load capacitance at the output of the circuit. 
  * Load Analysis also involves several checks, including maximum load, minimum load, and fanout checks, to ensure that the circuit functions correctly and meets its timing specifications.

* Clock Skew Analysis
  * Is a process of analyzing the timing behavior of a digital circuit with respect to the variation in arrival times of the clock signal at different points in the circuit.
  * Clock Skew Analysis involves several checks to ensure that the circuit functions correctly and meets its timing specifications.  
 
* Pulse Width Checks
  * Ensure that the pulse widths are within the required limits to prevent timing violations, such as setup and hold violations.


### Latch Timing

While data is launched and captured at the edges in **flip-flops**, **latches** allow for more flexible timing, as data can be accepted at any time before the latch closes. This means that latches can tolerate greater variation in delay times compared to flip-flops. When there are differences in delay times between two logic paths, **time borrowing** is possible in latched-based designs. This means that if one logic path has more delay and the other has less delay, the time is borrowed from the logic path with less delay in the next clock cycle. This helps to maintain the required timing and ensures that the circuit operates correctly.

![image](https://user-images.githubusercontent.com/62239145/220428472-1d1caeef-1063-46e1-a28e-6561d833d778.png)

### STA Text Report
 
![image](https://user-images.githubusercontent.com/62239145/220428686-53475a7a-04ee-467d-b8ce-73748d73db5d.png)

The STA text report generates a report that shows the critical path of the circuit, which is the longest path from an input to an output that determines the maximum delay in the circuit. The report also shows the timing margins, which is the difference between the calculated delay and the maximum allowed delay for the design to function properly.


## Lab 
### Liberty Files 

The .lib file is an ASCII file that contains timing and power parameters for individual circuit components. It includes information such as timing models, interconnect delays, and I/O delay paths. This file is used to calculate maximum timing values, perform timing checks, and optimize design.
The Liberty File Reference from UC Berkley (https://people.eecs.berkeley.edu/~alanmi/publications/other/liberty07_03.pdf) is a useful resource for learning more about this topic.

![image](https://user-images.githubusercontent.com/62239145/220446228-0bebf760-3a44-465c-a227-d512fe73fda6.png)

#### Steps to Follow

* Understanding LIB parsing
 * Type 'cd lab2' to enter the lab2 directory
 * Type 'ls' you will see 2 library files with the following names: simple_min.lib and simple_max.lib
 ![image](https://user-images.githubusercontent.com/62239145/220447445-c3114cc1-b654-487b-9bc1-f4e8d8634645.png)
 * open them using the leafpad command 
 * our task is to 'find all the cells in simple_max.lib'
 * We could use a command grep to display all the instances where the cell is mentioned in the .lib file.  
 * use the code ```grep "cell " simple_max.lib ``` to print all the instances where the keyword **cell** is used
 ![image](https://user-images.githubusercontent.com/62239145/220451495-bf7cbfed-b185-422b-ac19-8d919d40349a.png)
 *  
     
### SPEF 

### Timing Report 














