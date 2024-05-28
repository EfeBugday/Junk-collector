import pygame
import random

# Initialize Pygame
pygame.init()

# Define constants
GRID_SIZE = 20
CELL_SIZE = 50
WINDOW_SIZE = GRID_SIZE * CELL_SIZE
QUOTA = 500

# Define item types and their properties
ITEMS = {
    'C': {'name': 'Can', 'value': 5, 'count': int(0.05 * GRID_SIZE * GRID_SIZE)},
    'W': {'name': 'Wooden Crate', 'value': 10, 'count': int(0.02 * GRID_SIZE * GRID_SIZE)},
    'P': {'name': 'Golden Pendant', 'value': 100, 'count': int(0.005 * GRID_SIZE * GRID_SIZE)}
}

# Load images
IMAGES = {
    'C': pygame.image.load('can.png'),
    'W': pygame.image.load('wooden_crate.png'),
    'P': pygame.image.load('golden_pendant.png')
}

# Resize images to fit in the grid cells
for key in IMAGES:
    IMAGES[key] = pygame.transform.scale(IMAGES[key], (CELL_SIZE, CELL_SIZE))

# Colors
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

# Initialize the screen
screen = pygame.display.set_mode((WINDOW_SIZE, WINDOW_SIZE))
pygame.display.set_caption('Item Collection Game')

# Create the grid
grid = [['' for _ in range(GRID_SIZE)] for _ in range(GRID_SIZE)]

# Function to scatter items randomly on the grid
def scatter_items():
    for item_type, properties in ITEMS.items():
        count = properties['count']
        while count > 0:
            x, y = random.randint(0, GRID_SIZE - 1), random.randint(0, GRID_SIZE - 1)
            if grid[x][y] == '':
                grid[x][y] = item_type
                count -= 1

# Draw the grid and items
def draw_grid():
    for x in range(GRID_SIZE):
        for y in range(GRID_SIZE):
            rect = pygame.Rect(x * CELL_SIZE, y * CELL_SIZE, CELL_SIZE, CELL_SIZE)
            pygame.draw.rect(screen, WHITE, rect)
            pygame.draw.rect(screen, BLACK, rect, 1)
            if grid[x][y]:
                screen.blit(IMAGES[grid[x][y]], rect.topleft)

# Main game loop
def main():
    scatter_items()
    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        screen.fill(BLACK)
        draw_grid()
        pygame.display.flip()

    pygame.quit()

if __name__ == '__main__':
    main()
