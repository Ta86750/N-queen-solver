#include <iostream>
#include <vector>
using namespace std;

// Function to print the solution
void printSolution(const vector<vector<int>>& board, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (board[i][j] == 1)
                cout << "Q ";
            else
                cout << ". ";
        }
        cout << endl;
    }
}

// Function to check if a queen can be placed on board[row][col]
// This function is called when "col" queens are already placed
// in columns from 0 to col -1. So we need to check the rows
// and two diagonals for conflicts
bool isSafe(const vector<vector<int>>& board, int row, int col, int n) {
    // Check this row on left side
    for (int i = 0; i < col; i++) {
        if (board[row][i] == 1) {
            return false;
        }
    }

    // Check upper diagonal on left side
    for (int i = row, j = col; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 1) {
            return false;
        }
    }

    // Check lower diagonal on left side
    for (int i = row, j = col; i < n && j >= 0; i++, j--) {
        if (board[i][j] == 1) {
            return false;
        }
    }

    return true;
}

// Backtracking function to solve the N-Queens problem
bool solveNQueens(vector<vector<int>>& board, int col, int n) {
    // If all queens are placed, return true
    if (col >= n) {
        return true;
    }

    // Consider this column and try placing this queen in all rows one by one
    for (int i = 0; i < n; i++) {
        if (isSafe(board, i, col, n)) {
            // Place this queen in board[i][col]
            board[i][col] = 1;

            // Recur to place rest of the queens
            if (solveNQueens(board, col + 1, n)) {
                return true;
            }

            // If placing queen in board[i][col] doesn't lead to a solution,
            // remove queen from board[i][col] (backtrack)
            board[i][col] = 0;
        }
    }

    // If the queen cannot be placed in any row in this column, return false
    return false;
}

// Main function to solve the N-Queens problem
void nQueens(int n) {
    // Initialize the board with 0s (0 means no queen placed)
    vector<vector<int>> board(n, vector<int>(n, 0));

    // Try to solve the N-Queens problem using backtracking
    if (solveNQueens(board, 0, n)) {
        // Print the solution
        printSolution(board, n);
    } else {
        cout << "Solution does not exist" << endl;
    }
}

// Driver code
int main() {
    int n;
    cout << "Enter the value of N: ";
    cin >> n;

    nQueens(n);
    return 0;
}
