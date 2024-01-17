# Preface

If you spot any contradictions or loopholes, please let me know. I just *had* to throw this out there so there might be something that I've overlooked.

# Intro

**Hert** is a minimalist, 64-bit, little-endian processor design. Notable features include:

- 2 execution pipelines per core, one for each of the two subinstructions in an instruction
- Single-block instructions with two 18-bit immediate suboperands (where 64-bit operands can be split between multiple instructions)
- Built-in process management
- No saved context between process switches, and the ability for a process to extend its time slice

# Processes

When the processor turns on, it reads the first block in the drive, which is how many instructions, starting from the second block, are part of the root process. It loads all of them to memory then starts the root process.

The processor is hardwired to manage processes. It includes a circular linked list for keeping track of processes: the instruction pointer list (IPL), with a max of 1048576 address items (8MB).

When a process starts, the address of its first instruction is appended to the IPL. The 20-bit index of that item is the process's ID.

The processor loops through the IPL and executes the instruction under each pointer who is incremented afterward.

When a process ends, the preceding IPL item of its IPL item points to the next item of its IPL item.

The processor includes an allocation bit array that covers every allocatable chunk which is 128 blocks. The array has 268435456 bits (32MB). The processor can only cover up to 268435456 blocks (256GB) of memory.

# Cache

Each cache line consists of 8 blocks (each block is 64 bits), and has an existence bit and a busyness bit. The existence bit denotes whether the cache line contains data, and the busyness bit denotes whether the cache line is busy caching to a cache line which takes 8 cycles (one cycle per block). Memory is directly accessed when trying to access a busy cache line.

Each cache line has a range tag that is the 9 bits after the first 22 bits of the address of the 8 memory blocks it caches.

Cache lines have 19-bit indices, with a size of 524288 blocks (32MB). Cache lines accept 31-bit input addresses, selectively covering the first 2147483648 blocks (16GB) of memory. A cache line index is the 19 bits after the first 3 bits of an address. A cache line block index is the first 3 bits of the address. If the last 33 bits of an address are not zero, memory is directly accessed.

When an index clash happens: when using an address whose cache line has a different range tag, the cache line is replaced.

# Pins

Pin Group Mnemonic|Direction|Description
------------------|---------|-----------
GND               |Out      |Ground
VCC               |In       |Supply voltage
P0-P63            |In/out   |The process ID bus
A0-A63            |In/out   |The address bus
D0-D63            |In/out   |The data bus, or in case of the network controller, the address of the memory chunk that is the data
RE                |Out      |Read enable
WE                |Out      |Write enable
SDI               |Out      |Subdata index: the suboperand index of the instruction, but instead of it being about the operand, its about the full data to be written.
MA                |Out      |Memory access: whether memory is targetted
DA                |Out      |Drive access: whether the drive controller is targetted
NA                |Out      |Network access: whether the network controller is targetted
C0-C63            |Out      |The destination chunk address: the memory chunk address to which the network controller should write the incoming data
AA                |Out      |Audio access: whether the audio controller is targetted
VA                |Out      |Video access: whether the video controller is targetted
GA                |Out      |Graphics acceleration access: whether the GPU is targetted
PDA               |Out      |Peripheral data access: whether the peripheral data controller is targetted
TS                |In       |Target signal: whether the target is inputting data
PS                |In       |Peripheral signal: whether a peripheral is inputting data
IPI0-IPI32        |In       |The input peripheral index
OPI0-OPI32        |Out      |The output peripheral index

# Registers

Abbreviation|Size|Details
------------|----|-------
CI          |64b |The clock incremental: incrementing every other clock cycle
PI          |32b |The index of the current process
IP          |64b |The instruction pointer
OPHI        |64b |The output peripheral index
AR          |64b |The primary general-purpose register
BR          |64b |The secondary general-purpose register
SR          |8b  |The status register

# Status Register Flags

Mnemonic|Description
--------|-----------
T       |The truthiness (non-zero) flag
C       |The carry flag
S       |The sign flag*
P       |The parity flag*
O       |The overflow flag
R       |The remainder flag: whether a division yielded a remainder

\* The processor deduces the position of the sign and parity bit based on the (destination's) arithmetic type.

# Instruction Format

Every instruction spans a single block and is composed of two subinstructions that are executed in parallel. The first subinstruction in an instruction is called a *preinstruction*, and the second one is called a *postinstruction*. These are the fields in a subinstruction:

Name                |Size|Description
--------------------|----|-----------
Lingerence          |1b  |Whether to extend the execution of the current process by one instruction/cycle, and increment its IPL item
Opcode              |7b  |See [Opcodes](#opcodes).
Addressing mode     |3b  |<ol start="0"><li>Immediate — The argument* is the immediate suboperand<li>Immediately absolute — The argument is at the address that is the immediate suboperand<li>Immediately indirect — The argument is at the address at the address that is the immediate suboperand<li>Immediately relative — The argument is at the address that is IP plus the immediate suboperand<li>AR — The argument* is AR<li>AR-absolute — The argument is at the address that is AR<li>AR-indirect — The argument is at the address at the address that is AR<li>AR-relative — The argument is at the address that is IP plus AR</ol>
Arithmetic type     |2b  |The argument and destination are:<ol start="0"><li>Unsigned integers<li>Signed** integers<li>Exponent-signed [ISE](https://github.com/ghoomy/universe/blob/main/computer%20science/interlaced%20significand%20and%20exponent.md) numbers<li>Fully signed ISE numbers</ol>
Suboperand index    |1b  |If this is a preinstruction, the immediate suboperand is:<ol start="0"><li>The first 18 bits of the operand<li>The second 18 bits of the operand</ol>Otherwise, if this is a postinstruction, the immediate suboperand is:<ol><li>The third 18 bits of the operand<li>The last 10 bits of the operand</ol>
Immediate suboperand|18b |

\* The argument is the value extracted from the suboperand.

\** A sign of 0 denotes positivity.

# Opcodes

\#|Mnemonic|Description
--|--------|-----------
0 |NOP     |Cook some kielbasa.
1 |RCI     |Write CI to AR.
2 |RPI     |Write PI to AR.
3 |RIP     |Write IP to AR.
4 |RRP     |Write RP to AR.
5 |WAR     |Write the argument to AR.
6 |WBR     |Write the argument to BR.
7 |NOT     |Invert AR.
8 |ANA     |AND AR with the argument.
9 |ORA     |OR AR with the argument.
10|XRA     |XOR AR with the argument.
11|LLA     |Logically left-shift AR.
12|LRA     |Logically right-shift AR.
13|ALA     |Arithmetically left-shift AR.
14|ARA     |Arithmetically right-shift AR.
15|RLA     |Left-rotate AR.
16|RRA     |Right-rotate AR.
17|ADA     |Add the argument to AR.
18|SBA     |Subtract the argument from AR.
19|MLA     |Multiply AR by the argument.
20|DVA     |Divide AR by the argument.
21|SIG     |Extract the ISE significand from BR to AR.
22|EXP     |Extract the ISE exponent from BR to AR.
23|ANB     |AND AR with BR.
24|ORB     |OR AR with BR.
25|XRB     |XOR AR with BR.
26|LLB     |Logically left-shift AR.
27|LRB     |Logically right-shift AR.
28|ALB     |Arithmetically left-shift AR.
29|ARB     |Arithmetically right-shift AR.
30|RLB     |Left-rotate AR.
31|RRB     |Right-rotate AR.
32|ADB     |Add BR to AR.
33|SBB     |Subtract BR from AR.
34|MLB     |Multiply AR by BR.
35|DVB     |Divide AR by BR.
36|JMP     |Write the argument to PI.
37|JPT     |Jump to the address that is the argument if T.
38|JPC     |Jump to the address that is the argument if C.
39|JPS     |Jump to the address that is the argument if S.
40|JPP     |Jump to the address that is the argument if P.
41|JPO     |Jump to the address that is the argument if O.
42|JPR     |Jump to the address that is the argument if R.
43|JAL     |Jump to the address that is BR if the allocation bit at the index that is the argument is set.
44|SPH     |Write the argument to OPHI.
45|RMA     |Read from the address that is the argument, into AR.
46|ALC     |Set the allocation bit at the index that is the argument.
47|DEA     |Unset the allocation bit at the index that is the argument.
48|WMA     |Set the allocation bit at the index that is AR, and write the argument to the block at the address that is AR.
49|WMB     |Set the allocation bit at the index that is BR, and write AR to the block at the address that is BR.
50|RTD     |Read to AR from the drive controller with the address that is the argument.
51|RTN     |Read from the network controller with the address that is the argument.
52|RTA     |Read to AR from the audio controller with the address that is the argument.
53|RTV     |Read to AR from the video controller with the address that is the argument.
54|RTG     |Read to AR from the GPU with the address that is the argument.
55|RTP     |Read to AR from the peripheral data controller with the address that is the argument.
56|WTM     |Write the argument as data to memory with the address that is BR.
57|WTD     |Write the argument as data to the drive controller with the address that is BR.
58|WTN     |Write the argument as data to the network controller with the address that is BR.
59|WTA     |Write the argument as data to the audio controller with the address that is BR.
60|WTV     |Write the argument as data to the video controller with the address that is BR.
61|WTG     |Write the argument as data to the GPU with the address that is BR.
62|WTP     |Write the argument as data to the peripheral data controller with the address that is BR.
