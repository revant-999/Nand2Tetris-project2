# Nand2Tetris Project 2: Boolean Arithmetic

This project implements fundamental arithmetic and logic circuits using the Hardware Description Language (HDL) from the Nand2Tetris course. The goal is to build an Arithmetic Logic Unit (ALU) and supporting components that can perform various arithmetic and logical operations on 16-bit binary numbers.

## Project Overview

Project 2 focuses on building the computational heart of a computer - the ALU. Starting from basic building blocks, we construct increasingly complex circuits that can perform arithmetic operations, logical operations, and generate status flags.

## Files Included

### Core Components

#### `HalfAdder.hdl`
- **Purpose**: Adds two 1-bit numbers
- **Inputs**: `a`, `b` (1-bit each)
- **Outputs**: `sum` (result bit), `carry` (overflow bit)
- **Functionality**: Computes `a + b` producing a 2-bit result

#### `FullAdder.hdl`
- **Purpose**: Adds three 1-bit numbers (including carry from previous stage)
- **Inputs**: `a`, `b`, `c` (1-bit each)
- **Outputs**: `sum` (result bit), `carry` (overflow bit)
- **Functionality**: Essential building block for multi-bit addition

#### `Add16.hdl`
- **Purpose**: 16-bit binary adder using two's complement representation
- **Inputs**: `a[16]`, `b[16]` (16-bit numbers)
- **Outputs**: `out[16]` (16-bit sum)
- **Implementation**: Chain of 16 FullAdders with carry propagation
- **Note**: Most significant carry bit is ignored (overflow handling)

#### `Inc16.hdl`
- **Purpose**: 16-bit incrementer (adds 1 to input)
- **Inputs**: `in[16]` (16-bit number)
- **Outputs**: `out[16]` (input + 1)
- **Implementation**: Optimized version of Add16 for adding constant 1

#### `ALU.hdl`
- **Purpose**: Arithmetic Logic Unit - the computational core
- **Inputs**: 
  - `x[16]`, `y[16]` (16-bit operands)
  - `zx`, `nx`, `zy`, `ny`, `f`, `no` (control bits)
- **Outputs**:
  - `out[16]` (computation result)
  - `zr` (zero flag: 1 if result is 0)
  - `ng` (negative flag: 1 if result is negative)

## ALU Operations

The ALU can perform 18 different operations based on 6 control bits:

### Control Bit Functions
- `zx`: Zero the x input (set x = 0)
- `nx`: Negate x input (bitwise NOT)
- `zy`: Zero the y input (set y = 0) 
- `ny`: Negate y input (bitwise NOT)
- `f`: Function select (0 = AND, 1 = ADD)
- `no`: Negate output (bitwise NOT)

### Supported Operations
```
Constants: 0, 1, -1
Unary ops: x, y, !x, !y, -x, -y, x+1, y+1, x-1, y-1
Binary ops: x+y, x-y, y-x, x&y, x|y
```

### Status Flags
- **Zero Flag (zr)**: Set to 1 when output equals zero
- **Negative Flag (ng)**: Set to 1 when output is negative (MSB = 1)

## Implementation Details

### Design Philosophy
The ALU follows a systematic approach:
1. **Input Preprocessing**: Conditionally zero or negate inputs
2. **Function Computation**: Perform either AND or ADD operation
3. **Output Postprocessing**: Conditionally negate the result
4. **Flag Generation**: Compute zero and negative status flags

### Key Techniques Used
- **Multiplexers**: For conditional operations (Mux16)
- **Carry Propagation**: In multi-bit addition
- **Bit Manipulation**: Using NOT16, AND16 operations
- **Flag Detection**: Using Or8Way for zero detection

## Building and Testing

### Prerequisites
- Nand2Tetris Hardware Simulator
- Basic logic gates (AND, OR, NOT, XOR, MUX)

### Testing Approach
Each component should be tested individually before integration:
1. Test HalfAdder with all 4 input combinations
2. Test FullAdder with all 8 input combinations  
3. Test Add16 with various 16-bit number pairs
4. Test Inc16 with edge cases (0, -1, max values)
5. Test ALU with the provided test script covering all 18 operations

### Common Implementation Pitfalls
- **Carry Chain**: Ensure proper carry propagation in Add16
- **Two's Complement**: Remember that negative numbers use two's complement
- **Flag Logic**: Zero flag requires checking ALL 16 bits are zero
- **MSB Handling**: Negative flag comes directly from bit 15

## Educational Goals

This project teaches:
- **Binary Arithmetic**: How computers perform mathematical operations
- **Hardware Design**: Building complex circuits from simple components
- **Abstraction Layers**: How high-level operations map to hardware
- **Computer Architecture**: Understanding the ALU's role in a CPU

## Next Steps

The ALU built in this project will be used in later projects as the computational engine for:
- CPU implementation (Project 5)
- Computer system integration (Project 5)
- Assembly language programming (Projects 4-6)

## Resources

- [Nand2Tetris Course Website](https://www.nand2tetris.org/)
- "The Elements of Computing Systems" by Nisan and Schocken
- Hardware Simulator documentation and test files
