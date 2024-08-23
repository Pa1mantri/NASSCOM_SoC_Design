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

1

For a software program to run on hardware, it passes through many interfaces and applications, such as system software, which handles memory allocation and low-level system functions, as well as a compiler that converts the software code into assembly instructions, and an assembler that translates these assembly instructions into machine code.

2

The interface that implements the instruction set architecture is the RTL Verilog code. This RTL code is converted into a layout that executes the program.







