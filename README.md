import tkinter as tk
from tkinter import colorchooser

class PaintApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Мини-приложение для рисования")

        self.canvas = tk.Canvas(root, bg='white', width=600, height=400)
        self.canvas.pack()

        self.brush_color = 'black'
        self.brush_size = 2

        # Кнопки управления
        self.color_button = tk.Button(root, text="Цвет", command=self.choose_color)
        self.color_button.pack(side=tk.LEFT)

        self.clear_button = tk.Button(root, text="Очистить", command=self.clear_canvas)
        self.clear_button.pack(side=tk.LEFT)

        # Привязка событий мыши
        self.canvas.bind("<B1-Motion>", self.paint)
        self.canvas.bind("<ButtonPress-1>", self.start_draw)
        self.canvas.bind("<ButtonRelease-1>", self.stop_draw)

    def choose_color(self):
        color = colorchooser.askcolor(title="Выберите цвет")[1]
        if color:
            self.brush_color = color

    def clear_canvas(self):
        self.canvas.delete("all")

    def start_draw(self, event):
        self.last_x, self.last_y = event.x, event.y

    def paint(self, event):
        x, y = event.x, event.y
        self.canvas.create_line(self.last_x, self.last_y, x, y,
                                fill=self.brush_color, width=self.brush_size, capstyle=tk.ROUND)
        self.last_x, self.last_y = x, y

    def stop_draw(self, event):
        pass  # Можно добавить доп. логику при отпускании кнопки

if __name__ == "__main__":
    root = tk.Tk()
    app = PaintApp(root)
    root.mainloop()
