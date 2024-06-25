## 体验奇妙的递归调用的妙用：

### 1.带颜色的分形树

<img width="324" alt="22222" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/46283137-4a94-45f5-a429-8013c1cb2ca8">

### 2.谢尔宾斯基三角形

```Python
import turtle
screen = turtle.Screen()
screen.tracer(0)
def draw_sierpinski(length,depth):
    if depth==0:
        for i in range(3):
            turtle.forward(length)
            turtle.left(120)
    else:
        draw_sierpinski(length/2,depth-1)# 递归调用自己
        turtle.forward(length/2)
        draw_sierpinski(length/2,depth-1)
        turtle.backward(length/2)
        turtle.left(60)
        turtle.forward(length/2)
        turtle.right(60)
        draw_sierpinski(length/2,depth-1)
        turtle.left(60)
        turtle.backward(length/2)
        turtle.right(60)
window = turtle.Screen()
window.bgcolor('white') 
turtle.speed(0)
turtle.up()
turtle.goto(-400,-300)
turtle.down()
# 使用不同的深度depth（这里是10）尝试产生不同的谢式三角形！
draw_sierpinski(800,8)  
screen.update()
```

### 3.科赫雪花

```Python
import turtle
wn = turtle.Screen()
wn.tracer(0)
turtle.width(4)
turtle.speed(1000)
def koch(t,order,size):
    if order == 0:
        t.forward(size)
    else:
        koch(t,order-1,size/3)
        t.left(60)
        koch(t,order-1,size/3)
        t.right(120)
        koch(t,order-1,size/3)
        t.left(60)
        koch(t,order-1,size/3)
for _ in range(3):
    koch(turtle,5,400)
    turtle.right(120)
```

### 4.另一种五角星雪花
```Python
#"Recursive Star"
import turtle
wn = turtle.Screen()
wn.tracer(0,0)
def star(turtle, n,r):
    """ draw a star of n rays of length r"""
    
    for k in range(0,n):
        turtle.forward(r)
        turtle.backward(r)
        turtle.left(360/n)
    """
    for k in range(5):
        turtle.fd(r)
    """
def recursive_star(turtle, n, r, depth):
    if depth == 0:
        star(turtle, n, 1.6)
    else:
        for k in range(0,n):
            turtle.forward(r)
            recursive_star(turtle, n, 0.4*r, depth - 1)
            turtle.backward(r)
            turtle.left(360/n)

fred = turtle.Turtle()
fred.speed(1000)
fred.width(2)
fred.color('red')
#star(fred,5,100)
recursive_star(fred,5,150,4)
```