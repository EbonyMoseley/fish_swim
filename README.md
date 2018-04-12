# fish_swim
This program generates random swimming fish.

Viewing this file requires importing the Python graphics module.


from graphics import *
import random

#determines how many fish/bubbles will be drawn/moved
num_fish = eval(input("Enter the number of fish you would like to swim: "))


#bubble class
class Bubble:

    def __init__(self,x,y,window):
        #makes bubbles and draws them
        self.bubble = Circle(Point(x,y),random.randint(3,10))
        self.bubble.setFill("white")
        self.bubble.draw(window)

    #moves bubbles
    def move_bub(self,dx,dy,height):
        self.bubble.move(dx,dy)
        center = self.bubble.getCenter().getY()
        #makes bubbles wrap window (lengthwise)
        if center < 0:
            self.bubble.move(0,height)



#fish class       
class Fish:
    
    #makes fish
    def __init__(self,x,y,dx,color):
        #makes tail
        self.tail = Oval(Point(x,y-15),Point(x+10,y+15))
        self.tail.setFill(color)
        #makes fish body
        self.body = Oval(Point(x-30,y-15),Point(x,y+15))
        self.body.setFill(color)
        #makes fish eye
        self.eye = Circle(Point(x-25,y-2),7)
        self.eye.setFill("white")
        self.pupil = Circle(Point(x-27,y-2),3)
        self.pupil.setFill("black")

        #parts of fish
        self.part_lst = [self.tail,self.body,self.eye,self.pupil]
        
        #used for determining fish speed
        self.dx = dx
        
    #draws fish
    def draw(self,window):
        #for loop used to draw each part of fish, one by one
        for part in self.part_lst:
            part.draw(window)
            
    #moves fish
    def move_fish(self,dx,dy,width):
        #for loop used to move each part of fish, one by one
        for part in self.part_lst:
            part.move(self.dx,dy)

            center = self.body.getCenter().getX()
            #makes fish "wrap around" window (widthwise)
            if center < 0:
                for part in self.part_lst:
                    part.move(width,0)
            
                   
        
def main():
    #window set to size of image
    width = 679
    height = 679
    
    #makes the graphics window
    win = GraphWin("Just Keep Swimmming",width,height,autoflush = False)

    #displays background image
    image_name = "OceanImage.gif"
    ocean_image = Image(Point(width/2,height/2),image_name)
    ocean_image.draw(win)
    
 
    #initializes lists
    fish_lst = []
    bubble_lst = []
    

    #for loop determined by user's input
    for i in range(num_fish):
        #fish and bubbles are in random places
        x = random.randint(0,679)
        y = random.randint(0,679)
        #used to have each fish at different speed
        dx = random.randint(-5,-1)
        #makes each fish a random color
        color = color_rgb(random.randint(0,255),random.randint(0,255),random.randint(0,255))

        #calls Fish class with actual parameters
        fish = Fish(x,y,dx,color)
        #adds each new fish to fish list
        fish_lst.append(fish)
        #draws fish using Fish class' draw method
        fish.draw(win)
        #calls Bubble class with actual parameters
        bubble = Bubble(x,y,win)
        #adds each new bubble to bubble list
        bubble_lst.append(bubble)
        

    #while boolean is true, move fish and bubbles
    keep_swim = True
    while keep_swim:
        update()
        #when user clicks, fish stop swimming
        if win.checkMouse() != None:
            keep_swim = False

        #moves fish using Fish class' move_fish method         
        for fish in fish_lst:
            #bobbing motion
            dy = random.randint(-5,5)
            fish.move_fish(dx,dy,width)
        #moves bubbles using Bubble class' move_bub method
        for bubble in bubble_lst:
            bubble.move_bub(random.randint(-1,1),-2,height)

            

main()
