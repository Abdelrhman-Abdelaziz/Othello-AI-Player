# Othello-AI-Player
In this project, we will explore the game playing of Othello using search algorithms and
heuristics. Othello is a two-player strategy game played on a 8x8 board, where each
player has pieces that are either black or white. The goal of the game is to have the
most pieces of your color on the board at the end of the game.

This game is Othello, also known as Reversi. The object
of this game is to make sandwiches of your opponents tiles,
flipping them over to your color (which will be black).
These sandwiches can be any thickness, so long as they
are bookended by one existing black tile and one new
black tile.

Game rules: https://www.youtube.com/watch?v=zFrlu3E18BA
# The GUI
You will need to create a GUI that allows for human vs. human, human vs. computer,
and computer vs. computer gameplay. The GUI must include the following main

# Game Playing Algorithms
1. The minimax algorithm: a basic search algorithm that examines all possible moves from
a given position and selects the move that leads to the best outcome for the current
player
2. Alpha-beta pruning: an improvement on the minimax algorithm that can reduce the
number of nodes that need to be searched.
3. Alpha-beta pruning with iterative deepening (depth is increased iteratively in the search
tree until the timing constraints are violated)

# Heuristics
we Used a combination of all of them.
1. Mobility
2. Coin Parity
3. Corners Captured
4. Stability

## RUNNING THE CODE:
 - Run `main` to play.
 ### FEATURES:
 - Bots: Test as much as you like, but each bot typically beats the one below.
   - The lvl 0 bot is different. It chooses a move randomly and won't skip unless forced to.
   - The lvl 10 bot will be a bit slow, but lvl 8 will almost certainly beat you most of the time
   - depends on combination of heuristics ( Mobility, Coin Parity, Corners Captured, Stability ) 
 - Scorekeepers show current scores
 - Progress bar with animation show current scores and game progress
 - Quit button is always active unless a Bot is thinking.
 - Board: Display the current game board and the current pieces on the board.
 - Move input: Allow human players to input their moves by clicking on the board
 - Game status: Display the current score, whose turn it is, and if the game is over.
 - Game options: Allow players to choose different game modes (e.g., human vs.
human, human vs. computer, computer vs. computer), choose the AI player
## FILES
- `main` contains the main loop and other functions.
- `GUI` contains animation classes. This includes `GUI`, which draws the game pieces, `Button`, which draws and holds visual data about the buttons, and `TextBox`, whcih holds display information about text surfaces to be displayed.
- `Board_Class` is self explanatory. The `Board` object holds the state of the board and has functions which access and mutate information about the game state.
- `AI` has a robot object which can look at a board and make decisions based on what it sees. 
- `Tuples` is a subclass of tuple with improved operations for adding/subtracting coordinates and other manipulations.

### [MAIN](main.py)
This file is where the game takes place. Here, all objects are instantiated and the game is played and rendered.

First, functions are defined which do well-defined things like make a move, change the board size, or set the scores. Next, all of the colors, settings, buttons, and text boxes are generated for future reference. Next, the main loop starts. The main loop is separated into event listening and screen display. 

First, pygame listens for clicking events. There are three possible game states: `game_active`, `settings_open`, and the title screen. Collisions are detected depending on the current game state. At all states, we listen for button presses, but during `game_active`, we check if a board click is valid and if legal, we move there.

Last, the display is updated and animations are moved forward by one step. In each of the game states, different objects are rendered and different animations take place.
### [GUI](GUI)
This file has three classes which are used for the creating all of the display effects in the game. 
- The `Tile` class holds information about the color, location, shape, and animation state of each tile during gameplay
  - The `Board` holds all information about the actual game, but the tiles are the only thing the player can actually see. 
 There are three animations which morph the dimensions of the tile: Placing, Flipping, and Removal.
- The `Button` class helps to concisely describe and display a clickable button on the screen.
  - The button object hold the dimensions, location, color, text, font, text color, text location, border width of a button on the screen. This would
       otherwise require a bunch of different variables, rectangle objects, and assignments: creating surfaces, rectangles, and several lines of code to render.
- The `TextBox` class is similar to a button but without a rectangle or any clickable functionality.
  - Text boxes to a lot less, but they hold the text rendering surface and the location rectangle in one
       more convenient place, rather than having separate `text_surf` and `text_rect` objects. Also, the `changeText` method is particularly helpful, because this would
       otherwise require constructing brand new objects from scratch
### [BOARD_CLASS](Board.py)
 This file contains only the `Board` class and a function which constructs a Board. A board object holds the game state (or hypothetical game states) at any time. It is represented as a list of lists:
 - The Board itself is constructed as a list of rows.
 - Each row is a list of ints indicated a friendly(1), enemy(-1), or no(0) tile
The logic and rules of the game occur via the functions of this class. These functions only work from the perspective
of whoever's turn it currently is. Thus, a value (1) must **always** mean friendly. For example, the `legalMoves` function tells you all of the moves for whomever is currently playing, and by inverting the board, we can can get all of the moves for the other person.

This board can be copied, reversed, and mutated according to game rules. As a result, board copies are stored in memory for the purpose of undoing moves, hypothetical boards are used by robots to assess possible moves and make predictions.
### [AI](AI.py)
This file contains only the `Bot` class. Robot objects hold information about their decision making parameters/priorities. The class
functions dictate the decision making process for choosing a move on a given `Board`, according to these parameters. The default strategy is to assign a utility to a given board state based which spaces I control versus which spaces the opponent controls. By assuming the opponent also assesses the board this way, we can predict their best move and give the opponent the worst set of options. Parameters include:
- Whether to prefer corner and edge spaces, and what score multiplier to assign to those spaces
- Whether the bot will skip repeatedly before eventually giving up and making a move (Patience)
- A special parameter to _turn off_ the bot's brain and choose a legal move at random
- How deeply to think, i.e. how far ahead to look in the game when considering the opponent's movement opportunities (`depth=0` is just counting tiles on the board)
The tought process of the robot includes:
- The `Evaluation` function evaluates the MinMAx of a given board state based on the mobility, coin parity, corners captured (as stability), and stability itself. The function assigns weights to these heuristics and combines them to calculate an overall utility value. This utility value is used to make decisions about the best move to choose on a given board. The function also handles the division by zero error to ensure stability is correctly calculated.
- Finally, by knowing long term utilities of the board after each possible move, choose the move with the best long term utility, given a board setup
#### [Tuples](Tuples.py)
This only contains the `Vector` class. It allows for slightly nicer manipulations of coordinates, with the functionality of adding and scaling points. It is not all necessary for this project.
#### SETTINGS
This file holds a tuple of the in-game settings from the **Settings** page. This way, you can close and reopen the game without the settings resetting every time.

