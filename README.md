# Snake-Game-Development
import random
import os
import pygame

pygame.init()

# Screen setup
screen_width = 800
screen_height = 600
game_window = pygame.display.set_mode((screen_width, screen_height))

# Colours
white = (255, 255, 255)
red = (255, 0, 0)
black = (0, 0, 0)

# Game title
pygame.display.set_caption("Snake with Vaibhav")
pygame.display.update()
clock = pygame.time.Clock()
font = pygame.font.SysFont(None, 55)

# Check if hiscore.txt exists, if not create it with 0
if not os.path.exists("hiscore.txt"):
    with open("hiscore.txt", "w") as f:
        f.write("0")

# Read the hiscore from the file
with open("hiscore.txt", "r") as f:
    hiscore = f.read()

# Convert hiscore to integer
try: # Exception handling
    hiscore = int(hiscore)

except ValueError:
    hiscore = 0
# Function to display score
def text_score(text, colour, x, y):
    screen_font = font.render(text, True, colour)
    game_window.blit(screen_font, (x, y))

# Function to plot the snake
def plot_snake(game_window, colour, snk_list, snake_size):
    for x, y in snk_list:
        pygame.draw.rect(game_window, colour, [x, y, snake_size, snake_size])  # Corrected this line

# Game loop
def gameloop():
    global hiscore  # Declare that we are using the global 'hiscore' variable

    # Game-specific variables
    exit_game = False
    game_over = False
    snake_x = 45
    snake_y = 55
    snake_size = 26
    fps = 60
    velocity_x = 0
    velocity_y = 0
    snk_list = []
    snk_length = 1

    food_x = random.randint(20, int(screen_width / 2))
    food_y = random.randint(20, int(screen_height / 2))

    score = 0
    init_velocity = 5

    while not exit_game:
        if game_over:
            # Update the hiscore if the current score is higher
            if score > hiscore:
                hiscore = score
                with open("hiscore.txt", "w") as f:
                    f.write(str(hiscore))

            game_window.fill(white)
            text_score("game over ", red, 80, 240)

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    exit_game = True
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RETURN:
                        gameloop()

        else:
            # Event handling
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    exit_game = True
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_RIGHT:
                        velocity_x = init_velocity
                        velocity_y = 0
                    if event.key == pygame.K_LEFT:
                        velocity_x = -init_velocity
                        velocity_y = 0
                    if event.key == pygame.K_UP:
                        velocity_y = -init_velocity
                        velocity_x = 0
                    if event.key == pygame.K_DOWN:
                        velocity_y = init_velocity
                        velocity_x = 0

            # Move the snake
            snake_x += velocity_x
            snake_y += velocity_y

            # Check for collision with food
            if abs(snake_x - food_x) < 6 and abs(snake_y - food_y) < 6:
                score += 1
                food_x = random.randint(20, int(screen_width / 2))
                food_y = random.randint(20, int(screen_height / 2))
                snk_length += 5  # Increase snake length

            # Fill background and display score
            game_window.fill(white)
            text_score("Vaibhav Score: " + str(score * 10) + "  Hiscore: " + str(hiscore * 10), red, 5, 5)

            # Draw food
            pygame.draw.rect(game_window, red, [food_x, food_y, snake_size, snake_size])

            # Update snake position and check for collisions
            [16:39, 2/6/2025] +91 91047 03483: head = [snake_x, snake_y]
            snk_list.append(head)
            if len(snk_list) > snk_length:
                del snk_list[0]

            # Check for collision with the snake itself
            if head in snk_list[:-1]:
                game_over = True

            # Check for boundaries
            if snake_x < 0 or snake_x > screen_width or snake_y < 0 or snake_y > screen_height:
                game_over = True

            # Plot the snake
            plot_snake(game_window, black, snk_list, snake_size)

        # Update the display and tick the clock
        pygame.display.update()
        clock.tick(fps)

    pygame.quit()
    quit()

gameloop()

