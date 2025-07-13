# ISAP-1 Computer simulation in Logisim
The improved version of the SAP-1 computer.

This is the process of making the ISAP-1 computer at the simulation level using the Logisim software, presented step by step.

By: Lincă Marius Gheorghe

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is part of a project made by me:
https://github.com/LincaMarius/ISAP-Computer

where I optimized the SAP-1 computer to create the ISAP-1 computer.

## ISAP-1 version 0.1
The Structure of the ISAP-1 computer version 0.1 is:

![ Figure 20 ](/Pictures/Figure20.png)

The Block Diagram of the Central Processing Unit of the ISAP-1 computer version 0.1 is:

![ Figure 21 ](/Pictures/Figure21.png)

The Block Diagram of the SAP-1 computer version 0.1 is:

![ Figure 22 ](/Pictures/Figure22.png)

The Block Diagram of the Control Unit version 0.1 is:

![ Figure 23 ](/Pictures/Figure23.png)

In order to simulate the SAP-1 computer using the Logisim software, we must design each individual module that makes up the SAP-1 computer.

The first block to be implemented is Clock and Reset

### Clock and Reset Implementation
The Clock and Reset block has the following input-output signals:
- CLK – clock signal that ensures synchronism in the operation of the Computer
- CLR – computer reset signal that initializes the Program Counter, Instruction Register and Control Unit

We have three control buttons: \
S5 – Reset button \
S6 – Program Step button \
S7 – Mode selection switch: Manual / Auto

This information can be seen in figure 3.

The implementation of the Clock and Reset Block in Logisim software is shown in the following figure

![ Figure 27 ](/Pictures/Figure27.png)

### Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter Counting - increment Program Counter content
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- CLR – Clear - reset signal that initializes the Program Counter to zero
- Ep – Program Counter Enable - output activation for putting data from the Program Counter on the Bus
- DOUT – Data Output – connects to the Bus

The implementation of the Program Counter block in Logisim is shown in the following figure:

![ Figure 28 ](/Pictures/Figure28.png)

The Program Counter is made with 4 JK flip-flops and increments the stored numerical value if the CP control signal is active and the positive edge of the clock signal occurs.

This action occurs for any instruction during [ microstep T2 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The output of this register is connected to the 4 least significant bits of the Bus through a 4-bit 3-state Buffer. The other 4 most significant bits are not connected and because we are using TTL logic circuits will appear to be high. The output is active only when the EP control signal is high.

This action occurs for any instruction during [ microstep T1 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The PROBE pins are used to view the contents of the Program Counter regardless of whether the output is in tri-state mode.

The W pin is used to display when this block is writing to the Bus.

### Address Register implementation
The Address Register has the following input, output and control signals:
- LAR – Load Address Register - loads data from the bus into the Address Register
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- DIN – Data Input - connects to the bus
- ABUS – Address Bus - control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 29 ](/Pictures/Figure29.png)
