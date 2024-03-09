# MyOS

## Project Description
The main objective is to create an OS from the ground up using Assembly Language (x86), followed by some C coding. Independent project to master git usage, low-level programming and overall computer software/hardware concepts.

## Notes on main.asm
We will be working with the BIOS (Basic Input/Output System), it starts the system and provides basic functions to do basic things like writing stuff to the screen using interrupts.
We will also be using the stack, with 'sp' -> stack pointer pointing to the beginning of the OS memory and growing downwards from it, guarenteeing no overlap and loss of memory. Using 'push' and 'pop' we can get items to and from the stack.
The BIOS will be expecting the OS to be in a certain location, so we give the assembler this information using the org directive, and, in response, memory locations will be calculated using this offset.
Start guarantees that main will always be the starting point.
### Interrupt types:
- Exception, divide by zero, segmentation fault, page fault;
- Hardware, keypress, mouse click;
- Software, INT instruction in assembly.
### Directive vs Instruction:
  -A directive gives a clue to the assembler that will after how a program will be compiled and is NOT translated into machine code, which makes is assembler specific - different assemblers have different directives;
  -An instruction is translated and executed by the CPU.
### Some Registers:
- 'ss', stack segment, point to segment in memory where the stack is located. 
- 'sp', stack pointer, points to the current top of the stack in memory, in our case it is also the beginning of the OS in memory. 
__PhysicalAddress = ss * 16 + sp__ on 16 bit incremental base segments.
- 'ax', accumulator register, arithmetic, logic and I/O operations.
- 'ds', data segment, points to a segment of data in memory
- 'es', extra segment, similar to 'ds' but usually used for additional storage.
- 'si', source index, general purpose, used to, for example, store inputs.
The bits directive tells the assembler to emit x-bit code. (in this case 16-bit).
'puts' function will print a string to the screen. Recieves a pointer to a string in ds:si (data segment:start index) and it prints charaters until it encounters a '/0'.
.Loop verifies if the current character is null and continues if not, using the ASCII in 'al' and providing the BIOS with a video interrupt, and if 'al' is null, exits.
.halt is used to maintain it running in an expectable state in case CPU starts unexpectedly.
### In nasm:
- $ -> special symbol that is equal to the memory offset of the current line.
- $$ -> special symbol that is equal to the memory offset of the beginning of the current section. (in our case, program).
- times is a directive that repeats a given instruction or piece of data a number of times.
- db stands for "define byte(s)", and it writes given byte(s) to the assembled binary file.
The BIOS expects the last bytes of the first sector to be AA55. One sector will have 512 bytes.
The 'dw' command stands for "define word(s). Writes given word(s) to the assembled binary file (little endian, 2 byte value). We are using this command to create the signature that ends the section.