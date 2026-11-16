# SimCore: An 8-Bit Processor Simulator

## Introduction

SimCore was developed as the final project for a core Computer Organization course, which provides a comprehensive exploration of computer architecture across multiple abstraction levels, from assembly language and C programming to digital logic design and processor operation. Because a central focus of the curriculum involves embedded systems and the interaction between software, hardware, and external I/O devices, this project was designed to model the execution mechanics of a simple processor architecture.

The project models the "Simple Processor," a compact 8-bit architecture featuring four general-purpose registers, 256 bytes of memory, and a core instruction set of 10 operations. The underlying digital design incorporates essential registers, such as the Program Counter (PC), Instruction Register (IR), and status flags (N and Z), alongside an Arithmetic Logic Unit (ALU), multiplexers, a register file (RF), and internal memory blocks.

## What is SimCore?

SimCore is a software-based simulator designed to model instruction execution cycles. The application allows users to select and simulate three core instruction types:

- **`add Ra, Rb`**: Adds the contents of register Ra and register Rb, storing the resulting value in Ra.
- **`sub Ra, Rb`**: Subtracts the contents of register Rb from Ra, storing the result in Ra.
- **`ori imm5`**: Performs a bit-wise logical OR operation between the contents of register R1 (configured by convention) and a 5-bit immediate constant (`imm5`), storing the result in R1.

For arithmetic operations (`add` and `sub`), the interface prompts the user to select registers via keyboard input. For immediate operations (`ori`), users can manually input a 5-bit binary constant.

Upon receiving the input parameters, the simulator transitions to an animated graphical processor screen that sequentially displays the required register enable-signals and multiplexer select bits for each clock cycle of the instruction execution.

\<insert picture (Screenshots of the SimCore interface, showing the main startup screen on the left and the animated execution of an add instruction on the right) here\>

## Operational Guide

The software is written in C and is structured to execute on a NIOS V processor environment instantiated on an FPGA evaluation board (such as the DE1-SoC via the Intel Monitor Program), or equivalently within a cloud-based simulator such as CPULator.

### Execution Steps

1. **Compilation and Loading:** Load the C source files into the development environment, compile, and load the program to initialize the disassembly and execution panes.

2. **Initialization:** Start the program execution. The welcome screen presents options to select an instruction:
   - Press `a` for `add Ra, Rb`
   - Press `s` for `sub Ra, Rb`
   - Press `o` for `ori imm5`

3. **Parameter Configuration:** For arithmetic instructions, enter the desired register numbers (e.g., `3` for R3, `2` for R2) and press the spacebar. For the `ori` instruction, input the 5-bit binary constant (`imm5`) using the `0` and `1` keys.

4. **Simulation Execution:** After encoding is complete, press the spacebar to initiate the animated cycle-by-cycle processor execution phase. Users may pause and resume the simulation at any point using the spacebar.

5. **Completion:** Upon execution completion, a status indicator appears. Press `e` to return to the main menu or `r` to replay the current instruction sequence.

## Architecture, Flowchart, and Block Diagram

The program execution flow is managed by a structured finite state machine architecture comprising the following operational states:

- **Main Screen:** Initial selection menu for instruction types.
- **Instruction Input Screens:** Dedicated interfaces for parameter and register configuration.
- **Encoding Module:** Binary instruction formatting prior to memory mapping.
- **Processor Simulation Module:** Graphical representation of the processor datapath, highlighting active logic blocks in designated colors and stepping through clock cycles via timer interrupts.

The underlying system architecture is divided into four primary functional groups:

- **Input Group:** Handles asynchronous keyboard input polling or interrupt handling.
- **Output Group:** Manages display rendering via a VGA interface, audio feedback generation, and secondary debugging aids such as onboard LEDs and hexadecimal displays.
- **Interrupt Service Routine (ISR) Group:** Manages system responsiveness using interrupt-based I/O synchronization for hardware timers and input peripherals.
- **Main Program Group:** Implements the primary control finite state machines, background rendering routines, and graphical display plotters.
