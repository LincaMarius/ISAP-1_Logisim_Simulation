# ISAP-1 Computer simulation in Logisim
The improved version of the SAP-1 computer.

This is the process of making the ISAP-1 computer at the simulation level using the Logisim program, presented step by step

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is a continuation of other projects made by me:
https://github.com/LincaMarius/ISAP-Computer \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set

where we optimized the SAP-1 calculator at the block diagram level and at the Instruction Set level.

The final structure of the ISAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

The block diagram of the Central Processing Unit of the ISAP-1 computer is:

![ Figure 2 ](/Pictures/Figure2.png)


## The format of the ISAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|


## The Main Instruction Set of the ISAP-1 computer is:

| Mnemonic | Opcode | Hex | Steps | Description                                                   |
|----------|--------|-----|-------|---------------------------------------------------------------|
| LDA      | 0000   | 0h  |   4   | Load RAM data into Accumulator                                |
| ADD      | 0001   | 1h  |   5   | Add RAM data to Accumulator                                   |
| SUB      | 0010   | 2h  |   5   | Substract RAM data from accumulator                           |
| LIL      | 0011   | 3h  |   3   | Load immediate value into the lower nibble of the Accumulator |
| LIH      | 0100   | 4h  |   3   | Load immediate value into the upper nibble of the Accumulator |
| STA      | 0101   | 5h  |   4   | Store Accumulator data into RAM                               |
| CMP      | 0110   | 6h  |   5   | Compare RAM data with Accumulator                             |
| JZ       | 1001   | 9h  |   3   | Jump if Zero flag is set                                      |
| JC       | 1010   | Ah  |   3   | Jump if Carry flag is set                                     |
| JS       | 1011   | Bh  |   3   | Jump if Sign flag is set                                      |
| JMP      | 1100   | Ch  |   3   | Unconditional jump to address n                               |
| IN       | 1101   | Dh  |   4   | Input data from Port p into the Accumulator                   |
| OUT      | 1110   | Eh  |   4   | Load Accumulator data into Output Register                    |
| -        | 1111   | Fh  |   -   | Prefix for extended instruction set                           |

In total, 13 out of 15 possible instructions are implemented.


## The Extended Instruction Set of the ISAP-1 computer is:

| Mnemonic | Opcode    | Hex | Steps | Operation                                 |
|----------|-----------|-----|-------|-------------------------------------------|
| NOP      | 1111 0000 | F0h |   2   | No Operation                              |
| INC      | 1111 0001 | F1h |   4   | Increment the contents of the Accumulator |
| DEC      | 1111 0010 | F2h |   4   | Decrement the contents of the Accumulator |
| NEG      | 1111 0011 | F3h |   5   | Negate Accumulator                        |
| CPY      | 1111 1110 | FEh |   3   | Copy Accumulator data into B register     |
| HLT      | 1111 1111 | FFh |   2   | Stop processing                           |

In total, 6 extended instructions out of 16 possible are implemented.


## Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter content increment
- LP - loading data from the Bus into the Program Counter
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Program Counter to zero
- EP – output activation for putting data from the Program Counter on the Bus
- Data Input – connects to the bus
- Data Output – connects to the bus

The implementation of the Program Counter block in Logisim is shown in the following figure:

![ Figure 3 ](/Pictures/Figure3.png)

The Program Counter is implemented with a 4-bit register and stores the 4 least significant bits of data received from the Bus when the LP control signal is active and the rising edge of the clock signal occurs.

The output of this register is connected to the 4 least significant bits of the Bus through an 8-bit 3-state Buffer. The other 4, most significant bits are permanently connected low. The output is active only when the EP control signal is high.

The PROBE pins are used to view the contents of the Program Counter regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the bus.

### Address Register implementation
The Address Register has the following input, output and control signals:
- LAR - load data from the bus in the Address Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus
- ABUS – control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 4 ](/Pictures/Figure4.png)

Pin R is to display when this block is reading from the bus.

### Instruction Register implementation
The Instruction Register has the following input, output and control signals:
- LI - loading data from the bus into the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Instruction Register to zero
- EI – enable output to put data from the Instruction Register on the bus
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus
- INSTR – output where the current instruction is presented to the Control Block

The implementation of the Instruction Register block in Logisim is shown in the following figure:

![ Figure 5 ](/Pictures/Figure5.png)

The PROBE pins are used to view the contents of the block regardless of whether the output is in tri-state mode. \
Pins W and R, are to indicate when this block is writing or reading from the bus. \

The lower nibble from the Instruction Register is presented on the output to the BUS for both the lower nibble and the upper nibble, this allows the implementation of LIL instructions which load an immediate value into the lower nibble of the Accumulator, but also of the LIH instruction which loads an immediate value into the upper nibble of the Accumulator.

### Control Unit Implementation
The Control Unit to control the ISAP-1 computer to execute the NOP instruction has the following input, output and control signals:
- INSTR – entry where the current instruction is presented by the Instruction Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Control Unit to zero
- CTRL – output where the control signals are presented

These control signals are:
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
-	PM = T2
-	LI = T2
-	CP = T2
-	LP = 0
-	EI = 0

We will consider all instructions to be NOP, so if an opcode does not have an associated instruction, it will automatically be treated as a NOP instruction.

By doing so I will be able to test the functionality of the computer using the minimum of electronic components. Thus, by reducing the number of components that enter the structure of a system, we reduce the probability of a defect occurring, and in case we have an implementation error, we reduce the number of blocks that must be tested by default, and we reduce the debugging time.

The implementation of the Control Unit block in Logisim using Combinational Logic according to the above Boolean formulas is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The Step Counter is a 3-bit counter, so we can have a maximum of 8 micro-steps.

The step decoder has 8 outputs labeled T1 to T8. The drivers connected to the output are to make the Logisim program happy which in their absence shows an error.

### Improved control unit implementation

This implementation has all outputs grouped together.
I propose their separation. We grouped the control signals into three groups:
- Control signals that cause a function block to write to the bus, at any given time only one such signal must be active: EP, PM, EI;
- Control signals that cause a function block to read data from the bus: LAR, LI, LP;
- Signals that modify the functionality of the modules and usually may not be related to bus-related operations: CP, NEXT.

I entered the NEXT control signal. This is to move to the next instruction if the current instruction has completed all steps, by resetting the Step Counter. This modification has the role of increasing the speed of instruction execution, provides variable length of microinstructions.

For example, in the case of the NOP instruction, steps T1 and T2 are used, that is, 2 out of 8 micro-steps are useful, the remaining 6 micro-steps are wasted time. This fact represents an efficiency of 2/8 = 0.25, i.e. 25%.
By using the use of the control signal noted NEXT we have T1 and T2 useful micro-steps and T3 for counter reset. So the efficiency is 2/3 = 0.67, which is 67% efficiency.

The optimized control block has the diagram shown in figure 7:

![ Figure 7 ](/Pictures/Figure7.png)

### Implementation of the Control Unit using a ROM memory

The implementation of the Control Unit block in Logisim using a ROM Memory according to [Table 1](https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set/blob/main/Pictures/Table1.png).

Since all instructions are considered as the NOP instruction, we will have a maximum number of steps equal to 2, the NEXT signal resets the Step Counter in step T3.

The question arises - what characteristics must the used memory have?

The first information needed is given by how many micro-steps are needed to correctly execute the most complex instruction. We have 3 bits encoding micro-steps T1 – T8.

These 3 bits will control the least significant bits of the ROM address, namely bits A0, A1, A2.

The following information is given by the binary code of the instruction being executed. The ISAP-1 computer has 4-bit coded instructions. These will control address bits A3, A4, A5 and A6.

We also implement a set of extended instructions that have no parameters and can be detected by the fact that the most significant 4 bits of the instruction are high. 

Thus, we will have one more bit to allow us to distinguish between normal instructions and extended instructions.

This bit will be the most significant bit of the ROM address, bit A7.

So, we need a memory that has 8 addresses, so 2^8 = 256 memory locations. \
The control signals are: EP, PM, EI, LAR, LI, LP, CP and NEXT. \
There are 8 control signals in total. So, we can use a standard ROM with 8 bits of data. \
The required ROM capacity is 256 x 8 = 2k Bytes.

By using the data from the link above and following the steps described we obtain the following table: \
[ Table 1 - photo](/Tables/Table1.png) \
[ Table 1 - Excel](/Tables/Table1.xlsx)

In these tables we have the synthesis of the data used to determine the content of the ROM memory that is part of the Control Block to execute only the NOP instruction.

Columns O, N, M, L, K, J, I , H represent the step being executed for the current microinstruction. The hexadecimal representation of the step is in column S, and the binary representation in columns R, Q, and P. The binary form of the Step being executed represents the values ​​for address bits A0, A1, and A2.

The binary code of the instruction being executed is represented in columns G, F, E, and D. These values correspond to address bits A3, A4, A5, and A6.

If an extended instruction is executed the EXT bit will be high. This bit controls address bit A7.

The value for the address is automatically calculated and displayed on the T and AH columns.

The control lines are connected to data bits D0 – D7 and can be seen on columns AB – U. The hexadecimal value to be written to each address is automatically calculated and displayed on columns AF and AI.

The complete form of the table is presented below

[ Table 2 - photo](/Tables/Table2.png) \
[ Table 2 - Excel](/Tables/Table2.xlsx)

The implementation of the Control Unit block in Logisim using a ROM Memory as shown above is:

![ Figure 8 ](/Pictures/Figure8.png)

Control ROM content is: [ CTRLROM1](/Logisim/Version_01/CTRLROM1).

The complete schematic of the ISAP-1 computer that correctly executes the NOP instruction is shown in the following figure:

![ Figure 9 ](/Pictures/Figure9.png)

The red LED indicates that the respective block is writing to the data bus
The green LED indicates that the respective block is reading data from the data bus.

The system has been tested and is working properly.

The simulation in the Logisim program is in the file:
[ ISAP-1_ver_01.circ ](/Logisim/Version_01/ISAP-1_ver_01.circ)

The contents of the ROM memory can be anything, and we can even test the system without connecting any device to the CPU buses. We will have a system that continuously increments the address.

The ROM contents in my simulation is: [ ROM1](/Logisim/Version_01/ROM1).

## LIL instruction implementation
LIL – Load immediate value into lower nibble of Accumulator. \
The full description of the implementation of the LIL instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#lil-instruction--load-immediate-value-into-lower-nibble-of-accumulator

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

The following equations result:
-	SEL = DM
-	LD = DM * #W

These equations are implemented with the following circuit:

![ Figure 18 ](/Pictures/Figure18.png)

## Update Control Unit for STA instruction implementation
The Control Unit to control the ISAP-1 computer for the execution of the new STA instruction needs to control in addition the following control signals:
- DM – connects to the Chip Enable pin of the RAM memory
- W – connects to the Write pin of the RAM memory

The Boolean equations for the signals that are active when the STA instruction is executed are:
-	LAR = T1 + STA * T3
-	EI = STA * T3
-	EA = STA * T4
-	DM = STA * T4
-	W = STA * T4

If we take into account the existing signals for the already implemented instructions and add the new signals we get the following equations for the control signals:
-	EI = LIL * T3 + LIH * T3 + STA * T3
-	LAR = T1 + STA * T3
-	LAL = LIL * T3
-	LAH = LIH * T3
-	EA = STA * T4
-	DM = STA * T4
-	W = STA * T4

We will consider all unimplemented instructions as NOP.

The implementation of the Control Unit block in Logisim for executing the new STA instruction is shown in the following figure:

![ Figure 19 ](/Pictures/Figure19.png)

The complete schematic of the ISAP-1 computer that correctly executes the new STA instruction is shown in the following figure:

![ Figure 20 ](/Pictures/Figure20.png)

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

The program runs completely from address 0 to address 15 and then continues from address 0 and thus runs indefinitely.

The system has been tested and is working properly.

The simulation of this version of the ISAP-1 computer in the Logisim program is in the file: 
[ ISAP-1_v4.circ ](/Logisim/ISAP-1_v4.circ)

The ROM contents in my simulation is: [ ROM4](/Logisim/ROM4)

## LDA instruction implementation
LDA – Load data from RAM memory from address n into Accumulator \
The full description of the implementation of the LDA instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#lda-instruction--load-the-accumulator

## Update Control Unit for LDA instruction implementation
The Control Unit to control the ISAP-1 computer to execute the new LDA instruction does not need any other command signals in addition to the existing ones.

The Boolean equations for the signals that are active when the LDA instruction is executed are:
-	LAR = T1 + LDA * T3
-	EI = LDA * T3
-	DM = LDA * T4
-	LAH = LDA * T4
-	LAL = LDA * T4

If we take into account the existing signals for the already implemented instructions and add the new signals we get the following equations for the control signals:
-	LAR = T1 + STA * T3 + LDA * T3
-	EI = LIL * T3 + LIH * T3 + STA * T3 + LDA * T3
-	LAL = LIL * T3 + LDA * T4
-	LAH = LIH * T3 + LDA * T4
-	EA = STA * T4
-	DM = STA * T4 + LDA * T4
-	W = STA * T4

We will consider all unimplemented instructions as NOP.

The implementation of the Control Unit block in Logisim for executing the new LDA instruction is shown in the following figure:

![ Figure 21 ](/Pictures/Figure21.png)

The complete schematic of the ISAP-1 computer that correctly executes the new LDA instruction is shown in the following figure:

![ Figure 22 ](/Pictures/Figure22.png)

The following program is loaded into the ROM memory:
| Address | Code | Asembly | Action      | Result  |
|---------|------|---------|-------------|---------|
| 00      | 31h  | LIL 1   | AL <- 1     | A = x1h |
| 01      | 41h  | LIH 1   | AH <- 1     | A = 11h |
| 02      | 70h  | STA 0   | [00] <- 11h |         |
| 03      | 00h  | LDA 0   | A <- [00]   | A = 11h |
| 04      | 75h  | STA 5   | [05] <- 11h |         |
| 05      | 05h  | LDA 5   | A <- [05]   | A = 11h |
| 06      | 7Ah  | STA A   | [0A] <- 11h |         |
| 07      | 0Ah  | LDA A   | A <- [0A]   | A = 11h |
| 08      | 7Fh  | STA F   | [0F] <- 11h |         |
| 09      | 0Fh  | LDA F   | A <- [0F]   | A = 11h |
| 10      | 73h  | STA 3   | [03] <- 11h |         |
| 11      | 03h  | LDA 3   | A <- [03]   | A = 11h |
| 12      | 76h  | STA 6   | [06] <- 11h |         |
| 13      | 06h  | LDA 6   | A <- [06]   | A = 11h |
| 14      | 79h  | STA 9   | [09] <- 11h |         |
| 15      | 7Ch  | STA C   | [0C] <- 11h |         |

The program runs completely from address 0 to address 15 and then continues from address 0 and thus runs indefinitely. \
The system has been tested and is working properly.

The simulation of this version of the ISAP-1 computer in the Logisim program is in the file: 
[ ISAP-1_v5.circ ](/Logisim/ISAP-1_v5.circ)

The ROM contents in my simulation is: [ ROM5](/Logisim/ROM5)

## ADD instruction implementation
ADD – Adds the numeric value from Address n with the numeric value stored in the Accumulator Register and stores the result in the Accumulator Register. \
The full description of the implementation of the ADD instruction is here: \
https://github.com/LincaMarius/ISAP-1_Computer_Instruction_Set#add-instruction--add-to-accumulator

To implement the ADD instruction at the simulation level we must first implement the B Register (Temporary Register) and the Arithmetic and Logic Unit (ALU).

## Register B implementation
Register B has the following input, output and control signals:
- LB - loading data from the bus into Register B
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus
- ALUB – the contents of Register B connect to the Logical and Arithmetic Unit

The implementation of the Register B block in Logisim is shown in the following figure:

![ Figure 24 ](/Pictures/Figure24.png)

## Arithmetic and Logic Unit Implementation
The Arithmetic and Logic Unit has the following input, output and control signals:
- ALUA – the contents of the Accumulator Register are connected to the Logical and Arithmetic Unit as operand A
- ALUB – the contents of Register B are connected to the Logical and Arithmetic Unit as operand B
- SU - Subtraction – control signal that orders the subtraction operation to be performed instead of addition
- EU - control signal that commands the activation of the outputs to put on the bus the result of the arithmetic operation performed
- FS - control signal that commands the selection of the Flag that is checked when a jump instruction is executed.
- DOUT - Data Output – connects to the bus
- F – output signal showing the state of the selected Flag

The implementation of the Arithmetic and Logical Unit in Logisim is shown in the following figure:

![ Figure 25 ](/Pictures/Figure25.png)



