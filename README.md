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

RTL Designs describes the flow of data between registers and the operations performed on the data.
Electronic Design Automation tools help design and verify whether the RTL code meets the specifications and functionality of the system.
A Process Design Kit (PDK) serves as an interface between the designer and the foundry. It contains all the necessary information and files required by the designer to design integrated circuits for a specific fabrication process offered by the foundry. The PDK includes various files and data that enable effective communication and collaboration between the designer and the foundry. These files help the designer understand the foundry's process, design rules, and available components, allowing them to create designs that are optimized for the foundry's manufacturing capabilities.
