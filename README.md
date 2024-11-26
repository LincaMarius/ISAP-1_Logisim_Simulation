# ISAP-1 Computer simulation in Logisim
The improved version of the SAP-1 computer.

This is the process of making the ISAP-1 computer at the simulation level using the Logisim program, presented step by step

By: Lincă Marius Gheorghe.

Pitești, Argeș, România, Europe.

https://github.com/LincaMarius

## About the project, brief description

The goal of this project is to create a more efficient version of the SAP (Simple As Possible) computer, but with minimal changes so that the instruction format remains the same.

This project is part of a project made by me:
https://github.com/LincaMarius/ISAP-Computer

where we optimized the SAP-1 computer

## ISAP-1 version 1

The Structure of the SAP-1 computer is:

![ Figure 1 ](/Pictures/Figure1.png)

The block diagram of the Central Processing Unit of the SAP-1 computer is:

![ Figure 2 ](/Pictures/Figure2.png)

### The format of the SAP-1 computer instructions is:

| 4 bits instruction code   | 4 bits operand (memory address)          |
|---------------------------|------------------------------------------|

We notice that the upper nibble is used to encode an instruction.

So, any instruction is encoded on 4 bits.

Thus, we can have a maximum of 2 ^ 4 = 16 instructions.

### The original instruction set of the SAP-1 computer is:

| Mnemonic | Opcode | Operation                                  |
|----------|--------|--------------------------------------------|
| LDA      | 0000   | Load RAM data into Accumulator             |
| ADD      | 0001   | Add RAM data to Accumulator                |
| SUB      | 0010   | Substract RAM data from accumulator        |
| OUT      | 1110   | Load Accumulator data into Output Register |
| HLT      | 1111   | Stop processing                            |

We can notice that the instructions 0011, 0100, 0101, 0110, 0111, 1000, 1001, 1010, 1011, 1100, 1101 are not used. So we can add 11 more new instructions.

In total, 5 out of 16 possible instructions are implemented.

In order to simulate the SAP-1 computer using the Logisim program, we must design each individual module that makes up the computer.

For this purpose we must use the detailed block diagram of the SAP-1 computer which is presented in the following figure.

![ Figure 3 ](/Pictures/Figure3.png)

The first block to be implemented is Clock and Reset

### Clock and Reset Implementation
The Clock and Reset block has the following input, output, and control signals:
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Program Counter to zero

We have three control buttons: \
S5 – Reset button \
S6 – Single step button \
S7 – Manual / Auto mode select

This information can be seen in figure 3.

The implementation of the Clock and Reset block in the Logisim program is shown in the following figure

![ Figure 4 ](/Pictures/Figure4.png)

### Program Counter implementation
Program Counter has the following input, output and control signals:
- CP – Program Counter content increment
- CLK – clock signal that ensures synchronism in the operation of the computer
- CLR – reset signal that initializes the Program Counter to zero
- EP – output activation for putting data from the Program Counter on the Bus
- Data Output – connects to the Bus

The implementation of the Program Counter block in Logisim is shown in the following figure:

![ Figure 5 ](/Pictures/Figure5.png)

The Program Counter is made with 4 JK Flip-Flops and increments the stored numerical value if the CP control signal is active and the positive edge of the clock signal occurs.

The output of this register is connected to the 4 least significant bits of the Bus through a 4-bit 3-state Buffer. The other 4 most significant bits are not connected and because we are using TTL logic circuits will appear to be high. The output is active only when the EP control signal is high.

The PROBE pins are used to view the contents of the Program Counter regardless of whether the output is in tri-state mode.

The W pin is used to display when this block is writing to the Bus.

### Address Register implementation
The Address Register has the following input, output and control signals:
- LAR - load data from the Bus in the Address Register
- CLK – clock signal that ensures synchronism in the operation of the computer
- DIN - Data Input – connects to the Bus
- ABUS – control output for the Address Bus

The implementation of the Address Register block in Logisim is shown in the following figure:

![ Figure 6 ](/Pictures/Figure6.png)

The Address Register is implemented with a 4-bit register and stores the 4 least significant bits of data received from the Bus when the LAR control signal is active and the rising edge of the clock signal occurs.

The output of this register is connected to 4 bits of the Address Bus. The output is permanently active.

Pin R is to display when this block is reading from the Bus.

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

![ Figure 7 ](/Pictures/Figure7.png)

The Instruction Register is implemented with an 8-bit register and stores 8 bits of data received from the Bus when the control signal LI is active and the rising edge of the clock signal occurs.

The lower nibble stored in the Instruction Register is presented on the bus output if the EI control signal is high and represents the parameter of the instruction being executed. The output is tri-stated using a 4-bit buffer and is active only when the EI control signal is high.

The PROBE pin are used to view the contents of the block regardless of whether the output is in tri-state mode.

Pins W and R, are to indicate when this block is writing or reading from the Bus.

The INSTR pin presents the current instruction to the Control Block where it is decoded.

### Implementation of the Accumulator Register
The Accumulator register has the following input, output and control signals:
- LA - load data from the Bus into the Accumulator Register
- CLK - clock signal that ensures synchronism in the operation of the computer
- EA - output control for putting data from the Accumulator Register on the Bus
- Data Input - connects to the Bus
- Data Output - connects to the Bus
- ALUA - the contents of the Accumulator are connected to the Arithmetic and Logic Unit

The implementation of the Accumulator Register block in Logisim is shown in the following figure:

![ Figure 8 ](/Pictures/Figure8.png)

The Accumulator Register is implemented with an 8-bit register that stores the data present at the input from the Bus when the LA control signal is high and the rising edge of the clock signal occurs.

The output is provided by a three-state buffer and is active only when the EA control signal is high.

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

The output is connected to the Logic and Arithmetic Unit permanently.

Pin R is to display when this block is reading from the bus.

### Arithmetic and Logic Unit Implementation
The Arithmetic and Logic Unit has the following input, output and control signals:
- ALUA – the contents of the Accumulator Register are connected to the Arithmetic and Logic Unit as operand A
- ALUB – the contents of Register B are connected to the Logical and Arithmetic Unit as operand B
- SU - Subtraction – control signal that activates subtraction operation to be performed if it is high
- EU - control signal that commands the activation of the outputs to put on the Bus the Result of the arithmetic operation performed
- DOUT - Data Output – connects to the bus

The implementation of the Arithmetic and Logical Unit in Logisim is shown in the following figure:

![ Figure 10 ](/Pictures/Figure10.png)

The Arithmetic and Logic Unit has as its main element an 8-bit Adder. The addition calculation is performed continuously.

The Subtraction operation is done by inverting the value of operand B (one's complement) and adds one by applying the SU signal to the Carry In input (two's complement).

The output to the Bus is provided by a three-state buffer and is active only when the EU control signal is high.

The W pin has the role of displaying when the Result is writing to the Bus.

### Implementation of the RAM Memory module
Figure 11 shows the Memory Block of the SAP-1 Computer seen from a functional point of view by the Computer when running a program.

![ Figure 11 ](/Pictures/Figure11.png)

The memory has two operating modes, dictated by the position of switch S2.

When S2 is in the Run position, the PGM signal is low and causes the Address Multiplexer to select the address from the input connected to the Memory Address Register. Also, the CE control signal is connected to the #CE control pin of the RAM.

The Schematic Diagram of the Memory Block in Run Mode is as follows

![ Figure 12 ](/Pictures/Figure12.png)

In this Diagram the Multiplexer can be ignored and we can obtain a simpler and easier to understand Diagram.

![ Figure 13 ](/Pictures/Figure13.png)

Thus, the memory of the SAP-1 computer in Run mode is used as a ROM memory. Control input #CE if high disables the output which is three-state, and if low at the output the data from the selected address is presented.

The RAM Memory module has the following inputs, outputs and control signals:
- #CE – Chip Enable - activates the memory chip
- Data – connects to the bus
- Address – connects to the address bus

It is noted that the RAM used does not have a #OE control pin for output inhibition, this function is taken over by the #CE pin.

I came to the conclusion that I can use a ROM memory model provided by the Logisim program.

The ROM memory model implemented in the Logisim program has the sel input which, if high, presents the data at the selected address at the output. When the sel pin is low, the output goes into high impedance mode.

This behavior allows the DM control signal to be directly connected to the sel pin of the ROM memory in the Logisim program.

![ Figure 14 ](/Pictures/Figure14.png)
