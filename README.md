# Python-king-pranjal-singh-
import tkinter as tk
from tkinter import scrolledtext, messagebox

def generate_code():
    user_prompt = entry.get("1.0", tk.END).strip().lower()
    if not user_prompt:
        messagebox.showerror("Error", "Please enter something!")
        return

    code_output.delete("1.0", tk.END)

    if "calculator" in user_prompt:
        code = """ 
def calculator():
    print("Simple Calculator")
    a = int(input("Enter first number: "))
    b = int(input("Enter second number: "))
    op = input("Enter operation (+,-,*,/): ")
    if op == '+':
        print("Result:", a+b)
    elif op == '-':
        print("Result:", a-b)
    elif op == '*':
        print("Result:", a*b)
    elif op == '/':
        print("Result:", a/b if b!=0 else "Division by zero not allowed")
    else:
        print("Invalid operation")

calculator()
"""
    elif "snake" in user_prompt or "game" in user_prompt:
        code = """ 
import pygame
import random

pygame.init()
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)
screen_width = 600
screen_height = 400
gameWindow = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Snake Game")

clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 35)

def text_screen(text, color, x, y):
    screen_text = font.render(text, True, color)
    gameWindow.blit(screen_text, [x,y])

def plot_snake(gameWindow, color, snk_list, snake_size):
    for x,y in snk_list:
        pygame.draw.rect(gameWindow, color, [x, y, snake_size, snake_size])

def gameLoop():
    exit_game = False
    game_over = False
    snake_x = 45
    snake_y = 55
    snake_size = 15
    fps = 15
    velocity_x = 0
    velocity_y = 0
    food_x = random.randint(20, screen_width/2)
    food_y = random.randint(20, screen_height/2)
    score = 0
    snk_list = []
    snk_length = 1

    while not exit_game:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                exit_game = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_RIGHT:
                    velocity_x = 10
                    velocity_y = 0
                if event.key == pygame.K_LEFT:
                    velocity_x = -10
                    velocity_y = 0
                if event.key == pygame.K_UP:
                    velocity_y = -10
                    velocity_x = 0
                if event.key == pygame.K_DOWN:
                    velocity_y = 10
                    velocity_x = 0
        snake_x += velocity_x
        snake_y += velocity_y

        if abs(snake_x - food_x)<10 and abs(snake_y - food_y)<10:
            score +=10
            food_x = random.randint(20, screen_width/2)
            food_y = random.randint(20, screen_height/2)
            snk_length +=5

        gameWindow.fill(white)
        text_screen("Score: " + str(score), red, 5, 5)
        pygame.draw.rect(gameWindow, red, [food_x, food_y, snake_size, snake_size])

        head = []
        head.append(snake_x)
        head.append(snake_y)
        snk_list.append(head)

        if len(snk_list)>snk_length:
            del snk_list[0]

        plot_snake(gameWindow, black, snk_list, snake_size)
        pygame.display.update()
        clock.tick(fps)

    pygame.quit()
    quit()

gameLoop()
"""
    elif "guess" in user_prompt:
        code = """
import random
number = random.randint(1, 100)
attempts = 0
print("Guess the number between 1 and 100")

while True:
    guess = int(input("Enter your guess: "))
    attempts += 1
    if guess < number:
        print("Too low!")
    elif guess > number:
        print("Too high!")
    else:
        print(f"Correct! You guessed it in {attempts} attempts.")
        break
"""
    else:
        code = "Sorry, I don't have code for that yet!"

    code_output.insert(tk.END, code)

root = tk.Tk()
root.title("Offline Auto-Coder")
root.geometry("700x600")

tk.Label(root, text="Describe:", font=("Arial", 12)).pack(pady=5)
entry = tk.Text(root, height=4, width=80, font=("Arial", 10))
entry.pack(pady=5)

tk.Button(root, text="Generate Code", font=("Arial", 12, "bold"), bg="lightblue",
          command=generate_code).pack(pady=10)

tk.Label(root, text="Result:", font=("Arial", 12)).pack(pady=5)
code_output = scrolledtext.ScrolledText(root, height=20, width=80, font=("Courier", 10))
code_output.pack(pady=5)

root.mainloop()
