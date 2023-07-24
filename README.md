# Calculator
My personalized calculator

import tkinter as tk
import math
from tkinter import font

def calculate():
    expression = entry.get()
    try:
        result = eval(expression)
        entry.delete(0, tk.END)
        entry.insert(tk.END, result)
        result_label.config(text=f"Result: {result}")
        history.append(expression + " = " + str(result))
    except Exception as e:
        result_label.config(text="Error")

def clear():
    entry.delete(0, tk.END)

def all_clear():
    entry.delete(0, tk.END)
    result_label.config(text="Result: ")
    history.clear()

def undo():
    entry_text = entry.get()
    entry.delete(len(entry_text) - 1)

def add_to_expression(value):
    entry.insert(tk.END, value)

def evaluate_expression():
    expression = entry.get()
    try:
        result = eval(expression)
        entry.delete(0, tk.END)
        entry.insert(tk.END, result)
        result_label.config(text=f"Result: {result}")
        history.append(expression + " = " + str(result))
    except Exception as e:
        result_label.config(text="Error")

def sin():
    value = float(entry.get())
    result = math.sin(math.radians(value))
    entry.delete(0, tk.END)
    entry.insert(tk.END, result)

def cos():
    value = float(entry.get())
    result = math.cos(math.radians(value))
    entry.delete(0, tk.END)
    entry.insert(tk.END, result)

def tan():
    value = float(entry.get())
    result = math.tan(math.radians(value))
    entry.delete(0, tk.END)
    entry.insert(tk.END, result)

def show_history():
    history_window = tk.Toplevel(window)
    history_window.title("History")
    history_window.geometry("400x300")
    history_window.configure(bg="black")

    history_label = tk.Label(history_window, text="Past History:", font=button_font, bg="black", fg="white")
    history_label.pack(pady=10)

    history_text = tk.Text(history_window, font=button_font, bg="black", fg="white")
    history_text.pack(fill=tk.BOTH, expand=True)

    clear_history_button = tk.Button(history_window, text="Clear History", font=button_font, relief=tk.GROOVE, bd=1, fg="white", bg="#999999", command=lambda: clear_history(history_text))
    clear_history_button.pack(pady=5)

    for i, item in enumerate(history, start=1):
        history_text.insert(tk.END, f"{i}. {item}\n")

def clear_history(history_text):
    history_text.delete(1.0, tk.END)
    history.clear()

# Create the main application window
window = tk.Tk()
window.title("Calculator")
window.geometry("350x450")
window.configure(bg="black")

# Create and set custom fonts
header_font = font.Font(family="Helvetica", size=18, weight="bold")
button_font = font.Font(family="Helvetica", size=16, weight="bold")

# Create and place widgets
header_label = tk.Label(window, text="Calculator", font=header_font, bg="black", fg="white")
header_label.pack(pady=10)

entry_frame = tk.Frame(window, bg="black")
entry_frame.pack(pady=10)

entry = tk.Entry(entry_frame, font=button_font, justify="right", bd=3)
entry.pack(pady=10, padx=20, fill=tk.BOTH, expand=True)

button_frame = tk.Frame(window, bg="black")
button_frame.pack(pady=10)

# Create buttons for digits and operators
buttons = [
    ('sin', 'cos', 'tan'),
    ('7', '8', '9', '/'),
    ('4', '5', '6', '*'),
    ('1', '2', '3', '-'),
    ('0', '.', '+', 'C')
]

for row in buttons:
    button_row = tk.Frame(button_frame, bg="black")
    button_row.pack(fill=tk.BOTH, expand=True)

    for value in row:
        if value in {'sin', 'cos', 'tan'}:
            if value == 'sin':
                btn = tk.Button(button_row, text=value, font=button_font, relief=tk.GROOVE, bd=1, fg="white",
                                bg="#222222", command=sin)
            elif value == 'cos':
                btn = tk.Button(button_row, text=value, font=button_font, relief=tk.GROOVE, bd=1, fg="white",
                                bg="#222222", command=cos)
            else:  # value == 'tan'
                btn = tk.Button(button_row, text=value, font=button_font, relief=tk.GROOVE, bd=1, fg="white",
                                bg="#222222", command=tan)
        elif value == 'C':
            btn = tk.Button(button_row, text=value, font=button_font, relief=tk.GROOVE, bd=1, fg="white",
                            bg="#ff4444", command=all_clear)
        else:
            btn = tk.Button(button_row, text=value, font=button_font, relief=tk.GROOVE, bd=1, fg="white",
                            bg="#222222", command=lambda v=value: add_to_expression(v) if v not in {'=', 'C'} else None)
        btn.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

# Equal button
equal_button = tk.Button(button_frame, text="=", font=button_font, relief=tk.GROOVE, bd=1, fg="white", bg="#ff8800", command=evaluate_expression)
equal_button.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

# Undo button
undo_button = tk.Button(entry_frame, text="\u21A9", font=button_font, relief=tk.GROOVE, bd=1, fg="white", bg="#999999", command=undo)
undo_button.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

# Past History button
history_button = tk.Button(entry_frame, text="⇤", font=button_font, relief=tk.GROOVE, bd=1, fg="white", bg="#999999", command=show_history)
history_button.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

result_label = tk.Label(window, text="", font=button_font, bg="black", fg="white")
result_label.pack(pady=10)

# List to store the calculation history
history = []

window.mainloop()
