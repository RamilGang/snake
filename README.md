# snake
import pygame
import random

pygame.init()

white = (255,  255,255)
yellow = (255,  255,102)
black = (0,  0,0)
red = (213,  50,80)
green = (0,  255,0)
blue = (50,  153,213)
darkgreen= (0,  100,0)
#фон игры
yraq = pygame.image.load("yraq.jpg")

dis_width = 500
dis_height = 500

dis = pygame.display.set_mode((dis_width, dis_height))
pygame.display.set_caption('Змейка от Рамильки и Андрейки')

clock = pygame.time.Clock()
#размер и скорость передвижения змейки
snake_block = 10
snake_speed = 10

font_style = pygame.font.SysFont("timesnewroman", 25)
score_font = pygame.font.SysFont("timesnewroman", 25)


def Score(score):
    value = score_font.render("Счёт: " + str(score), True, yellow)
    dis.blit(value, [dis_width / 2.5 ,0])


def snake_body (snake_block, snake_list):
    for x in snake_list:
        pygame.draw.rect(dis, black, [x[0], x[1], snake_block, snake_block]) #создание самой змейки как прямоугольника с заданным цветом

#текст выводимый на экран после поражения
def text(msg, color):
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 2.6, dis_height / 3])


def text2(msg, color):
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 4.5, dis_height / 2.4])


def text3(msg, color):
    mesg = font_style.render(msg, True, color)
    dis.blit(mesg, [dis_width / 2.4, dis_height / 2])
#--------------------------------------------------------------

#cоздание функций
def gameLoop():
    game_over = False
    game_close = False

    x1 = dis_width / 2
    y1 = dis_height / 2

    x1_change = 0
    y1_change = 0

    snake_List = []
    Length_of_snake = 1

    applex = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
    appley = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0


    while not game_over:

        while game_close == True:
            dis.fill(black)
            text("Ты проиграл!", red)
            text2("Нажмите A, чтобы продолжить", red)
            text3("Выйти-Q", red)

            Score(Length_of_snake - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_a:
                        gameLoop()
#передвижение c помощью события KEYDOWN
#---------------------------------------------------------------
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -snake_block
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = snake_block
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -snake_block
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = snake_block
                    x1_change = 0
#заданные границы игрового поля (игровое поле по разрешению экрана)
        if x1 >= dis_width or x1 < 0 or y1 >= dis_height or y1 < 0:
            game_close = True

        x1 += x1_change
        y1 += y1_change
#загруженный фон
        dis.blit(yraq,(0, 0))
        
        pygame.draw.rect(dis, red, [applex, appley, snake_block, snake_block])
        snake_Head = []
        snake_Head.append(x1)
        snake_Head.append(y1)
        snake_List.append(snake_Head)
        if len(snake_List) > Length_of_snake:
            del snake_List[0]

        for x in snake_List[:-1]:
            if x == snake_Head:
                game_close = True

        snake_body(snake_block, snake_List)
        Score(Length_of_snake - 1)

        pygame.display.update()
#спавн яблок значение (10) для разрешения экрана 500x500
        if x1 == applex and y1 == appley:
            applex = round(random.randrange(0, dis_width - snake_block) / 10.0) * 10.0
            appley = round(random.randrange(0, dis_height - snake_block) / 10.0) * 10.0
#длина змейки при съедании яблока
            Length_of_snake += 1

        clock.tick(snake_speed)

    pygame.quit()
    quit()


gameLoop()
