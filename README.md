import random

def print_board(board):
    for i in range(0, 9, 3):
        print(" | ".join(board[i:i+3]))
        if i < 6:
            print("---------")

def empty_cells(board):
    return [i for i, cell in enumerate(board) if cell == " "]

def is_winner(board, player):
    winning_combinations = [
        [0, 1, 2], [3, 4, 5], [6, 7, 8],  # Rows
        [0, 3, 6], [1, 4, 7], [2, 5, 8],  # Columns
        [0, 4, 8], [2, 4, 6]  # Diagonals
    ]
    return any(all(board[i] == player for i in combo) for combo in winning_combinations)

def is_game_over(board):
    return " " not in board or is_winner(board, "X") or is_winner(board, "O")

def evaluate(board):
    if is_winner(board, "X"):
        return 1
    elif is_winner(board, "O"):
        return -1
    else:
        return 0

def minimax(board, depth, alpha, beta, maximizing_player):
    if is_game_over(board):
        return evaluate(board)

    if maximizing_player:
        max_eval = float("-inf")
        for move in empty_cells(board):
            board[move] = "X"
            eval = minimax(board, depth + 1, alpha, beta, False)
            board[move] = " "
            max_eval = max(max_eval, eval)
            alpha = max(alpha, eval)
            if beta <= alpha:
                break
        return max_eval
    else:
        min_eval = float("inf")
        for move in empty_cells(board):
            board[move] = "O"
            eval = minimax(board, depth + 1, alpha, beta, True)
            board[move] = " "
            min_eval = min(min_eval, eval)
            beta = min(beta, eval)
            if beta <= alpha:
                break
        return min_eval

def best_move(board):
    best_score = float("-inf")
    best_move = None
    for move in empty_cells(board):
        board[move] = "X"
        score = minimax(board, 0, float("-inf"), float("inf"), False)
        board[move] = " "
        if score > best_score:
            best_score = score
            best_move = move
    return best_move

def play_game():
    board = [" " for _ in range(9)]
    print("Welcome to Tic-Tac-Toe!")
    print_board(board)

    while not is_game_over(board):
        # Player's turn
        while True:
            try:
                player_move = int(input("Enter your move (0-8): "))
                if 0 <= player_move <= 8 and board[player_move] == " ":
                    board[player_move] = "O"
                    break
                else:
                    print("Invalid move. Try again.")
            except ValueError:
                print("Invalid input. Please enter a number between 0 and 8.")

        print_board(board)

        if is_game_over(board):
            break

        # AI's turn
        print("AI is making a move...")
        ai_move = best_move(board)
        board[ai_move] = "X"
        print_board(board)

    # Game over
    if is_winner(board, "X"):
        print("AI wins!")
    elif is_winner(board, "O"):
        print("You win! (This shouldn't happen with perfect play)")
    else:
        print("It's a tie!")

if __name__ == "__main__":
    play_game()

	
