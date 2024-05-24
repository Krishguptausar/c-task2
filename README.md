#include <iostream>
#include <vector>

using namespace std;

const int BOARD_SIZE = 3;
const char EMPTY_CELL = ' ';
const char PLAYER_X = 'X';
const char PLAYER_O = 'O';

void initializeBoard(vector<vector<char>>& board) {
    board = vector<vector<char>>(BOARD_SIZE, vector<char>(BOARD_SIZE, EMPTY_CELL));
}

void displayBoard(const vector<vector<char>>& board) {
    for (int i = 0; i < BOARD_SIZE; ++i) {
        for (int j = 0; j < BOARD_SIZE; ++j) {
            cout << board[i][j];
            if (j < BOARD_SIZE - 1) cout << " | ";
        }
        cout << endl;
        if (i < BOARD_SIZE - 1) cout << "---+---+---" << endl;
    }
}

bool isValidMove(const vector<vector<char>>& board, int row, int col) {
    return row >= 0 && row < BOARD_SIZE && col >= 0 && col < BOARD_SIZE && board[row][col] == EMPTY_CELL;
}

bool checkWin(const vector<vector<char>>& board, char player) {
    for (int i = 0; i < BOARD_SIZE; ++i) {
        if (board[i][0] == player && board[i][1] == player && board[i][2] == player) return true;
        if (board[0][i] == player && board[1][i] == player && board[2][i] == player) return true;
    }
    if (board[0][0] == player && board[1][1] == player && board[2][2] == player) return true;
    if (board[0][2] == player && board[1][1] == player && board[2][0] == player) return true;
    return false;
}

bool checkDraw(const vector<vector<char>>& board) {
    for (int i = 0; i < BOARD_SIZE; ++i) {
        for (int j = 0; j < BOARD_SIZE; ++j) {
            if (board[i][j] == EMPTY_CELL) return false;
        }
    }
    return true;
}

void playerMove(vector<vector<char>>& board, char player) {
    int row, col;
    while (true) {
        cout << "Player " << player << ", enter your move (row and column): ";
        cin >> row >> col;
        if (isValidMove(board, row - 1, col - 1)) {
            board[row - 1][col - 1] = player;
            break;
        } else {
            cout << "Invalid move. Try again." << endl;
        }
    }
}

void gameLoop() {
    vector<vector<char>> board;
    char currentPlayer = PLAYER_X;
    bool gameWon = false, gameDraw = false;
    
    initializeBoard(board);
    
    while (!gameWon && !gameDraw) {
        displayBoard(board);
        playerMove(board, currentPlayer);
        gameWon = checkWin(board, currentPlayer);
        gameDraw = checkDraw(board);
        if (!gameWon && !gameDraw) {
            currentPlayer = (currentPlayer == PLAYER_X) ? PLAYER_O : PLAYER_X;
        }
    }
    
    displayBoard(board);
    
    if (gameWon) {
        cout << "Player " << currentPlayer << " wins!" << endl;
    } else {
        cout << "The game is a draw!" << endl;
    }
}

int main() {
    char playAgain;
    do {
        gameLoop();
        cout << "Do you want to play again? (y/n): ";
        cin >> playAgain;
    } while (playAgain == 'y' || playAgain == 'Y');
    
    return 0;
}
