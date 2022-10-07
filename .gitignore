import random
import sys
import pygame
from pygame.locals import *

# Global/Game Variables

windowWidth = 600
windowHeight = 499

# Setting height and width for game
window = pygame.display.set_mode((windowWidth, windowHeight))
elevation = windowHeight * 0.8
gameImages = {}
FPS = 32
pipeimage = 'images/pipe.png'
backgroundImage = 'images/background.jpg'
bird = 'images/bird.png'
sealevel = 'images/base.jfif'

def flappy():
    score = 0
    horizontal = int(windowWidth / 5)
    vertical = int(windowWidth / 2)
    ground = 0
    tempHeight = 100

    pipe1 = createPipe()
    pipe2 = createPipe()

    # List containing lower pipes
    downPipes = [
        {'x': windowWidth + 300 - tempHeight,
         'y': pipe1[1]['y']},
        {'x': windowWidth + 300 - tempHeight + (windowWidth / 2),
         'y': pipe2[1]['y']},
    ]

    # List Containing upper pipes
    upPipes = [
        {'x': windowWidth + 300 - tempHeight,
         'y': pipe1[0]['y']},
        {'x': windowWidth + 200 - tempHeight + (windowWidth / 2),
         'y': pipe2[0]['y']},
    ]

    # Pipe velocity x axis
    pipeVelX = -4

    # bird velocity
    birdVelY = -9
    birdMaxY = 10
    bird_Min_Vel_Y = -8
    birdAccY = 1

    birdFlapVel = -8
    birdFlapped = False

    while True:
        for event in pygame.event.get():
            if event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
                pygame.quit()
                sys.exit()
            if event.type == KEYDOWN and (event.key == K_SPACE or event.key == K_UP):
                if vertical > 0:
                    birdVelY = birdFlapVel
                    birdFlapped = True


        isGameOver = gameOver(horizontal, vertical, upPipes, downPipes)
        if isGameOver:
            return

        # Checking score..
        birdMidPos = horizontal + gameImages['flappybird'].get_width() / 2
        for pipe in upPipes:
            pipeMidPos = pipe['x'] + gameImages['pipeimage'][0].get_width() / 2
            if pipeMidPos <= birdMidPos < pipeMidPos + 4:
                score += 1
                print(f"Your your_score is {score}")

        if birdVelY < birdMaxY and not birdFlapped:
            birdVelY += birdAccY

        if birdFlapped:
            birdFlapped = False


        playerHeight = gameImages['flappybird'].get_height()
        vertical = vertical + min(birdVelY, elevation - vertical - playerHeight)

        # move pipes to the left
        for upperPipe, lowerPipe in zip(upPipes, downPipes):
            upperPipe['x'] += pipeVelX
            lowerPipe['x'] += pipeVelX

        # Add a new pipe when the first is
        # about to cross the leftmost part of the screen
        if 0 < upPipes[0]['x'] < 5:
            newpipe = createPipe()
            upPipes.append(newpipe[0])
            downPipes.append(newpipe[1])

        # if the pipe is out of the screen, remove it
        if upPipes[0]['x'] < -gameImages['pipeimage'][0].get_width():
            upPipes.pop(0)
            downPipes.pop(0)

        # Lets blit our game images now
        window.blit(gameImages['background'], (0, 0))
        for upperPipe, lowerPipe in zip(upPipes, downPipes):
            window.blit(gameImages['pipeimage'][0], (upperPipe['x'], upperPipe['y']))
            window.blit(gameImages['pipeimage'][1], (lowerPipe['x'], lowerPipe['y']))

        window.blit(gameImages['sealevel'], (ground, elevation))
        window.blit(gameImages['flappybird'], (horizontal, vertical))

        # Getting digits for score
        numbers = [int(x) for x in list(str(score))]
        width = 0

        # Finding width of score image
        for num in numbers:
            width += gameImages['scoreimages'][num].get_width()
        Xoffset = (windowWidth - width) / 1.1

        for num in numbers:
            window.blit(gameImages['scoreimages'][num], (Xoffset, windowWidth * 0.02))
            Xoffset += gameImages['scoreimages'][num].get_width()

        # Refreshing the game window and displaying the score.
        pygame.display.update()
        FPSClock.tick(FPS)

def createPipe():
    offset = windowHeight / 3
    pipeHeight = gameImages['pipeimage'][0].get_height()
    y2 = offset + random.randrange(0, int(windowHeight - gameImages['sealevel'].get_height() - 1.2 * offset))
    pipeX = windowWidth + 10
    y1 = pipeHeight - y2 + offset
    pipe = [
        # upper Pipe
        {'x': pipeX, 'y': -y1},

        # lower Pipe
        {'x': pipeX, 'y': y2}
    ]
    return pipe

def gameOver(horz, vert, upPipe, downPipe):
    if vert > elevation - 25 or vert < 0:
        return True

    for pipe in upPipe:
        pipeHeight = gameImages['pipeimage'][0].get_height()
        if (vert < pipeHeight + pipe['y'] and abs(horz - pipe['x']) < gameImages['pipeimage'][0].get_width()):
            return True

    for pipe in downPipe:
        if (vert + gameImages['flappybird'].get_height() > pipe['y']) and abs(horz - pipe['x']) < gameImages['pipeimage'][0].get_width():
            return True

    return False

if __name__ == "__main__":
    # Initializing
    pygame.init()
    FPSClock = pygame.time.Clock()

    pygame.display.set_caption("Flappy Remake") # Title

    # Loading images..
    gameImages['scoreimages'] = (
        pygame.image.load('images/0.png').convert_alpha(),
        pygame.image.load('images/1.png').convert_alpha(),
        pygame.image.load('images/2.png').convert_alpha(),
        pygame.image.load('images/3.png').convert_alpha(),
        pygame.image.load('images/4.png').convert_alpha(),
        pygame.image.load('images/5.png').convert_alpha(),
        pygame.image.load('images/6.png').convert_alpha(),
        pygame.image.load('images/7.png').convert_alpha(),
        pygame.image.load('images/8.png').convert_alpha(),
        pygame.image.load('images/9.png').convert_alpha()
    )
    gameImages['flappybird'] = pygame.image.load(bird).convert_alpha()
    gameImages['sealevel'] = pygame.image.load(sealevel).convert_alpha()
    gameImages['background'] = pygame.image.load(backgroundImage).convert_alpha()
    gameImages['pipeimage'] = (pygame.transform.rotate(pygame.image.load(pipeimage).convert_alpha(), 180), pygame.image.load(pipeimage).convert_alpha())

    print("Welcome to Flappy Remake")
    print("Press space or enter to start :)")

    while True:

        # Coordinates for bird
        horizontal = int(windowWidth / 5)
        vertical = int(
            (windowHeight - gameImages['flappybird'].get_height()) / 2)
        ground = 0

        while True:
            for event in pygame.event.get():

                if event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
                    pygame.quit()
                    sys.exit()

                elif event.type == KEYDOWN and (event.key == K_SPACE or event.key == K_UP):
                    flappy()

                else:
                    window.blit(gameImages['background'], (0, 0))
                    window.blit(gameImages['flappybird'], (horizontal, vertical))
                    window.blit(gameImages['sealevel'], (ground, elevation))
                    pygame.display.update()
                    FPSClock.tick(FPS)




