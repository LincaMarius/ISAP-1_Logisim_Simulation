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

## The instruction set of the ISAP-1 computer is:

| Mnemonic | Opcode | Hex | Operation                                                     |
|----------|--------|-----|---------------------------------------------------------------|
| LDA      | 0000   | 0h  | Load RAM data into Accumulator                                |
| ADD      | 0001   | 1h  | Add RAM data to Accumulator                                   |
| SUB      | 0010   | 2h  | Substract RAM data from accumulator                           |
| LIL      | 0011   | 3h  | Load immediate value into the lower nibble of the Accumulator |
| LIH      | 0100   | 4h  | Load immediate value into the upper nibble of the Accumulator |
| IN       | 0101   | 5h  | Input data into the Accumulator                               |
| JMP      | 0110   | 6h  | Unconditional jump to address n                               |
| OUT      | 1110   | Eh  | Load Accumulator data into Output Register                    |
| HLT      | 1111   | Fh  | Stop processing                                               |
