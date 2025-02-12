# Minesweeper AI with Deductive Inference

This project is part of Harvard's CS50 Introduction to Artificial Intelligence with Python course. It implements a Minesweeper game with an AI that uses a knowledge-based deductive inference approach to deduce safe moves and identify mines efficiently.

## Table of Contents

- [Minesweeper AI with Deductive Inference](#minesweeper-ai-with-deductive-inference)
  - [Table of Contents](#table-of-contents)
  - [About](#about)
  - [Project Structure](#project-structure)
  - [Requirements](#requirements)
  - [Running the Game](#running-the-game)
  - [Game Logic](#game-logic)
  - [Knowledge Representation and Inference](#knowledge-representation-and-inference)
  - [Deductive Inference Process](#deductive-inference-process)
  - [Knowledge Base Updates](#knowledge-base-updates)
  - [Code Implementation](#code-implementation)

## About

This project implements a Minesweeper game for Harvard's CS50 Introduction to Artificial Intelligence with Python course. The AI uses a knowledge-based deductive inference approach to represent board data as logical sentences, enabling it to deduce safe moves and identify mines effectively.

## Project Structure

```plaintext
.gitignore
assets/
  fonts/OpenSans-Regular.ttf
  images/flag.png
  images/mine.png
requirements.txt
runner.py
minesweeper.py
```

- **.gitignore**: Specifies files and directories to be ignored by git.
- **assets/**: Contains fonts and images used in the game interface.
  - **assets/fonts/OpenSans-Regular.ttf**: Font file used for text rendering.
  - **assets/images/flag.png**: Image representing a flagged cell.
  - **assets/images/mine.png**: Image representing a mine.
- **requirements.txt**: Lists the Python dependencies required for the project.
- **runner.py**: Main script to run the Minesweeper game with a graphical interface using Pygame.
- **minesweeper.py**: Contains the game logic and AI implementation, including the knowledge-based deductive inference approach.

## Requirements

- Python 3.x
- Pygame

Install the required dependencies using:
```sh
pip3 install -r requirements.txt
```

## Running the Game

To start the game, run the `runner.py` script:
```sh
python runner.py
```

## Game Logic

The game logic is implemented in `minesweeper.py`. Key components include:

- **Minesweeper Class**: Manages board creation, mine placement, and overall game state.
- **Sentence Class**: Represents a logical sentence about a set of cells and the number of mines among them.
- **MinesweeperAI Class**: Uses the knowledge base to deduce safe moves and identify mines by updating and inferring new information from the board.

## Knowledge Representation and Inference

The AI represents board data as logical sentences. Each sentence comprises a set of cells and a count of mines among them. This structure allows the AI to:

- Deduce safe cells when the count is zero.
- Identify mines when the number of cells equals the mine count.
- Infer new sentences from existing ones to refine its knowledge base.

## Deductive Inference Process

When a safe cell is revealed, the AI:

- Marks the cell as a move made.
- Updates the knowledge base with a new sentence reflecting the neighboring cells and mine count.
- Iteratively applies inference rules to mark additional cells as safe or as mines.

## Knowledge Base Updates

The AI continuously refines its knowledge base by:

- Marking cells as safe or as mines within existing sentences.
- Adding new sentences derived from the current board state.
- Inferring new information through logical relationships between sentences.

## Code Implementation

Key functions in `minesweeper.py` include:

- `add_knowledge(cell, count)`: Updates the knowledge base when a safe cell is revealed.
- `make_safe_move()`: Returns a move that is known to be safe.
- `make_random_move()`: Returns a random move when no safe move is available.

These functions work together to enable the AI to make intelligent decisions based on deductive inference, ensuring efficient gameplay.
