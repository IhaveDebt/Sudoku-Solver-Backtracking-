#include <iostream>
#include <vector>
using namespace std;

bool isValid(vector<vector<int>>& grid, int row, int col, int num) {
    for (int x = 0; x < 9; x++)
        if (grid[row][x] == num || grid[x][col] == num) return false;
    int startRow = row - row % 3, startCol = col - col % 3;
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (grid[i + startRow][j + startCol] == num) return false;
    return true;
}

bool solve(vector<vector<int>>& grid, int row, int col) {
    if (row == 8 && col == 9) return true;
    if (col == 9) { row++; col = 0; }
    if (grid[row][col] != 0) return solve(grid, row, col + 1);

    for (int num = 1; num <= 9; num++) {
        if (isValid(grid, row, col, num)) {
            grid[row][col] = num;
            if (solve(grid, row, col + 1)) return true;
            grid[row][col] = 0;
        }
    }
    return false;
}

int main() {
    vector<vector<int>> grid = {
        {5,3,0,0,7,0,0,0,0},
        {6,0,0,1,9,5,0,0,0},
        {0,9,8,0,0,0,0,6,0},
        {8,0,0,0,6,0,0,0,3},
        {4,0,0,8,0,3,0,0,1},
        {7,0,0,0,2,0,0,0,6},
        {0,6,0,0,0,0,2,8,0},
        {0,0,0,4,1,9,0,0,5},
        {0,0,0,0,8,0,0,7,9}
    };

    if (solve(grid, 0, 0)) {
        cout << "Solved Sudoku:\n";
        for (auto& row : grid) {
            for (auto& val : row) cout << val << " ";
            cout << endl;
        }
    } else cout << "No solution exists.\n";
}
