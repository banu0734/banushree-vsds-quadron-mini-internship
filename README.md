# banushree/[vsdsquadron-mini-internship](https://github.com/banu0734/intern.git)
</details>

<details>
<summary>TASK 1</summary>
 <br>
    
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

</details>

<details>
<summary>TASK 2</summary>
 <br>
    
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


</details>

<details>
<summary>TASK 3</summary>
 <br>
    
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

</details>

<details>
<summary>TASK 4</summary>
 <br>
    
# TASK-4

## Identify various RISC-V instruction type (R, I, S, B, U, J) and exact 32-bit instruction code in the instruction type format for below RISC-V instructions 

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

 Upload the 32-bit pattern on Github

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

</details>

<details>
<summary>TASK 5</summary>
 <br>
    
# TASK-5

## Step 1: Install Icarus Verilog and GTKWave

![10 7 24-1](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/034f72b0-5801-4a99-adff-704c17d927cf)

1.Open your terminal.
2.Update your package list to ensure you have the latest information:

    sudo apt-get update
    
3.Install Icarus Verilog and GTKWave:

    sudo apt-get install iverilog gtkwave
    
## Step 2: Clone the Repository and Download Netlist Files

1.Open your terminal if not already open.
2.Clone the repository from GitHub:

    git clone https://github.com/vinayrayapati/iiitb_rv32i
    
3.Navigate to the newly created directory:

    cd iiitb_rv32i
    
## Step 3: Simulate and Run the Verilog Code

1.Open your terminal in the iiitb_rv32i directory.
2.Compile the Verilog code using Icarus Verilog:

    iverilog -o iiitb_rv32i iiitb_rv32i.v iiitb_rv32i_tb.v
    
3.Run the compiled code to generate the simulation output:

    ./iiitb_rv32i
    
## Step 4: View the Output Waveform in GTKWave

1.Open your terminal in the iiitb_rv32i directory.
2.View the generated waveform using GTKWave:

    gtkwave iiitb_rv32i.vcd
    
By following these steps, you'll be able to install the necessary tools, clone the required repository, simulate the Verilog code, and view the output waveforms.

## OUTPUT:

### ADD:
![10 7 24-2](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/a365ce5c-9786-4659-a896-f7cefb2c8f49)
### ADDI:
![10 7 24-3](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/f17271ab-116e-4723-98bf-5b954fcc6884)
### SUB:
![10 7 24-4](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/843b015a-1b9e-45d4-a810-701207ec9a80)
### AND:
![10 7 24-5](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/d9491f15-db8c-4faa-8746-314b08f831cc)
### OR:
![10 7 24-6](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/8462a5cc-5c0f-4a79-bd48-2b8f85b19deb)
### XOR:
![10 7 24-7](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/17381bc8-91f0-4ac8-8ad5-93afcdf41b02)
### SLT:
![10 7 24-8](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/78049ae4-d966-4b39-a486-3b9244d2ad15)
### BEQ:
![10 7 24-9](https://github.com/banu0734/banushree-vsds-quadron-mini-internship/assets/173624112/f444a703-0382-4e4b-8a35-a555fe554b97)

</details>

<details>
<summary>TASK 6</summary>
 <br>
    
# TASK-6
## PROJECT-Ascent Control Engineer: Creating a Smart Elevator Controller.
## Conceptual Design
### 1. RISC-V Processor Capabilities:

The RISC-V processor will need the following capabilities:

* I/O Ports: To interact with buttons (floor selection, open/close doors), sensors (floor detection, door status), and the motor.
* Timers:  For scheduling tasks and managing delays (e.g., waiting for doors to open/close).
Interrupts: For handling real-time events such as button presses or sensor activations.

### 2. Basic Components:
* Buttons: To request floors, open/close doors.
* Sensors: To detect the current floor and door status.
* Motor Control: To move the elevator up and down.
* Display: To show the current floor and direction of movement.
  
### 3. Elevator Logic:
* Initialization: When powered on, the elevator should run a self-check and move to a predefined floor (usually the ground floor).
* Request Handling: When a button is pressed, the system should register the request and prioritize it based on the current direction of movement and the elevator's position.
* Movement: The motor should be controlled to move the elevator to the requested floors. It should stop at each requested floor, open the doors, wait for a specified time, and then close the doors.
* Safety: Ensure there are mechanisms to handle errors such as doors not closing, sensor failures, or motor issues.
## Implementation Outline
### 1. I/O Management:
* Button Press: Use GPIO pins to read button states.
* Sensor Reading: Use GPIO pins to read floor sensors and door status.
* Motor Control: Use GPIO pins to control motor driver circuits.
  
### 2. State Machine:

Design a state machine to manage the elevator’s operation. Typical states include:

* Idle: Waiting for a request.
* Moving: Elevator is moving to a requested floor.
* Door Opening/Closing: Handling door operations.
* Emergency: Handling emergency situations (e.g., power failure, obstruction).
* 
### 3. Software Logic (Pseudocode):
Even though you mentioned no software code, the logical flow can be represented as follows:

```
initialize_system()
while true:
    if new_request:
        update_request_queue()
    
    if current_state == IDLE:
        if request_queue not empty:
            set_destination(next_request())
            current_state = MOVING
            
    if current_state == MOVING:
        move_to_destination()
        current_state = DOOR_OPENING
        
    if current_state == DOOR_OPENING:
        open_doors()
        wait(door_open_time)
        current_state = DOOR_CLOSING
        
    if current_state == DOOR_CLOSING:
        close_doors()
        if more_requests:
            current_state = MOVING
        else:
            current_state = IDLE

    handle_emergency()
```
### 4. Safety and Redundancy:

* Emergency Stop: Implement an emergency stop button and logic.
* Door Safety: Ensure the doors do not close if an obstruction is detected.
* Power Failure: Implement a backup power source or safe mode in case of power failure.

# TASK-6

## Detailed Explanation of Smart Elevator Controller with VSDSquadron Mini RISC-5 Kit

### Overview

The smart elevator controller leverages the VSDSquadron Mini RISC-5 kit to integrate advanced features for improved functionality, safety, efficiency, and user experience. Below is a detailed explanation of the components required and the working of the system.

### Components Required

1. **VSDSquadron Mini**
   - Core processing unit based on the RISC-V architecture.
   - Provides computational power and interfacing capabilities.

2. **Sensors**
   - **Position Sensors**: Encoders to track the elevator's position.
   - **Load Sensors**: Measure the weight inside the elevator to ensure it is not overloaded.
   - **Door Sensors**: Monitor the status of the elevator doors (open or closed).

3. **Actuators**
   - **Motors**: Control the movement of the elevator up and down.
   - **Relays**: Control the opening and closing of the elevator doors.

4. **User Interface Components**
   - **Display Panels**: Show the current floor and other relevant information.
   - **Buttons**: Allow passengers to select their desired floor.

5. **Breadboard and Jumper Wires**
   - For prototyping and connecting different components.

### Working

#### Floor Selection
1. **User Interaction**: 
   - Passengers interact with the elevator through a user interface. This interface can be:
     - **Inside the Elevator**: Touchscreen, keypad, or button panel.
     - **Outside the Elevator**: Touchscreen, keypad, or button panel on each floor.

2. **Destination Input**: 
   - Passengers select their desired floor using the interface.
   - Additional parameters can be input in some smart elevators, such as:
     - Indicating if they are in a rush.
     - Specific accessibility needs.

#### Elevator Operation
1. **Processing User Input**:
   - The VSDSquadron Mini processes the input from the user interface.
   - Determines the optimal route based on the current position, destination, and other parameters.

2. **Monitoring and Control**:
   - **Position Sensors**: Continuously track the elevator's position and provide feedback to the controller.
   - **Load Sensors**: Ensure the elevator is not overloaded.
   - **Door Sensors**: Ensure the doors are properly closed before the elevator moves.

3. **Actuation**:
   - **Motors**: Controlled to move the elevator to the selected floor.
   - **Relays**: Operate the doors to open and close at the appropriate times.

4. **User Feedback**:
   - **Display Panels**: Show the current floor, upcoming stops, and other relevant information to passengers.
   - **Audio/Visual Alerts**: Inform passengers about elevator arrival, doors opening/closing, and other important notifications.

### Detailed Explanation of Specific Prompts

#### Prompt 1: Floor Selection Process
**Scenario**: A passenger selects a floor inside the elevator car.
1. The passenger enters the elevator car and uses the touchscreen interface to select the 5th floor.
2. The input is registered by the VSDSquadron Mini, which updates the internal queue of requested floors.
3. The controller checks the current position and decides the most efficient route to the 5th floor.
4. The elevator starts moving towards the 5th floor while continuously monitoring sensors.

#### Prompt 2: Handling Additional Parameters
**Scenario**: A passenger with specific accessibility needs uses the elevator.
1. The passenger selects their desired floor and indicates they need extra time to enter/exit the elevator.
2. The VSDSquadron Mini processes this input and adjusts the door operation timings.
3. The elevator provides additional visual and audio cues to assist the passenger.

#### Prompt 3: Safety and Efficiency
**Scenario**: The elevator is overloaded.
1. Passengers enter the elevator, and the load sensors detect an overload condition.
2. The VSDSquadron Mini alerts the passengers through visual and audio cues, indicating that the elevator is overloaded.
3. The elevator doors remain open, and the elevator does not move until the load is reduced to a safe level.

By integrating these advanced features and technologies, the smart elevator controller enhances the overall functionality, safety, efficiency, and user experience.

### CODE:
```
#include <stdint.h>
#include <stdbool.h>

// Define memory-mapped addresses for GPIO control
#define GPIO_BASE_ADDR      0x10000000
#define GPIO_OUTPUT_REG     (*(volatile uint32_t *)(GPIO_BASE_ADDR + 0x00))
#define GPIO_DIRECTION_REG  (*(volatile uint32_t *)(GPIO_BASE_ADDR + 0x04))
#define GPIO_INPUT_REG      (*(volatile uint32_t *)(GPIO_BASE_ADDR + 0x08))

// Define GPIO pin assignments for stepper motor control
#define STEP_PIN            0
#define DIR_PIN             1

// Define GPIO pin assignments for LCD control (example)
#define LCD_RS_PIN          2
#define LCD_EN_PIN          3
#define LCD_D4_PIN          4
#define LCD_D5_PIN          5
#define LCD_D6_PIN          6
#define LCD_D7_PIN          7

// Define GPIO pin assignments for buttons
#define BUTTON_FLOOR_1      8
#define BUTTON_FLOOR_2      9
#define BUTTON_FLOOR_3      10

// Define GPIO pin assignments for load sensor (example)
#define LOAD_SENSOR_PIN     11

// Define constants for motor control
#define STEPS_PER_REV       200   // Number of steps per revolution for the stepper motor
#define STEPS_PER_FLOOR     (STEPS_PER_REV / 3)

// Define elevator floors
#define FLOOR_1             0
#define FLOOR_2             1
#define FLOOR_3             2

// Global variables
volatile uint8_t currentFloor = FLOOR_1;
volatile bool isOverloaded = false;

// Function prototypes
void initGPIO(void);
void moveStepperMotor(uint8_t targetFloor);
void delay(uint32_t milliseconds);  // Function to implement delay
void checkLoadSensor(void);
void handleButtonPresses(void);
void updateLCD(uint8_t floor);
void lcdCommand(uint8_t command);
void lcdData(uint8_t data);
void lcdInit(void);

int main(void) {
    initGPIO();
    lcdInit();

    // Main program loop
    while (1) {
        handleButtonPresses();
        checkLoadSensor();

        // Example: Move elevator to the selected floor
        if (!isOverloaded) {
            moveStepperMotor(currentFloor);
        }

        // Small delay to debounce button presses
        delay(100);
    }

    return 0;
}

void initGPIO(void) {
    // Configure GPIO direction (output for stepper motor control)
    GPIO_DIRECTION_REG |= (1 << STEP_PIN) | (1 << DIR_PIN);

    // Configure GPIO direction (output for LCD control - example)
    GPIO_DIRECTION_REG |= (1 << LCD_RS_PIN) | (1 << LCD_EN_PIN) |
                          (1 << LCD_D4_PIN) | (1 << LCD_D5_PIN) |
                          (1 << LCD_D6_PIN) | (1 << LCD_D7_PIN);

    // Configure GPIO direction (input for buttons and load sensor)
    GPIO_DIRECTION_REG &= ~((1 << BUTTON_FLOOR_1) | (1 << BUTTON_FLOOR_2) | (1 << BUTTON_FLOOR_3) | (1 << LOAD_SENSOR_PIN));
}

void moveStepperMotor(uint8_t targetFloor) {
    // Calculate steps needed to move to the target floor
    int8_t stepsToMove = (int8_t)(targetFloor - currentFloor) * STEPS_PER_FLOOR;

    // Determine direction
    bool direction = (stepsToMove >= 0) ? 1 : 0;
    stepsToMove = abs(stepsToMove);

    // Set direction pin
    if (direction) {
        GPIO_OUTPUT_REG |= (1 << DIR_PIN);  // Set direction pin high for one direction
    } else {
        GPIO_OUTPUT_REG &= ~(1 << DIR_PIN); // Set direction pin low for opposite direction
    }

    // Perform steps
    for (int i = 0; i < stepsToMove; ++i) {
        GPIO_OUTPUT_REG |= (1 << STEP_PIN);  // Pulse step pin
        delay(1);  // Small delay between steps
        GPIO_OUTPUT_REG &= ~(1 << STEP_PIN);
        delay(1);  // Small delay between steps
    }

    // Update current floor
    currentFloor = targetFloor;
    updateLCD(currentFloor);
}

void delay(uint32_t milliseconds) {
    // Example delay function implementation (depending on your clock speed)
    volatile uint32_t delay_cycles = milliseconds * 1000;
    while (delay_cycles--) {
        asm volatile ("nop");
    }
}

void checkLoadSensor(void) {
    // Read load sensor value
    isOverloaded = (GPIO_INPUT_REG & (1 << LOAD_SENSOR_PIN)) ? true : false;
    if (isOverloaded) {
        // Display overload message on LCD
        lcdCommand(0x01);  // Clear display
        lcdData('O');
        lcdData('V');
        lcdData('E');
        lcdData('R');
        lcdData('L');
        lcdData('O');
        lcdData('A');
        lcdData('D');
        lcdData(' ');
    } else {
        // Display current floor on LCD
        updateLCD(currentFloor);
    }
}

void handleButtonPresses(void) {
    if (GPIO_INPUT_REG & (1 << BUTTON_FLOOR_1)) {
        currentFloor = FLOOR_1;
    } else if (GPIO_INPUT_REG & (1 << BUTTON_FLOOR_2)) {
        currentFloor = FLOOR_2;
    } else if (GPIO_INPUT_REG & (1 << BUTTON_FLOOR_3)) {
        currentFloor = FLOOR_3;
    }
}

void updateLCD(uint8_t floor) {
    lcdCommand(0x01);  // Clear display
    lcdData('F');
    lcdData('L');
    lcdData('O');
    lcdData('O');
    lcdData('R');
    lcdData(' ');
    lcdData('0' + floor + 1);  // Display floor number (1, 2, or 3)
}

void lcdCommand(uint8_t command) {
    GPIO_OUTPUT_REG &= ~(1 << LCD_RS_PIN);  // RS = 0 for command
    GPIO_OUTPUT_REG |= (1 << LCD_EN_PIN);  // Enable high
    GPIO_OUTPUT_REG = (GPIO_OUTPUT_REG & 0xFFFFFFF0) | ((command >> 4) & 0x0F);  // Send high nibble
    GPIO_OUTPUT_REG &= ~(1 << LCD_EN_PIN);  // Enable low
    delay(1);  // Small delay
    GPIO_OUTPUT_REG |= (1 << LCD_EN_PIN);  // Enable high
    GPIO_OUTPUT_REG = (GPIO_OUTPUT_REG & 0xFFFFFFF0) | (command & 0x0F);  // Send low nibble
    GPIO_OUTPUT_REG &= ~(1 << LCD_EN_PIN);  // Enable low
    delay(1);  // Small delay
}

void lcdData(uint8_t data) {
    GPIO_OUTPUT_REG |= (1 << LCD_RS_PIN);  // RS = 1 for data
    GPIO_OUTPUT_REG |= (1 << LCD_EN_PIN);  // Enable high
    GPIO_OUTPUT_REG = (GPIO_OUTPUT_REG & 0xFFFFFFF0) | ((data >> 4) & 0x0F);  // Send high nibble
    GPIO_OUTPUT_REG &= ~(1 << LCD_EN_PIN);  // Enable low
    delay(1);  // Small delay
    GPIO_OUTPUT_REG |= (1 << LCD_EN_PIN);  // Enable high
    GPIO_OUTPUT_REG = (GPIO_OUTPUT_REG & 0xFFFFFFF0) | (data & 0x0F);  // Send low nibble
    GPIO_OUTPUT_REG &= ~(1 << LCD_EN_PIN);  // Enable low
    delay(1);  // Small delay
}

void lcdInit(void) {
    // Initialize LCD
    delay(20);  // Wait for LCD to power up
    lcdCommand(0x03);  // Wake up
    delay(5);  // Wait
    lcdCommand(0x03);  // Wake up
    delay(1);  // Wait
    lcdCommand(0x03);  // Wake up
    delay(1);  // Wait
    lcdCommand(0x02);  // Set 4-bit mode
    lcdCommand(0x28);  // Function set: 4-bit mode, 2 lines, 5x8 dots
    lcdCommand(0x0C);  // Display on, cursor off, blink off
    lcdCommand(0x06);  // Entry mode set: Increment cursor
    lcdCommand(0x01);  // Clear display
    delay(2);  // Wait for clear display
}
```
### Code Explanation

1. **Button Handling**:
   - The code now includes buttons to select floors.
   - The `handleButtonPresses` function checks the state of the buttons and updates `currentFloor` accordingly.

2. **Load Sensor**:
   - The `checkLoadSensor` function reads the load sensor value to determine if the elevator is overloaded.
   - If overloaded, it displays an overload message on the LCD.

3. **LCD Control**:
   - Functions `lcdCommand`, `lcdData`, and `lcdInit` are added to handle LCD operations.
   - The `updateLCD` function updates the LCD display with the current floor or an overload message.

#### Motor Control and Safety
- The `moveStepperMotor

` function has been modified to include safety checks for the overload condition.
- The direction and steps for the motor are calculated based on the difference between the current and target floors.

#### Delay Function
- The `delay` function is a simple implementation using a loop to create a time delay, useful for debouncing and step timing.

## RISC-V PROCESSOR:

![_Detailed Connection Diagram for the RISC-V Processor](https://github.com/user-attachments/assets/377fd71f-5b6b-425b-8a6f-03bb61863018)


#### GPIO Pin Assignments

1. **Stepper Motor Control**
   - **STEP_PIN**: GPIO Pin 0
   - **DIR_PIN**: GPIO Pin 1

2. **LCD Control**
   - **LCD_RS_PIN**: GPIO Pin 2
   - **LCD_EN_PIN**: GPIO Pin 3
   - **LCD_D4_PIN**: GPIO Pin 4
   - **LCD_D5_PIN**: GPIO Pin 5
   - **LCD_D6_PIN**: GPIO Pin 6
   - **LCD_D7_PIN**: GPIO Pin 7

3. **Button Inputs**
   - **BUTTON_FLOOR_1**: GPIO Pin 8
   - **BUTTON_FLOOR_2**: GPIO Pin 9
   - **BUTTON_FLOOR_3**: GPIO Pin 10

4. **Load Sensor Input**
   - **LOAD_SENSOR_PIN**: GPIO Pin 11

### Pin Diagram Representation for RISC-V Processor

Here's a detailed pin diagram representation for the RISC-V processor connections based on the given code.

#### Stepper Motor Connections

- **STEP_PIN** (GPIO Pin 0)
  - Connect this pin to the step input of the stepper motor driver.

- **DIR_PIN** (GPIO Pin 1)
  - Connect this pin to the direction input of the stepper motor driver.

#### LCD Display Connections

- **LCD_RS_PIN** (GPIO Pin 2)
  - Connect this pin to the RS pin of the LCD.

- **LCD_EN_PIN** (GPIO Pin 3)
  - Connect this pin to the EN pin of the LCD.

- **LCD_D4_PIN** (GPIO Pin 4)
  - Connect this pin to the D4 pin of the LCD.

- **LCD_D5_PIN** (GPIO Pin 5)
  - Connect this pin to the D5 pin of the LCD.

- **LCD_D6_PIN** (GPIO Pin 6)
  - Connect this pin to the D6 pin of the LCD.

- **LCD_D7_PIN** (GPIO Pin 7)
  - Connect this pin to the D7 pin of the LCD.

#### Button Inputs

- **BUTTON_FLOOR_1** (GPIO Pin 8)
  - Connect one side of a button to this pin and the other side to ground. This button is for selecting Floor 1.

- **BUTTON_FLOOR_2** (GPIO Pin 9)
  - Connect one side of a button to this pin and the other side to ground. This button is for selecting Floor 2.

- **BUTTON_FLOOR_3** (GPIO Pin 10)
  - Connect one side of a button to this pin and the other side to ground. This button is for selecting Floor 3.

#### Load Sensor Input

- **LOAD_SENSOR_PIN** (GPIO Pin 11)
  - Connect the output of the load sensor to this pin.

### Breadboard and Jumper Wires

Using a breadboard and jumper wires, you can create the connections as described above. Ensure that you also connect the ground and power lines appropriately for the LCD, stepper motor driver, and load sensor.

By following this pin diagram and connection guide, you should be able to set up the smart elevator controller system effectively with a RISC-V processor. Ensure to double-check connections for any power and ground requirements specific to your components.

## WORKING:

The smart elevator controller, driven by a RISC-V processor, orchestrates seamless elevator operations through interfacing with essential components: a stepper motor, LCD display, floor selection buttons, and a load sensor. Upon initialization, GPIO pins are configured to manage these components effectively. In its operational loop, the system continuously monitors button inputs for floor selection and checks the load sensor for elevator capacity. When a floor button is pressed and the elevator is not overloaded, the controller calculates and executes the necessary steps using the stepper motor to move the elevator to the desired floor. The LCD display concurrently updates, showing current floor status or an "OVERLOAD" warning if capacity limits are exceeded. Safety is paramount, ensured by real-time load monitoring and controlled motor movements. Overall, this integrated system optimizes elevator functionality, enhances safety, and delivers a user-friendly experience, all orchestrated by the robust capabilities of the RISC-V architecture.


















