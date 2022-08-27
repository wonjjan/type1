import pygame
from pygame.locals import QUIT, KEYDOWN, K_BACKSPACE, K_RETURN
import sys
from quest import word
from random import sample
from random import randint as ri


pygame.init()

SUR = pygame.display.set_mode((300,300))
FPS = pygame.time.Clock()

life = 5
game_over = False

lmess = pygame.font.SysFont(None, 30)
qmess = pygame.font.SysFont(None, 60)
mess = pygame.font.SysFont(None, 50)
omess = pygame.font.SysFont(None, 60)

score = 0
rand_word = sample(word, 1)[0]

st = ""
while True:
    SUR.fill((255,255,255))
    for i in pygame.event.get():
        if i.type == QUIT:
            pygame.quit()
            sys.exit()
        elif i.type == KEYDOWN:
            if i.key == K_BACKSPACE:
                st = st[:-1]
            elif i.key == K_RETURN:
                if rand_word == st:
                    score += 100
                    rand_word = sample(word, 1)[0]
                else:
                    life -= 1
                    if life == 0:
                        game_over = True
                st = ""
            else:
                st += i.unicode
            print(st)            

    if not game_over:
        pygame.draw.line(SUR, (0,0,0), (0,200), (300,200), 3)

        m = mess.render(st, True, (0,0,0))
        mx, my = m.get_width(), m.get_height()
        SUR.blit(m, (150-mx//2, 250-my//2))

        
        q = qmess.render(rand_word, True, (0,0,255))
        qx, qy = q.get_width(), q.get_height()
        SUR.blit(q, (150-qx//2, 100-qy//2))

        l = lmess.render(f"LIFE {life}", True, (255,0,0))
        lx = l.get_width()
        SUR.blit(l, (300-lx-5, 5))

    else:
        o = omess.render(f"SCORE {score}", True, (0,0,0))
        ox, oy = o.get_width(), o.get_height()
        SUR.blit(o, (150-ox//2, 150-oy//2))




    pygame.display.flip()
    FPS.tick(25)
