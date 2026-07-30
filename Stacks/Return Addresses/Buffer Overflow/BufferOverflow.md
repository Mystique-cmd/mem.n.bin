A buffer oveflow in a stack frame happens when a program writes more data to a buffer ( a local array ) than the allocated space allows. 
Since buffers live inside the stack frames overflowing them means you will start overwriting the adjacement memory . This includes:
(i) Saved Registers
(ii) Local Variables
(iii) The return addresses

# How it works
a) A function declares a local buffer ( char buf[16] ).
b) If the user input is unchecked ( gets(buf) ) they can input more than 16 bytes
c) The extra bytes will spill into the stack frame out of buf.
d) This overwrites the base pointer, local variables and the return address.
e) When the function executes RET the CPU jumps to the corrupted return address which can be attacker controlled.

# Drill
Compile the overflowvuln.c file with protections off
` gcc -O0 -g -fno-stack-protector overflowvuln.c -o vuln `
