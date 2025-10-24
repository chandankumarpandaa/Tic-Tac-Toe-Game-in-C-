#include <iostream>
#include <cstdlib>   // For system("cls")

using namespace std;

// Function declarations
void printInputMatrix();
void printBoard();
int addMark();
int check();

// Global variables
char board[3][3] = {
    {' ', ' ', ' '},
    {' ', ' ', ' '},
    {' ', ' ', ' '}
};

int turn = 1;   // 1 = Player 1 | 0 = Player 2
char mark = 'O'; // 'O' for Player 1 | 'X' for Player 2
int input;

// --------------------- MAIN FUNCTION ---------------------
int main() {

    int won = 0;
    int validInput = 0;

    // Game loop for max 9 moves
    for (int i = 0; i < 9; i++) {
        system("cls");  // Clear the screen each turn

        printBoard();

        if (turn)
            cout << "Player 1 Turn (Symbol: O)" << endl;
        else
            cout << "Player 2 Turn (Symbol: X)" << endl;

        printInputMatrix();

        cout << "Enter Input from Input Matrix (1-9): ";
        cin >> input;

        // Validate user input
        while (input < 1 || input > 9) {
            cout << "Invalid Input. Please Re-Enter (1-9): ";
            cin >> input;
        }

        // Assign correct mark based on player
        mark = (turn) ? 'O' : 'X';

        // Try to place the mark
        validInput = addMark();
        if (!validInput) {
            i--;  // Repeat same turn if invalid input (cell occupied)
            continue;
        }

        // Check if anyone won
        won = check();
        if (won) {
            system("cls");
            printBoard();
            if (turn)
                cout << endl << "*** Player 1 (O) - You Won! 🎉 ***" << endl;
            else
                cout << endl << "*** Player 2 (X) - You Won! 🎉 ***" << endl;
            return 0;
        }

        // If all cells are filled and no winner
        if (i == 8) {
            system("cls");
            printBoard();
            cout << endl << "*** MATCH DRAW 🤝 ***" << endl;
        }

        // Switch turns
        turn = !turn;
    }

    return 0;
}

// --------------------- FUNCTION DEFINITIONS ---------------------

// Show number layout for reference
void printInputMatrix() {
    cout << endl << endl << "INPUT MATRIX (Choose 1–9)" << endl;
    cout << " 1 | 2 | 3 " << endl;
    cout << "-----------" << endl;
    cout << " 4 | 5 | 6 " << endl;
    cout << "-----------" << endl;
    cout << " 7 | 8 | 9 " << endl << endl;
}

// Print the game board
void printBoard() {
    cout << " " << board[0][0] << " | " << board[0][1] << " | " << board[0][2] << " " << endl;
    cout << "-----------" << endl;
    cout << " " << board[1][0] << " | " << board[1][1] << " | " << board[1][2] << " " << endl;
    cout << "-----------" << endl;
    cout << " " << board[2][0] << " | " << board[2][1] << " | " << board[2][2] << " " << endl;
}

// Place the mark on the board
int addMark() {
    for (int i = 0, k = 1; i < 3; i++) {
        for (int j = 0; j < 3; j++, k++) {
            if (k == input) {
                if (board[i][j] == ' ') {
                    board[i][j] = mark;
                    return 1;
                } else {
                    cout << "Cell already taken. Press any key to continue...";
                    return 0;
                }
            }
        }
    }
    return 0;
}

// Check win conditions
int check() {
    // Rows
    for (int i = 0; i < 3; i++)
        if (board[i][0] == mark && board[i][1] == mark && board[i][2] == mark)
            return 1;

    // Columns
    for (int i = 0; i < 3; i++)
        if (board[0][i] == mark && board[1][i] == mark && board[2][i] == mark)
            return 1;

    // Diagonals
    if (board[0][0] == mark && board[1][1] == mark && board[2][2] == mark)
        return 1;
    if (board[0][2] == mark && board[1][1] == mark && board[2][0] == mark)
        return 1;

    return 0;
}
