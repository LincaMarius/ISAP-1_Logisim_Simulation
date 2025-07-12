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

For this purpose we must use the detailed block diagram of the SAP-1 computer which is presented in the following figure.

![ Figure 3 ](/Pictures/Figure3.png)

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

This information can be seen in figure 3.

The implementation of the Clock and Reset Block in Logisim software is shown in the following figure

![ Figure 4 ](/Pictures/Figure4.png)

### Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter Counting - increment Program Counter content
- CLK – Clock - clock signal that ensures synchronism in the operation of the computer
- CLR – Clear - reset signal that initializes the Program Counter to zero
- EP – Program Counter Enable - output activation for putting data from the Program Counter on the Bus
- DOUT – Data Output – connects to the Bus

The implementation of the Program Counter Block in Logisim is shown in the following figure:

![ Figure 5 ](/Pictures/Figure5.png)

The Program Counter is made with 4 JK Flip-Flops and increments the stored numerical value if the CP control signal is active and the positive edge of the clock signal occurs.

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

The implementation of the Address Register Block in Logisim is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The Address Register is implemented with a 4-bit register and stores the 4 least significant bits of data received from the Bus when the LAR control signal is active and the rising edge of the clock signal occurs.

This action occurs for any instruction during [ microstep T1 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

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

![ Figure 7 ](/Pictures/Figure7.png)

The Instruction Register is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LI is active and the rising edge of the clock signal occurs.

This action occurs for any instruction during [ microstep T3 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure3.png).

The lower nibble stored in the Instruction Register is presented on the bus output if the EI control signal is high and represents the parameter of the instruction being executed. The output is tri-stated using a 4-bit buffer and is active only when the EI control signal is high.

The value of this parameter of the instruction being executed represents a memory address and is loaded into the Address Register at [ micro-step T4 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure4.png) of all instructions that have a parameter.

The PROBE pin are used to view the contents of the block regardless of whether the output is in tri-state mode.

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

![ Figure 8 ](/Pictures/Figure8.png)

The Accumulator Register is implemented with an 8-bit register that stores the data present at the input from the Bus when the LA control signal is high and the rising edge of the clock signal occurs.

This action occurs for all instructions involving the Accumulator such as: LDA, ADD, SUB and can be active during [ micro-step T5 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure4.png) or [ micro-step T6 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure5.png).

The output is provided by a three-state buffer and is active only when the EA control signal is high.

This action occurs when the OUT instruction is executed and is active for the duration of [ micro-step T4 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure7.png).

The PROBE pin is used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the Bus.

### Register B implementation
Register B has the following input, output and control signals:
- LB - loading data from the Bus into Register B
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the Bus
- DOUT - Data Output – connects to the Bus
- ALUB – the contents of Register B connect to the Logical and Arithmetic Unit

The implementation of the Register B block in Logisim is shown in the following figure:

![ Figure 9 ](/Pictures/Figure9.png)

Register B is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LB is active and the rising edge of the clock signal occurs.

This action occurs for any arithmetic instruction with are ADD and SUB during [ micro-step T5 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure5.png).

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

The implementation of the Arithmetic and Logical Unit in Logisim is shown in the following figure:

![ Figure 10 ](/Pictures/Figure10.png)

The Arithmetic and Logic Unit has as its main element an 8-bit Adder. The addition calculation is performed continuously.

This means that any change to any ALUA or ALUB input data immediately causes the output result to change. In reality there is a propagation delay of about 25ns for the 74LS83 chip.

The Subtraction operation is done by inverting the value of operand B (one's complement) and adds one by applying the SU signal to the Carry In input (two's complement).

The output to the Bus is provided by a three-state buffer and is active only when the EU control signal is high.

This action occurs for any arithmetic instruction with are ADD and SUB during [ micro-step T6 ](https://github.com/LincaMarius/ISAP-1_Instruction_Set/blob/main/Pictures/Figure5.png).

The W pin has the role of displaying when the Result is writing to the Bus.

### Implementation of the RAM Memory module
[ Figure 1 ]( https://github.com/LincaMarius/ISAP-1_Logisim_Simulation/blob/main/Pictures/Figure1.png) shows the structure of the SAP-1 computer seen from a modular point of view.

Figure 11 shows the Memory block of the SAP-1 computer and its connection to the system.

![ Figure 11 ](/Pictures/Figure11.png)

Thus, the memory of the SAP-1 computer in Run mode is used as a ROM memory. Control input #CE if high disables the output which is three-state, and if low at the output the data from the selected address is presented.

The RAM Memory module has the following inputs, outputs and control signals:
- #CE – Chip Enable - activates the memory chip
- #W – Write - determines the writing of data in memory
- #OE – Output Enable – data output to the Bus is activated
- Data – connects to the bus
- Address – connects to the address bus

It is noted that the RAM memory used, type 74LS83, does not have a control pin #OE for output inhibition, this function is taken over by the pin #CE.

After studying the Block Diagram of the SAP-1 computer, I determined that from a functional point of view the memory is used only in read mode. Therefore, it can be considered a ROM memory behavior.

This aspect is shown in the following figure.

![ Figure 12 ](/Pictures/Figure12.png)

I came to the conclusion that I can use a ROM memory model provided by the Logisim program.

The ROM memory model implemented in the Logisim program has the *sel* input which, if high, presents the data at the selected address at the output. When the *sel* pin is low, the output goes into high impedance mode.

This behavior allows the PM control signal to be directly connected to the *sel* pin of the ROM memory in the Logisim program.

![ Figure 13 ](/Pictures/Figure13.png)

### Output Module Implementation
Figure 14 shows the Block Diagram of the Output Module of the SAP-1 computer and the ISAP-1 computer.

![ Figure 14 ](/Pictures/Figure14.png)

The Output module has the following input, output and control signals:
- I/O - loading data from the Bus into the Output Module, the original name is LO
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus

The implementation of the Output Module in Logisim is shown in the following figure:

![ Figure 15 ](/Pictures/Figure15.png)

### Control Block Implementation
The Control Block is very important for the operation of any system and must be implemented with great care because the system's performance depends on it.

The design and implementation of the Control Block is presented in this repository: \
https://github.com/LincaMarius/ISAP-1_Control_Unit

### Implementation of the ISAP-1 Computer version 1.0 in Logisim
The functional implementation of the ISAP-1 Computer version 1 in the Logisim software is presented in the file [ ISAP-1_ver1_0.circ ](/Logisim/ISAP-1_ver1_0.circ).

An image of the computer implemented in the Logisim program after it has finished running the [ PRPGRAM_1 ]( https://github.com/LincaMarius/ISAP-1_Programs?tab=readme-ov-file#program-1) test program present in the ROM memory is:

![ Figure 16 ](/Pictures/Figure16.png)

This program is:

| Address | Instruction | Hexa |Assembly |    Action    |      Rezult       |
|---------|-------------|------|---------|--------------|-------------------|
|  0000	  |  0000 0010  |  04  |  LDA 4  | A <- [4]	    | A = 44h           |
|  0001   |	 0001 0001  |  11  |  ADD 1  | A <- A + [1]	| A = 44 + 11 = 55h |
|  0002   |	 0010 0000  |  20  |  SUB 0  | A <- A – [0]	| A = 55 – 4 = 51h  |
|  0003   |	 0011 0000  |  30  |  NOP	 |	    -		|       -	        |
|  0004   |	 0100 0100  |  44  |  NOP	 |	    -		|       -	        |
|  0005   |	 0101 0000  |  50  |  NOP	 |	    -		|       -	        |
|  0006   |	 0110 0000  |  60  |  NOP	 |	    -		|       -	        |
|  0007   |	 0111 0000  |  70  |  NOP	 |	    -		|       -	        |
|  0008   |	 1000 0000  |  80  |  NOP	 |	    -		|       -	        |
|  0009   |	 1001 0000  |  90  |  NOP	 |	    -		|       -	        |
|  000A   |	 1010 0000  |  A0  |  NOP	 |	    -		|       -	        |
|  000B   |	 1011 0000  |  B0  |  NOP	 |	    -		|       -	        |
|  000C   |	 1100 0000  |  C0  |  NOP	 |	    -		|       -	        |
|  000D   |	 1101 0000  |  D0  |  NOP	 |	    -		|       -	        |
|  000E   |	 1110 0000  |  E0  |  OUT	 | OUT <- A		| OUT = 51h	        |
|  000F   |	 1111 0000  |  F0  |  HLT	 | HLT		    | HLT	            |

The contents of the ROM memory are: \
[ ROM1 ](https://github.com/LincaMarius/ISAP-1_Programs/blob/main/ROMS/ROM1)

## ISAP-1 Model A Version 1.1
In Version 1.1, an improvement is made to the ISAP-1 computer by implementing the Variable Machine Cycle.

The Instruction Set remains unchanged but the Control Block implementation changes.

In the book on page 163 the authors present a method of improving the SAP-1 Computer by implementing the Variable Machine Cycle.

The schematic is shown in Figure 10-17 and consists of 5 inverters and a 12-input NAND gate that generates the #NOP signal when the Control Block output has the NOP instruction encoded in Hexadecimal as 3E3h and a two-input AND gate that resets the Ring Counter when the #NOP or #CLR signal is low. 

We can note that the SAP-1 Computer schematic is not modified to implement this functionality, only two logic gates are added.

The design and implementation of the Control Block using the original schematic that implements the Variable Machine Cycle are presented in this repository: \
https://github.com/LincaMarius/ISAP-1_Control_Unit#isap-1-model-a-version-11

The functional implementation of the ISAP-1 Computer version 1.1 in the Logisim software is presented in the file: \
[ ISAP-1_modelA_ver1_1.circ ](/Logisim/ISAP-1_modelA_ver1_1.circ)

An image of the computer implemented in the Logisim program after it has finished running the PRPOGRAM_1 test program present in the ROM memory is:

![ Figure 15 ](/Pictures/Figure15.png)

The contents of the ROM memory are: \
[ ROM1 ](https://github.com/LincaMarius/ISAP-1_Programs/blob/main/ROMS/ROM1)

## ISAP-1 Model B version 1.0
Revision B, I want to be an implementation of the SAP-1 computer that has the Control Unit made using microprogramming and storing microinstructions in ROM memory.

I want version 1 to be an implementation of the SAP-1 Computer that is as close to the original as possible. This will be the reference, the starting point for creating a better version.

The Block Diagram remains unchanged for Model B Version 1.0 of the SAP-1 and ISAP-1 Computers compared to Model A Version 1.0.

The Instruction Set remains unchanged for Model B Version 1.0 of the SAP-1 and ISAP-1 Computers compared to Model A Version 1.0.

This implies keeping all functional blocks implemented so far unchanged.

Only the Control Block needs to be modified from the implementation with logic gates to the implementation using ROM memory for storing microinstructions as presented in the book on page 161 in subchapter 10-8.

The design and implementation of the Control Block using a ROM memory are presented in this repository: \
https://github.com/LincaMarius/ISAP-1_Control_Unit#isap-1-model-b-version-10

The operation of the ISAP-1 computer, which has a Control Unit implemented using ROM memories, was tested by running the following programs: \
https://github.com/LincaMarius/ISAP-1_Programs

The functional implementation of the ISAP-1 Computer Model B version 1.0 in the Logisim program is presented in the file: \
[ISAP-1_modelB_ver1_0.circ](/Logisim/ISAP-1_modelB_ver1_0.circ).

## ISAP-1 Model B Version 1.1
In Version 1.1, an improvement is made to the ISAP-1 computer by implementing the Variable Machine Cycle.

The Instruction Set remains unchanged but the Control Block implementation changes.

In the book on page 163 the authors present a method of improving the SAP-1 Computer by implementing the Variable Machine Cycle.

The schematic is shown in Figure 10-17 and consists of 5 inverters and a 12-input NAND gate that generates the #NOP signal when the Control Block output has the NOP instruction encoded in Hexadecimal as 3E3h and a two-input AND gate that resets the Ring Counter when the #NOP or #CLR signal is low. 

We can note that the SAP-1 Computer schematic is not modified to implement this functionality, only two logic gates are added.

The design and implementation of the Control Block using the original schematic that implements the Variable Machine Cycle are presented in this repository: \
https://github.com/LincaMarius/ISAP-1_Control_Unit#isap-1-model-b-version-11

The functional implementation of the ISAP-1 Model B Computer Version 1.1 in Logisim software is presented in the file: \
[ ISAP-1_modelB_ver1_1.circ ](/Logisim/ISAP-1_modelB_ver1_1.circ)
