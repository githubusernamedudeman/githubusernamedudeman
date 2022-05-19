hello i am a beginner and have been coding a game and the background keeps going over the game help plz

import pygame
from random import randint

pygame.init()

import pygame
from pygame.locals import *
import sys
import os
def events():
    for event in pygame.event.get():
        if event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
            pygame.quit()
            sys.exit()

# define display surfaces
W, H = 1000, 600
HW, HH = W / 2, H/2
AREA = W * H

os.environ['SDL_VIDEO_WINDOW_POS'] = "50, 50"

pygame.init()
CLOCK = pygame.time.Clock()
DS = pygame.display.set_mode((W, H))
pygame.display.set_caption("code.Pylet - Seamless Background Scrolling")
FPS = 120

bkgd = pygame.image.load("loser.png").convert()
x = 0

DEFAULT_IMAGE_SIZE = (1000, 600)

bkgd = pygame.transform.scale(bkgd, DEFAULT_IMAGE_SIZE)

while True:
    events()

    rel_x = x % bkgd.get_rect().width
    DS.blit(bkgd, (rel_x - bkgd.get_rect().width, 0))
    if rel_x < W:
        DS.blit(bkgd, (rel_x, 0))
    x -= 1

    pygame.display.update()
    CLOCK.tick(FPS)

score = 0
total = 0

myfont = pygame.font.SysFont('monospace', 50)
# Jumping Variables

yVel = 0
Jumping = 0
# Dead People :D
Dead = False
# Making dictionaries with settings for everything.

display = {
    "width": 1000,
    "height": 600
}
character = {
  "width": 20,
  "height": 20,
  "x": 200,
  "y": 580,
  "velocity": 50
}
platform = {
    'y': 580,
    "x": 700,
    "pass": 0,
    "length": 20,
    "ammount": 2,
    "distanceApart": 50
}
spike = {
  "height": -15,
  "y": 600,
  "x": 700,
  "pass": 0,
  "length": 20,
  "ammount": 2,
  "distanceApart": 40
}
test = 0

def nextSection(a):
    spike["x"] = 700
    spike['pass'] += spike['ammount']
    spike['ammount'] = randint(5,8)
    spike['distanceApart'] = randint(7,8)*10
    return a + 1


def triangleDraw(i):  # Draws the triangles
    pygame.draw.polygon(win,(0,0,0),((spike["x"]+spike['distanceApart']*i,spike["y"]),(spike['x']+spike['distanceApart']*i+spike["length"],spike['y']),(spike['x']+spike['length']/2+spike['distanceApart']*i,spike['y']+spike['height'])))

def jump():  # Start Jumping
    global yVel
    global Jumping
    if Jumping == 0:
        Jumping = 1
        yVel = 12
        character['y'] = character['y']-yVel
        if character['y'] > platform['y']:
            Jumping = 0
        yVel -= 0.5


def cJump():  # Contine Jump
    global yVel
    global Jumping
    if Jumping == 0:
        if character['y'] > platform['y']:
            pass
        else:
            Jumping = 1
    if Jumping == 1:
        character['y'] = character['y']-yVel
        if character['y'] > platform['y']:
            Jumping = 2
        yVel -= 0.5
    elif Jumping == 2:
        Jumping =3
    elif Jumping == 3:
        Jumping = 4
    elif Jumping == 4:
        Jumping = 5
    elif Jumping == 5:                                      
        Jumping = 0


def next():
    cJump()
    pygame.draw.rect(win, (255, 0, 0), (character["x"], character["y"], character["width"], character["height"]))
    pygame.display.update()
    spike['x'] -= 5

# Launching the window, setting it to the dimensions of the `display` dictionary.
win = pygame.display.set_mode((display["width"], display["height"]))

jump()
while True:  # Main Game Loop
    pygame.time.delay(10)
    win.fill((255, 255, 255))
    for i in range(spike['ammount']):  # Spike Drawing
        triangleDraw(i)
    keys = pygame.key.get_pressed()
    if keys[pygame.K_UP] or keys[pygame.K_w]:  # Checks if need to Jump
        jump()
    for event in pygame.event.get():  # Quit statement
        if event.type == pygame.QUIT:
            break
    if spike['x']+spike['distanceApart']*spike['ammount'] < character['x']:  # checks to start next section
        i = nextSection(7)
    if spike['pass'] < 100:  # Win Statement
        textsurface = myfont.render("Debug {0}".format(Dead), False, (255, 255, 255))
        textsurface2 = myfont.render("Percentage {0}%".format(spike['pass']), False, (0, 0, 0))
        win.blit(textsurface, (0,10))
        win.blit(textsurface2, (300,10))
    else:
        textsurface2 = myfont.render("YOU WIN",False, (255,0,0))
        win.blit(textsurface2, (300,10))
        break
    for i in range(spike['ammount']):  # Checks if death occurs
        if spike['x']+spike['distanceApart']*i <= character['x'] and spike['x']+spike['distanceApart']*i+spike['length'] >= character['x']:
            posOnSpike = abs(character['x'] - (spike['x']+spike['length']/2))
            test = 1
            if posOnSpike*2+spike['y'] > character['y'] and spike['y'] < character['y'] or posOnSpike*2+spike['y'] > character['y'] + character['height'] and spike['y'] < character['y']+character['height']:
                textsurface2 = myfont.render("oy mate",False, (255,0,0))
                win.blit(textsurface2, (300,10))
                Dead = True
        else:
            None
    # Drawing Stuff
    next()
    if Dead:

        import webbrowser

        webbrowser.open('https://www.youtube.com/shorts/G_IgFwA5GH4')
        break

pygame.quit()
