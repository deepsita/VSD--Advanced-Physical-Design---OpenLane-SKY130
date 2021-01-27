# OpenLane-Workshop
The workflow for automation of RTL to GDSII using Sky-water PDK and open-source tool chain called OpenLANE.
## Framework of the workshop 
  ### Aim - Learn - Assess - Practice - Bridge the gap - Industry Ready
  The workshop is hosted on virtual learning platform called Intelligent Assesment Technology which unlocks the potential of everyone. The course is designed to bring up a solution in bridging the gap of theoretical concepts and practical experience.

## Introduction
The 5-day workshop covers the open-source tool chain called openlane using the open-source sky-water pdk for each step in RTL-GDSII. The transformation of circuit description into physical layout which describes the position of instances and the interconnections is called the physical design in VLSI Design.
The workshop targets the day-wise step-upof the skills required for the physical design in VLSI System Design.
## Day-1
### SoC Design and Open-Source Tool Chain 
#### Components of a SoC - 
* The chip has the core where all the logic, Macros, Foundary IP's lies and pads for signal I/O and the total die after the floorplanning, placement, routing etc are completed. 
* Interface between an application and actual hardware is brought in by the system software. 
* However, the hardware understands only the binaries, and hence an abstract interface which starts with Intruction Set Architecture, then the RTL Description.
* Once RTL description is done, then the process of conversion of RTL to physical chip is done. 
* RISC-V is an open-source standard instruction set architecture (ISA). Each SoC has a RISC-V processor, memory, a range of I/O, and interfaces for embedding user functions. PicoSoC example components are UART, SPI memory controller, Scratchpad SRAM memory, SPI flash memo. The reference design of Raven PicoSoc - picorv32 is considered for further steps.
### ASIC Design Flow 
* The complete ASIC design requires three major components
  * RTL description of the design
  * EDA Tool
  * PDK(Process design kit)
* The RTL description of the Soc is done as the initial step. The reference design of Raven PicoSoc - picorv32 is considered for further steps.

#### Open-Source tools for chip design - OpenLane Tool Chain 
| Design Step                                | Tool      |
|--------------------------------------------|-----------|
| Synthesis                                  | Yosys     |
| Floorplanning                              | OpenROAD  |
| Placement                                  | OpenROAD  |
| Clock Tree Synthesis (CTS)                 | OpenROAD  |
| Layout Edit, view                          | MAGIC     |
| Pre-layout, post-layout, spice simulations | NGSPICE   |
| Static Timing Analysis                     | OpenTimer |

#### Open-Source PDK for chip design
Process Design Kit(PDKs) contains device models, digital standard cell libraries, process design rules etc. used for fabrication. The SkyWater Open Source PDK is a collaboration between Google and SkyWater Technology Foundry to provide a fully open source Process Design Kit and related resources, which can be used to create manufacturable designs at SkyWater’s facility.
The latest SkyWater SKY130 PDK design resources can be viewed at the following locations:
* [On Github @ google/skywater-pdk](https://github.com/google/skywater-pdk)
* Google CodeSearch interface @ https://cs.opensource.google/skywater-pdk
* [foss-eda-tools.googlesource.com/skywater-pdk](https://foss-eda-tools.googlesource.com/skywater-pdk)

#### Final Steps
* The RTL is converted to gate level netlist with Synthesis tool called Yosys embedded in the Openlane tool chain.
* Then comes the floorplanning, placement, Routing, CTS.
* The most important step is Static Timing Analysis (STA) which should be performed at each and every juncture of the design step in order to meet the requirements of the design.

### OpenLane - Basics and Synthesis
#### The OpenLane Directory Structure
There are two directories inside the working directory, 1. "openlane_flow" where all the design and the flow based files are placed. 2. The "sky130" folder has "libs.ref" and "libs.tech" , these are the folders which have the files specific to tool and specific to technology. The designs are saved in the path 
" /openlane_working_dir/openlane_flow/designs".
#### Synthesis of RTL
Steps for the synthesis of the design using openlane are
 * Invoke the openlane tool : - The openlane tool can be invoked from the flow.tcl file. The interactive mode switch gives the option to work with the tool configurations on the fly at any step of the PD.


         ./flow.tcl -interactive
     
 * Import the packages:- The package required to run the openlane flow is to be specified.
        
        package require openlane 0.9
 * Design setup :- The preparation of the design is to be done to create the required directories for further steps. The switch -tag is optional and can be used when the specific folder is required to be used for the further steps. -overwrite is one more optional switch which can be utilised when the values of the configuration are required to be changed.
 
 
        prep -design picorv32a
        prep -design picorv32a -tag <any name>
        prep -design picorv32a -tag <any name> -overwrite
 All the specified steps can be observed in the below images. 
 ![Screenshot (416)](https://user-images.githubusercontent.com/25682001/106051614-b049e000-610e-11eb-9f0a-bd378e67b158.png)
 
![Screenshot (447)](https://user-images.githubusercontent.com/25682001/106052251-5d245d00-610f-11eb-9bec-206b4a9b219e.png)
 
 * Synthesis :- The synthesis is done by Yosys and ABC tools for the conversion of RTL to netlist description. The Analysis of Flop Ratio, Instance Count, timing behavior etc can be performed after the synthesis if the design. The synthesis is done by following command.


        run_synthesis

The synthesis result can be utilised tocount the Buffer Ratio, FLop Ratio etc. The buffer ratio can be observedfrom the results obtained as below.

![Screenshot (448)](https://user-images.githubusercontent.com/25682001/106053808-39faad00-6111-11eb-87ad-b800645ec6e2.png)

 
## Day-2
##Chip Floorplanning and Standard Cells
The concepts of chip floor planning considerations, Binding the library to the design, cell design flow and timing charecterisation parameters were presented. 
### Height and width of core
* Core is the section of chip where the fundamental logic is placed
* Die consists of core and it is a ssemiconductor material on which the circuit is fabricated.
* Utilisation factor is the ratio of Area occupied by netlist and the Total Area of core. The 100% utilisation refers to Utilisation factor being 1. However, in the practical scenarios only 50-60% utilisation is considered in order to provide place for other routing and filler cells etc.
* Aspect Ratio is the ratio o the heught and width of the core. The Aspect ratio of 1 refers to a square chip. 
### Pre-placed Cells
* Pre-placed cells are those which are implemented once and instantiated many times. The arrangement of these IPs in the chip is referred to as Floorplanning.
* These IPs/blocks have user defined locations and hence are placed in chip before the automated placement and routing.

### Decoupling Capacitors
* Large complex circuits will have high amount of switching current.
* Noise mArgin specs define the logic'0' and logic '1' valid voltages and undefined regions.
* The high switching current demand can be solved by addition of decoupling capacitors in parallel with the circuit.

### Power Planning
* The power planning should be done in a way that the driver and load be close to each other in the 'L' sense. 
* The improper power planning i.e., single power and Ground lines can lead to 'Ground bounce' and 'Voltage Drrop'.
* So the plan should have multiple 'VDD', 'VSS' lines running through the circuit.

### Pin Placement

* The conectivity of the chip to the outside world.
* The placement of the pins is dependent on the position (near/far) but not on the ordering. 
* The clock ports are found to be bigger than the general data ports in order to necessitate for less resistance. 

### Placement and Routing

* Bind the netlist with the physical cells i.e., library cells
* Placement of the logic
* optimise the placement by inserting repeater


## Design and Charecterisation of Library cells
### Intro to Library Standard Cells
* Standard Cells in the library can be ANDGate, Or gate, Buffer, DFF etc.
* Each cell have different functionality
* cells with same functionality can have varied sizes.
* cells with same functionality can have varied switching thresholds, rise and fall delys etc.
### Cell design flow
* Inputs
  * Process design Kits
  * DRC LVS rules of tech node
  * SPICE models
  * Library and User defined Specs like cell-height, supply voltage, metal layers, pin location, gate length
* Design Steps
  * Circuit design - Transitor based implementation of the required functionality
  * Layout Design - stick diagrams
  * Charecterisation
* Outputs
  * Circuit Description Language
  * GDSII
  * Extracted Spice netlist
  * Timing, noise etc.
### Flow of Charecterisation 
 * Model Libraries
 * Spice model
 * circuit model
 * Power Supply
 * Input Stimulus
 * Output Capacitance
 * Define the required Charecterisation (DC/ Transient).
### Timing Charecterisation
 * Timing Threshold Definition
    * slew Rise,fall
    * IN Rise ,fall
    * OUT Rise,fall 
 * Propagation Delay
 * Transition Time
 
### SPICE Simulations
* SPICE Deck
* NGSPICE commands for simulation of spice definitions of the circuits
* Evaluation of Static and Dynamic behavior of CMOS INverter
### Art of Layout - Euler's path and stick diagrams
* Importance of ordering of inputs i.e., poly bars in the layout.
* Increase of complexity and wiring if not properly ordered
* Eulers path defines the best input ordering to meet the needs of layout. 
  * Network graphs are constructed
  * Number the nodes
  * Transistors as edges between the nodes
* The Layout should be drawn in lines with the Design rules specified for tech node under consideration.
### CMOS Fabrication Process
A 16-mask process is explained with each step in detail 
* Selection of Substrate
* Active region for Transistors - Mask1
* N-well, P-well formation  - Mask2 and Mask-3
* Formation of Gate -Mask 4, Mask-5, Mask-6
* Lightly doped drain formation - Mask-7, Mask-8
* Source and Drain Formation - Mask-9, Mask-10
* Contacts and Interconnects - Mask-11
* High level metal formation -Mask-12 to Mask-16
