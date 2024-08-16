# Ex.No: 1  Implementation of HotPotato game using Queue 
### DATE:                                                                            
### AIM: 
To write a python program to simulate the process of passing an item among players and eliminating players based on the given rules until a single winner is determined.
### Algorithm:
1. Initialize the Queue: Create a queue and enqueue all the participants.
2. Pass the Potato: Dequeue the first person in the queue and enqueue them at the end. This simulates passing the potato.
3. Count the Passes: Repeat the passing for a given number of times.
4. Eliminate the Holder: After the set number of passes, remove the person who holds the potato (dequeue the front of the queue).
5. Repeat: Continue the process until only one person remains in the queue.
### Program:
```
Developed By:P Siva Naga Nithin
Reg.No: 212221240037
```
```
import pygame
import sys
import random
import math

# Initialize Pygame
pygame.init()

# Constants
SCREEN_WIDTH, SCREEN_HEIGHT = 800, 600
BACKGROUND_COLOR = (30, 30, 30)
PLAYER_COLOR = (255, 100, 100)
FONT_COLOR = (255, 255, 255)
FPS = 60

# Setup the screen
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Hot Potato Game")
clock = pygame.time.Clock()

# Font
font = pygame.font.Font(None, 36)

# Players
players = ["Alice", "Bob", "Charlie", "David", "Eve"]
num_players = len(players)

# Circle Positions
center_x, center_y = SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2
radius = 200
angles = [2 * math.pi * i / num_players for i in range(num_players)]

# Game variables
current_index = 0
timer = 0
pass_interval = 1000  # Time in milliseconds between passes

def draw_players():
    for i, angle in enumerate(angles):
        x = center_x + radius * math.cos(angle)
        y = center_y + radius * math.sin(angle)
        pygame.draw.circle(screen, PLAYER_COLOR, (int(x), int(y)), 40)
        name_text = font.render(players[i], True, FONT_COLOR)
        screen.blit(name_text, (int(x) - name_text.get_width() // 2, int(y) - name_text.get_height() // 2))

def remove_player(index):
    del players[index]
    del angles[index]
    for i in range(len(angles)):
        angles[i] = 2 * math.pi * i / len(players)

# Main Game Loop
running = True
while running:
    screen.fill(BACKGROUND_COLOR)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    if len(players) > 1:
        timer += clock.get_time()
        if timer >= pass_interval:
            timer = 0
            current_index = (current_index + 1) % len(players)

        draw_players()

        # Highlight the player with the potato
        x = center_x + radius * math.cos(angles[current_index])
        y = center_y + radius * math.sin(angles[current_index])
        pygame.draw.circle(screen, (255, 255, 0), (int(x), int(y)), 50, 5)
        
        if random.random() < 0.01:  # Randomly remove a player
            remove_player(current_index)
            current_index %= len(players)

    else:
        # Display the winner
        winner_text = font.render(f"The winner is {players[0]}", True, FONT_COLOR)
        screen.blit(winner_text, (SCREEN_WIDTH // 2 - winner_text.get_width() // 2, SCREEN_HEIGHT // 2 - winner_text.get_height() // 2))

    pygame.display.flip()
    clock.tick(FPS)

pygame.quit()
sys.exit()

```
### Output:
<img width="801" alt="Screenshot 2024-08-09 at 2 42 37 PM" src="https://github.com/user-attachments/assets/d82643a6-4db3-4ce3-8e84-2d8ee97cb639">
<img width="804" alt="Screenshot 2024-08-09 at 2 41 46 PM" src="https://github.com/user-attachments/assets/e0c6221e-cf02-4770-b045-430e82d34a14">



### Result:
Thus the simple HotPotato game was implemented using Queue.
