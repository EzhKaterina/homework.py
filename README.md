# Создайте программу для игры в 'Крестики-нолики'
# НЕОБЯЗАТЕЛЬНО Добавить игру против бота с интеллектом


import random

random_number = random.randrange

pole = [9,8,7,
        6,5,4,
        3,2,1]

victories = [[0,1,2],
             [3,4,5],
             [6,7,8],
             [0,3,6],
             [1,4,7],
             [2,5,8],
             [0,4,8],
             [2,4,6]]


def print_pole():
    print(f'{pole[0]}', end=" ")
    print(f'{pole[1]}', end=" ")
    print(f'{pole[2]}')

    print(f'{pole[3]}', end=" ")
    print(f'{pole[4]}', end=" ")
    print(f'{pole[5]}')

    print(f'{pole[6]}', end=" ")
    print(f'{pole[7]}', end=" ")
    print(f'{pole[8]}')


def step_pole(step, symbol):
    win = pole.index(step)
    pole[win] = symbol


# текущий результат игры
def get_result():
    win = ""

    for i in victories:
        if pole[i[0]] == "X" and pole[i[1]] == "X" and pole[i[2]] == "X":
            win = "X"
        if pole[i[0]] == "O" and pole[i[1]] == "O" and pole[i[2]] == "O":
            win = "O"

    return win


# Искусственный интеллект: поиск линии с нужным количеством X и O на победных линиях
def check_line(sum_O, sum_X):
    step = ""
    for line in victories:
        o = 0
        x = 0

        for j in range(0, 3):
            if pole[line[j]] == "O":
                o = o + 1
            if pole[line[j]] == "X":
                x = x + 1

        if o == sum_O and x == sum_X:
            for j in range(0, 3):
                if pole[line[j]] != "O" and pole[line[j]] != "X":
                    step = pole[line[j]]

    return step


# Ии: выбор хода
def AI():
    step = ""

    # если на какой либо из победных линий 2 свои фигуры и 0 чужих - ставим
    step = check_line(2, 0)

    # если на какой либо из победных линий 2 чужие фигуры и 0 своих - ставим
    if step == "":
        step = check_line(0, 2)

        # если 1 фигура своя и 0 чужих - ставим
    if step == "":
        step = check_line(1, 0)

        # центр пуст, то занимаем центр
    if step == "":
        if pole[4] != "X" and pole[4] != "O":
            step = 5

            # если центр занят, то занимаем первую ячейку
    if step == "":
        if pole[0] != "X" and pole[0] != "O":
            step = 1

    return step

game_over = False
human = True

while game_over == False:

    #показать поле
    print_pole()


    if human == True:
        symbol = "X"
        step = int(input("Человек, ваш ход: "))
    else:
        print("Компьютер делает ход: ")
        symbol = "O"
        step = AI()


    if step != "":
        step_pole(step, symbol)  # делаем ход в указанную ячейку
        win = get_result()  # определим победителя
        if win != "":
            game_over = True
        else:
            game_over = False
    else:
        print("Ничья!")
        game_over = True
        win = "дружба"

    human = not (human)


print_pole()
print("Победил", win)
