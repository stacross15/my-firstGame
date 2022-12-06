# my-firstGame
#Stacy Ross
#COSC1315
#myGame assignment
#12/05/2022

#This is the First Part:

import turtle #provides the graphics
import math #to calculate the points
import random #to randomize the chattans

#Screen size, title/name, and color
window = turtle.Screen()
window.setup(width=600, height=600)
window.title("Star Wars Game by Stacy")#changed
window.bgcolor("blue")#changed from black to blue

window.tracer(0)
#players character graphic
vertex = ((0,15),(-15,0),(-18,5),(-18,-5),(0,0),(18,-5),(18, 5),(15, 0))
window.register_shape("player", vertex)
#the chattan character grahic
asVertex = ((0, 10), (5, 7), (1,1)#changed from (3,3)
            , (10,0), (7, 4), (8, -6), (0, -10), (-5, -5), (-7, -7), (-10, 0), (-5, 4), (-1, 8))
window.register_shape("chattan", asVertex)

####################
#This is the Second Part:

#graphic and speed function
class Ankur(turtle.Turtle):
    def __init__(self):
        turtle.Turtle.__init__(self)

        self.speed(0)
        self.penup()

#coordinates function
def ankur1(t1, t2):
    x1 = t1.xcor()
    y1 = t1.ycor()
    
    x2 = t2.xcor()
    y2 = t2.ycor()
    
    taauko = math.atan2(y1 - y2, x1 - x2)
    taauko = taauko * 180.0 / 3.14159
    
    return taauko


player = Ankur()
player.color("white")
player.shape("player")
player.score = 0

####################
#This is the Third Part

missiles = []
for _ in range(3):#bullets grahic and speed 
    missile = Ankur()
    missile.color("orange")#changed from red to orange
    missile.shape("arrow")
    missile.speed = 1
    missile.state = "ready"
    missile.hideturtle()
    missiles.append(missile)
#Score graphics
pen = Ankur()
pen.color("white")
pen.hideturtle()
pen.goto(0, 250)
pen.write("Score: 0", False, align = "center", font = ("Arial", 24, "normal"))

####################
#This is the Fourth Part


chattans = []
#bad guys graphics and settings
for _ in range(5):   
    chattan = Ankur()
    chattan.color("yellow")#changed from brown to yellow
    chattan.shape("arrow")

    chattan.speed  = random.randint(2, 3)/50
    chattan.goto(0, 0)
    taauko = random.randint(0, 260)
    distance = random.randint(300, 400)
    chattan.setheading(taauko)
    chattan.fd(distance)
    chattan.setheading(ankur1(player, chattan))
    chattans.append(chattan)

####################
#This is the Functions for Defence Part

def baanya():
    player.lt(20)
    
def daanya():
    player.rt(20)
    
def fire_missile():
    for missile in missiles:
        if missile.state == "ready":
            missile.goto(0, 0)
            missile.showturtle()
            missile.setheading(player.heading())
            missile.state = "fire"
            break


window.listen()
window.onkey(baanya, "Left") #keys to control the player to turn left
window.onkey(daanya, "Right") #keys to control the player to turn right
window.onkey(fire_missile, "space") #keys to control the player to fire

####################
#This is the Functioning the Code Part-1

sakkyo = False
while True:

    window.update()
    player.goto(0, 0)
    
#firing graphics and speed
    for missile in missiles:
        if missile.state == "fire":
            missile.fd(missile.speed)
        
        if missile.xcor() > 300 or missile.xcor() < -300 or missile.ycor() > 300 or missile.ycor() < -300:
            missile.hideturtle()
            missile.state = "ready"

    for chattan in chattans:    
        chattan.fd(chattan.speed)
        
        for missile in missiles:
            if chattan.distance(missile) < 20:
                taauko = random.randint(0, 260)
                distance = random.randint(600, 800)
                chattan.setheading(taauko)
                chattan.fd(distance)
                chattan.setheading(ankur1(player, chattan))
                chattan.speed += 0.01
                
                missile.goto(600, 600)
                missile.hideturtle()
                missile.state = "ready"
                
                player.score += 10
                pen.clear()
                pen.write("Score: {}".format(player.score), False, align = "center", font = ("Arial", 24, "normal"))

        ####################
        #This is the Functioning the Code Part-2

        if chattan.distance(player) < 20:
            taauko = random.randint(0, 260)
            distance = random.randint(600, 800)
            chattan.setheading(taauko)
            chattan.fd(distance)
            chattan.setheading(ankur1(player, chattan))
            chattan.speed += 0.005
            sakkyo = True
            player.score -= 30
            pen.clear()
            pen.write("Score: {}".format(player.score), False, align = "center", font = ("Arial", 24, "normal"))
    if sakkyo == True:
        player.hideturtle()
        missile.hideturtle()
        for a in chattans:
            a.hideturtle()
        pen.clear()
        break

window.mainloop
