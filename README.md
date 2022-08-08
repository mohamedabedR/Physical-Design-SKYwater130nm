# OpenSource Physical Design
  This repository includes all the studies and the skills that have beed gained during the [Advanced Physical Design Using OpenLANE / SKY130](https://www.vlsisystemdesign.com/advanced-physical-design-using-openlane-sky130/) workshop. It is primarily foucused on a complete RTL2GDS flow using the open-soucre flow named OpenLANE. [PICORV32A](https://github.com/cliffordwolf/picorv32) RISC-V core design is used for the purpose.

# Table of Contents
  - [Introduction To RTL to GDSII Flow](#introduction-to-rtl-to-gdsii-flow)
  - [About Google SkyWater PDK](#about-google-skywater-pdk)
  - [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [Introduction To RISC-V](#introduction-to-risc-v)
    - [SoC Design and OpenLANE](#soc-design-and-openlane)
      - [Open-Source PDK Directory Structure](#open-source-pdk-directory-structure)
    - [Open-Source EDA Tools](#open-source-eda-tools)
      - [OpenLANE Initialization and design preperation](#openlane-initialization)
      - [Design Synthesis and Results](#design-synthesis-and-results)
  - [Day 2 - Good floorplan vs bad floorplan and introduction to library cells](#day-2---good-floorplan-vs-bad-floorplan-and-introduction-to-library-cells)
    - [Chip Floorplanning](#chip-floorplanning)
      - [Utilization Factor and Aspect Ratio](#utilization-factor-and-aspect-ratio)
      - [Power Planning](#power-planning)
      - [Pin Placement](#pin-placement)
      - [Floorplan using OpenLANE](#floorplan-using-openlane)
      - [Review Floorplan Layout in Magic](#review-floorplan-layout-in-magic)
    - [Placement](#placement)
      - [Placement and Optimization](#placement-and-optimization)
      - [Placement using OpenLANE](#placement-using-openlane)
  - [Day 3 - Design library cell using Magic Layout and ngspice characterization](#day-3---design-library-cell-using-magic-layout-and-ngspice-characterization)
    - [CMOS Inverter Design using Magic](#cmos-inverter-design-using-magic)
    - [Extract SPICE Netlist from Standard Cell Layout](#extract-spice-netlist-from-standard-cell-layout)
    - [Transient Analysis using NGSPICE](#transient-analysis-using-ngspice)
  - [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)
    
  - [Day 5 - Final steps for RTL2GDS](#day-5---final-steps-for-rtl2gds)
   
  - [References](#references)
  - [Acknowledgement](#acknowledgement)
 
# Introduction To RTL to GDSII Flow
  RTL to GDSII Flow refers to the all the steps involved in converting a RTL Design code to a fabrication ready GDSII format that cuold be fabricated and ready to be used inthe indusrty .
  The RTL to GSDII flow consists of following steps:
  - RTL Synthesis
  - Static Timing Analysis(STA)
  - Design for Testability(DFT)
  - Floorplanning
  - Placement
  - Clock Tree Synthesis(CTS)
  - Routing
  - GDSII Streaming
 
 All the steps are further discussed in details in the repository.
  
# About Google SkyWater PDK
  Google and SkyWater Technology Foundry in collaboration have released a completely open-source Process Design Kit(PDK) in May, 2020. The current release target to a SKY130 (i.e. 130 nm) process node is available as [SkyWater Open Source PDK](https://github.com/google/skywater-pdk). The PDK provides Physical VLSI Designer with a wide range of flexibility in design choices. All the designs and simulations listed in this repository are carried out using the same SkyWater Open Source PDK.

  
# Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
 
 ## Introduction To RISC-V
   RISC-V is a new ISA that's available under open, free and non-restrictive licences. RISC-V ISA delivers a new level of free, extensible software and hardware freedom on architecture.
   A brief design for RISC-V core can be refered [here](https://github.com/ShonTaware/RISC-V_Core_4_Stage)
   
 ## SoC Design and OpenLANE
 ### Open-Source PDK Directory Structure
   All the PDK are listed under the `pdks/` directory. Along with the `Sky130A` we are using some other open-source PDKs and other related files are also available in the directory.  
    
   <img src="Day 1/design file.png">
  

      
 ## Open-Source EDA Tools
 ### OpenLANE Initialization and design preperation
   For invoking OpenLANE in Linux Ubuntu, we should first run the docker everytime we use OpenLANE. This is done by using the following script:
    
    docker  
   
   A custom shell script or commands can be generated to make the task simpler.
   
   - To invoke OpenLANE run the `./flow.tcl` script.
   - OpenLANE supports two modes of operation: interactive and autonomous.
   - To use interactive mode use `-interactive` flag with `./flow.tcl`
   -then we shloud use `packeage require openlane 0.9 ` to  activate the 0.9 version of the openlane
   - then preparing the design by using `prep -design picorv32a ` we can replace the `picorv32a` with any design file we have.
   
   <img src="Day 1/openlane prep.png">  
   <img src="Day 1/merging.png"> 
    
 ### Design Synthesis and Results
   The first step in OpenLANE flow is RTL Synthesis of the design loaded. This is done using the following command.
   
    run_synthesis
   
   <img src="Day 1/synthesis 1.png">
   <img src="Day 1/synthesis 2.png">
   we can see from this report the chip area and the number of every cell have been used in this chip 
   
# Day 2 - Good floorplan vs bad floorplan and introduction to library cells
 ## Chip Floorplanning
   Chip Floorplanning is the arrangement of logical block, library cells, pins on silicon chip. It makes sure that every module has been assigned an appropriate area and aspect ratio, every pin of the module has connection with other modules or periphery of the chip and modules are arranged in a way such that it consumes lesser area on a chip.
   
 ### Utilization Factor and Aspect Ratio
   Utilization Factor is ratio of the area of core used by the standart cells to the total core area.
   Aspect ratio is the ratio between the hight and the width of the core
   
 ### Power Planning
   Power planning is a step in which power grid network is created to distribute power to each part of the design equally. T
 ### Pin Placement
   Pin placement is a important part of floorplanning to maintaine small distances between the cells and I/O and this will help us to keep the delay as possible as we could . 
 
 ### Floorplan using OpenLANE
   Floorplanning in OpenLANE is done using the following command. 
    
    run_floorplan
   
   Successful floorplanning gives a `def` file as output. This file contains the die area and placement of standard cells.
   
   <img src="Day 2/floorplan results.png">
 
 ### Review Floorplan Layout in Magic
   Magic Layout Tool is used for visualizing the layout after floorplan. In order to view floorplan in Magic, following three files are required:
    1. Technology File (`sky130A.tech`)
    2. Merged LEF file (`merged.lef`)
    3. DEF File
    
   <img src="Day 2/floorplanlayout.png">
   <img src="Day 2/floorplanlayout2.png">

 
 ## Placement
 ### Placement and Optimization
   The next step after floorplanning is placement. Placement determines location of each of the components on the die. Placement does not just place the standard cells available in the synthesized netlist. It also optimizes the design, thereby removing any timing violations created due to the relative placement on die.
   
 ### Placement using OpenLANE
   Placement in OpenLANE is done using the following command. 
    
    run_placement
   
   Placement is carried out as an iterative process till the value of overflow converges to 0.
      <img src="Day 2/placementlayout.png">
      <img src="Day 2/placementlayout.png">
   
 
 
# Day 3 - Design library cell using Magic Layout and ngspice characterization
  Every Design is represented by equivalent cell design. All the standard cell designs are available in the Cell Library. A fully custom cell design that meets all rules can be added to the library. To begin with, a CMOS Inverter is designed in Magic Layout Tool and analysis is carried out using NGSPICE tool.
  
 ## CMOS Inverter Design using Magic
  The inverter design is done using Magic Layout Tool. It takes the technology file as an input (`sky130A.tech` in this case). Magic tool provide a very easy to use interface to design various layers of the layout. It also has an in-built DRC check fetaure.
  The snippet below shows a layout for CMOS Inverter with and without design rule violations.
  
   <img src="Day 3/Screenshot 2022-08-07 103924.png">
   <img src="Day 3/layout drc check.png">

  
 ## Extract SPICE Netlist from Standard Cell Layout
  Magic provide to the custumers a way to simulate the designed cells by extracting the netlist and  simulate it with SPICE .
  
  Extraction of SPICE model for a given layout is done in two stages.
  1. Extract the circuit from the layout design.
  
    extract all
  
  2. Convert the extracted circuit to SPICE model.
    
    ext2spice cthresh 0 rthresh 0
    ext2spice
  
  The extracted SPICE model like the first snippet shown below. Some modification are done to the SPICE netlist for the purpose of simulations, which is shown in the second snippet below.
  
 <img src="Day 3/layout extract.png">
 <img src="Day 3/teminal1.png">
 the extracted file is sky130_inv.ext 


  
 ## Transient Analysis using NGSPICE
  The SPICE netlist generated in previous step is simulated using the NGSPICE tool. NGSPICE is an open-source mixed-level/mixed-signal electronic spice circuit simulator.
  The command used to invoke NGSPICE is shown below.
  
    ngspice <name-of-SPICE-netlist-file>
    <img src="Day 3/spice run.png">

    
  Following command is used to plot waveform in ngspice tool.
    
    ngspice 1 -> plot Y vs time A
    
    <img src="Day 3/spice plot.png">

  

   
# References
  - RISC-V: https://riscv.org/
  - VLSI System Design: https://www.vlsisystemdesign.com/
  - OpenLANE: https://github.com/The-OpenROAD-Project/OpenLane

# Acknowledgement
  - [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
  - [Nickson Jose](https://github.com/nickson-jose)
  - [Praharsha Mahurkar](https://github.com/praharshapm)
  - Akurathi Radhika
