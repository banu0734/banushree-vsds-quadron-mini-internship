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
