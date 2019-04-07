# Chapter 1 - Before You Begin
Overview of concepts necessary to understand the rest of the book.  
Sample code (fragments): https://www.wiley.com/go/shellcodershandbook

## Basic Concepts
Need an understanding of:  
* computer languages
* operating systems
* architectures

Definitions:  
* Vulnerability (n.): A flaw in a system’s security that can lead to an
attacker utilizing the system in a manner other than the designer
intended. This can include impacting the availability of the system,
elevating access privileges to an unintended level, complete control
of the system by an unauthorized party, and many other possibilities.
Also known as a security hole or security bug.
* Exploit (v.); To take advantage of a vulnerability so that the target
system reacts in a manner other than which the designer intended.
* Exploit (n.): The tool, set of instructions, or code that is used to take
advantage of a vulnerability. Also known as a Proof of Concept (POC).
* 0day (n.): An exploit for a vulnerability that has not been publicly dis-
closed. Sometimes used to refer to the vulnerability itself.
Fuzzer (n.): A tool or application that attempts all, or a wide range of,
unexpected input values to a system. The purpose of a fuzzer is to
determine whether a bug exists in the system, which could later be
exploited without having to fully know the target system’s internal
functioning.

## Memory management
Need to understand 32-bit Intel architecture (IA32).  

When a program is executed:
1. OS creates an address space in which the program will run.
2. Information from the executable's file is loaded into the address space. (see segments below)
3. stack and heap are initialised. (see below)

### Segments
* .data segment \[writeable]: contains static initialised data; used for global variables.
* .bss segment \[writeable]: contains uninitialised data; used for global variables.
* .text segment \[read-only]: contains program instructions

### Stack
= LIFO data structure used to store:
* local variables
* information related to function calls
* information needed to clean up stack after a function returns

The stack grows towards lower memory addresses.

### Heap
= (roughly FIFO) data structure used to store dynamic variables.

The heap grows towards higher memory addresses.

### Memory space diagram
```
↑ Lower addresses (0x08000000)
Shared libraries
.text
.bss
Heap (grows ↓)
Stack (grows ↑)
env pointer
Argc
↓ Higher addresses (0xbfffffff)
```
### Further reading
* First half of Chapter 15
* https://linux-mm.org

## Assembly
This book focuses on IA32, but also covers discovery and exploitation on other architectures.  
Need to understand:
* number systems
* data sizes
* number sign representations

## Registers
= Memory (usually directly connected to circuitry) responsible for manipulations that allow modern computers to function, and which can be manipulated with assembly instructions.
Four categories:
* General purpose
* Segment
* Control
* Other

### General purpose registers
Used to perform common mathematical operations, store data and addresses, offset addresses, perform counting, etc..  
Example: EAX, EBX, ECX  
Special register: ESP (Extended Stack Pointer) = points to memory address where the next stack operation will take place.

### Segment registers
16-bit. Used to keep track of segments and allow backwards compatibility with 16-bit applications.

### Control registers
Used to control the function of the processor.  
Most important one: EIP (Extended Instruction Pointer) = contains the address of the next machine instrcution to be executed.

### Other registers
Misc.  
Example: EFLAGS (Extended Flags) = comprises many single-bit registers used to store the results of various tets performed by the processor.

## Recognizing C and C++ Code Constructs in Assembly
Solid understanding of C is critical for vulnerability development.  
Book contains 4 examples of C code and the associated assembly code.
