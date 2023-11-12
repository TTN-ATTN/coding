# PROBLEM: SUDOKU
## Approach: [Backtracking algorithm](https://www.geeksforgeeks.org/backtracking-algorithms/)
### Description:
Given the 9x9 sudoku grid with numbers and 'X', which stands for empty cells

Input:                

Output:


Sudoku is a great exercise for practicing algorithm. To solve this, we need to understand the rules of 9x9 sudoku:
1. Every number can only be used once in the same column and row.
2. 9x9 consists of 9 3x3 grids, which contain exactly 9 numbers (from 1 to 9) and no duplicate.

Let's do some set up first before solving the problem:
```cpp
#include <bits/stdc++.h>
using namespace std;
#define ll long long

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
                s[i][j] = num;
                cRow[i][num] = cCol[j][num] = cSq[i/3][j/3][num] = true;
            }
        }
    }
}
```

After this, we need the algorithm for Solve() function. Let's break down the problem:
- Placing the numbers into cells
- Checking the conditions

