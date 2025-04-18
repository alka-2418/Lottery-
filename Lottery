import tkinter as tk
from tkinter import messagebox
import random

# Constants
TOTAL_NUMBERS = 49
NUMBERS_TO_PICK = 6
TIME_LIMIT = 60  # Time limit in seconds

# Global variables
selected_numbers = []
registered_user = None
time_remaining = TIME_LIMIT
timer_running = False


# Function to register user
def register_user():
    global registered_user
    username = username_entry.get().strip()
    if not username:
        messagebox.showwarning("Warning", "Please enter your name to register.")
        return
    registered_user = username
    messagebox.showinfo("Welcome", f"Welcome, {registered_user}! Let's play the lottery.")
    registration_frame.pack_forget()
    game_frame.pack()
    start_timer()


# Function to select/deselect a number
def select_number(number, button):
    global selected_numbers
    if number in selected_numbers:
        selected_numbers.remove(number)
        button.config(bg="white", fg="#333")
    elif len(selected_numbers) < NUMBERS_TO_PICK:
        selected_numbers.append(number)
        button.config(bg="#00c6ff", fg="white")
    else:
        messagebox.showwarning("Warning", f"You can only select {NUMBERS_TO_PICK} numbers!")


# Generate random winning numbers
def generate_winning_numbers():
    return random.sample(range(1, TOTAL_NUMBERS + 1), NUMBERS_TO_PICK)


# Play the game
def play_game():
    global timer_running
    timer_running = False  # Stop the timer

    if len(selected_numbers) != NUMBERS_TO_PICK:
        messagebox.showwarning("Warning", f"Please select exactly {NUMBERS_TO_PICK} numbers.")
        return

    winning_numbers = generate_winning_numbers()
    matches = set(selected_numbers) & set(winning_numbers)

    result_message = (
        f"Winning Numbers: {', '.join(map(str, winning_numbers))}\n"
        f"Your Numbers: {', '.join(map(str, selected_numbers))}\n"
        f"Matches: {len(matches)}\n"
        f"{'🎉 Congratulations, you won!' if len(matches) == NUMBERS_TO_PICK else 'Better luck next time!'}"
    )

    messagebox.showinfo("Result", result_message)
    reset_game()


# Reset the game
def reset_game():
    global selected_numbers, time_remaining, timer_running
    selected_numbers = []
    time_remaining = TIME_LIMIT
    timer_running = True
    timer_label.config(text=f"Time Remaining: {time_remaining}s")

    for widget in number_frame.winfo_children():
        widget.config(bg="white", fg="#333")


# Timer function
def start_timer():
    global time_remaining, timer_running
    timer_running = True

    def countdown():
        global time_remaining, timer_running
        if time_remaining > 0 and timer_running:
            time_remaining -= 1
            timer_label.config(text=f"Time Remaining: {time_remaining}s")
            root.after(1000, countdown)
        elif time_remaining == 0:
            timer_running = False
            messagebox.showinfo("Time's Up", "Time's up! You didn't select all your numbers.")
            reset_game()

    countdown()


# Initialize main application window
root = tk.Tk()
root.title("Lottery Game")
root.geometry("500x750")
root.configure(bg="#1d3557")

# Registration frame
registration_frame = tk.Frame(root, bg="#1d3557")
registration_frame.pack()

welcome_label = tk.Label(
    registration_frame,
    text="Welcome to the Lottery Game!",
    font=("Arial", 24, "bold"),
    bg="#1d3557",
    fg="white",
)
welcome_label.pack(pady=20)

username_label = tk.Label(
    registration_frame,
    text="Enter your name to register:",
    font=("Arial", 14),
    bg="#1d3557",
    fg="white",
)
username_label.pack(pady=10)

username_entry = tk.Entry(registration_frame, font=("Arial", 14), width=30)
username_entry.pack(pady=10)

register_button = tk.Button(
    registration_frame,
    text="Register",
    font=("Arial", 14),
    bg="#00c6ff",
    fg="white",
    command=register_user,
)
register_button.pack(pady=20)

# Game frame
game_frame = tk.Frame(root, bg="#1d3557")
title = tk.Label(
    game_frame, text="Lottery Game", font=("Arial", 24, "bold"), bg="#1d3557", fg="white"
)
title.pack(pady=20)

instruction = tk.Label(
    game_frame, text="Select 6 numbers and try your luck!", font=("Arial", 14), bg="#1d3557", fg="white"
)
instruction.pack()

# Timer label
timer_label = tk.Label(
    game_frame,
    text=f"Time Remaining: {TIME_LIMIT}s",
    font=("Arial", 14),
    bg="#1d3557",
    fg="white",
)
timer_label.pack(pady=10)

number_frame = tk.Frame(game_frame, bg="#1d3557")
number_frame.pack(pady=20)

# Create buttons for numbers
for i in range(1, TOTAL_NUMBERS + 1):
    btn = tk.Button(
        number_frame,
        text=i,
        width=4,
        height=2,
        bg="white",
        fg="#333",
        font=("Arial", 10),
        command=lambda num=i, b=None: select_number(num, b or btn),
    )
    btn.grid(row=(i - 1) // 7, column=(i - 1) % 7, padx=5, pady=5)

play_button = tk.Button(
    game_frame, text="Play", font=("Arial", 16), bg="#00c6ff", fg="white", command=play_game
)
play_button.pack(pady=20)

# Start the application
root.mainloop()
