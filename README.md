

# SimpleLang Compiler for an 8-bit CPU

## Overview

This project implements a basic compiler for a minimalistic high-level language called **SimpleLang** designed to run on an 8-bit CPU. The language includes basic constructs such as variable declarations, assignments, arithmetic operations, and conditional statements.

The compiler translates SimpleLang code into assembly code that can be run on an 8-bit CPU simulator. This project is intended for educational purposes, helping users understand compiler construction and the architecture of an 8-bit CPU.

## Features

- **Variable Declaration**: `int x;`
- **Assignment**: `x = 5;`
- **Arithmetic Operations**: `x + y`, `x - y`
- **Conditional Statements**: `if (x == y) { ... }`

## Project Structure

- **`lexer.c`**: Handles tokenizing the input source code.
- **`parser.c`**: Converts tokens into an Abstract Syntax Tree (AST).
- **`codegen.c`**: Defines the structures for the AST and includes the code generation logic.
- **`main.c`**: The entry point of the program. Handles file input/output and invokes the lexer, parser, and code generator.

## How the Compiler Works

### 1. Lexical Analysis (Lexer)

The lexer reads the source code and breaks it down into a sequence of tokens. These tokens represent the basic elements of the language, such as keywords (`int`, `if`), identifiers (variable names), operators (`+`, `-`, `==`), and literals (numbers).

Example of tokenizing `int x = 5 + 3;`:

```
Token(INT, "int")
Token(IDENTIFIER, "x")
Token(EQUAL, "=")
Token(NUMBER, "5")
Token(PLUS, "+")
Token(NUMBER, "3")
Token(SEMICOLON, ";")
```

### 2. Parsing

The parser takes the list of tokens and organizes them into an Abstract Syntax Tree (AST). The AST is a hierarchical representation of the program structure, capturing the relationships between the tokens.

For example, the code `int x = 5 + 3;` would be parsed into an AST where the `BinaryOpNode` represents the addition operation, and the `VariableDec` node represents the declaration and initialization of `x`.

### 3. Code Generation

The AST is then traversed to generate the corresponding assembly code for the 8-bit CPU. The generated assembly code uses operations like `ldi`, `add`, and `sta` to manipulate the CPU registers and memory.

For example, `int x = 5 + 3;` might generate the following assembly code:

```assembly
ldi A 5       ; Load 5 into A register
ldi B 3       ; Load 3 into B register
add           ; Add A and B, store result in A
sta 1         ; Store the result into memory location for x
```

### 4. Handling Conditionals

Conditionals are handled by generating assembly code for both the `then` and `else` branches. The code includes jump instructions (`jnz`, `jmp`) to control the flow based on the comparison results.

For example, the code `if (x == y) { y = y + 1; }` might generate:

```assembly
lda 1           ; Load x into A
cmp 2           ; Compare A with memory location of y
jnz else_branch ; Jump to else_branch if x != y
lda 2           ; Load y into A
ldi B 1         ; Load 1 into B
add             ; Add A and B
sta 2           ; Store result back to y
jmp endif       ; Jump to endif
else_branch:
endif:
```

## How to Run

### Prerequisites

- Git
- GCC (G++ for C++)
- A text editor (Notepad, Vim, Nano, etc.)
- 8-bit CPU Simulator (Clone from [GitHub](https://github.com/lightcode/8bit-computer))


### Building the Project

1. **Clone this repository:**
   ```sh
   git clone https://github.com/rishav-raushan/Compiler-SimpleLang-
   cd Compiler-SimpleLang-
   ```
2. **Compile the compiler:**
   ```sh
   gcc codegen.c -o codegen
   ```
3. **Run the compiler:**
   ```sh
   ./codegen
   ```
4. **Check the generated assembly output:**
   ```sh
   cat output.asm
   ```
5. **Run the assembly code in the 8-bit CPU simulator.**

## Input Example (input.txt)
```c
int a;
int b;
int c;
a = 10;
b = 20;
c = a + b;
if (c == 30) {
 c = c + 1;
} else {
 c = c - 1;
}
```

## Expected Output (output.asm)
```assembly
ldi A 10
mov M A 100
ldi A 20
mov M A 101
mov A M 100
add M 101
mov M A 102
mov A M 102
cmp A 30
jne end_if_0
mov A M 102
inc A
mov M A 102
jmp end_else_1
end_if_0:
mov A M 102
dec A
mov M A 102
end_else_1:
hlt
```

The output assembly file will be generated in the same directory as the source file, with the same name but with a `.asm` extension.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue if you have suggestions or find any bugs.
