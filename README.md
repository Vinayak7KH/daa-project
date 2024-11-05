import tkinter as tk
from tkinter import messagebox

# Function to solve 0/1 Knapsack problem
def knapsack(values, weights, capacity):
    n = len(values)
    # Initialize DP table
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]

    # Build the table in bottom-up manner
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(values[i - 1] + dp[i - 1][w - weights[i - 1]], dp[i - 1][w])
            else:
                dp[i][w] = dp[i - 1][w]

    # Find items included in the optimal solution
    result = []
    w = capacity
    for i in range(n, 0, -1):
        if dp[i][w] != dp[i - 1][w]:
            result.append(i - 1)  # item index
            w -= weights[i - 1]

    # Return maximum value and the items selected
    return dp[n][capacity], result

# Function to handle input and run knapsack algorithm
def solve_knapsack():
    try:
        # Parse input values
        values = list(map(int, value_entry.get().split()))
        weights = list(map(int, weight_entry.get().split()))
        capacity = int(capacity_entry.get())

        if len(values) != len(weights):
            messagebox.showerror("Input Error", "The number of values and weights must be the same.")
            return

        # Solve knapsack problem
        max_value, items_selected = knapsack(values, weights, capacity)

        # Display results
        result_label.config(text=f"Maximum Value: {max_value}")
        items_label.config(text=f"Items selected (0-based index): {items_selected}")

    except ValueError:
        messagebox.showerror("Input Error", "Please enter valid integers.")

# Set up the Tkinter window
root = tk.Tk()
root.title("Knapsack Problem Solver")

# Instructions
instruction_label = tk.Label(root, text="Enter weights and values as space-separated integers:")
instruction_label.pack()

# Entry for weights
weight_label = tk.Label(root, text="Weights:")
weight_label.pack()
weight_entry = tk.Entry(root)
weight_entry.pack()

# Entry for values
value_label = tk.Label(root, text="Values:")
value_label.pack()
value_entry = tk.Entry(root)
value_entry.pack()

# Entry for capacity
capacity_label = tk.Label(root, text="Knapsack Capacity:")
capacity_label.pack()
capacity_entry = tk.Entry(root)
capacity_entry.pack()

# Button to solve knapsack
solve_button = tk.Button(root, text="Solve Knapsack", command=solve_knapsack)
solve_button.pack()

# Result labels
result_label = tk.Label(root, text="")
result_label.pack()
items_label = tk.Label(root, text="")
items_label.pack()

# Start Tkinter main loop
root.mainloop()
