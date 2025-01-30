import math
import random
import tkinter as tk
from tkinter import ttk, messagebox
import matplotlib.pyplot as plt
import numpy as np
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg
 
 
class ScientificCalculator:
    def __init__(self, root):
        self.root = root
        self.root.title("Scientific Calculator with Graphing")
        # Create main frames
        self.calc_frame = ttk.Frame(root)
        self.calc_frame.grid(row=0, column=0, padx=5, pady=5)
        self.graph_frame = ttk.Frame(root)
        self.graph_frame.grid(row=0, column=1, padx=5, pady=5)
        self.angle_mode = "DEG"  # Default to degrees
        # Calculator display
        self.display = ttk.Entry(self.calc_frame, width=40, justify="right")
        self.display.grid(row=0, column=0, columnspan=4, padx=5, pady=5)
        # Graph input
        self.graph_label = ttk.Label(self.calc_frame, text="Function to plot (use x as variable):")
        self.graph_label.grid(row=1, column=0, columnspan=4, padx=5, pady=2)
        self.graph_input = ttk.Entry(self.calc_frame, width=40)
        self.graph_input.grid(row=2, column=0, columnspan=3, padx=5, pady=2)
        self.plot_button = ttk.Button(self.calc_frame, text="Plot", command=self.plot_function)
        self.plot_button.grid(row=2, column=3, padx=5, pady=2)
        # Range inputs
        self.range_frame = ttk.Frame(self.calc_frame)
        self.range_frame.grid(row=3, column=0, columnspan=4, pady=2)
        ttk.Label(self.range_frame, text="X range:").grid(row=0, column=0, padx=2)
        self.x_min = ttk.Entry(self.range_frame, width=8)
        self.x_min.insert(0, "-10")
        self.x_min.grid(row=0, column=1, padx=2)
        ttk.Label(self.range_frame, text="to").grid(row=0, column=2, padx=2)
        self.x_max = ttk.Entry(self.range_frame, width=8)
        self.x_max.insert(0, "10")
        self.x_max.grid(row=0, column=3, padx=2)
        # Mode button (DEG/RAD)
        self.mode_button = ttk.Button(self.calc_frame, text="DEG", command=self.toggle_angle_mode)
        self.mode_button.grid(row=4, column=0, padx=2, pady=2)
        # Calculator buttons
        buttons = [
            ('7', 5, 0), ('8', 5, 1), ('9', 5, 2), ('/', 5, 3),
            ('4', 6, 0), ('5', 6, 1), ('6', 6, 2), ('*', 6, 3),
            ('1', 7, 0), ('2', 7, 1), ('3', 7, 2), ('-', 7, 3),
            ('0', 8, 0), ('.', 8, 1), ('=', 8, 2), ('+', 8, 3),
            ('sin', 9, 0), ('cos', 9, 1), ('tan', 9, 2), ('^', 9, 3),
            ('√', 10, 0), ('log', 10, 1), ('ln', 10, 2), ('()', 10, 3),
            ('π', 11, 0), ('e', 11, 1), ('C', 11, 2), ('⌫', 11, 3)
        ]
        # Create and place calculator buttons
        for (text, row, col) in buttons:
            self.create_button(text, row, col)
        self.brackets_open = False
        # Initialize matplotlib figure
        self.figure, self.ax = plt.subplots(figsize=(6, 4))
        self.canvas = FigureCanvasTkAgg(self.figure, master=self.graph_frame)
        self.canvas.get_tk_widget().pack()
    def create_button(self, text, row, col):
        button = ttk.Button(self.calc_frame, text=text, command=lambda: self.button_click(text))
        button.grid(row=row, column=col, padx=2, pady=2)
    def toggle_angle_mode(self):
        self.angle_mode = "RAD" if self.angle_mode == "DEG" else "DEG"
        self.mode_button.config(text=self.angle_mode)
    def to_radians(self, value):
        if self.angle_mode == "DEG":
            return math.radians(float(value))
        return float(value)
    def calculate_trig(self, func, value):
        rad_value = self.to_radians(value)
        if func == 'sin':
            return math.sin(rad_value)
        elif func == 'cos':
            return math.cos(rad_value)
        elif func == 'tan':
            return math.tan(rad_value)
    def plot_function(self):
        try:
            # Clear previous plot
            self.ax.clear()
            # Get function expression and range
            expr = self.graph_input.get()
            x_min = float(self.x_min.get())
            x_max = float(self.x_max.get())
            # Create x values
            x = np.linspace(x_min, x_max, 1000)
            # Replace mathematical expressions
            expr = expr.replace('sin', 'np.sin')
            expr = expr.replace('cos', 'np.cos')
            expr = expr.replace('tan', 'np.tan')
            expr = expr.replace('log', 'np.log10')
            expr = expr.replace('ln', 'np.log')
            expr = expr.replace('^', '**')
            expr = expr.replace('π', str(math.pi))
            expr = expr.replace('e', str(math.e))
            # Calculate y values
            y = eval(expr)
            # Plot the function
            self.ax.plot(x, y)
            self.ax.grid(True)
            self.ax.axhline(y=0, color='k', linestyle='-', alpha=0.3)
            self.ax.axvline(x=0, color='k', linestyle='-', alpha=0.3)
            # Set labels
            self.ax.set_xlabel('x')
            self.ax.set_ylabel('y')
            self.ax.set_title(f'y = {self.graph_input.get()}')
            # Update the canvas
            self.canvas.draw()
        except Exception as e:
            messagebox.showerror("Error", f"Invalid function: {str(e)}")
    def button_click(self, value):
        current = self.display.get()
        if value == '=':
            try:
                expression = current
                # Handle trigonometric functions
                for func in ['sin', 'cos', 'tan']:
                    if func in expression:
                        start = expression.find(func) + len(func)
                        end = len(expression)
                        value = expression[start:end]
                        result = self.calculate_trig(func, value)
                        self.display.delete(0, tk.END)
                        self.display.insert(tk.END, str(result))
                        return
                # Handle other mathematical expressions
                expression = expression.replace('π', str(math.pi))
                expression = expression.replace('e', str(math.e))
                expression = expression.replace('^', '**')
                expression = expression.replace('√', 'math.sqrt')
                expression = expression.replace('log', 'math.log10')
                expression = expression.replace('ln', 'math.log')
                result = eval(expression)
                self.display.delete(0, tk.END)
                self.display.insert(tk.END, str(result))
            except Exception as e:
                messagebox.showerror("Error", str(e))
                self.display.delete(0, tk.END)
        elif value == 'C':
            self.display.delete(0, tk.END)
        elif value == '⌫':
            self.display.delete(len(current)-1, tk.END)
        elif value == '()':
            if not self.brackets_open:
                self.display.insert(tk.END, '(')
                self.brackets_open = True
            else:
                self.display.insert(tk.END, ')')
                self.brackets_open = False
        else:
            self.display.insert(tk.END, value)
 
def main():
    root = tk.Tk()
    calculator = ScientificCalculator(root)
    root.mainloop()
 
if __name__ == "__main__":
    main()