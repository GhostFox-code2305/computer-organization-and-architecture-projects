# computer-organization-and-architecture-projects
assigments, test, projects and any code I wrote while doing CS3520 computer organization and architecture


FOR C CODE
# Requirements

  * g++ v5.4 or newer
  * Boost 1.58.0 or newer
    * The `boost/multiprecision/float128.hpp` header must be available
  * Make

## Installed Boost Packages

For development the following boost packages were installed.

```
libboost-date-time1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed]
libboost-filesystem1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed]
libboost-iostreams1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed]
libboost-python1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed,automatic]
libboost-regex1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed,automatic]
libboost-system1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed]
libboost-test-dev/xenial,now 1.58.0.1ubuntu1 amd64 [installed]
libboost-test1.58-dev/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed,automatic]
libboost-test1.58.0/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed,automatic]
libboost1.58-dev/xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 amd64 [installed,automatic]
libboost1.58-doc/xenial-updates,xenial-updates,now 1.58.0+dfsg-5ubuntu3.1 all [installed]
```

This listing was obtained by running `apt list --installed | grep boost`.


# Compilation

The code can be compiled with the provided makefile using the standard `make`
command.

If compiling the code manually, or integrating into a larger program, include
the following flags:

```
FLAGS=-std=c++17 -fsanitize=address -fuse-ld=gold -Wall -MMD \
      -fext-numeric-literals -lquadmath #-O3
```

Note that flag `-fuse-ld=gold` is only required on certain Ubuntu systems due
to a know bug with g++ 5.x.


# Sample Execution & Output

If run without command line arguments, using

```
./precisionEstimate
```

the following usage message will be displayed.

```
Usage: ./precisionEstimate numExecs
```

If run using 

```
./precisionEstimate 100000000
```

output *simliar* to

```
   0 secs | 1.19209e-07
   1 secs | 2.22045e-16
  17 secs | 1.92593e-34
```

will  be displayed. Note that the precision estimates will vary by
architecture/system.


FOR MIPS ASSEMBLY CODE

Part 1: MIPS Assembler

In this part, you will build an assembler for a subset of MIPS assembly. The assembler translates assembly code into machine code, the code that runs on hardware. This exercise will demonstrate how we get from assembly code, something that is still somewhat human-readable, to machine code.

To do this part of the assignment, you will need to understand how MIPS assembly is encoded at the level of bits. To this end, the MIPS documentation is your friend. You can find information about how to encode instructions (and their semantics) in chapter A.10.2 (starting with page A–49) of the SPIM Simulator manual. You may also find the notes on addressing modes (page A–45) helpful.

Your job is to write the assem function in mips_sim.ml:

let rec assem (prog : program) : state = ...

At a high level, you will transform a list of assembly instructions, a program, into an initial starting state. A state consists of a register file, memory and program counter.

Implementation Details

    Instructions to implement: add, beq, jr, jal, li, lui, ori, lw, sw.
    Please note that the beq instruction has been altered to take an offset directly instead of a label. (These offsets are measured in bytes.) This is done to keep things simple–otherwise you would need to keep track of labels and their corresponding offsets!
    For the sake of grading simplicity, please both start the program counter and load your generated machine code into memory at address 0x00000000.
    The load immediate (li) instruction is a pseudoinstruction. This means that it is ok to use it in a MIPS assembly program, but MIPS hardware does not actually support this instruction. Instead, the hardware supports a li instruction by encoding it as two instructions, a lui and ori. Thus, your assembler should accept any MIPS assembly program that contains a li instruction, but your interp function should only accept lui and ori instructions to model the MIPS hardware.

Hint: How registers are ordered in the assembly code for an instruction may differ from the order of register fields in the corresponding machine language encoding. In addition to the SPIM simulator chapter, you can refer to (but not alter) parse.mly for the semantics of the instructions you are implementing.
Part 2: MIPS Interpreter

In this part, you will build an interpreter for MIPS code, similar to the SPIM simulator. The goal of this exercise is to become more familiar with the MIPS instruction set architecture and to gain more experience writing ML code.

The interpreter is structured as a function:

let rec interp (init_state : state) : state = ...

Your job is to write the interp function which simulates one step of execution at a time until program termination (for the purposes of this assignment, treat 0x00000000 as the program termination “instruction”). During a step, you should fetch a word’s worth (4 bytes) of values from memory, starting at the address given by the program counter.

You should then decode the instruction and perform the associated operation. For example, if the instruction bytes decode to an "add $1,$2,$3", then you should update register one, with the sum of the values in registers two and three, and update the program counter so that it points to the next instruction.

Look closely at the Int32 module OCaml standard library as it provides most of the functions that you will need for doing bit manipulation.

Implementation Details

    For the sake of grading simplicity, please implement your MIPS with little-endian memory rather than big-endian.
    You should raise an error if your interp function tries to execute a value from memory that is not a valid machine code instruction. (This does not include 0x00000000, the program termination “instruction”.)
