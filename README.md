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

## Address Register implementation
The Address Register has the following input, output and control signals:
- LAR - load data from the bus in the Address Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus
- ABUS – control output for the address bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 4 ](/Pictures/Figure4.png)

The Address Register is implemented with a 4-bit register and stores the 4 least significant bits of data received from the Bus when the LAR control signal is active and the rising edge of the clock signal occurs.

The output of this register is connected to 4 bits of the Address Bus through a 4-bit Buffer. The output is permanently active. The buffer is needed to provide the Fan-out for the control of the address bus, because they can connect to it: RAM Memory, ROM Memory, 16 I/O devices.

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

![ Figure 5 ](/Pictures/Figure5.png)

The Instruction Register is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LI is active and the rising edge of the clock signal occurs.

The lower nibble stored in the Instruction Register is output on the bus for both the lower nibble and the upper nibble, this allows implementation of LIL instructions that load an immediate value into the lower nibble of the Accumulator but also the LIH instruction which loads an immediate value into the upper nibble of the Accumulator. The output is tri-stated by using an 8-bit buffer and is active only when the EI control signal is high.

The PROBE pin are used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the bus.

The INSTR pin presents the current instruction to the Control Block where it is decoded.


## Implementation of the Accumulator Register
The Accumulator register has the following input, output and control signals:
- LAH - loading data from the bus into the upper Nibble Accumulator Register
- LAL - load data from the bus into the lower Nibble Accumulator Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- EA – output activation for putting data from the Accumulator Register on the bus
- Data Input – connects to the bus
- Data Output – connects to the bus
- ALUA – the contents of the Accumulator connect to the Logical and Arithmetic Unit

The implementation of the Accumulator Register block in Logisim is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The Accumulator register is implemented with two 4-bit registers that independently store the lower nibble when the control signal LAL is high and the rising edge of the clock signal occurs, or the upper nibble when the control signal LAH is high and the rising edge of the clock signal occurs. 

The output is provided by a three-state buffer and is active only when the EA control signal is high.

The PROBE pin is used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the bus.


## Register B implementation
Register B has the following input, output and control signals:
- LB - loading data from the bus into Register B
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the bus
- DOUT - Data Output – connects to the bus
- ALUB – the contents of Register B connect to the Logical and Arithmetic Unit

The implementation of the Register B block in Logisim is shown in the following figure:

![ Figure 7 ](/Pictures/Figure7.png)

Register B is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LB is active and the rising edge of the clock signal occurs.

The output is connected to the Logic and Arithmetic Unit permanently.

Pin R is to display when this block is reading from the bus.


## Arithmetic and Logic Unit Implementation
The Arithmetic and Logic Unit has the following input, output and control signals:
- ALUA – the contents of the Accumulator Register are connected to the Logical and Arithmetic Unit as operand A
- ALUB – the contents of Register B are connected to the Logical and Arithmetic Unit as operand B
- SU - Subtract – control signal that orders the subtraction operation to be performed if it is high
- EU - control signal that commands the activation of the outputs to put on the bus the result of the arithmetic operation performed
- DOUT - Data Output – connects to the bus
- FLAGS – output signal showing the state of the Flags

The implementation of the Arithmetic and Logical Unit in Logisim is shown in the following figure:

![ Figure 8 ](/Pictures/Figure8.png)

The Arithmetic and Logic Unit has as its main element an 8-bit Adder. The addition calculation is performed continuously.

The Subtraction operation is done by inverting the value of operand B (one's complement) and adds one by applying the SU signal to the Carry In input (two's complement).

The output to the Bus is provided by a three-state buffer and is active only when the EU control signal is high.

The state of the Flags is stored in a 3-bit register and passed to the Control Block to help execute conditional jump instructions.

The W pin has the role of displaying when this block is writing to the bus.


## Implementation of the Constants Generator block
The Constants Generator module has the following inputs, outputs and control signals:
- SC1 – Constant Setting 1 – constant setting generated 1
- EC – Enable Constant Output – data output to the Bus is activated
- DOUT - Data Output – connects to the bus

The implementation of the Constants Generator in Logisim is presented in the following figure:

![ Figure 9 ](/Pictures/Figure9.png)


## Implementation of the RAM Memory module
The RAM Memory module has the following inputs, outputs and control signals:
- #CE – Chip Enable - activates the memory chip
- #W – Write - determines the writing of data in memory
- #OE – Output Enable – data output to the Bus is activated
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


