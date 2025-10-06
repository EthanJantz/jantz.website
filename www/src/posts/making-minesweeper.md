+++
layout = "../../layouts/MarkdownPostLayout.astro"
title = 'Making Minesweeper'
description = "A write-up on making the game Minesweeper."
pubDate = 2025-03-16
+++

I've been thinking about the game [Minesweeper](<https://en.wikipedia.org/wiki/Minesweeper_(video_game)>) lately and decided that I would take a stab at writing the game from scratch. This started out as a relatively straightforward project, but I hit some snags and learned some cool things along the way that I thought would be interesting to write about. If you're less interested in reading about the development process and more interested in reading the code, go visit the [project's GitHub repository](https://www.github.com/ethanjantz/py-sweeper), where you can find all of the code. I put some extra effort in to document the code and write some simple tests, so if you're comfortable with python it should be pretty easy to follow! If you're more interested in the story of how this was developed, for whatever reason, continue reading!

## The Rules

Minesweeper is a relatively simple game. There is a grid of cells and a subset of them contain mines. The cells are numbered based on the number of mines in cells adjacent to that cell. For example, let's look at the following board:

```
_|1|2|3|4|
1|1|X|X|1|
2|1|2|2|1|
3| | | | |
```

Cells [1, 2] and [1, 3] contain mines and so the cells surrounding it are numbered accordingly, with each cell identifying the number of mines they are touching.

Let's assume the player started the game by selecting cell [3, 2]. Their board would look like this:

```
_|1|2|3|4|
1|?|?|?|?|
2|1|2|2|1|
3| | | | |
```

From this state, the player can use logic to deduce the location of the mines with the information provided by the revealed cells. Once they revealed the cells at locations [1, 1] and [1, 4] the game would be considered won. If, however, the player made a mistake and revealed a mine they would lose the game.

## The Core Structure of the Program

I set up my Minesweeper game with two core components: The player object and the board object. Most of the actual action happens in the board object, which holds both the full board, the player's view of the board, and the information necessary to determine the game state (whether the game is won or lost). The player object holds the current board, gets input from the player, and checks to make sure the game has been won or lost. The gameplay loop happens in `**main**``.

The core gameplay loop works like this:

1. Initialize the board
2. Initialize the player
3. Get the first cell input from the player and generate the board.
4. Reveal the first selection and show the board. The first cell selected is never a mine.
5. Until the game has been won (all non-mine cells revealed) or lost (a mine is revealed), ask the player to select the next cell to be revealed.

Initializing the objects is fairly straightforward. The program defines the number of mines and the size of the board. One of the early decisions I had to make was how to handle separating the full board, or as I called it the `real_board`, and the player's view of the board. I ended up creating a separate grid that is the same size as the `real_board` but filled with '?' values, which I called `player_board`. This board is revealed as the player interacts with the game and is shown after each selection the player makes. Keeping all of this information within the `Board` object ended up being a good choice, as it made writing the gameplay components straightforward.

### Generating the Board

Board generation went through a few iterations but mostly kept the same core structure. Mines are randomly placed throughout the board and then numbers are placed on the blank cells depending on the number of adjacent mines. Ensuring the first cell doesn't contain a mine, and that it also gives the player enough information to make an informed second move was a mechanic I had to iterate on. I'm pretty happy with how that ended up, the code is legible!

This was the first function I felt necessitated tests. On my first draft of `generate_board` I kept running into situations where the board had way more mines than it should have. I had been working on writing tests for a work project recently, so I decided to do the same for this program. After putting together a collection of tests and running the python debugger tool I eventually found out that I had ordered the logic of mine placement wrong. This turned out to be a common problem in developing this game. Once that was fixed, board generation was checked off and I was able to move on to the more fun part, revealing cells.

### Revealing Cells

Figuring out how to properly reveal cells on the board took the most time and research of any problem I ran into with this project. I plugged away at coding this out the hard way -- without doing any research and just guessing at the best way to approach it. My initial attempts generally ended with one of two bad outcomes:

- The entire board would be revealed on the first guess.
- The board would only ever be revealed one cell at a time.

The way that the logic is supposed to work looks like this:

1. The player selects a cell.
2. The cell is revealed, and if it's a mine the reveal process ends.
3. If the cell is not a mine, check to see if there is a mine in an adjacent cell. If there is one, stop revealing cells.
4. Otherwise, perform step 3 for every adjacent cell recursively until all of the cells that don't have mines that are connected to the selected cell are revealed.

I ran into so many problems writing this function, mostly because I was stubborn and refused to look up the answer. At one point I had a function that used counters, recursed, and was tracking way too many variables to make sense. Eventually I caved and found a [post from a Ruby developer](https://thagomizer.com/blog/2017/03/30/depth-first-search-minesweeper.html) that introduced me to the well-known algorithm Depth First Search. They gave a great explanation of how it works and some nicely formatted pseudocode that pointed me in the right direction. With that post I moved forward and landed on a python implementation that made sense. This is certainly one of the bigger lessons of this project: stay humble and do your research so you don't have to reinvent the wheel.

### Tests and Documentation

Another component of this project that I wanted to spend time on was testing and documentation. Each function has a docstring explaining what the function does, what the inputs are, and what the program was expecting in the output. This helped me think through each component a bit more thoughtfully and keep the whole of the program in mind. Whenever I got stuck on a problem with the code I would jump over to `test_minesweeper.py` and write tests for the function I was having trouble with. This was a great way to take a breather and make sure I understood what was going on in the code. If I learned anything (aside from the importance of research) from this project it was that writing tests is an easy way to improve your understanding of a codebase.

## Conclusion

And that is gist of how I wrote `py-sweeper`. There are a number of things I'd like to expand on from the core program. For one, the actual game of Minesweeper includes a feature that allows the user to place flags on suspected mines. This and other core game components like a GUI are things I'd like to eventually implement; and from there I have some ideas for things that I could do to expand the game into something more unique. One idea I was bouncing around with help from a fellow Minesweeper enjoyer was the idea of turning Minesweeper into a roguelike. I'm imagining turning it into a pseudo-dungeon crawler where the player is on a boat and travels across different instances of Minesweeper puzzles, maybe with some meta-game currency or different types of mines that show up.
