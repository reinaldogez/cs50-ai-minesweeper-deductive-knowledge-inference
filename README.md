## Table of Contents

- [Table of Contents](#table-of-contents)
- [About](#about)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Running the Game](#running-the-game)
- [Game Logic](#game-logic)
  - [The Purpose of the `Sentence` Class](#the-purpose-of-the-sentence-class)
- [Knowledge Representation and Inference](#knowledge-representation-and-inference)
- [Deductive Inference Process](#deductive-inference-process)
- [Knowledge Base Updates](#knowledge-base-updates)
- [Code Implementation](#code-implementation)
  - [The add\_knowledge Method](#the-add_knowledge-method)
  - [The update\_knowledge Method](#the-update_knowledge-method)

## About

This project implements a Minesweeper game for **Harvard's CS50 Introduction to Artificial Intelligence with Python** course. The AI uses a knowledge-based deductive inference approach to represent board data as logical sentences, enabling it to deduce safe moves and identify mines effectively.

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

### The Purpose of the `Sentence` Class

The `Sentence` class is designed to represent a logical sentence about a set of cells on the Minesweeper board. Each instance of this class encapsulates two main pieces of information:

1. **Set of cells (`self.cells`)**:  
   A set of coordinates (e.g., `(i, j)`) that have not yet been determined to be safe or mines.

2. **Count (`self.count`)**:  
   A number indicating how many of the cells in the set are mines.

This structure allows the AI to build and update a knowledge base about the game. For example, if a safe cell is revealed and indicates that there are 2 mines among its neighboring cells, the AI can create a sentence that includes those neighboring cells with a count of 2. From this information, combined with other sentences, the AI can deduce:

- **Which cells are certainly mines**:  
  If the number of cells in the set equals the count, then all cells in the set must be mines.

- **Which cells are certainly safe**:  
  If the count is zero, then all cells in the set are safe.

Furthermore, the methods `mark_mine` and `mark_safe` allow the sentence to be updated when a cell is discovered to be a mine or is safe, by removing that cell from the set and adjusting the count if necessary.

In summary, the `Sentence` class is crucial for the AI's deductive inference process, as it provides the logical structure that enables the extraction of new information from the knowledge already acquired during the game.

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
- `add_knowledge(cell, count)`:  
  This method is called when the board reveals a safe cell and provides the number of neighboring mines. It marks the cell safe, updates existing knowledge, and creates a new sentence about undetermined neighboring cells. This information is used for further inference to identify safe cells or mines.

- `update_knowledge(self)`:  
  This method refines the AI's understanding by iteratively processing its knowledge base. It:
  1. Iterates over all logical sentences, marking cells as safe or as mines based on the evidence (e.g., if the number count matches the number of cells).
  2. Propagates new information by updating sentences (removing determined cells and adjusting counts).
  3. Derives new sentences when one sentence is a subset of another, thereby enhancing overall inference.
  
  Together, these methods allow the AI to deduce new information continuously, making better decisions during gameplay.

### The add_knowledge Method

The `add_knowledge` method is a core part of the AI's strategy in the Minesweeper game. It is called whenever the game reveals a safe cell along with a number that indicates how many of its neighboring cells contain mines. Here’s a detailed explanation of its steps:

1. **Marking the Move and the Cell as Safe:**  
   The method starts by marking the cell as a move that has been made and adds the cell to the set of known safe cells. This ensures the AI won’t try to explore the same cell again.

2. **Updating Existing Knowledge:**  
   Every sentence (representing a logical statement about a group of cells and the number of mines among them) in the knowledge base is updated to reflect that this cell is safe. This helps keep the knowledge in sync with confirmed safe areas.

3. **Consistent Inference:**  
   When a safe cell is revealed, the count provided tells the AI exactly how many mines exist in the neighboring cells. The method uses this count to:
   - Identify which neighboring cells are not yet determined.
   - Adjust the count if some of those neighbors are already known mines.
   - Build a new sentence comprised only of the still-undecided neighboring cells along with the revised mine count.

4. **Expanding the Knowledge Base:**  
   The newly derived sentence is then added to the knowledge base. This enriched collection of logical statements allows the AI to further deduce the status of other cells:
   - If a sentence indicates that a certain number of cells must be mines, all cells in that sentence can be marked as such.
   - Conversely, if the count is zero, all cells in the sentence are marked as safe.
   - The AI uses these deductions to possibly update or remove redundant sentences from its knowledge base.

5. **Iterative Deduction:**  
   Finally, by continuously adding new knowledge and applying logical inference, the AI refines its strategy, increasing its chances of identifying safe moves and avoiding mines. This iterative process is key to solving the Minesweeper board efficiently.

In summary, `add_knowledge` is essential for converting local observations (a cell’s mine count) into global insights (identifying other safe or mined cells) using logical inference.

### The update_knowledge Method

The `update_knowledge` method is responsible for refining the AI's overall understanding of the board by continuously processing and updating its knowledge base. It works in an iterative loop to draw further inferences from already gathered information. Here's a deeper explanation of its process:

1. **Iterative Update Loop:**  
   The method iterates repeatedly over the current knowledge base until no further cells can be marked as safe or as mines. This ensures that every piece of information is fully exploited.

2. **Marking Cells as Safe or Mines:**  
   For each sentence within the knowledge base, the method checks if the sentence provides enough evidence to conclude definitively that:
   - **All cells in the sentence are mines:** If the number of mines equals the number of cells in that sentence, each cell is marked as a mine.
   - **All cells in the sentence are safe:** If the count is zero, then every cell in the sentence is marked as safe.

3. **Inference of New Knowledge:**  
   When a cell is marked as safe or as a mine, this new information is propagated back into all sentences within the knowledge base. Sentences are updated by:
   - Removing the determined cells from the sets in each sentence.
   - Adjusting the mine counts accordingly, if a cell is marked as a mine.

4. **Adding Derived Sentences:**  
   The method identifies subset relationships among sentences. If one sentence's cell set is completely contained within another's, a new sentence can be inferred from the difference between the two sentences' cell sets with an adjusted mine count. This newly derived sentence is then added to the knowledge base, enriching the AI's overall understanding.

5. **Cleanup of the Knowledge Base:**  
   Redundant or empty sentences are removed to keep the knowledge base concise. This prevents unnecessary repetitions and optimizes the inference process.

In summary, `update_knowledge` is a crucial method that ensures the Minesweeper AI constantly improves its board interpretation. By propagating new information and deriving new logical sentences, the AI can efficiently identify which cells are safe and which contain mines, leading to better decision-making during gameplay.