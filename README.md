# RISC-V-Single-Cycle-Pipelined-Processor-Design-in-System-Verilog

This repository contains the implementation of a 3-stage Pipelined Processor based on the RISC-V Instruction Set Architecture (ISA).

### Supported Instructions

#### **Data Processing Instructions**
- **R-type**: ADD, SUB, SLL, SLT, SLTU, XOR, SRL, SRA, OR, AND
- **I-type**: ADDI, SLTI, SLTIU, XORI, ORI, ANDI, SLLI, SRLI, SRAI
- **U-type**: LUI, AUIPC

#### **Memory Access Instructions**
- **I-type**: LB, LH, LW, LBU, LHU
- **S-type**: SB, SH, SW

#### **Flow Control Instructions**
- **J-type**: JAL
- **I-type**: JALR
- **B-type**: BEQ, BNE, BLT, BGE, BLTU, BGEU

#### **CSR Instructions**
- CSRRW, MRET

---

### Interrupt Handling
The processor includes functionality to handle timer interrupts. When a timer interrupt occurs:
1. The interrupt is registered in the `mip` register.
2. The value of the Program Counter (PC) is saved in the `mepc` register.
3. The interrupt service routine (ISR) provided by the programmer is executed.
4. At the end of the ISR, the `mret` instruction restores the PC to the value stored in `mepc`, resuming program execution from where the interrupt occurred.

---

### Pipelining Hazards: 3-Stage vs. 5-Stage Pipeline

#### **Structural Hazards**
- **3-stage and 5-stage pipelines**: No structural hazards occur as instruction memory and data memory are implemented as separate memories.

#### **Data Hazards**
- **3-stage pipeline**: Data hazards between the current instruction and the 1st-previous instruction are resolved through forwarding from the Memory-Writeback stage to the Decode-Execute stage. For load instructions, a single stall cycle is required.
- **5-stage pipeline**: Data hazards between the current instruction and the 1st-previous or 2nd-previous instructions are handled via forwarding from the Memory to Execute stage and from the Writeback to Execute stage. For load instructions, a single stall cycle is necessary.

#### **Control Hazards**
- **3-stage pipeline**: Only the Decode-Execute stage is flushed for branch and jump instructions, as the target address is determined during this stage.
- **5-stage pipeline**: Both the Decode and Execute stages are flushed, as the target address is resolved after the Execute stage.

---

### Processor Functionality Testing
The processor’s functionality has been validated using two assembly programs:
1. **GCD (Greatest Common Divisor)**
2. **Factorial**

For the factorial example, refer to `inst.mem`. For the GCD example, see the specified commit in the repository.

---

### Compilation & Simulation Guidelines

#### **Compilation**
The RTL code can be compiled by running the `compile.bat` file included in the repository.
Alternatively, use the following commands:

- To compile specific files:
  ```
  vlog names_of_all_system_verilog_files
  ```
- To compile all SystemVerilog files in the directory:
  ```
  vlog *.sv
  ```
Compilation creates a `work` folder in the current directory, which stores all generated files.

#### **Simulation**
To simulate the compiled RTL, use the following command:
```
vsim -c name_of_toplevel_module -do "run -all"
```
This generates a `.vcd` file containing the simulation behavior of the design.

#### **Viewing the VCD Waveform File**
To view the waveform of the design, run:
```
gtkwave dumpfile_name.vcd
```
Here, `dumpfile_name` will be `processor.vcd` by default. This opens the waveform viewer, allowing you to analyze signals and verify the design’s behavior.

