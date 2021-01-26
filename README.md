# OpenLANE-Workshop
The step by step workflow for automation of RTL to GDSII using Sky-water PDK and open-source tool OpenLANE.
## Framework of the workshop 
  ### Aim - Learn - Assess - Practice - Bridge the gap - Industry Ready
  The workshop is hosted on virtual learning platform called Intelligent Assesment Technology which unlocks the potential of everyone. The course is designed to bring up a solution in bridging the gap of theoretical concepts and practical experience.

## Introduction
The 5-day workshop covers the open-source tool chain using the open-source sky-water pdk for each step in RTL-GDSII.
The various tools used in openlane flow are Yosys for synthesis, opensta for Static timing analysis, OpenROAD for floorplanning, placement, CTS, global routing etc.

  
The RTL-GDSII flow is an iterative process, where in there is a need to be watchful on timing/area/power requirements. The design should be modified in order to not violate the timing, which is also an iterative procedure. ONce the timing violations are fixed, again the steps from floorplan needs to be done. 

## Study  & Review of components of RISC-V based picoSoC.
### Components of a SoC
* The chip has the core where all the logic, Macros, Foundary IP's lies and pads for signal I/O and the total die after the floorplanning, placement, routing etc are completed. 
* Interface between an application and actual hardware is brought in by the system software. 
* However, the hardware understands only the binaries, and hence an abstract interface which starts with Intruction Set Architecture, then the RTL Description.
* Once RTL description is done, then the process of conversion of RTL to physical chip is done. 
### Introduction to RISC-V picoSoc
RISC-V is an open-source standard instruction set architecture (ISA). Each SoC has a RISC-V processor, memory, a range of I/O, and interfaces for embedding user functions. PicoSoC example components are UART, SPI memory controller, Scratchpad SRAM memory, SPI flash memo.
### Design Flow 
* The RTL description of the Soc is done as the initial step. The reference design of Raven PicoSoc - picorv32 is considered for further steps.
* The RTL is converted to gate level netlist with Synthesis tool called Yosys embedded in the Openlane tool chain.
* Then comes the floorplanning, placement, Routing, CTS.
* The most important step is Static Timing Analysis (STA) which should be performed at each and every juncture of the design step in order to meet the requirements of the design.
#### Open-Source tools for chip design
| Design Step                                | Tool      |
|--------------------------------------------|-----------|
| Synthesis                                  | Yosys     |
| Floorplanning                              | OpenROAD  |
| Placement                                  | OpenROAD  |
| Clock Tree Synthesis (CTS)                 | OpenROAD  |
| Layout Edit, view                          | MAGIC     |
| Pre-layout, post-layout, spice simulations | NGSPICE   |
| Static Timing Analysis                     | OpenTimer |

## Inception of Open Source EDA
