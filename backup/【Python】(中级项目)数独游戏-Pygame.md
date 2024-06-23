# 项目名称

数独游戏

# 项目简介
-------------------------------------------------------------
程序实现了一个基于Pygame库的界面，玩家可以通过键盘输入数字来填充数独格子，并通过回车键来观察解法的可视化过程。程序能够解决提供的默认数独题目，如果玩家输入的数字导致数独不符合规则，会显示错误提示。

# 操作方法
1. 玩家可以按下键盘上的'D'键重置到默认数独题目，按下'R'键清空数独板。
2. 当数独解决完成时，会提示完成并可以按下'R'或'D'键重新开始。
3. 如果输入数字，然后按RETURN回车键将会可视化推理过程。

# 所需要的库(模块)

Pygame，如未安装，请在命令行下输入：

pip install pygame

# 程序源代码

```Python
import pygame

pygame.font.init()
screen = pygame.display.set_mode((600, 600))
pygame.display.set_caption('数独游戏(回溯法)')
img = pygame.image.load('icon.png')
pygame.display.set_icon(img)
font1 = pygame.font.SysFont('Arial', 40)
font2 = pygame.font.SysFont('Arial', 20)
x = 0 # 这是
y = 0 #
dif = 500 / 9 # 表示每个格子的大小
val = 0

# 默认的数独题目
grid = [
   [7, 8, 0, 4, 0, 0, 1, 2, 0],
   [6, 0, 0, 0, 7, 5, 0, 0, 9],
   [0, 0, 0, 6, 0, 1, 0, 7, 8],
   [0, 0, 7, 0, 4, 0, 2, 6, 0],
   [0, 0, 1, 0, 5, 0, 9, 3, 0],
   [9, 0, 4, 0, 6, 0, 0, 0, 5],
   [0, 7, 0, 3, 0, 0, 0, 1, 2],
   [1, 2, 0, 0, 0, 7, 4, 0, 0],
   [0, 4, 9, 2, 0, 6, 0, 0, 7]
]


def get_coord(pos):
   """将鼠标位置转换为格子坐标(行/列)"""
   global x, y
   x = pos[0] // dif
   y = pos[1] // dif

def draw_box():
   """绘制小格子的边框，可以更好地观察每个格子的位置"""
   for i in range(2):
       pygame.draw.line(screen, (255, 0, 0), (x * dif-3, (y + i) * dif),
                        (x * dif + dif + 3, (y + i)*dif), 7)
       pygame.draw.line(screen, (0, 0, 255), ((x + i) * dif, y * dif),
                        ((x + i) * dif, y * dif + dif), 7)

def draw():
   """画出数独板9x9的格子，是一个一个画出来的"""
   for i in range(9):
       for j in range(9):
           if grid[i][j] != 0:
               pygame.draw.rect(screen, (0, 255, 255),
                                (i * dif, j * dif, dif + 1, dif + 1))
               text1 = font1.render(str(grid[i][j]), 1, (255, 0, 0))
               screen.blit(text1, (i * dif + 18, j * dif + 6))
   # 画出粗的横线和粗的竖线
   for i in range(10):
       if i % 3 == 0:
           thick = 7
       else:
           thick = 1
       pygame.draw.line(screen, (0, 0, 0), (0, i * dif),
                        (500, i * dif), thick)
       pygame.draw.line(screen, (0, 0, 0), (i * dif, 0),
                        (i * dif, 500), thick)

def draw_val(val):
   """将输入的数字显示在格子中"""
   text1 = font1.render(str(val), 1, (0, 0, 0))
   screen.blit(text1, (x * dif + 18, y * dif + 6))

def raise_error_1():
   """显示错误提示"""
   text1 = font1.render('错误!!!', 1, (0, 0, 0))
   screen.blit(text1, (20, 570))

def raise_error_2():
   """显示错误提示2"""
   text1 = font1.render('错误，不是有效的值', 1, (0, 0, 0))
   screen.blit(text1, (20, 570))

def valid(m, i, j, val):
   """判断输入的数字是否有效"""
   for it in range(9):
       if m[i][it] == val: # 如果发现当前行有相同的数字，返回False
           return False
       if m[it][j] == val: # 如果发现当前列有相同的数字，返回False
           return False

   it = i // 3 # 计算当前格子所在的3x3格子的左上角的行号
   jt = j // 3 # 计算当前格子所在的3x3格子的左上角的列号

   for i in range(it * 3, it * 3 + 3):# 遍历当前格子所在的3x3格子的每一行
       for j in range(jt * 3, jt * 3 + 3):# 遍历当前格子所在的3x3格子的每一列
           if m[i][j] == val:
               return False
   return True

def solve(grid, i, j):
   """使用回溯法来解数独"""
   while grid[i][j] != 0:
       if i < 8:
           i += 1
       elif i == 8 and j < 8:
           i = 0
           j += 1
       elif i == 8 and j == 8:
           return True
   pygame.event.pump() # 刷新事件队列，防止鼠标点击事件影响程序运行
   for it in range(1, 10):
       if valid(grid, i, j, it) == True:
           grid[i][j] = it
           x = i
           y = j
           screen.fill((255, 255, 255))
           draw()
           draw_box()
           pygame.display.update()
           pygame.time.delay(20)

           if solve(grid, i, j) == 1:
               return True
           else:
               grid[i][j] = 0
           screen.fill((255, 255, 255))

           draw()
           draw_box()
           pygame.display.update()
           pygame.time.delay(50)
   return False

def instruction():
  text1 = font2.render(
      'D--RESET TO DEFAULT / R--CLEAR BOARD\n', 1, (0, 0, 0))
  text2 = font2.render(
      'ENTER VALUES AND PRESS ENTER TO VISUALIZE\n', 1, (0, 0, 0))
  screen.blit(text1, (20, 520))
  screen.blit(text2, (20, 540))
def result():
  text1 = font1.render('FINISHED PRESS R or D\n', 1, (0, 0, 0))
  screen.blit(text1, (20, 570))


run = True
flag_1 = 0 # 标识是否点击了格子
flag_2 = 0 # 标识是否输入了数字
rs = 0     # 标识是否数独解决完成
error = 0  # 标识是否输入了错误的数字
while run:
   screen.fill((255, 255, 255))
   for event in pygame.event.get():
       if event.type == pygame.QUIT:
           run = False
       if event.type == pygame.MOUSEBUTTONDOWN:
           flag_1 = 1
           pos = pygame.mouse.get_pos()
           get_coord(pos)
       if event.type == pygame.KEYDOWN:
           if event.key == pygame.K_LEFT:
               x -= 1
               flag_1 = 1
           if event.key == pygame.K_RIGHT:
               x += 1
               flag_1 = 1
           if event.key == pygame.K_UP:
               y -= 1
               flag_1 = 1
           if event.key == pygame.K_DOWN:
               y += 1
               flag_1 = 1
           if event.key == pygame.K_1:
               val = 1
           if event.key == pygame.K_2:
               val = 2
           if event.key == pygame.K_3:
               val = 3
           if event.key == pygame.K_4:
               val = 4
           if event.key == pygame.K_5:
               val = 5
           if event.key == pygame.K_6:
               val = 6
           if event.key == pygame.K_7:
               val = 7
           if event.key == pygame.K_8:
               val = 8
           if event.key == pygame.K_9:
               val = 9
           if event.key == pygame.K_RETURN:
               flag_2 = 1
           # If R pressed clear sudoku board
           if event.key == pygame.K_r:
               rs = 0
               error = 0
               flag_2 = 0
               grid = [
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0],
                   [0, 0, 0, 0, 0, 0, 0, 0, 0]
               ]

           # If D pressed reset board to default
           if event.key == pygame.K_d:
               rs = 0
               error = 0
               flag_2 = 0
               grid = [
                   [7, 8, 0, 4, 0, 0, 1, 2, 0],
                   [6, 0, 0, 0, 7, 5, 0, 0, 9],
                   [0, 0, 0, 6, 0, 1, 0, 7, 8],
                   [0, 0, 7, 0, 4, 0, 2, 6, 0],
                   [0, 0, 1, 0, 5, 0, 9, 3, 0],
                   [9, 0, 4, 0, 6, 0, 0, 0, 5],
                   [0, 7, 0, 3, 0, 0, 0, 1, 2],
                   [1, 2, 0, 0, 0, 7, 4, 0, 0],
                   [0, 4, 9, 2, 0, 6, 0, 0, 7]
               ]

   if flag_2 == 1:
       """如果要自动填充数独，则调用solve函数"""
       if solve(grid, 0, 0) == False:
           error = 1
       else:
           rs = 1
       flag_2 = 0

   if val != 0:
       draw_val(val)
       if valid(grid, int(x), int(y), val) == True:
           grid[int(x)][int(y)] = val
           flag_1 = 0
       else:
           grid[int(x)][int(y)] = 0
           raise_error_2()
       val = 0

   if error == 1:
       raise_error_1()
   if rs == 1:
       result()
   draw()
   if flag_1 == 1:
       draw_box()
   instruction()
   pygame.display.update()

pygame.quit()

```

