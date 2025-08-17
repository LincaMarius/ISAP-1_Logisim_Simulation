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

## ISAP-1 computer version 0.1
The Structure of the ISAP-1 computer version 0.1 is:

![ Figure 26 ](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure26.png)

The Block Diagram of the Central Processing Unit of the ISAP-1 computer version 0.1 is:

![ Figure 14 ](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure14.png)

The Block Diagram of the Control Unit version 0.1 is:

![ Figure 15 ](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure15.png)

In order to simulate the SAP-1 computer using the Logisim software, we must design each individual module that makes up the SAP-1 computer.

The first block to be implemented is Clock and Reset

### Clock and Reset Implementation
The Clock and Reset block has the following input-output signals:
- CLK – clock signal that ensures synchronism in the operation of the Computer
- CLR – computer reset signal that initializes the Program Counter, Instruction Register and Control Unit
- HLT – blocks the clock signal, thus the execution of any future instruction is stopped

We have three control buttons: \
S5 – Reset button \
S6 – Program Step button \
S7 – Mode selection switch: Manual / Auto

This information can be seen in [Figure 11](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure11.png), [Figure 24](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure24.png) and [Figure 25](https://github.com/LincaMarius/ISAP-1_Block_Diagram/blob/main/Pictures/Figure25.png).

The implementation of the Clock and Reset Block in Logisim software is shown in the following figure

![ Figure 37 ](/Pictures/Figure37.png)

### Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter Counting - increment Program Counter content
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- CLR – Clear - reset signal that initializes the Program Counter to zero
- EP – Program Counter Enable - output activation for putting data from the Program Counter on the Bus
- DOUT – Data Output – connects to the Bus

The implementation of the Program Counter Block in Logisim is shown in the following figure:

![ Figure 38 ](/Pictures/Figure38.png)

The Program Counter is made with 4 JK Flip-Flops and increments the stored numerical value if the CP control signal is active and the positive edge of the clock signal occurs.

This action occurs for any instruction during [microstep T2](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure1.png).

The output of this register is connected to the 4 least significant bits of the Bus through a 4-bit Tre-state Buffer. The other 4 most significant bits are not connected and because we are using TTL logic chips will appear to be high. The output is active only when the EP control signal is high.

This action occurs for any instruction during [ microstep T1 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure1.png).

The PROBE pins are used to view the contents of the Program Counter regardless of whether the output is in tri-state mode.

The W pin is used to display when this block is writing to the Bus.

### Address Register implementation
The Address Register has the following input, output and control signals:
- LAR – Load Address Register - loads data from the bus into the Address Register
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- DIN – Data Input - connects to the bus
- ABUS – Address Bus - control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 39 ](/Pictures/Figure39.png)

The Address Register is implemented with a 4-bit register and stores the 4 least significant bits of data received from the Bus when the LAR control signal is active and the rising edge of the clock signal occurs.

This action occurs for any instruction during [ microstep T1 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure1.png).

The output of this register is connected to 4 bits of the Address Bus. The output is permanently active.

Pin R is to display when this block is reading from the Bus.

### Instruction Register implementation
The Instruction Register has the following input, output and control signals:
- LI - loading data from the Bus into the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Instruction Register to zero
- EI – enable output to put data from the Instruction Register on the Bus
- DIN - Data Input – connects to the Bus
- DOUT - Data Output – connects to the Bus
- INSTR – output where the current instruction is presented to the Control Block

The implementation of the Instruction Register block in Logisim is shown in the following figure:

![ Figure 40 ](/Pictures/Figure40.png)

The Instruction Register is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LI is active and the rising edge of the clock signal occurs.

This action occurs for any instruction during [ microstep T3 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure1.png).

The lower nibble stored in the Instruction Register is presented on the bus output if the EI control signal is high and represents the parameter of the instruction being executed. The output is tri-stated using a 4-bit buffer and is active only when the EI control signal is high.

The value of this parameter of the instruction being executed represents a memory address and is loaded into the Address Register at [ micro-step T4 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure4.png) of all instructions that have a parameter.

The PROBE pin is used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the Bus.

The INSTR pin presents the current instruction to the Control Block where it is decoded.

### Implementation of the Accumulator Register
The Accumulator register has the following input, output and control signals:
- LA – Load Accumulator - load data from the Bus into the Accumulator Register
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- EA – Enable Accumulator - output control for putting data from the Accumulator Register on the Bus
- DIN - Data Input - connects to the Bus
- DOUT - Data Output - connects to the Bus
- ALUA - the contents of the Accumulator are connected to the Arithmetic and Logic Unit

The implementation of the Accumulator Register block in Logisim is shown in the following figure:

![ Figure 41 ](/Pictures/Figure41.png)

The Accumulator Register is implemented with an 8-bit register that stores the data present at the input from the Bus when the LA control signal is high and the rising edge of the clock signal occurs.

This action occurs for all instructions involving the Accumulator such as: LDA, ADD, SUB and can be active during 
[ micro-step T5 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure2.png) or 
[ micro-step T6 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The output is provided by a three-state buffer and is active only when the EA control signal is high.

This action occurs when the OUT instruction is executed and is active for the duration of [ micro-step T4 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure7.png).

The PROBE pin is used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the bus.

### Register B implementation
Register B has the following input, output and control signals:
- LB - loading data from the Bus into Register B
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the Bus
- DOUT - Data Output – connects to the Bus
- ALUB – the contents of Register B connect to the Logical and Arithmetic Unit

The implementation of the Register B block in Logisim is shown in the following figure:

![ Figure 42 ](/Pictures/Figure42.png)

Register B is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LB is active and the rising edge of the clock signal occurs.

This action occurs for any arithmetic instruction with are ADD and SUB during [ micro-step T5 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The output is connected to the Logic and Arithmetic Unit permanently.

Pin R is to display when this block is reading from the bus.

### Adder/Subtractor Implementation
The SAP-1 computer does not have an Arithmetic and Logical Unit because the Logical Unit is missing. So we only have an Arithmetic Unit.

In the following we used the ALU notation for simplicity, even though we only have one Adder/Subtractor.

The Arithmetic Unit has the following input, output and control signals:
- ALUA – the contents of the Accumulator Register are connected to the Arithmetic Unit as operand A
- ALUB – the contents of Register B are connected to the Arithmetic Unit as operand B
- SU - Subtraction – control signal that activates subtraction operation to be performed if it is high
- EU - control signal that commands the activation of the outputs to put on the Bus the Result of the arithmetic operation performed
- DOUT - Data Output – connects to the bus

The implementation of the Arithmetic Unit in Logisim is shown in the following figure:

![ Figure 43 ](/Pictures/Figure43.png)

The Arithmetic and Logic Unit has as its main element an 8-bit Adder. The addition calculation is performed continuously.

This means that any change to any ALUA or ALUB input data immediately causes the output result to change. In reality there is a propagation delay of about 25ns for the 74LS83 chip.

The Subtraction operation is done by inverting the value of operand B (one's complement) and adds one by applying the SU signal to the Carry In input (two's complement).

The output to the Bus is provided by a three-state buffer and is active only when the EU control signal is high.

This action occurs for any arithmetic instruction with are ADD and SUB during [ micro-step T6 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The W pin has the role of displaying when the Result is writing to the bus.

### Control Block Implementation
The Control Block is very important for the operation of any system and must be implemented with great care because the system's performance depends on it.

The design and implementation of the Control Block is presented in this repository:
https://github.com/LincaMarius/ISAP-1_Control_Unit

### Implementation of the ISAP-1 Computer version 0.1 in Logisim
The functional implementation of the ISAP-1 Computer version 1 in the Logisim program is presented in the file [ ISAP-1_ver0_1.circ ](/Logisim/ISAP-1_ver0_1.circ).

An image of the computer implemented in the Logisim program while running is shown in the following figure:

![ Figure 31 ](/Pictures/Figure31.png)
