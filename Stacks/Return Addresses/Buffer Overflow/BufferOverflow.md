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
Compile the overflowvuln.c file with protections off to allow for the compiler to not generate the error below.
![](./images/image1.png)
` gcc -O0 -g -fno-stack-protector overflowvuln.c -o vuln `
 Modern compilers restrict the compilation of the program with the gets() function since it is obsolete and unsafe. To practice the bufferoverflow there are several was to go about it:
 
## Use fgets() instead:
This is how fgets works:
In C it safely reads strings by limiting input size unlike gets().
It reads upto n- 1 characters from a stream, stops at newline or EOF and always null terminates the buffer.
The risks and consideration invoved are:
a) You often need to strip the newline manually with ` strchr `
b) It is still vulnerable if misused in ways such as passing it in unsafe functions such as `strcpy()`  ` printf("%s")` without boundchecks introducing exploitation opportunities.

## Force Legacy Mode
