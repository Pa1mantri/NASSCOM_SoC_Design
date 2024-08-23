# Digital VLSI SOC Design and Planning

In this workshop, we will see what are all the steps involved in making an Application Specific Intergrated Circuit(ASIC) from RTL to GDSII.

## Day-1
The field we are exploring involves the chips inside boards such as the Arduino board and the VSDSquadron. The image below shows Arduino board.

![arduino](https://github.com/user-attachments/assets/6deca6bb-2499-4791-a94f-bc36ee51062d)

The block diagram of the board, along with all its peripherals, can be found in the image below.

![block_arduino](https://github.com/user-attachments/assets/ed52e87c-9390-423f-9469-cc4a774f7556)

Since we are dealing with only the central chip, if we open it, it will reveal the package with connections to the chip inside.

![package](https://github.com/user-attachments/assets/64c2600f-279e-452e-bbd4-03798da8e242)

Connections to and from the outside world occur through the I/O pads. The entire logic (netlist) is placed inside the core. The logic, along with the I/O pads, is referred to as the DIE. 

![Die](https://github.com/user-attachments/assets/5038674b-0dc9-49f2-83d4-9feb51397a17)

The core is filled with various IPs and macros.

![ip n macro](https://github.com/user-attachments/assets/208945f0-b102-4e37-969c-5f365496d07e)

### Introduction to RISC-V

RISC-V is an instruction set architecture which is an interface between software(C, python, java programs) and hardware(Layout).

![ISA_interface](https://github.com/user-attachments/assets/fc8b8b4c-89c1-4c75-9756-6fd4920ce6a1)

For a software program to run on hardware, it passes through many interfaces and applications, such as system software, which handles memory allocation and low-level system functions, as well as a compiler that converts the software code into assembly instructions, and an assembler that translates these assembly instructions into machine code.

![2](https://github.com/user-attachments/assets/efe6c0dd-826f-4952-8ed4-24596e42b9af)

The interface that implements the instruction set architecture is the RTL Verilog code. This RTL code is converted into a layout that executes the program.

![3](https://github.com/user-attachments/assets/ce37d9de-4ee5-4a37-8837-92631485d817)

### OpenLane

OpenLane is a flow developed by efabless for opensource skywater sky130 PDK. Three major requirements for an ASIC design are

1. RTL Designs
2. EDA Tools
3. PDK

![Screenshot 2024-08-23 134031](https://github.com/user-attachments/assets/fac9fd0b-1f00-467d-9fa1-c6c284eceac2)

RTL Designs describes the flow of data between registers and the operations performed on the data.

Electronic Design Automation tools help design and verify whether the RTL code meets the specifications and functionality of the system.

A Process Design Kit (PDK) serves as an interface between the designer and the foundry. It contains all the necessary information and files required by the designer to design integrated circuits for a specific fabrication process offered by the foundry. The PDK includes various files and data that enable effective communication and collaboration between the designer and the foundry. These files help the designer understand the foundry's process, design rules, and available components, allowing them to create designs that are optimized for the foundry's manufacturing capabilities.

The steps in the RTL to GDSII flow include 

![Screenshot 2024-08-23 134057](https://github.com/user-attachments/assets/587edb1f-b778-4916-a8df-b2502fa556ab)

**Synthesis**: Synthesis is the process of converting RTL code into a netlist, which comprises standard cell libraries and their connections.

*Opensource Tools used* : yosys,ABC

![Screenshot 2024-08-23 134113](https://github.com/user-attachments/assets/91153eaa-dd25-4111-bb77-45edefd41c6c)

**Floorplan**: This is the process of defining the layout of the ASIC, including the placement of standard cells, macros, and I/O pads. The height and width of the core are decided in this step. Ideally, the power distribution network is also established at this stage. In OpenLane, this occurs after CTS. All preplaced cells are located in the floor plan stage.

![Screenshot 2024-08-23 134151](https://github.com/user-attachments/assets/ac4dbf51-efb4-483d-8ba1-8d02df4b8931)

**Placement**: refers to the process of positioning standard cells in rows on the ASIC. This step is essential for ensuring that the design meets performance, area, and power requirements. Placement occurs in two stages: Global Placement and Detailed Placement.

![Screenshot 2024-08-23 134222](https://github.com/user-attachments/assets/ff8d3af2-47ab-40b9-a5c9-ea608c9b390f)

**Clock Tree Synthesis**:  After CTS, timing checks are performed to ensure setup and hold constraints are met. If timing is not met, previous steps like placement may need to be revisited and optimized.

![Screenshot 2024-08-23 134235](https://github.com/user-attachments/assets/efadf71d-c4b3-4eee-a01f-5a263b18419c)

**Routing**: Signal routing happens after clock routing in CTS stage. Routing is divided into Global routing and Detailed routing.

![Screenshot 2024-08-23 134257](https://github.com/user-attachments/assets/619c03f9-ee0b-4f12-89aa-776ae9328d09)

**Signoff**: There should not be any DRC or LVS errors for the design to converge.

![Screenshot 2024-08-23 134314](https://github.com/user-attachments/assets/8e8888c3-f85a-4b22-8b91-4ea08602bfb7)

### OpenLane ASIC Flow

![Screenshot 2024-08-23 160230](https://github.com/user-attachments/assets/41856789-9ccb-4801-8719-460c6f65b037)

## RTL to GDSII practical implementation

### Commands to initiate the flow
```
docker
./flow.tcl -interactive
package require openlane
prep -design picorv32a
```
![Screenshot 2024-08-21 144728](https://github.com/user-attachments/assets/04c2e99b-2ecf-471e-a66b-89edd90f3733)

This step will create a folder in runs directory.

![Screenshot 2024-08-21 144926](https://github.com/user-attachments/assets/82e2838a-3e59-4e2d-bbd6-775988dec523)

After ```run_synthesis``` command, it will generate a netlist file ```picorv32a.synthesis.v``` in the results synthesis folder. 

![Screenshot 2024-08-21 151932](https://github.com/user-attachments/assets/8cd350dc-d68e-4fb7-9144-fef2b4eb297c)

Chip area of the module is obtained at the end of the synthesis. Flop ratio is calculated using the statistics obtained from the synthesis step.

Flop ratio = no.of Dff used / total no. of cells

![Screenshot 2024-08-21 150951](https://github.com/user-attachments/assets/d591bffa-cfa7-4f38-b378-29716c152a65)

![Screenshot 2024-08-21 151340](https://github.com/user-attachments/assets/41b1a9b0-807d-46e7-b6b1-589c668db336)

![Screenshot 2024-08-21 150919](https://github.com/user-attachments/assets/6c6b6b26-4498-4828-89b9-e40e56436c7c)

## Day-2

**Implementation of floorplan**

```run_floorplan```

Utilization factor and aspect ratio can be assigned at this stage. If there are any macros, these pre-placed macros are positioned in this step. Decoupling capacitors and pin placements are also handled at this stage.

A ```.def``` file is generated at the end of the floorplan stage. The .def file can be viewed using the Magic tool.

![Screenshot 2024-08-21 164032](https://github.com/user-attachments/assets/9e583500-d709-49eb-9b24-5e2e7022cd18)

Using the ```FP_IO_MODE``` switch set to 0 and 1, one mode sets the I/O pins equidistantly, while the other places them randomly.

![Screenshot 2024-08-21 165022](https://github.com/user-attachments/assets/ea6283ed-cacf-4643-9488-abdd15d84613)

![Screenshot 2024-08-21 171006](https://github.com/user-attachments/assets/e69da08f-3899-4ae4-8380-6de1240f1949)

Vertical and Horizontal pins are set using the switches ```FP_IO_VMETAL``` and ```FP_IO_HMETAL```

![Screenshot 2024-08-21 165716](https://github.com/user-attachments/assets/1bbd7007-752e-47bd-a84a-39eabd391e37)

