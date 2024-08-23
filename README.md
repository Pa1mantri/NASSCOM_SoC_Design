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

**Implementation of Placement**

```run_placement```

Placement determines the arrangement of standard cells within the chip. It refers to the process of positioning standard cells, macros, and I/O pads on the ASIC layout.

Both floor planning and placement do not add any extra logic to the synthesized netlist.

![Screenshot 2024-08-21 171538](https://github.com/user-attachments/assets/980558cb-0527-42ca-9c1b-b506f42daa5b)

![Screenshot 2024-08-21 171717](https://github.com/user-attachments/assets/578554fd-3d65-43e2-8936-2fc92f2dc3c9)

## Day-3

### Characterization of an inverter cell 

In this case, Characterization of an inverter starts from the .mag file, not from scratch. From this inverter layout, a SPICE deck is generated, and then the characterization of the cell is performed. Characterization involves simulating the inverter's performance under various conditions using ``ngspice``. We calculate Rise transition delay, Fall transition delay, rise cell delay, fall cell delay. Create a LEF file from the layout which can be used to include this cell in any larger design.

![Screenshot 2024-08-21 181855](https://github.com/user-attachments/assets/6de4099a-d65d-4c01-abe3-c4c6bef50e42)

Opening the ``sky130_inv.mag`` file in magic

![Screenshot 2024-08-21 182102](https://github.com/user-attachments/assets/9ef7c627-d4ee-4d70-83fc-d6021fd008f8)

extracting spice from the mag file using the commands 

```
extract all
ext2spice cthresh 0 rthresh 0
ext2spice
```
![Screenshot 2024-08-21 182406](https://github.com/user-attachments/assets/6f24dab9-30ba-4923-9a03-d224e84d8f82)

Generated ```sky130_inv.spice``` file

![Screenshot 2024-08-21 182453](https://github.com/user-attachments/assets/b4b2987d-549e-4189-a754-6e7720527778)

Make some changes to the spice desk by including Vdd, input voltage (pulse) and trans response

![Screenshot 2024-08-21 193301](https://github.com/user-attachments/assets/598a724a-d278-4446-80ed-5d7474800974)

Running the modified spice file using ngspice

![Screenshot 2024-08-21 194223](https://github.com/user-attachments/assets/e2d0a45e-f9b8-4bbb-b440-740e303b6b4f)

Plotting different values from the graph

![Screenshot 2024-08-22 101910](https://github.com/user-attachments/assets/830c4023-4ee2-4377-a796-f78facc1e5b6)

Rise time: Time taken for the output waveform to transition from 20% to 80% of its maximum value.

x0 = 2.164  y0 = 0.659

x1 = 2.205 y1 = 2.639

``rise time = x1 - x0 = 0.041ns``

Fall time:  Time taken for the output waveform to transition from 80% to 20% of its maximum value.

x0 = 4.040  y0 = 2.64

x1 = 4.068 y1 = 0.660

``fall time = x1 - x0 = 0.028ns``

Rise cell delay: The time taken for a 50% transition at the output (0 to 1) corresponding to a 50% transition at the input (1 to 0)

x0 = 2.186  y0 = 1.65

x1 = 2.151 y1 = 1.65

``Propogation delay = 0.03ns``

Fall cell delay : The time taken for a 50% transition at the output (1 to 0) corresponding to a 50% transition at the input (0 to 1)

x0 = 4.054  y0 = 1.65

x1 = 4.05 y1 = 1.65

``propogation delay = 0.004ns``

![Screenshot 2024-08-22 110502](https://github.com/user-attachments/assets/b5c9acd8-9ff0-48ce-861c-0aef949ef2fb)

writing lef file ``sky130_INV_PA1.lef``

![Screenshot 2024-08-22 112826](https://github.com/user-attachments/assets/4b9e126f-3b90-4a7a-8d62-7fb41658d5fd)

## Day-4

To include the customized LEF file ```sy130_INV_PA1.lef``` into the ```picorv32a``` design, the config.tcl file must be updated. The LEF file and the corresponding libraries also need to be included in the config file.

```
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
```
These commands need to be added to the flow to incorporate the custom inverter cell into the ASIC flow during synthesis.

The cell sky130_INV_PA1 has been used 1554 times in the synthesis.

![Screenshot 2024-08-22 141656](https://github.com/user-attachments/assets/f163657c-d92b-4755-aa37-00b40b2327ec)

![Screenshot 2024-08-22 150404](https://github.com/user-attachments/assets/ce1eef3f-efc4-4d40-8351-c19e912ec219)

During the synthesis, the chip area of the module is 147712.9184

tns = -711.59 wns = -23.89

![Screenshot 2024-08-22 141801](https://github.com/user-attachments/assets/5fc17d74-1d37-48df-879b-114b23697118)

By changing the ``SYNTH_STRATEGY`` from AREA to DELAY, the TNS and WNS values changed to zero, but this resulted in an increased chip area of 181730.544.

commands to change the swithces

```
set ::$env(SYNTH_STRATAGEY) "DELAY 3"
echo $::env(SYNTH_STRATAGEY) - To print the value
```

Similar switches to optimize the netlist at synthesis stage

```
SYNTH_MAX_FANOUT
SYNTH_SIZING
SYNTH_BUFFERING
SYNTH_DRIVING_CELL
```
![Screenshot 2024-08-22 154329](https://github.com/user-attachments/assets/68a98f81-68c5-41b1-ad3a-e124ece88f7e)

![Screenshot 2024-08-22 154314](https://github.com/user-attachments/assets/9d287e41-f823-4577-b3c6-06fd0dfd8ae2)

Running floorplan 



