# Sokoban

A console-based implementation of the classic Sokoban puzzle game written in C, with automatic player pathfinding and support for undoing previous pushes.

---

## What is Sokoban?

This implementation uses a different control scheme than a traditional game. Instead of controlling the player step by step, the user selects a crate and the direction in which it should be pushed. The program automatically finds a path for the player to the position required to perform the push.

The game reads an initial board configuration from standard input and then processes commands until the end-of-input marker is reached.

---

## Features

- Dynamic board representation
- Loading boards of arbitrary dimensions
- Crates identified by unique letters
- Automatic player pathfinding
- Breadth-first search (BFS) for finding reachable player positions
- Push validation based on the board state
- Undoing previously completed pushes
- Full board-state history
- Dynamic memory allocation
- Console-based input and output
- Example input and expected-output files for testing

---

## Technologies

### Language and Tools

- C
- GCC
- CMake
- Standard C library
- Dynamic memory allocation

### Data Structures and Algorithms

- Dynamic arrays
- Linked lists
- Stacks
- Queues
- Breadth-first search (BFS)
- 2D board representation

---

## Board Representation

Each board cell is represented by a single ASCII character:

| Symbol | Meaning |
| --- | --- |
| `-` | Empty non-goal cell |
| `+` | Empty goal cell |
| `#` | Wall |
| `@` | Player on a non-goal cell |
| `*` | Player on a goal cell |
| `a`–`z` | Crate on a non-goal cell |
| `A`–`Z` | Crate on a goal cell |

Lowercase and uppercase letters identify the same crate. For example, `a` and `A` represent the same crate, with the latter indicating that the crate is currently on a goal.

The board is stored dynamically as an array of dynamically allocated strings, allowing rows of different lengths.

---

## Game Commands

Commands are read from standard input after the initial board description.

| Command | Description |
| --- | --- |
| Empty line | Print the current board |
| `[a-z][2/4/6/8]` | Push the selected crate in the specified direction |
| `0` | Undo the most recent successful push |
| `.` | End the command sequence |

Directions use the following notation:

| Key | Direction |
| --- | --- |
| `2` | Down |
| `4` | Left |
| `6` | Right |
| `8` | Up |

If a requested push is impossible, for example because the destination is outside the board or occupied by another crate or a wall, the command has no effect.

---

## Automatic Pathfinding

The player does not have to be moved manually.

When a push command is received, the program determines the position from which the player needs to push the selected crate. It then searches for a path from the player's current position to that position.

The search considers only cells that the player can currently move through and treats the selected crate as an obstacle.

A breadth-first search is used to find a reachable position:

```text
Player position
      │
      ▼
 Breadth-first search
      │
      ▼
Required pushing position
      │
      ▼
    Push crate
```

This separates player movement from crate movement: the command specifies only the intended push, while the program determines how the player gets there.

---

## Undo System

The game keeps track of successful pushes so that previous states can be restored.

Each successful push stores a copy of the board state. The history is maintained using a stack, so the most recent state is restored first.

```text
Current state
     │
     ▼
Board state stack
     │
     ├── most recent state
     ├── previous state
     └── older states
```

When the `0` command is used, the most recent successful push is undone and the corresponding previous board state is restored.

If there is no previous successful push, the command has no effect.

---

## Memory Management

The board and its history are dynamically allocated.

The implementation manages memory for:

- board rows,
- board copies used by the undo system,
- command buffers,
- stack nodes,
- BFS queue storage.

Temporary structures are released after use, and board states stored in the undo history are freed when they are removed from the stack.

---

## Building

The project uses CMake.

Create a build directory:

```bash
mkdir build
cd build
```

Configure the project:

```bash
cmake ..
```

Build the executable:

```bash
cmake --build .
```

The resulting executable is:

```text
sokoban
```

---

## Running

Run the program from the build directory:

```bash
./sokoban
```

The board and subsequent commands are read from standard input.

For example, if the initial board and commands are stored in a file:

```bash
./sokoban < input.txt
```

---

## Example Tests

The `examples/` directory contains sample input and expected output files.

A test can be run with:

```bash
./sokoban < examples/przyklad1.in > output.txt
```

The generated output can then be compared with the expected result:

```bash
diff output.txt examples/przyklad1.out
```

The same procedure can be used for the other provided examples.

---

## Project Structure

```text
.
├── examples/
│   ├── przyklad1.in
│   ├── przyklad1.out
│   ├── przyklad2.in
│   ├── przyklad2.out
│   ├── przyklad3.in
│   └── przyklad3.out
├── CMakeLists.txt
├── main.c
└── README.md
```

### Implementation

The main implementation is contained in:

- `main.c` — board management, command processing, pathfinding, crate movement, undo history, and memory management.

### Examples

The `examples/` directory contains input/output pairs used to demonstrate and test the program.

---

## Academic Context

This project was developed as a course assignment for **Introduction to Informatics** at the University of Warsaw during the Winter Semester 2024/25.
