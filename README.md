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
| STA      | 0111   | 7H  | Store Accumulator data into RAM                               |
| OUT      | 1110   | Eh  | Load Accumulator data into Output Register                    |
| HLT      | 1111   | Fh  | Stop processing                                               |

## Implementation of NOP instruction

The NOP instruction has only the Fetch portion present in all instructions, but has nothing in the execution portion of the instruction. \
Binary form:  xxxx \
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

Since steps T1 and T2 are present and identical in any instruction we can say that they are independent of the executed instruction so we can rewrite the instructions as follows:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T2
-	LI = T2

As can be seen from the diagram in figure 5 and from the Boolean equations to implement the NOP instruction we need the following functional blocks:
-	Program Counter
-	Address Register
-	Program Memory as ROM Memory
-	Instruction Register
-	Control Unit

## Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter content increment
- LP - loading data from the bus into the Program Counter
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Program Counter to zero
- EP – output activation for putting data from the Program Counter on the bus
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus

The implementation of the Program Counter block in Logisim is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The PROBE pins are used to view the contents of the block regardless of whether the output is in tri-state mode. \
Pins W and R, are to indicate when this block is writing or reading from the bus.

## Address Register implementation
The Address Register has the following input, output and control signals:
- LAR - load data from the bus in the Address Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus
- ABUS – control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 7 ](/Pictures/Figure7.png)

Pin R is to display when this block is reading from the bus.

## Instruction Register implementation
The Instruction Register has the following input, output and control signals:
- LI - loading data from the bus into the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Instruction Register to zero
- EI – enable output to put data from the Instruction Register on the bus
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus
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
- CTRL – output where the control signals are presented

The Boolean equations for all these signals that are active when the NOP instruction is executed are:
-	EP = T1
-	LAR = T1
-	CP = T2
-	PM = T2
-	LI = T2
-	LP = 0
-	EI = 0

We will consider all instructions to be NOP, so if an opcode does not have an associated instruction, it will automatically be treated as a NOP instruction.

The implementation of the Control Unit block in Logisim is shown in the following figure:

![ Figure 9 ](/Pictures/Figure9.png)

Since all instructions are considered as the NOP instruction, we will have a maximum number of steps equal to 2, the NEXT signal resets the Step Counter in step T3.

The complete schematic of the ISAP-1 computer that correctly executes the NOP instruction is shown in the following figure:

![ Figure 10 ](/Pictures/Figure10.png)

The red LED indicates that the respective block is writing to the data bus
The green LED indicates that the respective block is reading data from the data bus.

The system has been tested and is working properly.

The simulation in the Logisim program is in the file:
[ ISAP-1_v1.circ ](/Logisim/ISAP-1_v1.circ)

The contents of the ROM memory can be anything, and we can even test the system without connecting any device to the CPU buses. We will have a system that continuously increments the address.

The ROM contents in my simulation is: [ ROM1](/Logisim/ROM1)

## LIL instruction implementation
LIL – Load immediate value into lower nibble of Accumulator. \
The full description of the implementation of the LIL instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#lil-instruction--load-immediate-value-into-lower-nible-of-accumulator \

To implement the LIL instruction at the simulation level we must first implement the Accumulator register.

## Implementation of the Accumulator Register
The Accumulator register has the following input, output and control signals:
- CLK – clock signal that ensures synchronism in the operation of the computer
- EA – output activation for putting data from the Accumulator Register on the bus
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus
- ALUA – the contents of the Accumulator connect to the Logical and Arithmetic Unit

Implementation of the LIL instruction requires the Accumulator Block to have the LAL control signal:
- LAL - loading data from the bus into the Accumulator Register in the lower nibble

Implementation of the LIH instruction requires the Accumulator Block to have the LAH control signal:
- LAH - loading data from the bus into the Accumulator Register in the upper nibble

The implementation of the Accumulator Register block in Logisim is shown in the following figure:

![ Figure 11 ](/Pictures/Figure11.png)

Now the Control Block must be modified to implement the control signals for the LIL instruction

## Update Control Unit
The Control Unit to control the ISAP-1 computer to execute the NOP instruction and the LIL instruction additionally has the following input, output and control signals necessary to control the Accumulator block:
- LAL - control signal that commands the loading of data from the bus into the Accumulator Register in the lower nibble
- LAH - control signal that commands the loading of data from the bus into the Accumulator Register in the upper nibble
- EA – control signal that commands the activation of the outputs to put the data from the Accumulator Register on the bus

The Boolean equations for all these signals that are active when the NOP instruction and the LIL instruction are executed are:
-	LAL = LIL * T3
-	EI = LIL * T3

We will consider all unimplemented instructions as NOP.

The implementation of the Control Unit block in Logisim for the execution of the LIL instruction is shown in the following figure:

![ Figure 12 ](/Pictures/Figure12.png)

Since the complexity of the Control Block increases, I propose grouping the output signals into three distinct groups:
- command signals that will cause a function block to put data on the bus
- command signals that will cause a function block to read data from the bus
- command signals that will control an action of a functional block that has no effect on the data bus

The optimized implementation of the Control Unit block in Logisim for the optimized LIL execution is shown in the following figure:

![ Figure 13 ](/Pictures/Figure13.png)

Since all unimplemented instructions are considered as a NOP instruction, we will have a maximum number of steps equal to 3, the NEXT signal resets the Step Counter in step T4.

The complete schematic of the ISAP-1 computer that correctly executes the NOP instruction and the LIL instruction is shown in the following figure:

![ Figure 14 ](/Pictures/Figure14.png)

The system has been tested and is working properly.

The simulation of this version of the ISAP-1 computer in the Logisim program is in the file: 
[ ISAP-1_v2.circ ](/Logisim/ISAP-1_v2.circ)

The ROM contents in my simulation is: [ ROM2](/Logisim/ROM2)

## LIH instruction implementation
LIH – Load an immediate value into the upper nibble of the Accumulator

The full description of the implementation of the LIH instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#lih-instruction--load-immediate-value-into-upper-nible-of-accumulator

## Update Control Unit for LIH instruction implementation
The Control Unit to control the ISAP-1 computer to execute the new LIH instruction does not need additional input, output and control signals.

The Boolean equations for the signals that are active when the LIH instruction is executed are:
-	EI = LIH * T3
-	LAH = LIH * T3

If we take into account the existing signals for the already implemented instructions and add the new signals we get the following equations for the control signals:
-	EI = LIL * T3 + LIH * T3
-	LAL = LIL * T3
-	LAH = LIH * T3

We will consider all unimplemented instructions as NOP.

The implementation of the Control Unit block in Logisim for executing the new LIH instruction is shown in the following figure:

![ Figure 15 ](/Pictures/Figure15.png)

The complete schematic of the ISAP-1 computer correctly executing the new LIH instruction is shown in the following figure:

![ Figure 16 ](/Pictures/Figure16.png)

The system has been tested and is working properly.

The simulation of this version of the ISAP-1 computer in the Logisim program is in the file: 
[ ISAP-1_v3.circ ](/Logisim/ISAP-1_v3.circ)

The ROM contents in my simulation is: [ ROM3](/Logisim/ROM3)

## STA instruction implementation
STA – Store data from Accumulator in RAM memory at address n \
The full description of the implementation of the STA instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#sta-instruction--store-accumulator

To implement the STA instruction at the simulation level, we must first implement the RAM according to the diagram in [ Figure 2 ](/Pictures/Figure2.png).

## Implementation of the RAM Memory module
The RAM Memory module has the following inputs, outputs and control signals:
- CE – Chip Enable activates the memory chip
- W – Write causes data to be written to memory
- Data – connects to the bus
- Address – connects to the address bus

Since the RAM memory model present in the Logisim program has a different operating mode than a real memory chip we implemented a module called “RAMtest” to determine this behavior.

This module is shown in the following figure:

![ Figure 17 ](/Pictures/Figure17.png)

Thus we determined the following behavior:

|          | SEL |  LD | Description       |
|----------|-----|-----|-------------------|
| Write	   |  1  |  0  |                   |
| Read	   |  1  |  1  | LD set before SEL |
| Disabled |  0  |  0  |                   |

But I want to use signals used by a real memory chip #CS, #OE and #WE:

|          | #WE | #CS | #OE |
|----------|-----|-----|-----|
| Write    |  0  |  0  |  1  |
| Read	   |  1  |  0  |  0  |
| Disabled |  1  |  0  |  1  |
| Disabled |  1  |  1  |  1  |

The ISAP-1 computer uses two active high signals DM and W to control the RAM memory:

|          | DM  |  W  | 
|----------|-----|-----|
| Write    |  1  |  1  | 
| Read	   |  1  |  0  | 
| Disabled |  0  |  0  | 
| Disabled |  0  |  1  |

From the first and third tables the following state table results:
|          |        | SEL | LD  |
|----------|--------|-----|-----|
| Write    | DM + W |  1  |  0  |
| Read	   | DM     |  1  |  1  |
| Disabled |    -   |  0  |  0  |

LD setat înaintea lui SEL



## Update Control Unit for STA instruction implementation
The Control Unit to control the ISAP-1 computer for the execution of the new STA instruction needs to control in addition the following control signals:
- DM – connects to the Chip Enable pin of the RAM memory
- W – connects to the Write pin of the RAM memory

The Boolean equations for the signals that are active when the STA instruction is executed are:
-	LAR = STA * T1 + STA * T3 = STA * ( T1 + T3)
-	EI = STA * T3
-	EA = STA * T4
-	DM = STA * T4
-	W = STA * T4
-	NEXT = NOP * T3 + STA * T5

If we take into account the existing signals for the already implemented instructions and add the new signals we get the following equations for the control signals:
-	EI = LIL * T3 + LIH * T3 + STA * T3
-	LAL = LIL * T3
-	LAH = LIH * T3
-	EA = STA * T4
-	DM = STA * T4
-	W = STA * T4
-	NEXT = NOP * T3 + LIL * T4 + LIH * T4 + STA * T5

We will consider all unimplemented instructions as NOP.

The implementation of the Control Unit block in Logisim for executing the new STA instruction is shown in the following figure:

![ Figure 18 ](/Pictures/Figure18.png)

The complete schematic of the ISAP-1 computer that correctly executes the new STA instruction is shown in the following figure:

![ Figure 19 ](/Pictures/Figure19.png)

The following program is loaded into the ROM memory:

| Address | Code | Asembly | Action      | Result  |
|---------|------|---------|-------------|---------|
| 00      | 31h  | LIL 1   | AL <- 1     | A = x1h |
| 01      | 41h  | LIH 1   | AH <- 1     | A = 11h |
| 02      | 72h  | STA 2   | [02] <- 11h |         |
| 03      | 33h  | LIL 3   | AL <- 3     | A = 13h |
| 04      | 45h  | LIH 5   | AH <- 5     | A = 53h |
| 05      | 75h  | STA 5   | [05] <- 53h |         |
| 06      | 34h  | LIL 4   | AL <- 4     | A = 54h |
| 07      | 44h  | LIH 4   | AH <- 4     | A = 44h |
| 08      | 76h  | STA 6   | [06] <- 44h |         |
| 09      | 38h  | LIL 8   | AL <- 8     | A = 48h |
| 10      | 42h  | LIH 2   | AH <- 2     | A = 28h |
| 11      | 71h  | STA 1   | [01] <- 28h |         |
| 12      | 37h  | LIL 7   | AL <- 7     | A = 27h |
| 13      | 47h  | LIH 7   | AH <- 7     | A = 77h |
| 14      | 79h  | STA 9   | [09] <- 77h |         |
| 15      | 48h  | LIH 8   | AH <- 8     | A = 87h |

The program runs completely from address 0 to address 15 and then continues from address 0 and thus runs indefinitely. \
The system has been tested and is working properly. \
The simulation of this version of the ISAP-1 computer in the Logisim program is in the file: 
[ ISAP-1_v4.circ ](/Logisim/ISAP-1_v4.circ) 
(to download the file - mouse right click and choose – Save link as... )

## LDA instruction implementation
LDA – Load data from RAM memory from address n into Accumulator \
The full description of the implementation of the LDA instruction is here: \

