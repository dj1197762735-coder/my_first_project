
#     def __init__(self):
#         pygame.init()
#         self.screen=pygame.display.set_mode((1200,800))
#         pygame.display.set_caption("Alien Invasion")
#     def run_game(self):
#         while True:
#             for event in pygame.event.get():
#                 if event.type==pygame.QUIT:
#                     sys.exit()
#             pygame.display.flip()
# if __name__=='__main__':
#     ai=AlienInvasion()
#     ai.run_game()

#     def __init__(self):
#         pygame.init()
#         self.clock=pygame.time.Clock()
#         self.screen=pygame.display.set_mode((1200,800))
#         pygame.display.set_caption("Alien Invasion")
#         self.bg_color=(230,230,230)
#     def run_game(self):
#         while True:
#             for event in pygame.event.get():
#                 if event.type==pygame.QUIT:
#                     sys.exit()
#             self.screen.fill(self.bg_color)
#             pygame.display.flip()
#             self.clock.tick(60)
# if __name__=='__main__':
#     ai=AlienInvasion()
#     ai.run_game()
import sys
import pygame
from settings import Settings
from ship import Ship
from bullet import Bullet
class AlienInvasion:
    def __init__(self):
        pygame.init()
        self.clock=pygame.time.Clock()
        self.settings=Settings()
        # self.screen=pygame.displayself.set_mode((0,0),pygame.FULLSCREEN)
        # self.settings.screen_width=self.screen.get_rect().width
        # self.settings.screen_height=.screen.get_rect().height
        self.screen=pygame.display.set_mode((self.settings.screen_width,self.settings.screen_height))
        pygame.display.set_caption("飞船游戏")
        self.ship=Ship(self)
        self.bullets=pygame.sprite.Group()
    def run_game(self):
        while True:
            self._check_events()
            self.ship.update()
    def _update_bullets(self):
        self.bullets.update()
        for bullet in self.bullets.copy():
            if bullet.rect.bottom<=0:
                self.bullets.remove(bullet)
            print(len(self.bullets))
            self._update_screen()
            self.clock.tick(120)
    def _check_events(self):
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
            elif event.type==pygame.KEYDOWN:
                self._check_keydown_events(event)
            elif event.type==pygame.KEYUP:
                self._check_keyup_events(event)
    def _check_keydown_events(self,event):
        if event.key==pygame.K_RIGHT:
            self.ship.moving_right=True
        elif event.key==pygame.K_LEFT:
            self.ship.moving_left=True
        elif event.key==pygame.K_q:
            sys.exit()
        elif event.key==pygame.K_SPACE:
            self._fire_bullet()
    def _check_keyup_events(self,event):
        if event.key==pygame.K_RIGHT:
            self.ship.moving_right=False
        elif event.key==pygame.K_LEFT:
            self.ship.moving_left=False
            self.ship.rect.x+=1
    def _fire_bullet(self):
        if len(self.bullets)<self.settings.bullets_allowed:
            new_bullet=Bullet(self)
            self.bullets.add(new_bullet)
    def _update_screen(self):
        self.screen.fill(self.settings.bg_color)
        for bullet in self.bullets.sprites():
            bullet.draw_bullet()
        self.ship.blitme()
        pygame.display.flip()
if __name__=='__main__':
    ai=AlienInvasion()
    ai.run_game()
