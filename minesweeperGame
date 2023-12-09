#Подключение библиотек
from tkinter import * 
from tkinter import simpledialog, messagebox
import random






class Minesweeper: #Определение переменных и т.д
    def __init__(self, master, level):
        self.master = master
        self.level = level
        self.GRID_SIZE, self.SQUARE_SIZE, self.MINES_NUM = self.get_difficulty_settings(level)
        self.mines = set(random.sample(range(1, self.GRID_SIZE**2 + 1), self.MINES_NUM))
        self.clicked = set()
        self.marked = set()
        self.remaining_mines = self.MINES_NUM
        self.flag_counter = 0

        self.create_widgets()

    def get_difficulty_settings(self, level): # Настройки для уровней сложности
        
        if level == "легкий":     
            return 5, 30, 3
        elif level == "средний":
            return 10, 25, 10
        elif level == "тяжелый":
            return 25, 20, 75
        else:
            raise ValueError("Invalid difficulty level")

    def create_widgets(self): # Создани е интерфейса
        
        self.c = Canvas(self.master, width=self.GRID_SIZE * self.SQUARE_SIZE, height=self.GRID_SIZE * self.SQUARE_SIZE)
        self.c.bind("<Button-1>", self.click)
        self.c.bind("<Button-3>", self.mark_mine)
        self.c.pack()

        self.mine_counter_var = StringVar()
        self.mine_counter_var.set(f"Mines Left: {self.remaining_mines}")
        mine_counter_label = Label(self.master, textvariable=self.mine_counter_var)
        mine_counter_label.pack()

        self.flag_counter_var = StringVar()
        self.flag_counter_var.set(f"Flags: {self.flag_counter}")
        flag_counter_label = Label(self.master, textvariable=self.flag_counter_var)
        flag_counter_label.pack()

        for i in range(self.GRID_SIZE):
            for j in range(self.GRID_SIZE):
                self.c.create_rectangle(i * self.SQUARE_SIZE, j * self.SQUARE_SIZE,
                                        i * self.SQUARE_SIZE + self.SQUARE_SIZE,
                                        j * self.SQUARE_SIZE + self.SQUARE_SIZE, fill='gray')

    def check_mines(self, neighbors): # Проверка на соседние клетки
        return len(self.mines.intersection(neighbors))

    def generate_neighbors(self, square): # Создание соседей для клеток
        row, col = (square - 1) // self.GRID_SIZE, (square - 1) % self.GRID_SIZE
        neighbors = set()
        for i in range(max(0, row - 1), min(row + 2, self.GRID_SIZE)):
            for j in range(max(0, col - 1), min(col + 2, self.GRID_SIZE)):
                neighbors.add(i * self.GRID_SIZE + j + 1)
        return neighbors

    def end_game(self, is_winner=False): # Завершение игры с уведомлением о проигрыше/выйгрыше
        if is_winner:
            messagebox.showinfo("Конец игры", "Поздравляю! Вы выиграли!")
        else:
            messagebox.showinfo("Конец игры", "Конец игры. Вы наступили на мину.")
        self.master.quit()

    def clearance(self, ids): # Раскрытие клеток, где нету мин
        self.clicked.add(ids)
        ids_neigh = self.generate_neighbors(ids)
        around = self.check_mines(ids_neigh)

        if ids in self.mines:
            self.c.itemconfig(ids, fill="red")
            self.end_game(False)
            return

        self.c.itemconfig(ids, fill="green")

        if around == 0:
            neigh_list = list(ids_neigh - self.clicked)
            while neigh_list:
                item = neigh_list.pop()
                self.c.itemconfig(item, fill="green")
                item_neigh = self.generate_neighbors(item)
                item_around = self.check_mines(item_neigh)
                if item_around > 0:
                    if item not in self.clicked:
                        x1, y1, _, _ = self.c.coords(item)
                        self.c.create_text(x1 + self.SQUARE_SIZE / 2,
                                           y1 + self.SQUARE_SIZE / 2,
                                           text=str(item_around),
                                           font="Arial {}".format(int(self.SQUARE_SIZE / 2)),
                                           fill='yellow')
                else:
                    neigh_list.extend(set(item_neigh) - self.clicked)
                    neigh_list = list(set(neigh_list))
                self.clicked.add(item)
        else:
            x1, y1, _, _ = self.c.coords(ids)
            self.c.create_text(x1 + self.SQUARE_SIZE / 2,
                               y1 + self.SQUARE_SIZE / 2,
                               text=str(around),
                               font="Arial {}".format(int(self.SQUARE_SIZE / 2)),
                               fill='yellow')

        if len(self.clicked) == self.GRID_SIZE**2 - self.MINES_NUM:
            self.end_game(True)

    def click(self, event): # Обработка левого клика мыши
        ids = self.c.find_withtag(CURRENT)[0]
        if ids not in self.clicked and ids not in self.marked:
            self.clearance(ids)
        self.c.update()

    def mark_mine(self, event): # Обработка правого клика мыши для флажков
        ids = self.c.find_withtag(CURRENT)[0]
        if ids not in self.clicked:
            if ids in self.marked:
                self.marked.remove(ids)
                self.flag_counter -= 1
                self.c.itemconfig(ids, fill="gray")
            else:
                self.marked.add(ids)
                self.flag_counter += 1
                self.c.itemconfig(ids, fill="yellow")
        



def get_difficulty_level(): # Запрос уровня сложности у игрока
    root = Tk()
    root.withdraw()
    level_choice = simpledialog.askstring("Сапер", "Выберите уровень сложности >> (легкий, средний, тяжелый,):")
    return level_choice.lower()


def start_game(difficulty_level): # Запуск выбранного уровня сложности
    print(f"Starting Minesweeper - Difficulty Level: {difficulty_level.capitalize()}")
    root = Tk()
    root.title("Сапер")
    Minesweeper(root, difficulty_level)
    root.mainloop()


if __name__ == "__main__":
    difficulty_level = get_difficulty_level()

    if difficulty_level in ["легкий", "средний", "тяжелый" ]: # Проверка на уровень сложности
        start_game(difficulty_level)
    else:
        print("Invalid difficulty level. Exiting...")
