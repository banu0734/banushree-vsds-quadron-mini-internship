# banushree/[vsdsquadron-mini-internship](https://github.com/banu0734/intern.git)
# Task-1
# Opening and writting program in terminal window.
writing a C programming code to count from 1 to n.
initializing and creating a leafpad named assignment1
![assignment1-1](https://github.com/banu0734/intern/assets/173624112/e0fdbf69-1541-486e-be7b-454f7a13ec25)
writting a code in leafpad for sum of 1 to 5
![assignment1-2](https://github.com/banu0734/intern/assets/173624112/ffb2c678-6465-4aa5-8e97-3cec24665156)
output:
![assignment1-3](https://github.com/banu0734/intern/assets/173624112/54d42f92-9185-4459-b47e-470f468e8195)
writting a code for sum of 1 to 100.
![assignment1-4](https://github.com/banu0734/intern/assets/173624112/1d9857e1-97de-48b5-9dc6-c0f660304a57)
output:
![assignment1-5](https://github.com/banu0734/intern/assets/173624112/676e4942-15ac-4e61-9717-fb611dda97cd)
# Running same program in RISC-V simulator
![assignment1-6](https://github.com/banu0734/intern/assets/173624112/319a80f3-e262-4703-b1c4-a29804fbeb28)
![assignment1-7](https://github.com/banu0734/intern/assets/173624112/679e9f14-8f41-4727-83ae-915558c34f50)
![assignment1-9](https://github.com/banu0734/intern/assets/173624112/40201ba9-d93b-471e-985c-7bc6f4554aee)
![assignment1-8](https://github.com/banu0734/intern/assets/173624112/7f70bbb0-8b14-4dd2-a515-24aedec665de)

# Task-2
# CODE

```

#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h> // for abs function

#define NUM_FLOORS 10
#define NUM_ELEVATORS 3

// Define states for each elevator
typedef enum {
    UP,
    IDLE,
    DOWN,
    STOPPED
} ElevatorState;

// Elevator structure
typedef struct {
    int currentFloor;
    ElevatorState state;
} Elevator;

Elevator elevators[NUM_ELEVATORS];

// Function to initialize elevators
void initializeElevators() {
    for (int i = 0; i < NUM_ELEVATORS; ++i) {
        elevators[i].currentFloor = 0;
        elevators[i].state = IDLE;
    }
}

// Function to move an elevator to a specific floor
void moveElevator(Elevator *elevator, int targetFloor) {
    if (elevator->currentFloor < targetFloor) {
        elevator->state = UP;
        while (elevator->currentFloor < targetFloor) {
            elevator->currentFloor++;
            printf("Elevator is moving up, current floor: %d\n", elevator->currentFloor);
        }
        // In a real scenario, update elevator position in hardware
        elevator->state = STOPPED;
    } else if (elevator->currentFloor > targetFloor) {
        elevator->state = DOWN;
        while (elevator->currentFloor > targetFloor) {
            elevator->currentFloor--;
            printf("Elevator is moving down, current floor: %d\n", elevator->currentFloor);
        }
        // In a real scenario, update elevator position in hardware
        elevator->state = STOPPED;
    }
    printf("Elevator arrived at floor %d\n", elevator->currentFloor);
}

// Function to assign an elevator to a floor request
void assignElevator(int floor) {
    // Simple algorithm: find the closest idle elevator
    int minDistance = NUM_FLOORS + 1; // Initialize to a value larger than any possible distance
    Elevator *closestElevator = NULL;

    for (int i = 0; i < NUM_ELEVATORS; ++i) {
        if (elevators[i].state == IDLE) {
            int distance = abs(elevators[i].currentFloor - floor);
            if (distance < minDistance) {
                minDistance = distance;
                closestElevator = &elevators[i];
            }
        }
    }

    if (closestElevator != NULL) {
        moveElevator(closestElevator, floor);
    } else {
        printf("No available elevators to serve floor %d\n", floor);
    }
}

int main() {
    initializeElevators();

    // Example: simulate requests to floors
    assignElevator(5);
    assignElevator(8);
    assignElevator(2);
    assignElevator(6);

    return 0;
}

```
# Detailed explaination and breakdown of the code
## Header Files and Definitions

```
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h> // for abs function

#define NUM_FLOORS 10
#define NUM_ELEVATORS 3
```
### Header Files:

stdio.h: For standard input/output functions, such as printf.
stdbool.h: For Boolean type (true and false), although it's not directly used in this code.
stdlib.h: For standard library functions, specifically the abs function which calculates the absolute value of an integer.

### Constants:

NUM_FLOORS: Defines the number of floors in the building (10 floors).
NUM_ELEVATORS: Defines the number of elevators in the system (3 elevators).

## Enumeration and Structure Definitions

```
typedef enum {
    UP,
    IDLE,
    DOWN,
    STOPPED
} ElevatorState;
```
### ElevatorState Enum: Defines possible states of an elevator:
* UP: Elevator is moving up.
* IDLE: Elevator is not moving and is waiting for a request.
* DOWN: Elevator is moving down.
* STOPPED: Elevator has stopped moving.

```
typedef struct {
    int currentFloor;
    ElevatorState state;
} Elevator;

Elevator elevators[NUM_ELEVATORS];
```
### Elevator Struct: Defines the properties of an elevator:

      * *currentFloor:* The current floor where the elevator is located.
      * state: The current state of the elevator, using the ElevatorState enum.
### Elevators Array:

An array of NUM_ELEVATORS (3) Elevator structures to represent each elevator in the system.

## Function to Initialize Elevators
```
void initializeElevators() {
    for (int i = 0; i < NUM_ELEVATORS; ++i) {
        elevators[i].currentFloor = 0;
        elevators[i].state = IDLE;
    }
}
```
### initializeElevators:

This function initializes all elevators to be at the ground floor (floor 0) and sets their state to IDLE.

## Function to Move an Elevator to a Specific Floor
```
void moveElevator(Elevator *elevator, int targetFloor) {
    if (elevator->currentFloor < targetFloor) {
        elevator->state = UP;
        while (elevator->currentFloor < targetFloor) {
            elevator->currentFloor++;
            printf("Elevator is moving up, current floor: %d\n", elevator->currentFloor);
        }
        elevator->state = STOPPED;
    } else if (elevator->currentFloor > targetFloor) {
        elevator->state = DOWN;
        while (elevator->currentFloor > targetFloor) {
            elevator->currentFloor--;
            printf("Elevator is moving down, current floor: %d\n", elevator->currentFloor);
        }
        elevator->state = STOPPED;
    }
    printf("Elevator arrived at floor %d\n", elevator->currentFloor);
}
```
### moveElevator:

*Parameters:* Takes a pointer to an Elevator and the targetFloor to which the elevator should move.

*Logic:* 

       * If the currentFloor of the elevator is less than the targetFloor, it sets the state to UP and increments the currentFloor until it reaches the targetFloor.
       * If the currentFloor is greater than the targetFloor, it sets the state to DOWN and decrements the currentFloor until it reaches the targetFloor.
       * Once the elevator reaches the target floor, it sets the state to STOPPED.
       * Throughout the process, it prints the current floor of the elevator to simulate movement.

## Function to Assign an Elevator to a Floor Request

```
void assignElevator(int floor) {
    int minDistance = NUM_FLOORS + 1; // Initialize to a value larger than any possible distance
    Elevator *closestElevator = NULL;

    for (int i = 0; i < NUM_ELEVATORS; ++i) {
        if (elevators[i].state == IDLE) {
            int distance = abs(elevators[i].currentFloor - floor);
            if (distance < minDistance) {
                minDistance = distance;
                closestElevator = &elevators[i];
            }
        }
    }

    if (closestElevator != NULL) {
        moveElevator(closestElevator, floor);
    } else {
        printf("No available elevators to serve floor %d\n", floor);
    }
}
```

### assignElevator:
* *Parameter:* Takes the floor where an elevator is requested.
* *Logic:*

          * Initializes minDistance to a value larger than any possible distance (i.e., NUM_FLOORS + 1).
          * Iterates through each elevator to find the closest idle elevator to the requested floor.
          * If an idle elevator is found that is closer than the current minDistance, updates minDistance and * sets closestElevator to this elevator.
          * If a closest idle elevator is found, calls moveElevator to move it to the requested floor.
          * If no idle elevator is available, prints a message indicating no elevators are available.
  
## Main Function
```
int main() {
    initializeElevators();

    // Example: simulate requests to floors
    assignElevator(5);
    assignElevator(8);
    assignElevator(2);
    assignElevator(6);

    return 0;
}
```
### main:

    * *Initialization:* Calls initializeElevators to set all elevators to the ground floor and state to IDLE.
    * *Simulated Requests:* Simulates requests to floors 5, 8, 2, and 6 by calling assignElevator with these floor numbers.
    * *Return:* Ends the program with return 0.

## Typing the code in Leafpad

![task2-1](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/6137bf1e-8f42-4f4e-9608-f077a2eb89a7)

## Code written in Leafpad

![task2-1](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/ecae2a1b-8b1c-489a-bc5c-8e9edbd886b2)

![task2-2](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/bdbbe7b5-2ddc-4a5c-9852-104aae7e9290)

![task2-3](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/5ddad748-cc8b-4018-8485-7b2a1f9c4b5d)

![task2-4](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/a5fd07ab-b035-4537-a0f1-b8c7f7dd3266)

![task2-5](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/9569b3df-2896-4844-993c-18ec40ab2cfa)

![task2-6](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/21ec0ff2-4cd8-4e14-b1ca-0915961c4af9)

### OUTPUT OBTAINED:

![task2-7](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/2feebd2c-4061-484a-a4d4-bc30dcdb621e)


## Initializing the code in RISC-V GCC

![task2-8r](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/ac81c7b5-e849-41b0-8ffb-febbeaab8cda)

![task2-9r](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/2bbaeedc-320b-4b5d-9035-ac26bc79edb6)

![task2-10r](https://github.com/banu0734/intern-vsds-quadron-mini-internship/assets/173624112/18aa9ea4-b06c-4eb8-bcc6-08ce800a1dd4)

# TASK-3

## SPIKE Simulation and observation with -O1 and -Ofast. Upload snapshot of compiled C Code, RISC-V Objdmp with above options on your GitHub repo

![task3-1](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/aa4a7cb4-3c2c-41e0-8ab8-7080997bcc43)

*This image shows a terminal window in an Ubuntu operating system with a code snippet and some terminal commands executed.*

## Terminal Commands:

### riscv64-unknown-elf-gcc -O1 -mabi=lp64 -march=rv64i -o task2.0 task2.c
    *This command compiles the C code (task2.c) using the riscv64-unknown-elf-gcc compiler for a RISC-V 64-bit architecture.
    *-O1 is a compiler optimization level.
    *-mabi=lp64 specifies the ABI (Application Binary Interface) used.
    *-march=rv64i specifies the target architecture (RISC-V 64-bit).
    *-o task2.0 specifies the output file name (task2.0).
### ls -ltr task2.0

This command lists the details of the task2.0 file in long format, sorted by modification time in reverse order.

![task3-2](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/b4c3a766-461f-4835-a74c-ee9c619ef247)

*The images show a terminal window with disassembled code, most likely from a RISC-V assembly program, along with a calculator application performing hexadecimal calculations.*

![task3-3](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/1b0a5f15-8799-4bd3-bb59-d134ce733e92)

### Calculator Application:

    *The calculator is being used in programming mode to perform hexadecimal arithmetic.
    *10314 - 102D8 = 3C: This subtraction calculates the difference between the addresses 10314 and 102D8, which equals 3C in hexadecimal.
    *3C ÷ 4 = F: This division calculates 3C divided by 4, which equals F in hexadecimal.

These disassembly snippets and calculations indicate the process of analyzing and manipulating low-level assembly code, likely for debugging or reverse engineering purposes. The use of the calculator in hexadecimal mode is common in such tasks to understand address offsets and instruction lengths.
## Same Operation in -Ofast optimization level

![task3-4](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/f30c612f-3da2-4c8c-90f3-5405c49b594a)

### If the code were compiled with the -Ofast optimization level, it would mean that the compiler uses the highest optimization level available, including aggressive optimizations that might break strict compliance with some language standards but aim for maximum performance.

### Terminal Command:

    *riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o task2.0 task2.c
    *-Ofast is used instead of -O1.
    *This enables all -O3 optimizations and more, disregarding strict standards compliance in favor of performance.

### Explanation:


    *riscv64-unknown-elf-gcc: The compiler use
    *-mabi=lp64: Specifies the ABI.
    *-march=rv64i: Specifies the target architecture.
    *-o task2.0: Specifies the output file name.
    
### Effects of -Ofast Optimization:

More Aggressive Optimizations: The -Ofast option enables more aggressive optimizations compared to -O1.

### Impact on Performance:

* In general, -Ofast should produce faster executables compared to -O1, as it applies more optimizations.
* This can be especially beneficial in performance-critical applications like an elevator control system where quick response times are crucial.
  
![task3-5](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/07ead424-f568-4a48-86c1-1449eeaa1316)

* When the code is compiled with the -Ofast optimization flag, it generally means that the compiler will apply aggressive optimizations to make the code run as fast as possible. This can result in significant changes in the structure and content of the generated assembly code compared to a non-optimized or less-optimized build.

* Let's analyze the main function with the starting address at 100b0 and the next instruction address at 10108 in an -Ofast optimized build.
  
![task3-6](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/e53a12e3-428d-472f-8910-57e0f902cf5b)

### Address Calculation
* The second image suggests using hexadecimal arithmetic to determine instruction addresses. For instance:

* If main starts at 100b0, and the next significant block starts at 10108, the difference (10108 - 100b0) is 58 (in hex).
Dividing 58 by 4 gives 16 (in hex), which means there are 22 (decimal) instructions between these points.

# TASK-4

## "Identify various RISC-V instruction type (R, I, S, B, U, J) and exact 32-bit instruction code in the instruction type format for below RISC-V instructions 
ADD r6, r2, r1
SUB r7, r1, r2
AND r8, r1, r3
OR r9, r2, r5
XOR r10, r1, r4
SLT r11, r2, r4
ADDI r12, r4, 5
SW r3, r1, 2
SRL r16, r14, r2
BNE r0, r1, 20
BEQ r0, r0, 15
LW r13, r1, 2
SLL r15, r1, r2

Upload the 32-bit pattern on Github"

## Detailed Explanation of RISC-V Instruction Types:

The RISC-V architecture supports several types of instructions, each with a specific format for encoding the instruction fields. The primary types are R, I, S, B, U, and J. Here's a detailed breakdown of each type, including the fields they use and their bit positions within a 32-bit instruction.

### 1. R-Type Instructions

R-type instructions are used for register-register operations.

* Format: funct7 rs2 rs1 funct3 rd opcode
* Bit positions: [31:25] [24:20] [19:15] [14:12] [11:7] [6:0]

| Field	| Bits | Description |
| --- | --- | --- |
| opcode	| 7	| Specifies the operation type (e.g., 0110011 for R-type) |
| rd	| 5	| Destination register |
| funct3	| 3 |	Operation modifier |
| rs1 |	5	| First source register |
| rs2 |	5	| Second source register |
| funct7 |	7 |	Operation modifier |

### 2. I-Type Instructions

I-type instructions are used for immediate operations, load operations, and some system instructions.

* Format: imm[11:0] rs1 funct3 rd opcode
* Bit positions: [31:20] [19:15] [14:12] [11:7] [6:0]
  
| Field |	Bits | Description |
| --- | --- | --- |
| opcode |	7 | Specifies the operation type (e.g., 0010011 for arithmetic immediates) |
| rd |	5 |	Destination register |
| funct3 |	3 |	Operation modifier |
| rs1 |	5 |	Source register |
| imm |	12 |	Immediate value |

### 3. S-Type Instructions

S-type instructions are used for store operations.

* Format: imm[11:5] rs2 rs1 funct3 imm[4:0] opcode
* Bit positions: [31:25] [24:20] [19:15] [14:12] [11:7] [6:0]
  
| Field |	Bits |	Description |
| --- | --- | --- |
|opcode |	7	 |Specifies the operation type (e.g., 0100011 for stores) |
| imm |	7 + 5	| Immediate value split into two parts (imm[11:5] and imm[4:0]) |
| funct3 |	3	| Operation modifier |
| rs1	| 5	| Base register |
| rs2	| 5	| Source register |

### 4. B-Type Instructions

B-type instructions are used for conditional branch operations.

* Format: imm[12] imm[10:5] rs2 rs1 funct3 imm[4:1] imm[11] opcode
* Bit positions: [31] [30:25] [24:20] [19:15] [14:12] [11:8] [7] [6:0]
  
| Field | 	Bits |	Description |
| --- | --- | --- |
| opcode |	7	| Specifies the operation type (e.g., 1100011 for branches) |
| imm |	1 + 6 + 4 + 1	| Immediate value split into four parts |
| funct3 |	3	| Operation modifier |
| rs1	| 5 | 	First source register |
| rs2	| 5	| Second source register |

### 5. U-Type Instructions

U-type instructions are used for upper immediate operations.

* Format: imm[31:12] rd opcode
* Bit positions: [31:12] [11:7] [6:0]
  
| Field |	Bits |	Description |
| --- | --- | --- |
| opcode |	7	| Specifies the operation type (e.g., 0110111 for LUI) |
| rd |	5	| Destination register |
| imm	| 20	| Immediate value (upper 20 bits) |

### 6. J-Type Instructions

J-type instructions are used for jump operations.

* Format: imm[20] imm[10:1] imm[11] imm[19:12] rd opcode
* Bit positions: [31] [30:21] [20] [19:12] [11:7] [6:0]

| Field	| Bits |	Description |
| --- | --- | --- |
| opcode	| 7 | 	Specifies the operation type (e.g., 1101111 for jumps) |
| imm	| 1 + 10 + 1 + 8 |	Immediate value split into four parts |
| rd	| 5 |	Destination register |

## exact 32-bit instruction code in the instruction type format

### R-Type Instructions

1. ADD r6, r2, r1

* Opcode: 0110011
* Funct3: 000
* Funct7: 0000000
* rs1: 00010 (r2)
* rs2: 00001 (r1)
* rd: 00110 (r6)
* Instruction: 0000000 00001 00010 000 00110 0110011
* Hexadecimal: 0x006101b3
  
2. SUB r7, r1, r2

* Opcode: 0110011
* Funct3: 000
* Funct7: 0100000
* rs1: 00001 (r1)
* rs2: 00010 (r2)
* rd: 00111 (r7)
* Instruction: 0100000 00010 00001 000 00111 0110011
* Hexadecimal: 0x406081b3
  
3. AND r8, r1, r3

* Opcode: 0110011
* Funct3: 111
* Funct7: 0000000
* rs1: 00001 (r1)
* rs2: 00011 (r3)
* rd: 01000 (r8)
* Instruction: 0000000 00011 00001 111 01000 0110011
* Hexadecimal: 0x003081b3
  
4. OR r9, r2, r5

* Opcode: 0110011
* Funct3: 110
* Funct7: 0000000
* rs1: 00010 (r2)
* rs2: 00101 (r5)
* rd: 01001 (r9)
* Instruction: 0000000 00101 00010 110 01001 0110011
* Hexadecimal: 0x005101b3

5. XOR r10, r1, r4

* Opcode: 0110011
* Funct3: 100
* Funct7: 0000000
* rs1: 00001 (r1)
* rs2: 00100 (r4)
* rd: 01010 (r10)
* Instruction: 0000000 00100 00001 100 01010 0110011
* Hexadecimal: 0x004081b3

6. SLT r11, r2, r4

* Opcode: 0110011
* Funct3: 010
* Funct7: 0000000
* rs1: 00010 (r2)
* rs2: 00100 (r4)
* rd: 01011 (r11)
* Instruction: 0000000 00100 00010 010 01011 0110011
* Hexadecimal: 0x004101b3

7. SLL r15, r1, r2

* Opcode: 0110011
* Funct3: 001
* Funct7: 0000000
* rs1: 00001 (r1)
* rs2: 00010 (r2)
* rd: 01111 (r15)
* Instruction: 0000000 00010 00001 001 01111 0110011
* Hexadecimal: 0x002081b3

### I-Type Instructions

1. ADDI r12, r4, 5

* Opcode: 0010011
* Funct3: 000
* Immediate: 000000000101 (5)
* rs1: 00100 (r4)
* rd: 01100 (r12)
* Instruction: 000000000101 00100 000 01100 0010011
* Hexadecimal: 0x00520293

2. LW r13, r1, 2

* Opcode: 0000011
* Funct3: 010
* Immediate: 000000000010 (2)
* rs1: 00001 (r1)
* rd: 01101 (r13)
* Instruction: 000000000010 00001 010 01101 0000011
* Hexadecimal: 0x00208083
  
### S-Type Instructions

1. SW r3, r1, 2
   
* Opcode: 0100011
* Funct3: 010
* Immediate: 000000000010 (2, split into imm[11:5] and imm[4:0])
* imm[11:5]: 0000000
* imm[4:0]: 00010
* rs1: 00001 (r1)
* rs2: 00011 (r3)
* Instruction: 0000000 00011 00001 010 00010 0100011
* Hexadecimal: 0x00208223

### B-Type Instructions

1. BNE r0, r1, 20

* Opcode: 1100011
* Funct3: 001
* Immediate: 0000000000100 (20, split into imm[12], imm[10:5], imm[4:1], and imm[11])
* imm[12]: 0
* imm[10:5]: 000001
* imm[4:1]: 0100
* imm[11]: 0
* rs1: 00001 (r1)
* rs2: 00000 (r0)
* Instruction: 000001 00000 00001 001 0100 1100011
* Hexadecimal: 0x01410063

2. BEQ r0, r0, 15

* Opcode: 1100011
* Funct3: 000
* Immediate: 0000000000111 (15, split into imm[12], imm[10:5], imm[4:1], and imm[11])
* imm[12]: 0
* imm[10:5]: 000001
* imm[4:1]: 0111
* imm[11]: 0
* rs1: 00000 (r0)
* rs2: 00000 (r0)
* Instruction: 000001 00000 00000 000 0111 1100011
* Hexadecimal: 0x00700063

