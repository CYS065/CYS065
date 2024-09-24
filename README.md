import pygame
import random

# Initialize the game
pygame.init()

# Set the width and height of the game window
window_width = 800
window_height = 600
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption("Snake Game")

# Set the colors
black = (0, 0, 0)
white = (255, 255, 255)
red = (255, 0, 0)

# Set the snake properties
snake_block_size = 20
snake_speed = 15

# Define the snake
def snake(snake_block_size, snake_list):
    for x in snake_list:
        pygame.draw.rect(window, black, [x[0], x[1], snake_block_size, snake_block_size])

# Run the game loop
def game_loop():
    game_over = False
    game_close = False

    # Set the initial position of the snake
    x1 = window_width / 2
    y1 = window_height / 2

    # Set the initial change in position of the snake
    x1_change = 0
    y1_change = 0

    # Create an empty list to store the snake's body
    snake_list = []
    length_of_snake = 1

    # Generate the initial position of the food
    foodx = round(random.randrange(0, window_width - snake_block_size) / 20.0) * 20.0
    foody = round(random.randrange(0, window_height - snake_block_size) / 20.0) * 20.0

    # Run the game until it is over
    while not game_over:

        # If the game is closed, display the game over message
        while game_close:
            window.fill(white)
            font_style = pygame.font.SysFont(None, 50)
            message = font_style.render("Game Over! Press Q-Quit or C-Play Again", True, red)
            window.blit(message, [window_width / 6, window_height / 3])
            pygame.display.update()

            # Check for user input to quit or play again
            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        game_loop()

        # Check for user input to move the snake
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block_size
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block_size
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block_size
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block_size
                    x1_change = 0

        # Check if the snake hits the boundaries of the game window
        if x1 >= window_width or x1 < 0 or y1 >= window_height or y1 < 0:
            game_close = True

        # Update the position of the snake
        x1 += x1_change
        y1 += y1_change
        window.fill(white)
        pygame.draw.rect(window, red, [foodx, foody, snake_block_size, snake_block_size])
        snake_head = []
        snake_head.append(x1)
        snake_head.append(y1)
        snake_list.append(snake_head)
        if len(snake_list) > length_of_snake:
            del snake_list[0]

        # Check if the snake hits itself
        for x in snake_list[:-1]:
            if x == snake_head:
                game_close = True

        # Draw the snake
        snake(snake_block_size, snake_list)

        # Update the game window
        pygame.display.update()

        # Check if the snake eats the food
        if x1 == foodx and y1 == foody:
            foodx = round(random.randrange(0, window_width - snake_block_size) / 20.0) * 20.0
            foody = round(random.randrange(0, window_height - snake_block_size) / 20.0) * 20.0
            length_of_snake += 1

        # Set the game speed
        clock = pygame.time.Clock()
        clock.tick(snake_speed)

    # Quit the game
    pygame.quit()


# Run the game loop
game_loop()
