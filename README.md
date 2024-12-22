# smart-ai-tic-tac-toe
# Its a tic tac toe game on python vs an ai that chooses its moves based on the best next move

def minimax(board, is_maximizing):
    if check_win(board, "O"):
        return 1
    elif check_win(board, "X"):
        return -1
    elif all(cell != " " for row in board for cell in row):
        return 0
    
    if is_maximizing:
        best_score = -float("inf")
        for i in range(3):
            for j in range(3):
                if board[i][j] == " ":
                    board[i][j] = "O"
                    score = minimax(board, False)
                    board[i][j] = " "
                    best_score = max(score, best_score)
        return best_score
    else:
        best_score = float("inf")
        for i in range(3):
            for j in range(3):
                if board[i][j] == " ":
                    board[i][j] = "X"
                    score = minimax(board, True)
                    board[i][j] = " "
                    best_score = min(score, best_score)
        return best_score

def smart_ai_move(board):
    best_score = -float("inf")
    best_move = None
    for i in range(3):
        for j in range(3):
            if board[i][j] == " ":
                board[i][j] = "O"
                score = minimax(board, False)
                board[i][j] = " "
                if score > best_score:
                    best_score = score
                    best_move = (i, j)
    return best_move
import random

# Display the board
def display_board(board):
    print("\n")
    print(" " + board[0][0] + " | " + board[0][1] + " | " + board[0][2])
    print("---+---+---")
    print(" " + board[1][0] + " | " + board[1][1] + " | " + board[1][2])
    print("---+---+---")
    print(" " + board[2][0] + " | " + board[2][1] + " | " + board[2][2])
    print("\n")

# Check for a win
def check_win(board, player):
    for i in range(3):
        if board[i][0] == board[i][1] == board[i][2] == player:
            return True
        if board[0][i] == board[1][i] == board[2][i] == player:
            return True
    if board[0][0] == board[1][1] == board[2][2] == player:
        return True
    if board[0][2] == board[1][1] == board[2][0] == player:
        return True
    return False

# Random AI move
def random_ai_move(board):
    available_positions = [(i, j) for i in range(3) for j in range(3) if board[i][j] == " "]
    return random.choice(available_positions)

# Play the game
def play_game():
    board = [[" " for _ in range(3)] for _ in range(3)]
    players = ["X", "O"]
    human = players[0]
    ai = players[1]
    current_player = human
    
    display_board(board)

    while True:
        if current_player == human:
            print(f"{human}'s turn.")
            try:
                row = int(input("Enter row (0-2): "))
                col = int(input("Enter column (0-2): "))
            except ValueError:
                print("Invalid input! Please enter numbers only (0, 1, or 2).")
                continue
            
            if row not in [0, 1, 2] or col not in [0, 1, 2]:
                print("Invalid position! Enter values between 0 and 2.")
                continue

            if board[row][col] != " ":
                print("Position already taken. Try again.")
                continue
        else:
            print("AI's turn.")
            row, col = smart_ai_move(board)

        board[row][col] = current_player
        display_board(board)

        if check_win(board, current_player):
            print(current_player + " WINS!")
            break

        if " " not in [pos for row in board for pos in row]:
            print("It's a tie!")
            break

        current_player = human if current_player == ai else ai


if __name__ == "__main__":
    play_game()

