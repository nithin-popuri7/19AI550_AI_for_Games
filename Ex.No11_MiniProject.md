# Ex.No: 11  Mini Project 
### Date: 25.10.2024
### Register Number: 212221240037
### Aim: 
To write a Python program to simulate the game using Pygame.
### Algorithm:
1. **Initialize the Game Environment**: 
    - Initialize Pygame and set up screen dimensions, colors, and fonts.
2. **Define Game Elements**: 
    - Define player and monster positions, speeds, and characteristics.
3. **Game Mechanics**: 
    - Implement player movement, monster AI, and obstacle collision.
4. **Power-Ups**: 
    - Add logic for power-ups that temporarily alter gameplay.
5. **Game Loop**: 
    - Continuously update the screen, check for events, and refresh display until the game is over or won.
6. **Winning and Losing Conditions**: 
    - Define conditions to win or lose the game, displaying relevant messages.

### Program:
~~~py
import pygame
import random
import math
import time

# Initialize Pygame
pygame.init()

# Screen dimensions and title
WIDTH, HEIGHT = 1000, 700
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Survivor")

# Colors and Fonts
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
YELLOW = (255, 255, 0)
font = pygame.font.Font(None, 36)
title_font = pygame.font.Font(None, 48)

# Player and Monster settings (using circles instead of images)
player_radius = 25
monster_radius = 35
player_pos = [WIDTH // 2, HEIGHT // 2]
player_speed = 5

monster_pos = [random.randint(0, WIDTH), random.randint(0, HEIGHT)]
monster_speed = 2
monster_speed_increment = 0.1  # Increase monster speed with time

# Power-up settings
power_up_active = False
power_up_type = None
power_up_duration = 5
power_up_timer = 0
power_up_pos = [random.randint(50, WIDTH - 50), random.randint(50, HEIGHT - 50)]
power_up_spawn_time = random.randint(5, 10)

# Obstacle settings
obstacles = []
for _ in range(5):  # Adding 5 obstacles randomly
    obstacles.append(pygame.Rect(random.randint(0, WIDTH - 50), random.randint(0, HEIGHT - 50), 50, 50))

# Game variables
running = True
score = 0
level = 1
game_over = False
won_game = False
start_time = time.time()
time_played = 0

# Game loop
while running:
    screen.fill(BLACK)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False
        elif event.type == pygame.KEYDOWN and (game_over or won_game):
            # Reset the game on key press after loss or win
            player_pos = [WIDTH // 2, HEIGHT // 2]
            monster_pos = [random.randint(0, WIDTH), random.randint(0, HEIGHT)]
            monster_speed = 2
            game_over = False
            won_game = False
            score = 0
            start_time = time.time()
            time_played = 0
            level = 1
            power_up_active = False

    if not game_over and not won_game:
        # Update score, time played, and level
        score += 1
        time_played = int(time.time() - start_time)
        if time_played // 10 > level and level < 10:  # Level up every 10 seconds until level 10
            level += 1
            monster_speed += monster_speed_increment

        # Check for win condition
        if level == 10:
            won_game = True

        # Power-up logic
        if not power_up_active and time_played % power_up_spawn_time == 0:
            power_up_active = True
            power_up_type = random.choice(["speed", "freeze"])
            power_up_timer = time_played + power_up_duration
            power_up_pos = [random.randint(50, WIDTH - 50), random.randint(50, HEIGHT - 50)]

        if power_up_active:
            if time_played >= power_up_timer:
                power_up_active = False
                power_up_type = None

            # Check if player collects the power-up
            if math.hypot(player_pos[0] - power_up_pos[0], player_pos[1] - power_up_pos[1]) < player_radius:
                if power_up_type == "speed":
                    player_speed += 3
                elif power_up_type == "freeze":
                    monster_speed /= 2
                power_up_active = False

        # Player movement with obstacle collision
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and player_pos[0] - player_radius > 0:
            player_pos[0] -= player_speed
        if keys[pygame.K_RIGHT] and player_pos[0] + player_radius < WIDTH:
            player_pos[0] += player_speed
        if keys[pygame.K_UP] and player_pos[1] - player_radius > 0:
            player_pos[1] -= player_speed
        if keys[pygame.K_DOWN] and player_pos[1] + player_radius < HEIGHT:
            player_pos[1] += player_speed

        # Check if player is inside an obstacle
        player_rect = pygame.Rect(player_pos[0] - player_radius, player_pos[1] - player_radius, player_radius * 2, player_radius * 2)
        for obstacle in obstacles:
            if player_rect.colliderect(obstacle):
                game_over = True

        # Monster chasing behavior
        dx = player_pos[0] - monster_pos[0]
        dy = player_pos[1] - monster_pos[1]
        distance = math.hypot(dx, dy)
        if distance != 0:
            dx, dy = dx / distance, dy / distance  # Normalize direction
            monster_pos[0] += dx * monster_speed
            monster_pos[1] += dy * monster_speed

        # Check for loss condition
        if distance < (player_radius + monster_radius):
            game_over = True

    # Draw game title and level
    title_text = title_font.render(f"SURVIVOR - Level {level}", True, RED)
    screen.blit(title_text, (WIDTH // 2 - title_text.get_width() // 2, 20))

    # Draw player, monster, and power-up as circles
    pygame.draw.circle(screen, GREEN, player_pos, player_radius)
    pygame.draw.circle(screen, RED, monster_pos, monster_radius)
    if power_up_active:
        color = YELLOW if power_up_type == "speed" else BLUE
        pygame.draw.circle(screen, color, power_up_pos, 20)

    # Draw obstacles
    for obstacle in obstacles:
        pygame.draw.rect(screen, BLUE, obstacle)

    # Display score, time, and level
    score_text = font.render(f"Score: {score}", True, WHITE)
    time_text = font.render(f"Time Played: {time_played}s", True, WHITE)
    level_text = font.render(f"Level: {level}", True, WHITE)
    screen.blit(score_text, (10, 10))
    screen.blit(time_text, (10, 40))
    screen.blit(level_text, (10, 70))

    # Display game over or win message
    if game_over:
        game_over_text = font.render("Game Over! Press any key to restart", True, RED)
        screen.blit(game_over_text, (WIDTH // 2 - game_over_text.get_width() // 2, HEIGHT // 2))
    elif won_game:
        win_text = font.render("Congratulations, you won! Press any key to restart", True, GREEN)
        screen.blit(win_text, (WIDTH // 2 - win_text.get_width() // 2, HEIGHT // 2))

    # Update display and set frame rate
    pygame.display.flip()
    pygame.time.Clock().tick(60)  # Setting FPS to 60 for smoother gameplay

pygame.quit()

~~~
### Output:

![1](https://github.com/user-attachments/assets/c9fb55e4-d975-4b71-99a8-d2ba9986740c)
![2](https://github.com/user-attachments/assets/74638947-720c-4d0b-bbff-c1dbac71b462)

### Result:
Thus, the simple game was implemented using Python and Pygame.
