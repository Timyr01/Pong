from pygame import*

img_ball = 'ball.png'
img_racket = 'racket.jpg'

class GameSprite(sprite.Sprite): 
    def __init__(self, player_image, player_x, player_y, player_speed, wight, height):
        super().__init__()
        self.image = transform.scale(image.load(player_image), (wight, height))  
        self.speed = player_speed
        self.rect = self.image.get_rect()
        self.rect.x = player_x
        self.rect.y = player_y

    def reset(self):
        window.blit(self.image, (self.rect.x, self.rect.y))

class Player(GameSpite):

    def update_r(self):
        keys = key.get_pressed()
        if keys[K_UP] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_DOWN] and self.rect.y < win_height - 80:
            self.rect.y += self.speed

    def update_l(self):
        keys = key.get_pressed()
        if keys[K_w] and self.rect.y > 5:
            self.rect.y -= self.speed
        if keys[K_s] and self.rect.y < win_height - 80:
            self.rect.y += self.speed

ball = GameSpite('tenis_ball.png', 200, 200, 4, 50, 50)
racket1 = Player('-', 30, 200, 4, 50, 150)
racket2 = Player('-', 520, 200, 4, 50, 150)

speed_x = 3
speed_y = 3

while game:
    for e in event.get():
        if e.type == QUIT:
            game = False

    if finish != True:
        window.fill(back)
        racket1.update_l()
        racket2.update_r()
        ball.rect.x += speed_x
        ball.rect.y += speed_y

        if sprite.collide_rect(racket1, ball) or sprite.collide_rect(racket2, ball):
            speed_x *= -1
            speed_y *= 1

        if ball.rect.y > win_height - 50 or ball.rect.y < 0:
            speed_y *= -1

        if ball.rect.x < 0:
            finish = True
            window.blit(lose1, (200,200))
            game_over = True
        
        if ball.rect.x > win.width:
            finish = True
            window.blit(lose2, (200,200))
            game_over = True


window = display.set_mode((500,500)) 
col1 = (0, 0, 255)
mw.fill(col1)
cloock = time.Clock()

run = True
while run:
    for e in event.get():
        if e.type == QUIT:
            run = False
    display.update()
    cloock.tick(40)



racket1.reset()
racket2.reset()
ball.reset()
