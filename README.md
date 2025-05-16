import pygame
import random
import sys

# Initialize
pygame.init()

# Screen
width, height = 500, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Truck Dodge Game")

# Colors
WHITE = (255, 255, 255)
RED = (200, 0, 0)
BLUE = (0, 0, 255)

# Clock
clock = pygame.time.Clock()

# Truck
truck_width = 50
truck_height = 80
truck_x = width // 2 - truck_width // 2
truck_y = height - truck_height - 10
truck_speed = 5

# Obstacle
obs_width = 50
obs_height = 50
obs_x = random.randint(0, width - obs_width)
obs_y = -obs_height
obs_speed = 5

font = pygame.font.SysFont(None, 40)

def message(text):
    msg = font.render(text, True, RED)
    screen.blit(msg, [width // 4, height // 2])
    pygame.display.update()
    pygame.time.wait(2000)

# Game loop
running = True
while running:
    screen.fill(WHITE)

    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    # Keys
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT] and truck_x > 0:
        truck_x -= truck_speed
    if keys[pygame.K_RIGHT] and truck_x < width - truck_width:
        truck_x += truck_speed

    # Draw Truck
    pygame.draw.rect(screen, BLUE, (truck_x, truck_y, truck_width, truck_height))

    # Draw Obstacle
    pygame.draw.rect(screen, RED, (obs_x, obs_y, obs_width, obs_height))
    obs_y += obs_speed

    # Reset obstacle
    if obs_y > height:
        obs_y = -obs_height
        obs_x = random.randint(0, width - obs_width)

    # Collision
    if (truck_y < obs_y + obs_height and
        truck_y + truck_height > obs_y and
        truck_x < obs_x + obs_width and
        truck_x + truck_width > obs_x):
        message("Game Over!")
        obs_y = -obs_height  # Reset after crash

    pygame.display.update()
    clock.tick(60)
