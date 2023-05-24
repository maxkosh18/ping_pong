# ping_pong
from pygame import *
from random import randint

class GameSprite(sprite.Sprite):
    def __init__(self, player_image, player_x, player_y, player_speed, width, height):
        super(). __init__()
        self.image = transform.scale(image.load(player_image), (width, height))
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSprite):
    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < height - 80:
            self.rect.y += self.speed
    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < height - 80:
            self.rect.y += self.speed
    
width = 700
height = 500
window = display.set_mode((width, height))
display.set_caption('pygame ping_pong')
background = transform.scale(image.load('backgr.jpg'), (width, height))

finish = False
game = True
FPS = 60
clock = time.Clock()

racket1 = Player('storona.png', 30, 200, 4, 50, 150)
racket2 = Player('storona.png', 600, 200, 4, 50, 150)
ball = GameSprite('ball.png', 200, 200, 4, 50, 50)

font.init()
font = font.SysFont('Arial', 35)
lose1 = font.render('PLAYER 1 LOSE!', True, (180, 0, 0))
lose2 = font.render('PLAYER 2 LOSE!', True, (180, 0, 0))

speed_x = 3
speed_y = 3

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False
    if finish != True:
        racket1.update_l()
        racket2.update_r()
        ball.rect.x += speed_x
        ball.rect.y += speed_y
        
        window.blit(background,(0,0))
        racket1.reset()
        racket2.reset()
        ball.reset()
        
        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            speed_x *= -1
            speed_y *= 1
        if ball.rect.y > height-50 or ball.rect.y < 0:
            speed_y *= -1
        if ball.rect.x < 0:
            finish = True
            window.blit(lose1, (200, 200))
            game_over = True
        if ball.rect.x > 700:
            finish = True
            window.blit(lose2, (200, 200))
            game_over = True

    display.update()
    clock.tick(FPS)
