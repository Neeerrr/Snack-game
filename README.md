# Snack-game using Pythone


<h2>In Below I wil instruct and explain the code of snake gmae</h2> 
<h3>Required Modules</h3>
Before running the program make sure that you have installed the required modules:<br>
Pygame<br>
sys<br>
random<br>
copy<br>
time<br>

Especially the pygame.

 Install The Module using this command below  
 
<code>pip install pygame </code><br>

Rest all modules come pre-installed in python. Hit the above command in your Window Command Prompt or terminal to install the Required Module.


# Main Code
<pre>
<code>import pygame
import sys
import copy
import random
import time</code>
  </pre>
These are all modules you have to import into your code. Each of them has different uses in our program like we need random to spawn snake foods and manage other things which will be occurred randomly in our game.

<pre>
<code>pygame.init()

width = 500
height = 500
scale = 10
score = 0

food_x = 10
food_y = 10

display = pygame.display.set_mode((width, height))
pygame.display.set_caption("Snake Game")
clock = pygame.time.Clock()

background = (23, 32, 42)
snake_colour = (236, 240, 241)
food_colour = (148, 49, 38)
snake_head = (247, 220, 111)</code>
  </pre>

So this part of the code is for setup of the game window like resolution and initializing the score and food points with also setup of the background color of the game with also defining the color of the snake, snakehead, and food color.

# ----------- Snake Class ----------------
<pre>
<code>class Snake:
    def __init__(self, x_start, y_start):
        self.x = x_start
        self.y = y_start
        self.w = 10
        self.h = 10
        self.x_dir = 1
        self.y_dir = 0
        self.history = [[self.x, self.y]]
        self.length = 1

    def reset(self):
        self.x = width/2-scale
        self.y = height/2-scale
        self.w = 10
        self.h = 10
        self.x_dir = 1
        self.y_dir = 0
        self.history = [[self.x, self.y]]
        self.length = 1

    def show(self):
        for i in range(self.length):
            if not i == 0:
                pygame.draw.rect(display, snake_colour, (self.history[i][0], self.history[i][1], self.w, self.h))
            else:
                pygame.draw.rect(display, snake_head, (self.history[i][0], self.history[i][1], self.w, self.h))

    def check_eaten(self):
        if abs(self.history[0][0] - food_x) < scale and abs(self.history[0][1] - food_y) < scale:
            return True

    def grow(self):
        self.length += 1
        self.history.append(self.history[self.length-2])

    def death(self):
        i = self.length - 1
        while i > 0:
            if abs(self.history[0][0] - self.history[i][0]) < self.w and abs(self.history[0][1] - self.history[i][1]) < self.h and self.length > 2:
                return True
            i -= 1

    def update(self):
        i = self.length - 1
        while i > 0:
            self.history[i] = copy.deepcopy(self.history[i-1])
            i -= 1
        self.history[0][0] += self.x_dir*scale
        self.history[0][1] += self.y_dir*scale</code>
  </pre>


So in these parts of the code contain snake class having different functions properties like snake eater, check, reset and restart, etc. This determines the important properties of the snake game.

# ----------- Food Class --------------
<pre>
<code>class Food:
    def new_location(self):
        global food_x, food_y
        food_x = random.randrange(1, width/scale-1)*scale
        food_y = random.randrange(1, height/scale-1)*scale

    def show(self):
        pygame.draw.rect(display, food_colour, (food_x, food_y, scale, scale))


def show_score():
    font = pygame.font.SysFont("Copperplate Gothic Bold", 20)
    text = font.render("Score: " + str(score), True, snake_colour)
    display.blit(text, (scale, scale))</code>
  </pre>


Like we have created a snake class similarly we also need a food class. Which is an important aspect of our game. Our score class contains functions like show score, locations, etc. These functions will randomly decide the spawn of food for snakes which will be done with the help of a random module.

# ----------- Main Game Loop -------------
<pre>
<code>def gameLoop():
    loop = True

    global score

    snake = Snake(width/2, height/2)
    food = Food()
    food.new_location()

    while loop:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                sys.exit()
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_q:
                    pygame.quit()
                    sys.exit()
                if snake.y_dir == 0:
                    if event.key == pygame.K_UP:
                        snake.x_dir = 0
                        snake.y_dir = -1
                    if event.key == pygame.K_DOWN:
                        snake.x_dir = 0
                        snake.y_dir = 1

                if snake.x_dir == 0:
                    if event.key == pygame.K_LEFT:
                        snake.x_dir = -1
                        snake.y_dir = 0
                    if event.key == pygame.K_RIGHT:
                        snake.x_dir = 1
                        snake.y_dir = 0

        display.fill(background)

        snake.show()
        snake.update()
        food.show()
        show_score()

        if snake.check_eaten():
            food.new_location()
            score += 1
            snake.grow()

        if snake.death():
            score = 0
            font = pygame.font.SysFont("Copperplate Gothic Bold", 50)
            text = font.render("Game Over!", True, snake_colour)
            display.blit(text, (width/2-50, height/2))
            pygame.display.update()
            time.sleep(3)
            snake.reset()

        if snake.history[0][0] > width:
            snake.history[0][0] = 0
        if snake.history[0][0] < 0:
            snake.history[0][0] = width

        if snake.history[0][1] > height:
            snake.history[0][1] = 0
        if snake.history[0][1] < 0:
            snake.history[0][1] = height

        pygame.display.update()
        clock.tick(10)
                                   gameLoop()</code>
  </pre>


# Out put 
![image](https://user-images.githubusercontent.com/98451723/169068990-80c06274-523a-410a-9655-a848ab4a4398.png)



Know it's done enjoy the game ☺️☺️


