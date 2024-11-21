# Ex.No: 6  Implementation of Zombie survival game using A* search  
### DATE : 13.09.2024
### REGISTER NUMBER : 212221240037
### AIM: 
To write a python program to simulate the Zomibie Survival game using A* Search 
### Algorithm:
1. Start the program
2. Import the necessary modules
3. Initiate the pygame engine and window
4. Collect the Zombie image and resize it within a display window 
5. Create a Euclidean distance heuristic function to find the distance from current location to Target position
6.  Move the Zombie towards the target by A* search 
7.  In main, create the obstacles and move the player by Key movements up, down,left and right 
10.  Update the display every time 
11.  Stop the program
 ### Program:
```py
import heapq
import random

# Game constants
GRID_SIZE = 20
ZOMBIE_SPEED = 1
PLAYER_SPEED = 1
SUPPLY_SPAWN_RATE = 0.1
ESCAPE_ROUTE_SPAWN_RATE = 0.05

# Game grid representation
grid = [[0 for _ in range(GRID_SIZE)] for _ in range(GRID_SIZE)]

# Player and zombie starting positions
player_x, player_y = random.randint(0, GRID_SIZE - 1), random.randint(0, GRID_SIZE - 1)
zombie_x, zombie_y = random.randint(0, GRID_SIZE - 1), random.randint(0, GRID_SIZE - 1)

# Initialize player and zombie positions on the grid
grid[player_x][player_y] = 1  # Player
grid[zombie_x][zombie_y] = 2  # Zombie

# Supply and escape route spawn
for _ in range(int(GRID_SIZE * GRID_SIZE * SUPPLY_SPAWN_RATE)):
    supply_x, supply_y = random.randint(0, GRID_SIZE - 1), random.randint(0, GRID_SIZE - 1)
    grid[supply_x][supply_y] = 3  # Supply

for _ in range(int(GRID_SIZE * GRID_SIZE * ESCAPE_ROUTE_SPAWN_RATE)):
    escape_x, escape_y = random.randint(0, GRID_SIZE - 1), random.randint(0, GRID_SIZE - 1)
    grid[escape_x][escape_y] = 4  # Escape route

# A\* search implementation
def heuristic(node, goal):
    return abs(node[0] - goal[0]) + abs(node[1] - goal[1])

def cost(node1, node2):
    return 1  # Uniform cost for simplicity

def a_star_search(start, goal):
    open_list = []
    closed_list = set()
    parent = {}
    g_score = {start: 0}
    f_score = {start: heuristic(start, goal)}

    heapq.heappush(open_list, (f_score[start], start))

    while open_list:
        current_node = heapq.heappop(open_list)[1]

        if current_node == goal:
            return reconstruct_path(parent, current_node)

        closed_list.add(current_node)

        for neighbor in get_neighbors(current_node):
            if neighbor in closed_list:
                continue

            tentative_g_score = g_score[current_node] + cost(current_node, neighbor)

            if neighbor not in g_score or tentative_g_score < g_score[neighbor]:
                parent[neighbor] = current_node
                g_score[neighbor] = tentative_g_score
                f_score[neighbor] = tentative_g_score + heuristic(neighbor, goal)
                heapq.heappush(open_list, (f_score[neighbor], neighbor))

    return None

def get_neighbors(node):
    x, y = node
    neighbors = []
    for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
        nx, ny = x + dx, y + dy
        if 0 <= nx < GRID_SIZE and 0 <= ny < GRID_SIZE:
            neighbors.append((nx, ny))
    return neighbors

def reconstruct_path(parent, current_node):
    path = [current_node]
    while current_node in parent:
        current_node = parent[current_node]
        path.append(current_node)
    return path[::-1]

# Game loop
while True:
    # Print game grid
    for row in grid:
        print(' '.join(str(cell) for cell in row))
    print()

    # Player movement
    player_move = input("Enter a direction (W/A/S/D): ")
    if player_move.upper() == 'W':
        player_x -= PLAYER_SPEED
    elif player_move.upper() == 'S':
        player_x += PLAYER_SPEED
    elif player_move.upper() == 'A':
        player_y -= PLAYER_SPEED
    elif player_move.upper() == 'D':
        player_y += PLAYER_SPEED

    # Check for game over
    if (player_x, player_y) == (zombie_x, zombie_y):
        print("Game Over! You've been caught by the zombie!")
        break

    # Check for escape route
    if grid[player_x][player_y] == 4:
        print("Congratulations! You've found an escape route!")
        break

    # Update game grid
    grid[player_x][player_y] = 1  # Player
    grid[zombie_x][zombie_y] = 2  # Zombie

    # Zombie movement using A\* search
    zombie_path = a_star_search((zombie_x, zombie_y), (player_x, player_y))
    if zombie_path:
                zombie_x, zombie_y = zombie_path[1]

    # Check for supply collection
    if grid[player_x][player_y] == 3:
        print("You've collected a supply!")
        grid[player_x][player_y] = 1  # Reset to player position

    # Update game grid
    grid[player_x][player_y] = 1  # Player
    grid[zombie_x][zombie_y] = 2  # Zombie

    # Print game status
    print(f"Player position: ({player_x}, {player_y})")
    print(f"Zombie position: ({zombie_x}, {zombie_y})")
    print()
```
### Output:
![image](https://github.com/user-attachments/assets/dcc4679b-a096-4b67-a4ca-6664973dba99)

### Result:
Thus the simple Zombie survival game was implemented using python.
