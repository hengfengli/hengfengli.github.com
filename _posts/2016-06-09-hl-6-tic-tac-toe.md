---
layout: post
title: "[HL-6] Tic-Tac Toe Problem"
categories: 
    - algorithm
tags: 
    - Tic-tac-toe
published: true
---
<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [What is the Tic-tac-toe problem?](#what-is-the-tic-tac-toe-problem)
- [Define the game framework](#define-the-game-framework)
- [A O(n^2) solution](#a-on2-solution)
- [An improved O(n) solution](#an-improved-on-solution)
- [An ultimately improved O(1) solution](#an-ultimately-improved-o1-solution)

<!-- /TOC -->

# What is the Tic-tac-toe problem? 

**Leetcode 348.** Design a Tic-tac-toe game that is played between two 
players on a n x n grid. 

You may assume the following rules:

* A move is guaranteed to be valid and is placed on an empty block.
* Once a winning condition is reached, no more moves is allowed.
* A player who succeeds in placing n of their marks in a horizontal, 
vertical, or diagonal row wins the game.

Example:

~~~ java
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the 
board. 

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
~~~

Follow up:

* Could you do better than O(n^2) per move() operation?

Hint:

* Could you trade extra space such that move() operation can be done 
in O(1)?
* You need two arrays: int rows[n], int cols[n], plus two variables: 
diagonal, anti_diagonal. 

# Define the game framework

First, we describe the game's APIs as follows: 

~~~python
class TicTacToe
-----------------------------------------------------------------
      TicTacToe(int n)          create a game given a n x n board
int   move(x, y, id)            a player id makes a move at (x,y)
-----------------------------------------------------------------
~~~

Next, we write the game framework in Python: 

~~~python
class TicTacToe:
    def __init__(self, n):
        self.board = [[None for j in range(n)] for i in range(n)]
    
    def move(self, x, y, id):
        pass
~~~

Then, write the test cases: 

~~~python
toe = TicTacToe(3)
assert toe.move(0, 0, 1) == 0
assert toe.board[0][0] == 1

assert toe.move(0, 2, 2) == 0
assert toe.board[0][2] == 2

assert toe.move(2, 2, 1) == 0
assert toe.board[2][2] == 1

assert toe.move(1, 1, 2) == 0
assert toe.board[1][1] == 2

assert toe.move(2, 0, 1) == 0
assert toe.board[2][0] == 1

assert toe.move(1, 0, 2) == 0
assert toe.board[1][0] == 2

assert toe.move(2, 1, 1) == 1
assert toe.board[2][1] == 1
~~~


# A O(n^2) solution

The simplest solution is to count the number of cells for each player
on every row and column, plus diagonal and anti-diagonal. If it is
empty, we do nothing. If the cell is occupied by the first player,
we increase 1.  Otherwise, for the second player, we decrease 1.
Once we find a count that equals to n, there is a winner. 

~~~python
class TicTacToe:
    def __init__(self, n):
        self.n = n
        self.board = [[None for j in range(n)] for i in range(n)]
    
    def val(self, row, col):
        v = self.board[row][col]
        if not v:
            return 0
        elif v == 1:
            return 1
        else:
            return -1
    
    def win(self):
        """
        Check every row and column, diagonal, anti-diagonal
        """
        rows = [0 for i in range(self.n)]
        cols = [0 for i in range(self.n)]
        for row in xrange(self.n):
            for col in xrange(self.n):
                
                rows[row] += self.val(row, col)
                cols[col] += self.val(row, col)
                
                if abs(rows[row]) == self.n or abs(cols[col]) == self.n:
                    # print "row and col check!"
                    return True
        
        diagonal = 0
        anti_diagonal = 0
        for row, col in zip(range(self.n), range(self.n)):
            diagonal += self.val(row, col)
            anti_diagonal += self.val(row, self.n-col-1)
        
        # print "diagonal and anti-diagonal!", diagonal, anti_diagonal
        return abs(diagonal) == self.n or abs(anti_diagonal) == self.n
        
    def move(self, x, y, id):
        if self.board[x][y] != None:
            raise Exception("The cell is not empty!")
        
        self.board[x][y] = id
        
        # print self.board
        # print "win?", self.win()
        
        return 1 if self.win() else 0
~~~


# An improved O(n) solution

We can create sentinel for each row, column, diagonal, and anti_diagonal. 
When we add a new move, we update these sentinels. For any new move, 
we just need to check the max value of these sentinels. If we scan
all sentinels, the time complexity is O(n). 

~~~python
class TicTacToe:
    def __init__(self, n):
        self.n = n
        self.board = [[None for j in range(n)] for i in range(n)]
        self.rows = [0 for i in range(self.n)]
        self.cols = [0 for i in range(self.n)]
        self.diagonal = 0
        self.anti_diagonal = 0
    
    def move(self, x, y, id):
        if self.board[x][y] != None:
            raise Exception("The cell is not empty!")
        
        self.board[x][y] = id
        
        inc = 1 if id == 1 else -1
        
        self.rows[x] += inc
        self.cols[y] += inc
        if x == y: 
            self.diagonal += inc
        if x == self.n-1-y:
            self.anti_diagonal += inc
        
        max_row = max(map(abs, self.rows))
        max_col = max(map(abs, self.cols))
        
        max_count = max([max_row, max_col, abs(self.diagonal), abs(self.anti_diagonal)])
        
        return 1 if max_count == self.n else 0
~~~

# An ultimately improved O(1) solution

Why do we need to scan the sentinels? We only need to check updated
sentinels when adding a new move. This reduces the time to O(1). 

~~~python
def move(self, x, y, id):
    ...
    vals = [self.rows[x], self.cols[y], self.diagonal, self.anti_diagonal]
    vals = map(abs, vals)
    max_count = max(vals)

    return 1 if max_count == self.n else 0
~~~

The whole procedure is as follows: 

![](/img/hl-6-tic-tac-toe.png)