This is the process of making the ISAP-1 computer at the simulation level using the Logisim program, which I went through.

# ISAP-1 Computer simulation in Logisim

The improved version of the SAP-1 computer.

This is the process of optimizing the functionality of the SAP-1 computer at the instruction set level, which I went through.

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius/ISAP-1_Logisim

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is a continuation of other projects made by me:
https://github.com/LincaMarius/ISAP-Computer \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set

where we optimized the SAP-1 calculator at the block diagram level and at the instruction level.
I got four block diagrams:

The final block diagram of the CPU part of the system is: 

![ Figure 1 ](/Pictures/Figure1.png)

The final block diagram of the Computer Memory system is:

![ Figure 2 ](/Pictures/Figure2.png)

The final block diagram of the Input system of this computer is:

![ Figure 3 ](/Pictures/Figure3.png)

We will have an input device in the form of a binary keyboard.
Program memory bank register is an input-output device.

The final block diagram of the Output system of this computer is:

![ Figure 4 ](/Pictures/Figure4.png)

The output device is the old Output Register, but modified by having access to the address bus.
Thus, we can control the display format (unsigned, signed, hexadecimal, text), by sending commands to the output device on port 1, and data is sent on port 0.

## The format of the ISAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|

## The main instruction set of the ISAP-1 computer is:

| Mnemonic | Opcode | Hex | Operation                                                     |
|----------|--------|-----|---------------------------------------------------------------|
| LDA      | 0000   | 0h  | Load RAM data into Accumulator                                |
| ADD      | 0001   | 1h  | Add RAM data to Accumulator                                   |
| SUB      | 0010   | 2h  | Substract RAM data from accumulator                           |
| LIL      | 0011   | 3h  | Load immediate value into the lower nibble of the Accumulator |
| LIH      | 0100   | 4h  | Load immediate value into the upper nibble of the Accumulator |
| IN       | 0101   | 5h  | Input data from port p into the Accumulator                   |
| JMP      | 0110   | 6h  | Unconditional jump to address n                               |
| OUT      | 1110   | Eh  | Load Accumulator data into Output Register                    |
| HLT      | 1111   | Fh  | Stop processing                                               |

## Implementation of NOP instruction

The NOP instruction has only the Fetch portion present in all instructions, but has nothing in the execution portion of the instruction. \
Binary form:  1111 0000 \
Operation:  no operation \
No action is performed. This instruction can be used in programs to delay the execution of an action while waiting for a response from slow peripherals. \
Example: NOP

The timing diagram for the LDA instruction is as follows:

![ Figure 5 ](/Pictures/Figure5.png)

We can summarize the value of the time control signals shown in this diagram in the following table:

![ Table 1 ](/Pictures/Table1.png)

Signals represented in Red: are active when data is written to the Data BUS \
Signals represented in Green: are active when reading data from the Data BUS \
Signals shown in Black: their activation has no influence on the Data BUS

<code style="color : red">If we implement the Control Block using a ROM memory, the data in this table will be used to realize its content.</code>

The Boolean equations for the signals that are active when the NOP instruction is executed are:
-	EP = NOP * T1
-	LAR = NOP * T1
-	CP = NOP * T2
-	PM = NOP * T2
-	LI = NOP * T2
-	NEXT = NOP * T3

Since steps T1 and T2 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the instructions as follows:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T2
-	LI = T2
-	NEXT = NOP * T3

As can be seen from the diagram in figure 5 and from the Boolean equations to implement the NOP instruction we need the following functional blocks:
-	Program Counter
-	Address Register
-	Program Memory as ROM Memory
-	Instruction Register
-	Control Unit

## Program Counter implementation
Program Counter has the following input, output and control signals:
- Cp – Program Counter content increment
- Lp - loading data from the bus into the Program Counter
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Program Counter to zero
- Ep – output activation for putting data from the Program Counter on the bus
- Data Input – connects to the bus
- Data Output – connects to the bus

The implementation of the Program Counter block in Logisim is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The PROBE pins are used to view the contents of the block regardless of whether the output is in tri-state mode. \
Pins W and R, are to indicate when this block is writing or reading from the bus.

## Address Register implementation
The Address Register has the following input, output and control signals:
- Lar - load data from the bus in the Address Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- Data Input – connects to the bus
- ABUS – control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 7 ](/Pictures/Figure7.png)

Pin R is to display when this block is reading from the bus.

## Instruction Register implementation
The Instruction Register has the following input, output and control signals:
- Li - loading data from the bus into the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Instruction Register to zero
- They – enable output to put data from the Instruction Register on the bus
- Data Input – connects to the bus
- Data Output – connects to the bus
- INSTR – output where the current instruction is presented to the Control Block

The implementation of the Instruction Register block in Logisim is shown in the following figure:

![ Figure 8 ](/Pictures/Figure8.png)

The PROBE pins are used to view the contents of the block regardless of whether the output is in tri-state mode. \
Pins W and R, are to indicate when this block is writing or reading from the bus. \

The lower nibble from the Instruction Register is presented on the output to the BUS for both the lower nibble and the upper nibble, this allows the implementation of LIL instructions which load an immediate value into the lower nibble of the Accumulator, but also of the LIH instruction which loads an immediate value into the upper nibble of the Accumulator.

## Control Unit Implementation
The Control Unit to control the ISAP-1 computer to execute the NOP instruction has the following input, output and control signals:
- INSTR – entry where the current instruction is presented by the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Control Unit to zero
- INSTR – output where the current instruction is presented to the Control Block
- Lar – control signal that commands the loading of data from the bus into the Address Register
- Cp – control signal that commands the increment of the content of the Program Counter
- Lp - control signal that commands the loading of data from the bus into the Program Counter
- Ep – control signal that commands the activation of the outputs to put the data from the Program Counter on the bus
- Li - control signal that commands the loading of data from the bus into the Instruction Register
- Ei – control signal that commands the activation of the outputs to put the data from the Instruction Register on the bus
- Pm – control signal that commands the activation of the outputs to put data from the ROM Program Memory on the bus

The Boolean equations for all these signals that are active when the NOP instruction is executed are:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T2
-	LI = T2
-	NEXT = NOP * T3
-	LP = 0
-	EI = 0

We will consider all instructions to be NOP, so if an opcode does not have an associated instruction, it will automatically be treated as a NOP instruction.

By doing so I will be able to test the functionality of the computer using the minimum of electronic components. Thus, by reducing the number of components that enter the structure of a system, we reduce the probability of a defect occurring, and in case we have an implementation error, we reduce the number of blocks that must be tested by default, and we reduce the debugging time.

Result:
-	NEXT = T3

The implementation of the Control Unit block in Logisim is shown in the following figure:

![ Figure 9 ](/Pictures/Figure9.png)

The complete schematic of the ISAP-1 computer that correctly executes the NOP instruction is shown in the following figure:

![ Figure 10 ](/Pictures/Figure10.png)

The red LED indicates that the respective block is writing to the data bus
The green LED indicates that the respective block is reading data from the data bus.

The system has been tested and is working properly.

The simulation in the Logisim program is in the file:
[ ISAP-1_v1.circ ](/ISAP-1_v1.circ)