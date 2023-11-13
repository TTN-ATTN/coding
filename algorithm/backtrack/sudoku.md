# PROBLEM: SUDOKU
## Approach: [Backtracking algorithm](https://www.geeksforgeeks.org/backtracking-algorithms/)
## Required knowledge: [Recursion](https://www.geeksforgeeks.org/introduction-to-recursion-data-structure-and-algorithm-tutorials/), [Brute Force](https://www.geeksforgeeks.org/brute-force-approach-and-its-pros-and-cons/)
### Description:
Given the 9x9 sudoku grid with numbers and 'X', which stands for empty cells

Sudoku is a great exercise for practicing backtracking algorithm. To solve this, we need to understand the rules of 9x9 sudoku:
1. Every number can only be used once in the same column and row.
2. 9x9 consists of 9 3x3 grids, which contain exactly 9 numbers (from 1 to 9) and no duplicate.

Let's do some set up first before solving the problem:
```cpp
#include <bits/stdc++.h>
using namespace std;

int main()
{
  Input();
  Solve();
  return 0;
}
```
> This is the simple set up I often use.

Firstly, we need to store the position of numbers in the 9x9 grid, the appearance of each number in columns, rows and 3x3 grid. My approach is using 3 2D arrays and 1 3D array (all global):

```cpp
int grid[9][9] = {0}; // 0 stands for empty cells
bool cCol[9][10]={false}; // check appearance in columns
bool cRow[9][10]={false}; // check appearance in rows
bool cSq[3][3][10]={false}; // check appearance in 3x3 grids
```

Now comes to the Input() part, we need to allocate the numbers into the grid and mark their appearance, with 'X' being the default set up. My approach:
```cpp
void Input()
{   
    char c;
    for(int i = 0; i < 9; i++)
    {
        for(int j = 0; j < 9; j++)
        {
            cin >> c;
            if(c!='X')
            {   
                int num = c - '0';
                grid[i][j] = num;
                cRow[i][num] = cCol[j][num] = cSq[i/3][j/3][num] = true;
            }
        }
    }
}
```

After this, we need the algorithm for Solve() function. Let's break down the problem:
- Placing the numbers into cells
- Checking if the cells are valid

The check conditions are quite straightforward, so we will work with them first.
```cpp
bool Valid(int row, int col , int num)
{
  return !(cSq[row/3][col/3][num] || cRow[row][num] || cCol[col][num]);
}
```
Another way:

```cpp
bool Valid(int row, int col , int num)
{
  return (!cRow[row][num] && !cCol[col][num] && !cSq[row/3][col/3][num]);
}
```
Now comes to the hard part, the backtracking algorithm in Solve() function. Here is my idea of solving the sudoku:
> Tries to fill the cells of the Sudoku grid recursively. If a cell already contains a number, it moves to the next cell. If a cell is empty, it tries to fill it with a number from 1 to 9 that does not violate the Sudoku rules. If no number can be placed in the current cell, it backtracks to the previous cell and tries the next number. If the loop reach the end of the grid, print the solved grid.

Based on that idea, my function is:
```cpp

bool Try(int row, int col)
{
    if(row > 8) return true;
    if(col > 8) return Try(row + 1, 0);
    if(grid[row][col] != 0) return Try(row, col + 1);
    for(int i = 1; i <= 9; i++)
    {
        if(Valid(row, col, i))
        {
            grid[row][col] = i;
            cSq[row/3][col/3][i] = cRow[row][i] = cCol[col][i] = true;
            if(Try(row, col + 1))
                return true;
            grid[row][col] = 0;
            cSq[row/3][col/3][i] = cRow[row][i] = cCol[col][i] = false;
        }
    }
    return false;
}

void Solve(){
  if(Try(0,0)){
    for(int i = 0; i < 9; i++){
      for(int j = 0; j < 9; j++) cout << grid[i][j];
      cout << '\n';
    }
  }
}
```

Put everything together, we have the whole program:
```cpp
#include <bits/stdc++.h>
using namespace std;

int grid[9][9] = {0}; // 0 stands for empty cells
bool cCol[9][10]={false}; // check appearance in columns
bool cRow[9][10]={false}; // check appearance in rows
bool cSq[3][3][10]={false}; // check appearance in 3x3 grids

void Input()
{   
    char c;
    for(int i = 0; i < 9; i++)
    {
        for(int j = 0; j < 9; j++)
        {
            cin >> c;
            if(c!='X')
            {   
                int num = c - '0';
                grid[i][j] = num;
                cRow[i][num] = cCol[j][num] = cSq[i/3][j/3][num] = true;
            }
        }
    }
}

bool Valid(int row, int col , int num)
{
  return !(cSq[row/3][col/3][num] || cRow[row][num] || cCol[col][num]);
}

bool Try(int row, int col)
{
    if(row > 8) return true;
    if(col > 8) return Try(row + 1, 0);
    if(grid[row][col] != 0) return Try(row, col + 1);
    for(int i = 1; i <= 9; i++)
    {
        if(Valid(row, col, i))
        {
            grid[row][col] = i;
            cSq[row/3][col/3][i] = cRow[row][i] = cCol[col][i] = true;
            if(Try(row, col + 1))
                return true;
            grid[row][col] = 0;
            cSq[row/3][col/3][i] = cRow[row][i] = cCol[col][i] = false;
        }
    }
    return false;
}

void Solve(){
  if(Try(0,0)){
    for(int i = 0; i < 9; i++){
      for(int j = 0; j < 9; j++) cout << grid[i][j];
      cout << '\n';
    }
  }
}

int main()
{
  Input();
  Solve();
  return 0;
}

```
I hope this will be helpful, happy coding!
