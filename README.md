# Maze Solver

This project is a simple maze solver implemented in Python using the curses library. The maze is represented as a grid, and the algorithm finds the shortest path from the start position 'O' to the end position 'X'.

## Table of Contents
- [Description](#description)
- [Code Overview](#code-overview)

## Description
The maze solver visualizes the process of finding the shortest path in a maze using a breadth-first search algorithm. It uses the curses library to display the maze and the path-finding process in the terminal.

## Installation

### Prerequisites

- Python 3.x

### Installation Steps
1. **Clone the repository:**
  
   git clone https://github.com/yourusername/maze-solver.git
   
   cd maze-solver
   
### Code Overview

1. **Import Libraries**
import curses

from curses import wrapper

import queue

import time

3. **Maze Definition**
The maze is defined as a 2D list, where # represents walls, O represents the start, and X represents the end.

maze = [
    ["#", "O", "#", "#", "#", "#", "#", "#", "#"],
    ["#", " ", " ", " ", " ", " ", " ", " ", "#"],
    ["#", " ", "#", "#", " ", "#", "#", " ", "#"],
    ["#", " ", "#", " ", " ", " ", "#", " ", "#"],
    ["#", " ", "#", " ", "#", " ", "#", " ", "#"],
    ["#", " ", "#", " ", "#", " ", "#", " ", "#"],
    ["#", " ", "#", " ", "#", " ", "#", "#", "#"],
    ["#", " ", " ", " ", " ", " ", " ", " ", "#"],
    ["#", "#", "#", "#", "#", "#", "#", "X", "#"]
]

3. **Print Maze Function**

def print_maze(maze, stdscr, path=[]):

    BLUE = curses.color_pair(1)
    
    RED = curses.color_pair(2)

    for i, row in enumerate(maze):
    
        for j, value in enumerate(row):
        
            if (i, j) in path:
            
                stdscr.addstr(i, j*2, "X", RED)
                
            else:
            
                stdscr.addstr(i, j*2, value, BLUE)
                
4. **Find Start Position Function**
def find_start(maze, start):

    for i, row in enumerate(maze):
   
        for j, value in enumerate(row):
   
            if value == start:
   
                return i, j
   
    return None
   
5. **Find Path Function**

def find_path(maze, stdscr):

    start = "O"
    
    end = "X"
    
    start_pos = find_start(maze, start)

    q = queue.Queue()
    
    q.put((start_pos, [start_pos]))

    visited = set()

    while not q.empty():
    
        current_pos, path = q.get()
        
        row, col = current_pos

        stdscr.clear()
        
        print_maze(maze, stdscr, path)
        
        time.sleep(0.2)
        
        stdscr.refresh()

        if maze[row][col] == end:
        
            return path

        neighbors = find_neighbors(maze, row, col)
        
        for neighbor in neighbors:
        
            if neighbor in visited:
            
                continue

            r, c = neighbor
            
            if maze[r][c] == "#":
            
                continue

            new_path = path + [neighbor]
            
            q.put((neighbor, new_path))
            
            visited.add(neighbor)
            6. **Find Neighbors Function**

def find_neighbors(maze, row, col):

    neighbors = []

    if row > 0:  # UP
    
        neighbors.append((row - 1, col))
        
    if row + 1 < len(maze):  # DOWN
    
        neighbors.append((row + 1, col))
        
    if col > 0:  # LEFT
    
        neighbors.append((row, col - 1))
        
    if col + 1 < len(maze[0]):  # RIGHT
    
        neighbors.append((row, col + 1))

    return neighbors
    
7. **Main Function**

def main(stdscr):

    curses.init_pair(1, curses.COLOR_BLUE, curses.COLOR_BLACK)
    
    curses.init_pair(2, curses.COLOR_RED, curses.COLOR_BLACK)

    find_path(maze, stdscr)
    
    stdscr.getch()
    
8. **Wrapper Call**

wrapper(main)
