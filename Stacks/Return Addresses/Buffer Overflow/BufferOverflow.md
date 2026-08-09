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
To compile the program using legacy C standard you can simply tell the compiler through the following command.
`gcc -O0 -fno-stack-protector -std=gnu89 overflowvuln.c -o vuln`

The flag `-std=gnu89` swithches GCC to GNU dialect of ANSI C . `-Oo` disables optimizations
Notes:
a) Modern glibc ships gets() but still returns a depracated error
b) If the libc does not allow gets() you can manually declare it

## Manual Declaration
If you want to bypass the compiler error and still pass gets()  you can manually declare the function prototype before using it. This tricks the compiler into accepting the call even though modern headers do not provide it anymore.
It can be declared by the following line of code:
` char *gets(char *buf)`

## Vulnerable Alternatives
