# Physical Design using OpenLane 
The workflow for automated RTL to GDSII using Sky-water PDK and open-source tool chain called OpenLane is the objective of the workshop. 
## Framework of the workshop 
  ### Aim - Learn - Assess - Practice - Bridge the gap - Industry Ready
  The workshop is hosted on virtual learning platform called Intelligent Assesment Technology which unlocks the potential of everyone. The course is designed to bring up a solution in bridging the gap of theoretical concepts and practical experience.


## Introduction
The 5-day workshop covers the open-source tool chain called openlane using the open-source sky-water pdk for each step in RTL-GDSII. The transformation of circuit description into physical layout which describes the position of instances and the interconnections is called the physical design in VLSI Design.
The workshop targets the day-wise step-upof the skills required for the physical design in VLSI System Design.
<img width="325" alt="image_39b568fa-144a-4e00-834c-cd8cd70d24b420210129_151448" src="https://user-images.githubusercontent.com/25682001/106277463-2c533d80-625f-11eb-9bd9-4ce9d3989ff3.jpg">
#Contents

- [Day-1](#Day-1)

  * [SoC Design and Open-Source Tool Chain ](#SoC Design and Open-Source Tool Chain)
  
   
  * [OpenLane - Basics and Synthesis](#OpenLane - Basics and Synthesis)
  
   
 - [Day-2](#Day-2)

  * [Chip Floorplanning](#Chip Floorplanning)
  
	
  * [Design and Charecterisation of Library cells](#Design and Charecterisation of Library cells)
  
  
	
- [Day-3](#Day-3)

  * [CMOS Fabrication Process](#CMOS Fabrication Process)
  
  * [SPICE Simulations](#SPICE Simulations)  
  
  
  
- [Day-4](#Day-4) 

  * [Insertion of Custom Cell in the reference design](#Insertion of Custom Cell in the reference design)
  
  * [Synthesis](#Synthesis)
	
  * [CTS](#CTS) 
  
  
- [Day-5](#Day-5) 

  * [Power Distribution](#PDN) 
  
  * [Routing](#Routing) 
  
  * [SPEF Extraction](#SPEF Extraction) 

  

  
  
## Day-1
### SoC Design and Open-Source Tool Chain 
#### Components of a SoC 
* The chip has the core where all the logic, Macros, Foundary IP's lies and pads for signal I/O and the total die after the floorplanning, placement, routing etc are completed. 
* Interface between an application and actual hardware is brought in by the system software. 
* However, the hardware understands only the binaries, and hence an abstract interface which starts with Intruction Set Architecture, then the RTL Description.
* Once RTL description is done, then the process of conversion of RTL to physical chip is done. 
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
 
<img width="500" alt="Screenshot (416)" src="https://user-images.githubusercontent.com/25682001/106051614-b049e000-610e-11eb-9f0a-bd378e67b158.png"> 
<img width="500" alt="Screenshot (447)" src="https://user-images.githubusercontent.com/25682001/106052251-5d245d00-610f-11eb-9bec-206b4a9b219e.png"> 
 
 * Synthesis :- The synthesis is done by Yosys and ABC tools for the conversion of RTL to netlist description. The Analysis of Flop Ratio, Instance Count, timing behavior etc can be performed after the synthesis if the design. The synthesis is done by following command.


        run_synthesis

The synthesis result can be utilised tocount the Buffer Ratio, FLop Ratio etc. The buffer ratio can be observedfrom the results obtained as below.

<img width="500" alt="Screenshot (448)" src="https://user-images.githubusercontent.com/25682001/106053808-39faad00-6111-11eb-87ad-b800645ec6e2.png"> 

## Day-2
## Chip Floorplanning
### Height and width of core
* Core is the section of chip where the fundamental logic is placed
* Die consists of core and it is a ssemiconductor material on which the circuit is fabricated.
* Utilisation factor is the ratio of Area occupied by netlist and the Total Area of core. The 100% utilisation refers to Utilisation factor being 1.
* Aspect Ratio is the ratio o the heught and width of the core. The Aspect ratio of 1 refers to a square chip. 
### Pre-placed Cells
* Pre-placed cells are those which are implemented once and instantiated many times. 
* The arrangement of these IPs in the chip is referred to as Floorplanning.
### Decoupling Capacitors
* Large complex circuits will have high amount of switching current.
* Noise margin specs define the logic'0' and logic '1' valid voltages and undefined regions.
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

The floorplanning of the design can be done with the following command


      run_floorplan
      
This generates a floorplan.def file whhich can be viwed using magic with the command shown in below image.

 <img width="500" alt="Screenshot (449)" src="https://user-images.githubusercontent.com/25682001/106056594-da060580-6114-11eb-8065-e4846ad32dc1.png"> 


The resultant floorplan can be seen as below

<img width="500" alt="Screenshot (450)" src="https://user-images.githubusercontent.com/25682001/106057140-8647ec00-6115-11eb-8865-08cbe288ac6c.png">


The Placement of the floor planned design can be done with the following command


    run_placement
    
The overflow value needs to be converging to 0. And the placement cell legalization should be reported.

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
## Day-3
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
### SPICE Simulations
* SPICE Deck
* NGSPICE commands for simulation of spice definitions of the circuits
* Evaluation of Static and Dynamic behavior of CMOS INverter
* Timing Parameters:
    * Rise Delay : Time taken for waveform to rise from 20% to 80% of VDD.
    * Fall Delay : Time taken for waveform to fall from 80% to 20% of VDD.
   *  Propagation Delay : Measured between 50% of Input transition to 50% of Output transition.

The Inverter cell under consideration can be cloned from the specified git repo and the layout can be viewed in MAGIC to check for any DRC errors and then the SPICE netlist is extracted and the analysis is performed.

<img width="500" alt="1" src="https://user-images.githubusercontent.com/25682001/106283218-a4256600-6267-11eb-8b8b-1d0f26d076c2.png">
<img width="500" alt="2" src="https://user-images.githubusercontent.com/25682001/106283202-a12a7580-6267-11eb-9baf-aa6efcbcbc72.png">
<img width="500" alt="4" src="https://user-images.githubusercontent.com/25682001/106283212-a2f43900-6267-11eb-8fca-a0632f8f8f03.png">

The spice deck basically contains the model files, component connectivity information, Type of analysis to be done, Load capacitance values etc. The previously extracted spice is edited according to the connectivity and libraries. The Spice Deck for the custom designed inverter cell can be as below.

<img width="500" alt="5" src="https://user-images.githubusercontent.com/25682001/106282945-4abd3700-6267-11eb-9743-aa0c2d394614.png">

The command to run the spice netlist is shown below, 


    ngspice sky130_inv.spice



<img width="500" alt="6" src="https://user-images.githubusercontent.com/25682001/106283911-81e01800-6268-11eb-8bcc-5fd50c7e4543.png">

Since the simulation commands are given inside the spice file, there is no requirement to give the commands explicitly except for plotting the waveform. 
 
  
    plot y vs time a
    
The simulation result can be seen in the below image.

<img width="500" alt="7" src="https://user-images.githubusercontent.com/25682001/106283907-80165480-6268-11eb-86c2-4ba5349846b3.png">

The timing parameters can be calculated from the cordinates as shown below.

<img width="500" alt="cell_rise_delay_prop_delay" src="https://user-images.githubusercontent.com/25682001/106287404-c077d180-626c-11eb-8115-fe47295c19ff.png">


## Day-4

### Insertion of Custom Cell in the reference design
The designed standard cell is plugged into a complex design and perform it's PnR in the openlane flow. The standard cell chosen is a basic CMOS inverter and the design into which it's plugged into is a pre-built picorv32a core.

The steps to be performed are as below
1. create a bounding box with the command    $property FIXED_BBOX {0 0 138 272}.
2. Insertion of all layers to form Power/Ground Rails, PMOS/NMOS Transistors, Input and Output pins.
3. LEF file Preperation Steps: - This needs few more sub-steps  i.e.,i) Defining port ii) setting class ii)use attributes to each port.

<img width="500" alt="9" src= "https://user-images.githubusercontent.com/25682001/106292480-dee0cb80-6272-11eb-8677-561af911bd7f.png">
<img width="500" alt="10" src= "https://user-images.githubusercontent.com/25682001/106292496-e43e1600-6272-11eb-97b2-a96f233b6e1f.png>
<img width="500" alt="11" src= "https://user-images.githubusercontent.com/25682001/106292460-da1c1780-6272-11eb-983f-e04c4ba74c39.png">

4. LEF file EXtraction. The command for LEF file extraction from the layout is

        lef write sky130_inv.lef
        
   The lef file output is as in the below image
   
   <img width="500" alt="12" src= "https://user-images.githubusercontent.com/25682001/106292829-4dbe2480-6273-11eb-9d8d-0251ba37dc82.png">
   
5. Insertion of custom LEF into the openlane flow requires the following commands to be included in the config.tcl
  
        set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
        set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
        add_lefs -src $lefs
### Synthesis        
Now, repeat the openlane flow steps i.e, synthesis, floorplan and placement.
Any slack violations can be rectified by one or more of following mechanisms
* SYNTH_STRATEGY checking
* Cell BUFFERING
* Cell replpacement manually by observing the critical path with OPENSTA tool
* Fanout value optimisation

### CTS
* There are different algorithms for the clock tree synthesis out of which H-Tree is the most popular one. Clock skew is the difference between arrival times of the clock for sequential elements across the design.  
The CTS can be performed with the command

      run_cts
 
 OpenLANE will generate a new .def fil after CTS. To view this, invoke the .def file with the Magic tool. 
 
 <img width="500" alt="openroad_cmds_post_cts" src= "https://user-images.githubusercontent.com/25682001/106294994-e5247700-6275-11eb-9646-c613e40b22b8.png">
 <img width="500" alt= "post_cts_slack" src= "https://user-images.githubusercontent.com/25682001/106295005-e81f6780-6275-11eb-91b6-b0765fa7dc6e.png">
 <img width="500" alt= "ppostcts_hold_slack" src=  "https://user-images.githubusercontent.com/25682001/106295013-eb1a5800-6275-11eb-986b-f52a61efe3bf.png">
 
 The next steps to follow are 
 * Invoke the openroad tool with the command %openroad
 * The lef and def files are to be read
 * .db file is to be created for further analysis
 
  
        read_def $env(CURRENT_DEF)
        write_db post_cts.db
        
 The .def is utilised for PDN to continue

## Day 5

### PDN
 After the post CTS STA analysis and timing closure, the power distribution network anfd final routing can be performed.
 The PDN will create: 
 * Ring to complete core
 * Straps to bring power to the chip
 * Power rails for standard cells
 * Power halo to any preplaced cells.

The command used for PDN is

      gen_pdn
      
 The PDN should be performed on the final cts.def file as shown in the below image.
 
  <img width="500" alt="pdn" src= "https://user-images.githubusercontent.com/25682001/106294996-e5bd0d80-6275-11eb-8b5c-0f4586fb6201.png">
  
  The completed PDN writes the database and the pitch value of metal1 shown in the image should be the height of the standard cells.
 <img width="500" alt="pdn_compl" src= "https://user-images.githubusercontent.com/25682001/106295001-e6ee3a80-6275-11eb-9fea-4a53030adb19.png">
### Routing
Global routing is done inside the openroad itslef. The Titron-route does the detailed routing and the command as shown in image below.
  <img width="500" alt="route_cmd" src= "https://user-images.githubusercontent.com/25682001/106295018-ec4b8500-6275-11eb-8879-d36de51c4eca.png">

### SPEF Extraction
<img width="500" alt="spef" src= "https://user-images.githubusercontent.com/25682001/106350648-ae457400-62fc-11eb-8b76-7def596635ee.png">
