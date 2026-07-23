Function Arguments in stack frames are the first layer of data that gets placed when a function is called.
They define how values are passed into the function, and depending on the architecture and calling convention, they may live in registers or spill inot the stack

## How Function Arguments Are Stored
i) Registers first - On System V AMD64 the first six integer/ pointer arguments go into registers : %rdi, %rsi, %rdx, %rcx, %r8, %r9.
On Windows x64, the first four go into %rcx, %rdx, %r8, %r9
ii) Stack Spill - If there are more arguments than the registers can hold, the extras are pushed onto the stack
(iii) Alignment - The stack pointer %rsp is aligned to 16bytes before a call. Arguments are padded if necessary to maintain alignement

## Why this matters in Exploitation
i) Buffer Overflows - if arguments spill onto the stack, they sit right next to local variables and saved registers and these are prime targets for corruption
ii) ROP Chains - Understanding which arguments are in registers vs stack lets you craft payloads correctly
iii) Reverse Engineering - Recognizing argument placement helps reconstruct function prototypes from disassembly

In the file "funcarg.c"
- a -> %rdi
- b -> %rsi
- c -> %rdx
- d -> %rcx
- e -> %r8
- f -> %r9
- g -> pushed onto the stack

Drill:
Compile it with `gcc -O0 -fno-stack-protector`
Run gdb and inspect registers after the call
Use x/16gx $rsp to see spilled arguments on the stack
Draw a layout : registers vs stack

